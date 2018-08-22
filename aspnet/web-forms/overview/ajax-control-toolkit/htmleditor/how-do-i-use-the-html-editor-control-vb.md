---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Como usar o controle de Editor de HTML? (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor é um controle do AJAX ASP.NET que permite facilmente criar e editar o conteúdo HTML por meio de botões em uma barra de ferramentas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 11d9251644f1daf4257e1bfa3c9405fc0c46a5d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824403"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>Como usar o controle de Editor de HTML? (VB)
====================
por [Microsoft](https://github.com/microsoft)

> HTMLEditor é um controle do AJAX ASP.NET que permite facilmente criar e editar o conteúdo HTML por meio de botões em uma barra de ferramentas.


O objetivo deste tutorial é fornecer uma visão geral do controle de Editor de HTML incluído com o AJAX Control Toolkit. O Editor de HTML inclui opções para alterar o tamanho da fonte, selecionando uma fonte, alterando a cor do plano de fundo, modificando a cor de primeiro plano, adicionando links, a adição de imagens, alterando o alinhamento de texto e execução de recortar, copiar e colar operações (veja a Figura 1).


[![O Editor de HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: O Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


O editor de HTML permite que você inserir o conteúdo usando um modo de design, ou você pode inserir HTML diretamente. Você também é fornecidas com a opção de visualizar o conteúdo HTML (consulte a Figura 2).


[![Design, HTML e visualização de botões](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: botões de Design, HTML e Preview ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


Neste tutorial, você aprenderá como exibir o Editor de HTML, como personalizar os botões de barra de ferramentas que aparecem no Editor de HTML e como evitar ataques de scripts entre sites.

## <a name="displaying-the-html-editor"></a>Exibindo o Editor de HTML

Antes de usar o Editor de HTML em uma página ASP.NET, você primeiro deve adicionar um controle ScriptManager à página. O controle do ScriptManager está localizado abaixo da guia Extensões AJAX na caixa de ferramentas Visual Studio/Visual Web Developer Express.

Você deve colocar o controle ScriptManager na parte superior da página antes de quaisquer outros controles na página. Por exemplo, você pode colocá-lo imediatamente abaixo do lado do servidor abrindo &lt;formulário&gt; marca.

O controle de Editor de HTML está localizado na caixa de ferramentas com o restante dos controles do AJAX Control Toolkit. Ele é chamado de controle do Editor (veja a Figura 3).


[![O controle de Editor de HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: controle o Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Depois que você arrasta o Editor de HTML para uma página, você pode definir suas propriedades na folha de propriedades. Por exemplo, normalmente você deseja definir as propriedades Width e Height. Listagem 1 contém o código-fonte para uma página ASP.NET que contém um editor de HTML.

**Listagem 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

A página na listagem 1 contém um controle de Editor de HTML, um controle de botão e um controle Literal. Quando você clica no botão, o conteúdo do Editor de HTML aparece no controle Literal (veja a Figura 4).


[![Enviando um formulário com um Editor de HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: enviando um formulário com um Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


A propriedade de conteúdo do Editor de HTML é usada para recuperar o conteúdo HTML inserido no Editor de HTML. Lembre-se de que esse conteúdo HTML pode conter JavaScript. Na próxima seção, discutiremos como você pode impedir ataques de injeção de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizando a barra de ferramentas do Editor de HTML

É possível personalizar exatamente quais botões aparecem no editor. Por exemplo, você talvez queira remover da guia HTML para impedir que os usuários alternem o Editor de HTML para o modo HTML. Ou, você talvez queira remover a lista de lista suspensa de tamanho da fonte para impedir que os usuários criem excessivamente grande de texto em um Fórum da mensagem post (consulte a Figura 5).


[![Um Editor de HTML personalizado](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: personalizado de um Editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Você pode personalizar os botões da barra de ferramentas ao derivar um novo Editor de HTML da classe base do Editor. Por exemplo, o editor personalizado na listagem 2 contém apenas os botões da barra de negrito e itálico. Todos os outros botões da barra de ferramentas foram removidos. Além disso, na guia HTML foi removida da parte inferior do editor (mas as guias de Design e visualização ainda estão lá).

**Listagem 2 - aplicativo\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Você deve adicionar a classe na listagem 2 ao seu aplicativo\_pasta de código para que a classe será compilada automaticamente. Se o aplicativo\_pasta de código não existe no seu site e em seguida, você pode simplesmente adicionar a pasta.

Depois de criar um editor personalizado, você pode adicioná-lo a uma página ASP.NET da mesma forma que você adicione o Editor de HTML normal (consulte a listagem 3).

**Listagem 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitando ataques de Cross-Site XSS (script)

Sempre que você aceita a entrada do usuário e exibir novamente a entrada em seu site, você potencialmente abrir seu site a ataques de scripts entre sites (XSS). Em teoria, um hacker mal-intencionado poderia enviar código JavaScript que é executado quando a entrada é exibida novamente. O JavaScript pode ser usado para roubar senhas de usuário ou outras informações confidenciais.

Normalmente, você pode anular ataques XSS de codificação qualquer entrada que você se recuperar de um usuário antes de exibi-lo em uma página da web HTML. No entanto, a saída do Editor de HTML de codificação HTML não apenas codificaria &lt;script&gt; marcas, ele também deve codificar todas as marcas HTML. Em outras palavras, você perderia toda a formatação, como o tipo de fonte, tamanho da fonte e cor do plano de fundo.

Se você estiver coletando informações confidenciais de seus usuários, como senhas, números de cartão de crédito e números do seguro social - você não deve exibir o conteúdo não codificado recuperados de um usuário com o Editor de HTML. Você deve usar o Editor de HTML somente em situações em que você não exibir novamente o conteúdo HTML, ou o conteúdo HTML que está sendo enviado para seu site por uma parte confiável.

Por exemplo, imagine que você está criando um aplicativo de blog. Nessa situação, faz sentido usar o Editor de HTML ao redigir postagens de blog. Você é o único que envia uma postagem de blog e, presumivelmente, você pode confiar em você mesmo para não enviar JavaScript mal-intencionado. No entanto, ele não faz sentido usar o Editor de HTML ao permitir que usuários anônimos postar comentários. Você deve ter cuidado especialmente em situações em que os usuários enviam informações confidenciais como senhas. Potencialmente, um usuário mal-intencionado poderia postar um comentário que contém o JavaScript certo para roubo de uma senha.

## <a name="summary"></a>Resumo

Neste tutorial, você foram fornecidos com uma breve visão geral do controle de Editor de HTML incluído no AJAX Control Toolkit. Você aprendeu a usar o Editor de HTML para aceitar o conteúdo avançado de um usuário e enviar o conteúdo para o servidor. Também discutimos como você pode personalizar os botões de barra de ferramentas que são exibidos pelo Editor de HTML. Por fim, você aprendeu a evitar ataques de scripts entre sites ao usar o Editor de HTML para aceitar a entrada potencialmente mal-intencionado.

> [!div class="step-by-step"]
> [Anterior](how-do-i-use-the-html-editor-control-cs.md)
