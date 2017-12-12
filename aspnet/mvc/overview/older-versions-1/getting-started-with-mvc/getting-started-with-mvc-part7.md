---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "Adicionando validação para o modelo | Microsoft Docs"
author: shanselman
description: "Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 25c939bc8121589f91914e553d56e8f0975115b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="8f7c2-104">Adicionando validação para o modelo</span><span class="sxs-lookup"><span data-stu-id="8f7c2-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="8f7c2-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="8f7c2-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="8f7c2-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="8f7c2-107">Você criará um aplicativo web simples que leituras e gravações de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="8f7c2-108">Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="8f7c2-109">Nesta seção, vamos implementar o suporte necessário para habilitar a validação de entrada em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="8f7c2-110">Irá garantir que nosso conteúdo de banco de dados sempre está correto e fornecer mensagens de erro úteis para os usuários finais quando eles tentem e inserem dados de filme que não é válidos.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="8f7c2-111">Vamos começar adicionando um pouco lógica de validação para a classe do filme.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="8f7c2-112">Clique com o botão direito na pasta do modelo e selecione Adicionar classe.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="8f7c2-113">Nome de sua classe filme.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-113">Name your class Movie.</span></span>

<span data-ttu-id="8f7c2-114">Quando criamos o modelo de entidade de filme anteriormente, o IDE criado uma classe do filme.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="8f7c2-115">Na verdade, parte da classe de filme pode estar em um arquivo e parte em outro.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="8f7c2-116">Isso é chamado de uma classe parcial.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-116">This is called a Partial Class.</span></span> <span data-ttu-id="8f7c2-117">Vamos estender a classe de filme de outro arquivo.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="8f7c2-118">Vamos criar uma classe parcial de filme que aponta para uma classe"amigos" com alguns atributos que fornecerá as dicas de validação para o sistema.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="8f7c2-119">Vamos marcar o título e o preço conforme necessário e também insistir que o preço seja em um determinado intervalo.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="8f7c2-120">Clique com botão direito na pasta modelos e selecione Adicionar classe.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="8f7c2-121">Nome de sua classe filme e clique no botão Okey.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="8f7c2-122">É aqui que nosso parcial filme classe parece.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="8f7c2-123">Execute novamente o seu aplicativo e tentar inserir um filme com um preço acima de 100.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="8f7c2-124">Após ter enviado o formulário, você receberá um erro.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="8f7c2-125">O erro é detectado no lado do servidor e ocorre depois que o formulário é enviado.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="8f7c2-126">Observe como os auxiliares HTML internos de ASP.NET MVC foram inteligentes exibir a mensagem de erro e manter os valores para que possamos dentro de elementos de texto:</span><span class="sxs-lookup"><span data-stu-id="8f7c2-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="8f7c2-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f7c2-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="8f7c2-128">Isso funciona muito bem, mas seria interessante se poderia informamos ao usuário no lado do cliente, imediatamente, antes do servidor é envolvido.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="8f7c2-129">Vamos habilitar alguma validação do lado do cliente com JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="8f7c2-130">Adicionando validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="8f7c2-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="8f7c2-131">Como nossa classe filme já tem alguns atributos de validação, vamos apenas precisará adicionar alguns arquivos JavaScript para nosso modelo de exibição de Create.aspx e adicionar uma linha de código para habilitar a validação do lado do cliente ocorra.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="8f7c2-132">No VWD acesse nossa pasta de modos de exibição/filme e abra Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="8f7c2-133">Abra a pasta de Scripts no Gerenciador de soluções e arraste os seguintes três scripts para dentro do &lt;head&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="8f7c2-134">MicrosoftAjax</span><span class="sxs-lookup"><span data-stu-id="8f7c2-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="8f7c2-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="8f7c2-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="8f7c2-136">Deseja que esses arquivos de script são exibidos nessa ordem.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="8f7c2-137">Além disso, adicione esta linha única acima de Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="8f7c2-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="8f7c2-138">Aqui está o código mostrado no IDE.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="8f7c2-139">[![Filmes - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8f7c2-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="8f7c2-140">Executar seu aplicativo, visite /Movies/Create novamente e clique em criar sem inserir os dados.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="8f7c2-141">As mensagens de erro aparecem imediatamente sem a página flash que associadas ao enviar dados de volta para o servidor.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="8f7c2-142">Isso ocorre porque o ASP.NET MVC agora está validando a entrada no cliente (usando JavaScript) e no servidor.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="8f7c2-143">[![Criar - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8f7c2-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="8f7c2-144">Isso está procurando válido!</span><span class="sxs-lookup"><span data-stu-id="8f7c2-144">This is looking good!</span></span> <span data-ttu-id="8f7c2-145">Agora vamos adicionar uma coluna adicional para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8f7c2-145">Let's now add one additional column to the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8f7c2-146">[Anterior](getting-started-with-mvc-part6.md)
[Próximo](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="8f7c2-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
