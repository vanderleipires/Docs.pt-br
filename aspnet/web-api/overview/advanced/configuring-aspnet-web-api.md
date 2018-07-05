---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurando a API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8fd08098b5a425f2cbd7939f5f90550b98c34071
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387069"
---
<a name="configuring-aspnet-web-api-2"></a>Configurando a API Web ASP.NET 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como configurar a API Web do ASP.NET.

- [Definições de configuração](#settings)
- [Configurando a API Web com a hospedagem do ASP.NET](#webhost)
- [Configurando a API Web com auto-hospedagem OWIN](#selfhost)
- [Serviços de API da Web global](#services)
- [Configuração por controlador](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Definições de configuração

Configurações de configuração de API da Web são definidas na [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.

| Membro | Descrição |
| --- | --- |
| **DependencyResolver** | Permite a injeção de dependência para controladores. Ver [usando o resolvedor de dependência de API da Web](dependency-injection.md). |
| **Filtros** | Filtros de ação. |
| **Formatadores** | [Formatadores de tipo de mídia](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Especifica se o servidor deve incluir detalhes do erro, como mensagens de exceção e rastreamentos de pilha, nas mensagens de resposta HTTP. Ver [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializador** | Uma função que executa a inicialização final do **HttpConfiguration**. |
| **MessageHandlers** | [Manipuladores de mensagens HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Uma coleção de regras para parâmetros de associação em ações do controlador. |
| **Propriedades** | Um recipiente de propriedades genérico. |
| **Rotas** | A coleção de rotas. Ver [roteamento na API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Serviços** | A coleção de serviços. Ver [Services](#services). |


## <a name="prerequisites"></a>Pré-requisitos

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configurando a API Web com a hospedagem do ASP.NET

Em um aplicativo ASP.NET, configure a API da Web chamando [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) na **Application\_iniciar** método. O **configurar** leva um delegado com um único parâmetro do tipo **HttpConfiguration**. Execute todos de sua configuração dentro do delegado.

Aqui está um exemplo que usa um delegado anônimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

No Visual Studio 2017, o modelo de projeto "Aplicativo Web do ASP.NET" configurará automaticamente o código de configuração, se você selecionar "API Web" no **novo projeto ASP.NET** caixa de diálogo.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

O modelo de projeto cria um arquivo chamado WebApiConfig.cs dentro do aplicativo\_pasta inicial. Esse arquivo de código define o delegado em que você deve colocar seu código de configuração de API da Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

O modelo de projeto também adiciona o código que chama o delegado do **Application\_iniciar**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configurando a API Web com auto-hospedagem OWIN

Se você estiver hospedagem interna com o OWIN, crie um novo **HttpConfiguration** instância. Executar qualquer configuração nessa instância e, em seguida, passar a instância para o **Owin.UseWebApi** método de extensão.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

O tutorial [OWIN de uso para Self ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) mostra as etapas completas.

<a id="services"></a>
## <a name="global-web-api-services"></a>Serviços de API da Web global

O **HttpConfiguration.Services** coleção contém um conjunto de serviços globais que API Web usa para executar várias tarefas, como a negociação de conteúdo e a seleção de controlador.

> [!NOTE]
> O **Services** coleção não é um mecanismo de finalidade geral para injeção de dependência ou de descoberta de serviço. Ele armazena somente os tipos de serviço que são conhecidos como a estrutura da API da Web.


O **Services** coleção foi inicializada com um conjunto padrão de serviços, e você pode fornecer suas próprias implementações personalizadas. Alguns serviços dão suporte a várias instâncias, enquanto outros podem ter apenas uma instância. (No entanto, você também pode fornecer serviços no nível do controlador, consulte [por controlador configuração](#percontrollerconfig).

Serviços de instância única


| Serviço | Descrição |
| --- | --- |
| **IActionValueBinder** | Obtém uma associação para um parâmetro. |
| **IApiExplorer** | Obtém as descrições das APIs expostas pelo aplicativo. Ver [criando uma página de ajuda para uma API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtém uma lista de assemblies para o aplicativo. Ver [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valida um modelo que é lido do corpo da solicitação por um formatador de tipo de mídia. |
| **IContentNegotiator** | Realiza a negociação de conteúdo. |
| **IDocumentationProvider** | Fornece documentação para APIs. O padrão é **nulo**. Ver [criando uma página de ajuda para uma API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica se o host deve armazenar em buffer de corpo de entidade de mensagens HTTP. |
| **IHttpActionInvoker** | Invoca uma ação do controlador. Ver [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Seleciona uma ação do controlador. Ver [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Ativa um controlador. Ver [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Seleciona um controlador. Ver [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fornece uma lista dos tipos de controlador de API da Web no aplicativo. Ver [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializa a estrutura de rastreamento. Ver [rastreamento na API Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fornece um gravador de rastreamento. O padrão é um gravador de rastreamento "no-op". Ver [rastreamento na API Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fornece um cache de validadores de modelo. |

Serviços de várias instâncias


|                 Serviço                 |                                                                                                              Descrição                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Retorna uma lista de filtros para uma ação do controlador.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Retorna um associador de modelo para um determinado tipo.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fornece metadados para um modelo.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fornece um validador para um modelo.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Cria um provedor de valor. Para obter mais informações, consulte o blog de Mike Stall postagem [como criar um provedor de valor personalizado no API da Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Para adicionar uma implementação personalizada para um serviço de várias instâncias, chame **Add** ou **inserir** sobre o **serviços** coleção:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Para substituir um serviço de instância única com uma implementação personalizada, chame **substituir** sobre o **Services** coleção:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuração por controlador

Você pode substituir as configurações a seguir em uma base por controlador:

- Formatadores de tipo de mídia
- Regras de associação de parâmetro
- Serviços

Para fazer isso, defina um atributo personalizado que implementa o **IControllerConfiguration** interface. Em seguida, aplique o atributo para o controlador.

O exemplo a seguir substitui os formatadores de tipo de mídia padrão com um formatador personalizado.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

O **IControllerConfiguration.Initialize** método utiliza dois parâmetros:

- Uma **HttpControllerSettings** objeto
- Uma **HttpControllerDescriptor** objeto

O **HttpControllerDescriptor** contém uma descrição do controlador, você pode examinar para fins informativos (digamos, para distinguir entre os dois controladores).

Use o **HttpControllerSettings** objeto para configurar o controlador. Este objeto contém o subconjunto de parâmetros de configuração que pode ser substituído em uma base por controlador. Todas as configurações que você não altere o padrão global **HttpConfiguration** objeto.
