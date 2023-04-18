## 什么是 OpenIddict?

OpenIddict is **an open source and versatile framework for building standard-compliant OAuth 2.0/OpenID Connect servers**
in any ASP.NET Core 2.1 (and higher) and legacy ASP.NET 4.6.1 (and higher) applications.

OpenIddict was born in late 2015 and was initially based on [AspNet.Security.OpenIdConnect.Server](https://github.com/aspnet-contrib/AspNet.Security.OpenIdConnect.Server)
(codenamed ASOS), a low-level OpenID Connect server middleware inspired by the OAuth 2.0 authorization server middleware developed by Microsoft for the OWIN project
and the first OpenID Connect server ever created for ASP.NET Core.

In 2020, ASOS was merged into OpenIddict 3.0 to form a unified stack under the OpenIddict umbrella, while still offering an easy-to-use approach for new users
and a low-level experience for advanced users thanks to a "degraded mode" that allows using OpenIddict in a stateless way (i.e without a backing database).

As part of this process, native support for `Microsoft.Owin` was added to OpenIddict 3.0 to allow using it in legacy ASP.NET 4.6.1 (and higher) applications,
making it an excellent candidate for replacing `OAuthAuthorizationServerMiddleware` and `OAuthBearerAuthenticationMiddleware` without having to migrate to ASP.NET Core.

## Core concepts

### User authentication

Unlike other solutions, **OpenIddict exclusively focuses on the OAuth 2.0/OpenID Connect protocol aspects of the authorization process**
and leaves user authentication up to the implementer: OpenIddict can be natively used with any form of user authentication like password, token,
federated or Integration Windows Authentication. While convenient, using a membership stack like ASP.NET Core Identity is not required.

Integration with OpenIddict is typically done by enabling the pass-through mode to handle requests in a controller action
or in a minimal API handler or, for more complex scenarios, by directly using its advanced events model.

### Pass-through mode

As with `OAuthAuthorizationServerMiddleware`, OpenIddict allows handling authorization, logout and token requests in custom controller actions or any other
middleware able to hook into the ASP.NET Core or OWIN request processing pipeline. In this case, OpenIddict will always validate incoming requests first
(e.g by ensuring the mandatory parameters are present and valid) before allowing the rest of the pipeline to be invoked: should any validation error occur,
OpenIddict will automatically reject the request before it reaches user-defined controller actions or custom middleware.

```csharp
builder.Services.AddOpenIddict()
    .AddServer(options =>
    {
        // Enable the authorization and token endpoints.
        options.SetAuthorizationEndpointUris("/authorize")
               .SetTokenEndpointUris("/token");

        // Enable the authorization code flow.
        options.AllowAuthorizationCodeFlow();

        // Register the signing and encryption credentials.
        options.AddDevelopmentEncryptionCertificate()
               .AddDevelopmentSigningCertificate();

        // Register the ASP.NET Core host and configure the authorization endpoint
        // to allow the /authorize minimal API handler to handle authorization requests
        // after being validated by the built-in OpenIddict server event handlers.
        //
        // Token requests will be handled by OpenIddict itself by reusing the identity
        // created by the /authorize handler and stored in the authorization codes.
        options.UseAspNetCore()
               .EnableAuthorizationEndpointPassthrough();
    });
```

```csharp
app.MapGet("/authorize", async (HttpContext context) =>
{
    // Resolve the claims stored in the principal created after the Steam authentication dance.
    // If the principal cannot be found, trigger a new challenge to redirect the user to Steam.
    var principal = (await context.AuthenticateAsync(SteamAuthenticationDefaults.AuthenticationScheme))?.Principal;
    if (principal is null)
    {
        return Results.Challenge(properties: null, new[] { SteamAuthenticationDefaults.AuthenticationScheme });
    }

    var identifier = principal.FindFirst(ClaimTypes.NameIdentifier)!.Value;

    // Create a new identity and import a few select claims from the Steam principal.
    var identity = new ClaimsIdentity(TokenValidationParameters.DefaultAuthenticationType);
    identity.AddClaim(new Claim(Claims.Subject, identifier));
    identity.AddClaim(new Claim(Claims.Name, identifier).SetDestinations(Destinations.AccessToken));

    return Results.SignIn(new ClaimsPrincipal(identity), properties: null, OpenIddictServerAspNetCoreDefaults.AuthenticationScheme);
});
```

### Events model

OpenIddict implements a powerful event-based model for its server and validation stacks: each part of the request processing logic is implemented as an event handler
that can be removed, moved to a different position in the pipeline or replaced by a custom handler to override the default logic used by OpenIddict:

```csharp
/// <summary>
/// Contains the logic responsible of rejecting authorization requests that don't specify a valid prompt parameter.
/// </summary>
public class ValidatePromptParameter : IOpenIddictServerHandler<ValidateAuthorizationRequestContext>
{
    /// <summary>
    /// Gets the default descriptor definition assigned to this handler.
    /// </summary>
    public static OpenIddictServerHandlerDescriptor Descriptor { get; }
        = OpenIddictServerHandlerDescriptor.CreateBuilder<ValidateAuthorizationRequestContext>()
            .UseSingletonHandler<ValidatePromptParameter>()
            .SetOrder(ValidateNonceParameter.Descriptor.Order + 1_000)
            .SetType(OpenIddictServerHandlerType.BuiltIn)
            .Build();

    /// <inheritdoc/>
    public ValueTask HandleAsync(ValidateAuthorizationRequestContext context)
    {
        if (context is null)
        {
            throw new ArgumentNullException(nameof(context));
        }

        // Reject requests specifying prompt=none with consent/login or select_account.
        if (context.Request.HasPrompt(Prompts.None) && (context.Request.HasPrompt(Prompts.Consent) ||
                                                        context.Request.HasPrompt(Prompts.Login) ||
                                                        context.Request.HasPrompt(Prompts.SelectAccount)))
        {
            context.Logger.LogInformation(SR.GetResourceString(SR.ID6040));

            context.Reject(
                error: Errors.InvalidRequest,
                description: SR.FormatID2052(Parameters.Prompt),
                uri: SR.FormatID8000(SR.ID2052));

            return default;
        }

        return default;
    }
}
```

In OpenIddict itself, event handlers are typically defined as dedicated classes but they can also be registered using delegates:

```csharp
services.AddOpenIddict()
    .AddServer(options =>
    {
        options.AddEventHandler<HandleConfigurationRequestContext>(builder =>
            builder.UseInlineHandler(context =>
            {
                // Attach custom metadata to the configuration document.
                context.Metadata["custom_metadata"] = 42;

                return default;
            }));
    });
```
