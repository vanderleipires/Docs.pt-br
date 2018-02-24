---
title: "Configurar a autenticação do Windows no núcleo do ASP.NET"
author: ardalis
description: "Este artigo descreve como configurar a autenticação do Windows no ASP.NET Core, usando o IIS, IIS Express, o HTTP. sys e WebListener."
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: c229537e7f533eea2173dbc51b8d0d0e097d434a
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/23/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>Configurar a autenticação do Windows em um aplicativo do ASP.NET Core

Por [Steve Smith](https://ardalis.com) e [Scott Addie](https://twitter.com/Scott_Addie)

Autenticação do Windows pode ser configurada para aplicativos ASP.NET Core hospedados no IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), ou [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>O que é autenticação do Windows?

Autenticação do Windows se baseia no sistema operacional para autenticar os usuários de aplicativos do ASP.NET Core. Você pode usar a autenticação do Windows quando o servidor é executado em uma rede corporativa usando identidades do domínio do Active Directory ou outras contas do Windows para identificar os usuários. Autenticação do Windows é mais adequada para ambientes de intranet em que os usuários, os aplicativos cliente e servidores web pertencem ao mesmo domínio do Windows.

[Saiba mais sobre a autenticação do Windows e instalá-lo para o IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Habilitar a autenticação do Windows em um aplicativo do ASP.NET Core

O modelo de aplicativo de Web do Visual Studio pode ser configurado para dar suporte à autenticação do Windows.

### <a name="use-the-windows-authentication-app-template"></a>Use o modelo de aplicativo de autenticação do Windows

No Visual Studio:
1. Crie um novo Aplicativo Web ASP.NET Core. 
1. Selecione o aplicativo Web da lista de modelos.
1. Selecione o **alterar autenticação** botão e selecione **autenticação do Windows**. 

Execute o aplicativo. O nome de usuário aparece no canto superior direito do aplicativo.

![Tela de navegador de autenticação do Windows](windowsauth/_static/browser-screenshot.png)

Para o trabalho de desenvolvimento usando o IIS Express, o modelo fornece todas as configurações necessárias para usar a autenticação do Windows. A seção a seguir mostra como configurar manualmente um aplicativo ASP.NET Core para autenticação do Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Configurações do Visual Studio para Windows e autenticação anônima

O projeto do Visual Studio **propriedades** da página **depurar** guia fornece caixas de seleção para autenticação anônima e autenticação do Windows.

![Tela de navegador de autenticação do Windows](windowsauth/_static/vs-auth-property-menu.png)

Como alternativa, essas duas propriedades podem ser definidas no *launchSettings.json* arquivo:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Habilitar a autenticação do Windows com o IIS

O IIS usa o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicativos ASP.NET Core. A ANCM fluxos autenticação do Windows para o IIS por padrão. Configuração da autenticação do Windows é executada no IIS, não o projeto de aplicativo. As seções a seguir mostram como usar o Gerenciador do IIS para configurar um aplicativo ASP.NET Core para usar a autenticação do Windows.

### <a name="create-a-new-iis-site"></a>Criar um novo site do IIS

Especifique um nome e uma pasta e permitir que ele criar um novo pool de aplicativos.

### <a name="customize-authentication"></a>Personalizar a autenticação

Abra o menu de autenticação para o site.

![Menu de autenticação do IIS](windowsauth/_static/iis-authentication-menu.png)

Desabilitar a autenticação anônima e habilitar a autenticação do Windows.

![Configurações de autenticação do IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publicar seu projeto para a pasta de site do IIS

Usando o Visual Studio ou o .NET Core CLI, publica o aplicativo para a pasta de destino.

![Caixa de diálogo de publicação do Visual Studio](windowsauth/_static/vs-publish-app.png)

Saiba mais sobre [publicar no IIS](xref:host-and-deploy/iis/index).

Inicie o aplicativo para verificar a autenticação do Windows está funcionando.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Habilitar a autenticação do Windows com o HTTP.sys ou WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [HTTP.sys](xref:fundamentals/servers/httpsys) para dar suporte a cenários de hospedagem interna no Windows. O exemplo a seguir configura o host de web do aplicativo para usar o HTTP. sys com autenticação do Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [WebListener](xref:fundamentals/servers/weblistener) para dar suporte a cenários de hospedagem interna no Windows. O exemplo a seguir configura o host de web do aplicativo para usar WebListener com autenticação do Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Trabalhar com a autenticação do Windows

O estado de configuração do acesso anônimo determina o modo no qual o `[Authorize]` e `[AllowAnonymous]` os atributos são usados no aplicativo. As seções a seguir explicam como lidar com os estados de configuração permitidas e não permitidas do acesso anônimo.

### <a name="disallow-anonymous-access"></a>Não permitir acesso anônimo

Quando a autenticação do Windows está habilitada e o acesso anônimo é desabilitado, o `[Authorize]` e `[AllowAnonymous]` atributos não têm nenhum efeito. Se o site do IIS (ou servidor HTTP. sys ou WebListener) é configurado para não permitir acesso anônimo, a solicitação nunca atinge seu aplicativo. Por esse motivo, o `[AllowAnonymous]` atributo não é aplicável.

### <a name="allow-anonymous-access"></a>Permitir o acesso anônimo

Quando a autenticação do Windows e o acesso anônimo estão habilitados, use o `[Authorize]` e `[AllowAnonymous]` atributos. O `[Authorize]` atributo permite proteger partes do aplicativo que realmente exigem a autenticação do Windows. O `[AllowAnonymous]` atributo substituições `[Authorize]` atributo uso em aplicativos que permitem o acesso anônimo. Consulte [autorização simples](xref:security/authorization/simple) para obter detalhes de uso do atributo.

No núcleo do ASP.NET 2. x, o `[Authorize]` atributo requer configuração adicional no *Startup.cs* desafiar solicitações anônimas para a autenticação do Windows. A configuração recomendada varia ligeiramente com base no servidor web que está sendo usado.

> [!NOTE]
> Por padrão, os usuários que não têm autorização para acessar uma página são apresentados com um documento em branco. O [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) podem ser configurados para fornecer aos usuários uma experiência melhor de "Acesso negado".

#### <a name="iis"></a>IIS

Se usar o IIS, adicione o seguinte para o `ConfigureServices` método: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Se usar o HTTP. sys, adicione o seguinte para o `ConfigureServices` método:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Representação

ASP.NET Core não implementa a representação. Os aplicativos executados com a identidade de aplicativo para todas as solicitações, usando a identidade de processo ou pool de aplicativo. Se você precisar executar explicitamente uma ação em nome do usuário, use `WindowsIdentity.RunImpersonated`. Executar uma única ação neste contexto e, em seguida, feche o contexto.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Observe que `RunImpersonated` não dá suporte a operações assíncronas e não deve ser usado para cenários complexos. Por exemplo, solicitações inteiras ou cadeias de middleware de encapsulamento não é suportado ou recomendado.

---
