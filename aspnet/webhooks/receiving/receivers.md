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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="4ac9a-103">Receptores de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4ac9a-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="4ac9a-104">Receber WebHooks depende de quem é o remetente.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="4ac9a-105">Às vezes, há etapas adicionais Registrando um WebHook verificando se o assinante realmente está escutando.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="4ac9a-106">Alguns WebHooks fornecem um modelo de push-pull em que a solicitação HTTP POST contém apenas uma referência para as informações de evento que é, em seguida, ser recuperadas de forma independente.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="4ac9a-107">Geralmente, o modelo de segurança varia um pouco.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="4ac9a-108">O objetivo do Microsoft ASP.NET WebHooks é torná-la mais simples e mais consistente para conectar sua API sem gastar muito tempo para descobrir como tratar qualquer variante específico de WebHooks.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="4ac9a-109">Um destinatário de WebHook é responsável para aceitar e verificar WebHooks de um remetente específico.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="4ac9a-110">Um destinatário de WebHook pode dar suporte a qualquer número de WebHooks, cada um com sua própria configuração.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="4ac9a-111">Por exemplo, o receptor GitHub WebHook pode aceitar WebHooks de qualquer número de repositórios GitHub.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="4ac9a-112">URIs de destinatário do WebHook</span><span class="sxs-lookup"><span data-stu-id="4ac9a-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="4ac9a-113">Instalando o Microsoft ASP.NET WebHooks você obter um controlador de WebHook geral que aceita solicitações de WebHook de um número aberta de serviços.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="4ac9a-114">Quando uma solicitação chega, ele escolhe o receptor apropriado que você instalou para lidar com um remetente de WebHook específico.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="4ac9a-115">O URI deste controlador é o URI de WebHook registrar com o serviço e é da forma:</span><span class="sxs-lookup"><span data-stu-id="4ac9a-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="4ac9a-116">Por motivos de segurança, muitos receptores de WebHook exigem que o URI é um *https* URI e em alguns casos, ele também deverá conter um parâmetro de consulta adicional que é usado para impor que apenas a parte pretendida pode enviar WebHooks para o URI acima .</span><span class="sxs-lookup"><span data-stu-id="4ac9a-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="4ac9a-117">O <em> <receiver> </em> componente é o nome do destinatário, por exemplo <em>github</em> ou <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="4ac9a-118">O *{id}* é um identificador opcional que pode ser usado para identificar uma determinada configuração de destinatário do WebHook.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="4ac9a-119">Isso pode ser usado para registrar N WebHooks com um destinatário específico.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="4ac9a-120">Por exemplo, os URIs de três a seguir pode ser usado para registrar para três WebHooks independentes:</span><span class="sxs-lookup"><span data-stu-id="4ac9a-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="4ac9a-121">Instalando um receptor do WebHook</span><span class="sxs-lookup"><span data-stu-id="4ac9a-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="4ac9a-122">Para receber WebHooks usando o Microsoft ASP.NET WebHooks, você primeiro instalar o pacote do Nuget para o provedor de WebHook ou provedores que você deseja receber WebHooks de.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="4ac9a-123">Os pacotes do Nuget são nomeados [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) onde a última parte indica que o serviço de suporte.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="4ac9a-124">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="4ac9a-124">For example</span></span>

<span data-ttu-id="4ac9a-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornece suporte para recebimento WebHooks do GitHub e [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornece suporte para recebimento WebHooks gerado pelo ASP. WebHooks NET.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="4ac9a-126">Fora da caixa, você pode encontrar suporte para o Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, margem de atraso, distribuição, Trello e WordPress, mas é possível dar suporte a qualquer número de outros provedores.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="4ac9a-127">Configurando um receptor do WebHook</span><span class="sxs-lookup"><span data-stu-id="4ac9a-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="4ac9a-128">Receptores de WebHook são configurados por meio de [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) específicas implementações dessa interface e interface podem ser registradas com qualquer modelo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="4ac9a-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="4ac9a-129">A implementação padrão usa as configurações de aplicativo que pode ser definida no arquivo Web. config, ou, se o uso de aplicativos Web do Azure, pode ser definida por meio de [Portal do Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4ac9a-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Configurações do aplicativo do Azure](_static/AzureAppSettings.png)

<span data-ttu-id="4ac9a-131">O formato para chaves de configuração de aplicativo é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4ac9a-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="4ac9a-132">O valor é uma lista separada por vírgulas de valores que correspondem a *{id}* valores para os quais WebHooks tiver sido registrados, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4ac9a-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="4ac9a-133">Inicializando um receptor do WebHook</span><span class="sxs-lookup"><span data-stu-id="4ac9a-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="4ac9a-134">Receptores de WebHook são inicializados registrando-os, normalmente no *WebApiConfig* classe estática, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4ac9a-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
