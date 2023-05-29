# OpenIddict

### 这是一个让你沉迷的 OpenID Connect 技术栈。

[![Build status](https://github.com/openiddict/openiddict-core/workflows/build/badge.svg?branch=dev&event=push)](https://github.com/openiddict/openiddict-core/actions?query=workflow%3Abuild+branch%3Adev+event%3Apush)

## 什么是 OpenIddict？

OpenIddict 旨在提供一个**多功能的解决方案**，以在**任何 ASP.NET Core 2.1（及更高版本）应用程序中实现 OpenID Connect 客户端、服务器和令牌验证支持**。
**同时，由于使用了原生的 Microsoft.Owin 4.2 集成，因此也完全支持 ASP.NET 4.6.1（及更高版本）应用程序**。

OpenIddict 完全支持 **[code/implicit/hybrid flows](http://openid.net/specs/openid-connect-core-1_0.html)**、 **[client credentials/resource owner password grants](https://tools.ietf.org/html/rfc6749)** 以及 [device authorization flow](https://tools.ietf.org/html/rfc8628).

OpenIddict 原生支持 **[Entity Framework Core](https://www.nuget.org/packages/OpenIddict.EntityFrameworkCore)**、
**[Entity Framework 6](https://www.nuget.org/packages/OpenIddict.EntityFramework)** 和 **[MongoDB](https://www.nuget.org/packages/OpenIddict.MongoDb)**
，并且可以实现自定义存储来支持其他提供者。

--------------

## 快速上手

**强烈建议寻求简单易用的解决方案的开发者使用 [OrchardCore and its OpenID module](https://docs.orchardcore.net/en/latest/docs/reference/modules/OpenId/)**，
该模块基于 OpenIddict 构建，提供了合理的默认设置，并提供了内置管理 GUI 以便轻松注册 OpenID 客户端应用程序。

**如果要使用 OpenIddict 实现自定义的 OpenID Connect 服务器，请阅读 [快速上手](https://documentation.openiddict.com/guides/getting-started.html)**.

**演示如何将 OpenIddict 与不同的 OAuth 2.0/OpenID Connect 流程结合使用的示例**
可以在[专用存储库](https://github.com/openiddict/openiddict-samples)中找到。

--------------

## 兼容性矩阵

| Web 框架版本 | .NET 运行时版本 | OpenIddict 4.x                          |
|-----------------------|----------------------|-----------------------------------------|
| ASP.NET Core 2.1      | .NET Framework 4.6.1 | :heavy_check_mark: :information_source: |
| ASP.NET Core 2.1      | .NET Framework 4.7.2 | :heavy_check_mark:                      |
| ASP.NET Core 2.1      | .NET Framework 4.8   | :heavy_check_mark:                      |
| ASP.NET Core 2.1      | .NET Core 2.1        | :exclamation:                           |
|                       |                      |                                         |
| ASP.NET Core 3.1      | .NET Core 3.1        | :heavy_check_mark:                      |
|                       |                      |                                         |
| ASP.NET Core 5.0      | .NET 5.0             | :exclamation:                           |
| ASP.NET Core 6.0      | .NET 6.0             | :heavy_check_mark:                      |
| ASP.NET Core 7.0      | .NET 7.0             | :heavy_check_mark:                      |
|                       |                      |                                         |
| Microsoft.Owin 4.2    | .NET Framework 4.6.1 | :heavy_check_mark: :information_source: |
| Microsoft.Owin 4.2    | .NET Framework 4.7.2 | :heavy_check_mark:                      |
| Microsoft.Owin 4.2    | .NET Framework 4.8   | :heavy_check_mark:                      |

:exclamation: **注意：Microsoft 不再支持在 .NET Core 2.1 上运行的 ASP.NET Core 2.1 和在 .NET 5.0 上运行的 ASP.NET Core 5.0。虽然 OpenIddict 4.x 仍然可以在 .NET Core 2.1 上使用，因为它兼容 .NET Standard 2.0，但强烈建议用户迁移到 ASP.NET Core/.NET 6.0**。
ASP.NET Core 2.1 在 .NET Framework 4.6.1（及更高版本）上仍然得到充分支持。

:information_source: **注意：当目标框架为 .NET Framework 4.6.1 时，以下功能不可用**：
 - X.509 开发加密/签名证书：调用 `AddDevelopmentEncryptionCertificate()` 或 `AddDevelopmentSigningCertificate()`
如果找不到有效的开发证书并且必须生成新证书，将导致在运行时抛出 `PlatformNotSupportedException`。
 - X.509 ECDSA 签名证书/密钥：调用 `AddSigningCertificate()` 或 `AddSigningKey()`
使用 ECDSA 证书/密钥将始终导致在运行时抛出 `PlatformNotSupportedException`。

--------------

## 认证

与许多其他身份提供者不同，**OpenIddict不是一个即插即用的解决方案，而是需要编写自定义代码**才能运行（通常至少需要一个授权控制器），这使它成为认证计划的不良候选者。

虽然参考实现可以按原样提交，但**这并不能保证OpenIddict用户部署的实现符合标准**。

相反，鼓励**开发人员在实现自己的逻辑后，针对自己的部署**执行一致性测试。

> 样例代码仓库包含一个专门为 OpenID Connect 提供者认证工具设计的[专用示例](https://github.com/openiddict/openiddict-samples/tree/dev/samples/Contruum/Contruum.Server) ，以展示 OpenIddict 可以轻松用于认证实现的认证。为了尽快执行认证测试，该示例不包括任何成员身份或同意功能（对于需要在身份之间切换的测试提供了两个硬编码的身份）。

--------------

## 资源

**你正在寻找更多资源来帮助你开始使用 OpenIddict 吗？** 不要错过这些有趣的博客文章：

- **[OpenIddict 4.0 preview1 is out](https://kevinchalet.com/2022/06/22/openiddict-4-0-preview1-is-out/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[介绍 OpenIddict 提供程序](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers/issues/694)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[介绍 OpenIddict 客户端](https://kevinchalet.com/2022/02/25/introducing-the-openiddict-client/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[使用 BFF 和 OpenIddict 来保护 Blazor WASM ASP.NET Core 托管应用程序](https://damienbod.com/2022/01/03/secure-a-blazor-wasm-asp-net-core-hosted-app-using-bff-and-openiddict/)** by [Damien Bowden](https://github.com/damienbod)
- **[如何使用 Virto Commerce B2B 电子商务使用 OpenIddict 保护 ASP.NET 核心应用程序：技术案例研究](https://virtocommerce.com/blog/how-to-secure-aspnet-core-applications-with-openiddict-using-virto-commerce-platform)** by [Virto Commerce](https://virtocommerce.com/)
- **[OpenIddict 3.0 正式发布](https://kevinchalet.com/2020/12/23/openiddict-3-0-general-availability/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[Setting up an Authorization Server with OpenIddict](https://dev.to/robinvanderknaap/setting-up-an-authorization-server-with-openiddict-part-i-introduction-4jid)** by [Robin van der Knaap](https://dev.to/robinvanderknaap)
- **[Introducing OpenIddict 3.0's first release candidate version](https://kevinchalet.com/2020/11/17/introducing-openiddict-3-0-s-first-release-candidate-version/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[OpenIddict 3.0 beta6 is out](https://kevinchalet.com/2020/10/27/openiddict-3-0-beta6-is-out/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[Introducing Quartz.NET support and new languages in OpenIddict 3.0 beta4](https://kevinchalet.com/2020/10/02/introducing-quartz-net-support-and-new-languages-in-openiddict-3-0-beta4/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[Introducing localization support in OpenIddict 3.0 beta3](https://kevinchalet.com/2020/08/03/introducing-localization-support-in-openiddict-3-0-beta3/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[OpenIddict 3.0 beta2 is out](https://kevinchalet.com/2020/07/08/openiddict-3-0-beta2-is-out/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[Introducing OpenIddict 3.0 beta1](https://kevinchalet.com/2020/06/11/introducing-openiddict-3-0-beta1/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[Adding OpenIddict 3.0 to an OWIN application](https://kevinchalet.com/2020/03/03/adding-openiddict-3-0-to-an-owin-application/)** by [Kévin Chalet](https://github.com/kevinchalet)
- **[Creating an OpenID Connect server proxy with OpenIddict 3.0's degraded mode](https://kevinchalet.com/2020/02/18/creating-an-openid-connect-server-proxy-with-openiddict-3-0-s-degraded-mode/)** by [Kévin Chalet](https://github.com/kevinchalet)

**由第三方维护的基于 OpenIddict 的项目**:

- **[OrchardCore OpenID module](https://github.com/OrchardCMS/OrchardCore)**: turnkey OpenID Connect server and token validation solution, built with multitenancy in mind
- **[OpenIddict UI](https://github.com/thomasduft/openiddict-ui)** by [Thomas Duft](https://github.com/thomasduft): headless UI for managing client applications and scopes
- **[P41.OpenIddict.CouchDB](https://github.com/panoukos41/couchdb-openiddict)** by [Panos Athanasiou](https://github.com/panoukos41): CouchDB stores for OpenIddict
- **[pixel-identity](https://github.com/Nfactor26/pixel-identity)** by [Nishant Singh](https://github.com/Nfactor26): Ready to host OpenID Connect service using OpenIddict and ASP.NET Identity with a Blazor-based UI for managing users, roles, applications and scopes with support for multiple databases.

--------------

## 安全策略

安全问题和漏洞应该通过发送电子邮件至security@openiddict.com进行私下报告。
您应该在24小时内收到回复。如果由于某些原因您没有收到回复，请通过电子邮件跟进以确保我们收到了您的原始消息。

--------------

## 支持

如果需要支持，请在创建 GitHub 工单之前确保 [赞助该项目](https://github.com/sponsors/kevinchalet) .
如果您不是赞助者，可以在 Gitter 或 StackOverflow 上发布您的问题：

- **Gitter: [https://gitter.im/openiddict/openiddict-core](https://gitter.im/openiddict/openiddict-core)**
- **StackOverflow: [https://stackoverflow.com/questions/tagged/openiddict](https://stackoverflow.com/questions/tagged/openiddict)**

--------------

## 夜间构建

如果您想尝试最新的功能和错误修复，可以使用 OpenIddict 的夜间构建 MyGet feed。
要引用 OpenIddict MyGet feed，请**创建一个 `NuGet.config` 文件**（位于解决方案的根目录下）：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget" value="https://api.nuget.org/v3/index.json" />
    <add key="openiddict" value="https://www.myget.org/F/openiddict/api/v3/index.json" />
  </packageSources>
</configuration>
```

--------------

## 贡献者

**OpenIddict** 由 **[Kévin Chalet](https://github.com/kevinchalet)** 积极维护。 欢迎贡献。可以使用 pull requests 提交。

**特别感谢 [我们的赞助商](https://github.com/sponsors/kevinchalet#sponsors) 的大力支持：**:

<a href="https://volosoft.com/"><img src="https://volosoft.com/assets/logos/volosoft-logo-dark.svg" width="500px" alt="Volosoft logo" /></a>

<!-- sponsors --><a href="https://github.com/sebastienros"><img src="https://github.com/sebastienros.png" width="60px" alt="Sébastien Ros" /></a><a href="https://github.com/schmitch"><img src="https://github.com/schmitch.png" width="60px" alt="Schmitt Christian" /></a><a href="https://github.com/cryo75"><img src="https://github.com/cryo75.png" width="60px" alt="" /></a><a href="https://github.com/florianwachs"><img src="https://github.com/florianwachs.png" width="60px" alt="Florian Wachs" /></a><a href="https://github.com/SebastianStehle"><img src="https://github.com/SebastianStehle.png" width="60px" alt="Sebastian Stehle" /></a><a href="https://github.com/communicatie-cockpit"><img src="https://github.com/communicatie-cockpit.png" width="60px" alt="Communicatie Cockpit" /></a><a href="https://github.com/KeithT"><img src="https://github.com/KeithT.png" width="60px" alt="" /></a><a href="https://github.com/Skrypt"><img src="https://github.com/Skrypt.png" width="60px" alt="Jasmin Savard" /></a><a href="https://github.com/ThomasBjallas"><img src="https://github.com/ThomasBjallas.png" width="60px" alt="Thomas" /></a><a href="https://github.com/mcalasa"><img src="https://github.com/mcalasa.png" width="60px" alt="Michael Calasanz" /></a><a href="https://github.com/feededit"><img src="https://github.com/feededit.png" width="60px" alt="" /></a><a href="https://github.com/DigitalOpsDev"><img src="https://github.com/DigitalOpsDev.png" width="60px" alt="DigitalOps Co. Ltd." /></a><a href="https://github.com/jacojvv-dev"><img src="https://github.com/jacojvv-dev.png" width="60px" alt="Jaco Jansen van Vuuren" /></a><a href="https://github.com/EYERIDE-Fleet-Management-System"><img src="https://github.com/EYERIDE-Fleet-Management-System.png" width="60px" alt="EYERIDE Fleet Management System" /></a><a href="https://github.com/salimz1"><img src="https://github.com/salimz1.png" width="60px" alt="" /></a><a href="https://github.com/hypdeb"><img src="https://github.com/hypdeb.png" width="60px" alt="Julien Debache" /></a><a href="https://github.com/StanlyLife"><img src="https://github.com/StanlyLife.png" width="60px" alt="Stian Håve" /></a><a href="https://github.com/ravindUwU"><img src="https://github.com/ravindUwU.png" width="60px" alt="Ravindu Liyanapathirana" /></a><a href="https://github.com/dlandi"><img src="https://github.com/dlandi.png" width="60px" alt="HieronymusBlaze" /></a><a href="https://github.com/ahanoff"><img src="https://github.com/ahanoff.png" width="60px" alt="Akhan Zhakiyanov" /></a><a href="https://github.com/CorentinBrossutti1"><img src="https://github.com/CorentinBrossutti1.png" width="60px" alt="Corentin BROSSUTTI" /></a><a href="https://github.com/blowdart"><img src="https://github.com/blowdart.png" width="60px" alt="Barry Dorrans" /></a><a href="https://github.com/devqsrl"><img src="https://github.com/devqsrl.png" width="60px" alt="DevQ S.r.l." /></a><a href="https://github.com/dgxhubbard"><img src="https://github.com/dgxhubbard.png" width="60px" alt="" /></a><a href="https://github.com/verdie-g"><img src="https://github.com/verdie-g.png" width="60px" alt="Grégoire" /></a><a href="https://github.com/xperiandri"><img src="https://github.com/xperiandri.png" width="60px" alt="Andrii Chebukin" /></a><!-- sponsors -->

--------------

## License

该项目使用 **Apache 许可证** 进行许可。这意味着您可以自由使用、修改和分发该项目。
更多详情请参见 [http://www.apache.org/licenses/LICENSE-2.0.html](http://www.apache.org/licenses/LICENSE-2.0.html) 。
