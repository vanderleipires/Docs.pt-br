---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Introdução ao ASP.NET Web Pages - criando um Layout consistente | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como usar layouts para criar uma aparência consistente para as páginas em um site que usa as páginas da Web ASP.NET. Ele pressupõe que você tenha concluído a...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: a6a007678d58547e9987ebda46bd08ae8aea66f7
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021165"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introdução ao ASP.NET Web Pages - criando um Layout consistente
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como usar *layouts* para criar uma aparência consistente para as páginas em um site que usa as páginas da Web ASP.NET. Ele pressupõe que você tenha concluído a série por meio [excluir do banco de dados em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> O que você aprenderá:
> 
> - É o que uma página de layout.
> - Como combinar as páginas de layout com conteúdo dinâmico.
> - Como passar valores para uma página de layout.


## <a name="about-layouts"></a>Sobre Layouts

As páginas que você criou até agora todas forem concluídas, páginas autônomas. Todos eles pertencem ao mesmo site, mas eles não têm uma aparência padrão ou todos os elementos comuns.

A maioria dos sites têm uma aparência consistente e o layout. Por exemplo, se você for para o [Microsoft.com/web](https://www.microsoft.com/web/) do site e olhar em volta, você verá que as páginas de todas as aderem a um layout geral e um tema visual:

![Página do site Microsoft.com/Web que mostra o layout do cabeçalho, área de navegação, área de conteúdo e do rodapé](layouts/_static/image1.png)

Uma *ineficiente* maneira para criar esse layout seria definir um cabeçalho, barra de navegação e rodapé separadamente em cada uma das suas páginas. Você poderia ser duplicando a mesma marcação cada vez. Se você quiser alterar algo (por exemplo, atualize o rodapé), você precisaria alterar cada página separadamente.

É aí que estão *páginas de layout* entram. Em páginas de Web do ASP.NET, você pode definir uma página de layout que fornece um contêiner geral para páginas em seu site. Por exemplo, a página de layout pode conter o cabeçalho, área de navegação e rodapé. A página de layout inclui um espaço reservado para onde vai o conteúdo principal.

Em seguida, você pode definir páginas de conteúdo individuais que contêm a marcação e o código para essa página somente. Páginas de conteúdo não precisam ser completar páginas HTML; eles nem precisam ter um `<body>` elemento. Eles também têm uma linha de código que diz ao ASP.NET qual página de layout que você deseja exibir o conteúdo. Aqui está uma figura que mostra mais ou menos como funciona essa relação:

![Diagrama conceitual que mostra duas páginas de conteúdo e uma página de layout no qual eles se ajustam](layouts/_static/image2.png)

Essa interação é fácil de entender quando você vê-lo em ação. Neste tutorial, você alterará as páginas de filmes para usar um layout.

## <a name="adding-a-layout-page"></a>Adicionando uma página de Layout

Você começará criando uma página de layout que define um layout de página típico com um cabeçalho, rodapé e uma área para o conteúdo principal. No site WebPagesMovies, adicionar uma página CSHTML denominada  *\_layout. cshtml*.

O sublinhado à esquerda ( `_` ) caractere é significativo. Se o nome de uma página começa com um sublinhado, o ASP.NET não enviará diretamente nessa página para o navegador. Essa convenção permite definir as páginas que são necessárias para seu site, mas que os usuários não devem ser capazes de solicitar diretamente.

Substitua o conteúdo da página com o seguinte:

[!code-html[Main](layouts/samples/sample1.html)]

Como você pode ver, essa marcação é apenas o HTML que usa `<div>` elementos para definir três seções na página, além do ano mais `<div>` elemento para conter as três seções. O rodapé contém um pouco de código Razor: `@DateTime.Now.Year`, que será renderizado o ano atual nesse local na página.

Observe que há um link para uma folha de estilo nomeada *Movies.css*. A folha de estilos é onde os detalhes do layout físico dos elementos serão definidos. Você vai criar que em breve.

O recurso somente incomum desta  *\_layout. cshtml* página é o `@Render.Body()` linha. Esse é o espaço reservado aonde o conteúdo quando este layout é mesclado com outra página.

## <a name="adding-a-css-file"></a>Adicionando um arquivo. CSS

A maneira preferencial para definir a organização real (ou seja, aparência) dos elementos na página é usar regras de (CSS) de folha de estilos em cascata. Portanto, você criará um *. CSS* arquivo com as regras para seu novo layout.

No WebMatrix, selecione a raiz do seu site. Em seguida, no **arquivos** guia de faixa de opções, clique na seta sob o **New** botão e, em seguida, clique em **nova pasta**.

![A opção 'Nova pasta' em novo na faixa de opções.](layouts/_static/image3.png)

Nomeie a nova pasta *estilos*.

![Nomear a nova pasta 'Estilos'](layouts/_static/image4.png)

Dentro do novo *estilos* pasta, crie um arquivo chamado *Movies.css*.

![Criando um novo arquivo Movies.css](layouts/_static/image5.png)

Substitua o conteúdo do novo *. CSS* arquivo com o seguinte:

[!code-css[Main](layouts/samples/sample2.css)]

Não falaremos muito sobre essas regras CSS, exceto a Observe duas coisas. Uma é que além de definir fontes e tamanhos, as regras de usam posicionamento absoluto para estabelecer o local do cabeçalho, rodapé e área de conteúdo principal. Se você for novo no posicionamento no CSS, você pode ler o [posicionamento CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial no site W3Schools.

A outra coisa a observar é que, na parte inferior, podemos ter copiado as regras de estilo que foram originalmente definida individualmente na *Movies.cshtml* arquivo. Essas regras foram usadas na [Introdução aos dados exibindo por usando páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para tornar o `WebGrid` auxiliar renderizar a marcação que adicionou faixas à tabela. (Se você pretende usar um *. CSS* arquivo para definições de estilo, você também pode colocar as regras de estilo para o site inteiro nele.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Atualizando o arquivo de filmes para usar o Layout

Agora você pode atualizar os arquivos existentes no seu site para usar o novo layout. Abra o *Movies.cshtml* arquivo. Na parte superior, como a primeira linha de código, adicione o seguinte:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Agora começa a página dessa forma:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Esta linha de código diz ao ASP.NET que, quando o *filmes* página é executada, ele deve ser mesclado com o  *\_layout. cshtml* arquivo.

Uma vez que o *Movies.cshtml* arquivo agora usa uma página de layout, você pode remover a marcação da *Movies.cshtml* página já foi solucionado pelo  *\_layout. cshtml*arquivo. Tire o `<!DOCTYPE>`, `<html>`, e `<body>` de abertura e fechamento de marcas. Remova todo o `<head>` elemento e seu conteúdo, que inclui as regras de estilo para a grade, uma vez que você já tem essas regras um *. CSS* arquivo. Enquanto você faz isso, alterar existente `<h1>` elemento para um `<h2>` elemento; você tem um `<h1>` elemento na página de layout já. Alterar o `<h2>` texto para "Lista de filmes".

Normalmente você não teria que fazer esses tipos de alterações em uma página de conteúdo. Quando você começar seu site com uma página de layout, criar páginas de conteúdo sem todos esses elementos para começar. Nesse caso, no entanto, você está convertendo uma página autônoma para uma que usa um layout, portanto, não há um pouco de limpeza.

Quando tiver terminado, o *Movies.cshtml* página ficará semelhante ao seguinte:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Teste o Layout

Agora você pode ver a aparência do layout. No WebMatrix, clique com botão direito do *Movies.cshtml* página e selecione **iniciar no navegador**. Quando o navegador exibe a página, ele se parece com esta página:

![Página de filmes renderizada usando um layout](layouts/_static/image6.png)

ASP.NET mesclou o conteúdo da página Movies.cshtml para o  *\_layout. cshtml* página direita, onde o `RenderBody` método é. E claro o  *\_layout. cshtml* página referências uma *. CSS* arquivo que define a aparência da página.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Atualizando a página de AddMovie para usar o Layout

O verdadeiro benefício de layouts é que você pode usá-los para todas as páginas em seu site. Abra o *AddMovie.cshtml* página.

Você pode se lembrar de que o *AddMovie.cshtml* página originalmente tinha algumas regras CSS nele para definir a aparência das mensagens de erro de validação. Como você tem um *. CSS* arquivo do seu site agora, você pode mover essas regras para o *. CSS* arquivo. Removê-los da *AddMovie.cshtml* do arquivo e adicioná-los na parte inferior do *Movies.css* arquivo. Você está movendo as regras a seguir:

[!code-css[Main](layouts/samples/sample6.css)]

Agora, verifique os mesmos tipos de alterações no *AddMovie.cshtml* que você fez para *Movies.cshtml* — adicione `Layout="~/_Layout.cshtml;` e remova a marcação HTML que agora é irrelevante. Alterar o `<h1>` elemento para `<h2>`. Quando terminar, a página se parecerá com este exemplo:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Execute a página. Agora, ele se parece com esta ilustração:

![Renderizado usando um layout de página 'Adicionar filmes'](layouts/_static/image7.png)

Você deseja fazer alterações semelhantes para as páginas no site — *EditMovie.cshtml* e *DeleteMovie.cshtml*. No entanto, antes de fazer, você pode fazer outra alteração no layout que torna um pouco mais flexível.

## <a name="passing-title-information-to-the-layout-page"></a>Passando informações de título para a página de Layout

O  *\_layout. cshtml* página que você criou tem um `<title>` elemento que é definido como "Meu Site de filme". A maioria dos navegadores exibir o conteúdo desse elemento como o texto em uma guia:

![A página &lt;título&gt; elemento exibido em uma guia do navegador](layouts/_static/image8.png)

Essas informações de título são genéricas. Suponha que você deseja que o texto do título para ser mais específico para a página atual. (O texto do título também é usado pelos mecanismos de pesquisa para determinar qual é sua página sobre.) Você pode passar informações de uma página de conteúdo, como *Movies.cshtml* ou *AddMovie.cshtml* para o layout da página e, em seguida, use essas informações para personalizar a página de layout processa.

Abra o *Movies.cshtml* página novamente. No código na parte superior, adicione a seguinte linha:

[!code-csharp[Main](layouts/samples/sample8.cs)]

O `Page` objeto está disponível em todos os *. cshtml* páginas e é para essa finalidade, ou seja, para compartilhar informações entre uma página e seu layout.

Abra o<em>\_layout. cshtml</em> página. Alterar o `<title>` , de modo que ele se parece com essa marcação:

[!code-html[Main](layouts/samples/sample9.html)]

Esse código renderiza tudo o que está no `Page.Title` propriedade diretamente nesse local na página.

Execute o *Movies.cshtml* página. Desta vez, a guia do navegador mostra o que é passado como o valor do `Page.Title`:

![Uma guia do navegador mostrando o título criado dinamicamente](layouts/_static/image9.png)

Se desejar, exiba a origem da página no navegador. Você pode ver que o `<title>` elemento é renderizado como `<title>List Movies</title>`.

> [!TIP] 
> 
> **O objeto de página**
> 
> Um recurso útil do `Page` é que ele é um objeto dinâmico — o `Title` propriedade não é um nome reservado ou fixo. Você pode usar *qualquer* nome do valor de `Page` objeto. Por exemplo, você poderia facilmente passar o título usando uma propriedade chamada `Page.CurrentName` ou `Page.MyPage`. A única restrição é que o nome deve seguir as regras normais de quais propriedades podem ser nomeadas. (Por exemplo, o nome não pode conter um espaço.)
> 
> Você pode passar qualquer número de valores usando o `Page` objeto. Se você quiser passar informações de filmes para a página de layout, você pode passar valores usando algo como `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`. (Ou outros nomes inventada para armazenar as informações.) O único requisito — que é provavelmente óbvia — é que você precisa usar os mesmos nomes na página de conteúdo e a página de layout.
> 
> As informações transmitidas por meio de `Page` objeto não está limitado a apenas texto a ser exibido na página de layout. Você pode passar um valor para a página de layout e, em seguida, o código na página de layout pode usar o valor para decidir se deve exibir uma seção da página, o que *. CSS* de arquivos para usar e assim por diante. Os valores que você passe a `Page` objeto são como quaisquer outros valores que você usar no código. É assim que os valores são originadas na página de conteúdo e são passados para a página de layout.


Abra o *AddMovie.cshtml* da página e adicione uma linha na parte superior do código que fornece um título para o *AddMovie.cshtml* página:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Execute o *AddMovie.cshtml* página. Você verá o novo título lá:

![Uma guia do navegador mostrando o título 'Adicionar filmes' criado dinamicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Atualizando as páginas restantes para usar o Layout

Agora você pode concluir as páginas restantes no seu site para que eles usem o novo layout. Abra *EditMovie.cshtml* e *DeleteMovie.cshtml* por sua vez e fazer as mesmas alterações em cada um.

Adicione a linha de código que vincula a página de layout:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Adicione uma linha para definir o título da página:

[!code-csharp[Main](layouts/samples/sample12.cs)]

ou:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Remover toda a marcação HTML estranhas — basicamente, deixar apenas os bits que estão dentro do `<body>` elemento (mais o bloco de código na parte superior).

Alterar o `<h1>` elemento seja um `<h2>` elemento.

Quando você fez essas alterações, teste cada e certifique-se de que ela está exibindo corretamente e se o título está correto.

## <a name="parting-thoughts-about-layout-pages"></a>Pensamentos para encerrar sobre páginas de Layout

Neste tutorial, você criou uma  *\_layout. cshtml* página e usado o `RenderBody` método para mesclar o conteúdo de outra página. Que é o padrão básico para usar layouts em páginas da Web.

Páginas de layout têm recursos adicionais que não abordaremos aqui. Por exemplo, você pode aninhar as páginas de layout — uma página de layout por sua vez pode referenciar outro. Layouts aninhados podem ser útil se você estiver trabalhando com subseções de um site que exigem diferentes layouts. Você também pode usar métodos adicionais (por exemplo, `RenderSection`) para configurar a chamada seções na página de layout.

A combinação de páginas de layout e *. CSS* arquivos é poderoso. Como você verá na próxima série de tutoriais, no WebMatrix, você pode criar um site com base em um *modelo*, que oferece um site que possui funcionalidade predefinidas nele. Os modelos faça bom uso de CSS para criar sites que combinam bem e que têm recursos como menus e páginas de layout. Aqui está uma captura de tela da home page de um site com base em um modelo, mostrando os recursos que usam páginas de layout e CSS:

![Layout criado pelo modelo de site do WebMatrix mostrando o cabeçalho, área de navegação, área de conteúdo, seção opcional e links de logon](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Listagem completa para a página de filme (atualizada para usar uma página de Layout)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Página concluída listagem para adicionar a página de filme (atualizada para o Layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Listagem completa da página para página de exclusão de filme (atualizada para o Layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Listagem de página completa para a página de edição do filme (atualizada para o Layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Próximo

O próximo tutorial, você aprenderá como publicar seu site na Internet para que qualquer pessoa pode vê-lo.

## <a name="additional-resources"></a>Recursos adicionais

- [Criar uma aparência consistente](https://go.microsoft.com/fwlink/?LinkID=202891) — um artigo que fornece mais detalhes sobre como trabalhar com layouts. Ele também descreve como passar um valor para uma página de layout que mostra ou oculta a parte do conteúdo.
- [Páginas de Layout com o Razor aninhadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogs de Mike Brind um exemplo de como aninhar páginas de layout. (Inclui um download das páginas).

> [!div class="step-by-step"]
> [Anterior](deleting-data.md)
> [Próximo](publishing.md)
