---
title: "Introdução ao ASP.NET Core e o Entity Framework 6"
author: tdykstra
description: Este artigo mostra como usar o Entity Framework 6 em um aplicativo do ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Introdução ao ASP.NET Core e o Entity Framework 6

Por [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), e [Tom Dykstra](https://github.com/tdykstra)

Este artigo mostra como usar o Entity Framework 6 em um aplicativo do ASP.NET Core.

## <a name="overview"></a>Visão geral

Para usar o Entity Framework 6, seu projeto deve compilar no .NET Framework, como Entity Framework 6 não oferece suporte a .NET Core. Se você precisar de recursos de plataforma cruzada, será necessário atualizar para [Entity Framework Core](https://docs.microsoft.com/ef/).

A maneira recomendada para usar o Entity Framework 6 em um aplicativo do ASP.NET Core é colocar o contexto de EF6 e classes de modelo em uma biblioteca de classes do projeto que tem como destino framework completo. Adicione uma referência para a biblioteca de classes do projeto ASP.NET Core. Consulte o exemplo [solução do Visual Studio com projetos EF6 e ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Não é possível colocar um contexto EF6 em um projeto do ASP.NET Core porque projetos .NET Core não dão suporte a todas as funcionalidades que EF6 comandos como *Enable-Migrations* exigem.

Independentemente do tipo de projeto em que você localize seu contexto EF6, somente ferramentas de linha de comando EF6 trabalhar com um contexto de EF6. Por exemplo, `Scaffold-DbContext` só está disponível no Entity Framework Core. Se você precisar a engenharia reversa de um banco de dados em um modelo de EF6, consulte [Code First para um banco de dados](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Estrutura completa de referência e EF6 no projeto do ASP.NET Core

Seu projeto do ASP.NET Core precisa referenciar EF6 e do .NET framework. Por exemplo, o *. csproj* arquivo do projeto ASP.NET Core será semelhante ao exemplo a seguir (somente as partes relevantes do arquivo são mostradas).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Se você estiver criando um novo projeto, use o **aplicativo Web do ASP.NET Core (.NET Framework)** modelo.

## <a name="handle-connection-strings"></a>Cadeias de caracteres do identificador de conexão

As ferramentas de linha de comando EF6 que você usará no projeto de biblioteca de classe EF6 requerem um construtor padrão para que possam instanciar o contexto. Mas, você provavelmente desejará especificar a cadeia de caracteres de conexão para usar no projeto ASP.NET Core, caso em que o construtor de contexto deve ter um parâmetro que permite que você passe a cadeia de caracteres de conexão. Aqui está um exemplo.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Como seu contexto EF6 não tem um construtor sem parâmetros, seu projeto EF6 precisa fornecer uma implementação de [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). As ferramentas de linha de comando EF6 encontrará e usar essa implementação, portanto, pode criar uma instância do contexto. Aqui está um exemplo.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

Nesse código de exemplo, o `IDbContextFactory` implementação passa em uma cadeia de caracteres de conexão embutidas. Esta é a cadeia de conexão que irá usar as ferramentas de linha de comando. Você vai querer implementar uma estratégia para garantir que a biblioteca de classe usa a mesma cadeia de conexão que usa o aplicativo de chamada. Por exemplo, você pode obter o valor de uma variável de ambiente em ambos os projetos.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Configurar a injeção de dependência no projeto do ASP.NET Core

O projeto de núcleo *Startup.cs* arquivo, configurar o contexto de EF6 para injeção de dependência (DI) `ConfigureServices`. Os objetos de contexto EF devem ser delimitados para um tempo de vida de cada solicitação.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Em seguida, você pode obter uma instância do contexto em seus controladores de usando a injeção de dependência. O código é semelhante ao que você escreve para um contexto de EF principais:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Aplicativo de exemplo

Para um aplicativo de exemplo de funcionamento, consulte o [solução do Visual Studio de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) que acompanha este artigo.

Este exemplo pode ser criado do zero pelas seguintes etapas no Visual Studio:

* Crie uma solução.

* **Adicionar novo projeto > Web > aplicativo Web do ASP.NET Core (.NET Framework)**

* **Adicionar novo projeto > área de trabalho clássica do Windows > (.NET Framework) da biblioteca de classe**

* Em **Package Manager Console** (PMC) para ambos os projetos, execute o comando `Install-Package Entityframework`.

* No projeto de biblioteca de classe, crie classes de modelo de dados e uma classe de contexto e uma implementação de `IDbContextFactory`.

* Em PMC para o projeto de biblioteca de classe, execute os comandos `Enable-Migrations` e `Add-Migration Initial`. Se você tiver definido o projeto do ASP.NET Core como projeto de inicialização, adicionar `-StartupProjectName EF6` a esses comandos.

* No projeto principal, adicione uma referência ao projeto de biblioteca de classe.

* No projeto principal, em *Startup.cs*, registrar o contexto para injeção de dependência.

* No projeto principal, em *appSettings. JSON*, adicione a cadeia de caracteres de conexão.

* No projeto principal, adicione um controlador e as exibições para verificar que você pode ler e gravar dados. (Observe que a estrutura MVC do ASP.NET Core não funcionará com o contexto de EF6 referenciado da biblioteca de classe.)

## <a name="summary"></a>Resumo

Este artigo fornece orientações básicas para usar o Entity Framework 6 em um aplicativo do ASP.NET Core.

## <a name="additional-resources"></a>Recursos adicionais

* [Entity Framework - configuração baseada em código](https://msdn.microsoft.com/data/jj680699.aspx)
