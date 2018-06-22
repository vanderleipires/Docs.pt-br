---
title: Testes de unidade de páginas Razor no núcleo do ASP.NET
author: guardrex
description: Saiba como criar testes de unidade para aplicativos de páginas Razor.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: bde1bef78fcc7ac1d570057d54636ea0f5490de8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274401"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Testes de unidade de páginas Razor no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

ASP.NET Core dá suporte a testes de unidade de aplicativos de páginas Razor. Testes de dados acessem DAL (camada) e garantir que os modelos de página:

* Partes de um aplicativo de páginas Razor funcionam independentemente e juntos como uma unidade durante a construção do aplicativo.
* Classes e métodos limitada a escopos de responsabilidade.
* Existe documentação adicional sobre o comportamento do aplicativo.
* Regressões, que são feitos por atualizações para o código de erros, são encontradas durante a implantação e compilação automatizada.

Este tópico pressupõe que você tenha uma compreensão básica de aplicativos de páginas Razor e testes de unidade. Se você estiver familiarizado com conceitos de teste ou com aplicativos de páginas Razor, consulte os tópicos a seguir:

* [Introdução a Páginas do Razor](xref:razor-pages/index)
* [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Teste de unidade c# no .NET Core usando xUnit e teste dotnet](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O projeto de exemplo é composto de dois aplicativos:

| Aplicativo         | Pasta do projeto                        | Descrição |
| ----------- | ------------------------------------- | ----------- |
| Aplicativo de mensagem | *src/RazorPagesTestSample*            | Permite que um usuário adicionar, excluir um, exclua todas as e analisar as mensagens. |
| Aplicativo de teste    | *tests/RazorPagesTestSample.Tests*    | Usado para o aplicativo de mensagem de teste de unidade: acesso a dados DAL (camada) e o modelo de página de índice. |

Os testes podem ser executados usando os recursos internos de teste de um IDE, como [Visual Studio](https://www.visualstudio.com/vs/). Se usar [código do Visual Studio](https://code.visualstudio.com/) ou a linha de comando, execute o seguinte comando no prompt de comando no *tests/RazorPagesTestSample.Tests* pasta:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organização de aplicativo de mensagem

O aplicativo de mensagem é um sistema de mensagem páginas Razor simples com as seguintes características:

* A página de índice do aplicativo (*Pages/Index.cshtml* e *Pages/Index.cshtml.cs*) fornece uma interface do usuário e a página de métodos de modelo para controlar a adição, exclusão e análise de mensagens (palavras médias por mensagem) .
* Uma mensagem é descrita pelo `Message` classe (*Data/Message.cs*) com duas propriedades: `Id` (chave) e `Text` (mensagem). O `Text` propriedade é necessária e limitada a 200 caracteres.
* As mensagens são armazenadas usando [banco de dados do Entity Framework na memória](/ef/core/providers/in-memory/)&#8224;.
* O aplicativo contém uma camada de acesso de dados (DAL) em sua classe de contexto de banco de dados, `AppDbContext` (*Data/AppDbContext.cs*). Os métodos DAL são marcados como `virtual`, que permite a simulação de métodos para uso em testes.
* Se o banco de dados está vazio na inicialização do aplicativo, o repositório de mensagens foi inicializado com três mensagens. Essas *propagado mensagens* também são usados em testes.

&#8224;O tópico EF [teste com InMemory](/ef/core/miscellaneous/testing/in-memory), explica como usar um banco de dados na memória para testes com MSTest. Este tópico usa o [xUnit](https://xunit.github.io/) framework de teste. Conceitos de teste e implementações de teste em estruturas de teste diferentes são semelhantes, mas não idêntica.

Embora o aplicativo não usa o [padrão repositório](http://martinfowler.com/eaaCatalog/repository.html) e não é um exemplo efetivação do [padrão de unidade de trabalho (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), páginas Razor dá suporte a esses padrões de desenvolvimento. Para obter mais informações, consulte [criar a camada de persistência de infraestrutura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementando o repositório e padrões de unidade de trabalho em um aplicativo ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), e [controlador de teste lógica de](/aspnet/core/mvc/controllers/testing) (o exemplo implementa o padrão de repositório).

## <a name="test-app-organization"></a>Organização do aplicativo de teste

O aplicativo de teste é um aplicativo de console dentro de *tests/RazorPagesTestSample.Tests* pasta.

| Pasta do aplicativo de teste | Descrição |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contém testes de unidade para a DAL.</li><li>*IndexPageTests.cs* contém testes de unidade para o modelo de página de índice.</li></ul> |
| *Utilitários*     | Contém o `TestingDbContextOptions` método usado para criar o novo banco de dados opções de contexto para cada teste de unidade DAL para que o banco de dados é redefinido para sua condição de linha de base para cada teste. |

A estrutura de teste é [xUnit](https://xunit.github.io/). O objeto do framework de simulação é [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Testes de unidade de dados acessem a DAL (camada)

O aplicativo de mensagem tem uma DAL com quatro métodos contidos no `AppDbContext` classe (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Cada método tem um ou dois testes de unidade em que o aplicativo de teste.

| Método DAL               | Função                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Obtém um `List<Message>` do banco de dados classificado pelo `Text` propriedade. |
| `AddMessageAsync`        | Adiciona um `Message` no banco de dados.                                          |
| `DeleteAllMessagesAsync` | Exclui todos os `Message` entradas do banco de dados.                           |
| `DeleteMessageAsync`     | Exclui um único `Message` do banco de dados por `Id`.                      |

Testes de unidade da DAL requerem [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) ao criar um novo `AppDbContext` para cada teste. Uma abordagem para criar o `DbContextOptions` para cada teste usar um [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

O problema com essa abordagem é que cada teste recebe o banco de dados no estado em que o teste anterior a deixou. Isso pode ser um problema ao tentar gravar testes de unidade atômica que não interfiram uns aos outros. Para forçar o `AppDbContext` para usar um novo contexto de banco de dados para cada teste, forneça um `DbContextOptions` instância com base em um novo provedor de serviço. O aplicativo de teste mostra como fazer isso usando seu `Utilities` método de classe `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Usando o `DbContextOptions` na unidade de DAL testes permite que cada teste executado atomicamente com uma instância de banco de dados atualizado:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Cada método de teste no `DataAccessLayerTest` classe (*UnitTests/DataAccessLayerTest.cs*) segue um padrão semelhante organizar Act declaração:

1. Organizar: O banco de dados está configurado para o teste de e/ou o resultado esperado é definido.
1. Ação: O teste é executado.
1. Assert: Asserções são feitas para determinar se o resultado de teste for bem-sucedida.

Por exemplo, o `DeleteMessageAsync` método é responsável por remover uma única mensagem identificada por seu `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Há dois testes para este método. Um teste verifica se o método exclui uma mensagem quando a mensagem está presente no banco de dados. Os outros testes de método que o banco de dados não serão alteradas se a mensagem `Id` para exclusão não existe. O `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` método é mostrado abaixo:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Primeiro, o método executa a etapa de organizar, onde a preparação para a etapa de ação ocorre. As mensagens de propagação são obtidas e mantidas em `seedMessages`. As mensagens de propagação são salvos no banco de dados. A mensagem com um `Id` de `1` está definido para exclusão. Quando o `DeleteMessageAsync` o método é executado, as mensagens esperadas devem ter todas as mensagens, exceto aquele com um `Id` de `1`. O `expectedMessages` variável representa esse resultado esperado.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

O método atua: O `DeleteMessageAsync` método é executado, passando o `recId` de `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Por fim, o método obtém o `Messages` do contexto e o compara a `expectedMessages` declarando que os dois são iguais:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Para comparar os dois `List<Message>` são os mesmos:

* As mensagens são ordenadas por `Id`.
* Pares de mensagens são comparados no `Text` propriedade.

Um método de teste semelhante, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` verifica o resultado da tentativa de excluir uma mensagem que não existe. Nesse caso, as mensagens esperadas no banco de dados devem ser iguais para as mensagens reais após o `DeleteMessageAsync` o método é executado. Não deve haver nenhuma alteração ao conteúdo do banco de dados:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Testes de unidade dos métodos do modelo de página

Outro conjunto de testes de unidade é responsável por testes dos métodos do modelo de página. No aplicativo de mensagem, os modelos de página de índice são encontrados no `IndexModel` classe em *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Método de modelo de página | Função |
| ----------------- | -------- |
| `OnGetAsync` | Obtém as mensagens de DAL para a interface do usuário usando o `GetMessagesAsync` método. |
| `OnPostAddMessageAsync` | Se o `ModelState` é válido, chamadas `AddMessageAsync` para adicionar uma mensagem para o banco de dados. |
| `OnPostDeleteAllMessagesAsync` | Chamadas `DeleteAllMessagesAsync` para excluir todas as mensagens no banco de dados. |
| `OnPostDeleteMessageAsync` | Executa `DeleteMessageAsync` para excluir uma mensagem com o `Id` especificado. |
| `OnPostAnalyzeMessagesAsync` | Se uma ou mais mensagens no banco de dados, calcula o número médio de palavras por mensagem. |

Os métodos do modelo de página são testados usando sete testes o `IndexPageTests` classe (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). Os testes de usam o padrão de Act Assert organizar familiar. Esses testes se concentram em:

* Determinando se os métodos seguem o comportamento correto quando o `ModelState` é inválido.
* Confirmando os métodos geram corretas `IActionResult`.
* Verificando se as atribuições de valor de propriedade são feitas corretamente.

Geralmente, esse grupo de testes simular os métodos da DAL para produzir os dados esperados para a etapa de Act onde um método de modelo de página é executado. Por exemplo, o `GetMessagesAsync` método o `AppDbContext` é simulada para produzir saída. Quando esse método é executado um método de modelo de página, a simulação retorna o resultado. Os dados não vem do banco de dados. Isso cria condições de teste previsível e confiável para usando a DAL nos testes de modelo de página.

O `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` teste mostra como o `GetMessagesAsync` método é simulado para o modelo de página:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Quando o `OnGetAsync` o método é executado na etapa Act, ele chama o modelo de página `GetMessagesAsync` método.

Etapa de ação de teste de unidade (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` modelo de página `OnGetAsync` método (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

O `GetMessagesAsync` método DAL não retorna o resultado para a chamada de método. A versão fictícias do método retorna o resultado.

No `Assert` etapa, as mensagens reais (`actualMessages`) são atribuídos a partir do `Messages` propriedade do modelo de página. Também é realizada uma verificação de tipo quando as mensagens forem atribuídas. As mensagens esperadas e reais são comparadas por seus `Text` propriedades. O teste afirma que os dois `List<Message>` instâncias contêm as mesmas mensagens.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Página criar outros testes neste grupo de objetos de modelo que incluem o `DefaultHttpContext`, o `ModelStateDictionary`, uma `ActionContext` para estabelecer o `PageContext`, um `ViewDataDictionary`e um `PageContext`. Eles são úteis para conduzir testes. Por exemplo, o aplicativo de mensagem estabelece um `ModelState` erro com `AddModelError` para verificar se uma opção válida `PageResult` é retornado quando `OnPostAddMessageAsync` é executado:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Recursos adicionais

* [Teste de unidade c# no .NET Core usando xUnit e teste dotnet](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Controladores de teste](xref:mvc/controllers/testing)
* [O código de teste de unidade](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Testes de integração](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Guia de Introdução ao xUnit.net (.NET Core/ASP.NET núcleos)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guia de início rápido Moq](https://github.com/Moq/moq4/wiki/Quickstart)
