---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Acessando dados do modelo de um controlador (c#) | Microsoft Docs
author: Rick-Anderson
description: "Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5ee29dbc5b4566273592041d94458104e6e0f65e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Acessando dados do modelo de um controlador (c#)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.
> 
> 
> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.

Nesta seção, você criará um novo `MoviesController` classe e gravar o código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição. Certifique-se de criar seu aplicativo antes de continuar.

Clique com botão direito do *controladores* pasta e criar um novo `MoviesController` controlador. Selecione as seguintes opções:

- Nome do controlador: **MoviesController**. (Esse é o padrão. )
- Modelo: **controlador com ações de leitura/gravação e modos de exibição, usando o Entity Framework**.
- Classe de modelo: **filme (MvcMovie.Models)**.
- Classe de contexto de dados: **MovieDBContext (MvcMovie.Models)**.
- Modos de exibição: **Razor (CSHTML)**. (O padrão).

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Clique em **Adicionar**. O Visual Web Developer cria os seguintes arquivos e pastas:

- *Um MoviesController.cs* arquivo do projeto *controladores* pasta.
- Um *filmes* pasta do projeto *exibições* pasta.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* no novo *Views\Movies* pasta.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

O mecanismo do ASP.NET MVC 3 scaffolding criado automaticamente o CRUD (criar, ler, atualizar e excluir) métodos de ação e o modo de exibição. Agora você tem um aplicativo web totalmente funcional que lhe permite criar, listar, editar e excluir entradas do filme.

Execute o aplicativo e navegue até o `Movies` controlador acrescentando */Movies* para a URL na barra de endereços do navegador. Porque o aplicativo depender de roteamento padrão (definido no *global. asax* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o padrão `Index` método de ação do `Movies` controlador. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Criando um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Clique o **criar** botão faz com que o formulário seja enviado para o servidor, onde as informações de filme é salvo no banco de dados. Em seguida, você será redirecionado para a */Movies* URL, onde você pode ver o filme recém-criado na lista.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinar o código gerado

Abra o *Controllers\MoviesController.cs* de arquivo e examine o gerado `Index` método. Uma parte do controlador de filme com o `Index` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

A seguinte linha da `MoviesController` classe instancia um contexto de banco de dados do filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados do filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Uma solicitação para o `Movies` retorna todas as entradas de `Movies` tabela do banco de dados do filme e, em seguida, passa os resultados para o `Index` exibição.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Fortemente tipado modelos e o @model palavra-chave

Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.

ASP.NET MVC também fornece a capacidade de passar fortemente tipado dados ou objetos para um modelo de exibição. Isso fortemente tipado abordagem permite melhor tempo de compilação de seu código e mais rico IntelliSense no editor do Visual Web Developer. Estamos usando essa abordagem com o `MoviesController` classe e *cshtml* modelo de exibição.

Observe como o código cria um [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar a `Index` método de ação. O código, em seguida, passa essa `Movies` lista do controlador para o modo de exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Incluindo um `@model` instrução na parte superior do arquivo do modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição. Quando você criou o controlador de filme, o Visual Web Developer incluído automaticamente o seguinte `@model` instrução na parte superior do *cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passado para o modo de exibição usando uma `Model` objeto que é fortemente tipado. Por exemplo, no *cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução sobre fortemente tipada `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Porque o `Model` objeto é fortemente tipado (como um `IEnumerable<Movie>` objeto), cada `item` objeto no loop é digitado como `Movie`. Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Trabalhando com o SQL Server Compact

Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que não existe, portanto Code First criada a banco de dados automaticamente. Você pode verificar se ela é criada examinando o *aplicativo\_dados* pasta. Se você não vir o *Movies.sdf* de arquivo, clique no **Mostrar todos os arquivos** no botão de **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *aplicativo\_dados* pasta.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Clique duas vezes em *Movies.sdf* abrir **Server Explorer**. Em seguida, expanda o **tabelas** pasta para ver as tabelas que foram criadas no banco de dados.

> [!NOTE]
> Se você receber um erro quando você clicar duas vezes *Movies.sdf*, certifique-se de que você instalou [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam). (Para links para o software, consulte a lista de pré-requisitos na parte 1 Esta série de tutoriais). Se você instalar a versão agora, você terá que fechar e reabrir o Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Há duas tabelas, uma para o `Movie` conjunto de entidades e, em seguida, o `EdmMetadata` tabela. O `EdmMetadata` tabela é usada pelo Entity Framework para determinar quando o modelo e o banco de dados estão fora de sincronia.

Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Clique com botão direito do `Movies` de tabela e selecione **editar o esquema de tabela**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base na sua `Movie` classe.

Quando tiver terminado, feche a conexão. (Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Agora você tem o banco de dados e uma página de lista simples para exibir o conteúdo dele. O seguinte tutorial, vamos examinar o restante do código scaffolding e adicionar um `SearchIndex` método e uma `SearchIndex` modo de exibição que permite que você pesquise filmes neste banco de dados.

>[!div class="step-by-step"]
[Anterior](adding-a-model.md)
[Próximo](examining-the-edit-methods-and-edit-view.md)
