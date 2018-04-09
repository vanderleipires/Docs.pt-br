---
uid: webhooks/receiving/receivers
title: Receptores de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Receptores de WebHooks do ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-webhooks-receivers"></a>Receptores de WebHooks do ASP.NET

Receber WebHooks depende de quem é o remetente. Às vezes, há etapas adicionais Registrando um WebHook verificando se o assinante realmente está escutando. Alguns WebHooks fornecem um modelo de push-pull em que a solicitação HTTP POST contém apenas uma referência para as informações de evento que é, em seguida, ser recuperadas de forma independente. Geralmente, o modelo de segurança varia um pouco.

O objetivo do Microsoft ASP.NET WebHooks é torná-la mais simples e mais consistente para conectar sua API sem gastar muito tempo para descobrir como tratar qualquer variante específico de WebHooks.

Um destinatário de WebHook é responsável para aceitar e verificar WebHooks de um remetente específico. Um destinatário de WebHook pode dar suporte a qualquer número de WebHooks, cada um com sua própria configuração. Por exemplo, o receptor GitHub WebHook pode aceitar WebHooks de qualquer número de repositórios GitHub.

## <a name="webhook-receiver-uris"></a>URIs de destinatário do WebHook

Instalando o Microsoft ASP.NET WebHooks você obter um controlador de WebHook geral que aceita solicitações de WebHook de um número aberta de serviços. Quando uma solicitação chega, ele escolhe o receptor apropriado que você instalou para lidar com um remetente de WebHook específico.

O URI deste controlador é o URI de WebHook registrar com o serviço e é da forma:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de segurança, muitos receptores de WebHook exigem que o URI é um *https* URI e em alguns casos, ele também deverá conter um parâmetro de consulta adicional que é usado para impor que apenas a parte pretendida pode enviar WebHooks para o URI acima .

O <em> <receiver> </em> componente é o nome do destinatário, por exemplo <em>github</em> ou <em>slack</em>.

O *{id}* é um identificador opcional que pode ser usado para identificar uma determinada configuração de destinatário do WebHook. Isso pode ser usado para registrar N WebHooks com um destinatário específico. Por exemplo, os URIs de três a seguir pode ser usado para registrar para três WebHooks independentes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalando um receptor do WebHook

Para receber WebHooks usando o Microsoft ASP.NET WebHooks, você primeiro instalar o pacote do Nuget para o provedor de WebHook ou provedores que você deseja receber WebHooks de. Os pacotes do Nuget são nomeados [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) onde a última parte indica que o serviço de suporte. Por exemplo

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornece suporte para recebimento WebHooks do GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornece suporte para recebimento WebHooks gerado pelo ASP. WebHooks NET.

Fora da caixa, você pode encontrar suporte para o Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, margem de atraso, distribuição, Trello e WordPress, mas é possível dar suporte a qualquer número de outros provedores.

## <a name="configuring-a-webhook-receiver"></a>Configurando um receptor do WebHook

Receptores de WebHook são configurados por meio de [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) específicas implementações dessa interface e interface podem ser registradas com qualquer modelo de injeção de dependência. A implementação padrão usa as configurações de aplicativo que pode ser definida no arquivo Web. config, ou, se o uso de aplicativos Web do Azure, pode ser definida por meio de [Portal do Azure](https://portal.azure.com/).

![Configurações do aplicativo do Azure](_static/AzureAppSettings.png)

O formato para chaves de configuração de aplicativo é o seguinte:

```
MS_WebHookReceiverSecret_<receiver>
```

O valor é uma lista separada por vírgulas de valores que correspondem a *{id}* valores para os quais WebHooks tiver sido registrados, por exemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializando um receptor do WebHook

Receptores de WebHook são inicializados registrando-os, normalmente no *WebApiConfig* classe estática, por exemplo:

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
