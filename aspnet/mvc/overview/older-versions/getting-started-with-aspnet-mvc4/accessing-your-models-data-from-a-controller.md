---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Acessando dados do seu modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2b5747f49a31a6f3559609bbae765025e43c650b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824155"
---
<a name="accessing-your-models-data-from-a-controller"></a>Acessando dados do seu modelo de um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.


Nesta seção, você criará um novo `MoviesController` de classe e escrever um código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição.

**Compilar o aplicativo** antes de ir para a próxima etapa.

Clique com botão direito do *controladores* pasta e crie um novo `MoviesController` controlador. As opções a seguir não aparecerá até que você criar seu aplicativo. Selecione as seguintes opções:

- Nome do controlador: **MoviesController**. (Esse é o padrão. )
- Modelo: **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework**.
- Classe de modelo: **Movie (mvcmovie. Models)**.
- Classe de contexto de dados: **MovieDBContext (mvcmovie. Models)**.
- Modos de exibição: **Razor (CSHTML)**. (O padrão).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Clique em **Adicionar**. Visual Studio Express cria os seguintes arquivos e pastas:

- *Um MoviesController.cs* arquivo do projeto *controladores* pasta.
- Um *filmes* pasta do projeto *modos de exibição* pasta.
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*, e *index. cshtml* no novo *exibições \ filmes* pasta.

ASP.NET MVC 4 criado automaticamente o CRUD (criar, ler, atualizar e excluir) os métodos de ação e modos de exibição para você (a criação automática de métodos de ação CRUD e exibições é conhecida como scaffolding). Agora você tem um aplicativo web totalmente funcional que permite que você criar, listar, editar e excluir entradas de filme.

Execute o aplicativo e navegue até a `Movies` controlador por meio do acréscimo */Movies* para a URL na barra de endereços do navegador. Porque o aplicativo é contar com o roteamento padrão (definido na *global. asax* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteado para o padrão `Index` método de ação do `Movies` controlador. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Criação de um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Clicar a **criar** botão faz com que o formulário seja enviado ao servidor, onde as informações do filme é salvo no banco de dados. Em seguida, você será redirecionado para o */Movies* URL, onde você pode ver o filme recém-criado na lista.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinando o código gerado

Abra o *Controllers\MoviesController.cs* do arquivo e examine o gerado `Index` método. Uma parte do controlador de filmes com o `Index` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

A seguinte linha do `MoviesController` classe cria uma instância de um contexto de banco de dados de filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Uma solicitação para o `Movies` controlador retorna todas as entradas na `Movies` tabela do banco de dados do filme e, em seguida, passa os resultados para o `Index` modo de exibição.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fortemente tipados e a @model palavra-chave

Neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.

ASP.NET MVC também fornece a capacidade de passar fortemente tipados dados ou objetos para um modelo de exibição. Isso fortemente tipado a abordagem permite um melhor tempo de compilação de seu código e o IntelliSense mais sofisticado no editor do Visual Studio. O mecanismo de scaffolding no Visual Studio usou essa abordagem com o `MoviesController` modelos de classe e o modo de exibição quando ele criou os métodos e os modos de exibição.

No *Controllers\MoviesController.cs* arquivo examinar gerado `Details` método. Uma parte do controlador de filmes com o `Details` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Se um `Movie` for encontrado, uma instância da `Movie` modelo é passado para o modo de exibição de detalhes. Examinar o conteúdo do *Views\Movies\Details.cshtml* arquivo.

Ao incluir um `@model` instrução na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição. Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado. Por exemplo, nos *details. cshtml* modelo, o código passa cada campo de filme para o `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) auxiliares HTML com rigidez de tipos `Model` objeto. Os modelos de exibição e criar e editar métodos também passam um objeto de modelo de filme.

Examine os *index. cshtml* modelo de exibição e o `Index` método no *MoviesController.cs* arquivo. Observe como o código cria uma [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar no `Index` método de ação. Em seguida, o código passa esta `Movies` lista do controlador para o modo de exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Quando você criou o controlador movie, Visual Studio Express incluiu automaticamente a seguinte `@model` instrução na parte superior de *index. cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passou para o modo de exibição, usando um `Model` objeto fortemente tipado. Por exemplo, nos *index. cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução fortemente tipado `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Porque o `Model` objeto é fortemente tipado (como uma `IEnumerable<Movie>` objeto), cada `item` objeto no loop é tipado como `Movie`. Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Trabalhando com o SQL Server LocalDB

Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que ainda não existia, portanto, o código primeiro criou o banco de dados automaticamente. Você pode verificar se ele é foi criado examinando os *App\_dados* pasta. Se você não vir as *Movies.mdf* arquivo, clique no **Mostrar todos os arquivos** botão no **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *App\_dados* pasta.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Clique duas vezes em *Movies.mdf* para abrir **DATABASE EXPLORER**, em seguida, expanda o **tabelas** pasta para ver a tabela de filmes.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Se o Gerenciador de banco de dados não aparecer, do **ferramentas** menu, selecione **conectar-se ao banco de dados**, cancelar, em seguida, o **Escolher fonte de dados** caixa de diálogo. Isso forçará abrir o Gerenciador de banco de dados.


> [!NOTE]
> Se você estiver usando VWD ou o Visual Studio 2010 e receber um erro semelhante a qualquer um dos seguintes procedimentos:
> 
> - O banco de dados ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. Arquivos MDF' não pode ser aberto porque sua versão 706 é. Este servidor suporta a versão 655 e versões anteriores. Não há suporte para um caminho de downgrade.
> - &quot;Exceção InvalidOperation não foi tratada pelo código do usuário&quot; o SqlConnection fornecido não especifica um catálogo inicial.
> 
> Você precisa instalar o [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Verifique se o `MovieDBContext` cadeia de caracteres de conexão especificada na página anterior.


Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Clique com botão direito do `Movies` de tabela e selecione **abrir definição de tabela** para ver a tabela de estrutura que o Entity Framework Code First criado para você.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base em seu `Movie` classe.

Quando tiver terminado, feche a conexão com o botão direito clicando *MovieDBContext* e selecionando **fechar Conexão**. (Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Agora você tem o banco de dados e uma página de lista simples para exibir o conteúdo dele. No próximo tutorial, vamos examinar o restante do código gerado por scaffolding e adicionar um `SearchIndex` método e um `SearchIndex` modo de exibição que permite pesquisar filmes neste banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
