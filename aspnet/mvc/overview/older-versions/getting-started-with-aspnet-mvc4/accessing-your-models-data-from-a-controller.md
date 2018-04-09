---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Acessando dados do modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Acessando dados do modelo de um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Nesta seção, você criará um novo `MoviesController` classe e gravar o código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição.

**Compile o aplicativo** antes de ir para a próxima etapa.

Clique com botão direito do *controladores* pasta e criar um novo `MoviesController` controlador. As opções a seguir não aparecerá até que você criar seu aplicativo. Selecione as seguintes opções:

- Nome do controlador: **MoviesController**. (Esse é o padrão. )
- Modelo: **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework**.
- Classe de modelo: **filme (MvcMovie.Models)**.
- Classe de contexto de dados: **MovieDBContext (MvcMovie.Models)**.
- Modos de exibição: **Razor (CSHTML)**. (O padrão).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Clique em **Adicionar**. O Visual Studio Express cria os seguintes arquivos e pastas:

- *Um MoviesController.cs* arquivo do projeto *controladores* pasta.
- Um *filmes* pasta do projeto *exibições* pasta.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* no novo *Views\Movies* pasta.

ASP.NET MVC 4 criado automaticamente o CRUD (criar, ler, atualizar e excluir) métodos de ação e o modo de exibição (a criação automática de métodos de ação CRUD e modos de exibição é conhecida como scaffolding). Agora você tem um aplicativo web totalmente funcional que lhe permite criar, listar, editar e excluir entradas do filme.

Execute o aplicativo e navegue até o `Movies` controlador acrescentando */Movies* para a URL na barra de endereços do navegador. Porque o aplicativo depender de roteamento padrão (definido no *global. asax* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o padrão `Index` método de ação do `Movies` controlador. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Criando um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Clique o **criar** botão faz com que o formulário seja enviado para o servidor, onde as informações de filme é salvo no banco de dados. Em seguida, você será redirecionado para a */Movies* URL, onde você pode ver o filme recém-criado na lista.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinar o código gerado

Abra o *Controllers\MoviesController.cs* de arquivo e examine o gerado `Index` método. Uma parte do controlador de filme com o `Index` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

A seguinte linha da `MoviesController` classe instancia um contexto de banco de dados do filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados do filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Uma solicitação para o `Movies` retorna todas as entradas de `Movies` tabela do banco de dados do filme e, em seguida, passa os resultados para o `Index` exibição.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Fortemente tipado modelos e o @model palavra-chave

Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.

ASP.NET MVC também fornece a capacidade de passar fortemente tipado dados ou objetos para um modelo de exibição. Isso fortemente tipado abordagem permite melhor tempo de compilação de seu código e mais rico IntelliSense no editor do Visual Studio. O mecanismo de scaffolding no Visual Studio usado essa abordagem com o `MoviesController` modelos de classe e o modo de exibição quando criado os métodos e as exibições.

No *Controllers\MoviesController.cs* arquivo examinar gerado `Details` método. Uma parte do controlador de filme com o `Details` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Se um `Movie` for encontrado, uma instância do `Movie` modelo é passado para o modo de exibição de detalhes. Examine o conteúdo do *Views\Movies\Details.cshtml* arquivo.

Incluindo um `@model` instrução na parte superior do arquivo do modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição. Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado. Por exemplo, o *Details.cshtml* modelo, o código passa cada campo de filme o `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) auxiliares HTML com rigidez de tipos `Model` objeto. Os métodos de criar e editar e exibir modelos também passam um objeto de modelo do filme.

Examine o *cshtml* modelo de exibição e o `Index` método o *MoviesController.cs* arquivo. Observe como o código cria um [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar a `Index` método de ação. O código, em seguida, passa essa `Movies` lista do controlador para o modo de exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Quando você criou o controlador de filme, Visual Studio Express incluído automaticamente o seguinte `@model` instrução na parte superior do *cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passado para o modo de exibição usando uma `Model` objeto que é fortemente tipado. Por exemplo, no *cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução sobre fortemente tipada `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Porque o `Model` objeto é fortemente tipado (como um `IEnumerable<Movie>` objeto), cada `item` objeto no loop é digitado como `Movie`. Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Trabalhando com o SQL Server LocalDB

Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que não existe, portanto Code First criada a banco de dados automaticamente. Você pode verificar se ela é criada examinando o *aplicativo\_dados* pasta. Se você não vir o *Movies.mdf* de arquivo, clique no **Mostrar todos os arquivos** no botão de **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *aplicativo\_dados* pasta.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Clique duas vezes em *Movies.mdf* abrir **Pesquisador de objetos de banco de dados**, em seguida, expanda o **tabelas** pasta para ver a tabela de filmes.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Se o Gerenciador de banco de dados não aparecer, do **ferramentas** menu, selecione **conectar-se ao banco de dados**, cancelar o **Escolher fonte de dados** caixa de diálogo. Isso forçará abrir o Gerenciador de banco de dados.


> [!NOTE]
> Se você estiver usando o Visual Studio 2010 ou o VWD e receber um erro semelhante a qualquer um dos seguintes procedimentos:
> 
> - O banco de dados ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' não pode ser aberto porque sua versão 706 é. Este servidor suporta a versão 655 e versões anteriores. Não há suporte para um caminho de downgrade.
> - &quot;Exceção InvalidOperation não foi manipulada pelo código do usuário&quot; a SqlConnection fornecida não especifica um catálogo inicial.
> 
> Você precisa instalar o [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Verifique se o `MovieDBContext` cadeia de caracteres de conexão especificada na página anterior.


Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Clique com botão direito do `Movies` de tabela e selecione **abrir definição de tabela** para ver a tabela de estrutura que Entity Framework Code First criado para você.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base na sua `Movie` classe.

Quando tiver terminado, feche a conexão com um clique direito *MovieDBContext* e selecionando **fechar Conexão**. (Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Agora você tem o banco de dados e uma página de lista simples para exibir o conteúdo dele. O seguinte tutorial, vamos examinar o restante do código scaffolding e adicionar um `SearchIndex` método e uma `SearchIndex` modo de exibição que permite que você pesquise filmes neste banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
