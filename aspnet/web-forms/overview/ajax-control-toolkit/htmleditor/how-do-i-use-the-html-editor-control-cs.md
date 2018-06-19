---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Como usar o controle de Editor de HTML (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor é um controle de AJAX do ASP.NET que permite que você facilmente criar e editar conteúdo HTML por meio de botões na barra de ferramentas.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fca18948c0e4f1323f214dc0033f19fa44efad47
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879771"
---
<a name="how-do-i-use-the-html-editor-control-c"></a>Como usar o controle de Editor de HTML (C#)
====================
por [Microsoft](https://github.com/microsoft)

> HTMLEditor é um controle de AJAX do ASP.NET que permite que você facilmente criar e editar conteúdo HTML por meio de botões na barra de ferramentas.


O objetivo deste tutorial é fornecer uma visão geral do controle de Editor de HTML incluído com o Kit de ferramentas de controle AJAX. O Editor de HTML inclui opções para alterar o tamanho da fonte, selecionando uma fonte, alterando a cor de plano de fundo, modificando a cor de primeiro plano, adicionando links, a adição de imagens, alterando o alinhamento de texto e execução de recortar, copiar e colar operações (consulte a Figura 1).


[![O Editor de HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Figura 01**: O Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


O editor de HTML permite que você inserir o conteúdo usando um modo de design, ou você pode inserir HTML diretamente. Também são fornecidas com a opção para visualizar o conteúdo HTML (consulte a Figura 2).


[![Design, HTML e visualização de botões](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Figura 02**: Design, HTML e visualização botões ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


Neste tutorial, você aprenderá como exibir o Editor de HTML, como personalizar os botões de barra de ferramentas que aparecem no Editor de HTML e como evitar ataques de scripts entre sites.

## <a name="displaying-the-html-editor"></a>Exibir o Editor de HTML

Antes de usar o Editor de HTML em uma página ASP.NET, primeiro adicione um controle ScriptManager à página. O controle ScriptManager está localizado sob a guia Extensões AJAX na caixa de ferramentas Visual Studio/Visual Web Developer Express.

Você deve colocar o controle ScriptManager na parte superior da página antes de quaisquer outros controles na página. Por exemplo, você pode colocá-lo imediatamente abaixo do lado do servidor abertura &lt;formulário&gt; marca.

O controle de Editor de HTML está localizado na caixa de ferramentas com o restante dos controles AJAX Control Toolkit. Ele é chamado de controle de Editor (consulte a Figura 3).


[![O controle de Editor de HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Figura 03**: controle o Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


Depois que você arrasta o Editor de HTML para uma página, você pode definir suas propriedades na folha de propriedades. Por exemplo, normalmente você deseja definir as propriedades de altura e largura. Listar 1 contém a fonte de uma página ASP.NET que contém um editor de HTML.

**Listando 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

A página na listagem 1 contém um controle de Editor de HTML, um controle de botão e um controle Literal. Quando você clica no botão, o conteúdo do Editor de HTML exibido no controle Literal (consulte a Figura 4).


[![Enviar um formulário com um Editor de HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Figura 04**: enviar um formulário com um Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


A propriedade de conteúdo do Editor de HTML é usada para recuperar o conteúdo HTML inserido no Editor de HTML. Lembre-se de que esse conteúdo HTML pode conter JavaScript. Na próxima seção, discutiremos como você pode impedir ataques de injeção de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizando a barra de ferramentas do Editor de HTML

Você pode personalizar exatamente quais botões devem aparecer no editor. Por exemplo, você talvez queira remover guia HTML para impedir que os usuários de alternar o Editor de HTML no modo de HTML. Ou, talvez você queira remover a lista suspensa tamanho da fonte para impedir que usuários criem texto excessivamente grande em um Fórum da mensagem post (consulte a Figura 5).


[![Um Editor de HTML personalizado](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Figura 05**: personalizadas de um Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Você pode personalizar os botões da barra de ferramentas por um novo Editor de HTML derivam da classe base do Editor. Por exemplo, o editor personalizado na listagem 2 só contém botões de barra de ferramentas para negrito e itálico. Todos os outros botões da barra de ferramentas foram removidos. Além disso, a guia HTML foi removida da parte inferior do editor (mas as guias de Design e visualização ainda existem).

**A listagem 2 - aplicativo\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Você deve adicionar a classe na lista 2 para seu aplicativo\_pasta de código para que a classe será compilada automaticamente. Se o aplicativo\_código pasta não existir no seu site, você pode simplesmente adicionar a pasta.

Depois de criar um editor personalizado, você pode adicioná-lo a uma página ASP.NET da mesma maneira que você adicione o Editor de HTML normal (consulte a listagem 3).

**A listagem 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitando ataques de script entre sites (XSS)

Sempre que você aceita a entrada de um usuário e exibir novamente essa entrada no seu site, você possivelmente abrir seu site a ataques de scripts entre sites (XSS). Em teoria, um hacker mal-intencionado pode enviar o código JavaScript que é executado quando a entrada é exibida novamente. O JavaScript pode ser usado para roubar senhas de usuário ou outras informações confidenciais.

Normalmente, você pode anular ataques XSS de codificação qualquer entrada recuperar de um usuário antes de exibi-la em uma página da web HTML. No entanto, a saída do Editor de HTML de codificação HTML não seria apenas codificar &lt;script&gt; marcas, ele também deve codificar todas as marcas HTML. Em outras palavras, você perderia toda a formatação, como o tipo de fonte, tamanho da fonte e cor do plano de fundo.

Se você estiver coletando informações confidenciais de seus usuários — como senhas, números de cartão de crédito e números de previdência social - você não deve exibir o conteúdo não codificado recuperados de um usuário com o Editor de HTML. Você deve usar o Editor de HTML somente em situações em que você não exibir novamente o conteúdo HTML, ou o conteúdo HTML que está sendo enviado ao seu site por um terceiro confiável.

Por exemplo, imagine que você está criando um aplicativo de blog. Nessa situação, faz sentido usar o Editor de HTML ao compor postagens de blog. São a única pessoa que envia uma postagem de blog e, provavelmente, você pode confiar por conta própria para não enviar JavaScript mal-intencionados. No entanto, ele não faz sentido usar o Editor de HTML quando eles tiverem permissão para enviar comentários. Tenha cuidado especialmente em situações em que os usuários enviam informações confidenciais, como senhas. Potencialmente, um usuário mal-intencionado pode lançar um comentário que contém o JavaScript direito para roubar uma senha.

## <a name="summary"></a>Resumo

Neste tutorial, você foram fornecidos com uma breve visão geral do controle de Editor de HTML incluído no Kit de ferramentas de controle AJAX. Você aprendeu a usar o Editor de HTML para aceitar conteúdo formatado de um usuário e enviar o conteúdo para o servidor. Também discutimos como você pode personalizar os botões de barra de ferramentas que são exibidos pelo Editor de HTML. Por fim, você aprendeu como evitar ataques de scripts entre sites ao usar o Editor de HTML para aceitar a entrada potencialmente mal-intencionada.

> [!div class="step-by-step"]
> [Avançar](how-do-i-use-the-html-editor-control-vb.md)
