---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: "Introdução a páginas da Web ASP.NET - criar um Layout consistente | Microsoft Docs"
author: tfitzmac
description: "Este tutorial mostra como usar layouts para criar uma aparência consistente para as páginas em um site que usa as páginas da Web ASP.NET. Ele pressupõe que você tenha concluído o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 692adc5a03892f27c91fe8868c8eab6ce08f49cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introdução a páginas da Web ASP.NET - criar um Layout consistente
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como usar *layouts* para criar uma aparência consistente para as páginas em um site que usa as páginas da Web ASP.NET. Ele pressupõe que você tenha concluído a série por meio de [excluir do banco de dados em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> O que você aprenderá:
> 
> - É que uma página de layout.
> - Como combinar páginas de layout com conteúdo dinâmico.
> - Como passar valores para uma página de layout.


## <a name="about-layouts"></a>Sobre Layouts

As páginas que você criou até agora todos foi concluídas, páginas autônomas. Todos eles pertencem ao mesmo site, mas eles não têm qualquer elementos comuns ou uma aparência padrão.

A maioria dos sites tem uma aparência consistente e o layout. Por exemplo, se você for para o [Microsoft.com/web](https://www.microsoft.com/web/) do site e pesquisa, você verá que as páginas de todos os aderem a um layout geral e um tema visual:

![Mostra o layout do cabeçalho, área de navegação, área de conteúdo e do rodapé de página de site Microsoft.com/Web](layouts/_static/image1.png)

Um *ineficiente* maneira de criar este layout seria definir um cabeçalho, a barra de navegação e o rodapé separadamente em cada uma das páginas. Você poderia ser duplicar a mesma marcação cada vez. Se você quiser alterar algo (por exemplo, atualize o rodapé), você precisa alterar cada página separadamente.

É aí que *páginas de layout* entrar. Em páginas de Web do ASP.NET, você pode definir uma página de layout que fornece um contêiner geral para páginas em seu site. Por exemplo, a página de layout pode conter o cabeçalho, a área de navegação e o rodapé. A página de layout inclui um espaço reservado para onde vai o conteúdo principal.

Em seguida, você pode definir páginas de conteúdo individuais que contêm a marcação e o código para essa página somente. Páginas de conteúdo não têm de ser páginas HTML completas. eles ainda não precisam ter um `<body>` elemento. Eles também têm uma linha de código que diz ao ASP.NET que você deseja exibir o conteúdo de página de layout. Aqui está uma figura que mostra aproximadamente como funciona esse relação:

![Diagrama conceitual que mostra duas páginas de conteúdo e uma página de layout no qual eles se](layouts/_static/image2.png)

Essa interação é fácil de entender quando você vê-lo em ação. Neste tutorial, você alterará as páginas de filmes para usar um layout.

## <a name="adding-a-layout-page"></a>Adicionando uma página de Layout

Comece criando uma página de layout que define um layout de página típico com um cabeçalho, rodapé e uma área para o conteúdo principal. No site WebPagesMovies, adicione uma página CSHTML chamada  *\_cshtml*.

O sublinhado à esquerda ( `_` ) caractere é significativo. Se o nome da página começa com um sublinhado, ASP.NET não enviar uma página diretamente para o navegador. Essa convenção permite definir páginas que são necessárias para o seu site, mas que os usuários não devem ser capazes de solicitar diretamente.

Substitua o conteúdo da página com o seguinte:

[!code-html[Main](layouts/samples/sample1.html)]

Como você pode ver, essa marcação é apenas HTML que usa `<div>` elementos para definir três seções na página de mais de um mais `<div>` elemento para conter as três seções. O rodapé contém um pouco de código Razor: `@DateTime.Now.Year`, que processará o ano atual nesse local na página.

Observe que há um link para uma folha de estilos chamada *Movies.css*. A folha de estilos é onde os detalhes do layout físico dos elementos serão definidos. Você criará que em alguns instantes.

O recurso somente incomuns na  *\_cshtml* página é a `@Render.Body()` linha. É o espaço reservado aonde o conteúdo quando este layout é mesclado com outra página.

## <a name="adding-a-css-file"></a>Adicionando um arquivo. CSS

A melhor maneira de definir a organização real (ou seja, aparência) dos elementos na página é usar regras de folhas de estilos em cascata. Portanto, você criará uma *. CSS* arquivo com as regras para o novo layout.

No WebMatrix, selecione a raiz do seu site. Em seguida, no **arquivos** guia da faixa de opções, clique na seta sob o **novo** botão e, em seguida, clique em **nova pasta**.

![A opção 'Nova pasta' abaixo de novo na faixa de opções.](layouts/_static/image3.png)

Nomeie a nova pasta *estilos*.

![Nomear a nova pasta 'Estilos'](layouts/_static/image4.png)

Dentro do novo *estilos* pasta, crie um arquivo chamado *Movies.css*.

![Criar um novo arquivo Movies.css](layouts/_static/image5.png)

Substitua o conteúdo do novo *. CSS* arquivo com o seguinte:

[!code-css[Main](layouts/samples/sample2.css)]

Nós não falaremos muito sobre essas regras CSS, exceto ao Observe duas coisas. Uma é que além de definir fontes e tamanhos, as regras de usam o posicionamento absoluto para estabelecer o local do cabeçalho, rodapé e área de conteúdo principal. Se você estiver familiarizado com o posicionamento no CSS, você pode ler o [posicionamento CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial no site W3Schools.

Outra coisa a observar é que, na parte inferior, podemos copiou as regras de estilo que foram originalmente definida individualmente no *Movies.cshtml* arquivo. Essas regras foram usadas no [Introdução à exibição de dados, usando ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para tornar o `WebGrid` auxiliar renderizar a marcação que adicionou faixas para a tabela. (Se você pretender usar um *. CSS* arquivo de definições de estilo, você também pode colocar as regras de estilo para o site inteiro nele.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Atualizando o arquivo de filmes para usar o Layout

Agora você pode atualizar os arquivos existentes no seu site para usar o novo layout. Abra o *Movies.cshtml* arquivo. Na parte superior, como a primeira linha de código, adicione o seguinte:

[!code-csharp[Main](layouts/samples/sample3.cs)]

A página agora inicia desta forma:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Esta linha de código diz ao ASP.NET que, quando o *filmes* página é executada, ela deve ser mesclada com a  *\_cshtml* arquivo.

Como o *Movies.cshtml* arquivo agora usa uma página de layout, você pode remover a marcação do *Movies.cshtml* página tem manipulados pelo  *\_cshtml*arquivo. Retire a `<!DOCTYPE>`, `<html>`, e `<body>` marcas abertura e fechamento. Retire a todo o `<head>` elemento e seu conteúdo, que inclui as regras de estilo para a grade, desde que você já tem essas regras um *. CSS* arquivo. Enquanto você faz isso, altere o existente `<h1>` elemento para uma `<h2>` elemento; você não possui um `<h1>` elemento na página de layout já. Alterar o `<h2>` texto para "Lista de filmes".

Normalmente você não precisa fazer esses tipos de alterações em uma página de conteúdo. Quando você inicia o site com uma página de layout, criar páginas de conteúdo sem todos esses elementos em primeiro lugar. Nesse caso, no entanto, você está convertendo uma página de autônomo para um que usa um layout, para que haja um pouco de limpeza.

Quando você terminar, o *Movies.cshtml* página se parecerá com o seguinte:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Teste o Layout

Agora você pode ver a aparência do layout. No WebMatrix, clique com botão direito do *Movies.cshtml* página e selecione **iniciar no navegador**. Quando o navegador exibe a página, ele se parece com esta página:

![Renderizado usando um layout de página de filmes](layouts/_static/image6.png)

ASP.NET foi mesclado o conteúdo da página Movies.cshtml no  *\_cshtml* página direita, onde o `RenderBody` é método. E claro o  *\_cshtml* página referências um *. CSS* arquivo que define a aparência da página.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Atualizar a página AddMovie para usar o Layout

O benefício real de layouts é que você pode usá-los para todas as páginas em seu site. Abra o *AddMovie.cshtml* página.

Você deve se lembrar que o *AddMovie.cshtml* página originalmente tinha algumas regras CSS nele para definir a aparência das mensagens de erro de validação. Como você tem um *. CSS* arquivo para o seu site agora, você pode mover as regras para o *. CSS* arquivo. Removê-los do *AddMovie.cshtml* de arquivos e adicioná-los à parte inferior do *Movies.css* arquivo. Você está movendo as regras a seguir:

[!code-css[Main](layouts/samples/sample6.css)]

Fazer os mesmos tipos de alterações em *AddMovie.cshtml* que você fez para *Movies.cshtml* — adicione `Layout="~/_Layout.cshtml;` e remover a marcação HTML que agora é insignificante. Alterar o `<h1>` elemento `<h2>`. Quando terminar, a página será semelhante a este exemplo:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Execute a página. Agora parece que esta ilustração:

![Renderizado usando um layout de página 'Adicionar filmes'](layouts/_static/image7.png)

Você deseja fazer alterações semelhantes para as páginas no site — *EditMovie.cshtml* e *DeleteMovie.cshtml*. No entanto, antes de fazer, você pode fazer outra alteração no layout que torna um pouco mais flexível.

## <a name="passing-title-information-to-the-layout-page"></a>Passando informações de título para a página de Layout

O  *\_cshtml* página que você criou tem um `<title>` elemento que é definido como "Meu Site filme". A maioria dos navegadores exibir o conteúdo desse elemento como o texto em uma guia:

![A página &lt;título&gt; elemento exibido em uma guia do navegador](layouts/_static/image8.png)

Essas informações de título são genéricas. Suponha que você deseja que o texto do título para ser mais específico para a página atual. (O texto do título também é usado por mecanismos de pesquisa para determinar qual é a página sobre.) Você pode transmitir informações de uma página de conteúdo como *Movies.cshtml* ou *AddMovie.cshtml* para a página de layout e, em seguida, use essas informações para personalizar a página de layout que processa.

Abra o *Movies.cshtml* página novamente. No código na parte superior, adicione a seguinte linha:

[!code-csharp[Main](layouts/samples/sample8.cs)]

O `Page` o objeto está disponível em todos os *. cshtml* páginas e é para essa finalidade, ou seja, para compartilhar informações entre uma página e seu layout.

Abra o*\_cshtml* página. Alterar o `<title>` para que ela se pareça com essa marcação de elemento:

[!code-html[Main](layouts/samples/sample9.html)]

Esse código processa tudo o que está no `Page.Title` propriedade diretamente no local na página.

Execute o *Movies.cshtml* página. Neste momento, a guia navegador mostra o que é passado como o valor de `Page.Title`:

![Uma guia do navegador mostrar o título criado dinamicamente](layouts/_static/image9.png)

Se desejar, exiba a origem da página no navegador. Você pode ver que o `<title>` elemento é processado como `<title>List Movies</title>`.

> [!TIP] 
> 
> **O objeto de página**
> 
> Um recurso útil de `Page` é que ele é um objeto dinâmico — o `Title` propriedade não é um nome reservado ou fixo. Você pode usar *qualquer* para um valor de nome o `Page` objeto. Por exemplo, pode facilmente passado o título usando uma propriedade denominada `Page.CurrentName` ou `Page.MyPage`. A única restrição é que o nome deve seguir as regras normais de quais propriedades podem ser nomeadas. (Por exemplo, o nome não pode conter um espaço.)
> 
> Você pode passar qualquer número de valores usando o `Page` objeto. Se você quiser passar informações de filme para a página de layout, você poderia passar valores usando algo como `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`. (Ou outros nomes criado para armazenar as informações.) O único requisito — que é provavelmente óbvia — é que você precisa usar os mesmos nomes na página de conteúdo e a página de layout.
> 
> As informações que passam por meio de `Page` não está limitado a apenas texto a ser exibido na página de layout. Você pode passar um valor para a página de layout e, em seguida, o código na página de layout pode usar o valor para decidir se deseja exibir uma seção da página, o que *. CSS* de arquivos para usar e assim por diante. Os valores que você passa o `Page` objeto são como quaisquer outros valores que você use no código. É assim que os valores são originadas na página de conteúdo e são passados para a página de layout.


Abra o *AddMovie.cshtml* página e adicione uma linha à parte superior do código que fornece um título para o *AddMovie.cshtml* página:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Execute o *AddMovie.cshtml* página. Você verá o novo título existe:

![Uma guia do navegador mostrar o título 'Adicionar filmes' criado dinamicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Atualizando as páginas restantes para usar o Layout

Agora você pode concluir as páginas restantes no seu site para que eles usam o novo layout. Abra *EditMovie.cshtml* e *DeleteMovie.cshtml* por sua vez e fazer as mesmas alterações em cada um.

Adicione a linha de código que contém links para a página de layout:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Adicione uma linha para definir o título da página:

[!code-csharp[Main](layouts/samples/sample12.cs)]

ou:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Remover toda a marcação HTML estranhas – basicamente, deixe apenas os bits que estão dentro de `<body>` elemento (mais o bloco de código na parte superior).

Alterar o `<h1>` elemento seja um `<h2>` elemento.

Quando você fez essas alterações, teste cada e certifique-se de que ela está exibindo corretamente e se o título está correto.

## <a name="parting-thoughts-about-layout-pages"></a>Incluir sugestões sobre páginas de Layout

Neste tutorial, você criou um  *\_cshtml* página e usado o `RenderBody` método para mesclar o conteúdo de outra página. Que é o padrão básico para usar layouts em páginas da Web.

Páginas de layout têm recursos adicionais que não tenha sido abordada aqui. Por exemplo, você pode aninhar páginas de layout, uma página de layout por sua vez pode referenciar outro. Layouts aninhados podem ser útil se você estiver trabalhando com subseções de um site que exigem layouts diferentes. Você também pode usar métodos adicionais (por exemplo, `RenderSection`) para configurar a chamada seções na página de layout.

A combinação de páginas de layout e *. CSS* arquivos é eficiente. Como você verá na próxima série tutorial, no WebMatrix você pode criar um site com base em um *modelo*, que oferece um site que possui funcionalidade pré-compilada nele. Os modelos fazem bom uso das páginas de layout e CSS para criar sites que uma aparência excelente e que têm recursos, como menus. Aqui está uma captura de tela da página inicial de um site com base em um modelo, mostrando os recursos que usam páginas de layout e CSS:

![Layout criado pelo modelo de site do WebMatrix mostrando o cabeçalho, área de navegação, área de conteúdo, seção opcional e links de logon](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Listagem completa para a página de filme (atualizada para usar uma página de Layout)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Página concluir listagem para adicionar a página de filme (atualizada para o Layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Concluir a lista da página para excluir a página do filme (atualizada para o Layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Listagem de página concluída para a página de edição de filme (atualizada para o Layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial, você aprenderá como publicar seu site da Internet para que qualquer pessoa pode vê-lo.

## <a name="additional-resources"></a>Recursos adicionais

- [Criando uma aparência consistente](https://go.microsoft.com/fwlink/?LinkID=202891) — um artigo que fornece mais detalhes sobre como trabalhar com layouts. Ele também descreve como passar um valor para uma página de layout que mostra ou oculta o conteúdo.
- [Páginas de Layout com Razor aninhadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogs Mike Brind um exemplo de como aninhar páginas de layout. (Inclui um download das páginas.)

>[!div class="step-by-step"]
[Anterior](deleting-data.md)
[Próximo](publishing.md)
