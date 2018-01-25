---
uid: mvc/overview/getting-started/introduction/adding-search
title: Search | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="search"></a>Pesquisar
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Adicionando um método de pesquisa e a exibição de pesquisa

Nesta seção, você adicionará a funcionalidade de pesquisa para o `Index` método de ação que lhe permite pesquisar filmes por gênero ou nome.

## <a name="updating-the-index-form"></a>Atualizar o formulário de índice

Inicie atualizando o `Index` método de ação existente `MoviesController` classe. Aqui está o código:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

A primeira linha do `Index` método cria a seguinte [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consulta para selecionar os filmes:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

A consulta está definida neste ponto, mas ainda não foi executada no banco de dados.

Se o `searchString` parâmetro contém uma cadeia de caracteres, a consulta de filmes é modificada para filtrar o valor da cadeia de pesquisa, usando o seguinte código:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

O código `s => s.Title` acima é uma [Expressão Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Lambdas são usados no método [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consultas como argumentos para métodos de operadores de consulta padrão, como o [onde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) método usado no código acima. Consultas LINQ não são executadas quando elas são definidas ou quando eles são modificados chamando um método como `Where` ou `OrderBy`. Em vez disso, a execução da consulta é adiada, o que significa que a avaliação de uma expressão é atrasada até que seu valor realizada na verdade é iterada ou [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) método é chamado. No `Search` exemplo, a consulta é executada no *cshtml* exibição. Para obter mais informações sobre a execução de consulta adiada, consulte [Execução da consulta](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> O [contém](https://msdn.microsoft.com/library/bb155125.aspx) método é executado no banco de dados, não o código c# acima. No banco de dados, [contém](https://msdn.microsoft.com/library/bb155125.aspx) mapeia para [SQL como](https://msdn.microsoft.com/library/ms179859.aspx), que diferencia maiusculas de minúsculas.

Agora você pode atualizar o `Index` modo de exibição que exibirá o formulário para o usuário.

Execute o aplicativo e navegue até *filmes/índice*. Acrescente uma cadeia de consulta, como `?searchString=ghost`, à URL. Os filmes filtrados são exibidos.

![SearchQryStr](adding-search/_static/image1.png)

Se você alterar a assinatura do `Index` método tem um parâmetro chamado `id`, o `id` parâmetro corresponderá a `{id}` roteia o espaço reservado para o padrão definido no *aplicativo\_Start RouteConfig.cs* arquivo.

[!code-json[Main](adding-search/samples/sample4.json)]

O original `Index` método tem esta aparência:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

A modificação `Index` método seria semelhante ao seguinte:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Agora você pode passar o título de pesquisa como dados de rota (um segmento de URL), em vez de como um valor de cadeia de consulta.

![](adding-search/_static/image2.png)

No entanto, você não pode esperar que os usuários modifiquem a URL sempre que desejarem pesquisar um filme. Agora você adicionará da interface do usuário para ajudá-los filtrar filmes. Se você tiver alterado a assinatura do `Index` método de teste como passar o parâmetro de ID associado a rota, alterá-la novamente para que seu `Index` método usa um parâmetro de cadeia de caracteres chamado `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Abra o *Views\Movies\Index.cshtml* de arquivo e, imediatamente depois `@Html.ActionLink("Create New", "Create")`, adicione a marcação de formulário realçada abaixo:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

O `Html.BeginForm` auxiliar cria uma abertura `<form>` marca. O `Html.BeginForm` auxiliar faz com que o formulário postar a mesmo quando o usuário envia o formulário clicando o **filtro** botão.

Visual Studio 2013 tem uma boa melhoria ao exibir e editar arquivos de exibição. Quando você executa o aplicativo com um arquivo de exibição aberta, o Visual Studio 2013 invoca o método de ação do controlador correto para exibir o modo de exibição.

![](adding-search/_static/image3.png)

Com a exibição do índice aberto no Visual Studio (conforme mostrado na imagem acima), toque Ctr F5 ou F5 para executar o aplicativo e, em seguida, tente pesquisar por um filme.

![](adding-search/_static/image4.png)

Não há nenhum `HttpPost` de sobrecarga do `Index` método. Não é necessário, porque o método não estiver alterando o estado do aplicativo, apenas a filtragem de dados.

Você poderá adicionar o método `HttpPost Index` a seguir. Nesse caso, o chamador de ação corresponderia a `HttpPost Index` método e o `HttpPost Index` método seria executado conforme mostrado na imagem abaixo.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

No entanto, mesmo se você adicionar esta versão `HttpPost` do método `Index`, haverá uma limitação na forma de como isso tudo foi implementado. Imagine que você deseja adicionar uma pesquisa específica como Favoritos ou enviar um link para seus amigos para que eles possam clicar para ver a mesma lista filtrada de filmes. Observe que a URL para a solicitação HTTP POST é o mesmo que a URL para a solicitação GET (localhost:xxxxx filmes/índice) - não há nenhuma informação de pesquisa na URL em si. Direita agora, as informações de cadeia de caracteres de pesquisa são enviadas ao servidor como um valor de campo de formulário. Isso significa que não é possível capturar essas informações de pesquisa para indicadores ou enviar para amigos em uma URL.

A solução é usar uma sobrecarga de `BeginForm` que especifica que a solicitação POST deve adicionar as informações de pesquisa para a URL e que ela deve ser roteada para o `HttpGet` versão do `Index` método. Substituir o sem parâmetros `BeginForm` método com a seguinte marcação:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Agora, quando você enviar uma pesquisa, a URL contém uma cadeia de caracteres de consulta de pesquisa. A pesquisa também irá para o método de ação `HttpGet Index`, mesmo se você tiver um método `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Adicionando a pesquisa por gênero

Se você adicionou o `HttpPost` versão do `Index` método, excluí-lo agora.

Em seguida, você adicionará um recurso para permitir que os usuários procurar filmes por gênero. Substitua o método `Index` pelo seguinte código:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Esta versão do `Index` método aceita um parâmetro adicional, ou seja, `movieGenre`. As primeiras linhas de código criam um `List` objeto para manter gêneros de filme do banco de dados.

O código a seguir é uma consulta LINQ que recupera todos os gêneros do banco de dados.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

O código usa o `AddRange` método genérica `List` coleção para adicionar todos os gêneros distintos à lista. (Sem o `Distinct` modificador, gêneros duplicados serão adicionados, por exemplo, comédia poderia ser adicionada duas vezes em nosso exemplo). O código, em seguida, armazena a lista de gêneros no `ViewBag.MovieGenre` objeto. Armazenando dados de categoria (do tal um filme gênero) como um [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) do objeto em um `ViewBag`, acessando os dados de categoria em uma caixa de lista suspensa é uma abordagem típica para aplicativos MVC.

O código a seguir mostra como verificar o `movieGenre` parâmetro. Se não estiver vazia, o código adicional restringe a consulta de filmes para limitar os filmes selecionados para o gênero especificado.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Como mencionado anteriormente, a consulta não é executada na base de dados até que a lista de filme é iterada (que ocorre no modo de exibição, após o `Index` método de ação retorna).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>A adição de marcação para a exibição do índice para oferecer suporte à pesquisa por gênero

Adicionar uma `Html.DropDownList` auxiliar para o *Views\Movies\Index.cshtml* arquivo, antes de `TextBox` auxiliar. A marcação concluída é mostrada abaixo:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

No código a seguir:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

O parâmetro "MovieGenre" fornece a chave para o `DropDownList` auxiliar para localizar um `IEnumerable<SelectListItem>` no `ViewBag`. O `ViewBag` foi preenchido no método de ação:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

O parâmetro "All" fornece um rótulo de opção. Se você inspecionar essa escolha no seu navegador, você verá que o atributo de "valor" está vazio. Como o nosso controlador apenas filtra `if` a cadeia de caracteres não é `null` ou está vazia, enviando um valor vazio para `movieGenre` mostra todos os gêneros.

Você também pode definir uma opção para ser selecionada por padrão. Se você quisesse "Comédia" como a opção padrão, altere o código no controlador da seguinte forma:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Execute o aplicativo e navegue até *filmes/índice*. Tente fazer uma pesquisa por gênero, nome de filme e ambos os critérios.

![](adding-search/_static/image8.png)

Nesta seção, você criou um método de ação de pesquisa e o modo de exibição que permitem aos usuários a pesquisar por título do filme e gênero. Na próxima seção, você verá como adicionar uma propriedade para o `Movie` modelo e como adicionar um inicializador que criará automaticamente um banco de dados de teste.

>[!div class="step-by-step"]
[Anterior](examining-the-edit-methods-and-edit-view.md)
[Próximo](adding-a-new-field.md)
