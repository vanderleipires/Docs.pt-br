---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: "Examinando os métodos de edição e exibição de edição (c#) | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: b80332487e52930f3a75973f714d2532068ed012
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Examinando os métodos de edição e exibição de edição (c#)
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


Nesta seção, você examinará os métodos de ação gerado e modos de exibição para o controlador do filme. Em seguida, você adicionará uma página de pesquisa personalizada.

Execute o aplicativo e navegue até o `Movies` controlador acrescentando */Movies* para a URL na barra de endereços do navegador. Mantenha o ponteiro do mouse sobre um **editar** link para ver a URL que ela está vinculada.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

O **editar** link foi gerado pelo `Html.ActionLink` método o *Views\Movies\Index.cshtml* exibição:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

O `Html` objeto é um auxiliar que é exposto usando uma propriedade no `WebViewPage` classe base. O `ActionLink` método do auxiliar facilita gerar dinamicamente os hiperlinks HTML com links para os métodos de ação em controladores. O primeiro argumento para o `ActionLink` método é o texto do link para renderizar (por exemplo, `<a>Edit Me</a>`). O segundo argumento é o nome do método de ação ser invocado. O argumento final é um [objeto anônimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que gera os dados de rota (nesse caso, a ID de 4).

O link gerado mostrado na imagem anterior é `http://localhost:xxxxx/Movies/Edit/4`. A rota padrão usa o padrão de URL `{controller}/{action}/{id}`. Portanto, o ASP.NET converte `http://localhost:xxxxx/Movies/Edit/4` em uma solicitação para o `Edit` método de ação a `Movies` controlador com o parâmetro `ID` igual a 4.

Você também pode passar parâmetros de método de ação usando uma cadeia de caracteres de consulta. Por exemplo, a URL `http://localhost:xxxxx/Movies/Edit?ID=4` também passa o parâmetro `ID` de 4 para o `Edit` método de ação do `Movies` controlador.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Abra o `Movies` controlador. Os dois `Edit` métodos de ação são mostrados abaixo.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Observe se o segundo método de ação `Edit` é precedido pelo atributo `HttpPost`. Esse atributo especifica que que sobrecarga do `Edit` método pode ser chamado somente para solicitações POST. Você pode aplicar o `HttpGet` editar o atributo para o primeiro método, mas que não é necessário porque é o padrão. (Vamos nos referir a métodos de ação que recebem implicitamente a `HttpGet` como `HttpGet` métodos.)

O `HttpGet` `Edit` método usa o parâmetro de ID de filme, pesquise o filme usando o Entity Framework `Find` método e retorna o filme selecionado para o modo de exibição de edição. Quando o sistema de scaffolding criou a exibição de Edição, ele examinou a classe `Movie` e o código criado para renderizar os elementos `<label>` e `<input>` de cada propriedade da classe. O exemplo a seguir mostra a exibição de edição que foi gerada:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Observe como o modelo de exibição tem um `@model MvcMovie.Models.Movie` instrução na parte superior do arquivo – Especifica que o modo de exibição espera que o modelo para o modelo de exibição ser do tipo `Movie`.

O código de scaffolding usa várias *métodos auxiliares* para simplificar a marcação HTML. O [ `Html.LabelFor` ](https://msdn.microsoft.com/en-us/library/gg401864(VS.98).aspx) auxiliar exibe o nome do campo ("Title", "ReleaseDate", "Gênero" ou "Price"). O [ `Html.EditorFor` ](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) auxiliar exibe um HTML `<input>` elemento. O [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) auxiliar exibe quaisquer mensagens de validação associadas a essa propriedade.

Execute o aplicativo e navegue até o */Movies* URL. Clique em um link **Editar**. No navegador, exiba a origem da página. O HTML na página se parece com o exemplo a seguir. (A marcação de menu foi excluída para maior clareza).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

O `<input>` elementos estão em um HTML `<form>` elemento cujo `action` atributo está definido para enviar para o *filmes/editar* URL. Os dados do formulário serão lançados com o servidor quando o **editar** botão é clicado.

## <a name="processing-the-post-request"></a>Processando a solicitação POST

A lista a seguir mostra a versão `HttpPost` do método de ação `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

O associador de modelo de estrutura do ASP.NET usa os valores de formulário postado e cria um `Movie` objeto que é passado como o `movie` parâmetro. O `ModelState.IsValid` check-in do código verifica que os dados enviados do formulário podem ser usados para modificar um `Movie` objeto. Se os dados forem válidos, o código salva os dados do filme para o `Movies` coleção da `MovieDBContext` instância. O código, em seguida, salva os novos dados do filme ao banco de dados chamando o `SaveChanges` método `MovieDBContext`, que persiste as alterações no banco de dados. Depois de salvar os dados, o código redireciona o usuário para o `Index` método de ação de `MoviesController` classe, que faz com que o filme atualizado a ser exibido na lista de filmes.

Se os valores postados não são válidos, eles são reexibidos no formulário. O `Html.ValidationMessageFor` auxiliares no *Edit.cshtml* exibição modelo cuidam da exibição de mensagens de erro apropriado.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Observação sobre localidades** se você normalmente trabalha com uma localidade diferente do inglês, consulte [suporte validação ASP.NET MVC 3 com Non-English localidades.](https://msdn.microsoft.com/en-us/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Tornando o método de edição mais robusta

O `HttpGet` `Edit` método gerado pelo sistema scaffolding não verifica se a ID que é passada para ele é válida. Se um usuário remove o segmento de ID da URL (`http://localhost:xxxxx/Movies/Edit`), o seguinte erro é exibido:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Um usuário também pode passar uma ID que não existe no banco de dados, como `http://localhost:xxxxx/Movies/Edit/1234`. Você pode fazer duas alterações para o `HttpGet` `Edit` método de ação para resolver essa limitação. Primeiro, altere o `ID` parâmetro tenha um valor padrão de zero quando uma ID não é passada explicitamente. Você também pode verificar se o `Find` método encontrado, na verdade, um filme antes de retornar o objeto de filme para o modelo de exibição. A atualização `Edit` método é mostrado abaixo.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Se nenhum filme for encontrado, o `HttpNotFound` método é chamado.

Todos os `HttpGet` métodos seguem um padrão semelhante. Eles obtêm um objeto de filme (ou lista de objetos, no caso de `Index`) e passar o modelo para o modo de exibição. O `Create` método passa um objeto de filme vazio para o modo de exibição de criar. Todos os métodos que criam, editam, excluem ou, de outro modo, modificam dados fazem isso na sobrecarga `HttpPost` do método. Modificando dados em um método HTTP GET é um risco de segurança, conforme descrito na entrada de postagem de blog [ASP.NET MVC dica 46 – não use Excluir Links porque eles criar vulnerabilidades de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificando dados em um método GET também viola as práticas recomendadas HTTP e o padrão de arquitetura REST, que especifica que as solicitações GET não devem alterar o estado do seu aplicativo. Em outras palavras, executar uma operação GET deve ser uma operação de segurança que não tem efeitos colaterais.

## <a name="adding-a-search-method-and-search-view"></a>Adicionando um método de pesquisa e a exibição de pesquisa

Nesta seção, você adicionará um `SearchIndex` método de ação que lhe permite pesquisar filmes por gênero ou nome. Isso estará disponível com o *filmes/SearchIndex* URL. A solicitação exibirá um formulário HTML que contém os elementos de entrada que um usuário pode preencher para procurar um filme. Quando um usuário envia o formulário, o método de ação irá obter os valores de pesquisa lançados pelo usuário e usar os valores para pesquisar o banco de dados.

## <a name="displaying-the-searchindex-form"></a>Exibindo o formulário SearchIndex

Comece adicionando um `SearchIndex` método de ação existente `MoviesController` classe. O método retornará uma exibição que contém um formulário HTML. Aqui está o código:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

A primeira linha do `SearchIndex` método cria a seguinte [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) consulta para selecionar os filmes:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

A consulta está definida neste ponto, mas ainda não foi executada em relação ao armazenamento de dados.

Se o `searchString` parâmetro contém uma cadeia de caracteres, a consulta de filmes é modificada para filtrar o valor da cadeia de pesquisa, usando o seguinte código:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Consultas LINQ não são executadas quando elas são definidas ou quando eles são modificados chamando um método como `Where` ou `OrderBy`. Em vez disso, a execução da consulta é adiada, o que significa que a avaliação de uma expressão é atrasada até que seu valor realizada na verdade é iterada ou [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) método é chamado. No `SearchIndex` exemplo, a consulta é executada no modo de exibição SearchIndex. Para obter mais informações sobre a execução de consulta adiada, consulte [Execução da consulta](https://msdn.microsoft.com/en-us/library/bb738633.aspx).

Agora você pode implementar o `SearchIndex` modo de exibição que exibirá o formulário para o usuário. Clique dentro do `SearchIndex` método e depois clique em **adicionar exibição**. No **adicionar exibição** caixa de diálogo, especifique que você pretende passar um `Movie` objeto para o modelo de exibição como sua classe de modelo. No **modelo Scaffold** , escolha **lista**, em seguida, clique em **adicionar**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Quando você clica o **adicionar** botão, o *Views\Movies\SearchIndex.cshtml* exibir modelo é criado. Como você selecionou **lista** no **modelo Scaffold** lista, o Visual Web Developer gerados automaticamente (Scaffold) algum conteúdo padrão no modo de exibição. O scaffolding criou um formulário HTML. Ele examinou o `Movie` classe e o código criado para renderizar `<label>` elementos para cada propriedade da classe. Lista a seguir mostra o modo de criação que foi gerado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Execute o aplicativo e navegue até *filmes/SearchIndex*. Acrescente uma cadeia de consulta, como `?searchString=ghost`, à URL. Os filmes filtrados são exibidos.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Se você alterar a assinatura do `SearchIndex` método tem um parâmetro chamado `id`, o `id` parâmetro corresponderá a `{id}` roteia o espaço reservado para o padrão definido no *global. asax* arquivo.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

A modificação `SearchIndex` método seria semelhante ao seguinte:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

Agora você pode passar o título de pesquisa como dados de rota (um segmento de URL), em vez de como um valor de cadeia de consulta.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

No entanto, você não pode esperar que os usuários modifiquem a URL sempre que desejarem pesquisar um filme. Agora você adicionará da interface do usuário para ajudá-los filtrar filmes. Se você tiver alterado a assinatura do `SearchIndex` método de teste como passar o parâmetro de ID associado a rota, alterá-la novamente para que seu `SearchIndex` método usa um parâmetro de cadeia de caracteres chamado `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Abra o *Views\Movies\SearchIndex.cshtml* de arquivo e, imediatamente depois `@Html.ActionLink("Create New", "Create")`, adicione o seguinte:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

O exemplo a seguir mostra uma parte do *Views\Movies\SearchIndex.cshtml* arquivo com a marcação de filtragem adicional.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

O `Html.BeginForm` auxiliar cria uma abertura `<form>` marca. O `Html.BeginForm` auxiliar faz com que o formulário postar a mesmo quando o usuário envia o formulário clicando o **filtro** botão.

Execute o aplicativo e tente pesquisar por um filme.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Não há nenhum `HttpPost` de sobrecarga do `SearchIndex` método. Não é necessário, porque o método não estiver alterando o estado do aplicativo, apenas a filtragem de dados.

Você poderá adicionar o método `HttpPost SearchIndex` a seguir. Nesse caso, o chamador de ação corresponderia a `HttpPost SearchIndex` método e o `HttpPost SearchIndex` método seria executado conforme mostrado na imagem abaixo.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

No entanto, mesmo se você adicionar esta versão `HttpPost` do método `SearchIndex`, haverá uma limitação na forma de como isso tudo foi implementado. Imagine que você deseja adicionar uma pesquisa específica como Favoritos ou enviar um link para seus amigos para que eles possam clicar para ver a mesma lista filtrada de filmes. Observe que a URL para a solicitação HTTP POST é o mesmo que a URL para a solicitação GET (localhost:xxxxx filmes/SearchIndex) - não há nenhuma informação de pesquisa na URL em si. Direita agora, as informações de cadeia de caracteres de pesquisa são enviadas ao servidor como um valor de campo de formulário. Isso significa que não é possível capturar essas informações de pesquisa para indicadores ou enviar para amigos em uma URL.

A solução é usar uma sobrecarga de `BeginForm` que especifica que a solicitação POST deve adicionar as informações de pesquisa para a URL e que deve ser roteada para a versão HttpGet do `SearchIndex` método. Substituir o sem parâmetros `BeginForm` método com o seguinte:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

Agora, quando você enviar uma pesquisa, a URL contém uma cadeia de caracteres de consulta de pesquisa. A pesquisa também irá para o método de ação `HttpGet SearchIndex`, mesmo se você tiver um método `HttpPost SearchIndex`.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Adicionando a pesquisa por gênero

Se você adicionou o `HttpPost` versão do `SearchIndex` método, excluí-lo agora.

Em seguida, você adicionará um recurso para permitir que os usuários procurar filmes por gênero. Substitua o método `SearchIndex` pelo seguinte código:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Esta versão do `SearchIndex` método aceita um parâmetro adicional, ou seja, `movieGenre`. As primeiras linhas de código criam um `List` objeto para manter gêneros de filme do banco de dados.

O código a seguir é uma consulta LINQ que recupera todos os gêneros do banco de dados.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

O código usa o `AddRange` método genérica `List` coleção para adicionar todos os gêneros distintos à lista. (Sem o `Distinct` modificador, gêneros duplicados serão adicionados, por exemplo, comédia poderia ser adicionada duas vezes em nosso exemplo). O código, em seguida, armazena a lista de gêneros no `ViewBag` objeto.

O código a seguir mostra como verificar o `movieGenre` parâmetro. Se não estiver vazia ainda mais o código restringe a consulta de filmes para limitar os filmes selecionados para o gênero especificado.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>A adição de marcação para a exibição de SearchIndex para oferecer suporte à pesquisa por gênero

Adicionar uma `Html.DropDownList` auxiliar para o *Views\Movies\SearchIndex.cshtml* arquivo, antes de `TextBox` auxiliar. A marcação concluída é mostrada abaixo:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Execute o aplicativo e navegue até *filmes/SearchIndex*. Tente fazer uma pesquisa por gênero, nome de filme e ambos os critérios.

Nesta seção você examinou os métodos de ação de CRUD e gerados pela estrutura de modos de exibição. Você criou um método de ação de pesquisa e o modo de exibição que permitem aos usuários a pesquisar por título do filme e gênero. Na próxima seção, você verá como adicionar uma propriedade para o `Movie` modelo e como adicionar um inicializador que criará automaticamente um banco de dados de teste.

>[!div class="step-by-step"]
[Anterior](accessing-your-models-data-from-a-controller.md)
[Próximo](adding-a-new-field.md)
