---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Usando o Inspetor de página para Visual Studio 2012 no ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: O Inspetor de página para Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado. Selecione qualquer elemento no navegador integrado e o Inspetor de página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: d2c377f8466f8f324b75ce60860aa00c11bc0ffe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825088"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Usando o Inspetor de página para Visual Studio 2012 em Web Forms do ASP.NET
====================
por Tim Ammann

> O Inspetor de página para Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado. Selecione qualquer elemento no navegador integrado e Inspetor de página instantaneamente destaca o código-fonte e o CSS do elemento. Você pode procurar qualquer página em seu aplicativo rapidamente encontrar as fontes de marcação renderizada e usar as ferramentas de navegador dentro do ambiente do Visual Studio.
> 
> Este tutorial shwos como habilitar o modo de inspeção e, em seguida, rapidamente localizar e editar regras de CSS e texto dentro de seu projeto da web. O tutorial usa um projeto de aplicativo Web Forms, mas você também pode usar o Inspetor de página para projetos de Site e [MVC](https://go.microsoft.com/?linkid=9802002) aplicativos.
> 
> O tutorial tem as seguintes seções:
> 
> [Pré-requisitos](#_1_prerequisites)
> 
> [Criar um aplicativo Web](#_2_creating_a)
> 
> [Usar o Inspetor de página para exibir o aplicativo](#_3_using_page)
> 
> [Habilitar o modo de inspeção](#_4_inspection_mode)
> 
> [Use o Inspetor de página para fazer alterações à marcação](#_5_using_page)
> 
> [Modo de inspeção e a janela do HTML](#_6_inspection_mode)
> 
> [Visualizar alterações CSS na janela estilos](#_7_previewing_css)
> 
> [Sincronização automática CSS](#css_auto_sync)
> 
> [Usando o seletor de cor CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obter a versão mais recente do Inspetor de página, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Azure para .NET 2.0.


O Page Inspector é fornecido com o Microsoft Web Developer Tools. A versão mais recente é 1.3. Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre o Microsoft Visual Studio** da **ajudar** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Criar um aplicativo Web

Primeiro, você criará um aplicativo web que você usará com o Inspetor de página. No Visual Studio, escolha **arquivo** &gt; **novo projeto**. À esquerda, expanda **Visual c#**, selecione **Web**e, em seguida, selecione **aplicativo ASP.NET Web Forms**.

![Novo aplicativo de formulários da Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Clique em **OK**.

O aplicativo é aberto no **origem** modo de exibição.

![Novo aplicativo de formulários da Web no modo de exibição de fonte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Agora que você tem um aplicativo para trabalhar com, você pode usar o Inspetor de página para examinar e modificá-lo.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Usar o Inspetor de página para exibir o aplicativo

Em seguida, você exibirá o aplicativo com o Page Inspector. Na **Gerenciador de soluções**, com o botão direito do mouse no projeto e, em seguida, escolha **exibir no Page Inspector**.

![Exibir no Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Por padrão, quando o Inspetor de página é iniciado pela primeira vez, ele é encaixado como uma janela estreita no lado esquerdo do ambiente do Visual Studio. Deixe-encaixada no lado esquerdo e defina-o como uma largura que se sente confortável para você ou encaixá-la em uma das áreas de ferramenta sobre a parte superior, inferior ou direita:

![Posições de encaixe do Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Se você desencaixar a janela do Inspetor de página, você pode colocar a ele fora do Visual Studio, ou até mesmo em um segundo monitor se você tiver uma. No entanto, em ordem para ALT + TAB entre o Inspetor de página e o Visual Studio quando a janela do Inspetor de página estiver desencaixada, vá para **ferramentas** &gt; **opções** &gt;  **Ambiente** &gt; **guias e Windows**e, em **guia bem**, desmarque a caixa de seleção chamada **janelas de ferramentas flutuante permanecem sempre na parte superior das janela principal**:

![Desmarque a caixa de seleção de windows ferramenta flutuante para ALT + TAB entre o Visual Studio e a janela Inspetor de página desencaixada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

O painel superior da janela do Inspetor de página mostra a página atual em uma janela do navegador. O painel inferior mostra a página na marcação HTML à esquerda e algumas guias à direita que permitem que você inspecione os diferentes aspectos da página. O painel inferior é semelhante para o [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer. (No entanto, diferentemente das ferramentas de desenvolvedor, você pode usar o Inspetor de página dentro do Visual Studio.)

![Inspetor de Página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Neste tutorial, você usará o painel do navegador Inspetor de página e o **HTML** e **estilos** guias para ajudar você a rapidamente navegar e fazer alterações no aplicativo.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Habilitar o modo de inspeção

Em seguida, você verá como funciona o modo de inspeção do Inspetor de página. Na janela do Inspetor de página, clique o **inspecionar** botão.

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Para ver o modo de inspeção em ação, mova o mouse sobre diferentes partes da página dentro da janela do navegador Inspetor de página. Como você, o ponteiro do mouse muda para um grande sinal de adição e o elemento abaixo é realçado:

![Passar o mouse sobre o wrapper div.content](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Observe que, à medida que você move o ponteiro do mouse,

- O conteúdo no **origem** exibir muda para mostrar a marcação correspondente ao elemento selecionado na página. A marcação relevante é realçada. Se a fonte estiver em outro arquivo, esse arquivo é aberto no modo de exibição de código-fonte com a marcação relevante realçada.

- A marcação exibida na **HTML** guia no Inspetor de página também será alterado para corresponder ao elemento selecionado na página. No **HTML** guia, a marcação relevante é descrita.

- O **estilos** guia mostra as regras de CSS relevantes para a seleção atual.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Use o Inspetor de página para fazer alterações à marcação

Agora, você verá como você pode usar o Inspetor de página para encontrar e fazer alterações na marcação ou no texto cujo local pode não ser imediatamente óbvio.

Colocar o Inspetor de página no modo de inspeção e, em seguida, role até a parte inferior da página inicial.

Assim que você inserir a área do rodapé, o Inspetor de página abre o *Master* arquivo de layout na **origem** exibição em uma guia temporária para a direita das outras guias e realça a seção do mestre de página que você selecionou. Isso mostra como o Inspetor de página pode localizar e exibir o conteúdo em uma página que, na verdade, pode vir de um arquivo diferente daquele que você tiver aberto originalmente.

![Destaques de rodapé no modo de inspeção](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Na janela do navegador Inspetor de página, mova o ponteiro do mouse sobre a linha com os direitos autorais <a id="a"> </a>Observe.

No *Master* página, a linha correspondente é realçada.

![Linha de direitos autorais do rodapé realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Adicione algum texto ao final da linha na *Master* arquivo.

&lt;p&gt;&amp;copiarem; &lt;%: % DateTime.Now.Year&gt; -é o máximo meu aplicativo ASP.NET!&lt; / p&gt;

Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador Inspetor de página.

![É o máximo meu aplicativo ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Você deve ter imaginado se o rodapé foi na *default. aspx* página, mas ela tornou-se na página de layout mestre e o Inspetor de página encontrado-lo para você.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspeção e a janela do HTML

Em seguida, você terá uma olhada rápida na janela do HTML e como ele mapeia elementos para você.

Colocar o Inspetor de página no modo de inspeção.

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Clique na parte superior da página que diz "seu logotipo aqui". Você está examinando um determinado elemento mais detalhadamente, portanto, a exibição na janela do navegador não é alterado à medida que você move o ponteiro do mouse.

Agora mova o ponteiro do mouse para o **HTML** janela. À medida que você move o ponteiro do mouse, o Inspetor de página descreve o elemento dentro de **HTML** janela e realça o elemento correspondente na janela do navegador.

![Janela do HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Como antes, o Inspetor de página abre o *Master* arquivo para você em uma guia temporária. Clique na guia Master e a marcação correspondente é realçada na &lt;cabeçalho&gt; seção:

![Marcação realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Visualizar alterações CSS na janela estilos

Em seguida, você verá como é possível usar o Inspetor de página **estilos** janela para visualizar as alterações à CSS.

Clique o **inspecionar** botão para colocar o Inspetor de página no modo de inspeção.

Na janela do navegador Inspetor de página, mova o ponteiro do mouse sobre a seção de "Home Page" até que o **div.content wrapper** rótulo aparece.

![Focalizar os elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Clique dentro da seção de wrapper div.content uma vez e, em seguida, mova o ponteiro do mouse para o **estilos** janela. Sob o seletor de classe de wrapper. Content .featured, desmarque e marque a caixa de seleção para a propriedade de cor do plano de fundo.

![Cor do plano de fundo clara](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Observe como a alteração visualiza instantaneamente na janela do navegador Inspetor de página.

Selecione a caixa de seleção novamente, em seguida, clique duas vezes o valor da propriedade e alterá-la para `red`. A alteração mostra imediatamente:

![Cor do plano de fundo vermelho](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

O **estilos** torna a janela fácil de testar e visualizar o CSS é alterado antes de confirmar as alterações para o estilo da folha em si.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronização automática CSS

> [!NOTE]
> Este recurso requer a versão 1.3 do Inspetor de página.


O recurso de sincronização automática de CSS permite que você editar um arquivo CSS diretamente e ver as alterações imediatamente no navegador Inspetor de página.

Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.

No navegador Inspetor de página, mova o ponteiro do mouse sobre a seção de "Home Page" até que o **div.content wrapper** rótulo aparece. Clique para selecionar esse elemento.

O **estilos** janela mostra todas as regras CSS para este elemento. Role para baixo até localizar o wrapper de. Content .featured seletor de classe. Clique em .featured. Content-"wrapper". Inspetor de página abre o arquivo CSS que define esse estilo (CSS) e realça o estilo CSS correspondente.

![Arquivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Agora, altere o valor para `background-color` para "vermelho". A alteração aparece imediatamente no navegador Inspetor de página.

![Navegador Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Usando o seletor de cor CSS

Em seguida, você aprenderá como usar o Inspetor de página para localizar rapidamente e alterar o CSS do texto realçado no aplicativo padrão. Neste exemplo, você decidiu que não deseja que o realce azul e quiser alterá-lo para outra cor.

Clique o **inspecionar** botão.

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Na janela do navegador Inspetor de página, mova o ponteiro do mouse sobre o realçado "vídeos, tutoriais e exemplos de" texto para que o CSS "marcar" rótulo é exibido.

![Passar o mouse sobre o elemento de marca](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Clique no texto para selecioná-lo. O seletor de marca de CSS correspondente é exibida na parte inferior a **estilos** janela.

![seletor de marca na janela estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Clique no seletor de marca. Isso abre o *CSS* arquivo para o aplicativo web. Clique na guia de CSS, e o CSS correspondente para o seletor é realçado:

![seletor de marca na folha de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Selecione e remova a linha com a propriedade de cor do plano de fundo.

Agora você usará o novo seletor de cor CSS de 2012 do Visual Studio para escolher uma nova cor para o **marcar** propriedade de cor do plano de fundo.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Usando o seletor de cor de CSS do Visual Studio 2012

O editor de CSS no Visual Studio 2012 tem um seletor de cores que torna mais fácil escolher e inserir as cores. Ele tem uma barra de cores simple e um seletor de "pop-down" que oferece controle mais refinado.

O seletor de cor inclui uma paleta de cores padrão, dá suporte a nomes de cores padrão, os códigos de hash, as cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores usados mais recentemente no documento.

Na linha em que a propriedade de cor do plano de fundo estava, digite "bc" e pressione a seta para baixo uma vez.

Quando você digita o primeiro caractere de cada palavra em uma propriedade separados por hífen, como "background-color", o IntelliSense filtra a lista para mostrar apenas as propriedades que correspondem a:

![Valores de IntelliSense filtrado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Agora, digite dois-pontos. Quando você fizer isso, o nome da propriedade de cor de plano de fundo completa é inserido. Tipo de **#** ou **rgb (**, e a barra de seletor de cor é exibida:

![A barra de seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Para ver como funciona a barra do seletor de cores, clique em suas cores com o ponteiro do mouse, ou pressione a tecla de seta para baixo e, em seguida, use as teclas de seta esquerda e direita para percorrer as cores. Quando você visita uma cor, o valor correspondente para a propriedade de cor do plano de fundo é visualizado:

![valor da propriedade de cor do plano de fundo visualizado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

Neste ponto, você pode pressionar Enter para selecionar o valor e, em seguida, um ponto e vírgula (;) para concluir a entrada CSS. Por enquanto, vá para a próxima seção para que você possa ver como funciona o pop-down de seletor de cor.

#### <a name="using-the-color-picker-pop-down"></a>Usando o seletor de cor Pop-Down

Quando a barra de cores não tem a cor exata que você está procurando, você pode usar o seletor de cores pop-down.

Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores, ou pressione a seta para baixo uma ou duas vezes no teclado.

![Seletor de cor CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Clique em uma cor da barra de ferramentas vertical no lado direito. Isso mostra uma gradação de cor na janela principal. Escolha uma cor diretamente na barra vertical, pressionando Enter ou clique em qualquer ponto na janela principal, escolha com maior precisão.

Se houver uma cor na tela do computador que você deseja usar (ele não precisa estar dentro da interface de usuário do Visual Studio), você pode capturar o seu valor usando a ferramenta de conta-gotas no canto inferior direito.

Você também pode alterar a opacidade de uma cor movendo o controle deslizante na parte inferior do seletor de cores. Isso muda a cor valores para os valores RGBA porque o formato de RGBA pode representar a opacidade.

Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor do plano de fundo na *CSS* arquivo.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>A barra de atualização do Inspetor de página

Inspetor de página imediatamente detecta a alteração para o *CSS* arquivo (ou em qualquer arquivo no aplicativo) e exibe um alerta em uma barra de atualização.

![Barra de atualização](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Para salvar todos os seus arquivos e atualize o navegador Inspetor de página, pressione Ctrl + Alt + Enter ou clique na barra de atualização. A alteração na cor de realce é exibida no navegador:

![Cor de realce alterado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Observe que você atualizou convenientemente navegador Inspetor de página à direita de dentro do ambiente do Visual Studio. Usando o Inspetor de página em vez de um navegador externo permite que você fique no editor quando você desenvolve seus aplicativos web.

## <a name="see-also"></a>Consulte também

[Introdução ao Inspetor de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo do Channel 9)

[Mensagens de erro do Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
