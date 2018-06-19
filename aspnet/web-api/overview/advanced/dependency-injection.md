---
uid: web-api/overview/advanced/dependency-injection
title: Injeção de dependência no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como injetar dependências em seu controlador API Web do ASP.NET. Versões de software usadas no tutorial da Web API 2 Unity Application Block...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036509"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Injeção de dependência no ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Este tutorial mostra como injetar dependências em seu controlador API Web do ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Web API 2
> - [Bloco de aplicativos do Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (versão 5 também funciona)


## <a name="what-is-dependency-injection"></a>O que é a injeção de dependência?

Um *dependência* é qualquer objeto que requer a outro objeto. Por exemplo, é comum para definir um [repositório](http://martinfowler.com/eaaCatalog/repository.html) que controla o acesso a dados. Vamos ilustrar um exemplo. Primeiro, vamos definir um modelo de domínio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Aqui está uma classe simples do repositório que armazena itens em um banco de dados usando o Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Agora vamos definir um controlador de API da Web que oferece suporte a solicitações GET para `Product` entidades. (Estou deixando POST e outros métodos para simplificar.) Aqui está a primeira tentativa:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Observe que a classe do controlador depende `ProductRepository`, e estão permitindo que o controlador de criar o `ProductRepository` instância. No entanto, é uma boa ideia para codificar a dependência dessa forma, por vários motivos.

- Se você quiser substituir `ProductRepository` com uma implementação diferente, você também precisará modificar a classe do controlador.
- Se o `ProductRepository` tem dependências, você deve configurá-los dentro do controlador. Para um projeto grande com vários controladores, o código de configuração se torna espalhado por seu projeto.
- É difícil para teste de unidade, porque o controlador é codificado para consultar o banco de dados. Para um teste de unidade, você deve usar um repositório de simulação ou stub, que não é possível com o design de currect.

Podemos resolver esses problemas por *injetando* repositório no controlador. Primeiro, refatorar o `ProductRepository` classe em uma interface:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Em seguida, forneça o `IProductRepository` como um parâmetro de construtor:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Este exemplo usa [injeção de construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Você também pode usar *injeção de setter*, onde você pode definir a dependência por meio de uma propriedade ou método de setter.

Mas agora há um problema, pois o seu aplicativo não cria o controlador diretamente. API da Web cria o controlador quando ele encaminha a solicitação e a API da Web não sabe nada sobre `IProductRepository`. Isso é aqui que entra o resolvedor de dependência de API da Web.

## <a name="the-web-api-dependency-resolver"></a>O resolvedor de dependência de API da Web

API da Web define o **IDependencyResolver** interface para resolver as dependências. Aqui está a definição da interface:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

O **IDependencyScope** interface tem dois métodos:

- **GetService** cria uma instância de um tipo.
- **GetServices** cria uma coleção de objetos de um tipo especificado.

O **IDependencyResolver** método herda **IDependencyScope** e adiciona o **BeginScope** método. Vou falar sobre escopos posteriormente neste tutorial.

Quando a API da Web cria uma instância de controlador, ele primeiro chama **IDependencyResolver.GetService**, passando o tipo de controlador. Você pode usar esse gancho de extensibilidade para criar o controlador, resolver quaisquer dependências. Se **GetService** retorna null, a API da Web procura um construtor sem parâmetros na classe do controlador.

## <a name="dependency-resolution-with-the-unity-container"></a>Resolução de dependência com o contêiner do Unity

Embora você poderia escrever uma completa **IDependencyResolver** implementação do zero, a interface é realmente criada para agir como ponte entre a API da Web e os contêineres de IoC existentes.

Um contêiner IoC é um componente de software que é responsável por gerenciar as dependências. Você registrar tipos com o contêiner e, em seguida, usa o contêiner para criar objetos. O contêiner automaticamente descobre as relações de dependência. Vários contêineres IoC também permitem que você controle como escopo e tempo de vida do objeto.

> [!NOTE]
> "IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo. Um contêiner IoC constrói os objetos para você, o que "inverte" o fluxo normal de controle.


Para este tutorial, vamos usar [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; práticas recomendadas. (Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) Você pode usar o NuGet Package Manager para instalar o Unity. Do **ferramentas** menu do Visual Studio, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Aqui está uma implementação de **IDependencyResolver** que encapsula um contêiner do Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Se o **GetService** método não é possível resolver um tipo, ele deverá retornar **nulo**. Se o **GetServices** método não é possível resolver um tipo, ele deverá retornar um objeto de coleção vazia. Não gera exceções para tipos desconhecidos.


## <a name="configuring-the-dependency-resolver"></a>Configurando o resolvedor de dependência

Definir o resolvedor de dependência no **DependencyResolver** propriedade global **HttpConfiguration** objeto.

O código a seguir registra o `IProductRepository` interface com Unity e, em seguida, cria um `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Escopo de dependência e tempo de vida do controlador

Os controladores são criados por solicitação. Para gerenciar o tempo de vida do objeto, **IDependencyResolver** usa o conceito de um *escopo*.

O resolvedor de dependência anexado para o **HttpConfiguration** objeto tem escopo global. Quando a API da Web cria um controlador, ele chama **BeginScope**. Este método retorna um **IDependencyScope** que representa um escopo filho.

Em seguida, chama uma API da Web **GetService** no escopo filho para criar o controlador. Quando a solicitação for concluída, a API da Web chama **Dispose** no escopo filho. Use o **Dispose** método descarte as dependências do controlador.

Como você implementa **BeginScope** depende do contêiner IoC. Para Unity escopo corresponde a um contêiner filho:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Maioria dos contêineres IoC têm equivalentes semelhantes.
