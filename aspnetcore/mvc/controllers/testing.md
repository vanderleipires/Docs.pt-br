---
title: "Testando a lógica do controlador no ASP.NET Core"
author: ardalis
description: "Saiba como testar a lógica do controlador no ASP.NET Core com o Moq e o xUnit."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: cabb1d2498e6c993b327c2fb9719525ec2181f9e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="testing-controller-logic-in-aspnet-core"></a>Testando a lógica do controlador no ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Os controladores em aplicativos ASP.NET MVC devem ser pequenos e estar voltados para interesses da interface do usuário. Controladores grandes que lidam com interesses não referentes à interface do usuário são mais difíceis de serem testados e mantidos.

[Exibir ou baixar a amostra do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Testando os controladores

Os controladores são uma parte central de qualquer aplicativo ASP.NET Core MVC. Assim, você deve estar confiante de que eles se comportam conforme o esperado para o aplicativo. Testes automatizados podem fornecer essa confiança e podem detectar erros antes que eles atinjam a produção. É importante evitar colocar responsabilidades desnecessárias dentro dos controladores e garantir que o teste tenha como foco somente as responsabilidades do controlador.

A lógica do controlador deve ser mínima e não estar voltada para a lógica de negócios ou interesses de infraestrutura (por exemplo, acesso a dados). Teste a lógica do controlador, não a estrutura. Teste como o controlador *se comporta* de acordo com as entradas válidas ou inválidas. Teste as respostas do controlador de acordo com o resultado da operação de negócios executada por ele.

Responsabilidades típicas do controlador:

* Verificar `ModelState.IsValid`.
* Retornar uma resposta de erro se `ModelState` for inválido.
* Recuperar uma entidade de negócios da persistência.
* Executar uma ação na entidade de negócios.
* Salvar a entidade de negócios para persistência.
* Retornar um `IActionResult` apropriado.

## <a name="unit-testing"></a>Teste de unidade

O [teste de unidade](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) envolve o teste de uma parte de um aplicativo de teste em isolamento de sua infraestrutura e suas dependências. Ao executar o teste de unidade na lógica do controlador, somente o conteúdo de uma única ação é testada, não o comportamento de suas dependências ou da própria estrutura. Ao executar o teste de unidade nas ações do controlador, concentre-se apenas em seu comportamento. Um teste de unidade do controlador evita itens como [filtros](filters.md), [roteamento](../../fundamentals/routing.md) ou [associação de modelos](../models/model-binding.md). Ao se concentrarem no teste de apenas uma coisa, os testes de unidade geralmente são simples de serem escritos e rápidos de serem executados. Um conjunto bem escrito de testes de unidade pode ser executado com frequência sem muita sobrecarga. No entanto, os testes de unidade não detectam problemas na interação entre componentes, que é a finalidade dos [testes de integração](xref:mvc/controllers/testing#integration-testing).

Se você estiver escrevendo filtros personalizados, rotas, etc., execute o teste de unidade neles, mas não como parte dos testes em uma ação específica do controlador. Eles devem ser testados em isolamento.

> [!TIP]
> [Crie e execute testes de unidade com o Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).

Para demonstrar o teste de unidade, examine o controlador a seguir. Ele exibe uma lista de sessões de debate e permite que novas sessões de debate sejam criadas com um POST:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

O controlador está seguindo o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/), esperando receber da injeção de dependência uma instância de `IBrainstormSessionRepository`. Isso facilita muito o teste com o uso de uma estrutura de objeto fictício, como o [Moq](https://www.nuget.org/packages/Moq/). O método `HTTP GET Index` não tem nenhum loop ou branch e chama apenas um método. Para testar este método `Index`, precisamos verificar se um `ViewResult` for retornado, com um `ViewModel` do método `List` do repositório.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

O método `HomeController` `HTTP POST Index` (mostrado acima) deve verificar se:

* O método de ação retorna um `ViewResult` de Solicitação Inválida com os dados apropriados quando `ModelState.IsValid` é `false`

* O método `Add` no repositório é chamado e um `RedirectToActionResult` é retornado com os argumentos corretos quando `ModelState.IsValid` é verdadeiro.

O estado de modelo inválido pode ser testado com a adição de erros usando `AddModelError`, conforme mostrado no primeiro teste abaixo.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

O primeiro teste confirma que quando `ModelState` não é válido, o mesmo `ViewResult` é retornado como para uma solicitação `GET`. Observe que o teste não tenta passar um modelo inválido. Isso não funcionará de qualquer forma, pois a associação de modelos não está em execução (embora um [teste de integração](xref:mvc/controllers/testing#integration-testing) use a associação de modelos de exercícios). Nesse caso, a associação de modelos não está sendo testada. Esses testes de unidade estão testando apenas o que o código faz no método de ação.

O segundo teste verifica que quando `ModelState` é válido, um novo `BrainstormSession` é adicionado (por meio do repositório) e o método retorna um `RedirectToActionResult` com as propriedades esperadas. Chamadas fictícias que não são chamadas são normalmente ignoradas, mas a chamada a `Verifiable` no final da chamada de instalação permite que ele seja verificada no teste. Isso é feito com a chamada a `mockRepo.Verify`, que não será aprovado no teste se o método esperado não tiver sido chamado.

> [!NOTE]
> A biblioteca do Moq usada nesta amostra facilita a combinação de simulações verificáveis ou "estritas" com simulações não verificáveis (também chamadas de simulações "flexíveis" ou stubs). Saiba mais sobre como [personalizar o comportamento de Simulação com o Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Outro controlador no aplicativo exibe informações relacionadas a uma sessão de debate específica. Ele inclui uma lógica para lidar com valores de ID inválidos:

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

A ação do controlador tem três casos a serem testados, um para cada instrução `return`:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

O aplicativo expõe a funcionalidade como uma API Web (uma lista de ideias associadas a uma sessão de debate e um método para adicionar novas ideias a uma sessão):

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

O método `ForSession` retorna uma lista de tipos `IdeaDTO`. Evite retornar as entidades de domínio de negócios diretamente por meio de chamadas à API, pois, com frequência, elas incluem mais dados do que o cliente de API exige e acoplam desnecessariamente o modelo de domínio interno do aplicativo à API exposta externamente. O mapeamento entre entidades de domínio e os tipos que você retornará de forma eletrônica pode ser feito manualmente (usando um LINQ `Select`, conforme mostrado aqui) ou uma biblioteca como o [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Os testes de unidade para os métodos de API `Create` e `ForSession`:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Conforme mencionado anteriormente, para testar o comportamento do método quando `ModelState` é inválido, adicione um erro de modelo ao controlador como parte do teste. Não tente testar a validação ou a associação de modelos nos testes de unidade – teste apenas o comportamento do método de ação quando houver um valor `ModelState` específico.

O segundo teste depende do repositório retornar nulo. Portanto, o repositório fictício é configurado para retornar nulo. Não é necessário criar um banco de dados de teste (em memória ou de outra forma) e construir uma consulta que retornará esse resultado – isso pode ser feito em uma única instrução, conforme mostrado.

O último teste verifica se o método `Update` do repositório é chamado. Como fizemos anteriormente, a simulação é chamada com `Verifiable` e, em seguida, o método `Verify` do repositório fictício é chamado para confirmar se o método verificável foi executado. Não é responsabilidade do teste de unidade garantir que o método `Update` salva os dados; isso pode ser feito com um teste de integração.

## <a name="integration-testing"></a>Teste de integração

O [teste de integração](../../testing/integration-testing.md) é feito para garantir que módulos separados dentro do aplicativo funcionem corretamente juntos. Em geral, qualquer coisa que você possa testar com um teste de unidade, também pode testar com um teste de integração, mas o contrário não é verdadeiro. No entanto, os testes de integração tendem a ser muito mais lentos do que os testes de unidade. Portanto, é melhor testar tudo o que você puder com testes de unidade e usar testes de integração para cenários que envolvem vários colaboradores.

Embora eles ainda possam ser úteis, objetos fictícios raramente são usados em testes de integração. Em um teste de unidade, objetos fictícios são uma maneira eficiente de controlar como os colaboradores fora da unidade que está sendo testada devem se comportar para fins do teste. Em um teste de integração, colaboradores reais são usados para confirmar que todo o subsistema funciona junto corretamente.

### <a name="application-state"></a>Estado do aplicativo

Uma consideração importante ao executar testes de integração é como configurar o estado do aplicativo. Os testes precisam ser executados de forma independente entre si e, portanto, cada teste deve ser iniciado com o aplicativo em um estado conhecido. Se o aplicativo não usa um banco de dados ou não tem nenhuma persistência, isso pode não ser um problema. No entanto, a maioria dos aplicativos do mundo real persiste seu estado para algum tipo de armazenamento de dados. Portanto, as modificações feitas por um teste podem afetar outro teste, a menos que o armazenamento de dados seja redefinido. Usando o `TestServer` interno, é muito simples hospedar aplicativos ASP.NET Core em nossos testes de integração, mas isso não necessariamente permite acesso aos dados que serão usados por ele. Caso esteja usando um banco de dados real, uma abordagem é conectar o aplicativo a um banco de dados de teste, que pode ser acessado pelos testes, e garantir que ele seja redefinido para um estado conhecido antes da execução de cada teste.

Neste aplicativo de exemplo, estou usando o suporte de InMemoryDatabase do Entity Framework Core e, portanto, não consigo me conectar a ele em meu projeto de teste. Em vez disso, exponho um método `InitializeDatabase` da classe `Startup` do aplicativo, que chamo quando o aplicativo é iniciado se ele está no ambiente `Development`. Meus testes de integração automaticamente se beneficiam com isso, desde que eles definam o ambiente como `Development`. Não preciso me preocupar com a redefinição do banco de dados, pois o InMemoryDatabase é redefinido sempre que o aplicativo é reiniciado.

A classe `Startup`:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Você verá o método `GetTestSession` que costuma ser usado nos testes de integração abaixo.

### <a name="accessing-views"></a>Acessando exibições

Cada classe de teste de integração configura o `TestServer` que executará o aplicativo ASP.NET Core. Por padrão, o `TestServer` hospeda o aplicativo Web na pasta em que ele está em execução – nesse caso, a pasta do projeto de teste. Portanto, quando você tentar testar ações do controlador que retornam `ViewResult`, poderá receber este erro:

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

Para corrigir esse problema, você precisa configurar a raiz de conteúdo do servidor, para que ela possa localizar as exibições para o projeto que está sendo testado. Isso é feito por uma chamada a `UseContentRoot` na classe `TestFixture`, conforme mostrado abaixo:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

A classe `TestFixture` é responsável por configurar e criar o `TestServer`, configurar um `HttpClient` para se comunicar com o `TestServer`. Cada um dos testes de integração usa a propriedade `Client` para se conectar ao servidor de teste e fazer uma solicitação.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

No primeiro teste acima, a `responseString` armazena o HTML real renderizado de View, que pode ser inspecionado para confirmar se ele contém os resultados esperados.

O segundo teste constrói um POST de formulário com um nome de sessão exclusivo e executa POST dele para o aplicativo, verificando, em seguida, se o redirecionamento esperado é retornado.

### <a name="api-methods"></a>Métodos de API

Se o aplicativo expõe APIs Web, é uma boa ideia fazer com que os testes automatizados confirmem se elas são executadas como esperado. O `TestServer` interno facilita o teste de APIs Web. Se os métodos de API estiverem usando a associação de modelos, você sempre deverá verificar `ModelState.IsValid` e os testes de integração são o melhor lugar para confirmar se a validação do modelo está funcionando corretamente.

O seguinte conjunto de testes é direcionado ao método `Create` na classe [IdeasController](xref:mvc/controllers/testing#ideas-controller) mostrada acima:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

Ao contrário dos testes de integração de ações que retornam exibições HTML, os métodos de API Web que retornam resultados podem normalmente ser desserializados como objetos fortemente tipados, como mostra o último teste acima. Nesse caso, o teste desserializa o resultado para uma instância `BrainstormSession` e confirma se a ideia foi corretamente adicionada à sua coleção de ideias.

Você encontrará outros exemplos de testes de integração no [projeto de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) deste artigo.
