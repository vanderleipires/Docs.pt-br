---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Examinando os métodos de edição e exibição de edição (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 9f99981b41d0e67514f5667b00640ad9174d1e4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362661"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Examinando os métodos de edição e exibição de edição (VB)
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
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o c#, alterne para o [c# versão](../cs/examining-the-edit-methods-and-edit-view.md) deste tutorial.


Nesta seção, você examinará os métodos de ação gerado e os modos de exibição para o controlador de filmes. Em seguida, você adicionará uma página de pesquisa personalizada.

Execute o aplicativo e navegue até a `Movies` controlador por meio do acréscimo */Movies* para a URL na barra de endereços do navegador. Mantenha o ponteiro do mouse sobre um **editar** link para ver a URL para o qual ela está vinculada.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

O **edite** link foi gerado pelo `Html.ActionLink` método na *Views\Movies\Index.vbhtml* exibição:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

O `Html` objeto é um auxiliar exposto usando uma propriedade no `WebViewPage` classe base. O `ActionLink` método do auxiliar de torna mais fácil gerar dinamicamente os hiperlinks HTML que vinculam a métodos de ação nos controladores. O primeiro argumento para o `ActionLink` método é para renderizar o texto do link (por exemplo, `<a>Edit Me</a>`). O segundo argumento é o nome do método da ação ser invocado. O argumento final é um [objeto anônimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que gera os dados de rota (nesse caso, a ID de 4).

O link gerado mostrado na imagem anterior é `http://localhost:xxxxx/Movies/Edit/4`. A rota padrão usa o padrão de URL `{controller}/{action}/{id}`. Portanto, o ASP.NET converte `http://localhost:xxxxx/Movies/Edit/4` em uma solicitação para o `Edit` método de ação a `Movies` controlador com o parâmetro `ID` igual a 4.

Você também pode passar parâmetros de método de ação usando uma cadeia de caracteres de consulta. Por exemplo, a URL `http://localhost:xxxxx/Movies/Edit?ID=4` também passa o parâmetro `ID` de 4 para o `Edit` método de ação do `Movies` controlador.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Abra o `Movies` controlador. Os dois `Edit` métodos de ação são mostrados abaixo.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Observe se o segundo método de ação `Edit` é precedido pelo atributo `HttpPost`. Esse atributo especifica essa sobrecarga da `Edit` método pode ser chamado somente para as solicitações POST. Você pode aplicar o `HttpGet` editar o atributo para o primeiro método, mas isso não é necessário porque é o padrão. (Vamos nos referir aos métodos de ação são implicitamente atribuídos a `HttpGet` do atributo como `HttpGet` métodos.)

O `HttpGet` `Edit` método utiliza o parâmetro de ID de filme, pesquisa o filme usando o Entity Framework `Find` método e retorna o filme selecionado para a exibição de edição. Quando o sistema de scaffolding criou a exibição de Edição, ele examinou a classe `Movie` e o código criado para renderizar os elementos `<label>` e `<input>` de cada propriedade da classe. O exemplo a seguir mostra a exibição de edição que foi gerada:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Observe como o modelo de exibição tem um `@ModelType MvcMovie.Models.Movie` instrução na parte superior do arquivo — Isso especifica que a exibição espera que o modelo para o modelo de exibição ser do tipo `Movie`.

O código gerado por scaffolding usa vários *métodos auxiliares* para simplificar a marcação HTML. O [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) auxiliar exibe o nome do campo (&quot;título&quot;, &quot;ReleaseDate&quot;, &quot;gênero&quot;, ou &quot;preço &quot;). O [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) auxiliar exibe um HTML `<input>` elemento. O [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) auxiliar exibe quaisquer mensagens de validação associadas com aquela propriedade.

Execute o aplicativo e navegue até a */Movies* URL. Clique em um link **Editar**. No navegador, exiba a origem da página. O HTML na página é semelhante ao exemplo a seguir. (A marcação de menu foi excluída por motivos de clareza).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

O `<input>` elementos estão em um HTML `<form>` elemento cujo `action` atributo é definido para postar a */Movies/Edit* URL. Os dados de formulário serão postados no servidor quando o **editar** botão é clicado.

## <a name="processing-the-post-request"></a>Processando a solicitação POST

A lista a seguir mostra a versão `HttpPost` do método de ação `Edit`.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

O associador de modelo de estrutura do ASP.NET usa os valores de formulário postados e cria um `Movie` objeto que é passado como o `movie` parâmetro. O `ModelState.IsValid` check-in do código verifica a que os dados enviados no formulário podem ser usados para modificar um `Movie` objeto. Se os dados forem válidos, o código salva os dados de filme para o `Movies` coleção da `MovieDBContext` instância. O código, em seguida, salva os novos dados de filme ao banco de dados chamando o `SaveChanges` método de `MovieDBContext`, que persistir as alterações no banco de dados. Depois de salvar os dados, o código redireciona o usuário para o `Index` método de ação do `MoviesController` classe, que faz com que o filme atualizado para ser exibido na lista de filmes.

Se os valores postados não são válidos, eles são reexibidos no formulário. O `Html.ValidationMessageFor` auxiliares na *Edit.vbhtml* exibição modelo cuidar de exibir mensagens de erro apropriado.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Observação sobre localidades** se você normalmente trabalha com uma localidade diferente do inglês, consulte [que dão suporte a validação ASP.NET MVC 3 com as localidades do inglês.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Tornando o método de edição mais robusto

O `HttpGet` `Edit` método gerado pelo sistema de scaffolding não verifica se a ID que é passada para ele é válida. Se um usuário remover o segmento da ID da URL (`http://localhost:xxxxx/Movies/Edit`), o seguinte erro é exibido:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Um usuário também pode passar uma ID que não existe no banco de dados, tais como `http://localhost:xxxxx/Movies/Edit/1234`. Você pode fazer duas alterações para o `HttpGet` `Edit` método de ação para resolver essa limitação. Primeiro, altere o `ID` parâmetro deverá ter um valor padrão de zero quando uma ID não é passada explicitamente. Você também pode verificar se o `Find` , na verdade, o método encontrado um filme antes de retornar o objeto de filme para o modelo de exibição. Atualizado `Edit` método é mostrado abaixo.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Não se for encontrado nenhum filme, o `HttpNotFound` método é chamado.

Todos os `HttpGet` métodos seguem um padrão semelhante. Obtêm um objeto de filme (ou uma lista de objetos, no caso de `Index`) e passar o modelo para o modo de exibição. O `Create` método passa um objeto de filme vazio para o modo de exibição de criar. Todos os métodos que criam, editam, excluem ou, de outro modo, modificam dados fazem isso na sobrecarga `HttpPost` do método. Modificando dados em um método HTTP GET é um risco de segurança, conforme descrito na entrada de postagem de blog [ASP.NET MVC dica n º 46 – não use Excluir Links porque eles criaram buracos na segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificando dados em um método GET também viola as práticas recomendadas HTTP e o padrão de arquitetura REST, que especifica que as solicitações GET não devem alterar o estado do seu aplicativo. Em outras palavras, executar uma operação GET deve ser uma operação segura que não tem efeitos colaterais.

## <a name="adding-a-search-method-and-search-view"></a>Adicionando um método de pesquisa e a exibição de pesquisa

Nesta seção, você adicionará um `SearchIndex` método de ação que lhe permite pesquisar filmes por gênero ou nome. Isso estará disponível usando o */Movies/SearchIndex* URL. A solicitação será exibido um formulário HTML que contém os elementos de entrada que um usuário pode preencher para pesquisar um filme. Quando um usuário envia o formulário, o método de ação irá obter os valores de pesquisa postados pelo usuário e usar os valores para o banco de dados de pesquisa.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Exibindo o formulário SearchIndex

Comece adicionando um `SearchIndex` método de ação existente `MoviesController` classe. O método retornará uma exibição que contém um formulário HTML. Aqui está o código:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

A primeira linha do `SearchIndex` método cria o seguinte [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consulta para selecionar os filmes:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

A consulta é definida no momento, mas não ainda está sendo executada em relação ao armazenamento de dados.

Se o `searchString` parâmetro contém uma cadeia de caracteres, a consulta de filmes será modificada para filtrar o valor da cadeia de pesquisa, usando o seguinte código:

Caso contrário, em seguida, String.IsNullOrEmpty(searchString)   
 filmes = filmes. Onde (funções s.Title.Contains(searchString))   
 End If

Consultas LINQ não são executadas quando são definidas ou quando eles forem modificados chamando um método como `Where` ou `OrderBy`. Em vez disso, a execução da consulta é adiada, o que significa que a avaliação de uma expressão é atrasada até que seu valor realizado seja iterado, na verdade, ou o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) método é chamado. No `SearchIndex` exemplo, a consulta é executada no modo de exibição SearchIndex. Para obter mais informações sobre a execução de consulta adiada, consulte [Execução da consulta](https://msdn.microsoft.com/library/bb738633.aspx).

Agora você pode implementar o `SearchIndex` modo de exibição que exibirá o formulário ao usuário. Clique com botão direito dentro do `SearchIndex` método e depois clique em **adicionar exibição**. No **adicionar exibição** caixa de diálogo, especifique que você vai passar um `Movie` objeto para o modelo de exibição que sua classe de modelo. No **modelo de Scaffold** , escolha **lista**, em seguida, clique em **adicionar**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Quando você clica o **Add** botão, o *Views\Movies\SearchIndex.vbhtml* modelo de exibição é criado. Como você selecionou **lista** na **modelo Scaffold** listar, Visual Web Developer gerado automaticamente (geradas por scaffolding) algum conteúdo padrão no modo de exibição. O scaffolding criou um formulário HTML. Ele mostrou o `Movie` classe e o código criado para renderizar `<label>` elementos para cada propriedade da classe. Lista a seguir mostra a exibição de criação que foi gerada:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Execute o aplicativo e navegue até */Movies/SearchIndex*. Acrescente uma cadeia de consulta, como `?searchString=ghost`, à URL. Os filmes filtrados são exibidos.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Se você alterar a assinatura do `SearchIndex` método tenha um parâmetro denominado `id`, o `id` parâmetro corresponderá a `{id}` roteia o espaço reservado para o padrão definido no *global. asax* arquivo.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modificado `SearchIndex` método seria semelhante ao seguinte:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Agora você pode passar o título de pesquisa como dados de rota (um segmento de URL), em vez de como um valor de cadeia de consulta.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

No entanto, você não pode esperar que os usuários modifiquem a URL sempre que desejarem pesquisar um filme. Agora você adicionará da interface do usuário para ajudá-los filtrar filmes. Se você tiver alterado a assinatura do `SearchIndex` método para testar como passar o parâmetro de ID associado à rota, alterá-la para que sua `SearchIndex` método utiliza um parâmetro de cadeia de caracteres denominado `searchString`:

Abra o *Views\Movies\SearchIndex.vbhtml* do arquivo e apenas após `@Html.ActionLink("Create New", "Create")`, adicione o seguinte:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

O `Html.BeginForm` auxiliar cria uma abertura `<form>` marca. O `Html.BeginForm` auxiliar faz com que o formulário postar a mesmo quando o usuário envia o formulário clicando o **filtro** botão.

Execute o aplicativo e tente pesquisar um filme.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Não há nenhuma `HttpPost` sobrecarga da `SearchIndex` método. Você não precisa dele, porque o método não está alterando o estado do aplicativo, apenas filtrando os dados. Se você tiver adicionado o seguinte `HttpPost` `SearchIndex` método, o chamador de ação corresponderia a `HttpPost` `SearchIndex` método e o `HttpPost` `SearchIndex` método será executado conforme mostrado na imagem abaixo.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Adicionando uma pesquisa por gênero

Se você tiver adicionado a `HttpPost` versão do `SearchIndex` método, excluí-lo agora.

Em seguida, você adicionará um recurso para permitir que os usuários a pesquisar filmes por gênero. Substitua o método `SearchIndex` pelo seguinte código:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Esta versão dos `SearchIndex` método utiliza um parâmetro adicional, ou seja, `movieGenre`. As primeiras linhas de código é criar um `List` objeto para manter os gêneros de filme do banco de dados.

O código a seguir é uma consulta LINQ que recupera todos os gêneros do banco de dados.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

O código usa o `AddRange` método de genérica `List` coleção para adicionar todos os gêneros distintos à lista. (Sem o `Distinct` modificador, gêneros duplicados seriam adicionados — por exemplo, o comédia seria adicionada duas vezes em nosso exemplo). O código, em seguida, armazena a lista de gêneros no `ViewBag` objeto.

O código a seguir mostra como verificar o `movieGenre` parâmetro. Se não estiver vazio ainda mais o código restringe a consulta de filmes para limitar os filmes selecionados para o gênero especificado.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Adicionando marcação para a exibição de SearchIndex para dar suporte à pesquisa por gênero

Adicionar um `Html.DropDownList` auxiliar para o *Views\Movies\SearchIndex.vbhtml* do arquivo, antes de `TextBox` auxiliar. A marcação concluída é mostrada abaixo:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Execute o aplicativo e navegue até */Movies/SearchIndex*. Tente uma pesquisa por gênero, nome do filme e ambos os critérios.

Nesta seção você examinou os métodos de ação de CRUD e exibições geradas pela estrutura. Você criou um método de ação de pesquisa e uma exibição que permitem aos usuários a pesquisar por título do filme e gênero. Na próxima seção, você verá como adicionar uma propriedade para o `Movie` modelo e como adicionar um inicializador que criará automaticamente um banco de dados de teste.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Próximo](adding-a-new-field.md)
