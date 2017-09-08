---
title: "Lógica de teste do controlador no núcleo do ASP.NET"
author: ardalis
description: "Aprenda a lógica do controlador de teste no núcleo do ASP.NET com Moq e xUnit."
keywords: "ASP.NET Core, controlador, teste, teste, teste de unidade, testes de unidade, teste de integração, testes de integração, xUnit"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: d5b2bd0200082000aeaf8015cfff9c8c1ec1bdd9
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a>Lógica de teste do controlador no núcleo do ASP.NET

Por [Steve Smith](http://ardalis.com)

Os controladores em aplicativos ASP.NET MVC devem ser pequeno e se concentrem em questões de interface do usuário. Grandes controladores que lidam com preocupações de interface de usuário não são mais difíceis de testar e manter.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Controladores de teste

Os controladores são uma parte central de qualquer aplicativo MVC do ASP.NET Core. Como tal, você deve ter confiança que se comportam conforme o esperado para seu aplicativo. Testes automatizados podem fornecer essa confiança e podem detectar erros antes que elas atinjam a produção. É importante evitar colocar desnecessárias responsabilidades dentro de seus controladores e certifique-se o foco de testes somente nas responsabilidades de controlador.

Lógica do controlador deve ser mínima e não se concentrar na lógica ou infraestrutura do negócio (por exemplo, acesso a dados). Lógica do controlador, não o framework de teste. Teste como o controlador *se comporta* com base em entradas válidas ou inválidas. Teste as respostas do controlador com base no resultado da operação de negócios que ele executa.

Responsabilidades do controlador típico:

* Verifique se `ModelState.IsValid`.
* Retornar uma resposta de erro se `ModelState` é inválido.
* Recupere uma entidade de negócios de persistência.
* Execute uma ação na entidade de negócios.
* Salve a entidade de negócios para persistência.
* Retornar um apropriado `IActionResult`.

## <a name="unit-testing"></a>Testes de unidade

[Teste de unidade](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) envolve uma parte de um aplicativo de teste em isolamento de sua infraestrutura e as dependências. Quando a lógica do controlador, somente o conteúdo de uma única ação de teste de unidade é testada, não o comportamento de suas dependências ou a própria estrutura. Como você unidade suas ações do controlador de teste, verifique se que você se concentrar apenas em seu comportamento. Um teste de unidade do controlador evita coisas como [filtros](filters.md), [roteamento](../../fundamentals/routing.md), ou [associação de modelo](../models/model-binding.md). Focalizar no teste apenas uma coisa, testes de unidade geralmente são simples de escrever e rápido executar. Um conjunto bem escrito de testes de unidade pode ser executado com frequência sem muita sobrecarga. No entanto, os testes de unidade não detectam problemas na interação entre componentes, que é a finalidade de [testes de integração](xref:mvc/controllers/testing#integration-testing).

Se você estiver escrevendo filtros personalizados, rotas, etc, faça o teste de unidade-los, mas não como parte de seus testes em uma ação do controlador específico. Eles devem ser testados em isolamento.

> [!TIP]
> [Criar e executar testes de unidade com o Visual Studio](https://www.visualstudio.com/get-started/code/create-and-run-unit-tests-vs).

Para demonstrar o teste de unidade, examine o seguinte controlador. Ele exibe uma lista de sessões de debate e permite que novas sessões serão criados com uma POSTAGEM de discussão:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

O controlador é após o [princípio dependências explícitas](http://deviq.com/explicit-dependencies-principle/), esperando injeção de dependência para fornecê-la com uma instância do `IBrainstormSessionRepository`. Isso facilita bastante para teste usando uma estrutura de objeto, como [Moq](https://www.nuget.org/packages/Moq/). O `HTTP GET Index` método não tem nenhum método de um loop ou ramificação e somente chamadas. Para testar isso `Index` método, precisamos verificar se um `ViewResult` for retornado, com um `ViewModel` do repositório de `List` método.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

O `HomeController` `HTTP POST Index` método (mostrado acima) Verifique se:

* O método de ação retorna uma solicitação incorreta `ViewResult` com os dados apropriados quando `ModelState.IsValid` é`false`

* O `Add` é chamado de método no repositório e um `RedirectToActionResult` é retornado com os argumentos corretos quando `ModelState.IsValid` é verdadeiro.

Estado de modelo inválido pode ser testado com a adição de erros usando `AddModelError` conforme mostrado no primeiro teste.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

O primeiro teste confirma quando `ModelState` não é válido, o mesmo `ViewResult` é retornado como um `GET` solicitação. Observe que o teste não tente passar de um modelo inválido. Não funciona mesmo assim como a associação de modelo não está em execução (embora uma [teste de integração](xref:mvc/controllers/testing#integration-testing) usaria exercício associação de modelo). Nesse caso, associação de modelo não está sendo testada. Esses testes de unidade estão testando apenas o que faz o código no método de ação.

O segundo teste verifica que, quando `ModelState` for válido, um novo `BrainstormSession` é adicionada (por meio do repositório), e o método retorna um `RedirectToActionResult` com as propriedades esperadas. Fictícias chamadas que não forem chamadas são normalmente ignorado, mas a chamada `Verifiable` no final da instalação chamada permite que ele seja verificado no teste. Isso é feito com a chamada para `mockRepo.Verify`, que irá falhar no teste se o método esperado não foi chamado.

> [!NOTE]
> A biblioteca Moq usada neste exemplo torna fácil misturar simulações verificáveis ou "strict", com as simulações não verificável (também chamadas de "livre" simulações ou stubs). Saiba mais sobre [personalizar o comportamento de simulação com Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Outro controlador no aplicativo exibe informações relacionadas a uma sessão de debate específico. Ele inclui alguma lógica para lidar com valores de id inválida:

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

A ação do controlador tem três casos de teste, uma para cada `return` instrução:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

O aplicativo expõe a funcionalidade como uma API (uma lista de ideias associados a uma sessão de debate e um método para adicionar novas ideias para uma sessão) da web:

<a name=ideas-controller></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

O `ForSession` método retorna uma lista de `IdeaDTO` tipos. Evite retornar as entidades de domínio de negócios diretamente por meio de chamadas de API, desde frequentemente incluem mais dados do que o cliente API requer e eles desnecessariamente acoplar o modelo de domínio interno do aplicativo com a API que você expõe externamente. Mapeamento entre as entidades de domínio e os tipos que você irá retornar eletronicamente pode ser feito manualmente (usando um LINQ `Select` conforme mostrado aqui) ou usando uma biblioteca como [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Testes de unidade para o `Create` e `ForSession` métodos de API:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Como mencionado anteriormente, para testar o comportamento do método quando `ModelState` é inválido, adicionar um erro de modelo para o controlador como parte do teste. Não tente associação de modelo de validação ou modelo de teste nos testes de unidade - testar apenas o comportamento do seu método de ação quando houver uma com um determinado `ModelState` valor.

O segundo teste depende do repositório retornando nulo, para que o repositório de simulação é configurado para retornar null. Não é necessário para criar um banco de dados de teste (na memória ou de outra forma) e criar uma consulta que retornará esse resultado - ele pode ser feito em uma única instrução, conforme mostrado.

O último teste verifica se o repositório `Update` método é chamado. Como foi feito anteriormente, a simulação é chamada com `Verifiable` e, em seguida, fictícios do repositório `Verify` método é chamado para confirmar se o método verificável foi executado. Não é a responsabilidade de teste de unidade para garantir que o `Update` método salva os dados; isso pode ser feito com um teste de integração.

## <a name="integration-testing"></a>Testes de integração

[Testes de integração](../../testing/integration-testing.md) é feito para garantir módulos separados dentro de seu trabalho de aplicativo corretamente juntos. Em geral, qualquer coisa que você pode testar com um teste de unidade, você também pode testar com um teste de integração, mas o contrário não é true. No entanto, os testes de integração tendem a ser muito mais lento do que testes de unidade. Assim, é melhor testar tudo o que você pode com testes de unidade e usar testes de integração para cenários que envolvem vários parceiros.

Embora ainda pode ser útil, objetos fictícios raramente são usados em testes de integração. Em testes de unidade, objetos fictícios são uma maneira eficiente para controlar o comportamento do parceiros fora a unidade que está sendo testada para fins de teste. Em um teste de integração, colaboradores reais são usadas para confirmar que o subsistema inteiro juntos funciona corretamente.

### <a name="application-state"></a>Estado do aplicativo

Uma consideração importante ao executar testes de integração é a configuração de estado do aplicativo. Testes precisam ser executados independentemente um do outro e, portanto cada teste deve começar com o aplicativo em um estado conhecido. Se seu aplicativo não usar um banco de dados ou tiverem qualquer persistência, isso não pode ser um problema. No entanto, a maioria dos aplicativos do mundo real manter seu estado para algum tipo de repositório de dados, para que as modificações feitas por um teste podem afetar outro teste, a menos que o repositório de dados é redefinido. Usando o interno `TestServer`, é muito simples para hospedar aplicativos do ASP.NET Core em nossos testes de integração, mas que não necessariamente concede acesso aos dados que será usado. Se você estiver usando um banco de dados real, uma abordagem é ter o aplicativo se conectar a um banco de dados de teste, que os testes podem acessar e verifique se é redefinido para um estado conhecido antes de executa cada teste.

Neste aplicativo de exemplo, estou usando o suporte de InMemoryDatabase do Entity Framework Core, portanto apenas não consigo conectar a ele do meu projeto de teste. Em vez disso, expõem um `InitializeDatabase` método a partir do aplicativo `Startup` classe, o que posso chamar quando o aplicativo é iniciado se ele estiver no `Development` ambiente. Meu testes de integração automaticamente se beneficiar com isso, desde definir o ambiente `Development`. Não preciso se preocupar sobre como redefinir o banco de dados, pois o InMemoryDatabase é redefinido cada vez que o aplicativo reinicia.

O `Startup` classe:

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Você verá o `GetTestSession` método costumam ser usado em testes de integração abaixo.

### <a name="accessing-views"></a>Acessar modos de exibição

Cada classe de teste de integração configura o `TestServer` que executará o aplicativo ASP.NET Core. Por padrão, `TestServer` hospeda o aplicativo web na pasta onde ele está em execução - nesse caso, a pasta de projeto de teste. Assim, quando você tentar testar ações do controlador que retornam `ViewResult`, você pode ver este erro:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

Para corrigir esse problema, você precisa configurar a raiz do conteúdo do servidor, para que ele possa localizar os modos de exibição para o projeto que está sendo testado. Isso é feito por uma chamada para `UseContentRoot` no `TestFixture` classe, como mostrado abaixo:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

O `TestFixture` classe é responsável por configurar e criar o `TestServer`, configurar o um `HttpClient` para se comunicar com o `TestServer`. Cada uma da integração de testes usa o `Client` propriedade para se conectar ao servidor de teste e fazer uma solicitação.

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

No primeiro teste acima, o `responseString` mantém real renderizado o HTML da exibição, que pode ser inspecionada para confirmar que ele contém os resultados esperados.

O segundo teste constrói um POSTAGEM de formulário com um nome de sessão exclusiva e publica o aplicativo, verifica se o redirecionamento esperado é retornado.

### <a name="api-methods"></a>Métodos de API

Se seu aplicativo exponha APIs, ele é uma boa ideia ter testes automatizados confirmar que eles sejam executados conforme o esperado da web. O interno `TestServer` torna mais fácil testar as APIs da web. Se seus métodos de API estiver usando a associação de modelo, você sempre deve verificar `ModelState.IsValid`, e testes de integração são o melhor lugar para confirmar se a validação de modelo está funcionando corretamente.

O seguinte conjunto de destino de testes de `Create` método o [IdeasController](xref:mvc/controllers/testing#ideas-controller) classe mostrado acima:

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

Diferentemente de testes de integração de ações que retorna os modos de exibição HTML, os métodos de API da web que retornam resultados podem normalmente ser desserializados como objetos fortemente tipados, como mostra o último teste acima. Nesse caso, o teste desserializa o resultado para um `BrainstormSession` de instância e confirma que a ideia corretamente foi adicionada à sua coleção de ideias.

Você encontrará exemplos adicionais de testes de integração neste artigo [projeto de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).
