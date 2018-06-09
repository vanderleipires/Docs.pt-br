---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Usando o Inspetor de página no ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Inspetor de página no Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado. Selecione qualquer elemento no navegador integrado e o Page Inspector i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034510"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="e1d84-104">Usando o Inspetor de Página no ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e1d84-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="e1d84-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="e1d84-105">by Tim Ammann</span></span>

> <span data-ttu-id="e1d84-106">Inspetor de página no Visual Studio 2012 é uma ferramenta de desenvolvimento na web com um navegador integrado.</span><span class="sxs-lookup"><span data-stu-id="e1d84-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="e1d84-107">Selecione qualquer elemento no navegador integrado e o Page Inspector instantaneamente realça o CSS e o código-fonte do elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="e1d84-108">Você pode procurar qualquer modo de exibição do MVC, rapidamente localizar as fontes de marcação renderizada e usar as ferramentas de navegador à direita no ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1d84-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="e1d84-109">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="e1d84-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="e1d84-110">Este tutorial mostra como habilitar o modo de inspeção e, em seguida, localize rapidamente e Editar marcação e CSS dentro de seu projeto da web.</span><span class="sxs-lookup"><span data-stu-id="e1d84-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="e1d84-111">O tutorial usa um projeto MVC, mas você também pode usar o Page Inspector para [Web Forms](https://go.microsoft.com/?linkid=9802001) e outros aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e1d84-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="e1d84-112">O tutorial tem as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="e1d84-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="e1d84-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e1d84-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="e1d84-114">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="e1d84-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="e1d84-115">Use o Inspetor de página para procurar a uma exibição</span><span class="sxs-lookup"><span data-stu-id="e1d84-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="e1d84-116">Habilitar o modo de inspeção</span><span class="sxs-lookup"><span data-stu-id="e1d84-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="e1d84-117">Use o Inspetor de página para fazer as alterações de marcação</span><span class="sxs-lookup"><span data-stu-id="e1d84-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="e1d84-118">Modo de inspeção e a janela de HTML</span><span class="sxs-lookup"><span data-stu-id="e1d84-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="e1d84-119">Visualizar alterações de CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="e1d84-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="e1d84-120">Sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="e1d84-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="e1d84-121">Usando o seletor de cores CSS</span><span class="sxs-lookup"><span data-stu-id="e1d84-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="e1d84-122">Mapeamento de elementos dinâmicos de páginas para JavaScript</span><span class="sxs-lookup"><span data-stu-id="e1d84-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="e1d84-123">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e1d84-123">Prerequisites</span></span>

- <span data-ttu-id="e1d84-124">[O Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="e1d84-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="e1d84-125">Para obter a versão mais recente do Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Windows Azure para .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="e1d84-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="e1d84-126">O Page Inspector é fornecido com o Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="e1d84-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="e1d84-127">A versão mais recente é 1.3.</span><span class="sxs-lookup"><span data-stu-id="e1d84-127">The latest version is 1.3.</span></span> <span data-ttu-id="e1d84-128">Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre o Microsoft Visual Studio** do **ajuda** menu.</span><span class="sxs-lookup"><span data-stu-id="e1d84-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="e1d84-129">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="e1d84-129">Create a Web Application</span></span>

<span data-ttu-id="e1d84-130">Primeiro, crie um aplicativo web que você usará com o Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="e1d84-131">No Visual Studio, escolha **arquivo** &gt; **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="e1d84-132">À esquerda, expanda **Visual C#**, selecione **Web**e, em seguida, selecione **aplicativo Web do ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Novo aplicativo ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="e1d84-134">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-134">Click **OK**.</span></span>

<span data-ttu-id="e1d84-135">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de Internet**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="e1d84-136">Deixe **Razor** como o mecanismo de exibição padrão.</span><span class="sxs-lookup"><span data-stu-id="e1d84-136">Leave **Razor** as the default view engine.</span></span>

![Novo projeto ASP.NET MVC - aplicativo de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="e1d84-138">O aplicativo é aberto na **fonte** exibição.</span><span class="sxs-lookup"><span data-stu-id="e1d84-138">The application opens in **Source** view.</span></span>

![Novo aplicativo ASP.NET MVC no modo de exibição de fonte](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="e1d84-140">Agora que você tenha um aplicativo para trabalhar com, você pode usar o Inspetor de página para examinar e modificá-lo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="e1d84-141">Use o Inspetor de página para procurar a uma exibição</span><span class="sxs-lookup"><span data-stu-id="e1d84-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="e1d84-142">No Visual Studio 2012, clique qualquer exibição em seu projeto, selecione **exibir em Inspetor de página**, e o Page Inspector irá calcular a rota e exibir a página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="e1d84-143">Em **Solution Explorer**, expanda o **exibições** pasta e, em seguida, o **início** pasta.</span><span class="sxs-lookup"><span data-stu-id="e1d84-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="e1d84-144">O arquivo cshtml clique com botão direito e escolha **exibir em Inspetor de página**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Exibir cshtml no Inspetor de página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="e1d84-146">Por padrão, o Page Inspector é encaixado como uma janela no lado esquerdo do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1d84-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="e1d84-147">Se preferir, você pode encaixá-la em outro lugar ou desencaixar a janela.</span><span class="sxs-lookup"><span data-stu-id="e1d84-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="e1d84-148">Consulte [como: organizar e encaixar janelas](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1d84-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="e1d84-149">O painel superior da janela do Page Inspector mostra a página atual em uma janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="e1d84-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="e1d84-150">O painel inferior mostra a página na marcação HTML, juntamente com alguns guias que permitem que você inspecione os diferentes aspectos da página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="e1d84-151">O painel inferior é semelhante do [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e1d84-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplicativo ASP.NET MVC no Inspetor de página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="e1d84-153">Neste tutorial, você usará o **HTML** e **estilos** guias para navegar rapidamente e fazer alterações no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="e1d84-154">Modo de EnableInspection</span><span class="sxs-lookup"><span data-stu-id="e1d84-154">EnableInspection Mode</span></span>

<span data-ttu-id="e1d84-155">Para colocar o Inspetor de página no modo de inspeção, clique no **inspecionar** botão.</span><span class="sxs-lookup"><span data-stu-id="e1d84-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="e1d84-156">No modo de inspeção, quando você mantém o ponteiro do mouse sobre qualquer parte da página renderizada, o código ou marcação de origem correspondente é realçado.</span><span class="sxs-lookup"><span data-stu-id="e1d84-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Alternar o modo de inspeção](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="e1d84-158">Agora mova seu mouse sobre diferentes partes da página no Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="e1d84-159">Como é feito, o ponteiro do mouse muda para um sinal de adição grande e o elemento abaixo é realçado:</span><span class="sxs-lookup"><span data-stu-id="e1d84-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Focalizar div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="e1d84-161">Assim que você move o ponteiro do mouse, o Visual Studio realça a sintaxe do Razor correspondente no arquivo de origem.</span><span class="sxs-lookup"><span data-stu-id="e1d84-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="e1d84-162">Se o elemento HTML vierem de outro arquivo de origem, o Visual Studio abrirá automaticamente o arquivo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="e1d84-163">No Inspetor de página, o **HTML** guia mostra o HTML que foi gerado de sintaxe do Razor.</span><span class="sxs-lookup"><span data-stu-id="e1d84-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="e1d84-164">Assim que você move o ponteiro do mouse, os elementos HTML são realçados.</span><span class="sxs-lookup"><span data-stu-id="e1d84-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="e1d84-165">O **estilos** guia mostra as regras de CSS para o elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="e1d84-166">Use o Inspetor de página para fazer as alterações de marcação</span><span class="sxs-lookup"><span data-stu-id="e1d84-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="e1d84-167">Inspetor de página permite que você localize marcação cujo local pode não ser óbvio.</span><span class="sxs-lookup"><span data-stu-id="e1d84-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="e1d84-168">Você pode modificar a marcação e ver as alterações resultantes.</span><span class="sxs-lookup"><span data-stu-id="e1d84-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="e1d84-169">Para ver isso, clique em **inspecionar** e, em seguida, role até a parte inferior da página na janela do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="e1d84-170">Quando você move o ponteiro do mouse na área de rodapé, Page Inspector abre o \_arquivo cshtml e realça a seção da página de layout que você selecionou.</span><span class="sxs-lookup"><span data-stu-id="e1d84-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="e1d84-171">Como você pode ver, o rodapé são é definido no arquivo de layout e não a própria exibição.</span><span class="sxs-lookup"><span data-stu-id="e1d84-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Rodapé](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="e1d84-173">Agora mova o ponteiro do mouse sobre a linha com os direitos autorais <a id="a"> </a>Observe.</span><span class="sxs-lookup"><span data-stu-id="e1d84-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="e1d84-174">No \_cshtml página, a linha correspondente é realçada.</span><span class="sxs-lookup"><span data-stu-id="e1d84-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Linha de direitos autorais do rodapé realçada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="e1d84-176">Adicionar texto ao final da linha de \_arquivo cshtml.</span><span class="sxs-lookup"><span data-stu-id="e1d84-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="e1d84-177">&lt;p&gt;&amp;copiar; @DateTime.Now.Year -Meu aplicativo ASP.NET MVC é incrível!  &lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="e1d84-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="e1d84-178">Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Meu Rocks do aplicativo ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="e1d84-180">Você pode imaginar que o rodapé definidos no cshtml, mas acontece no \_cshtml e Inspetor de página encontrado-lo para você.</span><span class="sxs-lookup"><span data-stu-id="e1d84-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="e1d84-181">Modo de inspeção e a janela de HTML</span><span class="sxs-lookup"><span data-stu-id="e1d84-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="e1d84-182">Em seguida, você terá uma visão geral de janela HTML e como ele mapeia elementos para você.</span><span class="sxs-lookup"><span data-stu-id="e1d84-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="e1d84-183">Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e1d84-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e1d84-184">Clique na parte superior da página que diz "O logohere".</span><span class="sxs-lookup"><span data-stu-id="e1d84-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="e1d84-185">Se estiver examinando um determinado elemento mais detalhadamente, para que a exibição na janela do navegador não muda à medida que você move o ponteiro do mouse.</span><span class="sxs-lookup"><span data-stu-id="e1d84-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="e1d84-186">Agora mova o ponteiro do mouse para o **HTML** janela.</span><span class="sxs-lookup"><span data-stu-id="e1d84-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="e1d84-187">Assim que você move o ponteiro do mouse, o Page Inspector descreve o elemento dentro do **HTML** janela e realça o elemento correspondente na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="e1d84-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Janela HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="e1d84-189">Como antes, o Inspetor de página abre o \_arquivo cshtml para você em uma guia temporário. Clique o \_guia temporário cshtml e a marcação correspondente serão realçados no &lt;cabeçalho&gt; seção para você:</span><span class="sxs-lookup"><span data-stu-id="e1d84-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Marcação realçada](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="e1d84-191">Visualizar alterações CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="e1d84-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="e1d84-192">Em seguida, você usará o Page Inspector **estilos** janela para visualizar as alterações de CSS.</span><span class="sxs-lookup"><span data-stu-id="e1d84-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="e1d84-193">Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e1d84-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e1d84-194">Na janela do navegador do Inspetor de página, mova o ponteiro do mouse sobre a seção "Home Page" até que o **div.content wrapper** rótulo aparece.</span><span class="sxs-lookup"><span data-stu-id="e1d84-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Focalizar div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="e1d84-196">Clique dentro da seção de wrapper div.content uma vez e, em seguida, mova o ponteiro do mouse para o **estilos** janela.</span><span class="sxs-lookup"><span data-stu-id="e1d84-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="e1d84-197">O **Syles** janela mostra todas as regras CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="e1d84-198">Role para baixo até localizar o wrapper de. Content .featured seletor de classe.</span><span class="sxs-lookup"><span data-stu-id="e1d84-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="e1d84-199">Em seguida, desmarque a caixa de seleção para a propriedade de cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-199">Now clear the checkbox for the background-color property.</span></span>

![Cor de plano de fundo claro](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="e1d84-201">Observe como a alteração imediatamente visualiza na janela do navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="e1d84-202">Selecione a caixa de seleção novamente, clique duas vezes o valor da propriedade e alterá-la para vermelho.</span><span class="sxs-lookup"><span data-stu-id="e1d84-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="e1d84-203">A alteração mostra imediatamente:</span><span class="sxs-lookup"><span data-stu-id="e1d84-203">The change shows immediately:</span></span>

![Cor do plano de fundo vermelho](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="e1d84-205">O **estilos** torna-o fácil de testar e visualizar CSS alterações antes de confirmar as alterações para o estilo a janela da folha em si.</span><span class="sxs-lookup"><span data-stu-id="e1d84-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="e1d84-206">Sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="e1d84-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="e1d84-207">Este recurso requer a versão 1.3 do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="e1d84-208">O recurso de sincronização automática de CSS permite que você editar um arquivo CSS diretamente e ver as alterações imediatamente no navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="e1d84-209">Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e1d84-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e1d84-210">No navegador do Inspetor de página, mova o ponteiro do mouse na seção "Home Page" até que o **div.content wrapper** rótulo aparece.</span><span class="sxs-lookup"><span data-stu-id="e1d84-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="e1d84-211">Clique para selecionar esse elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-211">Click once to select this element.</span></span>

<span data-ttu-id="e1d84-212">O **Syles** janela mostra todas as regras CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="e1d84-213">Role para baixo até localizar o wrapper de. Content .featured seletor de classe.</span><span class="sxs-lookup"><span data-stu-id="e1d84-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="e1d84-214">Clique em ".featured. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="e1d84-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="e1d84-215">Inspetor de página abre o arquivo CSS que define esse estilo (site) e realça o estilo CSS correspondente.</span><span class="sxs-lookup"><span data-stu-id="e1d84-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="e1d84-216">Agora, altere o valor de `background-color` para "vermelho".</span><span class="sxs-lookup"><span data-stu-id="e1d84-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="e1d84-217">A alteração aparece imediatamente no navegador do Inspetor de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="e1d84-218">Usando o seletor de cores CSS</span><span class="sxs-lookup"><span data-stu-id="e1d84-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="e1d84-219">O editor de CSS no Visual Studio 2012 tem um seletor de cores que facilita a escolha e inserir cores.</span><span class="sxs-lookup"><span data-stu-id="e1d84-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="e1d84-220">O seletor de cor inclui uma paleta padrão de cores, dá suporte a nomes de cores padrão, códigos hash, cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores usado mais recentemente do documento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="e1d84-221">Na seção anterior, você alterou o valor da `background-color` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e1d84-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="e1d84-222">Para invocar o seletor de cores, coloque o ponto de inserção depois do nome da propriedade e digite **#** ou **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![A barra de seletor de cores CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="e1d84-224">Clique em uma cor para selecioná-lo, ou pressione a tecla de seta para baixo e, em seguida, use as teclas de seta para esquerda e direita para percorrer as cores.</span><span class="sxs-lookup"><span data-stu-id="e1d84-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="e1d84-225">Quando você visita uma cor, o valor hexadecimal correspondente é visualizado:</span><span class="sxs-lookup"><span data-stu-id="e1d84-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![valor de propriedade de cor de plano de fundo visualizado](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="e1d84-227">Se a barra de cores não tem a cor exata desejada, você pode usar o seletor de cor pop-down.</span><span class="sxs-lookup"><span data-stu-id="e1d84-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="e1d84-228">Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores ou pressione a seta para baixo de uma ou duas vezes no teclado.</span><span class="sxs-lookup"><span data-stu-id="e1d84-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Seletor de cores CSS Pop-Down](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="e1d84-230">Clique em uma cor de barra vertical no lado direito.</span><span class="sxs-lookup"><span data-stu-id="e1d84-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="e1d84-231">Isso mostra um gradiente dessa cor na janela principal.</span><span class="sxs-lookup"><span data-stu-id="e1d84-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="e1d84-232">Escolha uma cor diretamente na barra de ferramentas vertical, pressionando Enter ou clique em qualquer ponto na janela principal para escolher com maior precisão.</span><span class="sxs-lookup"><span data-stu-id="e1d84-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="e1d84-233">Se houver uma cor na tela do computador que você deseja usar (ele não precisa estar dentro da interface de usuário do Visual Studio), você pode capturar o seu valor usando a ferramenta de conta-gotas no canto inferior direito.</span><span class="sxs-lookup"><span data-stu-id="e1d84-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="e1d84-234">Você também pode alterar a opacidade da cor movendo o controle deslizante na parte inferior do seletor de cores.</span><span class="sxs-lookup"><span data-stu-id="e1d84-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="e1d84-235">Se o fizer, alterações de valores para valores RGBA cores porque o formato RGBA pode representar a opacidade.</span><span class="sxs-lookup"><span data-stu-id="e1d84-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="e1d84-236">Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor de plano de fundo no *Site.css* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="e1d84-237">A barra de atualização do Inspetor de página</span><span class="sxs-lookup"><span data-stu-id="e1d84-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="e1d84-238">O Page Inspector imediatamente detecta a alteração de *Site.css* de arquivos e exibe um alerta em uma barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="e1d84-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barra de atualização](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="e1d84-240">Para salvar todos os arquivos e atualizar o navegador do Inspetor de página, pressione Ctrl + Alt + Enter ou clique na barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="e1d84-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="e1d84-241">A alteração na cor de realce é exibida no navegador.</span><span class="sxs-lookup"><span data-stu-id="e1d84-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="e1d84-242">Mapeamento de elementos dinâmicos de páginas para JavaScript</span><span class="sxs-lookup"><span data-stu-id="e1d84-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="e1d84-243">Aplicativos web modernos, elementos da página geralmente são gerados dinamicamente com JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e1d84-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="e1d84-244">Isso significa que não há nenhuma marcação estática (HTML ou Razor) que corresponde a esses elementos de página.</span><span class="sxs-lookup"><span data-stu-id="e1d84-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="e1d84-245">Com a versão 1.3, Page Inspector agora pode mapear itens que foram adicionados dinamicamente para a página de volta para o código JavaScript correspondente.</span><span class="sxs-lookup"><span data-stu-id="e1d84-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="e1d84-246">Para demonstrar esse recurso, usaremos o [modelo de aplicativo de página única (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="e1d84-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e1d84-247">O modelo do SPA requer o [ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) atualizar.</span><span class="sxs-lookup"><span data-stu-id="e1d84-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="e1d84-248">No Visual Studio, escolha **arquivo** &gt; **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="e1d84-249">À esquerda, expanda **Visual C#**, selecione **Web**e, em seguida, selecione **aplicativo Web do ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="e1d84-250">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-250">Click **OK**.</span></span>

<span data-ttu-id="e1d84-251">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de página única**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="e1d84-252">No Gerenciador de soluções, expanda o **exibições** pasta e, em seguida, o **início** pasta.</span><span class="sxs-lookup"><span data-stu-id="e1d84-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="e1d84-253">O arquivo cshtml clique com botão direito e escolha **exibir em Inspetor de página**.</span><span class="sxs-lookup"><span data-stu-id="e1d84-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="e1d84-254">A primeira coisa que é exibido no navegador do Inspetor de página é uma página de logon.</span><span class="sxs-lookup"><span data-stu-id="e1d84-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="e1d84-255">Clique em "Inscrever-se" e crie um nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="e1d84-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="e1d84-256">Quando você se inscrever, o aplicativo registra você e cria uma lista de tarefas com alguns itens de exemplo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="e1d84-257">Clique em **inspecionar** para colocar o Inspetor de página no modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="e1d84-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="e1d84-258">No navegador do Inspetor de página, clique em um dos itens de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="e1d84-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="e1d84-259">Observe que, em vez de ser realçado em azul, o elemento é realçado em laranja, com "JS" ao lado do nome do elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="e1d84-260">Isso indica que o elemento foi criado dinamicamente por meio de script.</span><span class="sxs-lookup"><span data-stu-id="e1d84-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="e1d84-261">Além disso, um sublinhado laranja aparece no **pilha de chamadas** guia. Isso indica que o **pilha de chamadas** painel traz mais informações sobre o elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="e1d84-262">Clique no **pilha de chamadas** guia. O **pilha de chamadas** painel mostra a pilha de chamadas para a chamada de JavaScript que criou o elemento.</span><span class="sxs-lookup"><span data-stu-id="e1d84-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="e1d84-263">Chama a bibliotecas externas, como jQuery são recolhidas, para que você possa ver facilmente as chamadas para o seu script de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e1d84-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="e1d84-264">Para ver a pilha completa, inclusive chamadas para bibliotecas externas, você pode expandir os nós de rotulado como "Bibliotecas externas":</span><span class="sxs-lookup"><span data-stu-id="e1d84-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="e1d84-265">Se você clicar em um item na pilha de chamadas, o Visual Studio abre o arquivo de código e realça o script correspondente.</span><span class="sxs-lookup"><span data-stu-id="e1d84-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="e1d84-266">Consulte também</span><span class="sxs-lookup"><span data-stu-id="e1d84-266">See Also</span></span>

<span data-ttu-id="e1d84-267">[Introdução ao ASP.NET MVC 4 com o Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (site ASP.net)</span><span class="sxs-lookup"><span data-stu-id="e1d84-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="e1d84-268">[Apresentando o Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo do Channel 9)</span><span class="sxs-lookup"><span data-stu-id="e1d84-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="e1d84-269">[Mensagens de erro do Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="e1d84-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
