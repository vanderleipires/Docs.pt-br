---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introdução ao Tutorial do NerdDinner | Microsoft Docs
author: shanselman
description: É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um aplicativo pequeno, mas completo usando ASP.NE...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805104"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="87df8-104">Introdução ao Tutorial do NerdDinner</span><span class="sxs-lookup"><span data-stu-id="87df8-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="87df8-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="87df8-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="87df8-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="87df8-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="87df8-107">É a melhor maneira de aprender uma nova estrutura criar algo com ele.</span><span class="sxs-lookup"><span data-stu-id="87df8-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="87df8-108">Este tutorial explica como criar um pequeno, mas concluir, o aplicativo usando o ASP.NET MVC 1 e apresenta alguns dos principais conceitos por trás dele.</span><span class="sxs-lookup"><span data-stu-id="87df8-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="87df8-109">Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.</span><span class="sxs-lookup"><span data-stu-id="87df8-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="87df8-110">Tutorial do NerdDinner</span><span class="sxs-lookup"><span data-stu-id="87df8-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="87df8-111">É a melhor maneira de aprender uma nova estrutura criar algo com ele.</span><span class="sxs-lookup"><span data-stu-id="87df8-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="87df8-112">Este tutorial explica como criar um pequeno, mas concluir, o aplicativo usando o ASP.NET MVC e apresenta alguns dos principais conceitos por trás dele.</span><span class="sxs-lookup"><span data-stu-id="87df8-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="87df8-113">O aplicativo, vamos compilar é chamado "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="87df8-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="87df8-114">NerdDinner fornece uma maneira fácil para as pessoas a localizar e organizar os jantares online:</span><span class="sxs-lookup"><span data-stu-id="87df8-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="87df8-115">NerdDinner permite que os usuários registrados para criar, editar e excluir os jantares.</span><span class="sxs-lookup"><span data-stu-id="87df8-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="87df8-116">Impõe um conjunto consistente de validação e regras de negócios em todo o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="87df8-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="87df8-117">Os visitantes podem usar um mapa de baseados em AJAX para procurar próximos jantares sendo mantidos próximo a eles:</span><span class="sxs-lookup"><span data-stu-id="87df8-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="87df8-118">Clicar em um jantar vai levá-los a uma página de detalhes em que eles possam aprender mais sobre ele:</span><span class="sxs-lookup"><span data-stu-id="87df8-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="87df8-119">Se ele está interessado em participar da dinner podem fazer logon ou registre-se no site:</span><span class="sxs-lookup"><span data-stu-id="87df8-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="87df8-120">Eles podem, em seguida, clique em um link de RSVP baseados em AJAX para participar do evento:</span><span class="sxs-lookup"><span data-stu-id="87df8-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="87df8-121">A implementação do NerdDinner</span><span class="sxs-lookup"><span data-stu-id="87df8-121">Implementing NerdDinner</span></span>

<span data-ttu-id="87df8-122">Vamos começar nosso aplicativo NerdDinner, usando o arquivo -&gt;comando de novo projeto no Visual Studio para criar um novo projeto do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="87df8-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="87df8-123">Em seguida, incrementalmente adicionaremos recursos e funcionalidades.</span><span class="sxs-lookup"><span data-stu-id="87df8-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="87df8-124">Ao longo do caminho que abordaremos:</span><span class="sxs-lookup"><span data-stu-id="87df8-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="87df8-125">Como criar um novo projeto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="87df8-125">How to create a new ASP.NET MVC Project</span></span>](# "criar um novo projeto ASP.NET MVC")
2. [<span data-ttu-id="87df8-126">Como criar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="87df8-126">How to create a database</span></span>](# "criar um banco de dados")
3. [<span data-ttu-id="87df8-127">Como criar um modelo com validações de regra de negócio</span><span class="sxs-lookup"><span data-stu-id="87df8-127">How to build a model with business rule validations</span></span>](# "criar um modelo com validações de regra de negócios")
4. [<span data-ttu-id="87df8-128">Como usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes</span><span class="sxs-lookup"><span data-stu-id="87df8-128">How to use controllers and views to implement a listing/details UI</span></span>](# "usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes")
5. <span data-ttu-id="87df8-129">[Como fornecer CRUD (criar, ler, atualizar e excluir) dados de suporte de entrada de formulário](# "fornecem CRUD (Create, Read, Update, Delete) entrada de formulário de dados de suporte")</span><span class="sxs-lookup"><span data-stu-id="87df8-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="87df8-130">Como usar ViewData e implementar classes ViewModel</span><span class="sxs-lookup"><span data-stu-id="87df8-130">How to use ViewData and implement ViewModel classes</span></span>](# "usar ViewData e implementar Classes de ViewModel")
7. [<span data-ttu-id="87df8-131">Como usar novamente a interface do usuário usando páginas mestras e parciais</span><span class="sxs-lookup"><span data-stu-id="87df8-131">How to re-use UI using master pages and partials</span></span>](# "reutilização da interface do usuário usando páginas mestras e parciais")
8. [<span data-ttu-id="87df8-132">Como implementar a paginação de dados eficiente</span><span class="sxs-lookup"><span data-stu-id="87df8-132">How to implement efficient data paging</span></span>](# "implementar eficiente dados de paginação")
9. [<span data-ttu-id="87df8-133">Como proteger aplicativos usando a autenticação e autorização</span><span class="sxs-lookup"><span data-stu-id="87df8-133">How to secure applications using authentication and authorization</span></span>](# "aplicativos usando autenticação e autorização seguras")
10. [<span data-ttu-id="87df8-134">Como usar o AJAX para fornecer atualizações dinâmicas</span><span class="sxs-lookup"><span data-stu-id="87df8-134">How to use AJAX to deliver dynamic updates</span></span>](# "usar AJAX para fornecer atualizações dinâmicas")
11. [<span data-ttu-id="87df8-135">Como usar o AJAX para implementar cenários de mapeamento</span><span class="sxs-lookup"><span data-stu-id="87df8-135">How to use AJAX to implement mapping scenarios</span></span>](# "usar AJAX para implementar cenários de mapeamento")
12. [<span data-ttu-id="87df8-136">Como habilitar o teste de unidade automatizado</span><span class="sxs-lookup"><span data-stu-id="87df8-136">How to enable automated unit testing</span></span>](# "habilitar testes de unidade automatizados")

<span data-ttu-id="87df8-137">Você pode criar sua própria cópia do NerdDinner do zero ao concluir cada etapa, passo a passo neste capítulo.</span><span class="sxs-lookup"><span data-stu-id="87df8-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="87df8-138">Como alternativa, você pode baixar uma versão completa do código-fonte aqui: [NerdDinner no GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="87df8-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="87df8-139">Também é possível também opcionalmente [baixar uma versão gratuita do PDF deste tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se você quiser ler o tutorial off-line.</span><span class="sxs-lookup"><span data-stu-id="87df8-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="87df8-140">Você pode usar o Visual Studio 2008 ou o gratuito Visual Web Developer 2008 Express para compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="87df8-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="87df8-141">Você pode usar o SQL Server ou o Express do SQL Server gratuito para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="87df8-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="87df8-142">Você pode instalar o ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (gratuita) o usando V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="87df8-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="87df8-143">Agora vamos começar...</span><span class="sxs-lookup"><span data-stu-id="87df8-143">Now let's get started....</span></span>

<span data-ttu-id="87df8-144">Agora que já abordamos o que é NerdDinner, vamos arregaçar nossas mangas e escrever algum código.</span><span class="sxs-lookup"><span data-stu-id="87df8-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="87df8-145">Vamos começar usando arquivo -&gt;novo projeto no Visual Studio para criar o aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="87df8-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="87df8-146">Avançar</span><span class="sxs-lookup"><span data-stu-id="87df8-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
