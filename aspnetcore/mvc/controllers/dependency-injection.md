---
title: "Injeção de dependência nos controladores"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: f6b454da838308adddaaddb84073722f647af379
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="dependency-injection-into-controllers"></a>Injeção de dependência nos controladores

<a name=dependency-injection-controllers></a>

Por [Steve Smith](https://ardalis.com/)

Controladores MVC do ASP.NET Core devem solicitar suas dependências explicitamente por meio de seus construtores. Em algumas instâncias, ações do controlador individuais podem exigir um serviço e não pode fazer sentido para solicitar ao nível de controlador. Nesse caso, você também pode optar por injetar um serviço como um parâmetro no método de ação.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)

## <a name="dependency-injection"></a>Injeção de dependência

Injeção de dependência é uma técnica que segue o [princípio de inversão de dependência](http://deviq.com/dependency-inversion-principle/), permitindo que aplicativos de ser composto de módulos acoplados de forma flexível. ASP.NET Core tem suporte interno para [injeção de dependência](../../fundamentals/dependency-injection.md), que torna os aplicativos mais fácil de testar e manter.

## <a name="constructor-injection"></a>Injeção de construtor

Estende o suporte interno do ASP.NET Core injeção de dependência baseado no construtor para controladores MVC. Simplesmente adicionando um tipo de serviço para o seu controlador como um parâmetro de construtor, o ASP.NET Core tentará resolver daquele tipo usando seu contêiner de serviço. Os serviços são normalmente, mas não sempre, definidos usando interfaces. Por exemplo, se seu aplicativo tiver a lógica de negócios que depende do tempo atual, você pode injetar um serviço que recupera o tempo (em vez de embutir em código ele), que permite que os testes passar em implementações que usam um período de tempo.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementando uma interface como esta para que ele usa o relógio do sistema em tempo de execução é simples:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Com isso em vigor, podemos usar o serviço em nosso controlador. Nesse caso, adicionamos alguma lógica para o `HomeController` `Index` método para exibir uma saudação ao usuário com base na hora do dia.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Se executarmos o aplicativo agora, vamos provavelmente encontrará um erro:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Esse erro ocorre quando não configuramos um serviço no `ConfigureServices` método em nosso `Startup` classe. Para especificar que as solicitações de `IDateTime` devem ser resolvidos usando uma instância de `SystemDateTime`, adicione a linha realçada na lista abaixo para sua `ConfigureServices` método:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Esse serviço específico pode ser implementado usando qualquer uma das várias opções diferentes de tempo de vida (`Transient`, `Scoped`, ou `Singleton`). Consulte [injeção de dependência](../../fundamentals/dependency-injection.md) para entender como cada uma dessas opções de escopo afetará o comportamento do seu serviço.

Depois que o serviço tiver sido configurado, executando o aplicativo e navegar para a home page devem exibir a mensagem de tempo conforme o esperado:

![Saudação do servidor](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Consulte [lógica do controlador de teste](testing.md) para saber como solicitar explicitamente dependências [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) em controladores facilita o código de teste.

Injeção de dependência interna do ASP.NET Core dá suporte a apenas um único construtor para classes de solicitação de serviços. Se você tiver mais de um construtor, você pode receber uma exceção informando:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Como a mensagem de erro afirma, você pode corrigir esse problema ter apenas um único construtor. Você também pode [substituir o suporte de injeção de dependência padrão com uma implementação de terceiros](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), muitos dos quais oferecem suporte a vários construtores.

## <a name="action-injection-with-fromservices"></a>Injeção de ação com FromServices

Às vezes, você não precisa de um serviço para mais de uma ação em seu controlador. Nesse caso, talvez faça sentido para injetar o serviço como um parâmetro para o método de ação. Isso é feito marcando o parâmetro com o atributo `[FromServices]` conforme mostrado aqui:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Acessando configurações de um controlador

Acessando configurações de aplicativo ou configuração de dentro de um controlador é um padrão comum. Esse acesso deve usar o padrão de opções descrito na [configuração](../../fundamentals/configuration.md). Em geral não solicite configurações diretamente do seu controlador usando a injeção de dependência. Uma abordagem melhor é a solicitação de um `IOptions<T>` instância, onde `T` é a classe de configuração que você precisa.

Para trabalhar com o padrão de opções, você precisa criar uma classe que representa as opções, como este:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Em seguida, você precisa configurar o aplicativo para usar o modelo de opções e adicione sua classe de configuração para a coleção de serviços no `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Na lista acima, estamos configurando o aplicativo para ler as configurações de um arquivo no formato JSON. Você também pode configurar as configurações inteiramente no código, como é mostrado no código comentado acima. Consulte [configuração](../../fundamentals/configuration.md) para outras opções de configuração.

Depois que você tiver especificado um objeto de configuração fortemente tipado (nesse caso, `SampleWebSettings`) e ele foi adicionado à coleção de serviços, você pode solicitá-las de qualquer método de ação ou controlador solicitando uma instância de `IOptions<T>` (nesse caso, `IOptions<SampleWebSettings>`) . O código a seguir mostra como um solicita as configurações de um controlador:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Seguindo o padrão de opções permite que as configurações e seja dissociado uns dos outros e garante que o controlador é após [separação de preocupações](http://deviq.com/separation-of-concerns/), pois ele não precisa saber como e onde encontrar as configurações informações. Ele também facilita o controlador de teste de unidade [lógica do controlador de teste](testing.md), porque não há nenhum [adesivos](http://deviq.com/static-cling/) ou instanciação direta de classes de configuração dentro da classe do controlador.
