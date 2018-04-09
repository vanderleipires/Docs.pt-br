---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acessando dados do modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Acessando dados do modelo de um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Nesta seção, você criará um novo `MoviesController` classe e gravar o código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição.

**Compile o aplicativo** antes de ir para a próxima etapa. Se você não criar o aplicativo, você obterá um erro ao adicionar um controlador.

No Gerenciador de soluções, clique o *controladores* pasta e clique **adicionar**, em seguida, **controlador**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

No **adicionar Scaffold** caixa de diálogo, clique em **controlador MVC 5 com modos de exibição usando o Entity Framework**e, em seguida, clique em **adicionar**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Selecione **filme (MvcMovie.Models)** para a classe de modelo.
- Selecione **MovieDBContext (MvcMovie.Models)** para a classe de contexto de dados.
- Para o nome do controlador, digite **MoviesController**.

  A imagem abaixo mostra a caixa de diálogo concluída.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Clique em **Adicionar**. (Se você receber um erro, você provavelmente não compilar o aplicativo antes de iniciar a adição do controlador.) O Visual Studio cria os seguintes arquivos e pastas:

- *Um MoviesController.cs* arquivo o *controladores* pasta.
- Um *Views\Movies* pasta.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* no novo *Views\Movies* pasta.

Visual Studio criado automaticamente o [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) métodos de ação e o modo de exibição (a criação automática de métodos de ação CRUD e modos de exibição é conhecida como scaffolding). Agora você tem um aplicativo web totalmente funcional que lhe permite criar, listar, editar e excluir entradas do filme.

Execute o aplicativo e clique no **MVC filme** link (ou navegue até o `Movies` controlador acrescentando */Movies* para a URL na barra de endereços do navegador). Porque o aplicativo depender de roteamento padrão (definido no *aplicativo\_Start\RouteConfig.cs* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o padrão `Index` método de ação do `Movies` controlador. Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`. O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Criando um filme

Selecione o link **Criar Novo**. Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Você não poderá inserir pontos decimais ou vírgulas no campo preço. para dar suporte a validação jQuery para idiomas diferentes do inglês que usam uma vírgula (&quot;,&quot;) para um ponto decimal e formatos de data diferente do inglês dos EUA, você deve incluir *globalize.js* e específicos  *cultures/globalize.cultures.js* arquivo (de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. Mostrarei como fazer isso no tutorial Avançar. Por enquanto, insira apenas números inteiros como 10.


Clique o **criar** botão faz com que o formulário seja enviado para o servidor, onde as informações de filme é salvo no banco de dados. Em seguida, você será redirecionado para a */Movies* URL, onde você pode ver o filme recém-criado na lista.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.

## <a name="examining-the-generated-code"></a>Examinar o código gerado

Abra o *Controllers\MoviesController.cs* de arquivo e examine o gerado `Index` método. Uma parte do controlador de filme com o `Index` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Uma solicitação para o `Movies` retorna todas as entradas de `Movies` de tabela e, em seguida, passa os resultados para o `Index` exibição. A seguinte linha da `MoviesController` classe instancia um contexto de banco de dados do filme, conforme descrito anteriormente. Você pode usar o contexto de banco de dados do filme para consultar, editar e excluir filmes.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Fortemente tipado modelos e o @model palavra-chave

Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto. O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.

MVC também fornece a capacidade de passar *fortemente* digitado objetos para um modelo de exibição. Essa abordagem com rigidez de tipos permite melhor tempo de compilação mais rico e verificação do seu código [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) no editor do Visual Studio. O mecanismo de scaffolding no Visual Studio usado essa abordagem (ou seja, passando um *fortemente* com tipo de modelo) com o `MoviesController` modelos de classe e o modo de exibição quando criado os métodos e as exibições.

No *Controllers\MoviesController.cs* arquivo examinar gerado `Details` método. O `Details` método é mostrado abaixo.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

O `id` parâmetro geralmente é passado como dados de rota, por exemplo `http://localhost:1234/movies/details/1` definirá o controlador para o controlador de filme, a ação a ser `details` e `id` como 1. Você também pode transmitir a identificação com uma cadeia de caracteres de consulta da seguinte maneira:

`http://localhost:1234/movies/details?id=1`

Se um `Movie` for encontrado, uma instância do `Movie` modelo é passado para o `Details` exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examine o conteúdo do *Views\Movies\Details.cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Incluindo um `@model` instrução na parte superior do arquivo do modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição. Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado. Por exemplo, o *Details.cshtml* modelo, o código passa cada campo de filme o `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) auxiliares HTML com rigidez de tipos `Model` objeto. O `Create` e `Edit` métodos e modelos de exibição também passam um objeto de modelo do filme.

Examine o *cshtml* modelo de exibição e o `Index` método o *MoviesController.cs* arquivo. Observe como o código cria um [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar a `Index` método de ação. O código, em seguida, passa essa `Movies` lista da `Index` método de ação para o modo de exibição:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Quando você criou o controlador de filme, o Visual Studio incluídos automaticamente o seguinte `@model` instrução na parte superior de *cshtml* arquivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passado para o modo de exibição usando uma `Model` objeto que é fortemente tipado. Por exemplo, no *cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução sobre fortemente tipada `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Porque o `Model` objeto é fortemente tipado (como um `IEnumerable<Movie>` objeto), cada `item` objeto no loop é digitado como `Movie`. Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Trabalhando com o SQL Server LocalDB

Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que não existe, portanto Code First criada a banco de dados automaticamente. Você pode verificar se ela é criada examinando o *aplicativo\_dados* pasta. Se você não vir o *Movies.mdf* de arquivo, clique no **Mostrar todos os arquivos** no botão de **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *aplicativo\_dados* pasta.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Clique duas vezes em *Movies.mdf* abrir **SERVER EXPLORER**, em seguida, expanda o **tabelas** pasta para ver a tabela de filmes. Observe o ícone de chave ao lado de ID. Por padrão, o EF fará uma propriedade chamada ID, a chave primária. Para obter mais informações sobre o EF e MVC, consulte o tutorial excelente de Tom Dykstra em [MVC e EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Clique com botão direito do `Movies` de tabela e selecione **abrir definição de tabela** para ver a tabela de estrutura que Entity Framework Code First criado para você.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente. Entity Framework Code First criado automaticamente esse esquema para você com base na sua `Movie` classe.

Quando tiver terminado, feche a conexão com um clique direito *MovieDBContext* e selecionando **fechar Conexão**. (Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados. O seguinte tutorial, vamos examinar o restante do código scaffolding e adicionar um `SearchIndex` método e uma `SearchIndex` modo de exibição que permite que você pesquise filmes neste banco de dados. Para obter mais informações sobre como usar o Entity Framework com MVC, consulte [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-connection-string.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)
