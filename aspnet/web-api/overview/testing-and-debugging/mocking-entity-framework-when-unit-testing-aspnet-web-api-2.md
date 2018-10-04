---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulação do Entity Framework quando a API Web ASP.NET 2 de teste de unidade | Microsoft Docs
author: tfitzmac
description: Este guia e o aplicativo demonstram como criar testes de unidade para seu aplicativo de API Web 2 que usa o Entity Framework. Ele mostra como modificar o...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8945f913abe8fb8397d07a5994000fff2348f1f7
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795373"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulação do Entity Framework quando a API Web ASP.NET 2 de teste de unidade
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

[Baixe o projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Este guia e o aplicativo demonstram como criar testes de unidade para seu aplicativo de API Web 2 que usa o Entity Framework. Ele mostra como modificar o controlador gerados automaticamente para habilitar passando um objeto de contexto para teste e como criar objetos de teste que funcionam com o Entity Framework.
>
> Para obter uma introdução ao teste de unidade com a API Web ASP.NET, consulte [Unit Testing com o ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
>
> Este tutorial presume que você está familiarizado com os conceitos básicos da API Web ASP.NET. Para um tutorial de Introdução, consulte [Introdução ao ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2

## <a name="in-this-topic"></a>Neste tópico

Esse tópico contém as seguintes seções:

- [Pré-requisitos](#prereqs)
- [Baixar o código](#download)
- [Criar aplicativo com o projeto de teste de unidade](#appwithunittest)
- [Criar a classe de modelo](#modelclass)
- [Adicionar o controlador](#controller)
- [Adicionar a injeção de dependência](#dependency)
- [Instalar os pacotes NuGet no projeto de teste](#testpackages)
- [Criar o contexto do teste](#testcontext)
- [Criar testes](#tests)
- [Executar testes](#runtests)

Se você já tiver concluído as etapas em [Unit Testing com o ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), você pode pular para a seção [adicionar o controlador](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Pré-requisitos

Edição do Visual Studio 2017 Community, Professional ou Enterprise

<a id="download"></a>
## <a name="download-code"></a>Baixar o código

Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). O projeto que pode ser baixado inclui código de teste de unidade para este tópico e para o [unidade de teste de API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) tópico.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Criar aplicativo com o projeto de teste de unidade

Você pode criar um projeto de teste de unidade durante a criação de seu aplicativo ou adicionar um projeto de teste de unidade para um aplicativo existente. Este tutorial mostra como criar um projeto de teste de unidade ao criar o aplicativo.

Criar um novo aplicativo de Web do ASP.NET chamado **StoreApp**.

Nas janelas do novo projeto ASP.NET, selecione o **vazio** modelo e adicionar pastas e os principais referências de API da Web. Selecione o **adicionar testes de unidade** opção. O projeto de teste de unidade é automaticamente denominado **StoreApp.Tests**. Você pode manter esse nome.

![Criar projeto de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Depois de criar o aplicativo, você verá que ele contém dois projetos - **StoreApp** e **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Criar a classe de modelo

Em seu projeto de StoreApp, adicione um arquivo de classe para o **modelos** pasta denominada **Product.cs**. Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compile a solução.

<a id="controller"></a>
## <a name="add-the-controller"></a>Adicionar o controlador

Clique com botão direito na pasta controladores e selecione **Add** e **Novo Item de Scaffold**. Selecione o controlador Web API 2 com ações, usando o Entity Framework.

![Adicionar novo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Defina os seguintes valores:

- Nome do controlador: **ProductController**
- Classe de modelo: **produto**
- Classe de contexto de dados: [selecionar **novo contexto de dados** botão que preenche os valores vistos abaixo]

![Especifique o controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Clique em **adicionar** para criar o controlador com o código gerado automaticamente. O código inclui métodos para criar, recuperar, atualizar e excluir instâncias da classe de produto. O código a seguir mostra o método para adicionar um produto. Observe que o método retorna uma instância do **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult é um dos novos recursos na API Web 2, e ele simplifica o desenvolvimento de teste de unidade.

Na próxima seção, você personalizará o código gerado para facilitar a passagem de objetos de teste para o controlador.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Adicionar a injeção de dependência

Atualmente, a classe ProductController é codificado para usar uma instância da classe StoreAppContext. Você usará um padrão chamado injeção de dependência para modificar seu aplicativo e remover essa dependência embutido em código. Ao dividir essa dependência, você pode passar um objeto fictício durante o teste.

Clique com botão direito do **modelos** pasta e adicione uma nova interface chamada **IStoreAppContext**.

Substitua o código pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Abra o arquivo StoreAppContext.cs e faça as seguintes alterações realçadas. As alterações importantes a observar são:

- Classe StoreAppContext implementa a interface IStoreAppContext
- Método MarkAsModified é implementado


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Abra o arquivo ProductController.cs. Altere o código existente para coincidir com o código realçado. Essas alterações quebrar a dependência no StoreAppContext e habilitar a outras classes transmitir um objeto diferente para a classe de contexto. Essa alteração permitirá que você passar um contexto de teste durante testes de unidade.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Não há mais de uma alteração que você deve fazer no ProductController. No **PutProduct** método, substitua a linha que define o estado da entidade como modificada com uma chamada ao método MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compile a solução.

Agora você está pronto para configurar o projeto de teste.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar os pacotes NuGet no projeto de teste

Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp.Tests) não inclui todos os pacotes NuGet instalados. Outros modelos, como o modelo de API Web incluem alguns pacotes do NuGet no projeto de teste de unidade. Para este tutorial, você deve incluir o packge do Entity Framework e o pacote Microsoft ASP.NET Web API 2 Core ao projeto de teste.

Clique com botão direito no projeto StoreApp.Tests e selecione **gerenciar pacotes NuGet**. Você deve selecionar o projeto StoreApp.Tests para adicionar os pacotes para o projeto.

![Gerenciar pacotes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Dos pacotes Online, localizar e instalar o pacote EntityFramework (versão 6.0 ou posterior). Se parecer que o pacote EntityFramework já está instalado, você talvez tenha selecionado o projeto StoreApp em vez do projeto StoreApp.Tests.

![Adicionar o Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Localizar e instalar o pacote do Microsoft ASP.NET Web API 2 Core.

![instalar o pacote de núcleo de api da web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Feche a janela Gerenciar pacotes NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Criar o contexto do teste

Adicione uma classe chamada **TestDbSet** ao projeto de teste. Esta classe serve como a classe base para seu conjunto de dados de teste. Substitua o código pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Adicione uma classe chamada **TestProductDbSet** ao projeto de teste que contém o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Adicione uma classe chamada **TestStoreAppContext** e substitua o código existente pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Criar testes

Por padrão, o seu projeto de teste inclui um arquivo de teste vazio chamado **UnitTest1.cs**. Esse arquivo mostra os atributos que você usa para criar métodos de teste. Para este tutorial, você pode excluir esse arquivo como você adicionará uma nova classe de teste.

Adicione uma classe chamada **TestProductController** ao projeto de teste. Substitua o código pelo código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Executar testes

Agora você está pronto para executar os testes. Todos o método que são marcados com o **TestMethod** atributo será testado. Dos **teste** item de menu, execute os testes.

![executar testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Abra o **Gerenciador de testes** janela e observe os resultados dos testes.

![resultados de testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
