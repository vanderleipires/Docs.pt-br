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
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="b4d93-104">Usando o Inspetor de página para Visual Studio 2012 em Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b4d93-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="b4d93-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="b4d93-105">by Tim Ammann</span></span>

> <span data-ttu-id="b4d93-106">O Inspetor de página para Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado.</span><span class="sxs-lookup"><span data-stu-id="b4d93-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="b4d93-107">Selecione qualquer elemento no navegador integrado e Inspetor de página instantaneamente destaca o código-fonte e o CSS do elemento.</span><span class="sxs-lookup"><span data-stu-id="b4d93-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="b4d93-108">Você pode procurar qualquer página em seu aplicativo rapidamente encontrar as fontes de marcação renderizada e usar as ferramentas de navegador dentro do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4d93-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="b4d93-109">Este tutorial shwos como habilitar o modo de inspeção e, em seguida, rapidamente localizar e editar regras de CSS e texto dentro de seu projeto da web.</span><span class="sxs-lookup"><span data-stu-id="b4d93-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="b4d93-110">O tutorial usa um projeto de aplicativo Web Forms, mas você também pode usar o Inspetor de página para projetos de Site e [MVC](https://go.microsoft.com/?linkid=9802002) aplicativos.</span><span class="sxs-lookup"><span data-stu-id="b4d93-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="b4d93-111">O tutorial tem as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="b4d93-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="b4d93-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="b4d93-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="b4d93-113">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="b4d93-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="b4d93-114">Usar o Inspetor de página para exibir o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b4d93-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="b4d93-115">Habilitar o modo de inspeção</span><span class="sxs-lookup"><span data-stu-id="b4d93-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="b4d93-116">Use o Inspetor de página para fazer alterações à marcação</span><span class="sxs-lookup"><span data-stu-id="b4d93-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="b4d93-117">Modo de inspeção e a janela do HTML</span><span class="sxs-lookup"><span data-stu-id="b4d93-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="b4d93-118">Visualizar alterações CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="b4d93-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="b4d93-119">Sincronização automática CSS</span><span class="sxs-lookup"><span data-stu-id="b4d93-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="b4d93-120">Usando o seletor de cor CSS</span><span class="sxs-lookup"><span data-stu-id="b4d93-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b4d93-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="b4d93-121">Prerequisites</span></span>

- <span data-ttu-id="b4d93-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="b4d93-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="b4d93-123">Para obter a versão mais recente do Inspetor de página, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Azure para .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="b4d93-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="b4d93-124">O Page Inspector é fornecido com o Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="b4d93-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="b4d93-125">A versão mais recente é 1.3.</span><span class="sxs-lookup"><span data-stu-id="b4d93-125">The latest version is 1.3.</span></span> <span data-ttu-id="b4d93-126">Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre o Microsoft Visual Studio** da **ajudar** menu.</span><span class="sxs-lookup"><span data-stu-id="b4d93-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="b4d93-127">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="b4d93-127">Create a Web Application</span></span>

<span data-ttu-id="b4d93-128">Primeiro, você criará um aplicativo web que você usará com o Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="b4d93-129">No Visual Studio, escolha **arquivo** &gt; **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="b4d93-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b4d93-130">À esquerda, expanda **Visual c#**, selecione **Web**e, em seguida, selecione **aplicativo ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="b4d93-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Novo aplicativo de formulários da Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="b4d93-132">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4d93-132">Click **OK**.</span></span>

<span data-ttu-id="b4d93-133">O aplicativo é aberto no **origem** modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b4d93-133">The application opens in **Source** view.</span></span>

![Novo aplicativo de formulários da Web no modo de exibição de fonte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="b4d93-135">Agora que você tem um aplicativo para trabalhar com, você pode usar o Inspetor de página para examinar e modificá-lo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="b4d93-136">Usar o Inspetor de página para exibir o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b4d93-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="b4d93-137">Em seguida, você exibirá o aplicativo com o Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="b4d93-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="b4d93-138">Na **Gerenciador de soluções**, com o botão direito do mouse no projeto e, em seguida, escolha **exibir no Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="b4d93-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Exibir no Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="b4d93-140">Por padrão, quando o Inspetor de página é iniciado pela primeira vez, ele é encaixado como uma janela estreita no lado esquerdo do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4d93-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="b4d93-141">Deixe-encaixada no lado esquerdo e defina-o como uma largura que se sente confortável para você ou encaixá-la em uma das áreas de ferramenta sobre a parte superior, inferior ou direita:</span><span class="sxs-lookup"><span data-stu-id="b4d93-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Posições de encaixe do Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="b4d93-143">Se você desencaixar a janela do Inspetor de página, você pode colocar a ele fora do Visual Studio, ou até mesmo em um segundo monitor se você tiver uma.</span><span class="sxs-lookup"><span data-stu-id="b4d93-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="b4d93-144">No entanto, em ordem para ALT + TAB entre o Inspetor de página e o Visual Studio quando a janela do Inspetor de página estiver desencaixada, vá para **ferramentas** &gt; **opções** &gt;  **Ambiente** &gt; **guias e Windows**e, em **guia bem**, desmarque a caixa de seleção chamada **janelas de ferramentas flutuante permanecem sempre na parte superior das janela principal**:</span><span class="sxs-lookup"><span data-stu-id="b4d93-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Desmarque a caixa de seleção de windows ferramenta flutuante para ALT + TAB entre o Visual Studio e a janela Inspetor de página desencaixada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="b4d93-146">O painel superior da janela do Inspetor de página mostra a página atual em uma janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="b4d93-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="b4d93-147">O painel inferior mostra a página na marcação HTML à esquerda e algumas guias à direita que permitem que você inspecione os diferentes aspectos da página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="b4d93-148">O painel inferior é semelhante para o [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b4d93-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="b4d93-149">(No entanto, diferentemente das ferramentas de desenvolvedor, você pode usar o Inspetor de página dentro do Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="b4d93-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspetor de Página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="b4d93-151">Neste tutorial, você usará o painel do navegador Inspetor de página e o **HTML** e **estilos** guias para ajudar você a rapidamente navegar e fazer alterações no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="b4d93-152">Habilitar o modo de inspeção</span><span class="sxs-lookup"><span data-stu-id="b4d93-152">Enable Inspection Mode</span></span>

<span data-ttu-id="b4d93-153">Em seguida, você verá como funciona o modo de inspeção do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="b4d93-154">Na janela do Inspetor de página, clique o **inspecionar** botão.</span><span class="sxs-lookup"><span data-stu-id="b4d93-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="b4d93-156">Para ver o modo de inspeção em ação, mova o mouse sobre diferentes partes da página dentro da janela do navegador Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="b4d93-157">Como você, o ponteiro do mouse muda para um grande sinal de adição e o elemento abaixo é realçado:</span><span class="sxs-lookup"><span data-stu-id="b4d93-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Passar o mouse sobre o wrapper div.content](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="b4d93-159">Observe que, à medida que você move o ponteiro do mouse,</span><span class="sxs-lookup"><span data-stu-id="b4d93-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="b4d93-160">O conteúdo no **origem** exibir muda para mostrar a marcação correspondente ao elemento selecionado na página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="b4d93-161">A marcação relevante é realçada.</span><span class="sxs-lookup"><span data-stu-id="b4d93-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="b4d93-162">Se a fonte estiver em outro arquivo, esse arquivo é aberto no modo de exibição de código-fonte com a marcação relevante realçada.</span><span class="sxs-lookup"><span data-stu-id="b4d93-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="b4d93-163">A marcação exibida na **HTML** guia no Inspetor de página também será alterado para corresponder ao elemento selecionado na página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="b4d93-164">No **HTML** guia, a marcação relevante é descrita.</span><span class="sxs-lookup"><span data-stu-id="b4d93-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="b4d93-165">O **estilos** guia mostra as regras de CSS relevantes para a seleção atual.</span><span class="sxs-lookup"><span data-stu-id="b4d93-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="b4d93-166">Use o Inspetor de página para fazer alterações à marcação</span><span class="sxs-lookup"><span data-stu-id="b4d93-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="b4d93-167">Agora, você verá como você pode usar o Inspetor de página para encontrar e fazer alterações na marcação ou no texto cujo local pode não ser imediatamente óbvio.</span><span class="sxs-lookup"><span data-stu-id="b4d93-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="b4d93-168">Colocar o Inspetor de página no modo de inspeção e, em seguida, role até a parte inferior da página inicial.</span><span class="sxs-lookup"><span data-stu-id="b4d93-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="b4d93-169">Assim que você inserir a área do rodapé, o Inspetor de página abre o *Master* arquivo de layout na **origem** exibição em uma guia temporária para a direita das outras guias e realça a seção do mestre de página que você selecionou.</span><span class="sxs-lookup"><span data-stu-id="b4d93-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="b4d93-170">Isso mostra como o Inspetor de página pode localizar e exibir o conteúdo em uma página que, na verdade, pode vir de um arquivo diferente daquele que você tiver aberto originalmente.</span><span class="sxs-lookup"><span data-stu-id="b4d93-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Destaques de rodapé no modo de inspeção](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="b4d93-172">Na janela do navegador Inspetor de página, mova o ponteiro do mouse sobre a linha com os direitos autorais <a id="a"> </a>Observe.</span><span class="sxs-lookup"><span data-stu-id="b4d93-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="b4d93-173">No *Master* página, a linha correspondente é realçada.</span><span class="sxs-lookup"><span data-stu-id="b4d93-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Linha de direitos autorais do rodapé realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="b4d93-175">Adicione algum texto ao final da linha na *Master* arquivo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="b4d93-176">&lt;p&gt;&amp;copiarem; &lt;%: % DateTime.Now.Year&gt; -é o máximo meu aplicativo ASP.NET!&lt; / p&gt;</span><span class="sxs-lookup"><span data-stu-id="b4d93-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="b4d93-177">Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![É o máximo meu aplicativo ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="b4d93-179">Você deve ter imaginado se o rodapé foi na *default. aspx* página, mas ela tornou-se na página de layout mestre e o Inspetor de página encontrado-lo para você.</span><span class="sxs-lookup"><span data-stu-id="b4d93-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="b4d93-180">Modo de inspeção e a janela do HTML</span><span class="sxs-lookup"><span data-stu-id="b4d93-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="b4d93-181">Em seguida, você terá uma olhada rápida na janela do HTML e como ele mapeia elementos para você.</span><span class="sxs-lookup"><span data-stu-id="b4d93-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="b4d93-182">Colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="b4d93-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="b4d93-184">Clique na parte superior da página que diz "seu logotipo aqui".</span><span class="sxs-lookup"><span data-stu-id="b4d93-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="b4d93-185">Você está examinando um determinado elemento mais detalhadamente, portanto, a exibição na janela do navegador não é alterado à medida que você move o ponteiro do mouse.</span><span class="sxs-lookup"><span data-stu-id="b4d93-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="b4d93-186">Agora mova o ponteiro do mouse para o **HTML** janela.</span><span class="sxs-lookup"><span data-stu-id="b4d93-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="b4d93-187">À medida que você move o ponteiro do mouse, o Inspetor de página descreve o elemento dentro de **HTML** janela e realça o elemento correspondente na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="b4d93-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Janela do HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="b4d93-189">Como antes, o Inspetor de página abre o *Master* arquivo para você em uma guia temporária. Clique na guia Master e a marcação correspondente é realçada na &lt;cabeçalho&gt; seção:</span><span class="sxs-lookup"><span data-stu-id="b4d93-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Marcação realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="b4d93-191">Visualizar alterações CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="b4d93-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="b4d93-192">Em seguida, você verá como é possível usar o Inspetor de página **estilos** janela para visualizar as alterações à CSS.</span><span class="sxs-lookup"><span data-stu-id="b4d93-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="b4d93-193">Clique o **inspecionar** botão para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="b4d93-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b4d93-194">Na janela do navegador Inspetor de página, mova o ponteiro do mouse sobre a seção de "Home Page" até que o **div.content wrapper** rótulo aparece.</span><span class="sxs-lookup"><span data-stu-id="b4d93-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Focalizar os elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="b4d93-196">Clique dentro da seção de wrapper div.content uma vez e, em seguida, mova o ponteiro do mouse para o **estilos** janela.</span><span class="sxs-lookup"><span data-stu-id="b4d93-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="b4d93-197">Sob o seletor de classe de wrapper. Content .featured, desmarque e marque a caixa de seleção para a propriedade de cor do plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Cor do plano de fundo clara](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="b4d93-199">Observe como a alteração visualiza instantaneamente na janela do navegador Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="b4d93-200">Selecione a caixa de seleção novamente, em seguida, clique duas vezes o valor da propriedade e alterá-la para `red`.</span><span class="sxs-lookup"><span data-stu-id="b4d93-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="b4d93-201">A alteração mostra imediatamente:</span><span class="sxs-lookup"><span data-stu-id="b4d93-201">The change shows immediately:</span></span>

![Cor do plano de fundo vermelho](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="b4d93-203">O **estilos** torna a janela fácil de testar e visualizar o CSS é alterado antes de confirmar as alterações para o estilo da folha em si.</span><span class="sxs-lookup"><span data-stu-id="b4d93-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="b4d93-204">Sincronização automática CSS</span><span class="sxs-lookup"><span data-stu-id="b4d93-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="b4d93-205">Este recurso requer a versão 1.3 do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="b4d93-206">O recurso de sincronização automática de CSS permite que você editar um arquivo CSS diretamente e ver as alterações imediatamente no navegador Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="b4d93-207">Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="b4d93-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b4d93-208">No navegador Inspetor de página, mova o ponteiro do mouse sobre a seção de "Home Page" até que o **div.content wrapper** rótulo aparece.</span><span class="sxs-lookup"><span data-stu-id="b4d93-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="b4d93-209">Clique para selecionar esse elemento.</span><span class="sxs-lookup"><span data-stu-id="b4d93-209">Click once to select this element.</span></span>

<span data-ttu-id="b4d93-210">O **estilos** janela mostra todas as regras CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="b4d93-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b4d93-211">Role para baixo até localizar o wrapper de. Content .featured seletor de classe.</span><span class="sxs-lookup"><span data-stu-id="b4d93-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b4d93-212">Clique em .featured. Content-"wrapper".</span><span class="sxs-lookup"><span data-stu-id="b4d93-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="b4d93-213">Inspetor de página abre o arquivo CSS que define esse estilo (CSS) e realça o estilo CSS correspondente.</span><span class="sxs-lookup"><span data-stu-id="b4d93-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Arquivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="b4d93-215">Agora, altere o valor para `background-color` para "vermelho".</span><span class="sxs-lookup"><span data-stu-id="b4d93-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="b4d93-216">A alteração aparece imediatamente no navegador Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="b4d93-216">The change appears immediately in the Page Inspector browser.</span></span>

![Navegador Inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="b4d93-218">Usando o seletor de cor CSS</span><span class="sxs-lookup"><span data-stu-id="b4d93-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="b4d93-219">Em seguida, você aprenderá como usar o Inspetor de página para localizar rapidamente e alterar o CSS do texto realçado no aplicativo padrão.</span><span class="sxs-lookup"><span data-stu-id="b4d93-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="b4d93-220">Neste exemplo, você decidiu que não deseja que o realce azul e quiser alterá-lo para outra cor.</span><span class="sxs-lookup"><span data-stu-id="b4d93-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="b4d93-221">Clique o **inspecionar** botão.</span><span class="sxs-lookup"><span data-stu-id="b4d93-221">Click the **Inspect** button.</span></span>

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="b4d93-223">Na janela do navegador Inspetor de página, mova o ponteiro do mouse sobre o realçado "vídeos, tutoriais e exemplos de" texto para que o CSS "marcar" rótulo é exibido.</span><span class="sxs-lookup"><span data-stu-id="b4d93-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Passar o mouse sobre o elemento de marca](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="b4d93-225">Clique no texto para selecioná-lo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-225">Click the text to select it.</span></span> <span data-ttu-id="b4d93-226">O seletor de marca de CSS correspondente é exibida na parte inferior a **estilos** janela.</span><span class="sxs-lookup"><span data-stu-id="b4d93-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![seletor de marca na janela estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="b4d93-228">Clique no seletor de marca.</span><span class="sxs-lookup"><span data-stu-id="b4d93-228">Click the mark selector.</span></span> <span data-ttu-id="b4d93-229">Isso abre o *CSS* arquivo para o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="b4d93-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="b4d93-230">Clique na guia de CSS, e o CSS correspondente para o seletor é realçado:</span><span class="sxs-lookup"><span data-stu-id="b4d93-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![seletor de marca na folha de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="b4d93-232">Selecione e remova a linha com a propriedade de cor do plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="b4d93-233">Agora você usará o novo seletor de cor CSS de 2012 do Visual Studio para escolher uma nova cor para o **marcar** propriedade de cor do plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="b4d93-234">Usando o seletor de cor de CSS do Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b4d93-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="b4d93-235">O editor de CSS no Visual Studio 2012 tem um seletor de cores que torna mais fácil escolher e inserir as cores.</span><span class="sxs-lookup"><span data-stu-id="b4d93-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="b4d93-236">Ele tem uma barra de cores simple e um seletor de "pop-down" que oferece controle mais refinado.</span><span class="sxs-lookup"><span data-stu-id="b4d93-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="b4d93-237">O seletor de cor inclui uma paleta de cores padrão, dá suporte a nomes de cores padrão, os códigos de hash, as cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores usados mais recentemente no documento.</span><span class="sxs-lookup"><span data-stu-id="b4d93-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="b4d93-238">Na linha em que a propriedade de cor do plano de fundo estava, digite "bc" e pressione a seta para baixo uma vez.</span><span class="sxs-lookup"><span data-stu-id="b4d93-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="b4d93-239">Quando você digita o primeiro caractere de cada palavra em uma propriedade separados por hífen, como "background-color", o IntelliSense filtra a lista para mostrar apenas as propriedades que correspondem a:</span><span class="sxs-lookup"><span data-stu-id="b4d93-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valores de IntelliSense filtrado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="b4d93-241">Agora, digite dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="b4d93-241">Now type a colon.</span></span> <span data-ttu-id="b4d93-242">Quando você fizer isso, o nome da propriedade de cor de plano de fundo completa é inserido.</span><span class="sxs-lookup"><span data-stu-id="b4d93-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="b4d93-243">Tipo de **#** ou **rgb (**, e a barra de seletor de cor é exibida:</span><span class="sxs-lookup"><span data-stu-id="b4d93-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![A barra de seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="b4d93-245">Para ver como funciona a barra do seletor de cores, clique em suas cores com o ponteiro do mouse, ou pressione a tecla de seta para baixo e, em seguida, use as teclas de seta esquerda e direita para percorrer as cores.</span><span class="sxs-lookup"><span data-stu-id="b4d93-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="b4d93-246">Quando você visita uma cor, o valor correspondente para a propriedade de cor do plano de fundo é visualizado:</span><span class="sxs-lookup"><span data-stu-id="b4d93-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valor da propriedade de cor do plano de fundo visualizado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="b4d93-248">Neste ponto, você pode pressionar Enter para selecionar o valor e, em seguida, um ponto e vírgula (;) para concluir a entrada CSS.</span><span class="sxs-lookup"><span data-stu-id="b4d93-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="b4d93-249">Por enquanto, vá para a próxima seção para que você possa ver como funciona o pop-down de seletor de cor.</span><span class="sxs-lookup"><span data-stu-id="b4d93-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="b4d93-250">Usando o seletor de cor Pop-Down</span><span class="sxs-lookup"><span data-stu-id="b4d93-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="b4d93-251">Quando a barra de cores não tem a cor exata que você está procurando, você pode usar o seletor de cores pop-down.</span><span class="sxs-lookup"><span data-stu-id="b4d93-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="b4d93-252">Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores, ou pressione a seta para baixo uma ou duas vezes no teclado.</span><span class="sxs-lookup"><span data-stu-id="b4d93-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Seletor de cor CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="b4d93-254">Clique em uma cor da barra de ferramentas vertical no lado direito.</span><span class="sxs-lookup"><span data-stu-id="b4d93-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="b4d93-255">Isso mostra uma gradação de cor na janela principal.</span><span class="sxs-lookup"><span data-stu-id="b4d93-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="b4d93-256">Escolha uma cor diretamente na barra vertical, pressionando Enter ou clique em qualquer ponto na janela principal, escolha com maior precisão.</span><span class="sxs-lookup"><span data-stu-id="b4d93-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="b4d93-257">Se houver uma cor na tela do computador que você deseja usar (ele não precisa estar dentro da interface de usuário do Visual Studio), você pode capturar o seu valor usando a ferramenta de conta-gotas no canto inferior direito.</span><span class="sxs-lookup"><span data-stu-id="b4d93-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="b4d93-258">Você também pode alterar a opacidade de uma cor movendo o controle deslizante na parte inferior do seletor de cores.</span><span class="sxs-lookup"><span data-stu-id="b4d93-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="b4d93-259">Isso muda a cor valores para os valores RGBA porque o formato de RGBA pode representar a opacidade.</span><span class="sxs-lookup"><span data-stu-id="b4d93-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="b4d93-260">Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor do plano de fundo na *CSS* arquivo.</span><span class="sxs-lookup"><span data-stu-id="b4d93-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="b4d93-261">A barra de atualização do Inspetor de página</span><span class="sxs-lookup"><span data-stu-id="b4d93-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="b4d93-262">Inspetor de página imediatamente detecta a alteração para o *CSS* arquivo (ou em qualquer arquivo no aplicativo) e exibe um alerta em uma barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="b4d93-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra de atualização](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="b4d93-264">Para salvar todos os seus arquivos e atualize o navegador Inspetor de página, pressione Ctrl + Alt + Enter ou clique na barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="b4d93-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="b4d93-265">A alteração na cor de realce é exibida no navegador:</span><span class="sxs-lookup"><span data-stu-id="b4d93-265">The change in the highlight color appears in the browser:</span></span>

![Cor de realce alterado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="b4d93-267">Observe que você atualizou convenientemente navegador Inspetor de página à direita de dentro do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4d93-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="b4d93-268">Usando o Inspetor de página em vez de um navegador externo permite que você fique no editor quando você desenvolve seus aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="b4d93-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="b4d93-269">Consulte também</span><span class="sxs-lookup"><span data-stu-id="b4d93-269">See Also</span></span>

<span data-ttu-id="b4d93-270">[Introdução ao Inspetor de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo do Channel 9)</span><span class="sxs-lookup"><span data-stu-id="b4d93-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="b4d93-271">[Mensagens de erro do Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="b4d93-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
