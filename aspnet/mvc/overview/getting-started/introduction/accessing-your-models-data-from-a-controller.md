---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acessando dados do seu modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120147"
---
<a name="accessing-your-models-data-from-a-controller"></a>Acessando dados do seu modelo de um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Nesta seção, você criará um novo `MoviesController` de classe e escrever um código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição.

**Compilar o aplicativo** antes de ir para a próxima etapa. Se você não criar o aplicativo, você obterá um erro ao adicionar um controlador.

No Gerenciador de soluções, clique com botão direito a *controladores* pasta e clique **Add**, em seguida, **controlador**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

No **adicionar Scaffold** caixa de diálogo, clique em **controlador MVC 5 com modos de exibição usando o Entity Framework**e, em seguida, clique em **adicionar**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Selecione **Movie (mvcmovie. Models)** para a classe de modelo.
- Selecione **MovieDBContext (mvcmovie. Models)** para a classe de contexto de dados.
- Insira o nome do controlador **MoviesController**.

  A imagem abaixo mostra a caixa de diálogo concluída.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Clique em **Adicionar**. (Se você receber um erro, você provavelmente não compila o aplicativo antes de começar a adicionar o controlador.) Visual Studio cria os seguintes arquivos e pastas:

- *Um MoviesController.cs* arquivo o *controladores* pasta.
- Um *exibições \ filmes* pasta.
- *Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*, e *index. cshtml* no novo *exibições \ filmes* pasta.

Visual Studio criado automaticamente a [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) os métodos de ação e modos de exibição para você (a criação automática de métodos de ação CRUD e exibições é conhecida como scaffolding). Agora você tem um aplicativo web totalmente funcional que permite que você criar, listar, editar e excluir entradas de filme.

Execute o aplicativo e clique no **filme MVC** link (ou navegue até a `Movies` controlador por meio do acréscimo */Movies* para a URL na barra de endereços do navegador). Porque o aplicativo é contar com o roteamento padrão (definido na *App\_Start\RouteConfig.cs* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteado para o padrão `Index` método de ação do `Movies` controlador. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Criação de um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Você não poderá inserir pontos decimais ou vírgulas no campo de preço. para dar suporte a validação do jQuery para idiomas diferentes do inglês que usam uma vírgula (&quot;,&quot;) para um ponto decimal e formatos de data do inglês dos EUA, você deve incluir *globalize.js* seu específicas e  *cultures/globalize.cultures.js* arquivo (do [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. Mostrarei como fazer isso no próximo tutorial. Por enquanto, insira apenas números inteiros como 10.


Clicar a **criar** botão faz com que o formulário seja enviado ao servidor, onde as informações do filme é salvo no banco de dados. Em seguida, você será redirecionado para o */Movies* URL, onde você pode ver o filme recém-criado na lista.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinando o código gerado

Abra o *Controllers\MoviesController.cs* do arquivo e examine o gerado `Index` método. Uma parte do controlador de filmes com o `Index` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Uma solicitação para o `Movies` controlador retorna todas as entradas na `Movies` da tabela e, em seguida, passa os resultados para o `Index` modo de exibição. A seguinte linha do `MoviesController` classe cria uma instância de um contexto de banco de dados de filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fortemente tipados e a @model palavra-chave

Neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.

MVC também fornece a capacidade de passar *fortemente* tipada de objetos para um modelo de exibição. Essa abordagem fortemente tipada permite melhor tempo de compilação verificação do seu código e mais rico [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) no editor do Visual Studio. O mecanismo de scaffolding no Visual Studio usou essa abordagem (ou seja, passando um *fortemente* modelo tipado) com o `MoviesController` modelos de classe e o modo de exibição quando ele criou os métodos e os modos de exibição.

No *Controllers\MoviesController.cs* arquivo examinar gerado `Details` método. O `Details` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

O `id` parâmetro geralmente é passado como dados de rota, por exemplo `http://localhost:1234/movies/details/1` definirá o controlador ao controlador de filme, a ação a ser `details` e o `id` como 1. Você também pode passar a id com uma cadeia de caracteres de consulta da seguinte maneira:

`http://localhost:1234/movies/details?id=1`

Se um `Movie` for encontrado, uma instância das `Movie` modelo é passado para o `Details` exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examinar o conteúdo do *Views\Movies\Details.cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Ao incluir um `@model` instrução na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição. Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado. Por exemplo, nos *details. cshtml* modelo, o código passa cada campo de filme para o `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) auxiliares HTML com rigidez de tipos `Model` objeto. O `Create` e `Edit` métodos e modelos de exibição também passam um objeto de modelo de filme.

Examine os *index. cshtml* modelo de exibição e o `Index` método no *MoviesController.cs* arquivo. Observe como o código cria uma [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar no `Index` método de ação. Em seguida, o código passa esta `Movies` lista da `Index` método de ação para o modo de exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Quando você criou o controlador de filme, o Visual Studio incluiu automaticamente a seguinte `@model` instrução na parte superior de *index. cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passou para o modo de exibição, usando um `Model` objeto fortemente tipado. Por exemplo, nos *index. cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução fortemente tipado `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Porque o `Model` objeto é fortemente tipado (como uma `IEnumerable<Movie>` objeto), cada `item` objeto no loop é tipado como `Movie`. Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Trabalhando com o SQL Server LocalDB

Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que ainda não existia, portanto, o código primeiro criou o banco de dados automaticamente. Você pode verificar se ele é foi criado examinando os *App\_dados* pasta. Se você não vir as *Movies.mdf* arquivo, clique no **Mostrar todos os arquivos** botão no **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *App\_dados* pasta.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Clique duas vezes em *Movies.mdf* para abrir **Gerenciador de servidores**, em seguida, expanda o **tabelas** pasta para ver a tabela de filmes. Observe o ícone de chave ao lado de ID. Por padrão, o EF tornará uma propriedade chamada ID de chave primária. Para obter mais informações sobre o EF e ao MVC, consulte o tutorial excelente de Tom Dykstra no [MVC e ao EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Clique com botão direito do `Movies` de tabela e selecione **abrir definição de tabela** para ver a tabela de estrutura que o Entity Framework Code First criado para você.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base em seu `Movie` classe.

Quando tiver terminado, feche a conexão com o botão direito clicando *MovieDBContext* e selecionando **fechar Conexão**. (Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados. No próximo tutorial, vamos examinar o restante do código gerado por scaffolding e adicionar um `SearchIndex` método e um `SearchIndex` modo de exibição que permite pesquisar filmes neste banco de dados. Para obter mais informações sobre como usar o Entity Framework com MVC, consulte [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-connection-string.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
