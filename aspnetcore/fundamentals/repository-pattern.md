---
title: Padrão de repositório com o ASP.NET Core
author: ardalis
description: Saiba como implementar o padrão de design de aplicativo do repositório em um aplicativo ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342560"
---
# <a name="repository-pattern-with-aspnet-core"></a>Padrão de repositório com o ASP.NET Core

Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)

O *padrão de repositório* é um padrão de design que isola o acesso a dados por trás de abstrações de interface. A conexão com o banco de dados e a manipulação de objetos de armazenamento de dados é realizada por meio de métodos fornecidos pela implementação da interface. Consequentemente, não é necessário chamar um código para lidar com questões de banco de dados, como conexões, comandos e leitores.

A implementação do padrão de repositório com o ASP.NET Core confere estes benefícios:

* A organização do aplicativo é menos complexa, sem interdependência direta entre as camadas de acesso de dados e de negócios.
* É mais fácil de reutilizar o código de acesso de banco de dados, pois ele é gerenciado centralmente por um ou mais repositórios.
* É possível testar a unidade do domínio de negócios de forma independente na camada de banco de dados.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O [exemplo de aplicativo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) usa o padrão de repositório para inicializar e exibir uma lista de nomes de caracteres de filme. O aplicativo usa [Entity Framework Core](/ef/core/) e uma classe `ApplicationDbContext` para persistência de dados, mas a infraestrutura do banco de dados não fica aparente onde os dados são acessados. Os objetos de banco de dados e de acesso aos dados são abstraídos por trás de um [repositório](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Interface de repositório

Uma interface de repositório define as propriedades e métodos para implementação. No aplicativo de exemplo, a interface do repositório para dados de caracteres de filme é `ICharacterRepository`. `ICharacterRepository` define os métodos `ListAll` e `Add` necessários para trabalhar com instâncias de `Character` no aplicativo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` é definido como:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Tipo concreto de repositório

A interface é implementada por um tipo concreto. No aplicativo de exemplo, `CharacterRepository` gerencia o contexto de banco de dados e implementa os métodos `ListAll` e `Add`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Registrar o serviço de repositório

Os contextos do repositório e do banco de dados são registrados com o contêiner de serviço em `Startup.ConfigureServices`. No exemplo de aplicativo, `ApplicationDbContext` é configurado com a chamada para o método de extensão [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` é registrado como um serviço com escopo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Injetar uma instância do repositório

Em uma classe na qual o acesso ao banco de dados é exigido, uma instância do repositório é solicitada por meio do construtor e atribuída a um campo particular para uso por métodos da classe. No exemplo de aplicativo, `ICharacterRepository` é usado para:

* Preencha o banco de dados se não houver caracteres.
* Obtenha uma lista dos caracteres para exibição.

Observe como o código de chamada interage apenas com a implementação da interface, `CharacterRepository`. O código chamador não usa o `ApplicationDbContext` diretamente:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Interface genérica do repositório

Este tópico e seu exemplo de aplicativo demonstram a implementação mais simples do padrão de repositório, em que um repositório é criado para cada objeto de negócios. Se o aplicativo ultrapassar alguns objetos, uma *interface genérica de repositório* pode reduzir a quantidade de código necessária para implementar o padrão de repositório. Para saber mais, confira [DevIQ: padrão de repositório: interface genérica de repositório](http://deviq.com/repository-pattern/).

## <a name="additional-resources"></a>Recursos adicionais

* [DevIQ: padrão de repositório](https://deviq.com/repository-pattern/)
* [Injeção de dependência](xref:fundamentals/dependency-injection)
* [Injeção de dependência em exibições](xref:mvc/views/dependency-injection)
* [Injeção de dependência em controladores](xref:mvc/controllers/dependency-injection)
* [Injeção de dependência em manipuladores de requisitos](xref:security/authorization/dependencyinjection)
* [Inversão de Contêineres de Controle e o padrão de Injeção de Dependência](https://www.martinfowler.com/articles/injection.html)
