---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: API Web ASP.NET 2 de teste de unidade | Microsoft Docs
author: tfitzmac
description: Este guia e o aplicativo demonstram como criar testes de unidade simples para seu aplicativo de API Web 2. Este tutorial mostra como incluir um projeto de teste de unidade...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: da56b38809faf760b7c390eb76ac9c4556d635c6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376168"
---
<a name="unit-testing-aspnet-web-api-2"></a>API Web ASP.NET 2 de teste de unidade
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Este guia e o aplicativo demonstram como criar testes de unidade simples para seu aplicativo de API Web 2. Este tutorial mostra como incluir um projeto de teste de unidade em sua solução e escrever métodos de teste que verificam os valores retornados de um método de controlador.
> 
> Este tutorial presume que você está familiarizado com os conceitos básicos da API Web ASP.NET. Para um tutorial de Introdução, consulte [Introdução ao ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Os testes de unidade neste tópico são intencionalmente limitados a cenários de dados simples. Para cenários mais avançados de dados de teste de unidade, consulte [simulação do Entity Framework quando unidade de teste de API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - API Web 2


## <a name="in-this-topic"></a>Neste tópico

Esse tópico contém as seguintes seções:

- [Pré-requisitos](#prereqs)
- [Baixar o código](#download)
- [Criar aplicativo com o projeto de teste de unidade](#appwithunittest)

    - [Adicionar projeto de teste de unidade ao criar o aplicativo](#whencreate)
    - [Adicionar projeto de teste de unidade para um aplicativo existente](#addtoexisting)
- [Configurar o aplicativo de API Web 2](#setupproject)
- [Instalar os pacotes NuGet no projeto de teste](#testpackages)
- [Criar testes](#tests)
- [Executar testes](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Pré-requisitos

Edição do Visual Studio 2017 Community, Professional ou Enterprise

<a id="download"></a>
## <a name="download-code"></a>Baixar o código

Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). O projeto que pode ser baixado inclui código de teste de unidade para este tópico e para o [simulação do Entity Framework quando API de Web do ASP.NET de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tópico.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Criar aplicativo com o projeto de teste de unidade

Você pode criar um projeto de teste de unidade durante a criação de seu aplicativo ou adicionar um projeto de teste de unidade para um aplicativo existente. Este tutorial mostra os dois métodos para criar um projeto de teste de unidade. Para seguir este tutorial, você pode usar qualquer uma das abordagens.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Adicionar projeto de teste de unidade ao criar o aplicativo

Criar um novo aplicativo de Web do ASP.NET chamado **StoreApp**.

![Criar projeto](unit-testing-with-aspnet-web-api/_static/image1.png)

Nas janelas do novo projeto ASP.NET, selecione o **vazio** modelo e adicionar pastas e os principais referências de API da Web. Selecione o **adicionar testes de unidade** opção. O projeto de teste de unidade é automaticamente denominado **StoreApp.Tests**. Você pode manter esse nome.

![Criar projeto de teste de unidade](unit-testing-with-aspnet-web-api/_static/image2.png)

Depois de criar o aplicativo, você verá que ele contém dois projetos.

![dois projetos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Adicionar projeto de teste de unidade para um aplicativo existente

Se você não criou o projeto de teste de unidade, quando você criou seu aplicativo, você pode adicionar um a qualquer momento. Por exemplo, suponha que você já tiver um aplicativo chamado StoreApp, e você deseja adicionar testes de unidade. Para adicionar um projeto de teste de unidade, sua solução com o botão direito e selecione **Add** e **novo projeto**.

![Adicionar novo projeto à solução](unit-testing-with-aspnet-web-api/_static/image4.png)

Selecione **teste** no painel esquerdo e selecione **projeto de teste de unidade** para o tipo de projeto. Nomeie o projeto **StoreApp.Tests**.

![Adicionar projeto de teste de unidade](unit-testing-with-aspnet-web-api/_static/image5.png)

Você verá o projeto de teste de unidade em sua solução.

No projeto de teste de unidade, adicione uma referência ao projeto original.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurar o aplicativo de API Web 2

Em seu projeto de StoreApp, adicione um arquivo de classe para o **modelos** pasta denominada **Product.cs**. Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compile a solução.

Clique com botão direito na pasta controladores e selecione **Add** e **Novo Item de Scaffold**. Selecione **API Web 2 controlador - vazio**.

![Adicionar novo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

Defina o nome do controlador **SimpleProductController**e clique em **Add**.

![Especifique o controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

Substitua o código existente pelo código a seguir. Para simplificar este exemplo, os dados são armazenados em uma lista em vez de um banco de dados. A lista definida nesta classe representa os dados de produção. Observe que o controlador inclui um construtor que aceita como um parâmetro em uma lista de objetos do produto. Este construtor permite que você passe dados de teste ao teste de unidade. O controlador também inclui duas **async** métodos para ilustrar os métodos assíncronos de teste de unidade. Esses métodos assíncronos foram implementados chamando **fromresult** minimizar o código estranhos, mas normalmente os métodos incluiria as operações com uso intensivo de recursos.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

O método GetProduct retorna uma instância das **IHttpActionResult** interface. IHttpActionResult é um dos novos recursos na API Web 2, e ele simplifica o desenvolvimento de teste de unidade. Classes que implementam a interface IHttpActionResult são encontradas na [Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace. Essas classes representam as respostas possíveis de uma solicitação de ação, e eles correspondem aos códigos de status HTTP.

Compile a solução.

Agora você está pronto para configurar o projeto de teste.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar os pacotes NuGet no projeto de teste

Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp.Tests) não inclui todos os pacotes NuGet instalados. Outros modelos, como o modelo de API Web incluem alguns pacotes do NuGet no projeto de teste de unidade. Para este tutorial, você deve incluir o pacote Microsoft ASP.NET Web API 2 Core ao projeto de teste.

Clique com botão direito no projeto StoreApp.Tests e selecione **gerenciar pacotes NuGet**. Você deve selecionar o projeto StoreApp.Tests para adicionar os pacotes para o projeto.

![Gerenciar pacotes](unit-testing-with-aspnet-web-api/_static/image8.png)

Localizar e instalar o pacote do Microsoft ASP.NET Web API 2 Core.

![instalar o pacote de núcleo de api da web](unit-testing-with-aspnet-web-api/_static/image9.png)

Feche a janela Gerenciar pacotes NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Criar testes

Por padrão, o seu projeto de teste inclui um arquivo de teste vazio chamado Unittest1. Esse arquivo mostra os atributos que você usa para criar métodos de teste. Para testes de unidade, você pode usar esse arquivo ou criar seu próprio arquivo.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Para este tutorial, você criará sua própria classe de teste. Você pode excluir o arquivo UnitTest1.cs. Adicione uma classe chamada **TestSimpleProductController.cs**e substitua o código pelo código a seguir.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Executar testes

Agora você está pronto para executar os testes. Todos o método que são marcados com o **TestMethod** atributo será testado. Dos **teste** item de menu, execute os testes.

![executar testes](unit-testing-with-aspnet-web-api/_static/image11.png)

Abra o **Gerenciador de testes** janela e observe os resultados dos testes.

![resultados de testes](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Resumo

Você concluiu este tutorial. Os dados neste tutorial foi intencionalmente simplificados para focalizar as condições de teste de unidade. Para cenários mais avançados de dados de teste de unidade, consulte [simulação do Entity Framework quando unidade de teste de API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
