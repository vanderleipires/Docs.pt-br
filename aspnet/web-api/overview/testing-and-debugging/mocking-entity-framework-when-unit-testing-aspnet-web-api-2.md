---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulação do Entity Framework quando ASP.NET Web API 2 de teste de unidade | Microsoft Docs
author: tfitzmac
description: Este guia e o aplicativo demonstram como criar testes de unidade para seu aplicativo de API Web 2 que usa o Entity Framework. Ele mostra como modificar o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152859"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulação do Entity Framework quando ASP.NET Web API 2 de teste de unidade
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Este guia e o aplicativo demonstram como criar testes de unidade para seu aplicativo de API Web 2 que usa o Entity Framework. Ele mostra como modificar o controlador scaffolding para habilitar passando um objeto de contexto para teste e como criar objetos de teste que funcionam com o Entity Framework.
> 
> Para obter uma introdução ao teste de unidade com a API da Web ASP.NET, consulte [testes de unidade com o ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> Este tutorial pressupõe que você esteja familiarizado com os conceitos básicos da API Web do ASP.NET. Para um tutorial de Introdução, consulte [guia de Introdução ao ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>Neste tópico

Esse tópico contém as seguintes seções:

- [Pré-requisitos](#prereqs)
- [Baixar o código](#download)
- [Criar aplicativo com o projeto de teste de unidade](#appwithunittest)
- [Criar a classe de modelo](#modelclass)
- [Adicionar o controlador](#controller)
- [Adicionar a injeção de dependência](#dependency)
- [Instalar os pacotes do NuGet no projeto de teste](#testpackages)
- [Criar o contexto de teste](#testcontext)
- [Criar testes](#tests)
- [Executar testes](#runtests)

Se você já tiver concluído as etapas em [testes de unidade com o ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), você pode ignorar a seção [adicionar o controlador](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Pré-requisitos

Edição do Visual Studio 2017 Community, Professional ou Enterprise

<a id="download"></a>
## <a name="download-code"></a>Baixar o código

Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). O projeto para download inclui o código de teste de unidade para este tópico e o [unidade de teste ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tópico.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Criar aplicativo com o projeto de teste de unidade

Você pode criar um projeto de teste de unidade durante a criação de seu aplicativo ou adicionar um projeto de teste de unidade para um aplicativo existente. Este tutorial mostra como criar um projeto de teste de unidade ao criar o aplicativo.

Criar um novo aplicativo de Web do ASP.NET chamado **StoreApp**.

Nas janelas do novo projeto ASP.NET, selecione o **vazio** modelo e adicionar pastas e referências de núcleo para a API da Web. Selecione o **adicionar testes de unidade** opção. O projeto de teste de unidade é automaticamente denominado **StoreApp.Tests**. Você pode manter esse nome.

![Criar projeto de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Depois de criar o aplicativo, você verá que ele contém dois projetos - **StoreApp** e **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Criar a classe de modelo

No seu projeto de StoreApp, adicione um arquivo de classe para o **modelos** pasta denominada **Product.cs**. Substitua o conteúdo do arquivo com o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compile a solução.

<a id="controller"></a>
## <a name="add-the-controller"></a>Adicionar o controlador

Clique na pasta controladores e selecione **adicionar** e **Novo Item de Scaffold**. Selecione o controlador do Web API 2 com ações, usando o Entity Framework.

![Adicionar novo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Defina os seguintes valores:

- Nome do controlador: **ProductController**
- Classe de modelo: **produto**
- Classe de contexto de dados: [Selecione **novo contexto de dados** botão que preenche os valores mostrados abaixo]

![Especifique o controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Clique em **adicionar** para criar o controlador com o código gerado automaticamente. O código inclui métodos para criar, recuperar, atualizar e excluir instâncias da classe do produto. O código a seguir mostra o método para adicionar um produto. Observe que o método retorna uma instância de **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult é um dos novos recursos no API Web 2, e ele simplifica o desenvolvimento de teste de unidade.

Na próxima seção, você personalizará o código gerado para facilitar a passar objetos de teste para o controlador.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Adicionar a injeção de dependência

Atualmente, a classe ProductController é codificado para usar uma instância da classe StoreAppContext. Você usará um padrão chamado injeção de dependência para modificar seu aplicativo e remover essa dependência embutido. Dividindo essa dependência, você pode passar um objeto de simulação durante o teste.

Clique com botão direito do **modelos** pasta e adicione uma nova interface chamada **IStoreAppContext**.

Substitua o código com o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Abra o arquivo StoreAppContext.cs e faça as seguintes alterações realçadas. Observe as alterações importantes são:

- Classe StoreAppContext implementa a interface IStoreAppContext
- Método MarkAsModified está implementado


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Abra o arquivo ProductController.cs. Altere o código existente para corresponder o código realçado. Essas alterações quebrar a dependência no StoreAppContext e habilitar outras classes passar um objeto diferente para a classe de contexto. Essa alteração permite que você passe em um contexto de teste durante testes de unidade.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Há uma alteração de mais, que você deve fazer em ProductController. No **PutProduct** método, substitua a linha que define o estado da entidade para modificado com uma chamada ao método MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compile a solução.

Agora você está pronto para configurar o projeto de teste.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar os pacotes do NuGet no projeto de teste

Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp.Tests) não inclui todos os pacotes NuGet instalados. Outros modelos, como o modelo de API da Web, incluem alguns pacotes do NuGet no projeto de teste de unidade. Para este tutorial, você deve incluir o packge do Entity Framework e o pacote do Microsoft ASP.NET Web API 2 Core para o projeto de teste.

O projeto StoreApp.Tests e selecione **gerenciar pacotes NuGet**. Você deve selecionar o projeto StoreApp.Tests para adicionar os pacotes ao projeto.

![Gerenciar pacotes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Dos pacotes Online, localizar e instalar o pacote de EntityFramework (versão 6.0 ou posterior). Se parecer que o pacote EntityFramework já está instalado, você pode selecionou o projeto StoreApp em vez do projeto StoreApp.Tests.

![Adicionar o Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Localizar e instalar o pacote do Microsoft ASP.NET Web API 2 Core.

![instalar o pacote de núcleo de api da web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Feche a janela Gerenciar pacotes NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Criar o contexto de teste

Adicione uma classe denominada **TestDbSet** para o projeto de teste. Esta classe serve como a classe base para o conjunto de dados de teste. Substitua o código com o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Adicione uma classe denominada **TestProductDbSet** ao projeto de teste que contém o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Adicione uma classe denominada **TestStoreAppContext** e substitua o código existente com o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Criar testes

Por padrão, o projeto de teste inclui um arquivo de teste vazio chamado **UnitTest1.cs**. Esse arquivo mostra os atributos que você usa para criar métodos de teste. Para este tutorial, você pode excluir esse arquivo porque você irá adicionar uma nova classe de teste.

Adicione uma classe denominada **TestProductController** para o projeto de teste. Substitua o código com o código a seguir.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Executar testes

Agora você está pronto para executar os testes. Todo o método que são marcados com o **TestMethod** atributo será testado. Do **teste** item de menu, execute os testes.

![executar testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Abra o **Gerenciador de testes** janela e observe os resultados dos testes.

![resultados de testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
