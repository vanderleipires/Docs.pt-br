---
title: Trabalhando com o modelo de aplicativo
author: ardalis
description: 
keywords: "Core,ASP.NET do ASP.NET MVC de núcleos, o modelo de aplicativo"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-the-application-model"></a>Trabalhando com o modelo de aplicativo

Por [Steve Smith](https://ardalis.com/)

ASP.NET MVC de núcleo define uma *modelo de aplicativo* que representa os componentes de um aplicativo MVC. Você pode ler e manipular esse modelo para modificar o comportam de elementos MVC. Por padrão, o MVC segue certas convenções para determinar quais classes são considerados controladores, quais métodos essas classes são ações e o comportamento de parâmetros e roteamento. Você pode personalizar esse comportamento para atender às necessidades do seu aplicativo criando suas próprias convenções e aplicá-las globalmente ou como atributos.

## <a name="models-and-providers"></a>Modelos e os provedores

O modelo de aplicativo MVC do ASP.NET Core incluem abstratas interfaces e classes de implementação concreta que descrevem um aplicativo MVC. Esse modelo é o resultado do MVC descobrir controladores, ações, parâmetros de ação, rotas e filtros de acordo com as convenções padrão do aplicativo. Trabalhando com o modelo de aplicativo, você pode modificar seu aplicativo para seguir as convenções diferentes do comportamento padrão MVC. Os parâmetros, nomes, rotas e filtros são todos usados como dados de configuração para controladores e ações.

O modelo de aplicativo do ASP.NET Core MVC tem a seguinte estrutura:

* ApplicationModel
    * Controladores (ControllerModel)
        * Ações (ActionModel)
            * Parâmetros (ParameterModel)

Cada nível do modelo tem acesso a um comum `Properties` coleta e níveis inferiores podem acessar e substituir os valores de propriedade definidos por níveis mais altos na hierarquia. As propriedades são mantidas para o `ActionDescriptor.Properties` quando as ações são criadas. Em seguida, quando uma solicitação está sendo tratada, todas as propriedades uma convenção adicionados ou modificados pode ser acessada por meio de `ActionContext.ActionDescriptor.Properties`. Usando propriedades é uma ótima maneira de configurar seus filtros, associadores de modelo, etc. em uma base por ação.

> [!NOTE]
> O `ActionDescriptor.Properties` coleção não é thread-safe (para gravações) quando tiver terminado de inicialização do aplicativo. Convenções são a melhor maneira de adicionar com segurança os dados a esta coleção.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

Núcleo do ASP.NET MVC carrega o modelo de aplicativo usando um provedor padrão, definido pelo [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface. Esta seção aborda alguns dos detalhes de implementação interna de como essas funções de provedor. Este é um tópico avançado - a maioria dos aplicativos que aproveitam o modelo de aplicativo deve fazer isso ao trabalhar com as convenções.

Implementações do `IApplicationModelProvider` interface "encapsular", com cada chamada de implementação `OnProvidersExecuting` em ordem crescente com base em seu `Order` propriedade. O `OnProvidersExecuted` método é chamado em ordem inversa. A estrutura define vários provedores:

Primeiro (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Em seguida, (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> A ordem na qual dois provedores com o mesmo valor para `Order` são chamados não está definida e, portanto, não deve ser considerado.

> [!NOTE]
> `IApplicationModelProvider`é um conceito avançado para os autores do framework estender. Em geral, aplicativos devem usar as convenções e estruturas devem usar provedores. A principal diferença é que provedores sempre sejam executadas antes de convenções.

O `DefaultApplicationModelProvider` estabelece muitos dos comportamentos padrão usados pelo ASP.NET MVC de núcleo. Suas responsabilidades incluem:

* Adicionando filtros globais para o contexto
* Adicionando controladores para o contexto
* Adição de métodos públicos controlador como ações
* Adicionando parâmetros de método de ação para o contexto
* Aplicação de rota e outros atributos

Alguns comportamentos internos são implementados pelo `DefaultApplicationModelProvider`. Esse provedor é responsável pela construção de [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), que por sua vez faz referência a [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), e [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instâncias. O `DefaultApplicationModelProvider` classe é um detalhe de implementação de estrutura interna que pode e será alterado no futuro. 

O `AuthorizationApplicationModelProvider` é responsável por aplicar o comportamento associado com o `AuthorizeFilter` e `AllowAnonymousFilter` atributos. [Saiba mais sobre esses atributos](xref:security/authorization/simple).

O `CorsApplicationModelProvider` implementa o comportamento associado com o `IEnableCorsAttribute` e `IDisableCorsAttribute`e o `DisableCorsAuthorizationFilter`. [Saiba mais sobre CORS](xref:security/cors).

## <a name="conventions"></a>Convenções

O modelo de aplicativo define abstrações de convenção que fornecem uma maneira simples de personalizar o comportamento dos modelos de substituir o modelo inteiro ou provedor. Essas abstrações são a maneira recomendada para modificar o comportamento do seu aplicativo. Convenções fornecem uma maneira para escrever código que se aplicarão dinamicamente as personalizações. Enquanto [filtros](xref:mvc/controllers/filters) fornecem um meio de modificar o comportamento da estrutura, personalizações permitem que você controle como o aplicativo inteiro está conectado em conjunto.

As convenções a seguir estão disponíveis:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Convenções são aplicadas ao adicioná-los às opções do MVC ou implementando `Attribute`s e aplicá-las para controladores, ações ou parâmetros de ação (semelhante a [ `Filters` ](xref:mvc/controllers/filters)). Ao contrário de filtros, convenções são executadas apenas quando o aplicativo é iniciado, não como parte de cada solicitação.

### <a name="sample-modifying-the-applicationmodel"></a>Exemplo: Modificando o ApplicationModel

A seguinte convenção é usada para adicionar uma propriedade para o modelo de aplicativo. 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Convenções de modelo de aplicativo são aplicadas como opções quando MVC é adicionado no `ConfigureServices` em `Startup`.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Propriedades são acessíveis a partir de `ActionDescriptor` coleção properties em ações do controlador:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Exemplo: Modificar a descrição do ControllerModel

Como no exemplo anterior, o modelo do controlador também pode ser modificado para incluir propriedades personalizadas. Eles substituirão as propriedades existentes com o mesmo nome especificado no modelo de aplicativo. O atributo de convenção a seguir adiciona uma descrição no nível do controlador:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Essa convenção é aplicada como um atributo em um controlador.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

A propriedade "Descrição" é acessada da mesma maneira como nos exemplos anteriores.

### <a name="sample-modifying-the-actionmodel-description"></a>Exemplo: Modificar a descrição do ActionModel

Uma convenção de atributo separado pode ser aplicada a ações individuais, substituindo o comportamento já aplicado no nível do aplicativo ou controlador.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Aplicar isso a uma ação no controlador do exemplo anterior demonstra como ela substitui a convenção de nível de controlador:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Exemplo: Modificando o ParameterModel

A seguinte convenção pode ser aplicada a parâmetros de ação para modificar seus `BindingInfo`. A seguinte convenção requer que o parâmetro ser um parâmetro de rota. outros possíveis origens de vinculação (como valores de cadeia de caracteres de consulta) são ignoradas.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

O atributo pode ser aplicado a qualquer parâmetro de ação:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Exemplo: Modificando o nome de ActionModel

A convenção a seguir modifica o `ActionModel` para atualizar o *nome* da ação para o qual ela é aplicada. O novo nome é fornecido como um parâmetro para o atributo. Esse novo nome é usado pelo roteamento, para que isso afetará a rota usada para acessar este método de ação.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Esse atributo é aplicado a um método de ação de `HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Mesmo que o nome do método é `SomeName`, o atributo substitui a convenção MVC de usar o nome do método e substitui o nome da ação com `MyCoolAction`. Assim, a rota usada para alcançar essa ação é `/Home/MyCoolAction`.

> [!NOTE]
> Este exemplo é essencialmente o mesmo que usar o interno [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) atributo.

### <a name="sample-custom-routing-convention"></a>Exemplo: A convenção de roteamento personalizado

Você pode usar um `IApplicationModelConvention` para personalizar como roteamento funciona. Por exemplo, a seguinte convenção incorporará namespaces dos controladores em suas rotas, substituindo `.` no namespace com `/` na rota:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

A convenção é adicionada como uma opção na inicialização.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Você pode adicionar as convenções para sua [middleware](xref:fundamentals/middleware) acessando `MvcOptions` usando`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

Este exemplo se aplica a esta convenção a rotas que não estiver usando o roteamento de atributo em que o controlador tem "Namespace" em seu nome. O controlador a seguir demonstra essa convenção:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Uso do modelo de aplicativo em WebApiCompatShim

Núcleo do ASP.NET MVC usa um conjunto diferente de convenções do ASP.NET Web API 2. Usando convenções personalizadas, você pode modificar o comportamento de um aplicativo ASP.NET MVC de núcleo para ser consistente com que um aplicativo de API da Web. É fornecido Microsoft a [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) especificamente para essa finalidade.

> [!NOTE]
> Saiba mais sobre [Migrando do ASP.NET Web API](xref:migration/webapi).

Para usar a correção de compatibilidade de API da Web, você precisa adicionar o pacote ao seu projeto e, em seguida, adicione as convenções para MVC chamando `AddWebApiConventions` em `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

As convenções de fornecido pelo shim são aplicadas apenas a partes do aplicativo que tiveram determinados atributos aplicados a eles. Os seguintes quatro atributos são usados para controlar quais controladores devem ter seus convenções modificadas por convenções do shim:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Convenções de ação

O `UseWebApiActionConventionsAttribute` é usado para mapear o método HTTP para ações com base em seu nome (por exemplo, `Get` seriam mapeados para `HttpGet`). Só se aplica a ações que não usa o roteamento de atributo.

### <a name="overloading"></a>Sobrecarga

O `UseWebApiOverloadingAttribute` é usado para aplicar o `WebApiOverloadingApplicationModelConvention` convenção. Essa convenção adiciona um `OverloadActionConstraint` para o processo de seleção de ação, que limita as ações de candidato para aqueles para os quais a solicitação atenda a todos os parâmetros não opcionais.

### <a name="parameter-conventions"></a>Convenções de parâmetro

O `UseWebApiParameterConventionsAttribute` é usado para aplicar o `WebApiParameterConventionsApplicationModelConvention` convenção de ação. Essa convenção Especifica que tipos simples usados como parâmetros de ação associados do URI por padrão, enquanto tipos complexos são associados do corpo da solicitação.

### <a name="routes"></a>Rotas

O `UseWebApiRoutesAttribute` controla se o `WebApiApplicationModelConvention` convenção de controlador é aplicada. Quando habilitada, essa convenção é usada para adicionar suporte para [áreas](xref:mvc/controllers/areas) para a rota.

Além de um conjunto de convenções, o pacote de compatibilidade inclui um `System.Web.Http.ApiController` classe base que substitui aquela fornecida pela API da Web. Isso permite que os controladores gravados para a API da Web e herança de seu `ApiController` a funcionar como foram criados, durante a execução no ASP.NET MVC de núcleo. Essa classe de base do controlador está decorado com todos os `UseWebApi*` atributos listados acima. O `ApiController` expõe as propriedades, métodos e tipos de resultados que são compatíveis com as que constam na API da Web.

## <a name="using-apiexplorer-to-document-your-app"></a>Usando ApiExplorer para documentar seu aplicativo

O modelo de aplicativo expõe uma [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) propriedade em cada nível que pode ser usado para percorrer a estrutura do aplicativo. Isso pode ser usado para [gerar páginas de ajuda para suas APIs da Web usando ferramentas como o Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). O `ApiExplorer` propriedade expõe um `IsVisible` propriedade que pode ser definida para especificar quais partes do modelo do aplicativo devem ser expostos. Você pode configurar essa configuração usando uma convenção de:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Usando essa abordagem (e convenções adicionais, se necessário), você pode habilitar ou desabilitar a visibilidade de API em qualquer nível dentro de seu aplicativo. 
