---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Criar Layouts de página com páginas de exibição mestre (c#) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá como criar um layout de página comuns para várias páginas em seu aplicativo, tirando proveito da exibição de páginas mestras. Você pode usar um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 82500a311e1110713a60604027d018ba16539b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871240"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Criar Layouts de página com páginas de exibição mestre (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> Neste tutorial, você aprenderá como criar um layout de página comuns para várias páginas em seu aplicativo, tirando proveito da exibição de páginas mestras. Você pode usar uma página de exibição mestre, por exemplo, para definir um layout de página de duas colunas e use o layout de duas colunas para todas as páginas em seu aplicativo web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Criar Layouts de página com páginas de exibição mestre

Neste tutorial, você aprenderá como criar um layout de página comuns para várias páginas em seu aplicativo, tirando proveito da exibição de páginas mestras. Você pode usar uma página de exibição mestre, por exemplo, para definir um layout de página de duas colunas e use o layout de duas colunas para todas as páginas em seu aplicativo web.

Você também pode tirar proveito da exibição de páginas mestras para compartilhar conteúdo comum em várias páginas em seu aplicativo. Por exemplo, você pode colocar seu logotipo do site, links de navegação e anúncios de cabeçalho em uma página de exibição mestre. Dessa forma, todas as páginas em seu aplicativo deve exibir este conteúdo automaticamente.

Neste tutorial, você aprenderá como criar uma nova página mestra do modo de exibição e crie uma nova página de conteúdo de modo de exibição com base na página mestra.

### <a name="creating-a-view-master-page"></a>Criando uma página de exibição mestre

Vamos começar criando uma página de exibição mestre que define um layout de duas colunas. Você adicionar uma nova página mestra do modo de exibição para um projeto MVC clicando na pasta exibições \ compartilhadas, selecionando a opção de menu **adicionar, o novo Item**e selecionando o **página mestra de exibição MVC** modelo (consulte a Figura 1).


[![Adicionando uma página de exibição mestre](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figura 01**: adicionando uma página de exibição mestre ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Você pode criar mais de uma página de exibição mestre em um aplicativo. Cada página de exibição mestre pode definir um layout de página diferentes. Por exemplo, convém certas páginas têm um layout de duas colunas e outras páginas têm um layout de três colunas.

Uma página de exibição mestre é muito parecido com um modo de exibição padrão do ASP.NET MVC. No entanto, ao contrário do modo de exibição normal, uma página de exibição mestre contém um ou mais `<asp:ContentPlaceHolder>` marcas. O `<contentplaceholder>` marcas são usadas para marcar as áreas da página mestra que pode ser substituído em uma página de conteúdo individual.

Por exemplo, a página de exibição mestre na listagem 1 define um layout de duas colunas. Ele contém dois `<contentplaceholder>` marcas. Um `<ContentPlaceHolder>` para cada coluna.

**Listando 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

O corpo da exibição de página mestra na listagem 1 contém dois `<div>` marcas que correspondem às duas colunas. A classe de coluna de folha de estilos em cascata é aplicada a ambos `<div>` marcas. Essa classe é definida na folha de estilos declarada na parte superior da página mestra. Você pode visualizar como a página de exibição mestre será renderizada, alternando para o modo Design. Clique na guia Design na parte inferior esquerda do editor de código fonte (consulte a Figura 2).


[![Visualizando uma página mestra no designer](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figura 02**: visualizando uma página mestra no designer ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Criando uma página de exibição de conteúdo

Depois de criar uma página de exibição mestre, você pode criar a exibição de uma ou mais páginas de conteúdo com base na página modo de exibição mestre. Por exemplo, você pode criar uma página de conteúdo de exibição do índice para o controlador Home clicando na pasta Views\Home, selecionando **adicionar, o novo Item**, selecionando o **página de conteúdo de exibição do MVC** modelo, inserindo o nome Index.aspx e clicando no **adicionar** botão (consulte a Figura 3).


[![Adicionando uma página de exibição de conteúdo](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figura 03**: adicionando uma página de exibição de conteúdo ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Depois de clicar no botão Adicionar, um novo diálogo é exibida que permite que você selecione uma página de exibição mestre para associar com a página de exibição de conteúdo (consulte a Figura 4). Você pode navegar para a página de exibição mestre de Site.master que criamos na seção anterior.


[![Selecionar uma página mestra](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figura 04**: selecionar uma página mestra ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Depois de criar uma nova página de conteúdo de modo de exibição com base na página mestra Site.master, você pode obter o arquivo na listagem 2.

**A listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Observe que essa exibição contém um `<asp:Content>` marca que corresponde a cada uma da `<asp:ContentPlaceHolder>` marcas da página de exibição mestre. Cada `<asp:Content>` marca inclui um atributo ContentPlaceHolderID que aponta para a determinada `<asp:ContentPlaceHolder>` que ele substitui.

Além disso, observe que a página de exibição de conteúdo na listagem 2 não contém qualquer normal marcas de abertura e fechamento HTML. Por exemplo, ele não contém a abertura e fechamento `<html>` ou `<head>` marcas. Normal marcas de abertura e fechamento estão contidos na página modo de exibição mestre.

Qualquer conteúdo que você deseja exibir em uma página de exibição de conteúdo deve ser colocado em um `<asp:Content>` marca. Se você colocar qualquer HTML ou outro conteúdo fora essas marcas, será exibido um erro quando você tentar exibir a página.

Você não precisa substituir cada `<asp:ContentPlaceHolder>` marca de uma página mestre em uma página de exibição de conteúdo. Você precisa substituir um `<asp:ContentPlaceHolder>` quando você deseja substituir a marca com o conteúdo específico de marca.

Por exemplo, o modo de exibição do índice modificado na listagem 3 contém apenas duas `<asp:Content>` marcas. Cada uma da `<asp:Content>` marcas inclui algum texto.

**A listagem 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Quando o modo de exibição na listagem 3 é solicitado, ele renderiza a página na Figura 5. Observe que o modo de exibição renderiza uma página com duas colunas. Além disso, observe que o conteúdo da página de conteúdo de modo de exibição é mesclado com o conteúdo da página mestra exibição


[![A página de conteúdo de exibição do índice](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figura 05**: página de conteúdo do índice o modo de exibição ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modificar o conteúdo da página de exibição mestre

Um problema que você encontrar quase imediatamente ao trabalhar com páginas mestras do modo de exibição é o problema de modificar o conteúdo da exibição da página mestra quando exibir diferentes páginas de conteúdo são solicitadas. Por exemplo, convém que cada página em seu aplicativo web para ter um título exclusivo. No entanto, o título é declarado na página principal de exibição e não na página de conteúdo de exibição. Portanto, como você personalizar o título da página para cada página de exibição de conteúdo?

Há duas maneiras que você pode modificar o título exibido por uma página de exibição de conteúdo. Primeiro, você pode atribuir um título da página para o atributo de título do `<%@ page %>` diretiva declarado na parte superior de uma página de conteúdo da exibição. Por exemplo, se você deseja atribuir o título da página "Super ótimo site" para o modo de exibição do índice, você pode incluir a seguinte diretiva na parte superior do modo de exibição de índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Quando o modo de exibição do índice é renderizado no navegador, o título desejado é exibido na barra de título do navegador:


[![Barra de título do navegador](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Há um requisito importante que uma página de modo de exibição mestre deve atender para que o atributo title trabalhar. A página de exibição mestre deve conter um `<head runat="server">` marca em vez de um normal `<head>` marca para seu cabeçalho. Se o `<head>` marca não inclui o runat = atributo "server", em seguida, o título não aparecerá. O modo de exibição padrão página mestra inclui necessários `<head runat="server">` marca.

Uma abordagem alternativa para modificar o conteúdo da página mestra em uma página de conteúdo da exibição individual é encapsular a região que você deseja modificar em um `<asp:ContentPlaceHolder>` marca. Por exemplo, imagine que você deseja alterar não apenas o título, mas também as marcas meta, processadas por uma página de modo de exibição mestre. A página de modo de exibição mestre na listagem 4 contém um `<asp:ContentPlaceHolder>` marca dentro de seu `<head>` marca.

**A listagem 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Observe que o `<asp:ContentPlaceHolder>` marca na listagem 4 inclui conteúdo padrão: um título padrão e marcas de metadados padrão. Se você não substituir isso `<asp:ContentPlaceHolder>` marca em uma página de conteúdo de exibição individual, o conteúdo padrão será exibido.

A página de exibição de conteúdo na listagem 5 substitui o `<asp:ContentPlaceHolder>` marca para exibir um título personalizado e marcas meta personalizadas.

**Listando 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Resumo

Neste tutorial você forneceu uma introdução básica para exibir páginas mestras e páginas de conteúdo. Você aprendeu a criar nova exibição páginas mestras e criar páginas de conteúdo do modo de exibição com base neles. Também examinamos como você pode modificar o conteúdo de uma página mestra do modo de exibição de uma página de conteúdo do modo de exibição específico.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Próximo](passing-data-to-view-master-pages-cs.md)
