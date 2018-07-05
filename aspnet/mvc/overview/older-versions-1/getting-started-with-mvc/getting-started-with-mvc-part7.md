---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Adicionar validação ao modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: f8172b63fcaa7599f5dd733271d9ea7570e3bcb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802780"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="2e624-104">Adicionar validação ao modelo</span><span class="sxs-lookup"><span data-stu-id="2e624-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="2e624-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2e624-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="2e624-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2e624-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2e624-107">Você criará um aplicativo web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2e624-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2e624-108">Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="2e624-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="2e624-109">Nesta seção, vamos implementar o suporte necessário para habilitar a validação de entrada dentro do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2e624-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="2e624-110">Nós garantiremos que nosso conteúdo de banco de dados sempre está correto e fornecer mensagens de erro úteis para os usuários finais quando eles tente e insere dados de filme que não não válidos.</span><span class="sxs-lookup"><span data-stu-id="2e624-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="2e624-111">Vamos começar adicionando um pouco lógica de validação para a classe de filme.</span><span class="sxs-lookup"><span data-stu-id="2e624-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="2e624-112">Clique com o botão direito na pasta do modelo e selecione Add Class.</span><span class="sxs-lookup"><span data-stu-id="2e624-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="2e624-113">Nome de sua classe de filme.</span><span class="sxs-lookup"><span data-stu-id="2e624-113">Name your class Movie.</span></span>

<span data-ttu-id="2e624-114">Quando criamos o modelo de entidade do filme anteriormente, o IDE criado uma classe de filme.</span><span class="sxs-lookup"><span data-stu-id="2e624-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="2e624-115">Na verdade, parte da classe de filme pode ser em um arquivo e parte em outro.</span><span class="sxs-lookup"><span data-stu-id="2e624-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="2e624-116">Isso é chamado de uma classe parcial.</span><span class="sxs-lookup"><span data-stu-id="2e624-116">This is called a Partial Class.</span></span> <span data-ttu-id="2e624-117">Vamos estender a classe de filme de outro arquivo.</span><span class="sxs-lookup"><span data-stu-id="2e624-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="2e624-118">Vamos criar uma classe parcial do filme que aponta para uma amigo "class" com alguns atributos que fornecerá dicas de validação para o sistema.</span><span class="sxs-lookup"><span data-stu-id="2e624-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="2e624-119">Vamos marcar o título e o preço, conforme necessário e também insistir que o preço estar em um determinado intervalo.</span><span class="sxs-lookup"><span data-stu-id="2e624-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="2e624-120">Clique com botão direito na pasta modelos e selecione Add Class.</span><span class="sxs-lookup"><span data-stu-id="2e624-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="2e624-121">Nomeie sua classe de filme e clique no botão Okey.</span><span class="sxs-lookup"><span data-stu-id="2e624-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="2e624-122">Aqui está a aparência de nossa pesquisa de classe parcial do filme.</span><span class="sxs-lookup"><span data-stu-id="2e624-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="2e624-123">Execute novamente o seu aplicativo e tentar inserir um filme com um preço acima de 100.</span><span class="sxs-lookup"><span data-stu-id="2e624-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="2e624-124">Você obterá um erro depois de enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="2e624-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="2e624-125">O erro é capturado no lado do servidor e ocorre depois que o formulário é postado.</span><span class="sxs-lookup"><span data-stu-id="2e624-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="2e624-126">Observe como os auxiliares HTML internos de ASP.NET MVC foram inteligentes o suficiente para exibir a mensagem de erro e mantenha os valores para que possamos dentro dos elementos da caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="2e624-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="2e624-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2e624-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="2e624-128">Isso funciona muito bem, mas seria interessante se poderíamos dar ao usuário no lado do cliente, imediatamente, antes que o servidor é envolvido.</span><span class="sxs-lookup"><span data-stu-id="2e624-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="2e624-129">Vamos habilitar alguma validação do lado do cliente com o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2e624-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="2e624-130">Adicionando uma validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="2e624-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="2e624-131">Como nossa classe Movie já tem alguns atributos de validação, estamos apenas será necessário adicionar alguns arquivos de JavaScript para nosso modelo de exibição de aspx e adicionar uma linha de código para habilitar a validação do lado do cliente entrar em vigor.</span><span class="sxs-lookup"><span data-stu-id="2e624-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="2e624-132">De dentro do VWD, acesse nossa pasta de modos de exibição/filme e abra aspx.</span><span class="sxs-lookup"><span data-stu-id="2e624-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="2e624-133">Abra a pasta de Scripts no Gerenciador de soluções e arraste os seguintes três scripts para dentro do &lt;head&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="2e624-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="2e624-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="2e624-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="2e624-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="2e624-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="2e624-136">Você deseja que esses arquivos de script são exibidos nessa ordem.</span><span class="sxs-lookup"><span data-stu-id="2e624-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="2e624-137">Além disso, adicione essa única linha acima a Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="2e624-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="2e624-138">Aqui está o código mostrado no IDE.</span><span class="sxs-lookup"><span data-stu-id="2e624-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="2e624-139">[![Filmes - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2e624-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="2e624-140">Executar seu aplicativo, visite /Movies/Create novamente e clique em criar sem inserir nenhum dado.</span><span class="sxs-lookup"><span data-stu-id="2e624-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="2e624-141">As mensagens de erro são exibidas imediatamente sem a página flash que associamos ao enviar dados de volta para o servidor.</span><span class="sxs-lookup"><span data-stu-id="2e624-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="2e624-142">Isso é porque o ASP.NET MVC agora está validando a entrada em ambos o cliente (usando JavaScript) e no servidor.</span><span class="sxs-lookup"><span data-stu-id="2e624-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="2e624-143">[![Criar – Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2e624-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="2e624-144">Isso está funcionando bem!</span><span class="sxs-lookup"><span data-stu-id="2e624-144">This is looking good!</span></span> <span data-ttu-id="2e624-145">Vamos agora adicionar uma coluna adicional para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2e624-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e624-146">[Anterior](getting-started-with-mvc-part6.md)
> [Próximo](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="2e624-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
