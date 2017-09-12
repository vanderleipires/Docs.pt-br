---
title: "Configurar a autenticação do Windows no núcleo do ASP.NET"
author: ardalis
description: "Como configurar a autenticação do Windows no núcleo do ASP.NET"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurar a autenticação do Windows no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com)

Autenticação do Windows pode ser configurada para aplicativos ASP.NET Core hospedados com o IIS ou WebListener.

## <a name="what-is-windows-authentication"></a>O que é autenticação do Windows

Autenticação do Windows se baseia no sistema operacional para autenticar os usuários de aplicativos do ASP.NET Core. Você pode usar a autenticação do Windows quando o servidor é executado em uma rede corporativa usando identidades do domínio do Active Directory ou outras contas do Windows para identificar os usuários. Autenticação do Windows é uma forma segura de autenticação melhor adequada para ambientes de intranet onde os usuários, os aplicativos cliente e servidores web pertencem ao mesmo domínio do Windows.

[Saiba mais sobre a autenticação do Windows e instalá-lo para o IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>Habilitar a autenticação do Windows em um aplicativo do ASP.NET Core

O modelo de aplicativo de Web do Visual Studio pode ser configurado para dar suporte à autenticação do Windows.

### <a name="using-the-windows-authentication-app-template"></a>Usando o modelo de aplicativo de autenticação do Windows

No Visual Studio:
* Crie um novo Aplicativo Web ASP.NET Core. 
* Selecione o aplicativo Web da lista de modelos.
* Selecione o botão Alterar autenticação e selecione **autenticação do Windows**. 

Execute o aplicativo. O nome de usuário aparece no canto superior direito do aplicativo.

![Tela de navegador de autenticação do Windows](windowsauth/_static/browser-screenshot.png)

Para o trabalho de desenvolvimento usando o IIS Express, o modelo fornece todas as configurações necessárias para usar a autenticação do Windows. A próxima seção mostra como configurar um aplicativo ASP.NET Core manualmente para a autenticação do Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Configurações do Visual Studio para Windows e autenticação anônima

Página de propriedades do Visual Studio, a guia depurar fornece caixas de seleção para autenticação anônima e autenticação do Windows.

![Tela de navegador de autenticação do Windows](windowsauth/_static/vs-auth-property-menu.png)

Você também pode configurar essas propriedades no `launchSettings.json` arquivo:

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>Habilitar a autenticação do Windows com o IIS

O IIS usa o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicativos ASP.NET Core. A ANCM fluxos autenticação do Windows para o IIS por padrão. Configuração da autenticação do Windows é executada no IIS, não o projeto de aplicativo. As seções a seguir mostram como usar o Gerenciador do IIS para configurar um aplicativo ASP.NET Core para usar a autenticação do Windows:

### <a name="create-a-new-iis-site"></a>Criar um novo site do IIS

Especifique um nome e uma pasta e permitir que ele criar um novo pool de aplicativos.

### <a name="customize-authentication"></a>Personalizar a autenticação

Abra o menu de autenticação para o site.

![Menu de autenticação do IIS](windowsauth/_static/iis-authentication-menu.png)

Desabilitar a autenticação anônima e habilitar a autenticação do Windows.

![Configurações de autenticação do IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publicar seu projeto para a pasta de site do IIS

Usando o Visual Studio ou o .NET Core CLI, *publicar* seu aplicativo para a pasta de destino.

![Caixa de diálogo de publicação do Visual Studio](windowsauth/_static/vs-publish-app.png)

Saiba mais sobre [publicar no IIS](xref:publishing/iis).

Inicie o aplicativo para verificar a autenticação do Windows está funcionando.

## <a name="enabling-windows-authentication-with-weblistener"></a>Habilitar a autenticação do Windows com WebListener

Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [WebListener](xref:fundamentals/servers/weblistener) para dar suporte a cenários de hospedagem interna no Windows. O exemplo a seguir configura o host de web do aplicativo para usar WebListener com autenticação do Windows:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>Trabalhando com autenticação do Windows

Se seu aplicativo usa autenticação do Windows e acesso anônimo, você pode usar o ``[Authorize]`` e ``[AllowAnonymous]`` atributos. Aplicativos que não têm anônima habilitado não exigem ``[Authorize]``; o aplicativo é tratado como exigir autenticação, solicitações anônimas são rejeitadas. Observe que, se o site do IIS é configurado **não** para permitir acesso anônimo, o ``[AllowAnonymous]`` atributo faz **não** permitir solicitações anônimas. O ``[AllowAnonymous]`` atributo substituições ``[Authorize]`` atributo uso em aplicativos que permitem o acesso anônimo.

### <a name="impersonation"></a>Representação

ASP.NET Core não implementa a representação. Os aplicativos executados com a identidade de aplicativo para todas as solicitações, usando a identidade de processo ou pool de aplicativo. Se você precisar executar explicitamente uma ação em nome do usuário, use ``WindowsIdentity.RunImpersonated``. Executar uma única ação neste contexto e, em seguida, feche o contexto. Observe que ``RunImpersonated`` não oferece suporte a assíncrona e não deve ser usado para cenários complexos. Por exemplo, solicitações de inteiras ou cadeias de middleware de encapsulamento não é suportado nem recomendado.
