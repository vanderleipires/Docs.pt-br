---
title: Testes de integração no núcleo do ASP.NET
author: guardrex
description: Saiba como testes de integração garantem que os componentes do aplicativo funcionem corretamente no nível de infraestrutura, incluindo o banco de dados, o sistema de arquivos e a rede.
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 1895b06f1af9a9eb66c14aa5c7834497fc95d583
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277690"
---
# <a name="integration-tests-in-aspnet-core"></a>Testes de integração no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

Testes de integração Certifique-se de que os componentes do aplicativo funcionam corretamente em um nível que inclui a infra-estrutura de suporte do aplicativo, como o banco de dados, o sistema de arquivos e a rede. ASP.NET Core dá suporte a testes de integração usando uma estrutura de teste de unidade com um host de teste da web e um servidor de teste em memória.

Este tópico pressupõe um entendimento básico de testes de unidade. Se estiver familiarizado com conceitos de teste, consulte o [testes de unidade no .NET Core e .NET padrão](/dotnet/core/testing/) tópico e seu conteúdo vinculado.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O aplicativo de exemplo é um aplicativo de páginas Razor e pressupõe um entendimento básico de páginas Razor. Se estiver familiarizado com páginas Razor, consulte os tópicos a seguir:

* [Introdução a Páginas do Razor](xref:razor-pages/index)
* [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Testes de unidades de páginas Razor](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Introdução a testes de integração

Testes de integração avaliar os componentes do aplicativo em um nível mais amplo que [testes de unidade](/dotnet/core/testing/). Testes de unidade são usados para testar os componentes de software isolado, como métodos de classe individual. Testes de integração confirme que dois ou mais componentes do aplicativo funcionam em conjunto para produzir um resultado esperado, possivelmente incluindo todos os componentes necessários para processar totalmente uma solicitação.

Esses testes mais amplos são usados para testar a infraestrutura do aplicativo e o framework inteiro, geralmente, incluindo os seguintes componentes:

* Banco de Dados
* Sistema de arquivos
* Dispositivos de rede
* Pipeline de solicitação-resposta

Testes de unidade gerado de usar componentes, conhecidos como *falsificações* ou *simular objetos*, no lugar de componentes de infraestrutura.

Em contraste com testes de unidade, testes de integração:

* Use os componentes reais que o aplicativo usa em produção.
* Exigir mais código e o processamento de dados.
* Pode levar mais tempo.

Portanto, limite o uso dos testes de integração para os cenários mais importantes de infraestrutura. Se um comportamento pode ser testado usando um teste de unidade ou um teste de integração, escolha o teste de unidade.

> [!TIP]
> Não grave testes de integração para cada permutação possíveis de acesso a dados e arquivos com bancos de dados e sistemas de arquivos. Independentemente de quantas casas em um aplicativo interagir com bancos de dados e sistemas de arquivos, um conjunto de foco de leitura, gravação, atualização e exclusão integração testes são geralmente capazes de banco de dados de teste adequadamente e componentes do sistema de arquivos. Testes de unidade de uso para testes de lógica de método de rotina que interagem com esses componentes. Em testes de unidade, o uso da infraestrutura de falsificações/simulações resultado na execução de teste mais rápida.

> [!NOTE]
> Em discussões de testes de integração, o projeto testado é frequentemente chamado de *sistema em teste*, ou "SUT", de forma abreviada.

## <a name="aspnet-core-integration-tests"></a>Testes de integração do ASP.NET Core

Testes de integração no núcleo do ASP.NET requerem o seguinte:

* Um projeto de teste é usado para conter e executar os testes. O projeto de teste tem uma referência para o projeto ASP.NET Core testado, chamado de *sistema em teste* (SUT). _"SUT" é usada ao longo deste tópico para se referir ao aplicativo testado._
* O projeto de teste cria um host de teste da web para o SUT e usa um cliente do servidor de teste para manipular solicitações e respostas de SUT.
* Um executor de teste é usada para executar os testes e relatório dos resultados de teste.

Testes de integração seguem uma sequência de eventos que incluem o usual *organizar*, *Act*, e *Assert* etapas de teste:

1. O host da web do SUT está configurado.
1. Um cliente do servidor de teste é criado para enviar solicitações para o aplicativo.
1. O *organizar* etapa do teste é executada: O aplicativo de teste prepara uma solicitação.
1. O *Act* etapa do teste é executada: O cliente envia a solicitação e receber a resposta.
1. O *Assert* etapa do teste é executada: O *real* resposta é validada como um *passar* ou *falha* com base em um *esperado*  resposta.
1. O processo continua até que todos os testes são executados.
1. Os resultados de teste são relatados.

Normalmente, o host de teste da web está configurado Diferentemente de host de web normal do aplicativo para o teste é executado. Por exemplo, configurações de aplicativo diferente ou outro banco de dados podem ser usadas para os testes.

Componentes de infraestrutura, como o host do teste da web e o servidor de teste em memória ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), são fornecidas ou gerenciados pelo [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pacote. O uso deste pacote simplifica a criação de teste e a execução.

O `Microsoft.AspNetCore.Mvc.Testing` pacote lida com as seguintes tarefas:

* Copia o arquivo de dependências (*\*.deps*) de SUT para o projeto de teste *bin* pasta.
* Define a raiz de conteúdo para a raiz do projeto do SUT para que arquivos estáticos e páginas/modos de exibição são encontrados quando os testes são executados.
* Fornece o [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) classe para simplificar a inicialização SUT com `TestServer`.

O [testes de unidade](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentação descreve como configurar um projeto e teste executor de teste, junto com instruções detalhadas sobre como executar testes e recomendações sobre como a testes de nome e classes de teste.

> [!NOTE]
> Ao criar um projeto de teste para um aplicativo, separe os testes de unidade de testes de integração em diferentes projetos. Isso ajuda a garantir que componentes da infraestrutura de teste não sejam acidentalmente incluído nos testes de unidade. Também permite a separação de testes de unidade e a integração de controle sobre qual conjunto de testes são executados.

Não há praticamente nenhuma diferença entre a configuração para testes de aplicativos do Razor páginas e aplicativos MVC. A única diferença está em como os testes são nomeados. Em um aplicativo de páginas Razor, testes de pontos de extremidade de página geralmente são nomeados com a classe de modelo de página (por exemplo, `IndexPageTests` para testar a integração do componente para a página de índice). Em um aplicativo MVC, testes geralmente são organizados por classes do controlador e nomeados de acordo com os controladores de testam por eles (por exemplo, `HomeControllerTests` para testar a integração do componente para o controlador Home).

## <a name="test-app-prerequisites"></a>Pré-requisitos de aplicativo de teste

O projeto de teste deve:

* Uma referência de pacote para [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Use o SDK da Web no arquivo de projeto (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Essas prerequesities pode ser vistos no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Inspecione o *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* arquivo.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Testes básicos com o padrão WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) é usado para criar um [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) para os testes de integração. `TEntryPoint` é a classe de ponto de entrada de SUT, geralmente o `Startup` classe.

Teste classes implementam um *acessório classe* interface (`IClassFixture`) para indicar a classe contém testes e fornece instâncias de objeto compartilhado em todos os testes na classe.

### <a name="basic-test-of-app-endpoints"></a>Teste básico de pontos de extremidade do aplicativo

Classe de teste a seguir `BasicTests`, usa o `WebApplicationFactory` para inicializar o SUT e fornecer um [HttpClient](/dotnet/api/system.net.http.httpclient) para um método de teste, `Get_EndpointsReturnSuccessAndCorrectContentType`. O método verifica se o código de status de resposta foi bem-sucedida (códigos de status no intervalo de 200 299) e o `Content-Type` cabeçalho `text/html; charset=utf-8` para várias páginas do aplicativo.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) cria uma instância de `HttpClient` que segue redireciona automaticamente e manipula cookies.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Testar um ponto de extremidade seguro

Outro teste no `BasicTests` classe verifica que um ponto de extremidade seguro redireciona um usuário não autenticado a página de logon do aplicativo.

Em SUT, o `/SecurePage` página usa um [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção para aplicar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página. Para obter mais informações, consulte [convenções de autorização de páginas Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

No `Get_SecurePageRequiresAnAuthenticatedUser` de teste, uma [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) está configurado para não permitir redirecionamentos definindo [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) para `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Proibindo o cliente para acompanhar o redirecionamento, podem ser feitas as seguintes verificações:

* O código de status retornado pelo SUT pode ser verificado em relação a esperado [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) resultado, não o código de status final após o redirecionamento para a página de logon, o que seria [Httpstatuscode](/dotnet/api/system.net.httpstatuscode).
* O `Location` valor de cabeçalho em cabeçalhos de resposta é verificado para confirmar que ela começa com `http://localhost/Identity/Account/Login`, não a logon página resposta final, onde o `Location` cabeçalho não estar presente.

Para obter mais informações sobre `WebApplicationFactoryClientOptions`, consulte o [opções de cliente](#client-options) seção.

## <a name="customize-webapplicationfactory"></a>Personalizar WebApplicationFactory

Configuração do host da Web pode ser criada independentemente de classes de teste herdando de `WebApplicationFactory` para criar um ou mais fábricas personalizadas:

1. Herdar de `WebApplicationFactory` e substituir [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). O [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) permite a configuração da coleção de serviço com [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Banco de dados a propagação no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) é executada pelo `InitializeDbForTests` método. O método descrito o [integração testa o exemplo: organização de aplicativo de teste](#test-app-organization) seção.

2. Use a custom `CustomWebApplicationFactory` em classes de teste. O exemplo a seguir usa a fábrica no `IndexPageTests` classe:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Cliente do aplicativo de exemplo está configurado para impedir a `HttpClient` de seguir os redirecionamentos. Conforme explicado no [testar um ponto de extremidade seguro](#test-a-secure-endpoint) seção, isso permite que os testes para verificar o resultado da primeira de resposta do aplicativo. A primeira resposta é um redirecionamento de muitos desses testes com um `Location` cabeçalho.

3. Um teste típico usa a `HttpClient` e métodos auxiliares para processar a solicitação e resposta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Todas as solicitações POST para o SUT devem atender a verificação de antiforgery é criada automaticamente pelo aplicativo do [antiforgery sistema de proteção de dados](xref:security/data-protection/introduction). Para organizar para solicitação POST do teste, o aplicativo de teste deve:

1. Fazer uma solicitação para a página.
1. Analise o cookie antiforgery e o token de validação de solicitação da resposta.
1. Verifique a solicitação POST com a validação de cookie e solicitação antiforgery token em vigor.

O `SendAsync` métodos de extensão auxiliares (*Helpers/HttpClientExtensions.cs*) e o `GetDocumentAsync` método auxiliar (*Helpers/HtmlHelpers.cs*) no [doaplicativodeexemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) usar o [AngleSharp](https://anglesharp.github.io/) analisador para lidar com a verificação de antiforgery com os seguintes métodos:

* `GetDocumentAsync` &ndash; Recebe o [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) e retorna um `IHtmlDocument`. `GetDocumentAsync` usa uma fábrica que prepara um *resposta virtual* com base no original `HttpResponseMessage`. Para obter mais informações, consulte o [AngleSharp documentação](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` métodos de extensão para o `HttpClient` compor uma [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) e chame [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) para enviar solicitações para o SUT. Sobrecargas `SendAsync` aceite o formulário HTML (`IHtmlFormElement`) e o seguinte:
  - Enviar um botão do formulário (`IHtmlElement`)
  - Coleção de valores de formulário (`IEnumerable<KeyValuePair<string, string>>`)
  - Botão de envio (`IHtmlElement`) e valores de formulário (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) é um terceiro biblioteca usada para fins de demonstração neste tópico e o aplicativo de exemplo de análise. AngleSharp não é suportada ou necessárias para testes de integração de aplicativos do ASP.NET Core. Outros analisadores podem usados, como o [Html agilidade Pack (HAP)](http://html-agility-pack.net/). Outra abordagem é escrever um código para lidar com o token de verificação de solicitação e o cookie antiforgery o sistema antiforgery diretamente.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizar o cliente com WithWebHostBuilder

Quando a configuração adicional é necessária em um método de teste, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) cria um novo `WebApplicationFactory` com um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) personalizada posteriormente pela configuração.

O `Post_DeleteMessageHandler_ReturnsRedirectToRoot` testar o método do [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstra o uso de `WithWebHostBuilder`. Esse teste executa uma exclusão de registro no banco de dados disparando uma submissão de formulário no SUT.

Porque outro teste no `IndexPageTests` classe executa uma operação que exclui todos os registros no banco de dados e pode ser executada antes do `Post_DeleteMessageHandler_ReturnsRedirectToRoot` método, o banco de dados é propagado nesse método de teste para garantir que um registro está presente para SUT excluir. Selecionando o `deleteBtn1` botão do `messages` formulário no SUT é simulado na solicitação para o SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opções do cliente

A tabela a seguir mostra o padrão [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponíveis ao criar `HttpClient` instâncias.

| Opção | Descrição | Padrão |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Obtém ou define se `HttpClient` instâncias automaticamente devem seguir respostas de redirecionamento. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Obtém ou define o endereço base do `HttpClient` instâncias. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Obtém ou define se `HttpClient` instâncias devem tratar os cookies. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Obtém ou define o número máximo de respostas de redirecionamento que `HttpClient` instâncias devem seguir. | 7 |

Criar o `WebApplicationFactoryClientOptions` classe e passá-lo para o [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) método (os valores são mostrados no exemplo de código padrão):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Como a infraestrutura de teste infere o caminho de conteúdo raiz do aplicativo

O `WebApplicationFactory` construtor infere o caminho de conteúdo raiz do aplicativo por meio de pesquisa um [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) no assembly que contém os testes de integração com uma chave igual a `TEntryPoint` assembly `System.Reflection.Assembly.FullName`. No caso de um atributo com a chave correta não for encontrado, `WebApplicationFactory` reverterá para procurar por um arquivo de solução (*\*. sln*) e anexa o `TEntryPoint` nome do assembly para o diretório da solução. O diretório raiz do aplicativo (o caminho de conteúdo raiz) é usado para descobrir os modos de exibição e arquivos de conteúdo.

Na maioria dos casos, não é necessário definir explicitamente a raiz de conteúdo do aplicativo, como a lógica de pesquisa geralmente localiza a raiz de conteúdo correta em tempo de execução. Em situações especiais onde a conteúdo raiz não foi encontrada usando o algoritmo de pesquisa interna, o aplicativo de conteúdo raiz pode ser especificada explicitamente ou usando uma lógica personalizada. Para definir a raiz de conteúdo do aplicativo nessas situações, chame o `UseSolutionRelativeContentRoot` método de extensão do [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) pacote. Forneça um caminho relativo da solução e o padrão de nome ou glob do arquivo de solução opcional (padrão = `*.sln`).

Chamar o [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) extensão usando o método *um* das seguintes abordagens:

* Ao configurar classes de teste com `WebApplicationFactory`, fornecer uma configuração personalizada com o [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Ao configurar classes de teste com um personalizado `WebApplicationFactory`, herdam `WebApplicationFactory` e substituir [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Desativar a cópia de sombra

Cópia de sombra faz com que os testes executar em uma pasta diferente que a pasta de saída. Para testes funcione corretamente, a cópia de sombra deve ser desabilitada. O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) usa xUnit e desabilita a cópia de sombra para xUnit, incluindo um *xunit.runner.json* arquivo com a configuração correta. Para obter mais informações, consulte [Configurando xUnit.net com JSON](https://xunit.github.io/docs/configuring-with-json.html).

Adicionar o *xunit.runner.json* arquivo para a raiz do projeto de teste com o seguinte conteúdo:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Exemplo de testes de integração

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) é composto de dois aplicativos:

| Aplicativo | Pasta do projeto | Descrição |
| --- | -------------- | ----------- |
| Aplicativo de mensagem (o SUT) | *src/RazorPagesProject* | Permite que um usuário adicionar, excluir um, exclua todas as e analisar as mensagens. |
| Aplicativo de teste | *tests/RazorPagesProject.Tests* | Usado para teste de integração de SUT. |

Os testes podem ser executados usando os recursos internos de teste de um IDE, como [Visual Studio](https://www.visualstudio.com/vs/). Se usar [código do Visual Studio](https://code.visualstudio.com/) ou a linha de comando, execute o seguinte comando no prompt de comando no *tests/RazorPagesProject.Tests* pasta:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organização de aplicativo (SUT) da mensagem

O SUT é um sistema de mensagens do Razor páginas com as seguintes características:

* A página de índice do aplicativo (*Pages/Index.cshtml* e *Pages/Index.cshtml.cs*) fornece uma interface do usuário e a página de métodos de modelo para controlar a adição, exclusão e análise de mensagens (palavras médias por mensagem) .
* Uma mensagem é descrita pelo `Message` classe (*Data/Message.cs*) com duas propriedades: `Id` (chave) e `Text` (mensagem). O `Text` propriedade é necessária e limitada a 200 caracteres.
* As mensagens são armazenadas usando [banco de dados do Entity Framework na memória](/ef/core/providers/in-memory/)&#8224;.
* O aplicativo contém uma camada de acesso de dados (DAL) em sua classe de contexto de banco de dados, `AppDbContext` (*Data/AppDbContext.cs*).
* Se o banco de dados está vazio na inicialização do aplicativo, o repositório de mensagens foi inicializado com três mensagens.
* O aplicativo inclui um `/SecurePage` que pode ser acessado somente por um usuário autenticado.

&#8224;O tópico EF [teste com InMemory](/ef/core/miscellaneous/testing/in-memory), explica como usar um banco de dados na memória para testes com MSTest. Este tópico usa o [xUnit](https://xunit.github.io/) framework de teste. Conceitos de teste e implementações de teste em estruturas de teste diferentes são semelhantes, mas não idêntica.

Embora o aplicativo não usa o [padrão repositório](http://martinfowler.com/eaaCatalog/repository.html) e não é um exemplo efetivação do [padrão de unidade de trabalho (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), páginas Razor dá suporte a esses padrões de desenvolvimento. Para obter mais informações, consulte [criar a camada de persistência de infraestrutura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementando o repositório e padrões de unidade de trabalho em um aplicativo ASP.NET MVC](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), e [controlador de teste lógica de](/aspnet/core/mvc/controllers/testing) (o exemplo implementa o padrão de repositório).

### <a name="test-app-organization"></a>Organização do aplicativo de teste

O aplicativo de teste é um aplicativo de console dentro de *tests/RazorPagesProject.Tests* pasta.

| Pasta do aplicativo de teste | Descrição |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contém métodos de teste para roteamento, acessar uma página segura por um usuário não autenticado e como obter um perfil de usuário do GitHub e verificação de logon de usuário do perfil. |
| *IntegrationTests* | *IndexPageTests.cs* contém os testes de integração para a página de índice usando personalizado `WebApplicationFactory` classe. |
| *Auxiliares/utilitários* | <ul><li>*Utilities.CS* contém o `InitializeDbForTests` método usado para propagar o banco de dados de teste.</li><li>*HtmlHelpers.cs* fornece um método para retornar um AngleSharp `IHtmlDocument` para uso pelos métodos de teste.</li><li>*HttpClientExtensions.cs* fornecem sobrecargas para `SendAsync` para enviar solicitações para o SUT.</li></ul> |

A estrutura de teste é [xUnit](https://xunit.github.io/). Testes de integração são realizadas usando o [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), que inclui o [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Porque o [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pacote é usado para configurar o servidor de teste e host de teste, o `TestHost` e `TestServer` pacotes não exigem referências de pacote direto no arquivo de projeto do aplicativo de teste ou configuração de desenvolvedor do aplicativo de teste.

**O banco de dados para teste de propagação**

Testes de integração geralmente exigem um pequeno conjunto de dados no banco de dados antes da execução do teste. Por exemplo, uma exclusão chamadas de teste para a exclusão de registro do banco de dados, para que o banco de dados deve ter pelo menos um registro para a solicitação de exclusão seja bem-sucedida.

O aplicativo de exemplo sementes de banco de dados com três mensagens em *Utilities.cs* que testes podem usar quando elas são executadas:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Recursos adicionais

* [Testes de unidade](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Testes de unidades de páginas Razor](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Controladores de teste](xref:mvc/controllers/testing)
