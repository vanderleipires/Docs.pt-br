---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Acessando dados do seu modelo de um controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6b5a3e15ab5cefeb816d6f1176ce9ee1090761db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833249"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Acessando dados do seu modelo de um controlador (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o c#, alterne para o [c# versão](../cs/accessing-your-models-data-from-a-controller.md) deste tutorial.


Nesta seção, você criará um novo `MoviesController` de classe e escrever um código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição. Certifique-se de criar seu aplicativo antes de continuar.

Clique com botão direito do *controladores* pasta e crie um novo `MoviesController` controlador. Selecione as seguintes opções:

- Nome do controlador: **MoviesController**. (Esse é o padrão).
- Modelo: **controlador com ações de leitura/gravação e modos de exibição, usando o Entity Framework**.
- Classe de modelo: **Movie (mvcmovie. Models)**.
- Classe de contexto de dados: **MovieDBContext (mvcmovie. Models)**.
- Modos de exibição: **Razor (CSHTML)**. (O padrão).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Clique em **Adicionar**. O Visual Web Developer cria os seguintes arquivos e pastas:

- *Um MoviesController.vb* arquivo do projeto *controladores* pasta.
- Um *filmes* pasta do projeto *modos de exibição* pasta.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, e *Index.vbhtml* no novo *exibições \ filmes* pasta.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

O mecanismo de scaffolding do ASP.NET MVC 3 criado automaticamente o CRUD (criar, ler, atualizar e excluir) os métodos de ação e modos de exibição para você. Agora você tem um aplicativo web totalmente funcional que permite que você criar, listar, editar e excluir entradas de filme.

Execute o aplicativo e navegue até a `Movies` controlador por meio do acréscimo */Movies* para a URL na barra de endereços do navegador. Porque o aplicativo é contar com o roteamento padrão (definido na *global. asax* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteado para o padrão `Index` método de ação do `Movies` controlador. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Criação de um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Clicar a **criar** botão faz com que o formulário seja enviado ao servidor, onde as informações do filme é salvo no banco de dados. Em seguida, você será redirecionado para o */Movies* URL, onde você pode ver o filme recém-criado na lista.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinando o código gerado

Abra o *Controllers\MoviesController.vb* do arquivo e examine o gerado `Index` método. Uma parte do controlador de filmes com o `Index` método é mostrado abaixo.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

A seguinte linha do `MoviesController` classe cria uma instância de um contexto de banco de dados de filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Uma solicitação para o `Movies` controlador retorna todas as entradas na `Movies` tabela do banco de dados do filme e, em seguida, passa os resultados para o `Index` modo de exibição.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fortemente tipados e a @model palavra-chave

Neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.

ASP.NET MVC também fornece a capacidade de passar fortemente tipados dados ou objetos para um modelo de exibição. Isso fortemente tipado a abordagem permite um melhor tempo de compilação de seu código e o IntelliSense mais sofisticado no editor do Visual Web Developer. Estamos usando essa abordagem com o `MoviesController` classe e *Index.vbhtml* modelo de exibição.

Observe como o código cria uma [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar no `Index` método de ação. Em seguida, o código passa esta `Movies` lista do controlador para o modo de exibição:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Ao incluir um `@ModelType` instrução na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição. Quando você criou o controlador de filme, o Visual Web Developer incluiu automaticamente a seguinte `@model` instrução na parte superior de *Index.vbhtml* arquivo:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Isso `@ModelType` diretiva permite que você acesse a lista de filmes que o controlador passou para o modo de exibição, usando um `Model` objeto fortemente tipado. Por exemplo, nos *Index.vbhtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução fortemente tipado `Model` objeto:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Porque o `Model` objeto é fortemente tipado (como uma `IEnumerable<Movie>` objeto), cada `item` objeto no loop é tipado como `Movie`. Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Trabalhando com o SQL Server Compact

Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que ainda não existia, portanto, o código primeiro criou o banco de dados automaticamente. Você pode verificar se ele é foi criado examinando os *App\_dados* pasta. Se você não vir as *Movies.sdf* arquivo, clique no **Mostrar todos os arquivos** botão no **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *App\_dados* pasta.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Clique duas vezes em *Movies.sdf* para abrir **Gerenciador de servidores**. Em seguida, expanda o **tabelas** pasta para ver as tabelas que foram criadas no banco de dados.

> [!NOTE]
> Se você receber um erro quando você clica duas vezes *Movies.sdf*, verifique se você instalou **Visual Studio 2010 SP1 Tools para SQL Server Compact 4.0**. (Para obter links para o software, consulte a lista de pré-requisitos na parte 1 desta série de tutoriais). Se você instalar a versão, agora, você terá que fechar e reabrir o Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Há duas tabelas, uma para o `Movie` conjunto de entidades e, em seguida, o `EdmMetadata` tabela. O `EdmMetadata` pelo Entity Framework, a tabela é usada para determinar quando o modelo e o banco de dados estão fora de sincronia.

Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Clique com botão direito do `Movies` de tabela e selecione **Editar esquema da tabela**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base em seu `Movie` classe.

Quando tiver terminado, feche a conexão. (Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Agora você tem o banco de dados e uma página de lista simples para exibir o conteúdo dele. No próximo tutorial, vamos examinar o restante do código gerado por scaffolding e adicionar um `SearchIndex` método e um `SearchIndex` modo de exibição que permite pesquisar filmes neste banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
