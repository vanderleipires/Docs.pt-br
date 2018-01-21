---
title: "Partes do aplicativo no núcleo do ASP.NET"
author: ardalis
description: "Saiba como usar partes do aplicativo, que são abstrations sobre os recursos de um aplicativo, para configurar seu aplicativo para descobrir ou evitar o carregamento de recursos de um assembly."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 12c34b7a9521835533998c5609870bc712a6d48c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="application-parts-in-aspnet-core"></a>Partes do aplicativo no núcleo do ASP.NET

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Um *parte do aplicativo* é uma abstração sobre os recursos de um aplicativo, da qual MVC recursos, como controladores, componentes do modo de exibição, ou podem ser descobertos auxiliares de marcação. Um exemplo de uma parte do aplicativo é um AssemblyPart, que encapsula uma referência de assembly e expõe tipos e referências de compilação. *Recurso provedores* funcionam com partes do aplicativo para preencher os recursos de um aplicativo ASP.NET MVC de núcleo. É o caso de uso principal para partes do aplicativo permitir que você configure seu aplicativo para descobrir (ou evitar o carregamento) recursos MVC de um assembly.

## <a name="introducing-application-parts"></a>Apresentando as partes do aplicativo

Aplicativos MVC carregar seus recursos de [partes do aplicativo](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). Em particular, o [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) classe representa uma parte do aplicativo que é apoiada por um assembly. Você pode usar essas classes para descobrir e carregar recursos MVC, como controladores, componentes do modo de exibição, auxiliares de marcação e fontes de compilação do razor. O [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) é responsável por controlar as partes do aplicativo e os provedores de recursos disponíveis para o aplicativo MVC. Você pode interagir com o `ApplicationPartManager` em `Startup` quando você configura o MVC:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

Por padrão o MVC pesquisar a árvore de dependência e localizar controladores (mesmo em outros assemblies). Para carregar um assembly arbitrário (por exemplo, a partir de um plug-in que não é referenciado em tempo de compilação), você pode usar uma parte do aplicativo.

Você pode usar as partes do aplicativo para *evitar* procurando controladores em um determinado assembly ou local. Você pode controlar quais partes (ou assemblies) estão disponíveis para o aplicativo modificando o `ApplicationParts` coleção do `ApplicationPartManager`. A ordem das entradas na `ApplicationParts` coleção não é importante. É importante configurar totalmente o `ApplicationPartManager` antes de usá-lo para configurar serviços no contêiner. Por exemplo, você deve configurar totalmente o `ApplicationPartManager` antes de chamar `AddControllersAsServices`. Falha ao fazer isso, significa que os controladores em partes do aplicativo adicionado depois que a chamada de método não será afetada (não irá obter registrados como serviços) que pode resultar em bevavior incorreta do seu aplicativo.

Se você tiver um assembly que contém os controladores que você não deseja usar, removê-lo do `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Além de assembly do projeto e seus assemblies dependentes, o `ApplicationPartManager` incluirá partes para `Microsoft.AspNetCore.Mvc.TagHelpers` e `Microsoft.AspNetCore.Mvc.Razor` por padrão.

## <a name="application-feature-providers"></a>Provedores de recurso do aplicativo

Provedores de recurso de aplicativo examinar partes do aplicativo e fornece recursos para as partes. Há provedores de recurso interno para os seguintes recursos MVC:

* [Controladores](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Referência de metadados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Auxiliares de marcação](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Componentes do modo de exibição](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Recurso provedores herdam `IApplicationFeatureProvider<T>`, onde `T` é o tipo do recurso. Você pode implementar seu próprio recurso provedores para qualquer um dos tipos de recurso do MVC listados acima. A ordem dos provedores de recurso no `ApplicationPartManager.FeatureProviders` coleção pode ser importante, pois provedores posteriores possam reagir às ações tomadas pelo provedores anteriores.

### <a name="sample-generic-controller-feature"></a>Exemplo: Recurso do controlador genérico

Por padrão, o ASP.NET Core MVC ignora controladores genéricos (por exemplo, `SomeController<T>`). Este exemplo usa um provedor de recursos do controlador que é executado depois que o provedor padrão e adiciona instâncias de controlador genérico para uma lista especificada de tipos (definido em `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Os tipos de entidade:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

O provedor de recursos é adicionado no `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Por padrão, os nomes de controlador genérico usados para roteamento seriam do formulário *GenericController'1 [Widget]* em vez de *Widget*. O seguinte atributo é usado para modificar o nome que corresponde ao tipo genérico usado pelo controlador:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

O `GenericController` classe:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

O resultado, quando uma rota correspondente é solicitada:

![Exemplo de saída de amostra de aplicativo lê, 'Hello de um controlador de Sproket genérico'.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Exemplo: Recursos disponíveis de exibição

Você pode iterar por meio dos recursos preenchidos disponíveis para seu aplicativo solicitando uma `ApplicationPartManager` por meio de [injeção de dependência](../../fundamentals/dependency-injection.md) e usá-lo para preencher as instâncias dos recursos apropriados:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Exemplo de saída:

![Exemplo de saída de amostra de aplicativo](app-parts/_static/available-features.png)
