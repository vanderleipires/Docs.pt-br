---
uid: webhooks/receiving/receivers
title: Receptores de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Receptores de WebHooks do ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 376cb3e3fdc0bc7bd248da1f57e1064fb27b3cef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830586"
---
# <a name="aspnet-webhooks-receivers"></a>Receptores de WebHooks do ASP.NET

Receber WebHooks depende de quem é o remetente. Às vezes, há etapas adicionais, registrando um WebHook verificando que o assinante está realmente escutando. Alguns WebHooks fornecem um modelo de push e pull em que a solicitação HTTP POST contém apenas uma referência para as informações de evento que é, em seguida, a serem recuperados de forma independente. Geralmente, o modelo de segurança varia um pouco.

A finalidade de WebHooks de ASP.NET da Microsoft é torná-la mais simples e mais consistente para conectar sua API sem gastar muito tempo descobrindo como lidar com qualquer variante específico de WebHooks.

Um receptor de WebHook é responsável por aceitar e verificar WebHooks de um remetente específico. Um receptor de WebHook pode dar suporte a qualquer número de WebHooks, cada um com sua própria configuração. Por exemplo, o receptor de WebHook do GitHub pode aceitar WebHooks de qualquer número de repositórios do GitHub.

## <a name="webhook-receiver-uris"></a>URIs de receptor de WebHook

Instalando o Microsoft ASP.NET WebHooks, você obtém um controlador de WebHook geral que aceita solicitações de WebHook de um número em aberto de serviços. Quando uma solicitação chega, ele escolhe o receptor apropriado que você instalou para lidar com um remetente de WebHook específico.

O URI deste controlador é o URI do WebHook que você registrar com o serviço e a forma:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de segurança, muitos receptores de WebHook exigem que o URI é um *https* URI e em alguns casos, ele também deverá conter um parâmetro de consulta adicionais que é usado para impor que apenas a parte pretendida pode enviar WebHooks para o URI acima .

O <em> <receiver> </em> componente é o nome do destinatário, por exemplo <em>github</em> ou <em>slack</em>.

O *{id}* é um identificador opcional que pode ser usado para identificar uma determinada configuração de receptor de WebHook. Isso pode ser usado para registrar WebHooks N com um receptor em particular. Por exemplo, os URIs de três a seguir pode ser usado para se registrar para três WebHooks independentes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalando um receptor de WebHook

Para receber os WebHooks usando WebHooks do Microsoft ASP.NET, você primeiro instala o pacote do Nuget para o provedor de WebHook ou os provedores que você deseja receber WebHooks de. Os pacotes do Nuget são nomeados [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) onde a última parte indica o serviço de suporte. Por exemplo

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornece suporte para o recebimento de WebHooks do GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornece suporte para o recebimento de WebHooks gerado pelo ASP. WebHooks de NET.

Fora da caixa, você pode encontrar suporte para o Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, distribuição, Trello e WordPress, mas é possível oferecer suporte a qualquer número de outros provedores.

## <a name="configuring-a-webhook-receiver"></a>Configurando um receptor de WebHook

Receptores de WebHook são configurados por meio de [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interface e implementações específicas dessa interface podem ser registradas usando qualquer modelo de injeção de dependência. A implementação padrão usa as configurações de aplicativo que pode ser definida no arquivo Web. config ou, se usar aplicativos Web do Azure, pode ser definido por meio de [Portal do Azure](https://portal.azure.com/).

![Configurações de aplicativo do Azure](_static/AzureAppSettings.png)

O formato para as chaves de configuração de aplicativo é da seguinte maneira:

```
MS_WebHookReceiverSecret_<receiver>
```

O valor é uma lista separada por vírgulas de valores que corresponde a *{id}* valores para os quais WebHooks tiver sido registrados, por exemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializando um receptor de WebHook

Receptores de WebHook são inicializados, registrando-os, normalmente na *WebApiConfig* classe estática, por exemplo:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
