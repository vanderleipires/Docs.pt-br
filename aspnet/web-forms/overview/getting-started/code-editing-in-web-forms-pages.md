---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edição de código Web Forms do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="a8f5c-102">Código de edição Web Forms do ASP.NET no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a8f5c-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="a8f5c-103">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a8f5c-104">Em muitas páginas de formulário Web do ASP.NET, você escreve código no Visual Basic, c# ou outro idioma.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="a8f5c-105">O editor de código no Visual Studio pode ajudá-lo a escrever código rapidamente ao ajudá-lo a evitar erros.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="a8f5c-106">Além disso, o editor fornece maneiras para você criar códigos reutilizáveis para ajudar a reduzir a quantidade de trabalho que você precisa fazer.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="a8f5c-107">Este passo a passo ilustra vários recursos do editor de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="a8f5c-108">Durante este passo a passo, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="a8f5c-109">Corrija os erros de codificação embutido.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="a8f5c-110">Refatorar e renomear código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-110">Refactor and rename code.</span></span>
- <span data-ttu-id="a8f5c-111">Renomear variáveis e objetos.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-111">Rename variables and objects.</span></span>
- <span data-ttu-id="a8f5c-112">Inserir trechos de código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8f5c-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="a8f5c-113">Prerequisites</span></span>


<span data-ttu-id="a8f5c-114">Para concluir este passo a passo, você precisará de:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="a8f5c-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="a8f5c-116">O .NET Framework é instalado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="a8f5c-117">Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecida como Visual Studio em toda a série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="a8f5c-118">Se você estiver usando o Visual Studio, este passo a passo pressupõe que você selecionou o **desenvolvimento Web** conjunto de configurações na primeira vez que você iniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="a8f5c-119">Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

  <span data-ttu-id="a8f5c-120">Para obter uma introdução ao Visual Studio e ASP.NET, consulte [criando uma página de Web Forms do ASP.NET 4.5 básica no Visual Studio 2013](creating-a-basic-web-forms-page.md).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="a8f5c-121">Criando um projeto de aplicativo Web e uma página</span><span class="sxs-lookup"><span data-stu-id="a8f5c-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="a8f5c-122">Nesta parte do passo a passo, você cria um projeto de aplicativo Web e adicionar uma nova página a ele.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="a8f5c-123">Para criar um projeto de aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="a8f5c-123">To create a Web application project</span></span>

1. <span data-ttu-id="a8f5c-124">Abra o Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="a8f5c-125">No menu **Arquivo**, selecione **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="a8f5c-126">![Menu arquivo](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="a8f5c-127">A caixa de diálogo **Novo Projeto** é exibida.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="a8f5c-128">Selecione o **modelos**  - &gt; **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="a8f5c-129">Escolha o **aplicativo Web ASP.NET** modelo na coluna central.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="a8f5c-130">Nomeie o projeto ***BasicWebApp*** e clique no **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="a8f5c-131">![Caixa de diálogo Novo projeto](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="a8f5c-132">Em seguida, selecione o **Web Forms** modelo e clique no **Okey** botão para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="a8f5c-133">![Caixa de diálogo Novo projeto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="a8f5c-134">Visual Studio cria um novo projeto que inclui a funcionalidade predefinida com base no modelo de Web Forms.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="a8f5c-135">Criar uma nova página Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a8f5c-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="a8f5c-136">Quando você cria um novo aplicativo do Web Forms usando o **aplicativo Web ASP.NET** modelo de projeto, o Visual Studio adiciona uma página do ASP.NET (página Web Forms) chamada *Default.aspx*, bem como vários outros arquivos e pastas.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="a8f5c-137">Você pode usar o *Default.aspx* página como a home page do seu aplicativo da Web.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="a8f5c-138">No entanto, para este passo a passo, você irá criar e trabalhar com uma nova página.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="a8f5c-139">Para adicionar uma página para o aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="a8f5c-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="a8f5c-140">Em **Solution Explorer**, clique no nome do aplicativo Web (neste tutorial é o nome do aplicativo **BasicWebSite**) e, em seguida, clique em **adicionar**  - &gt; **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="a8f5c-141">A caixa de diálogo **Adicionar Novo Item** é exibida.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="a8f5c-142">Selecione o **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="a8f5c-143">Em seguida, selecione **formulário da Web** do meio lista e nomeie-o *FirstWebPage*.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="a8f5c-144">![Adicionar caixa de diálogo Novo Item](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="a8f5c-145">Clique em **adicionar** para adicionar a página de Web Forms ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="a8f5c-146">Visual Studio cria a nova página e abre-o.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="a8f5c-147">Em seguida, defina essa nova página como a página de inicialização padrão.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="a8f5c-148">Em **Solution Explorer**, clique com botão direito a nova página chamada *FirstWebPage* e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="a8f5c-149">Na próxima vez que você executa esse aplicativo para testar o nosso progresso automaticamente verá essa nova página no navegador.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="a8f5c-150">Corrigindo embutido erros de codificação</span><span class="sxs-lookup"><span data-stu-id="a8f5c-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="a8f5c-151">O editor de código no Visual Studio ajuda a evitar erros conforme você escrever o código e se você fez um erro, o editor de códigos ajuda você a corrigir o erro.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="a8f5c-152">Nesta parte do passo a passo, você irá escrever uma linha de código que ilustram os recursos de correção de erro no editor.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="a8f5c-153">Para corrigir erros de código simples no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f5c-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="a8f5c-154">Em **Design** exibir, clique duas vezes na página em branco para criar um manipulador para o **carga** evento para a página.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="a8f5c-155">Você está usando o manipulador de eventos somente como um local para escrever um código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="a8f5c-156">Dentro do manipulador, digite a seguinte linha que contém um erro e pressione **ENTER**:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="a8f5c-157">Quando você pressiona **ENTER**, o editor de código coloca um sublinhado vermelho e verde (normalmente chamadas &quot;curvadas&quot; linhas) em áreas do código que apresentam problemas.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="a8f5c-158">Um sublinhado verde indica um aviso.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-158">A green underline indicates a warning.</span></span> <span data-ttu-id="a8f5c-159">Um sublinhado vermelho indica um erro que você deve corrigir.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="a8f5c-160">Mantenha o ponteiro do mouse sobre `myStr` para ver uma dica de ferramenta que informa você sobre o aviso.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="a8f5c-161">Além disso, mantenha o ponteiro do mouse sobre o sublinhado vermelho para ver a mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="a8f5c-162">A imagem a seguir mostra o código com os sublinhados.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="a8f5c-163">![Bem-vindo texto no modo de Design](code-editing-in-web-forms-pages/_static/image5.png "bem-vindo texto no modo de Design")</span><span class="sxs-lookup"><span data-stu-id="a8f5c-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="a8f5c-164">O erro deve ser corrigido adicionando um ponto e vírgula `;` ao final da linha.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="a8f5c-165">O aviso simplesmente notifica que você não tiver usado o `myStr` variável ainda.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="a8f5c-166">Exibir o código atual configurações no Visual Studio de formatação selecionando **ferramentas**  - &gt; **opções**  - &gt; **fontes e Cores**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="a8f5c-167">Refatoração e renomear</span><span class="sxs-lookup"><span data-stu-id="a8f5c-167">Refactoring and Renaming</span></span>

<span data-ttu-id="a8f5c-168">Refatoração é uma metodologia de software que envolve reestruturação do seu código para tornar mais fácil de entender e manter, preservando sua funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="a8f5c-169">Um exemplo simples pode ser que você escreva o código em um manipulador de eventos para obter dados de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="a8f5c-170">À medida que desenvolve sua página, você descobre que você precisa acessar os dados de vários manipuladores diferentes.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="a8f5c-171">Portanto, você refatorar o código da página Criando um método de acesso a dados na página e inserindo chamadas para o método nos manipuladores.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="a8f5c-172">O editor de código inclui ferramentas para ajudá-lo a executar várias tarefas de refatoração.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="a8f5c-173">Neste passo a passo, você trabalhará com duas técnicas de refatoração: renomear variáveis e métodos de extração.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="a8f5c-174">Outras opções de refatoração incluem encapsular campos, promover variáveis locais para parâmetros de método e gerenciar parâmetros de método.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="a8f5c-175">A disponibilidade dessas opções de refatoração depende do local no código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="a8f5c-176">Refatoração do código</span><span class="sxs-lookup"><span data-stu-id="a8f5c-176">Refactoring Code</span></span>

<span data-ttu-id="a8f5c-177">Um cenário de refatoração comum é criar (Extrair) um método do código que está dentro de outro membro, como um método.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="a8f5c-178">Isso reduz o tamanho do membro original e torna o código extraído reutilizável.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="a8f5c-179">Nesta parte do passo a passo, você escrever códigos simples e, em seguida, extrair um método dele.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="a8f5c-180">Refatoração é suportada para c#, então você irá criar uma página que usa c# como sua linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="a8f5c-181">Para extrair um método em uma página c#</span><span class="sxs-lookup"><span data-stu-id="a8f5c-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="a8f5c-182">Alternar para **Design** exibição.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="a8f5c-183">No **caixa de ferramentas**, do **padrão** guia, arraste um [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) até a página.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="a8f5c-184">Clique duas vezes o **botão** controle para criar um manipulador para seu [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento e, em seguida, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="a8f5c-185">O código cria um **ArrayList** objeto, usa um loop para carregá-lo com valores e então usa outro loop para exibir o conteúdo do **ArrayList** objeto.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="a8f5c-186">Pressione **CTRL + F5** para executar a página e, em seguida, clique no **botão** para certificar-se de que você verá a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="a8f5c-187">Retornar ao editor de códigos e, em seguida, selecione as linhas seguintes no manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="a8f5c-188">A seleção, clique **refatorar**e, em seguida, escolha **extrair método**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="a8f5c-189">O **extrair método** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="a8f5c-190">No **nome do novo método** , digite **DisplayArray**e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="a8f5c-191">O editor de código cria um novo método chamado `DisplayArray`e coloca uma chamada para o novo método no **clique** manipulador onde o loop estava originalmente.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="a8f5c-192">Pressione **CTRL + F5** para executar a página novamente e, em seguida, clique no **botão**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="a8f5c-193">A página funciona como antes.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-193">The page functions the same as it did before.</span></span> <span data-ttu-id="a8f5c-194">O `DisplayArray` método agora pode ser chamada em qualquer lugar na classe de página.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="a8f5c-195">Renomeando variáveis</span><span class="sxs-lookup"><span data-stu-id="a8f5c-195">Renaming Variables</span></span>

<span data-ttu-id="a8f5c-196">Ao trabalhar com variáveis, bem como objetos, convém renomeá-los depois que eles já estão referenciados em seu código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="a8f5c-197">No entanto, a renomeação de variáveis e objetos pode causar quebra se você esquecer de renomear uma das referências do código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="a8f5c-198">Portanto, você pode usar a refatoração para executar a renomeação.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="a8f5c-199">Para usar a refatoração Renomear uma variável</span><span class="sxs-lookup"><span data-stu-id="a8f5c-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="a8f5c-200">No **clique** manipulador de eventos, localize a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="a8f5c-201">Clique no nome de variável `alist`, escolha **refatorar**e, em seguida, escolha **Renomear**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="a8f5c-202">O **Renomear** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="a8f5c-203">No **novo nome** , digite **ArrayList1** e verifique se o **visualizar alterações de referência** caixa de seleção foi selecionada.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="a8f5c-204">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-204">Then click **OK**.</span></span>

    <span data-ttu-id="a8f5c-205">O **visualizar alterações** caixa de diálogo aparece e exibe uma árvore que contém todas as referências para a variável que você está renomeando.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="a8f5c-206">Clique em **aplicar** para fechar o **visualizar alterações** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="a8f5c-207">As variáveis que fazem referência especificamente a instância que você selecionou são renomeadas.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="a8f5c-208">No entanto, observe que a variável `alist` na linha a seguir não é renomeada.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="a8f5c-209">A variável `alist` essa linha não é renomeada porque ela não representa o mesmo valor que a variável `alist` que você renomeou.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="a8f5c-210">A variável `alist` no `DisplayArray` declaração é uma variável local para esse método.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="a8f5c-211">Isso ilustra que usando refatoração Renomear variáveis é diferente de simplesmente executar uma ação de localização e substituição no editor; Refatoração renomeia variáveis com conhecimento da semântica da variável que ele está funcionando com.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="a8f5c-212">Inserindo trechos</span><span class="sxs-lookup"><span data-stu-id="a8f5c-212">Inserting Snippets</span></span>

<span data-ttu-id="a8f5c-213">Como há muitas tarefas de codificação que os desenvolvedores de Web Forms frequentemente precisam executar, o editor de código fornece uma biblioteca de trechos de código ou blocos de código boa.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="a8f5c-214">Você pode inserir trechos de código em sua página.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="a8f5c-215">Cada linguagem que você usa no Visual Studio tem pequenas diferenças na maneira como inserir trechos de código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="a8f5c-216">Para obter informações sobre como inserir trechos de código, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="a8f5c-217">Para obter informações sobre como inserir trechos de código no Visual c#, consulte [trechos de código do Visual c#](https://msdn.microsoft.com/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8f5c-218">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="a8f5c-218">Next Steps</span></span>

<span data-ttu-id="a8f5c-219">Este passo a passo ilustra os recursos básicos do editor de códigos do Visual Studio 2010 para corrigir erros em seu código, refatoração de código, renomeando variáveis e inserindo trechos de código em seu código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="a8f5c-220">Recursos adicionais no editor podem tornar o desenvolvimento de aplicativo rápido e fácil.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="a8f5c-221">Por exemplo, é possível:</span><span class="sxs-lookup"><span data-stu-id="a8f5c-221">For example, you might want to:</span></span>

- <span data-ttu-id="a8f5c-222">Saiba mais sobre os recursos do IntelliSense, como modificar opções do IntelliSense, gerenciar trechos de código e a pesquisa de trechos de código online.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="a8f5c-223">Para obter mais informações, veja [Usando o IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="a8f5c-224">Aprenda a criar seus próprios trechos de código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="a8f5c-225">Para obter mais informações, consulte [criando e usando trechos de código do IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="a8f5c-226">Saiba mais sobre os recursos específicos do Visual Basic de trechos de código do IntelliSense, como personalizar os trechos de código e solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="a8f5c-227">Para obter mais informações, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="a8f5c-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="a8f5c-228">Saiba mais sobre o c#-recursos específicos do IntelliSense, como refatoração e trechos de código.</span><span class="sxs-lookup"><span data-stu-id="a8f5c-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="a8f5c-229">Para obter mais informações, consulte [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8f5c-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
