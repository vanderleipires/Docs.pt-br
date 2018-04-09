---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurando o ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: de2396710fb9434c84bf14a2faa37b98154f34d8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="configuring-aspnet-web-api-2"></a>Configurando o ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como configurar a API da Web do ASP.NET.

- [Definições de configuração](#settings)
- [Configurando a API da Web com o ASP.NET de hospedagem](#webhost)
- [Configurando a API da Web com auto-hospedagem OWIN](#selfhost)
- [Serviços de API da Web global](#services)
- [Configuração por controlador](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Definições de configuração

Configurações de configuração da API da Web são definidas no [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.

| Membro | Descrição |
| --- | --- |
| **DependencyResolver** | Habilita a injeção de dependência para controladores. Consulte [usando o resolvedor de dependência de API da Web](dependency-injection.md). |
| **Filtros** | Filtros de ação. |
| **Formatadores** | [Formatadores de tipo de mídia](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Especifica se o servidor deve incluir os detalhes do erro, como mensagens de exceção e rastreamentos de pilha, nas mensagens de resposta HTTP. Consulte [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializador** | Uma função que executa a inicialização final do **HttpConfiguration**. |
| **MessageHandlers** | [Manipuladores de mensagens HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Uma coleção de regras para parâmetros de associação em ações do controlador. |
| **Propriedades** | Um recipiente de propriedades genéricas. |
| **Rotas** | A coleção de rotas. Consulte [roteamento na API da Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Serviços** | A coleção de serviços. Consulte [serviços](#services). |


## <a name="prerequisites"></a>Pré-requisitos

[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configurando a API da Web com o ASP.NET de hospedagem

Em um aplicativo ASP.NET, configure a API Web chamando [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) no **aplicativo\_iniciar** método. O **configurar** método usa um delegado com um único parâmetro do tipo **HttpConfiguration**. Faz a configuração dentro do delegado.

Aqui está um exemplo que usa um delegado anônimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

No Visual Studio de 2017, o modelo de projeto "Aplicativo Web do ASP.NET" define automaticamente o código de configuração, se você selecionar "API Web" no **novo projeto ASP.NET** caixa de diálogo.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

O modelo de projeto cria um arquivo chamado WebApiConfig.cs dentro do aplicativo\_pasta inicial. Arquivo de código define o delegado em que você deve colocar o seu código de configuração de API da Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

O modelo de projeto também adiciona o código que chama o representante de **aplicativo\_iniciar**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configurando a API da Web com auto-hospedagem OWIN

Se você estiver hospedagem interna com OWIN, crie um novo **HttpConfiguration** instância. Executar todas as configurações nessa instância e, em seguida, passe a instância para o **Owin.UseWebApi** método de extensão.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

O tutorial [OWIN de uso para Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) mostra as etapas completas.

<a id="services"></a>
## <a name="global-web-api-services"></a>Serviços de API da Web global

O **HttpConfiguration.Services** coleção contém um conjunto de serviços globais API da Web usa para executar várias tarefas, como a negociação de conteúdo e seleção de controlador.

> [!NOTE]
> O **serviços** coleção não é um mecanismo para fins gerais para injeção de dependência ou de descoberta de serviço. Ele armazena somente os tipos de serviço que são conhecidos para a estrutura da API Web.


O **serviços** coleção foi inicializada com um conjunto padrão de serviços e pode fornecer suas próprias implementações personalizadas. Alguns serviços oferecem suporte a várias instâncias, enquanto outros podem ter apenas uma instância. (No entanto, você também pode fornecer serviços no nível do controlador, consulte [por controlador configuração](#percontrollerconfig).

Serviços de instância única


| Serviço | Descrição |
| --- | --- |
| **IActionValueBinder** | Obtém uma associação para um parâmetro. |
| **IApiExplorer** | Obtém as descrições das APIs expostas pelo aplicativo. Consulte [criando uma página de ajuda para uma API da Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtém uma lista de assemblies para o aplicativo. Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valida um modelo que ler o corpo da solicitação por um formatador de tipo de mídia. |
| **IContentNegotiator** | Realiza a negociação de conteúdo. |
| **IDocumentationProvider** | Fornece documentação para APIs. O padrão é **nulo**. Consulte [criando uma página de ajuda para uma API da Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica se o host deve armazenar em buffer de corpo de entidade de mensagens HTTP. |
| **IHttpActionInvoker** | Invoca uma ação do controlador. Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Seleciona uma ação do controlador. Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Ativa um controlador. Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Seleciona um controlador. Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fornece uma lista dos tipos de controlador de API da Web no aplicativo. Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializa a estrutura de rastreamento. Consulte [rastreamento no ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fornece um gravador de rastreamento. O padrão é um gravador de rastreamento "no-op". Consulte [rastreamento no ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fornece um cache de validadores de modelo. |

Serviços de várias instâncias


|                 Serviço                 |                                                                                                              Descrição                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Retorna uma lista de filtros para uma ação do controlador.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Retorna um associador de modelo para um determinado tipo.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fornece metadados para um modelo.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fornece um validador para um modelo.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Cria um provedor de valor. Para obter mais informações, consulte Mike Stall postagem de blog [como criar um provedor de valor personalizado no WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Para adicionar uma implementação personalizada para um serviço de várias instâncias, chame **adicionar** ou **inserir** no **serviços** coleção:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Para substituir um serviço de instância única com uma implementação personalizada, chame **substituir** no **serviços** coleção:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuração por controlador

Você pode substituir as configurações a seguir em uma base por controlador:

- Formatadores de tipo de mídia
- Regras de associação de parâmetro
- Serviços

Para fazer isso, defina um atributo personalizado que implementa o **IControllerConfiguration** interface. Em seguida, aplique o atributo para o controlador.

O exemplo a seguir substitui os formatadores de tipo de mídia padrão por um formatador personalizado.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

O **IControllerConfiguration.Initialize** método aceita dois parâmetros:

- Um **HttpControllerSettings** objeto
- Um **HttpControllerDescriptor** objeto

O **HttpControllerDescriptor** contém uma descrição do controlador, você pode examinar para fins informativos (digamos, para distinguir entre os dois controladores).

Use o **HttpControllerSettings** objeto para configurar o controlador. Este objeto contém o subconjunto de parâmetros de configuração que pode ser substituído em uma base por controlador. Todas as configurações que você não altere o padrão global **HttpConfiguration** objeto.
