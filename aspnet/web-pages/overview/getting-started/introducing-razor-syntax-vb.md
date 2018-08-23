---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introdução à programação Web do ASP.NET usando a sintaxe do Razor (Visual Basic) | Microsoft Docs
author: tfitzmac
description: Este apêndice fornece uma visão geral da programação com páginas da Web ASP.NET no Visual Basic, usando a sintaxe do Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825181"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="d8bbd-103">Introdução à programação Web do ASP.NET usando a sintaxe do Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="d8bbd-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d8bbd-105">Este artigo fornece uma visão geral da programação com páginas da Web do ASP.NET usando a sintaxe do Razor e o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="d8bbd-106">O ASP.NET é uma tecnologia da Microsoft para a execução de páginas da web dinâmicas em servidores web.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="d8bbd-107">**O que você vai aprender**:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="d8bbd-108">Os 8 principais dicas para começar a programação de páginas Web ASP.NET usando a sintaxe do Razor de programação.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="d8bbd-109">Conceitos básicos de programação, será necessário.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="d8bbd-110">Qual código de servidor ASP.NET e a sintaxe do Razor é tudo sobre.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="d8bbd-111">Versões de software</span><span class="sxs-lookup"><span data-stu-id="d8bbd-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="d8bbd-112">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d8bbd-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d8bbd-113">Este tutorial também funciona com ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="d8bbd-114">A maioria dos exemplos de como usar páginas da Web ASP.NET com sintaxe do Razor usar c#.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="d8bbd-115">Mas a sintaxe do Razor também suporta Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="d8bbd-116">Para programar uma página da web ASP.NET no Visual Basic, você cria uma página da web com um *. vbhtml* extensão de nome de arquivo e, em seguida, adicione o código do Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="d8bbd-117">Este artigo fornece uma visão geral de como trabalhar com a linguagem Visual Basic e a sintaxe para criar páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="d8bbd-118">Os modelos de site padrão para o Microsoft WebMatrix (**padaria**, **Galeria de fotos**, e **Starter Site**, etc.) estão disponíveis nas versões c# e Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="d8bbd-119">Você pode instalar os modelos do Visual Basic, como pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="d8bbd-120">Modelos de site são instalados na pasta raiz do seu site em uma pasta chamada *Microsoft Templates*.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="d8bbd-121">Principais dicas de programação 8</span><span class="sxs-lookup"><span data-stu-id="d8bbd-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="d8bbd-122">Esta seção lista algumas dicas que com certeza, você precisa saber como começar a escrever código de servidor ASP.NET usando a sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="d8bbd-123">1. Adicione o código para uma página usando o caractere @</span><span class="sxs-lookup"><span data-stu-id="d8bbd-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="d8bbd-124">O `@` caractere inicia expressões embutidas, os blocos de instrução única e blocos de várias instruções:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="d8bbd-125">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d8bbd-127">**A codificação HTML**</span><span class="sxs-lookup"><span data-stu-id="d8bbd-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="d8bbd-128">Quando você exibe o conteúdo em uma página usando o `@` de caracteres, como nos exemplos anteriores, ASP.NET codifica em HTML a saída.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="d8bbd-129">Isso substitui os caracteres reservados de HTML (tal como `<` e `>` e `&`) com os códigos que permitem que os caracteres a serem exibidos como caracteres em uma página da web, em vez de sejam interpretados como marcações HTML ou entidades.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="d8bbd-130">Sem codificação HTML, a saída do seu código de servidor não seja exibido corretamente e pode expor uma página a riscos de segurança.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="d8bbd-131">Se sua meta é uma marcação HTML que processa marcas como marcação de saída (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinação de texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="d8bbd-132">Você pode ler mais sobre a codificação HTML no [trabalhando com formulários HTML em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="d8bbd-133">2. Coloque blocos de código com o código... Código de fim</span><span class="sxs-lookup"><span data-stu-id="d8bbd-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="d8bbd-134">Um bloco de código inclui uma ou mais instruções de código e está entre as palavras-chave `Code` e `End Code`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="d8bbd-135">Coloque a abertura `Code` palavra-chave imediatamente após o `@` caractere &#8212; não pode haver espaço em branco entre eles.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="d8bbd-136">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="d8bbd-138">3. Dentro de um bloco, você encerrar cada instrução de código com uma quebra de linha</span><span class="sxs-lookup"><span data-stu-id="d8bbd-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="d8bbd-139">Em um bloco de código do Visual Basic, cada instrução termina com uma quebra de linha.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="d8bbd-140">(Posteriormente neste artigo você verá uma maneira para encapsular uma instrução de código longo em várias linhas, se necessário.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="d8bbd-141">4. Usar variáveis para armazenar valores</span><span class="sxs-lookup"><span data-stu-id="d8bbd-141">4. You use variables to store values</span></span>

<span data-ttu-id="d8bbd-142">Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Você cria uma nova variável usando o `Dim` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="d8bbd-143">Você pode inserir valores de variáveis diretamente em uma página usando `@`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="d8bbd-144">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="d8bbd-146">5. Coloque os valores de cadeia de caracteres literal entre aspas duplas</span><span class="sxs-lookup"><span data-stu-id="d8bbd-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="d8bbd-147">Um *cadeia de caracteres* é uma sequência de caracteres que são tratados como texto.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="d8bbd-148">Para especificar uma cadeia de caracteres, coloque-o entre aspas duplas:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="d8bbd-149">Para inserir aspas duplas dentro de um valor de cadeia de caracteres, insira dois caracteres de aspas duplas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="d8bbd-150">Se você quiser que o caractere de aspas duplas para aparecer uma vez em que a saída da página, insira-o como `""` o entre aspas dentro de cadeia de caracteres e se você deseja que ele apareça duas vezes, insira-o como `""""` na cadeia de caracteres entre aspas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="d8bbd-151">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="d8bbd-153">6. Código do Visual Basic não diferencia maiusculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="d8bbd-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="d8bbd-154">A linguagem Visual Basic, não diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="d8bbd-155">Palavras-chave de programação (como `Dim`, `If`, e `True`) e nomes de variáveis (como `myString`, ou `subTotal`) podem ser gravados em qualquer caso.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="d8bbd-156">As seguintes linhas de código de atribuir um valor à variável `lastname` usando em letras minúsculas nomear e saída, em seguida, o valor da variável para a página usando um nome de letras maiusculas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="d8bbd-157">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="d8bbd-159">7. Grande parte da sua codificação envolve o trabalho com objetos</span><span class="sxs-lookup"><span data-stu-id="d8bbd-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="d8bbd-160">Um objeto representa uma coisa que você pode programar com o &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Objetos têm propriedades que descrevem suas características de &#8212; um objeto de caixa de texto tem uma `Text` propriedade, um objeto de solicitação tem um `Url` propriedade, uma mensagem de email tem um `From` propriedade e um objeto do cliente tem um `FirstName` propriedade.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="d8bbd-161">Objetos também têm métodos que são as &quot;verbos&quot; eles podem executar.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="d8bbd-162">Exemplos incluem um objeto de arquivo `Save` método, um objeto de imagem `Rotate` método e um objeto de email `Send` método.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="d8bbd-163">Geralmente, você trabalhará com o `Request` campos de objeto, que fornece informações como os valores de formulário na página (caixas de texto, etc.), o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. Este exemplo mostra como acessar propriedades do `Request` objeto e como chamar o `MapPath` método o `Request` objeto, que fornece o caminho absoluto da página no servidor:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="d8bbd-164">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="d8bbd-166">8. Você pode escrever código que toma decisões</span><span class="sxs-lookup"><span data-stu-id="d8bbd-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="d8bbd-167">Um recurso importante de páginas da web dinâmicas é que você pode determinar o que fazer com base em condições.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="d8bbd-168">A maneira mais comum para isso é com o `If` instrução (e opcional `Else` instrução).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="d8bbd-169">A instrução `If IsPost` é uma forma abreviada da gravação `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="d8bbd-170">Juntamente com `If` instruções, há uma variedade de maneiras de testar condições, repetidas blocos de código, e assim por diante, que são descritos neste artigo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="d8bbd-171">O resultado exibido em um navegador (depois de clicar em **enviar**):</span><span class="sxs-lookup"><span data-stu-id="d8bbd-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d8bbd-173">**HTTP GET e POST métodos e a propriedade IsPost**</span><span class="sxs-lookup"><span data-stu-id="d8bbd-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="d8bbd-174">O protocolo usado para páginas da web (HTTP) dá suporte a um número muito limitado de métodos (&quot;verbos&quot;) que são usados para fazer solicitações ao servidor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="d8bbd-175">Dois os mais comuns são GET, que é usado para ler uma página, e o POST, o que é usado para enviar uma página.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="d8bbd-176">Em geral, na primeira vez que um usuário solicita uma página, a página é solicitada usando GET.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="d8bbd-177">Se o usuário preenche um formulário e, em seguida, clica **enviar**, o navegador faz uma solicitação POST para o servidor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="d8bbd-178">Na programação da web, muitas vezes é útil saber se uma página está sendo solicitada como um GET ou como uma POSTAGEM para que você saiba como processar a página.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="d8bbd-179">Em páginas de Web do ASP.NET, você pode usar o `IsPost` propriedade para ver se uma solicitação é um GET ou uma POSTAGEM.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="d8bbd-180">Se a solicitação for uma POSTAGEM, o `IsPost` propriedade retornará true, e você pode fazer coisas como ler os valores das caixas de texto em um formulário.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="d8bbd-181">Muitos exemplos, você verá mostram como processar a página de forma diferente dependendo do valor de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="d8bbd-182">Um exemplo de código simples</span><span class="sxs-lookup"><span data-stu-id="d8bbd-182">A Simple Code Example</span></span>

<span data-ttu-id="d8bbd-183">Este procedimento mostra como criar uma página que ilustra as técnicas de programação básicas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="d8bbd-184">No exemplo, você cria uma página que permite aos usuários inserir dois números, em seguida, adiciona-os e exibe o resultado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="d8bbd-185">Em seu editor, crie um novo arquivo e nomeie- *AddNumbers.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="d8bbd-186">Copie o seguinte código e marcação para a página, substituindo qualquer coisa já na página.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="d8bbd-187">Aqui estão algumas coisas a observar:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="d8bbd-188">O `@` caractere inicia o primeiro bloco de código na página, e precede o `totalMessage` variável inserido na parte inferior.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="d8bbd-189">O bloco na parte superior da página é colocado entre `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="d8bbd-190">As variáveis `total`, `num1`, `num2`, e `totalMessage` armazenar vários números e uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="d8bbd-191">O valor de cadeia de caracteres literal atribuído para o `totalMessage` variável está entre aspas duplas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="d8bbd-192">Porque o código do Visual Basic não diferencia maiusculas de minúsculas, quando o `totalMessage` variável é usada na parte inferior da página, seu nome somente precisa corresponder a ortografia de declaração da variável na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="d8bbd-193">O uso de maiusculas e minúsculas não importa.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="d8bbd-194">A expressão `num1.AsInt()`  +  `num2.AsInt()` mostra como trabalhar com objetos e métodos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="d8bbd-195">O `AsInt` método em cada variável converte a cadeia de caracteres inserida por um usuário em um número inteiro (um inteiro) que pode ser adicionado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="d8bbd-196">O `<form>` marca inclui um `method="post"` atributo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="d8bbd-197">Isso especifica que quando o usuário clica **adicionar**, a página será enviada ao servidor usando o método HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="d8bbd-198">Quando a página é enviada, o código `If IsPost` é avaliada como true e a condicional código é executado, exibindo o resultado da adição de números.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="d8bbd-199">Salve a página e executá-lo em um navegador.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="d8bbd-200">(Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.) Insira dois números inteiros e, em seguida, clique no **adicionar** botão.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="d8bbd-202">Sintaxe e a linguagem Visual Basic</span><span class="sxs-lookup"><span data-stu-id="d8bbd-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="d8bbd-203">Anteriormente, você viu um exemplo básico de como criar uma página da web do ASP.NET e como você pode adicionar código de servidor a marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="d8bbd-204">Aqui você aprenderá as Noções básicas de como usar o Visual Basic para escrever código de servidor ASP.NET usando a sintaxe do Razor &#8212; ou seja, as regras linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="d8bbd-205">Se você tiver experiência com programação (especialmente se você já usou o C, C++, c#, Visual Basic ou JavaScript), grande parte o que você leia aqui será familiar.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="d8bbd-206">Você provavelmente precisará familiarizar-se apenas com como o código do WebMatrix é adicionado a marcação na *. vbhtml* arquivos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="d8bbd-207">A combinação de texto, marcação e código em blocos de código</span><span class="sxs-lookup"><span data-stu-id="d8bbd-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="d8bbd-208">Em blocos de código do servidor, você geralmente deseja texto e marcação para a página de saída.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="d8bbd-209">Se um bloco de código de servidor contiver o texto que não é código e que em vez disso, deve ser renderizado como está, o ASP.NET precisa ser capaz de distinguir que o texto do código.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="d8bbd-210">Há várias maneiras de fazer isso.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-210">There are several ways to do this.</span></span>

- <span data-ttu-id="d8bbd-211">Colocar o texto em um elemento de bloco HTML, como `<p></p>` ou `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="d8bbd-212">O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código do servidor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="d8bbd-213">Quando o ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), tudo o que renderiza o elemento e seu conteúdo como para o navegador (e resolve as expressões de código do servidor).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="d8bbd-214">Use o `@:` operador ou o `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="d8bbd-215">O `@:` gera uma única linha de conteúdo que contém o texto sem formatação ou marcas HTML sem correspondência; o `<text>` elemento abrange várias linhas de saída.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="d8bbd-216">Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="d8bbd-217">O exemplo a seguir repete o exemplo anterior, mas usa um único par de `<text>` marcas para delimitar o texto a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="d8bbd-218">No exemplo a seguir, o `<text>` e `</text>` marcas Coloque três linhas, todas com parte do texto dependente e marcas HTML não correspondentes (`<br />`), junto com código de servidor e as marcas HTML correspondentes.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="d8bbd-219">Novamente, você também pode preceder cada linha individualmente com a `@:` operador; ambas funcionam de forma.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="d8bbd-220">Quando você gerar o texto conforme mostrado nesta seção &#8212; usando um elemento HTML, o `@:` operador, ou o `<text>` elemento &#8212; ASP.NET não codificar com HTML a saída.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="d8bbd-221">(Conforme observado anteriormente, o ASP.NET codificar a saída de expressões de código do servidor e os blocos de código do servidor que são precedidos por `@`, exceto nos casos especiais observados nesta seção.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="d8bbd-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="d8bbd-222">Whitespace</span></span>

<span data-ttu-id="d8bbd-223">Os espaços extras em uma instrução (e fora de uma cadeia de caracteres literal) não afetam a instrução:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="d8bbd-224">Dividir instruções longas em várias linhas</span><span class="sxs-lookup"><span data-stu-id="d8bbd-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="d8bbd-225">É possível dividir uma instrução de código longo em várias linhas usando o caractere de sublinhado `_` (que no Visual Basic é chamado de *caractere de continuação*) após cada linha de código.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="d8bbd-226">Para interromper uma instrução para a próxima linha, no final da linha, adicione um espaço e, em seguida, o caractere de continuação.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="d8bbd-227">A instrução continue na próxima linha.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-227">Continue the statement on the next line.</span></span> <span data-ttu-id="d8bbd-228">Você pode encapsular declarações em tantas linhas conforme necessário melhorar a legibilidade.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="d8bbd-229">As instruções a seguir são os mesmos:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="d8bbd-230">No entanto, você não pode inserir uma linha no meio de um literal de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="d8bbd-231">O exemplo a seguir não funciona:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="d8bbd-232">Para combinar uma cadeia de caracteres longa é quebrada para várias linhas, como o código acima, você precisaria usar o *operador de concatenação* (`&`), que você verá neste artigo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="d8bbd-233">Comentários de código</span><span class="sxs-lookup"><span data-stu-id="d8bbd-233">Code comments</span></span>

<span data-ttu-id="d8bbd-234">Comentários permitem que você deixar observações para você ou terceiros.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="d8bbd-235">Comentários de sintaxe do Razor são prefixados com `@*` e terminar com `*@`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="d8bbd-236">Dentro de blocos de código, você pode usar os comentários de sintaxe do Razor, ou você pode usar o caractere de comentário Visual Basic comum, que é uma aspa simples (`'`) prefixado para cada linha.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="d8bbd-237">Variáveis</span><span class="sxs-lookup"><span data-stu-id="d8bbd-237">Variables</span></span>

<span data-ttu-id="d8bbd-238">Uma variável é um objeto nomeado que você pode usar para armazenar dados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="d8bbd-239">Você pode nomear variáveis, mas o nome deve começar com um caractere alfabético e não pode conter espaço em branco ou caracteres reservados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="d8bbd-240">No Visual Basic, como você viu anteriormente, não importa o caso das letras do nome de uma variável.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="d8bbd-241">Variáveis e tipos de dados</span><span class="sxs-lookup"><span data-stu-id="d8bbd-241">Variables and data types</span></span>

<span data-ttu-id="d8bbd-242">Uma variável pode ter um tipo de dados específico, que indica o tipo de dados é armazenado na variável.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="d8bbd-243">Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 12/4/2012 ou de março de 2009 ).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="d8bbd-244">E há muitos outros tipos de dados que você pode usar.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="d8bbd-245">No entanto, você não precisa especificar um tipo para uma variável.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="d8bbd-246">Na maioria dos casos ASP.NET pode descobrir o tipo com base em como os dados na variável está sendo usados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="d8bbd-247">(Ocasionalmente, você deve especificar um tipo; você verá exemplos em que isso é verdadeiro).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="d8bbd-248">Para declarar uma variável sem especificar um tipo, use `Dim` além do nome de variável (por exemplo, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="d8bbd-249">Para declarar uma variável com um tipo, use `Dim` mais o nome da variável, seguido por `As` e, em seguida, o nome do tipo (por exemplo, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="d8bbd-250">O exemplo a seguir mostra algumas expressões embutidas que usam as variáveis em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="d8bbd-251">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="d8bbd-253">Convertendo e tipos de dados de teste</span><span class="sxs-lookup"><span data-stu-id="d8bbd-253">Converting and testing data types</span></span>

<span data-ttu-id="d8bbd-254">Embora o ASP.NET geralmente pode determinar automaticamente um tipo de dados, às vezes, ele não é possível.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="d8bbd-255">Portanto, você pode precisar ajudar ASP.NET executando uma conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="d8bbd-256">Mesmo se você não precisa converter os tipos, às vezes é útil testar para ver qual tipo de dados você pode estar trabalhando com.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="d8bbd-257">O caso mais comum é que você precisa converter uma cadeia de caracteres para outro tipo, como em um inteiro ou uma data.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="d8bbd-258">O exemplo a seguir mostra um caso típico onde você deve converter uma cadeia de caracteres em um número.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="d8bbd-259">Como uma regra de entrada do usuário são enviados a você como cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="d8bbd-260">Mesmo se você tiver solicitado o usuário insira um número, e mesmo se eles inseriu um dígito, quando a entrada do usuário é enviada e lê-lo no código, os dados estão no formato de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="d8bbd-261">Portanto, você deve converter a cadeia de caracteres em um número.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="d8bbd-262">No exemplo, se você tentar executar cálculos nos valores sem convertê-los, o erro a seguir resulta, porque o ASP.NET não é possível adicionar duas cadeias de caracteres:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="d8bbd-263">Para converter os valores inteiros, você deve chamar o `AsInt` método.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="d8bbd-264">Se a conversão for bem-sucedida, você pode adicionar os números.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="d8bbd-265">A tabela a seguir lista alguns métodos comuns de conversão e teste para variáveis.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-265">The following table lists some common conversion and test methods for variables.</span></span>


:::row:::
    :::column:::
        <span data-ttu-id="d8bbd-266"><strong>Método</strong></span><span class="sxs-lookup"><span data-stu-id="d8bbd-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-267"><strong>Descrição</strong></span><span class="sxs-lookup"><span data-stu-id="d8bbd-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-268"><strong>Exemplo</strong></span><span class="sxs-lookup"><span data-stu-id="d8bbd-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-269">Converte uma cadeia de caracteres que representa um número inteiro (como &quot;593&quot;) em um inteiro.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-270">Converte uma cadeia de caracteres like &quot;verdadeira&quot; ou &quot;falso&quot; para um tipo booleano.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-271">Converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; um número de ponto flutuante.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-272">Converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; em um número decimal.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="d8bbd-273">(No ASP.NET, um número decimal é mais preciso do que um número de ponto flutuante.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span> :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-274">Converte uma cadeia de caracteres que representa um valor de data e hora para o ASP.NET `DateTime` tipo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-275">Converte qualquer tipo de dados em uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a><span data-ttu-id="d8bbd-276">Operadores</span><span class="sxs-lookup"><span data-stu-id="d8bbd-276">Operators</span></span>

<span data-ttu-id="d8bbd-277">Um operador é uma palavra-chave ou um caractere que informa ao ASP.NET que tipo de comando para executar em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="d8bbd-278">Visual Basic oferece suporte a muitos operadores, mas você só precisa reconhecer algumas para começar a desenvolver páginas da web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="d8bbd-279">A tabela a seguir resume os operadores mais comuns.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-279">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <span data-ttu-id="d8bbd-280"><strong>Operador</strong></span><span class="sxs-lookup"><span data-stu-id="d8bbd-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-281"><strong>Descrição</strong></span><span class="sxs-lookup"><span data-stu-id="d8bbd-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-282"><strong>Exemplos</strong></span><span class="sxs-lookup"><span data-stu-id="d8bbd-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-283">Operadores de cálculo usados em expressões numéricas.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-284">Atribuição e igualdade.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-284">Assignment and equality.</span></span> <span data-ttu-id="d8bbd-285">Dependendo do contexto, atribui o valor no lado direito de uma instrução para o objeto no lado esquerdo ou verifica os valores para igualdade.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-286">Desigualdade.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-286">Inequality.</span></span> <span data-ttu-id="d8bbd-287">Retorna `True` se os valores não forem iguais.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-288">Menor que, maior, menor ou igual e maior ou igual.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-289">Concatenação, que é usada para unir cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-290">Os operadores de incremento e decremento, que adicionar e subtrair 1 (respectivamente) a partir de uma variável.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-291">Ponto.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-291">Dot.</span></span> <span data-ttu-id="d8bbd-292">Usado para distinguir os objetos e suas propriedades e métodos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-293">Parênteses.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-293">Parentheses.</span></span> <span data-ttu-id="d8bbd-294">Usado para agrupar expressões, para passar parâmetros para métodos e para acessar membros de coleções e matrizes.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-295">Não.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-295">Not.</span></span> <span data-ttu-id="d8bbd-296">Inverte um valor true para false e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="d8bbd-297">Normalmente usado como uma forma abreviada para testar `False` (ou seja, para não `True`).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d8bbd-298">AND lógico e, que são usados para vincular condições ou juntos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="d8bbd-299">Trabalhando com arquivos e caminhos de pasta no código</span><span class="sxs-lookup"><span data-stu-id="d8bbd-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="d8bbd-300">Geralmente, você trabalhará com caminhos de arquivo e pasta em seu código.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="d8bbd-301">Aqui está um exemplo da estrutura de pasta física para um site, como pode aparecer em seu computador de desenvolvimento:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="d8bbd-302">Aqui estão alguns detalhes essenciais sobre URLs e caminhos:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="d8bbd-303">Uma URL começa com qualquer um de um nome de domínio (`http://www.example.com`) ou um nome de servidor (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="d8bbd-304">Uma URL corresponde a um caminho físico em um computador host.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="d8bbd-305">Por exemplo, `http://myserver` podem corresponder à pasta *C:\websites\mywebsite* no servidor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="d8bbd-306">Um caminho virtual é uma abreviação para representar caminhos no código sem ter que especificar o caminho completo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="d8bbd-307">Ele inclui a parte de uma URL que segue o nome de domínio ou servidor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="d8bbd-308">Quando você usa caminhos virtuais, você pode mover seu código para um domínio diferente ou um servidor sem ter que atualizar os caminhos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="d8bbd-309">Aqui está um exemplo para ajudá-lo a entender as diferenças:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="d8bbd-310">URL completa</span><span class="sxs-lookup"><span data-stu-id="d8bbd-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="d8bbd-311">Nome do servidor</span><span class="sxs-lookup"><span data-stu-id="d8bbd-311">Server name</span></span> | <span data-ttu-id="d8bbd-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="d8bbd-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="d8bbd-313">Caminho virtual</span><span class="sxs-lookup"><span data-stu-id="d8bbd-313">Virtual path</span></span> | <span data-ttu-id="d8bbd-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d8bbd-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="d8bbd-315">Caminho físico</span><span class="sxs-lookup"><span data-stu-id="d8bbd-315">Physical path</span></span> | <span data-ttu-id="d8bbd-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d8bbd-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="d8bbd-317">É a raiz virtual /, assim como a raiz da unidade c: unidade é \.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="d8bbd-318">(Os caminhos de pasta virtual sempre usam barras "/"). O caminho virtual de uma pasta não precisa ter o mesmo nome que a pasta física; ele pode ser um alias.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="d8bbd-319">(Em servidores de produção, o caminho virtual raramente corresponde a um caminho físico exato.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="d8bbd-320">Quando você trabalha com arquivos e pastas no código, às vezes, você precisará referenciar o caminho físico e, às vezes, um caminho virtual, dependendo de quais objetos você está trabalhando com.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="d8bbd-321">ASP.NET fornece essas ferramentas para trabalhar com caminhos de arquivo e pasta no código: o `Server.MapPath` método e o `~` operador e `Href` método.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="d8bbd-322">Conversão de caminhos virtuais físicos: o método Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="d8bbd-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="d8bbd-323">O `Server.MapPath` método converte um caminho virtual (como */default.cshtml*) em um caminho físico absoluto (como *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="d8bbd-324">Você usar esse método sempre que precisar de um caminho físico completo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="d8bbd-325">Um exemplo típico é quando você estiver lendo ou gravando um arquivo de texto ou arquivo de imagem no servidor web.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="d8bbd-326">Normalmente você não souber o caminho físico absoluto do seu site no servidor de hospedagem do site, para que esse método possa converter o caminho que você sabe, o caminho virtual — para o caminho correspondente no servidor para você.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="d8bbd-327">Passe o caminho virtual para um arquivo ou pasta para o método e retorna o caminho físico:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="d8bbd-328">Fazendo referência a raiz virtual: o ~ operador e o método de Href</span><span class="sxs-lookup"><span data-stu-id="d8bbd-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="d8bbd-329">Em um *. cshtml* ou *. vbhtml* arquivo, você pode referenciar o caminho virtual raiz usando o `~` operador.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="d8bbd-330">Isso é muito útil porque você pode mover páginas em um site, e todos os links para outras páginas contêm não será interrompidos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="d8bbd-331">Também é útil caso você nunca move seu site para um local diferente.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="d8bbd-332">Estes são alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="d8bbd-333">Se o site `http://myserver/myapp`, aqui está como ASP.NET irá tratar esses caminhos quando a página é executada:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="d8bbd-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="d8bbd-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="d8bbd-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="d8bbd-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="d8bbd-336">(Na verdade, você não verá esses caminhos como os valores da variável, mas ASP.NET tratará os caminhos como se isso é o que eles foram).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="d8bbd-337">Você pode usar o `~` operador no código do servidor (como acima) e na marcação, como este:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="d8bbd-338">Na marcação, você deve usar o `~` operador para criar caminhos a recursos como arquivos de imagem, outras páginas da web e arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="d8bbd-339">Quando a página é executada, o ASP.NET procura por meio da página (código e marcação) e resolve todos os `~` referências para o caminho apropriado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="d8bbd-340">Loops e lógica condicional</span><span class="sxs-lookup"><span data-stu-id="d8bbd-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="d8bbd-341">Código de servidor do ASP.NET permite que você execute tarefas com base em condições e escreva o código que repete instruções que um número específico de vezes ou seja, código que executa um loop).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="d8bbd-342">Condições de testes</span><span class="sxs-lookup"><span data-stu-id="d8bbd-342">Testing conditions</span></span>

<span data-ttu-id="d8bbd-343">Para testar uma condição simple é usar o `If...Then` instrução, que retorna `True` ou `False` com base em um teste que você especificar:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="d8bbd-344">O `If` palavra-chave inicia um bloco.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="d8bbd-345">O teste real (condição) segue o `If` palavra-chave e retorna true ou false.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="d8bbd-346">O `If` instrução termina com `Then`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="d8bbd-347">As instruções que serão executado se o teste seja verdadeiro são delimitadas por `If` e `End If`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="d8bbd-348">Uma `If` instrução pode incluir um `Else` bloco que especifica as instruções a serem executadas se a condição for falsa:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="d8bbd-349">Se um `If` instrução se inicia um bloco de código, você não precisa usar o normal `Code...End Code` instruções para incluir os blocos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="d8bbd-350">Você pode adicionar apenas `@` para o bloco, e ela funcionará.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="d8bbd-351">Essa abordagem funciona com `If` , bem como outro palavras-chave que são seguidas por blocos de código, incluindo de programação do Visual Basic `For`, `For Each`, `Do While`, etc.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="d8bbd-352">Você pode adicionar várias condições usando um ou mais `ElseIf` blocos:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="d8bbd-353">Neste exemplo, se a primeira condição na `If` bloco não for true, o `ElseIf` condição está marcada.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="d8bbd-354">Se essa condição for atendida, as instruções no `ElseIf` bloco são executados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="d8bbd-355">Se nenhuma das condições forem atendidas, as instruções no `Else` bloco são executados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="d8bbd-356">Você pode adicionar qualquer número de `ElseIf` bloqueia e, em seguida, fechar com uma `Else` bloquear como o &quot;todo o resto&quot; condição.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="d8bbd-357">Para testar um grande número de condições, use um `Select Case` bloco:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="d8bbd-358">O valor a ser testado está entre parênteses (no exemplo, a variável de dia da semana).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="d8bbd-359">Cada teste individual usa um `Case` instrução que um valor de lista.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="d8bbd-360">Se o valor de uma `Case` instrução corresponde ao valor de teste, o código em que `Case` bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="d8bbd-361">O resultado dos dois últimos blocos condicionais exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="d8bbd-363">Código de loop</span><span class="sxs-lookup"><span data-stu-id="d8bbd-363">Looping code</span></span>

<span data-ttu-id="d8bbd-364">Geralmente, você precisará executar as mesmas instruções repetidas vezes.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="d8bbd-365">Você pode fazer isso por meio de loops.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-365">You do this by looping.</span></span> <span data-ttu-id="d8bbd-366">Por exemplo, você sempre execute as mesmas instruções para cada item em uma coleção de dados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="d8bbd-367">Se você souber exatamente quantas vezes você deseja executar um loop, você pode usar um `For` loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="d8bbd-368">Esse tipo de loop é especialmente útil para de contagem ou contagem regressiva:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="d8bbd-369">O loop começa com o `For` palavra-chave, seguido de três elementos:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="d8bbd-370">Imediatamente após o `For` a instrução, você declara uma variável de contador (você não precisa usar `Dim`) e, em seguida, indique o intervalo, como em `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="d8bbd-371">Isso significa que a variável `i` começará a contagem em 10 e continuar até que ele atinja 20 (inclusivo).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="d8bbd-372">Entre os `For` e `Next` instruções é o conteúdo do bloco.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="d8bbd-373">Isso pode conter um ou mais instruções de código que executarem com cada loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="d8bbd-374">O `Next i` instrução termina o loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="d8bbd-375">Ele incrementa o contador e começa a próxima iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="d8bbd-376">A linha de código entre o `For` e `Next` linhas contém o código que é executado para cada iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="d8bbd-377">A marcação cria um novo parágrafo (`<p>` elemento) cada vez e adiciona uma linha na saída, exibindo o valor de i (o contador).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="d8bbd-378">Quando você executa essa página, o exemplo cria 11 linhas exibindo a saída, com o texto em cada linha que indica o número de item.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="d8bbd-380">Se você estiver trabalhando com uma coleção ou matriz, você geralmente usa um `For Each` loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="d8bbd-381">Uma coleção é um grupo de objetos semelhantes e o `For Each` loop permite que você executar uma tarefa em cada item na coleção.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="d8bbd-382">Esse tipo de loop é conveniente para coleções, pois ao contrário de um `For` loop, você não precisa incrementar o contador ou definir um limite.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="d8bbd-383">Em vez disso, o `For Each` loop código simplesmente ocorrerá por meio da coleção, até que ela seja concluída.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="d8bbd-384">Este exemplo retorna os itens a `Request.ServerVariables` coleção (que contém informações sobre seu servidor web).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="d8bbd-385">Ele usa um `For Each` loop para exibir o nome de cada item, criando um novo `<li>` elemento em uma lista com marcadores de HTML.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="d8bbd-386">O `For Each` palavra-chave é seguido por uma variável que representa um único item na coleção (no exemplo, `myItem`), seguido pelo `In` palavra-chave, seguido pela coleção que você deseja fazer o loop.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="d8bbd-387">No corpo do `For Each` loop, você pode acessar o item atual usando a variável declarada anteriormente por você.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="d8bbd-389">Para criar um loop de propósito mais geral, use o `Do While` instrução:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="d8bbd-390">Esse loop começa com o `Do While` palavra-chave, seguido por uma condição, seguido pelo bloco repetir.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="d8bbd-391">Loops normalmente incrementam (Adicionar ao) ou decrementar (subtrair de) uma variável ou um objeto usado para contagem.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="d8bbd-392">No exemplo, o `+=` operador adiciona 1 ao valor de uma variável de cada vez que o loop é executado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="d8bbd-393">(Para diminuir a uma variável em um loop de contagem regressiva, você usaria o operador de decremento `-=`.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="d8bbd-394">Objetos e coleções</span><span class="sxs-lookup"><span data-stu-id="d8bbd-394">Objects and Collections</span></span>

<span data-ttu-id="d8bbd-395">Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da web.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="d8bbd-396">Esta seção discute alguns objetos importantes que você trabalhará com frequência em seu código.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="d8bbd-397">Objetos de página</span><span class="sxs-lookup"><span data-stu-id="d8bbd-397">Page objects</span></span>

<span data-ttu-id="d8bbd-398">O objeto mais básico no ASP.NET é a página.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="d8bbd-399">Você pode acessar as propriedades do objeto page diretamente sem qualquer objeto qualificado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="d8bbd-400">O código a seguir obtém o caminho do arquivo da página, usando o `Request` objeto da página:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="d8bbd-401">Você pode usar propriedades do `Page` objeto do qual obter muitas informações, tais como:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="d8bbd-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-402">`Request`.</span></span> <span data-ttu-id="d8bbd-403">Como você já viu, esta é uma coleção de informações sobre a solicitação atual, incluindo o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="d8bbd-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-404">`Response`.</span></span> <span data-ttu-id="d8bbd-405">Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código de servidor concluiu a execução.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="d8bbd-406">Por exemplo, você pode usar essa propriedade para gravar informações na resposta.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="d8bbd-407">Objetos de coleção (matrizes e dicionários)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="d8bbd-408">Uma coleção é um grupo de objetos do mesmo tipo, como uma coleção de `Customer` objetos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="d8bbd-409">ASP.NET contém várias coleções internas, como o `Request.Files` coleção.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="d8bbd-410">Geralmente, você trabalhará com dados em coleções.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-410">You'll often work with data in collections.</span></span> <span data-ttu-id="d8bbd-411">Dois tipos de coleção comuns são as *array* e o *dicionário*.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="d8bbd-412">Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não quiser criar uma variável separada para manter cada item:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="d8bbd-413">Com matrizes, você declara um tipo de dados específico, como `String`, `Integer`, ou `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="d8bbd-414">Para indicar que a variável pode conter uma matriz, adicione parênteses para o nome da variável na declaração (como `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="d8bbd-415">Você pode acessar itens em uma matriz usando sua posição (índice) ou usando o `For Each` instrução.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="d8bbd-416">Índices de matriz são baseados em zero &#8212; ou seja, o primeiro item está na posição 0, o segundo item estiver na posição 1 e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="d8bbd-417">Você pode determinar o número de itens em uma matriz obtendo sua `Length` propriedade.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="d8bbd-418">Para obter a posição de um item específico na matriz (ou seja, para pesquisar a matriz), use o `Array.IndexOf` método.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="d8bbd-419">Você também pode fazer coisas como reverter o conteúdo de uma matriz (o `Array.Reverse` método) ou classificar o conteúdo (o `Array.Sort` método).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="d8bbd-420">A saída do código de matriz de cadeia de caracteres exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="d8bbd-422">Um dicionário é uma coleção de pares chave/valor, em que você fornece a chave (ou nome) para definir ou recuperar o valor correspondente:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="d8bbd-423">Para criar um dicionário, você deve usar o `New` palavra-chave para indicar que você está criando um novo `Dictionary` objeto.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="d8bbd-424">Você pode atribuir um dicionário para uma variável usando o `Dim` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="d8bbd-425">Você indica os tipos de dados dos itens no dicionário usando parênteses ( `( )` ).</span><span class="sxs-lookup"><span data-stu-id="d8bbd-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="d8bbd-426">No final da declaração, você deve adicionar outro par de parênteses, porque isso é, na verdade, um método que cria um novo dicionário.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="d8bbd-427">Para adicionar itens ao dicionário, você pode chamar o `Add` método da variável de dicionário (`myScores` nesse caso) e, em seguida, especifique uma chave e um valor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="d8bbd-428">Como alternativa, você pode usar parênteses para indicar a chave e fazer uma simples atribuição, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="d8bbd-429">Para obter um valor do dicionário, você pode especificar a chave entre parênteses:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="d8bbd-430">Chamar métodos com parâmetros</span><span class="sxs-lookup"><span data-stu-id="d8bbd-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="d8bbd-431">Como você viu neste artigo, os objetos que você programou com têm métodos.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="d8bbd-432">Por exemplo, uma `Database` objeto pode ter um `Database.Connect` método.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="d8bbd-433">Muitos métodos também tem um ou mais parâmetros.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="d8bbd-434">Um *parâmetro* é um valor que você passa para um método para habilitar o método concluir a tarefa.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="d8bbd-435">Por exemplo, examinar uma declaração para o `Request.MapPath` método, que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="d8bbd-436">Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="d8bbd-437">Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="d8bbd-438">(Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que ele aceitará.) Quando você chama esse método, você deve fornecer valores para todos os três parâmetros.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="d8bbd-439">Quando você estiver usando o Visual Basic com a sintaxe do Razor, você tem duas opções para passar parâmetros para um método: *parâmetros posicionais* ou *parâmetros nomeados*.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="d8bbd-440">Para chamar um método usando parâmetros posicionais, você pode passar os parâmetros em uma ordem estrita que é especificada na declaração de método.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="d8bbd-441">(Você normalmente saberia nesta ordem, lendo a documentação do método.) Você deve seguir a ordem, e você não pode ignorar qualquer um dos parâmetros &#8212; se necessário, você passa uma cadeia de caracteres vazia (`""`) ou para um parâmetro posicional que você não tiver um valor para null.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="d8bbd-442">O exemplo a seguir pressupõe que você tem uma pasta chamada *scripts* em seu site.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="d8bbd-443">O código chama o `Request.MapPath` e passa valores para os três parâmetros na ordem correta.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="d8bbd-444">Ele, em seguida, exibe o caminho de mapeada resultante.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="d8bbd-445">Quando há muitos parâmetros para um método, você pode manter seu código mais limpo e legível usando parâmetros nomeados.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="d8bbd-446">Para chamar um método usando parâmetros nomeados, especifique o nome do parâmetro seguido por `:=` e, em seguida, forneça o valor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="d8bbd-447">Uma vantagem dos parâmetros nomeados é que você pode adicioná-los em qualquer ordem desejada.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="d8bbd-448">(Uma desvantagem é que a chamada de método não é mais compacta.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="d8bbd-449">O exemplo a seguir chama o método acima, mas usa parâmetros para fornecer os valores nomeados:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="d8bbd-450">Como você pode ver, os parâmetros são passados em uma ordem diferente.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="d8bbd-451">No entanto, se você executar o exemplo anterior e este exemplo, eles retornar o mesmo valor.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="d8bbd-452">Manipulando erros</span><span class="sxs-lookup"><span data-stu-id="d8bbd-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="d8bbd-453">Instruções Try-Catch</span><span class="sxs-lookup"><span data-stu-id="d8bbd-453">Try-Catch statements</span></span>

<span data-ttu-id="d8bbd-454">Geralmente, você terá as instruções em seu código que pode falhar por razões de fora do seu controle.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="d8bbd-455">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d8bbd-455">For example:</span></span>

- <span data-ttu-id="d8bbd-456">Se seu código tenta abrir, criar, ler ou gravar um arquivo, todos os tipos de erros podem ocorrer.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="d8bbd-457">O arquivo que você deseja que pode não existir, ele pode ter sido bloqueado, o código pode não ter permissões e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="d8bbd-458">Da mesma forma, se seu código tenta atualizar registros em um banco de dados, pode haver problemas de permissões, a conexão ao banco de dados pode ser descartado, os dados para salvar podem estar inválido e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="d8bbd-459">Em termos de programação, essas situações são chamadas *exceções*.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="d8bbd-460">Se seu código encontrar uma exceção, ele gera (gera) uma mensagem de erro que é, na melhor das hipóteses, irritantes aos usuários.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="d8bbd-462">Em situações em que seu código poderá encontrar exceções e para evitar mensagens de erro desse tipo, você pode usar `Try/Catch` instruções.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="d8bbd-463">No `Try` instrução, que você executa o código que você está verificando.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="d8bbd-464">Em uma ou mais `Catch` instruções, você pode procurar específicos de erros (tipos específicos de exceções) que possam ter ocorrido.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="d8bbd-465">Você pode incluir tantos `Catch` instruções que você precisam procurar por erros que você está prevendo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="d8bbd-466">É recomendável que você evite usar o `Response.Redirect` método no `Try/Catch` instruções, porque isso pode causar uma exceção em sua página.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="d8bbd-467">O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite que o usuário abrir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="d8bbd-468">O exemplo deliberadamente usa um nome de arquivo incorreto para que ela fará com que uma exceção.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="d8bbd-469">O código inclui `Catch` instruções para duas exceções possíveis: `FileNotFoundException`, que ocorre se o nome do arquivo é ruim, e `DirectoryNotFoundException`, que ocorre se o ASP.NET ainda não é possível localizar a pasta.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="d8bbd-470">(Você pode remover o comentário uma instrução no exemplo a fim de ver como ele é executado quando tudo está funcionando corretamente.)</span><span class="sxs-lookup"><span data-stu-id="d8bbd-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="d8bbd-471">Se seu código não trata a exceção, você verá uma página de erro, como a captura de tela anterior.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="d8bbd-472">No entanto, o `Try/Catch` seção ajuda a impedir que o usuário ver esses tipos de erros.</span><span class="sxs-lookup"><span data-stu-id="d8bbd-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="d8bbd-473">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d8bbd-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="d8bbd-474">Documentação de referência</span><span class="sxs-lookup"><span data-stu-id="d8bbd-474">Reference Documentation</span></span>

- [<span data-ttu-id="d8bbd-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d8bbd-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="d8bbd-476">Linguagem Visual Basic</span><span class="sxs-lookup"><span data-stu-id="d8bbd-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
