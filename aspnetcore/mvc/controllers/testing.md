---
title: Lógica do controlador de teste no ASP.NET Core
author: ardalis
description: Saiba como testar a lógica do controlador no ASP.NET Core com o Moq e o xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751685"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Lógica do controlador de teste no ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Os controladores são uma parte central de qualquer aplicativo ASP.NET Core MVC. Assim, você deve estar confiante de que eles se comportam conforme o esperado para o aplicativo. Testes automatizados podem fornecer essa confiança e podem detectar erros antes que eles atinjam a produção. É importante evitar colocar responsabilidades desnecessárias dentro dos controladores e garantir que o teste tenha como foco somente as responsabilidades do controlador.

A lógica do controlador deve ser mínima e não estar voltada para a lógica de negócios ou interesses de infraestrutura (por exemplo, acesso a dados). Teste a lógica do controlador, não a estrutura. Teste como o controlador *se comporta* de acordo com as entradas válidas ou inválidas. Teste as respostas do controlador de acordo com o resultado da operação de negócios executada por ele.

Responsabilidades típicas do controlador:

* Verificar `ModelState.IsValid`.
* Retornar uma resposta de erro se `ModelState` for inválido.
* Recuperar uma entidade de negócios da persistência.
* Executar uma ação na entidade de negócios.
* Salvar a entidade de negócios para persistência.
* Retornar um `IActionResult` apropriado.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Testes de unidade da lógica do controlador

Os [testes de unidade](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) envolvem o teste de uma parte de um aplicativo isoladamente em relação à sua infraestrutura e às suas dependências. Ao executar o teste de unidade na lógica do controlador, somente o conteúdo de uma única ação é testada, não o comportamento de suas dependências ou da própria estrutura. Ao executar o teste de unidade nas ações do controlador, concentre-se apenas em seu comportamento. Um teste de unidade do controlador evita itens como [filtros](xref:mvc/controllers/filters), [roteamento](xref:fundamentals/routing) ou [associação de modelos](xref:mvc/models/model-binding). Ao se concentrarem no teste de apenas uma coisa, os testes de unidade geralmente são simples de serem escritos e rápidos de serem executados. Um conjunto bem escrito de testes de unidade pode ser executado com frequência sem muita sobrecarga. No entanto, os testes de unidade não detectam problemas na interação entre os componentes, que é a finalidade dos [testes de integração](xref:test/integration-tests).

Se você estiver escrevendo filtros personalizados e rotas, execute o teste de unidade neles de forma isolada, mas não como parte dos testes em uma ação do controlador específica.

> [!TIP]
> [Crie e execute testes de unidade com o Visual Studio](/visualstudio/test/unit-test-your-code).

Para demonstrar o teste de unidade, examine o controlador a seguir. Ele exibe uma lista de sessões de debate e permite que novas sessões de debate sejam criadas com um POST:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

O controlador está seguindo o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/), esperando receber da injeção de dependência uma instância de `IBrainstormSessionRepository`. Isso facilita muito o teste com o uso de uma estrutura de objeto fictício, como o [Moq](https://www.nuget.org/packages/Moq/). O método `HTTP GET Index` não tem nenhum loop ou branch e chama apenas um método. Para testar este método `Index`, precisamos verificar se um `ViewResult` for retornado, com um `ViewModel` do método `List` do repositório.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

O método `HomeController` `HTTP POST Index` (mostrado acima) deve verificar se:

* O método de ação retorna um `ViewResult` de Solicitação Inválida com os dados apropriados quando `ModelState.IsValid` é `false`.

* O método `Add` no repositório é chamado e um `RedirectToActionResult` é retornado com os argumentos corretos quando `ModelState.IsValid` é verdadeiro.

O estado de modelo inválido pode ser testado com a adição de erros usando `AddModelError`, conforme mostrado no primeiro teste abaixo.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

O primeiro teste confirma que quando `ModelState` não é válido, o mesmo `ViewResult` é retornado como para uma solicitação `GET`. Observe que o teste não tenta passar um modelo inválido. Isso não funcionará de qualquer forma, pois a associação de modelos não está em execução (embora um [teste de integração](xref:test/integration-tests) use a associação de modelos de exercícios). Nesse caso, a associação de modelos não está sendo testada. Esses testes de unidade estão testando apenas o que o código faz no método de ação.

O segundo teste verifica que quando `ModelState` é válido, um novo `BrainstormSession` é adicionado (por meio do repositório) e o método retorna um `RedirectToActionResult` com as propriedades esperadas. Chamadas fictícias que não são chamadas são normalmente ignoradas, mas a chamada a `Verifiable` no final da chamada de instalação permite que ele seja verificada no teste. Isso é feito com a chamada a `mockRepo.Verify`, que não será aprovado no teste se o método esperado não tiver sido chamado.

> [!NOTE]
> A biblioteca do Moq usada nesta amostra facilita a combinação de simulações verificáveis ou "estritas" com simulações não verificáveis (também chamadas de simulações "flexíveis" ou stubs). Saiba mais sobre como [personalizar o comportamento de Simulação com o Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Outro controlador no aplicativo exibe informações relacionadas a uma sessão de debate específica. Ele inclui uma lógica para lidar com valores de ID inválidos:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

A ação do controlador tem três casos a serem testados, um para cada instrução `return`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

O aplicativo expõe a funcionalidade como uma API Web (uma lista de ideias associadas a uma sessão de debate e um método para adicionar novas ideias a uma sessão):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

O método `ForSession` retorna uma lista de tipos `IdeaDTO`. Evite retornar as entidades de domínio de negócios diretamente por meio de chamadas à API, pois, com frequência, elas incluem mais dados do que o cliente de API exige e acoplam desnecessariamente o modelo de domínio interno do aplicativo à API exposta externamente. O mapeamento entre entidades de domínio e os tipos que você retornará de forma eletrônica pode ser feito manualmente (usando um LINQ `Select`, conforme mostrado aqui) ou uma biblioteca como a [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Os testes de unidade para os métodos de API `Create` e `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Conforme mencionado anteriormente, para testar o comportamento do método quando `ModelState` é inválido, adicione um erro de modelo ao controlador como parte do teste. Não tente testar a validação ou a associação de modelos nos testes de unidade – teste apenas o comportamento do método de ação quando houver um valor `ModelState` específico.

O segundo teste depende do repositório retornar nulo. Portanto, o repositório fictício é configurado para retornar nulo. Não é necessário criar um banco de dados de teste (em memória ou de outra forma) e construir uma consulta que retornará esse resultado – isso pode ser feito em uma única instrução, conforme mostrado.

O último teste verifica se o método `Update` do repositório é chamado. Como fizemos anteriormente, a simulação é chamada com `Verifiable` e, em seguida, o método `Verify` do repositório fictício é chamado para confirmar se o método verificável foi executado. Não é responsabilidade do teste de unidade garantir que o método `Update` salva os dados; isso pode ser feito com um teste de integração.

## <a name="additional-resources"></a>Recursos adicionais

* <xref:test/integration-tests>
