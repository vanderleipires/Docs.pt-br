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
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="e93be-104">Com o Page Inspector para Visual Studio 2012 em Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e93be-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="e93be-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="e93be-105">by Tim Ammann</span></span>

> <span data-ttu-id="e93be-106">Inspetor de página para o Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado.</span><span class="sxs-lookup"><span data-stu-id="e93be-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="e93be-107">Selecione qualquer elemento no navegador integrado e o Page Inspector instantaneamente realça o CSS e o código-fonte do elemento.</span><span class="sxs-lookup"><span data-stu-id="e93be-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="e93be-108">Você pode procurar qualquer página em seu aplicativo rapidamente localizar as fontes de marcação renderizada e usar as ferramentas de navegador à direita no ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e93be-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="e93be-109">Este tutorial shwos como habilitar o modo de inspeção e, em seguida, localize rapidamente e editar regras de CSS e texto dentro de seu projeto da web.</span><span class="sxs-lookup"><span data-stu-id="e93be-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="e93be-110">O tutorial usa um projeto de aplicativo Web Forms, mas você também pode usar o Inspetor de página para projetos de Site e [MVC](https://go.microsoft.com/?linkid=9802002) aplicativos.</span><span class="sxs-lookup"><span data-stu-id="e93be-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="e93be-111">O tutorial tem as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="e93be-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="e93be-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e93be-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="e93be-113">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="e93be-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="e93be-114">Use o Inspetor de página para exibir o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e93be-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="e93be-115">Habilitar o modo de inspeção</span><span class="sxs-lookup"><span data-stu-id="e93be-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="e93be-116">Use o Inspetor de página para fazer as alterações de marcação</span><span class="sxs-lookup"><span data-stu-id="e93be-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="e93be-117">Modo de inspeção e a janela de HTML</span><span class="sxs-lookup"><span data-stu-id="e93be-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="e93be-118">Visualizar alterações CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="e93be-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="e93be-119">Sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="e93be-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="e93be-120">Usando o seletor de cores CSS</span><span class="sxs-lookup"><span data-stu-id="e93be-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="e93be-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e93be-121">Prerequisites</span></span>

- <span data-ttu-id="e93be-122">[O Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="e93be-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="e93be-123">Para obter a versão mais recente do Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Azure para .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="e93be-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="e93be-124">O Page Inspector é fornecido com o Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="e93be-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="e93be-125">A versão mais recente é 1.3.</span><span class="sxs-lookup"><span data-stu-id="e93be-125">The latest version is 1.3.</span></span> <span data-ttu-id="e93be-126">Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre o Microsoft Visual Studio** do **ajuda** menu.</span><span class="sxs-lookup"><span data-stu-id="e93be-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="e93be-127">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="e93be-127">Create a Web Application</span></span>

<span data-ttu-id="e93be-128">Primeiro, você criará um aplicativo web que você usará com o Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="e93be-129">No Visual Studio, escolha **arquivo** &gt; **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e93be-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="e93be-130">À esquerda, expanda **Visual C#**, selecione **Web**e, em seguida, selecione **aplicativo ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="e93be-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Novo aplicativo de formulários da Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="e93be-132">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e93be-132">Click **OK**.</span></span>

<span data-ttu-id="e93be-133">O aplicativo é aberto na **fonte** exibição.</span><span class="sxs-lookup"><span data-stu-id="e93be-133">The application opens in **Source** view.</span></span>

![Novo aplicativo de formulários da Web no modo de exibição de fonte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="e93be-135">Agora que você tenha um aplicativo para trabalhar com, você pode usar o Inspetor de página para examinar e modificá-lo.</span><span class="sxs-lookup"><span data-stu-id="e93be-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="e93be-136">Use o Inspetor de página para exibir o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e93be-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="e93be-137">Em seguida, você exibirá o aplicativo com o Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="e93be-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="e93be-138">Em **Solution Explorer**, clique com botão direito no projeto e escolha **exibir em Inspetor de página**.</span><span class="sxs-lookup"><span data-stu-id="e93be-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Exibir em Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="e93be-140">Por padrão, quando o Page Inspector é iniciado pela primeira vez, ela estiver encaixada como uma janela específica no lado esquerdo do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e93be-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="e93be-141">Deixe o campo encaixado à esquerda e defina-a como uma largura que confortável para você ou encaixá-la em uma das áreas de ferramenta sobre a parte superior, inferior ou direita:</span><span class="sxs-lookup"><span data-stu-id="e93be-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Posições de encaixe do Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="e93be-143">Se você desencaixar janela Inspetor de página, você pode colocar a ele fora do Visual Studio, ou até mesmo em um segundo monitor se você tiver um.</span><span class="sxs-lookup"><span data-stu-id="e93be-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="e93be-144">No entanto, em ordem para ALT + TAB entre o Inspetor de página e o Visual Studio quando a janela do Page Inspector é desencaixada, vá para **ferramentas** &gt; **opções** &gt;  **Ambiente** &gt; **janelas e guias**e, em **guia bem**, desmarque a caixa de seleção chamada **janelas de ferramentas flutuante permanecem sempre na parte superior das janela principal**:</span><span class="sxs-lookup"><span data-stu-id="e93be-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Desmarque a caixa de seleção de windows ferramenta flutuante para ALT + TAB entre o Visual Studio e a janela do Page Inspector desencaixada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="e93be-146">O painel superior da janela do Page Inspector mostra a página atual em uma janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="e93be-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="e93be-147">O painel inferior mostra a página na marcação HTML à esquerda, e alguns guias à direita que permitem a você inspecionar diferentes aspectos da página.</span><span class="sxs-lookup"><span data-stu-id="e93be-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="e93be-148">O painel inferior é semelhante do [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e93be-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="e93be-149">(No entanto, ao contrário das ferramentas de desenvolvedor, você pode usar o Page Inspector dentro do Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="e93be-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspetor de Página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="e93be-151">Neste tutorial, você usará o painel do navegador do Inspetor de página e o **HTML** e **estilos** guias para ajudá-lo rapidamente navegar e fazer alterações no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e93be-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="e93be-152">Habilitar o modo de inspeção</span><span class="sxs-lookup"><span data-stu-id="e93be-152">Enable Inspection Mode</span></span>

<span data-ttu-id="e93be-153">Em seguida, você verá como funciona o modo de inspeção do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="e93be-154">Na janela do Inspetor de página, clique o **inspecionar** botão.</span><span class="sxs-lookup"><span data-stu-id="e93be-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspecione o elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="e93be-156">Para ver o modo de inspeção em ação, mova o mouse sobre diferentes partes da página na janela do navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="e93be-157">Como é feito, o ponteiro do mouse muda para um sinal de adição grande e o elemento abaixo é realçado:</span><span class="sxs-lookup"><span data-stu-id="e93be-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Focalizar div.content wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="e93be-159">Observe que, assim que você move o ponteiro do mouse,</span><span class="sxs-lookup"><span data-stu-id="e93be-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="e93be-160">O conteúdo **fonte** exibir muda para mostrar a marcação correspondente ao elemento selecionado na página.</span><span class="sxs-lookup"><span data-stu-id="e93be-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="e93be-161">A marcação relevante é realçada.</span><span class="sxs-lookup"><span data-stu-id="e93be-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="e93be-162">Se a origem estiver em outro arquivo, esse arquivo é aberto no modo de exibição de fonte com a marcação relevante realçada.</span><span class="sxs-lookup"><span data-stu-id="e93be-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="e93be-163">A marcação exibida no **HTML** guia no Inspetor de página também será alterado para corresponder ao elemento selecionado na página.</span><span class="sxs-lookup"><span data-stu-id="e93be-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="e93be-164">No **HTML** guia, a marcação relevante é descrita.</span><span class="sxs-lookup"><span data-stu-id="e93be-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="e93be-165">O **estilos** guia mostra as regras de CSS relevantes para a seleção atual.</span><span class="sxs-lookup"><span data-stu-id="e93be-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="e93be-166">Use o Inspetor de página para fazer as alterações de marcação</span><span class="sxs-lookup"><span data-stu-id="e93be-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="e93be-167">Agora, você verá como você pode usar o Inspetor de página para localizar e fazer alterações em marcação ou o texto cujo local pode não ser imediatamente óbvio.</span><span class="sxs-lookup"><span data-stu-id="e93be-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="e93be-168">Coloque o Inspetor de página no modo de inspeção e, em seguida, role até a parte inferior da página inicial.</span><span class="sxs-lookup"><span data-stu-id="e93be-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="e93be-169">Assim que você inserir a área do rodapé, Page Inspector abre o *Site.Master* arquivo layout em **fonte** exibição em uma guia temporária à direita das outras guias e realça a seção do mestre de página que você selecionado.</span><span class="sxs-lookup"><span data-stu-id="e93be-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="e93be-170">Isso mostra como o Inspetor de página podem localizar e exibir o conteúdo em uma página que realmente pode vir de um arquivo diferente daquele que você abriu originalmente.</span><span class="sxs-lookup"><span data-stu-id="e93be-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Destaques de rodapé no modo de inspeção](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="e93be-172">Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre a linha com os direitos autorais <a id="a"> </a>Observe.</span><span class="sxs-lookup"><span data-stu-id="e93be-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="e93be-173">No *Site.Master* página, a linha correspondente é realçada.</span><span class="sxs-lookup"><span data-stu-id="e93be-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Linha de direitos autorais do rodapé realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="e93be-175">Adicionar texto ao final da linha de *Site.Master* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e93be-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="e93be-176">&lt;p&gt;&amp;copiar; &lt;%: % DateTime.Now.Year&gt; -meu Rocks do aplicativo ASP.NET!&lt; / p&gt;</span><span class="sxs-lookup"><span data-stu-id="e93be-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="e93be-177">Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Meu Rocks do aplicativo ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="e93be-179">Você pode considerar que o rodapé estava no *Default.aspx* página, mas ele tornou-se na página de layout mestre e o Page Inspector encontrado-lo para você.</span><span class="sxs-lookup"><span data-stu-id="e93be-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="e93be-180">Modo de inspeção e a janela de HTML</span><span class="sxs-lookup"><span data-stu-id="e93be-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="e93be-181">Em seguida, você terá uma visão geral de janela HTML e como ele mapeia elementos para você.</span><span class="sxs-lookup"><span data-stu-id="e93be-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="e93be-182">Coloque o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e93be-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspecione o elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="e93be-184">Clique na parte superior da página que diz "seu logotipo aqui".</span><span class="sxs-lookup"><span data-stu-id="e93be-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="e93be-185">Se estiver examinando um determinado elemento mais detalhadamente, para que a exibição na janela do navegador não muda à medida que você move o ponteiro do mouse.</span><span class="sxs-lookup"><span data-stu-id="e93be-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="e93be-186">Agora mova o ponteiro do mouse para o **HTML** janela.</span><span class="sxs-lookup"><span data-stu-id="e93be-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="e93be-187">Assim que você move o ponteiro do mouse, o Page Inspector descreve o elemento dentro do **HTML** janela e realça o elemento correspondente na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="e93be-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Janela de HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="e93be-189">Como antes, o Inspetor de página abre o *Site.Master* arquivo para você em uma guia temporário. Clique na guia Site.Master e a marcação correspondente é realçada no &lt;cabeçalho&gt; seção:</span><span class="sxs-lookup"><span data-stu-id="e93be-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Marcação realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="e93be-191">Visualizar alterações CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="e93be-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="e93be-192">Em seguida, você verá como é possível usar o Page Inspector **estilos** janela para visualizar as alterações de CSS.</span><span class="sxs-lookup"><span data-stu-id="e93be-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="e93be-193">Clique o **inspecionar** botão para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e93be-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e93be-194">Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre a seção "Home Page" até que o **div.content wrapper** rótulo aparece.</span><span class="sxs-lookup"><span data-stu-id="e93be-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Focalizar os elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="e93be-196">Clique dentro da seção de wrapper div.content uma vez e, em seguida, mova o ponteiro do mouse para o **estilos** janela.</span><span class="sxs-lookup"><span data-stu-id="e93be-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="e93be-197">Sob o seletor de classe de invólucro. Content .featured, desmarque e marque a caixa de seleção para a propriedade de cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="e93be-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Cor de plano de fundo claro](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="e93be-199">Observe como a alteração imediatamente visualiza na janela do navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="e93be-200">Selecione a caixa de seleção novamente, em seguida, clique duas vezes o valor da propriedade e alterá-la para `red`.</span><span class="sxs-lookup"><span data-stu-id="e93be-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="e93be-201">A alteração mostra imediatamente:</span><span class="sxs-lookup"><span data-stu-id="e93be-201">The change shows immediately:</span></span>

![Cor do plano de fundo vermelho](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="e93be-203">O **estilos** torna-o fácil de testar e visualizar CSS alterações antes de confirmar as alterações para o estilo a janela da folha em si.</span><span class="sxs-lookup"><span data-stu-id="e93be-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="e93be-204">Sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="e93be-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="e93be-205">Este recurso requer a versão 1.3 do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="e93be-206">O recurso de sincronização automática de CSS permite que você editar um arquivo CSS diretamente e ver as alterações imediatamente no navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="e93be-207">Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e93be-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e93be-208">No navegador do Inspetor de página, mova o ponteiro do mouse na seção "Home Page" até que o **div.content wrapper** rótulo aparece.</span><span class="sxs-lookup"><span data-stu-id="e93be-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="e93be-209">Clique para selecionar esse elemento.</span><span class="sxs-lookup"><span data-stu-id="e93be-209">Click once to select this element.</span></span>

<span data-ttu-id="e93be-210">O **Syles** janela mostra todas as regras CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="e93be-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="e93be-211">Role para baixo até localizar o wrapper de. Content .featured seletor de classe.</span><span class="sxs-lookup"><span data-stu-id="e93be-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="e93be-212">Clique em ".featured. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="e93be-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="e93be-213">Inspetor de página abre o arquivo CSS que define esse estilo (site) e realça o estilo CSS correspondente.</span><span class="sxs-lookup"><span data-stu-id="e93be-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Arquivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="e93be-215">Agora, altere o valor de `background-color` para "vermelho".</span><span class="sxs-lookup"><span data-stu-id="e93be-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="e93be-216">A alteração aparece imediatamente no navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e93be-216">The change appears immediately in the Page Inspector browser.</span></span>

![Navegador do Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="e93be-218">Usando o seletor de cores CSS</span><span class="sxs-lookup"><span data-stu-id="e93be-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="e93be-219">Em seguida, você aprenderá como usar o Page Inspector para localizar rapidamente e alterar o CSS texto realçado no aplicativo padrão.</span><span class="sxs-lookup"><span data-stu-id="e93be-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="e93be-220">Neste exemplo, você decidir que não deseja que o realce azul e deseja alterá-la para outra cor.</span><span class="sxs-lookup"><span data-stu-id="e93be-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="e93be-221">Clique o **inspecionar** botão.</span><span class="sxs-lookup"><span data-stu-id="e93be-221">Click the **Inspect** button.</span></span>

![Inspecione o elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="e93be-223">Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre o realçado "vídeos, tutoriais e amostras" texto para que o CSS "marcar" rótulo é exibido.</span><span class="sxs-lookup"><span data-stu-id="e93be-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Passar o mouse sobre o elemento de marca](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="e93be-225">Clique no texto para selecioná-lo.</span><span class="sxs-lookup"><span data-stu-id="e93be-225">Click the text to select it.</span></span> <span data-ttu-id="e93be-226">O seletor de marca CSS correspondente é exibida na parte inferior da **estilos** janela.</span><span class="sxs-lookup"><span data-stu-id="e93be-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![seletor de marca na janela estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="e93be-228">Clique no seletor de marca.</span><span class="sxs-lookup"><span data-stu-id="e93be-228">Click the mark selector.</span></span> <span data-ttu-id="e93be-229">Isso abre o *Site.css* arquivo para o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e93be-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="e93be-230">Clique na guia do site e o CSS correspondente para o seletor é realçado:</span><span class="sxs-lookup"><span data-stu-id="e93be-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![seletor de marca na folha de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="e93be-232">Selecionar e remover a linha com a propriedade de cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="e93be-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="e93be-233">Agora você usará o novo seletor de cores CSS de 2012 do Visual Studio para escolher uma nova cor para o **marcar** propriedade de cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="e93be-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="e93be-234">Usando o seletor de cor de CSS do Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e93be-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="e93be-235">O editor de CSS no Visual Studio 2012 tem um seletor de cores que facilita a escolha e inserir cores.</span><span class="sxs-lookup"><span data-stu-id="e93be-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="e93be-236">Ele tem uma barra de cores simple e um seletor "suspenso pop" que oferece um controle mais preciso.</span><span class="sxs-lookup"><span data-stu-id="e93be-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="e93be-237">O seletor de cor inclui uma paleta padrão de cores, dá suporte a nomes de cores padrão, códigos hash, cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores usado mais recentemente do documento.</span><span class="sxs-lookup"><span data-stu-id="e93be-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="e93be-238">Na linha em que a propriedade de cor de plano de fundo foi, digite "bc" e pressione a seta para baixo de uma vez.</span><span class="sxs-lookup"><span data-stu-id="e93be-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="e93be-239">Quando você digita o primeiro caractere de cada palavra em uma propriedade separados hífen como "background-color", o IntelliSense filtra a lista para mostrar apenas as propriedades que correspondem a:</span><span class="sxs-lookup"><span data-stu-id="e93be-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valores de filtro do IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="e93be-241">Agora, digite dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="e93be-241">Now type a colon.</span></span> <span data-ttu-id="e93be-242">Quando você fizer isso, o nome da propriedade de cor de plano de fundo completo é inserido.</span><span class="sxs-lookup"><span data-stu-id="e93be-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="e93be-243">Tipo **#** ou **rgb (**, e a barra de seletor de cor é exibida:</span><span class="sxs-lookup"><span data-stu-id="e93be-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![A barra de seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="e93be-245">Para ver como funciona a barra do seletor de cores, clique em suas cores com o ponteiro do mouse, ou pressione a tecla de seta para baixo e, em seguida, use as teclas de seta para esquerda e direita para percorrer as cores.</span><span class="sxs-lookup"><span data-stu-id="e93be-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="e93be-246">Quando você visita uma cor, o valor correspondente para a propriedade de cor de plano de fundo é visualizado:</span><span class="sxs-lookup"><span data-stu-id="e93be-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valor de propriedade de cor de plano de fundo visualizado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="e93be-248">Neste ponto, você pode pressionar Enter para selecionar o valor e, em seguida, um ponto e vírgula (;) para concluir a entrada CSS.</span><span class="sxs-lookup"><span data-stu-id="e93be-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="e93be-249">Por enquanto, vá para a próxima seção para que você possa ver como funciona o seletor de cor pop-down.</span><span class="sxs-lookup"><span data-stu-id="e93be-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="e93be-250">Usando o seletor de cor Pop-Down</span><span class="sxs-lookup"><span data-stu-id="e93be-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="e93be-251">Quando a barra de cores não tem a cor exata que você está procurando, você pode usar o seletor de cores pop-down.</span><span class="sxs-lookup"><span data-stu-id="e93be-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="e93be-252">Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores ou pressione a seta para baixo de uma ou duas vezes no teclado.</span><span class="sxs-lookup"><span data-stu-id="e93be-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Seletor de cores CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="e93be-254">Clique em uma cor de barra vertical no lado direito.</span><span class="sxs-lookup"><span data-stu-id="e93be-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="e93be-255">Isso mostra um gradiente dessa cor na janela principal.</span><span class="sxs-lookup"><span data-stu-id="e93be-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="e93be-256">Escolha uma cor diretamente na barra de ferramentas vertical, pressionando Enter ou clique em qualquer ponto na janela principal para escolher com maior precisão.</span><span class="sxs-lookup"><span data-stu-id="e93be-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="e93be-257">Se houver uma cor na tela do computador que você deseja usar (ele não precisa estar dentro da interface de usuário do Visual Studio), você pode capturar o seu valor usando a ferramenta de conta-gotas no canto inferior direito.</span><span class="sxs-lookup"><span data-stu-id="e93be-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="e93be-258">Você também pode alterar a opacidade da cor movendo o controle deslizante na parte inferior do seletor de cores.</span><span class="sxs-lookup"><span data-stu-id="e93be-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="e93be-259">Se o fizer, alterações de valores para os valores RGBA cores porque o formato RGBA pode representar a opacidade.</span><span class="sxs-lookup"><span data-stu-id="e93be-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="e93be-260">Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor de plano de fundo no *Site.css* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e93be-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="e93be-261">A barra de atualização do Inspetor de página</span><span class="sxs-lookup"><span data-stu-id="e93be-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="e93be-262">O Page Inspector imediatamente detecta a alteração de *Site.css* arquivo (ou qualquer arquivo no aplicativo) e exibe um alerta em uma barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="e93be-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra de atualização](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="e93be-264">Para salvar todos os arquivos e atualizar o navegador do Inspetor de página, pressione Ctrl + Alt + Enter ou clique na barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="e93be-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="e93be-265">A alteração na cor de realce é exibida no navegador:</span><span class="sxs-lookup"><span data-stu-id="e93be-265">The change in the highlight color appears in the browser:</span></span>

![Cor de realce alterado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="e93be-267">Observe que você convenientemente atualizados o navegador do Inspetor de página diretamente no ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e93be-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="e93be-268">Com o Page Inspector, em vez de um navegador externo permite que você permaneça no editor quando desenvolver seus aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="e93be-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="e93be-269">Consulte também</span><span class="sxs-lookup"><span data-stu-id="e93be-269">See Also</span></span>

<span data-ttu-id="e93be-270">[Apresentando o Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo do Channel 9)</span><span class="sxs-lookup"><span data-stu-id="e93be-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="e93be-271">[Mensagens de erro do Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="e93be-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
