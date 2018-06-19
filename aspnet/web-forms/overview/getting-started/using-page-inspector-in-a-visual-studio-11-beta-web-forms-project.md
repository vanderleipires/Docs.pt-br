---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Com o Page Inspector do Visual Studio 2012 no ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Inspetor de página para o Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado. Selecione qualquer elemento no integrado do navegador e Inspetor de página...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28040321"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Com o Page Inspector para Visual Studio 2012 em Web Forms do ASP.NET
====================
por Tim Ammann

> Inspetor de página para o Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado. Selecione qualquer elemento no navegador integrado e o Page Inspector instantaneamente realça o CSS e o código-fonte do elemento. Você pode procurar qualquer página em seu aplicativo rapidamente localizar as fontes de marcação renderizada e usar as ferramentas de navegador à direita no ambiente do Visual Studio.
> 
> Este tutorial shwos como habilitar o modo de inspeção e, em seguida, localize rapidamente e editar regras de CSS e texto dentro de seu projeto da web. O tutorial usa um projeto de aplicativo Web Forms, mas você também pode usar o Inspetor de página para projetos de Site e [MVC](https://go.microsoft.com/?linkid=9802002) aplicativos.
> 
> O tutorial tem as seguintes seções:
> 
> [Pré-requisitos](#_1_prerequisites)
> 
> [Criar um aplicativo Web](#_2_creating_a)
> 
> [Use o Inspetor de página para exibir o aplicativo](#_3_using_page)
> 
> [Habilitar o modo de inspeção](#_4_inspection_mode)
> 
> [Use o Inspetor de página para fazer as alterações de marcação](#_5_using_page)
> 
> [Modo de inspeção e a janela de HTML](#_6_inspection_mode)
> 
> [Visualizar alterações CSS na janela estilos](#_7_previewing_css)
> 
> [Sincronização automática de CSS](#css_auto_sync)
> 
> [Usando o seletor de cores CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos

- [O Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obter a versão mais recente do Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Azure para .NET 2.0.


O Page Inspector é fornecido com o Microsoft Web Developer Tools. A versão mais recente é 1.3. Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre o Microsoft Visual Studio** do **ajuda** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Criar um aplicativo Web

Primeiro, você criará um aplicativo web que você usará com o Inspetor de página. No Visual Studio, escolha **arquivo** &gt; **novo projeto**. À esquerda, expanda **Visual C#**, selecione **Web**e, em seguida, selecione **aplicativo ASP.NET Web Forms**.

![Novo aplicativo de formulários da Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Clique em **OK**.

O aplicativo é aberto na **fonte** exibição.

![Novo aplicativo de formulários da Web no modo de exibição de fonte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Agora que você tenha um aplicativo para trabalhar com, você pode usar o Inspetor de página para examinar e modificá-lo.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Use o Inspetor de página para exibir o aplicativo

Em seguida, você exibirá o aplicativo com o Page Inspector. Em **Solution Explorer**, clique com botão direito no projeto e escolha **exibir em Inspetor de página**.

![Exibir em Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Por padrão, quando o Page Inspector é iniciado pela primeira vez, ela estiver encaixada como uma janela específica no lado esquerdo do ambiente do Visual Studio. Deixe o campo encaixado à esquerda e defina-a como uma largura que confortável para você ou encaixá-la em uma das áreas de ferramenta sobre a parte superior, inferior ou direita:

![Posições de encaixe do Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Se você desencaixar janela Inspetor de página, você pode colocar a ele fora do Visual Studio, ou até mesmo em um segundo monitor se você tiver um. No entanto, em ordem para ALT + TAB entre o Inspetor de página e o Visual Studio quando a janela do Page Inspector é desencaixada, vá para **ferramentas** &gt; **opções** &gt;  **Ambiente** &gt; **janelas e guias**e, em **guia bem**, desmarque a caixa de seleção chamada **janelas de ferramentas flutuante permanecem sempre na parte superior das janela principal**:

![Desmarque a caixa de seleção de windows ferramenta flutuante para ALT + TAB entre o Visual Studio e a janela do Page Inspector desencaixada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

O painel superior da janela do Page Inspector mostra a página atual em uma janela do navegador. O painel inferior mostra a página na marcação HTML à esquerda, e alguns guias à direita que permitem a você inspecionar diferentes aspectos da página. O painel inferior é semelhante do [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer. (No entanto, ao contrário das ferramentas de desenvolvedor, você pode usar o Page Inspector dentro do Visual Studio.)

![Inspetor de Página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Neste tutorial, você usará o painel do navegador do Inspetor de página e o **HTML** e **estilos** guias para ajudá-lo rapidamente navegar e fazer alterações no aplicativo.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Habilitar o modo de inspeção

Em seguida, você verá como funciona o modo de inspeção do Inspetor de página. Na janela do Inspetor de página, clique o **inspecionar** botão.

![Inspecione o elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Para ver o modo de inspeção em ação, mova o mouse sobre diferentes partes da página na janela do navegador do Inspetor de página. Como é feito, o ponteiro do mouse muda para um sinal de adição grande e o elemento abaixo é realçado:

![Focalizar div.content wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Observe que, assim que você move o ponteiro do mouse,

- O conteúdo **fonte** exibir muda para mostrar a marcação correspondente ao elemento selecionado na página. A marcação relevante é realçada. Se a origem estiver em outro arquivo, esse arquivo é aberto no modo de exibição de fonte com a marcação relevante realçada.

- A marcação exibida no **HTML** guia no Inspetor de página também será alterado para corresponder ao elemento selecionado na página. No **HTML** guia, a marcação relevante é descrita.

- O **estilos** guia mostra as regras de CSS relevantes para a seleção atual.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Use o Inspetor de página para fazer as alterações de marcação

Agora, você verá como você pode usar o Inspetor de página para localizar e fazer alterações em marcação ou o texto cujo local pode não ser imediatamente óbvio.

Coloque o Inspetor de página no modo de inspeção e, em seguida, role até a parte inferior da página inicial.

Assim que você inserir a área do rodapé, Page Inspector abre o *Site.Master* arquivo layout em **fonte** exibição em uma guia temporária à direita das outras guias e realça a seção do mestre de página que você selecionado. Isso mostra como o Inspetor de página podem localizar e exibir o conteúdo em uma página que realmente pode vir de um arquivo diferente daquele que você abriu originalmente.

![Destaques de rodapé no modo de inspeção](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre a linha com os direitos autorais <a id="a"> </a>Observe.

No *Site.Master* página, a linha correspondente é realçada.

![Linha de direitos autorais do rodapé realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Adicionar texto ao final da linha de *Site.Master* arquivo.

&lt;p&gt;&amp;copiar; &lt;%: % DateTime.Now.Year&gt; -meu Rocks do aplicativo ASP.NET!&lt; / p&gt;

Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador do Inspetor de página.

![Meu Rocks do aplicativo ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Você pode considerar que o rodapé estava no *Default.aspx* página, mas ele tornou-se na página de layout mestre e o Page Inspector encontrado-lo para você.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspeção e a janela de HTML

Em seguida, você terá uma visão geral de janela HTML e como ele mapeia elementos para você.

Coloque o Inspetor de página no modo de inspeção.

![Inspecione o elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Clique na parte superior da página que diz "seu logotipo aqui". Se estiver examinando um determinado elemento mais detalhadamente, para que a exibição na janela do navegador não muda à medida que você move o ponteiro do mouse.

Agora mova o ponteiro do mouse para o **HTML** janela. Assim que você move o ponteiro do mouse, o Page Inspector descreve o elemento dentro do **HTML** janela e realça o elemento correspondente na janela do navegador.

![Janela de HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Como antes, o Inspetor de página abre o *Site.Master* arquivo para você em uma guia temporário. Clique na guia Site.Master e a marcação correspondente é realçada no &lt;cabeçalho&gt; seção:

![Marcação realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Visualizar alterações CSS na janela estilos

Em seguida, você verá como é possível usar o Page Inspector **estilos** janela para visualizar as alterações de CSS.

Clique o **inspecionar** botão para colocar o Inspetor de página no modo de inspeção.

Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre a seção "Home Page" até que o **div.content wrapper** rótulo aparece.

![Focalizar os elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Clique dentro da seção de wrapper div.content uma vez e, em seguida, mova o ponteiro do mouse para o **estilos** janela. Sob o seletor de classe de invólucro. Content .featured, desmarque e marque a caixa de seleção para a propriedade de cor de plano de fundo.

![Cor de plano de fundo claro](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Observe como a alteração imediatamente visualiza na janela do navegador do Inspetor de página.

Selecione a caixa de seleção novamente, em seguida, clique duas vezes o valor da propriedade e alterá-la para `red`. A alteração mostra imediatamente:

![Cor do plano de fundo vermelho](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

O **estilos** torna-o fácil de testar e visualizar CSS alterações antes de confirmar as alterações para o estilo a janela da folha em si.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronização automática de CSS

> [!NOTE]
> Este recurso requer a versão 1.3 do Inspetor de página.


O recurso de sincronização automática de CSS permite que você editar um arquivo CSS diretamente e ver as alterações imediatamente no navegador do Inspetor de página.

Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.

No navegador do Inspetor de página, mova o ponteiro do mouse na seção "Home Page" até que o **div.content wrapper** rótulo aparece. Clique para selecionar esse elemento.

O **Syles** janela mostra todas as regras CSS para este elemento. Role para baixo até localizar o wrapper de. Content .featured seletor de classe. Clique em ".featured. Content-wrapper". Inspetor de página abre o arquivo CSS que define esse estilo (site) e realça o estilo CSS correspondente.

![Arquivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Agora, altere o valor de `background-color` para "vermelho". A alteração aparece imediatamente no navegador do Inspetor de página.

![Navegador do Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Usando o seletor de cores CSS

Em seguida, você aprenderá como usar o Page Inspector para localizar rapidamente e alterar o CSS texto realçado no aplicativo padrão. Neste exemplo, você decidir que não deseja que o realce azul e deseja alterá-la para outra cor.

Clique o **inspecionar** botão.

![Inspecione o elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre o realçado "vídeos, tutoriais e amostras" texto para que o CSS "marcar" rótulo é exibido.

![Passar o mouse sobre o elemento de marca](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Clique no texto para selecioná-lo. O seletor de marca CSS correspondente é exibida na parte inferior da **estilos** janela.

![seletor de marca na janela estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Clique no seletor de marca. Isso abre o *Site.css* arquivo para o aplicativo web. Clique na guia do site e o CSS correspondente para o seletor é realçado:

![seletor de marca na folha de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Selecionar e remover a linha com a propriedade de cor de plano de fundo.

Agora você usará o novo seletor de cores CSS de 2012 do Visual Studio para escolher uma nova cor para o **marcar** propriedade de cor de plano de fundo.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Usando o seletor de cor de CSS do Visual Studio 2012

O editor de CSS no Visual Studio 2012 tem um seletor de cores que facilita a escolha e inserir cores. Ele tem uma barra de cores simple e um seletor "suspenso pop" que oferece um controle mais preciso.

O seletor de cor inclui uma paleta padrão de cores, dá suporte a nomes de cores padrão, códigos hash, cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores usado mais recentemente do documento.

Na linha em que a propriedade de cor de plano de fundo foi, digite "bc" e pressione a seta para baixo de uma vez.

Quando você digita o primeiro caractere de cada palavra em uma propriedade separados hífen como "background-color", o IntelliSense filtra a lista para mostrar apenas as propriedades que correspondem a:

![Valores de filtro do IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Agora, digite dois-pontos. Quando você fizer isso, o nome da propriedade de cor de plano de fundo completo é inserido. Tipo **#** ou **rgb (**, e a barra de seletor de cor é exibida:

![A barra de seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Para ver como funciona a barra do seletor de cores, clique em suas cores com o ponteiro do mouse, ou pressione a tecla de seta para baixo e, em seguida, use as teclas de seta para esquerda e direita para percorrer as cores. Quando você visita uma cor, o valor correspondente para a propriedade de cor de plano de fundo é visualizado:

![valor de propriedade de cor de plano de fundo visualizado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

Neste ponto, você pode pressionar Enter para selecionar o valor e, em seguida, um ponto e vírgula (;) para concluir a entrada CSS. Por enquanto, vá para a próxima seção para que você possa ver como funciona o seletor de cor pop-down.

#### <a name="using-the-color-picker-pop-down"></a>Usando o seletor de cor Pop-Down

Quando a barra de cores não tem a cor exata que você está procurando, você pode usar o seletor de cores pop-down.

Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores ou pressione a seta para baixo de uma ou duas vezes no teclado.

![Seletor de cores CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Clique em uma cor de barra vertical no lado direito. Isso mostra um gradiente dessa cor na janela principal. Escolha uma cor diretamente na barra de ferramentas vertical, pressionando Enter ou clique em qualquer ponto na janela principal para escolher com maior precisão.

Se houver uma cor na tela do computador que você deseja usar (ele não precisa estar dentro da interface de usuário do Visual Studio), você pode capturar o seu valor usando a ferramenta de conta-gotas no canto inferior direito.

Você também pode alterar a opacidade da cor movendo o controle deslizante na parte inferior do seletor de cores. Se o fizer, alterações de valores para os valores RGBA cores porque o formato RGBA pode representar a opacidade.

Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor de plano de fundo no *Site.css* arquivo.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>A barra de atualização do Inspetor de página

O Page Inspector imediatamente detecta a alteração de *Site.css* arquivo (ou qualquer arquivo no aplicativo) e exibe um alerta em uma barra de atualização.

![Barra de atualização](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Para salvar todos os arquivos e atualizar o navegador do Inspetor de página, pressione Ctrl + Alt + Enter ou clique na barra de atualização. A alteração na cor de realce é exibida no navegador:

![Cor de realce alterado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Observe que você convenientemente atualizados o navegador do Inspetor de página diretamente no ambiente do Visual Studio. Com o Page Inspector, em vez de um navegador externo permite que você permaneça no editor quando desenvolver seus aplicativos web.

## <a name="see-also"></a>Consulte também

[Apresentando o Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo do Channel 9)

[Mensagens de erro do Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
