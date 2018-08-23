---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Criar Layouts de página com exibir páginas mestras (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá como criar um layout de página comuns para várias páginas em seu aplicativo, tirando proveito da exibição de páginas mestras. Você pode usar um...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ff74b1dc1d83b7ec1c8ecf6eca341a5cd14403f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831629"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>Criar Layouts de página com exibir páginas mestras (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> Neste tutorial, você aprenderá como criar um layout de página comuns para várias páginas em seu aplicativo, tirando proveito da exibição de páginas mestras. Você pode usar uma página mestra do modo de exibição, por exemplo, para definir um layout de página de duas colunas e use o layout de duas colunas para todas as páginas em seu aplicativo web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Criar Layouts de página com exibir páginas mestras

Neste tutorial, você aprenderá como criar um layout de página comuns para várias páginas em seu aplicativo, tirando proveito da exibição de páginas mestras. Você pode usar uma página mestra do modo de exibição, por exemplo, para definir um layout de página de duas colunas e use o layout de duas colunas para todas as páginas em seu aplicativo web.

Você também pode tirar proveito da exibição de páginas mestras para compartilhar conteúdo comum em várias páginas em seu aplicativo. Por exemplo, você pode colocar seu logotipo do site, links de navegação e anúncios de faixa em uma página de exibição mestre. Dessa forma, todas as páginas em seu aplicativo exibiria esse conteúdo automaticamente.

Neste tutorial, você aprenderá como criar uma nova página mestra do modo de exibição e criar uma nova página de conteúdo de modo de exibição com base na página mestra.

### <a name="creating-a-view-master-page"></a>Criando uma página mestra do modo de exibição

Vamos começar criando uma página mestra do modo de exibição que define um layout de duas colunas. Você adicionar uma nova página mestra do modo de exibição para um projeto MVC clicando na pasta Views\Shared, selecionando a opção de menu **adicionar, Item novo**e selecione o modelo de página mestra do modo de exibição de MVC (veja a Figura 1).


[![Adicionando uma página mestra do modo de exibição](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Figura 01**: adicionando uma página de exibição mestre ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


Você pode criar mais de uma página de exibição mestre em um aplicativo. Cada página mestra do modo de exibição pode definir um layout de página diferentes. Por exemplo, você talvez queira determinadas páginas para ter um layout de duas colunas e outras páginas para ter um layout de três colunas.

Uma página mestra do modo de exibição é muito parecido com um modo de exibição padrão do ASP.NET MVC. No entanto, ao contrário do modo de exibição normal, uma página mestra do modo de exibição contém um ou mais `<asp:ContentPlaceHolder>` marcas. O `<contentplaceholder>` marcas são usadas para marcar as áreas da página mestra que pode ser substituído em uma página de conteúdo individual.

Por exemplo, a página mestra do modo de exibição na listagem 1 define um layout de duas colunas. Ele contém dois `<contentplaceholder>` marcas. Um `<ContentPlaceHolder>` para cada coluna.

**Listagem 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

O corpo da exibição de página mestra na listagem 1 contém dois `<div>` marcas que correspondem às duas colunas. A classe de coluna de folha de estilos em cascata é aplicada a ambas `<div>` marcas. Essa classe é definida na folha de estilos declarada na parte superior da página mestra. Você pode visualizar como a página mestra do modo de exibição será renderizada, alternando para o modo Design. Clique na guia Design na parte inferior esquerda do editor de código fonte (veja a Figura 2).


[![Visualizando uma página mestra no designer](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Figura 02**: visualizando uma página mestra no designer ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Criando uma página de conteúdo do modo de exibição

Depois de criar uma página mestra do modo de exibição, você pode criar a exibição de uma ou mais páginas de conteúdo com base na página mestre de modo de exibição. Por exemplo, você pode criar uma página de conteúdo de modo de exibição de índice para o controlador Home pelo botão direito do mouse na pasta Views\Home, selecionando **Add, o novo Item**, selecionando o **página de conteúdo de exibição do MVC** modelo, inserindo o nome do aspx e clicando em Adicionar botão (veja a Figura 3).


[![Adicionando uma página de conteúdo do modo de exibição](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Figura 03**: adicionando uma página de conteúdo do modo de exibição ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


Depois de clicar no botão Adicionar, uma nova caixa de diálogo é exibida que permite que você selecione uma página de exibição mestre para associar com a página de exibição de conteúdo (consulte a Figura 4). Você pode navegar para a página de exibição mestre de site que criamos na seção anterior.


[![Selecionar uma página mestra](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Figura 04**: selecionar uma página mestra ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


Depois de criar uma nova página de conteúdo de modo de exibição com base na página mestra master, você pode obter o arquivo na listagem 2.

**Listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Observe que essa exibição contém um `<asp:Content>` marca que corresponde a cada um do `<asp:ContentPlaceHolder>` marcas da página de exibição mestre. Cada `<asp:Content>` marca inclui um atributo ContentPlaceHolderID que aponta para a determinada `<asp:ContentPlaceHolder>` que ele substitui.

Além disso, observe que a página de exibição de conteúdo na listagem 2 não contém qualquer um dos normal marcas de abertura e fechamento HTML. Por exemplo, ele não contém a abertura e fechamento `<html>` ou `<head>` marcas. Normal marcas de abertura e fechamento estão contidos na página mestre de modo de exibição.

Qualquer conteúdo que você deseja exibir em uma página de conteúdo do modo de exibição deve ser colocado em um `<asp:Content>` marca. Se você colocar qualquer HTML ou outros tipos de conteúdo fora essas marcas, você irá receber um erro ao tentar exibir a página.

Você não precisa substituir cada `<asp:ContentPlaceHolder>` uma marca de uma página mestra em uma página de exibição de conteúdo. Você só precisará substituir um `<asp:ContentPlaceHolder>` marca quando você quiser substituir a marca com conteúdo específico.

Por exemplo, o modo de exibição do índice modificado na listagem 3 contém apenas dois `<asp:Content>` marcas. Cada um do `<asp:Content>` marcas inclui algum texto.

**Listagem 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Quando o modo de exibição na listagem 3 é solicitado, ele processa a página na Figura 5. Observe que o modo de exibição renderiza uma página com duas colunas. Além disso, observe que o conteúdo da página de conteúdo de exibição é mesclado com o conteúdo da página modo de exibição mestre.


[![A página de conteúdo de modo de exibição de índice](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Figura 05**: página de conteúdo do índice o modo de exibição ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modificando o conteúdo da página de exibição mestre

Um problema que você encontrar quase imediatamente ao trabalhar com exibir páginas mestras é o problema de modificar o conteúdo de página mestra do modo de exibição quando as páginas de conteúdo de outro modo de exibição são solicitadas. Por exemplo, você deseja que cada página em seu aplicativo web para ter um título exclusivo. No entanto, o título é declarado na página mestre de modo de exibição e não na página de conteúdo de exibição. Então, como você personalizar o título da página para cada página de conteúdo do modo de exibição?

Há duas maneiras que você pode modificar o título exibido por uma página de conteúdo do modo de exibição. Primeiro, você pode atribuir um título de página para o atributo title do `<%@ page %>` diretiva declarado na parte superior da página de conteúdo de exibição. Por exemplo, se você deseja atribuir o título da página "Super ótimo site" para a exibição de índice, você pode incluir a seguinte diretiva na parte superior da exibição índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Quando o modo de exibição de índice é renderizado no navegador, o título desejado é exibido na barra de título do navegador:


[![Barra de título do navegador](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


Há um requisito importante que uma página de modo de exibição mestre deve satisfazer para que o atributo de título para funcionar. A página mestra do modo de exibição deve conter um `<head runat="server">` marca em vez de um normal `<head>` marca para o seu cabeçalho. Se o `<head>` marca não inclui o runat = "servidor" de atributo e em seguida, o título não aparecerá. Modo de exibição padrão, página mestra inclui necessários `<head runat="server">` marca.

Uma abordagem alternativa à modificação de conteúdo da página mestra de uma página de conteúdo de exibição individual é encapsular a região que você deseja modificar em um `<asp:ContentPlaceHolder>` marca. Por exemplo, imagine que você deseja alterar não apenas o título, mas também as marcas meta, processadas por uma página de modo de exibição mestre. A página de modo de exibição mestre na listagem 4 contém um `<asp:ContentPlaceHolder>` marca dentro de seu `<head>` marca.

**Listagem 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Observe que o `<asp:ContentPlaceHolder>` marca na listagem 4 inclui conteúdo padrão: um título padrão e marcas de meta do padrão. Se você não substituir isso `<asp:ContentPlaceHolder>` marca em uma página de conteúdo de exibição individual, em seguida, o conteúdo padrão será exibido.

A página de exibição de conteúdo na listagem 5 substitui o `<asp:ContentPlaceHolder>` marca para exibir um título personalizado e marcas meta personalizadas.

**Listagem 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Resumo

Este tutorial forneceu uma introdução básica para exibir páginas mestras e páginas de conteúdo. Você aprendeu a criar a nova exibição de páginas mestras e criar páginas de conteúdo do modo de exibição com base neles. Também examinamos como você pode modificar o conteúdo de uma página mestra do modo de exibição de uma página de conteúdo do modo de exibição específico.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [Próximo](passing-data-to-view-master-pages-vb.md)
