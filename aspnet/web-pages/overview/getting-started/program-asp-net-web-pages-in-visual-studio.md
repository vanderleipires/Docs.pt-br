---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Páginas de programação da Web do ASP.NET (Razor) usando o Visual Studio | Microsoft Docs
author: tfitzmac
description: Este apêndice explica como você pode usar o Visual Studio 2010 ou Visual Web Developer 2010 Express para programa de páginas da Web ASP.NET com a sintaxe do Razor.
ms.author: aspnetcontent
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 46807b464499b2e60d995d37f161ca129d38f439
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834449"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="73ecf-103">Programação de páginas da Web ASP.NET (Razor) usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73ecf-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="73ecf-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="73ecf-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="73ecf-105">Este artigo explica como você pode usar o Visual Studio ou Visual Web Developer Express para sites de páginas da Web do ASP.NET (Razor) do programa.</span><span class="sxs-lookup"><span data-stu-id="73ecf-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="73ecf-106">O que você aprenderá</span><span class="sxs-lookup"><span data-stu-id="73ecf-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="73ecf-107">O que você precisa instalar (se qualquer coisa) para trabalhar com páginas da Web do ASP.NET na sua versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73ecf-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="73ecf-108">Como adicionar suporte para páginas da Web do ASP.NET para o Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="73ecf-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="73ecf-109">Como usar recursos no Visual Studio para trabalhar com páginas Razor do ASP.NET, incluindo IntelliSense e o depurador.</span><span class="sxs-lookup"><span data-stu-id="73ecf-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73ecf-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="73ecf-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="73ecf-111">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="73ecf-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="73ecf-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="73ecf-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="73ecf-113">O WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="73ecf-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="73ecf-114">Este tutorial também funciona com ASP.NET Web Pages 2, o Visual Studio 2012, o Visual Studio 2010 e o WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="73ecf-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="73ecf-115">Você pode programar páginas da Web ASP.NET com sintaxe do Razor usando o WebMatrix ou outros editores de código.</span><span class="sxs-lookup"><span data-stu-id="73ecf-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="73ecf-116">Você também pode usar o Microsoft Visual Studio, que é um ambiente completo de desenvolvimento integrado (IDE) que fornece um conjunto poderoso de ferramentas para criar vários tipos de aplicativos (não apenas sites).</span><span class="sxs-lookup"><span data-stu-id="73ecf-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="73ecf-117">Para trabalhar com páginas Razor do ASP.NET, você pode usar uma das edições do Visual Studio completas ou a versão gratuita [Visual Studio Express para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span><span class="sxs-lookup"><span data-stu-id="73ecf-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="73ecf-118">Dois recursos particularmente útil que o Visual Studio fornece para a programação com páginas da web de ASP.NET Razor são:</span><span class="sxs-lookup"><span data-stu-id="73ecf-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="73ecf-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="73ecf-119">*IntelliSense*.</span></span> <span data-ttu-id="73ecf-120">O recurso IntelliSense incorporado ao Visual Studio é mais abrangente do que o IntelliSense no WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="73ecf-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="73ecf-121">*Depurador*.</span><span class="sxs-lookup"><span data-stu-id="73ecf-121">*Debugger*.</span></span> <span data-ttu-id="73ecf-122">O depurador permite solucionar problemas de seu código, interrompendo um programa enquanto está em execução, examinar variáveis e percorrendo o código linha por linha.</span><span class="sxs-lookup"><span data-stu-id="73ecf-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="73ecf-123">Usando o Visual Studio com diferentes versões de páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="73ecf-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="73ecf-124">Visual Studio 2012 e o Visual Studio 2013 incluem suporte para páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="73ecf-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="73ecf-125">(Os pacotes que são necessários para dar suporte a páginas da Web ASP.NET são instalados quando você instala o Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="73ecf-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="73ecf-126">Visual Studio 2010 não inclui suporte por padrão para páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="73ecf-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="73ecf-127">Para usar páginas da Web ASP.NET com o Visual Studio 2010, você deve instalar o pacote do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="73ecf-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="73ecf-128">Para obter o ASP.NET Web Pages 2, você instalar o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="73ecf-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="73ecf-129">A tabela a seguir resume o suporte para páginas da Web do ASP.NET em diferentes versões do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73ecf-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="73ecf-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="73ecf-130">Visual Studio 2010</span></span> | <span data-ttu-id="73ecf-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="73ecf-131">Visual Studio 2012</span></span> | <span data-ttu-id="73ecf-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="73ecf-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="73ecf-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="73ecf-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="73ecf-134">Instalar o ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="73ecf-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="73ecf-135">(Incluído)</span><span class="sxs-lookup"><span data-stu-id="73ecf-135">(Included)</span></span> | <span data-ttu-id="73ecf-136">(Incluído)</span><span class="sxs-lookup"><span data-stu-id="73ecf-136">(Included)</span></span> |
| <span data-ttu-id="73ecf-137">**Páginas da Web do ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="73ecf-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="73ecf-138">Atualização da Web do ASP.NET 3 NuGet por meio de páginas</span><span class="sxs-lookup"><span data-stu-id="73ecf-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="73ecf-139">(Incluído)</span><span class="sxs-lookup"><span data-stu-id="73ecf-139">(Included)</span></span> |

<span data-ttu-id="73ecf-140">Para trabalhar com o Visual Studio 2010, consulte [instalando o suporte para páginas da Web ASP.NET no Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="73ecf-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="73ecf-141">Iniciar o Visual Studio no WebMatrix</span><span class="sxs-lookup"><span data-stu-id="73ecf-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="73ecf-142">Se você iniciou o projeto no WebMatrix e deseja alternar para o Visual Studio, o WebMatrix fornece um botão para abrir facilmente o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73ecf-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="73ecf-143">Você deve ter o Visual Studio instalado em seu computador para que esse botão seja habilitado.</span><span class="sxs-lookup"><span data-stu-id="73ecf-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="73ecf-144">A imagem a seguir mostra o botão no WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="73ecf-144">The following image shows the button in WebMatrix.</span></span>

![Inicie o Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="73ecf-146">Quando você clica no botão, o projeto é aberto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73ecf-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="73ecf-147">Você pode alternar entre o WebMatrix e Visual Studio sem problemas.</span><span class="sxs-lookup"><span data-stu-id="73ecf-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="73ecf-148">Você será notificado se todos os arquivos foram alterados em outro ambiente e precisam ser recarregado para obter as últimas alterações.</span><span class="sxs-lookup"><span data-stu-id="73ecf-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="73ecf-149">Criando o Site do ASP.NET Razor no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73ecf-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="73ecf-150">Para criar um site do ASP.NET Razor no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="73ecf-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="73ecf-151">Inicie o Visual Studio ou Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="73ecf-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="73ecf-152">No **arquivo** menu, clique em **New Web Site**.</span><span class="sxs-lookup"><span data-stu-id="73ecf-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Criar novo site da web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="73ecf-154">No **New Web Site** caixa de diálogo, selecione o idioma a ser usado (Visual c# ou Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="73ecf-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="73ecf-155">Selecione o **Site da Web do ASP.NET (Razor)** modelo.</span><span class="sxs-lookup"><span data-stu-id="73ecf-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![site do Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="73ecf-157">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="73ecf-157">Click **OK**.</span></span>

<span data-ttu-id="73ecf-158">Seu novo projeto existe e é preenchido com algumas páginas da web padrão para ajudar você a começar.</span><span class="sxs-lookup"><span data-stu-id="73ecf-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="73ecf-159">Usando IntelliSense</span><span class="sxs-lookup"><span data-stu-id="73ecf-159">Using IntelliSense</span></span>

<span data-ttu-id="73ecf-160">Agora que você criou um site, você pode ver o funcionamento do IntelliSense no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73ecf-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="73ecf-161">No site que você acabou de criar, abrir o *cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="73ecf-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="73ecf-162">Após o `<h3>` marcas na página, digite `@ServerInfo.` (incluindo o ponto).</span><span class="sxs-lookup"><span data-stu-id="73ecf-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="73ecf-163">Observe como o IntelliSense exibe os métodos disponíveis para o `ServerInfo` auxiliar em uma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="73ecf-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="73ecf-165">Selecione o `GetHtml` método na lista e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="73ecf-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="73ecf-166">IntelliSense preenche automaticamente o método.</span><span class="sxs-lookup"><span data-stu-id="73ecf-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="73ecf-167">(Como com qualquer método em c#, você deve adicionar `()` caracteres após o método.)</span><span class="sxs-lookup"><span data-stu-id="73ecf-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
   <span data-ttu-id="73ecf-168">O código completo para o `GetHtml` método é semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="73ecf-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="73ecf-169">Pressione Ctrl + F5 para executar a página.</span><span class="sxs-lookup"><span data-stu-id="73ecf-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="73ecf-170">Isso é a página de aparência quando exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="73ecf-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![página padrão no navegador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="73ecf-172">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="73ecf-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="73ecf-173">Usando o depurador</span><span class="sxs-lookup"><span data-stu-id="73ecf-173">Using the Debugger</span></span>

1. <span data-ttu-id="73ecf-174">Na parte superior a *default. cshtml* página, após a linha que começa com `Page.Title`, adicione a seguinte linha de código:</span><span class="sxs-lookup"><span data-stu-id="73ecf-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="73ecf-175">Na margem cinza do editor para a esquerda do código, clique em Avançar essa nova linha para adicionar um *ponto de interrupção*.</span><span class="sxs-lookup"><span data-stu-id="73ecf-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="73ecf-176">Um ponto de interrupção é um marcador que informa o depurador para interromper a execução do programa nesse momento, para que você possa ver o que está acontecendo.</span><span class="sxs-lookup"><span data-stu-id="73ecf-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Definir ponto de interrupção](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="73ecf-178">Remova a chamada para o `ServerInfo.GetHtml` método e adicione uma chamada para o `@myTime` variável em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="73ecf-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="73ecf-179">Essa chamada exibe o valor de tempo atual que é retornado por nova linha de código.</span><span class="sxs-lookup"><span data-stu-id="73ecf-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="73ecf-180">Pressione F5 para executar a página no depurador.</span><span class="sxs-lookup"><span data-stu-id="73ecf-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="73ecf-181">A página para no ponto de interrupção que você definir.</span><span class="sxs-lookup"><span data-stu-id="73ecf-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="73ecf-182">A imagem a seguir mostra como a página aparece no editor com o ponto de interrupção (em amarelo).</span><span class="sxs-lookup"><span data-stu-id="73ecf-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![ponto de interrupção de depuração](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="73ecf-184">Na barra de ferramentas de depuração, clique o **intervir** botão (ou pressione F11) para executar a próxima linha de código.</span><span class="sxs-lookup"><span data-stu-id="73ecf-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="73ecf-185">Cada vez que você clicar nesse botão, avance a execução para a próxima linha de código.</span><span class="sxs-lookup"><span data-stu-id="73ecf-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Entrar em um botão](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="73ecf-187">Examinar o valor da `myTime` variável, mantendo o ponteiro do mouse sobre ele ou inspecionando os valores exibidos na **Locals** e **pilha de chamadas** windows.</span><span class="sxs-lookup"><span data-stu-id="73ecf-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="73ecf-188">Visual Studio exibe o valor da variável.</span><span class="sxs-lookup"><span data-stu-id="73ecf-188">Visual Studio display the value of the variable.</span></span>

    ![valor de hora do show](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="73ecf-190">Quando você terminar de examinar a variável e percorrer o código, pressione F5 para continuar a execução da página sem parar em cada linha.</span><span class="sxs-lookup"><span data-stu-id="73ecf-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="73ecf-191">Quando você tiver terminado de percorrer todo o código, o navegador exibe a página.</span><span class="sxs-lookup"><span data-stu-id="73ecf-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="73ecf-192">Para saber mais sobre o depurador e sobre como depurar o código no Visual Studio, consulte [instruções passo a passo: depuração de páginas da Web no Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="73ecf-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="73ecf-193">Usando o Razor em projetos do ASP.NET MVC com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73ecf-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="73ecf-194">A sintaxe do Razor é também usada extensivamente em projetos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="73ecf-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="73ecf-195">O MVC é uma maneira eficiente com base em padrões para criar sites dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="73ecf-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="73ecf-196">Se seu site de páginas da Web ASP.NET se torna difícil de manter, convém considerar convertê-la em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="73ecf-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="73ecf-197">Para obter um exemplo de criação de um aplicativo MVC, consulte [Introdução ao ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="73ecf-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="73ecf-198">Instalando o suporte para páginas da Web do ASP.NET no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="73ecf-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="73ecf-199">Esta seção mostra como instalar o Visual Web Developer Express 2010 e as ferramentas de páginas da Web do ASP.NET para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73ecf-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="73ecf-200">Se você ainda não tiver o Web Platform Installer, você deve baixá-lo da seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="73ecf-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="73ecf-201">Execute o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="73ecf-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="73ecf-202">Clique o **produtos** guia.</span><span class="sxs-lookup"><span data-stu-id="73ecf-202">Click the **Products** tab.</span></span>

    ![Guia de produtos do WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="73ecf-204">Pesquise **ASP.NET MVC 4** (para ASP.NET Web Pages 2) e, em seguida, clique em **Add**.</span><span class="sxs-lookup"><span data-stu-id="73ecf-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="73ecf-205">Esses produtos incluem ferramentas do Visual Studio para a criação de sites do ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="73ecf-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opções de instalação do WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="73ecf-207">Clique em **instalar** para concluir a instalação.</span><span class="sxs-lookup"><span data-stu-id="73ecf-207">Click **Install** to complete the installation.</span></span>
