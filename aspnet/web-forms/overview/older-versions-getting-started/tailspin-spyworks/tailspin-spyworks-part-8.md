---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Páginas finais, tratamento de exceção e conclusão | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 8 adiciona uma página de contato, sobre a página e a exceção...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f1d855e157f6a58995d301a793e660925767fb4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380247"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="88693-104">Parte 8: Páginas finais, tratamento de exceção e conclusão</span><span class="sxs-lookup"><span data-stu-id="88693-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="88693-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="88693-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="88693-106">Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="88693-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="88693-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="88693-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="88693-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="88693-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="88693-109">Parte 8 adiciona uma página de contato, sobre a página e manipulação de exceção.</span><span class="sxs-lookup"><span data-stu-id="88693-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="88693-110">Isso é a conclusão da série.</span><span class="sxs-lookup"><span data-stu-id="88693-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="88693-111">Entre em contato com a página (enviar e-mail do ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="88693-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="88693-112">Criar uma nova página chamada ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="88693-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="88693-113">Usando o designer, crie o seguinte formulário anotar especial para incluir o ToolkitScriptManager e o controle de Editor do AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="88693-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="88693-114">.</span><span class="sxs-lookup"><span data-stu-id="88693-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="88693-115">Clique duas vezes no botão "Enviar" para gerar um manipulador de eventos no arquivo code-behind e implementar um método para enviar as informações de contato como um email.</span><span class="sxs-lookup"><span data-stu-id="88693-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="88693-116">Esse código requer que seu arquivo Web. config contém uma entrada na seção de configuração que especifica o servidor SMTP a ser usado para enviar um email.</span><span class="sxs-lookup"><span data-stu-id="88693-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="88693-117">Sobre a página</span><span class="sxs-lookup"><span data-stu-id="88693-117">About Page</span></span>

<span data-ttu-id="88693-118">Crie uma página chamada AboutUs.aspx e adicione qualquer conteúdo que você deseja.</span><span class="sxs-lookup"><span data-stu-id="88693-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="88693-119">Manipulador de exceção global</span><span class="sxs-lookup"><span data-stu-id="88693-119">Global Exception Handler</span></span>

<span data-ttu-id="88693-120">Por fim, em todo o aplicativo, podemos ter lançada exceções e há circunstâncias imprevistas ou cold também causa sem tratamento de exceções em nosso aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="88693-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="88693-121">Queremos que nunca uma exceção sem tratamento a ser exibida para um visitante de site da web.</span><span class="sxs-lookup"><span data-stu-id="88693-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="88693-122">Além de ser uma experiência de usuário terrível exceções sem tratamento também podem ser um problema de segurança.</span><span class="sxs-lookup"><span data-stu-id="88693-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="88693-123">Para resolver esse problema, podemos irá implementar um manipulador de exceção global.</span><span class="sxs-lookup"><span data-stu-id="88693-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="88693-124">Para fazer isso, abra o arquivo global. asax e observe o seguinte manipulador de eventos gerados previamente.</span><span class="sxs-lookup"><span data-stu-id="88693-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="88693-125">Adicione código para implementar o aplicativo\_manipulador de erro da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="88693-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="88693-126">Em seguida, adicione uma página chamada Error.aspx à solução e adicione esse trecho de marcação.</span><span class="sxs-lookup"><span data-stu-id="88693-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="88693-127">Agora, na página\_carregar extração de manipulador de eventos, as mensagens de erro do objeto de solicitação.</span><span class="sxs-lookup"><span data-stu-id="88693-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="88693-128">Conclusão</span><span class="sxs-lookup"><span data-stu-id="88693-128">Conclusion</span></span>

<span data-ttu-id="88693-129">Já vimos que WebForms ASP.NET torna mais fácil para criar um site da Web sofisticado com acesso de banco de dados, associação, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="88693-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="88693-130">muito rapidamente.</span><span class="sxs-lookup"><span data-stu-id="88693-130">pretty quickly.</span></span>

<span data-ttu-id="88693-131">Espero que este tutorial tenha fornecido as ferramentas que você precisa para começar a criar seu próprio ASP.NET WebForms aplicativos!</span><span class="sxs-lookup"><span data-stu-id="88693-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="88693-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="88693-132">Previous</span></span>](tailspin-spyworks-part-7.md)
