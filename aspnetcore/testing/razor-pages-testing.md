---
title: "Unidade de páginas Razor e integração de teste no núcleo do ASP.NET"
author: guardrex
description: "Saiba como criar testes de unidade e a integração de aplicativos de páginas Razor."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/razor-pages-testing
ms.openlocfilehash: 7a3f1bfa8bec830216af37d89aa588a921485e6b
ms.sourcegitcommit: 4925a91ef4130ddb333f187ab13defe66f2c6cef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2017
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a>Unidade de páginas Razor e integração de teste no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

ASP.NET Core dá suporte à unidade e testes de integração de aplicativos de páginas Razor. Teste a camada de acesso a dados (DAL), modelos e componentes de página integrada ajuda a garantir:

* Partes de um aplicativo de páginas Razor funcionam independentemente e juntos como uma unidade durante a construção do aplicativo.
* Classes e métodos limitada a escopos de responsabilidade.
* Existe documentação adicional sobre o comportamento do aplicativo.
* Regressões, que são feitos por atualizações para o código de erros, são encontradas durante a implantação e compilação automatizada.

Este tópico pressupõe que você tenha uma compreensão básica do Razor páginas de aplicativos, teste de unidade e a integração de teste. Se você não estiver familiarizado com conceitos de teste ou de páginas Razor aplicativos, consulte os tópicos a seguir:

* [Introdução a Páginas do Razor](xref:mvc/razor-pages/index)
* [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Teste de unidade c# no .NET Core usando xUnit e teste dotnet](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Teste de integração](xref:testing/integration-testing)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O projeto de exemplo é composto de dois aplicativos:

| Aplicativo         | Pasta do projeto                        | Descrição |
| ----------- | ------------------------------------- | ----------- |
| Aplicativo de mensagem | *src/RazorPagesTestingSample*         | Permite que um usuário adicionar, excluir um, exclua todas as e analisar as mensagens. |
| Aplicativo de teste    | *tests/RazorPagesTestingSample.Tests* | Usado para testar o aplicativo de mensagem.<ul><li>Testes de unidade: camada de acesso a dados (DAL), modelo de página de índice</li><li>Testes de integração: página de índice</li></ul> |

Os testes podem ser executados usando os recursos internos de teste de um IDE, como [Visual Studio](https://www.visualstudio.com/vs/). Se usar [código do Visual Studio](https://code.visualstudio.com/) ou a linha de comando, execute o seguinte comando no prompt de comando no *tests/RazorPagesTestingSample.Tests* pasta:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organização de aplicativo de mensagem

O aplicativo de mensagem é um sistema de mensagem páginas Razor simples com as seguintes características:

* A página de índice do aplicativo (*Pages/Index.cshtml* e *Pages/Index.cshtml.cs*) fornece uma interface do usuário e a página de métodos de modelo para controlar a adição, exclusão e análise de mensagens (palavras médias por mensagem) .
* Uma mensagem é descrita pelo `Message` classe (*Data/Message.cs*) com duas propriedades: `Id` (chave) e `Text` (mensagem). O `Text` propriedade é necessária e limitada a 200 caracteres.
* As mensagens são armazenadas usando [banco de dados do Entity Framework na memória](/ef/core/providers/in-memory/)&#8224;.
* O aplicativo contém uma camada de acesso de dados (DAL) em sua classe de contexto de banco de dados, `AppDbContext` (*Data/AppDbContext.cs*). Os métodos DAL são marcados como `virtual`, que permite a simulação de métodos para uso em testes.
* No ambiente de desenvolvimento, o repositório de mensagens foi inicializado com três mensagens. Essas *propagado mensagens* também são usados no teste.

&#8224; O tópico EF [testes com InMemory](/ef/core/miscellaneous/testing/in-memory), explica como usar um banco de dados na memória para testes com MSTest. Este tópico usa o [xUnit](https://xunit.github.io/) estrutura de teste. Conceitos de teste e implementações de teste em estruturas de teste diferentes são semelhantes, mas não idêntica.

Embora o aplicativo não usa o [padrão repositório](http://martinfowler.com/eaaCatalog/repository.html) e não é um exemplo efetivação do [padrão de unidade de trabalho (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), páginas Razor dá suporte a esses padrões de desenvolvimento. Para obter mais informações, consulte [criar a camada de persistência de infraestrutura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementando o repositório e padrões de unidade de trabalho em um aplicativo ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), e [teste lógica do controlador](/aspnet/core/mvc/controllers/testing) (o exemplo implementa o padrão de repositório).

## <a name="test-app-organization"></a>Organização do aplicativo de teste

O aplicativo de teste é um aplicativo de console dentro de *tests/RazorPagesTestingSample.Tests* pasta:

| Pasta do aplicativo de teste    | Descrição |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs* contém os testes de integração para a página de índice.</li><li>*TestFixture.cs* cria o host de teste para testar o aplicativo de mensagem.</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* contém testes de unidade para a DAL.</li><li>*IndexPageTest.cs* contém testes de unidade para o modelo de página de índice.</li></ul> |
| *Utilitários*        | *Utilities.CS* contém o:<ul><li>`TestingDbContextOptions`método usado para criar o novo banco de dados opções de contexto para cada teste de unidade da DAL, para que o banco de dados é redefinido para sua condição de linha de base para cada teste.</li><li>`GetRequestContentAsync`método usado para preparar o `HttpClient` e o conteúdo para solicitações que são enviadas para o aplicativo de mensagem durante o teste de integração.</li></ul>

A estrutura de teste é [xUnit](https://xunit.github.io/). O objeto do framework de simulação é [Moq](https://github.com/moq/moq4). Testes de integração são realizadas usando o [Host de teste do ASP.NET Core](xref:testing/integration-testing#the-test-host).

## <a name="unit-testing-the-data-access-layer-dal"></a>Camada de acesso a dados (DAL) de teste de unidade

O aplicativo de mensagem tem uma DAL com quatro métodos contidos no `AppDbContext` classe (*src/RazorPagesTestingSample/Data/AppDbContext.cs*). Cada método tem um ou dois testes de unidade em que o aplicativo de teste.

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

O problema com essa abordagem é que cada teste recebe o banco de dados no estado em que o teste anterior a deixou. Isso pode ser um problema ao tentar gravar testes de unidade atômica que não interfiram uns aos outros. Para forçar o `AppDbContext` para usar um novo contexto de banco de dados para cada teste, forneça um `DbContextOptions` instância com base em um novo provedor de serviço. O aplicativo de teste mostra como fazer isso usando seu `Utilities` método de classe `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Usando o `DbContextOptions` na unidade de DAL testes permite que cada teste ser executada atomicamente com um uma instância de banco de dados atualizado:

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

Por exemplo, o `DeleteMessageAsync` método é responsável por remover uma única mensagem identificada por seu `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

Há dois testes para este método. Um teste verifica se o método exclui uma mensagem quando a mensagem está presente no banco de dados. Os outros testes de método que o banco de dados não serão alteradas se a mensagem `Id` para exclusão não existe. O `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` método é mostrado abaixo:

[!code-csharp[Main](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Primeiro, o método executa a etapa de organizar, onde a preparação para a etapa de ação ocorre. As mensagens de propagação são obtidas e mantidas em `seedMessages`. As mensagens de propagação são salvos no banco de dados. A mensagem com um `Id` de `1` está definido para exclusão. Quando o `DeleteMessageAsync` o método é executado, as mensagens esperadas devem ter todas as mensagens, exceto aquele com um `Id` de `1`. O `expectedMessages` variável representa esse resultado esperado.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

O método atua: O `DeleteMessageAsync` método é executado, passando o `recId` de `1`:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Por fim, o método obtém o `Messages` do contexto e o compara a `expectedMessages` declarando que os dois são iguais:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Para comparar os dois `List<Message>` são os mesmos:

* As mensagens são ordenadas por `Id`.
* Pares de mensagens são comparados no `Text` propriedade.

Um método de teste semelhante, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` verifica o resultado da tentativa de excluir uma mensagem que não existe. Nesse caso, as mensagens esperadas no banco de dados devem ser iguais para as mensagens reais após o `DeleteMessageAsync` o método é executado. Não deve haver nenhuma alteração ao conteúdo do banco de dados:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>Os métodos do modelo de página de teste de unidade

Outro conjunto de testes de unidade é responsável por testar os métodos do modelo de página. No aplicativo de mensagem, os modelos de página de índice são encontrados no `IndexModel` classe em *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.

| Método de modelo de página | Função |
| ----------------- | -------- | 
| `OnGetAsync` | Obtém as mensagens de DAL para a interface do usuário usando o `GetMessagesAsync` método. |
| `OnPostAddMessageAsync` | Se o `ModelState` é válido, chamadas `AddMessageAsync` para adicionar uma mensagem para o banco de dados. | 
| `OnPostDeleteAllMessagesAsync` | Chamadas `DeleteAllMessagesAsync` para excluir todas as mensagens no banco de dados. |
| `OnPostDeleteMessageAsync` | Executa `DeleteMessageAsync` para excluir uma mensagem com o `Id` especificado. |
| `OnPostAnalyzeMessagesAsync` | Se uma ou mais mensagens no banco de dados, calcula o número médio de palavras por mensagem. |

Os métodos do modelo de página são testados usando sete testes o `IndexPageTest` classe (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*). Os testes de usam o padrão de Act Assert organizar familiar. Esses testes se concentram em:

* Determinando se os métodos seguem o comportamento correto quando o `ModelState` é inválido.
* Confirmando os métodos geram corretas `IActionResult`.
* Verificando se as atribuições de valor de propriedade são feitas corretamente.

Geralmente, esse grupo de testes simular os métodos da DAL para produzir os dados esperados para a etapa de Act onde um método de modelo de página é executado. Por exemplo, o `GetMessagesAsync` método o `AppDbContext` é simulada para produzir saída. Quando esse método é executado um método de modelo de página, a simulação retorna o resultado. Os dados não vem do banco de dados. Isso cria condições de teste previsível e confiável para usando a DAL nos testes de modelo de página.

O `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` teste mostra como o `GetMessagesAsync` método é simulado para o modelo de página:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

Quando o `OnGetAsync` o método é executado na etapa Act, ele chama o modelo de página `GetMessagesAsync` método.

Etapa de ação de teste de unidade (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage`modelo de página `OnGetAsync` método (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

O `GetMessagesAsync` método DAL não retorna o resultado para a chamada de método. A versão fictícias do método retorna o resultado.

No `Assert` etapa, as mensagens reais (`actualMessages`) são atribuídos a partir do `Messages` propriedade do modelo de página. Também é realizada uma verificação de tipo quando as mensagens forem atribuídas. As mensagens esperadas e reais são comparadas por seus `Text` propriedades. O teste afirma que os dois `List<Message>` instâncias contêm as mesmas mensagens.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

Página criar outros testes neste grupo de objetos de modelo que incluem o `DefaultHttpContext`, o `ModelStateDictionary`, uma `ActionContext` para estabelecer o `PageContext`, um `ViewDataDictionary`e um `PageContext`. Eles são úteis para conduzir testes. Por exemplo, o aplicativo de mensagem estabelece um `ModelState` erro com `AddModelError` para verificar se uma opção válida `PageResult` é retornado quando `OnPostAddMessageAsync` é executado:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>Teste o aplicativo de integração

A integração de testes foco nos testes de componentes do aplicativo funcionam em conjunto. Testes de integração são realizadas usando o [Host de teste do ASP.NET Core](xref:testing/integration-testing#the-test-host). Processamento de ciclo de vida de resposta de solicitação completa é testado. Esses testes afirmar que a página gera o código de status correto e `Location` cabeçalho, se definido.

Uma exemplo do exemplo de teste de integração, verifica o resultado da solicitação de página de índice do aplicativo de mensagem (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

Não há nenhuma etapa de organizar. O `GetAsync` método é chamado no `HttpClient` para enviar uma solicitação GET para o ponto de extremidade. O teste declara que o resultado é um código de status 200 Okey.

Todas as solicitações POST para o aplicativo de mensagem devem satisfazer a verificação de antiforgery é criada automaticamente pelo aplicativo do [antiforgery sistema de proteção de dados](xref:security/data-protection/introduction). Para organizar para solicitação POST do teste, o aplicativo de teste deve:

1. Fazer uma solicitação para a página.
1. Analise o cookie antiforgery e o token de validação de solicitação da resposta.
1. Verifique a solicitação POST com a validação de cookie e solicitação antiforgery token em vigor.

O `Post_AddMessageHandler_ReturnsRedirectToRoot` método de teste:

* Prepara uma mensagem e o `HttpClient`.
* Faz uma solicitação POST para o aplicativo.
* Verifica que a resposta é um redirecionamento para a página de índice.

`Post_AddMessageHandler_ReturnsRedirectToRoot `método (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

O `GetRequestContentAsync` método utilitário gerencia Preparando o cliente com o cookie antiforgery e o token de solicitação de verificação. Observe como o método recebe um `IDictionary` que permite que o método de teste de chamada para transmitir dados para a solicitação codificar junto com o token de verificação de solicitação (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

Testes de integração também podem passar dados inválidos para o aplicativo para testar o comportamento de resposta do aplicativo. O aplicativo de mensagem limita o tamanho de mensagem para 200 caracteres (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

O `Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` teste `Message` explicitamente passa em texto com 201 caracteres "X". Isso resulta em um `ModelState` erro. A publicação não for redirecionada voltar à página de índice. Ele retorna um Okey de 200 com um `null` `Location` cabeçalho (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>Consulte também

* [Teste de unidade c# no .NET Core usando xUnit e teste dotnet](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Teste de integração](xref:testing/integration-testing)
* [Testando os controladores](xref:mvc/controllers/testing)
* [O código de teste de unidade](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [Guia de Introdução ao xUnit.net (.NET Core/ASP.NET núcleos)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guia de início rápido Moq](https://github.com/Moq/moq4/wiki/Quickstart)
