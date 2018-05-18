---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: Descobrir o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)

Este artigo descreve [Visual Studio](https://www.visualstudio.com/vs/) suporte para depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server. Este tópico explica como habilitar esse recurso e configuração de um projeto.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Habilitar o IIS

1. Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).
1. Selecione o **Internet Information Services** caixa de seleção.

![Caixa de seleção do mostrando recursos do Windows Internet Information Services marcada como um quadrado branco (não uma marca de seleção) indicando que alguns dos recursos do IIS estão habilitadas](development-time-iis-support/_static/enable_iis.png)

A instalação do IIS pode exigir uma reinicialização do sistema.

## <a name="configure-iis"></a>Configurar o IIS

O IIS deve ter um site configurado com o seguinte:

* Nome do host de um nome de host que corresponda a URL do perfil de inicialização do aplicativo.
* Associação para a porta 443, com um certificado atribuídos.

Por exemplo, o **nome de Host** para um site adicional é definido como "localhost" (o perfil de lançamento também usam "localhost" neste tópico). A porta é definida como "443" (HTTPS). O **certificado do IIS Express desenvolvimento** é atribuído ao site, mas nenhum certificado válido funciona:

![Adicione janela de site no IIS, mostrando o conjunto de associação para o localhost na porta 443 com um certificado atribuídos.](development-time-iis-support/_static/add-website-window.png)

Se a instalação do IIS já tiver um **Default Web Site** com um nome de host que coincide com o nome de host URL do perfil de inicialização do aplicativo:

* Adicione uma associação de porta para a porta 443 (HTTPS).
* Atribua um certificado válido para o site.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Habilitar o suporte do IIS de tempo de desenvolvimento no Visual Studio

1. Inicie o instalador do Visual Studio.
1. Selecione o **IIS suporte de tempo de desenvolvimento** componente. O componente está listado como opcionais no **resumo** painel para o **desenvolvimento ASP.NET e web** carga de trabalho. O componente instala o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos por trás do IIS do ASP.NET Core em uma configuração de proxy reverso.

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada. Na seção Web e Nuvem, o painel ASP.NET e desenvolvimento Web é selecionado. À direita na área do painel de resumo opcional, há uma caixa de seleção para IIS suporte de tempo de desenvolvimento.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurar o projeto

### <a name="https-redirection"></a>Redirecionamento de HTTPS

Para um novo projeto, selecione a caixa de seleção para **configurar para HTTPS** no **novo aplicativo de Web do ASP.NET Core** janela:

![Nova janela de aplicativo da Web do ASP.NET Core com a configuração para a caixa de seleção HTTPS.](development-time-iis-support/_static/new-app.png)

Em um projeto existente, use HTTPS Middleware de redirecionamento no `Startup.Configure` chamando o [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) método de extensão:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Inicie o IIS perfil

Crie um novo perfil de lançamento para adicionar suporte ao IIS de tempo de desenvolvimento:

1. Para **perfil**, selecione o **novo** botão. Nome do perfil "IIS" na janela pop-up. Selecione **Okey** para criar o perfil.
1. Para o **iniciar** configuração, selecione **IIS** da lista.
1. Marque a caixa de seleção **Iniciar navegador** e forneça a URL de ponto de extremidade. Use o protocolo HTTPS. Este exemplo usa `https://localhost/WebApplication1`.
1. No **variáveis de ambiente** seção, selecione o **adicionar** botão. Fornecer uma variável de ambiente com uma chave de `ASPNETCORE_ENVIRONMENT` e um valor de `Development`.
1. No **configurações do servidor Web** área, defina o **URL do aplicativo**. Este exemplo usa `https://localhost/WebApplication1`.
1. Salve o perfil.

![Janela de propriedades de projeto com a guia Depurar selecionada. As configurações de Perfil e de Inicialização são definidas para o IIS. O recurso de navegador de inicialização está habilitado com um endereço de https://localhost/WebApplication1. O mesmo endereço também é fornecido no campo URL do aplicativo da área de configurações do servidor Web.](development-time-iis-support/_static/project_properties.png)

Como alternativa, um perfil de lançamento para adicionar manualmente o [launchSettings.json](http://json.schemastore.org/launchsettings) arquivo no aplicativo:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Execute o projeto

Na IU do VS, definido no botão executar o **IIS** perfil e selecione o botão para iniciar o aplicativo:

![Botão execute na barra de ferramentas VS definida para o perfil de "IIS".](development-time-iis-support/_static/toolbar.png)

O Visual Studio pode solicitar uma reinicialização se não executar como administrador. Se solicitado, reinicie o Visual Studio.

Se for usado um certificado não confiável de desenvolvimento, o navegador pode exigir a criação de uma exceção para o certificado não confiável.

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index)
* [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Impor o HTTPS](xref:security/enforcing-ssl)
