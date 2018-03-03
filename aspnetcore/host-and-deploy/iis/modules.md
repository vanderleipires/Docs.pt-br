---
title: "Usar módulos do IIS com o ASP.NET Core"
author: guardrex
description: "Descobrir ativos e inativos módulos do IIS para aplicativos do ASP.NET Core e como gerenciar os módulos do IIS."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: a6610e33abdc3eafb5908728b3299e95e6e7183f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>Usar módulos do IIS com o ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aplicativos ASP.NET Core são hospedados pelo IIS em uma configuração de proxy reverso. Alguns dos módulos nativos do IIS e todos os módulos gerenciado do IIS não estão disponíveis para processar solicitações para aplicativos do ASP.NET Core. Em muitos casos, o ASP.NET Core oferece uma alternativa para os recursos gerenciados e nativos de módulos do IIS.

## <a name="native-modules"></a>Módulos nativos

| Módulo | .NET core ativo | Opção de núcleo do ASP.NET |
| ------ | :--------------: | ------------------- |
| **Autenticação anônima**<br>`AnonymousAuthenticationModule` | Sim | |
| **Autenticação básica**<br>`BasicAuthenticationModule` | Sim | |
| **Autenticação de mapeamento de certificação de cliente**<br>`CertificateMappingAuthenticationModule` | Sim | |
| **CGI**<br>`CgiModule` | Não | |
| **Validação de configuração**<br>`ConfigurationValidationModule` | Sim | |
| **Erros HTTP**<br>`CustomErrorModule` | Não | [Middleware de páginas de código de status](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Registro em log personalizado**<br>`CustomLoggingModule` | Sim | |
| **Documento padrão**<br>`DefaultDocumentModule` | Não | [Middleware de arquivos padrão](xref:fundamentals/static-files#serve-a-default-document) |
| **Autenticação Digest**<br>`DigestAuthenticationModule` | Sim | |
| **Pesquisa no Diretório**<br>`DirectoryListingModule` | Não | [Middleware de navegação de diretório](xref:fundamentals/static-files#enable-directory-browsing) |
| **Compactação dinâmica**<br>`DynamicCompressionModule` | Sim | [Middleware de compactação de resposta](xref:performance/response-compression) |
| **Rastreamento**<br>`FailedRequestsTracingModule` | Sim | [Registro do ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Cache de arquivo**<br>`FileCacheModule` | Não | [Middleware de Cache de Resposta](xref:performance/caching/middleware) |
| **Cache de HTTP**<br>`HttpCacheModule` | Não | [Middleware de Cache de Resposta](xref:performance/caching/middleware) |
| **Log HTTP**<br>`HttpLoggingModule` | Sim | [Registro do ASP.NET Core](xref:fundamentals/logging/index)<br>Implementações: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Redirecionamento de HTTP**<br>`HttpRedirectionModule` | Sim | [Middleware de regravação de URL](xref:fundamentals/url-rewriting) |
| **Autenticação de mapeamento de certificado de cliente do IIS**<br>`IISCertificateMappingAuthenticationModule` | Sim | |
| **Restrições de IP e domínio**<br>`IpRestrictionModule` | Sim | |
| **Filtros ISAPI**<br>`IsapiFilterModule` | Sim | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Sim | [Middleware](xref:fundamentals/middleware/index) |
| **Suporte de protocolo**<br>`ProtocolSupportModule` | Sim | |
| **Filtragem de Solicitações**<br>`RequestFilteringModule` | Sim | [Middleware de regravação de URL `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Monitor de Solicitações**<br>`RequestMonitorModule` | Sim | |
| **Regravação de URL**<br>`RewriteModule` | Yes&#8224; | [Middleware de regravação de URL](xref:fundamentals/url-rewriting) |
| **Inclusões do lado do servidor**<br>`ServerSideIncludeModule` | Não | |
| **Compactação estática**<br>`StaticCompressionModule` | Não | [Middleware de compactação de resposta](xref:performance/response-compression) |
| **Conteúdo Estático**<br>`StaticFileModule` | Não | [Middleware de arquivos estáticos](xref:fundamentals/static-files) |
| **Cache de token**<br>`TokenCacheModule` | Sim | |
| **Cache de URI**<br>`UriCacheModule` | Sim | |
| **Autorização de URL**<br>`UrlAuthorizationModule` | Sim | [Identidade do ASP.NET Core](xref:security/authentication/identity) |
| **Autenticação do Windows**<br>`WindowsAuthenticationModule` | Sim | |

&#8224; Do URL Rewrite Module `isFile` e `isDirectory` corresponde aos tipos não funciona com aplicativos do ASP.NET Core devido a alterações em [estrutura de diretórios](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Módulos gerenciados

| Módulo                  | .NET core ativo | Opção de núcleo do ASP.NET |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | Não               | |
| DefaultAuthentication   | Não               | |
| FileAuthorization       | Não               | |
| FormsAuthentication     | Não               | [Middleware de autenticação de cookie](xref:security/authentication/cookie) |
| OutputCache             | Não               | [Middleware de Cache de Resposta](xref:performance/caching/middleware) |
| Perfil                 | Não               | |
| RoleManager             | Não               | |
| ScriptModule-4.0        | Não               | |
| Session                 | Não               | [Middleware de sessão](xref:fundamentals/app-state) |
| UrlAuthorization        | Não               | |
| UrlMappingsModule       | Não               | [Middleware de regravação de URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | Não               | [Identidade do ASP.NET Core](xref:security/authentication/identity) |
| WindowsAuthentication   | Não               | |

## <a name="iis-manager-application-changes"></a>Alterações de aplicativo do Gerenciador do IIS

Ao usar o Gerenciador do IIS para definir as configurações, o *Web. config* o arquivo do aplicativo é alterado. Se a implantação de um aplicativo e incluindo *Web. config*, todas as alterações feitas com o Gerenciador do IIS são substituídas por implantado *Web. config* arquivo. Se forem feitas alterações para o servidor *Web. config* de arquivo, copie o atualizada *Web. config* arquivo no servidor para o local do projeto imediatamente.

## <a name="disabling-iis-modules"></a>Desabilitando módulos do IIS

Se um módulo do IIS é configurado no nível do servidor deve ser desabilitado para um aplicativo, um acréscimo para o aplicativo *Web. config* arquivo pode desabilitar o módulo. Deixe o módulo no lugar e desativá-lo usando um parâmetro de configuração (se disponível) ou remover o módulo do aplicativo.

### <a name="module-deactivation"></a>Desativação do módulo

Muitos módulos oferecem uma configuração que permite que eles sejam desabilitadas sem remover o módulo do aplicativo. Essa é a maneira mais simples e rápida para desativar um módulo. Por exemplo, o módulo de reescrita de URL do IIS pode ser desabilitado com o  **\<httpRedirect >** elemento *Web. config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Para obter mais informações sobre como desativar módulos com definições de configuração, siga os links a *elementos filho* seção [IIS \<System. webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Remoção do módulo

Se a opção para remover um módulo com uma configuração em *Web. config*, desbloqueie o módulo e desbloquear o  **\<módulos >** seção *Web. config* primeiro:

1. Desbloquear o módulo no nível do servidor. Selecione o servidor do IIS no Gerenciador do IIS **conexões** barra lateral. Abra o **módulos** no **IIS** área. Selecione o módulo na lista. No **ações** barra lateral à direita, selecione **Unlock**. Desbloquear tantos módulos que você planeja remover de *Web. config* mais tarde.

1. Implantar o aplicativo sem um  **\<módulos >** seção *Web. config*. Se um aplicativo é implantado com um *Web. config* que contém o  **\<módulos >** seção sem ter a seção desbloqueada pela primeira vez no Gerenciador do IIS, o Configuration Manager gera uma exceção ao tentar desbloquear a seção. Portanto, implante o aplicativo sem um  **\<módulos >** seção.

1. Desbloquear o  **\<módulos >** seção *Web. config*. No **conexões** barra lateral, selecione o site em **Sites**. No **gerenciamento** área, abra o **Editor de configuração**. Use os controles de navegação para selecionar o `system.webServer/modules` seção. No **ações** barra lateral à direita, selecione a **desbloquear** a seção.

1. Neste ponto, um  **\<módulos >** seção pode ser adicionada para o *Web. config* arquivo com um  **\<remover >** elemento para remover o módulo de o aplicativo. Vários  **\<remover >** elementos podem ser adicionados para remover vários módulos. Se *Web. config* alterações são feitas no servidor, imediatamente fazer as mesmas alterações no projeto *Web. config* arquivo localmente. Remover um módulo dessa maneira não afetará o uso do módulo com outros aplicativos no servidor.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Para uma instalação do IIS com os módulos padrão instalado, use a seguinte  **\<módulo >** seção para remover os módulos padrão.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

Um módulo do IIS também pode ser removido com *Appcmd.exe*. Forneça o `MODULE_NAME` e `APPLICATION_NAME` no comando:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Por exemplo, remover o `DynamicCompressionModule` do Site da Web padrão:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configuração do módulo mínimo

Somente módulos necessários para executar um aplicativo ASP.NET Core são o módulo de autenticação anônima e o módulo do ASP.NET Core.

![O Gerenciador do IIS abrir a módulos com a configuração de módulo mínimo mostrada](modules/_static/modules.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedar no Windows com o IIS](xref:host-and-deploy/iis/index)
* [Introdução às arquiteturas IIS: módulos no IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Visão geral de módulos do IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personalizando 7.0 funções e módulos do IIS](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
