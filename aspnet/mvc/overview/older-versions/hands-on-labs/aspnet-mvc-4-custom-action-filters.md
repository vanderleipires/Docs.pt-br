---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "Filtros de ação personalizada do ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC fornece filtros de ação para a execução de lógica de filtragem antes ou depois de um método de ação é chamado. Filtros de ação são tha de atributos personalizados..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 103cd68c576463d87d0077cc149f9b89c6e028e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="bcc8d-104">Filtros de ação personalizada do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="bcc8d-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="bcc8d-105">por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="bcc8d-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="bcc8d-106">ASP.NET MVC fornece filtros de ação para a execução de lógica de filtragem antes ou depois de um método de ação é chamado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="bcc8d-107">Filtros de ação são os atributos personalizados que fornecem um meio declarativo para adicionar o comportamento de ação de pré e pós-ação para métodos de ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="bcc8d-108">Este laboratório prático, você criará um atributo de filtro de ação personalizada na solução MvcMusicStore capturar solicitações do controlador e registra a atividade de um site em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="bcc8d-109">Você poderá adicionar o filtro de registro em log pela inclusão de qualquer controlador ou ação.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="bcc8d-110">Por fim, você verá o modo de exibição de log que mostra a lista de visitantes.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="bcc8d-111">Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="bcc8d-112">Se você não usou **ASP.NET MVC** antes, é recomendável que você passe **conceitos básicos do ASP.NET MVC 4** laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="bcc8d-113">Objetivos</span><span class="sxs-lookup"><span data-stu-id="bcc8d-113">Objectives</span></span>

<span data-ttu-id="bcc8d-114">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="bcc8d-115">Crie um atributo de filtro de ação personalizada para estender os recursos de filtragem</span><span class="sxs-lookup"><span data-stu-id="bcc8d-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="bcc8d-116">Aplicar um atributo de filtro personalizado por injeção de um nível específico</span><span class="sxs-lookup"><span data-stu-id="bcc8d-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="bcc8d-117">Registrar um filtros de ação personalizada globalmente</span><span class="sxs-lookup"><span data-stu-id="bcc8d-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="bcc8d-118">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="bcc8d-118">Prerequisites</span></span>

<span data-ttu-id="bcc8d-119">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="bcc8d-120">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="bcc8d-121">Configuração</span><span class="sxs-lookup"><span data-stu-id="bcc8d-121">Setup</span></span>

<span data-ttu-id="bcc8d-122">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="bcc8d-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="bcc8d-123">Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="bcc8d-124">Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="bcc8d-125">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="bcc8d-126">Exercícios</span><span class="sxs-lookup"><span data-stu-id="bcc8d-126">Exercises</span></span>

<span data-ttu-id="bcc8d-127">Este laboratório prático é composto pelos exercícios a seguir:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="bcc8d-128">Exercício 1: O log de ações</span><span class="sxs-lookup"><span data-stu-id="bcc8d-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="bcc8d-129">Exercício 2: Gerenciar vários filtros de ação</span><span class="sxs-lookup"><span data-stu-id="bcc8d-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="bcc8d-130">Tempo estimado para concluir este laboratório: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc8d-131">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="bcc8d-132">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="bcc8d-133">Exercício 1: O log de ações</span><span class="sxs-lookup"><span data-stu-id="bcc8d-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="bcc8d-134">Neste exercício, você aprenderá como criar um filtro de log de ação personalizada usando provedores de filtro do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="bcc8d-135">Para essa finalidade, você aplicará um filtro de registro em log para o site MusicStore que registra todas as atividades em controladores de selecionada.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="bcc8d-136">O filtro estenderá **ActionFilterAttributeClass** e substituir **OnActionExecuting** método capturar cada solicitação e, em seguida, execute as ações de registro em log.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="bcc8d-137">As contexto informações sobre solicitações HTTP, executar métodos, resultados e parâmetros será fornecida pelo ASP.NET MVC **ActionExecutingContext** classe **.**</span><span class="sxs-lookup"><span data-stu-id="bcc8d-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="bcc8d-138">Além disso, o ASP.NET MVC 4 tem provedores de filtros de padrão, você pode usar sem criar um filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="bcc8d-139">ASP.NET MVC 4 fornece os seguintes tipos de filtros:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="bcc8d-140">**Autorização** filtrar, que toma decisões de segurança sobre a possibilidade de executar um método de ação, como autenticação ou validação de propriedades da solicitação.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="bcc8d-141">**Ação** filtro, que envolve a execução do método de ação.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="bcc8d-142">Esse filtro pode executar processamento adicional, como fornecer dados adicionais para o método de ação, inspecionando o valor de retorno ou cancelando a execução do método de ação</span><span class="sxs-lookup"><span data-stu-id="bcc8d-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="bcc8d-143">**Resultado** filtro, que envolve a execução do objeto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="bcc8d-144">Esse filtro pode executar processamento adicional dos resultados, como modificar a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="bcc8d-145">**Exceção** filtro, que é executado se houver uma exceção sem tratamento em algum lugar lançada no método de ação, começando com os filtros de autorização e terminando com a execução do resultado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="bcc8d-146">Filtros de exceção podem ser usados para tarefas como registrar em log ou exibir uma página de erro.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="bcc8d-147">Para obter mais informações sobre filtros de provedores, visite este link do MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="bcc8d-148">Sobre o recurso de log de aplicativo de repositório de música do MVC</span><span class="sxs-lookup"><span data-stu-id="bcc8d-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="bcc8d-149">Esta solução de repositório de música tem uma nova tabela de modelo de dados para o log do site, **ActionLog**, com os seguintes campos: nome do controlador que recebeu uma solicitação, a ação de chamado, o IP do cliente e o carimbo de data / hora.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="bcc8d-150">![Modelo de dados. Tabela ActionLog. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de dados. Tabela ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="bcc8d-151">*Modelo de dados - ActionLog tabela*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="bcc8d-152">A solução oferece um modo de exibição do ASP.NET MVC para o log de ações que pode ser encontrado em **MvcMusicStores/exibições/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="bcc8d-153">![Exibição de Log de ação](aspnet-mvc-4-custom-action-filters/_static/image2.png "exibição de Log de ações")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="bcc8d-154">*Exibição de Log de ação*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-154">*Action Log view*</span></span>

<span data-ttu-id="bcc8d-155">Com isso considerando a estrutura, todo o trabalho se concentrarão na solicitação do controlador de interrupção e executar o registro em log usando o recurso de filtragem personalizada.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="bcc8d-156">Tarefa 1: criar um filtro personalizado para capturar a solicitação do controlador</span><span class="sxs-lookup"><span data-stu-id="bcc8d-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="bcc8d-157">Nesta tarefa, você criará uma classe de atributo de filtro personalizado que contém a lógica de registro em log.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="bcc8d-158">Para essa finalidade, você estenderá o ASP.NET MVC **ActionFilterAttribute** classe e implementar a interface **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc8d-159">O **ActionFilterAttribute** é a classe base para todos os filtros de atributo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="bcc8d-160">Ele fornece os seguintes métodos para executar uma lógica específica depois e antes da execução da ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="bcc8d-161">**OnActionExecuting**(ActionExecutingContext filterContext): antes da ação de método é chamado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="bcc8d-162">**OnActionExecuted**(ActionExecutedContext filterContext): depois que o método de ação é chamado e antes do resultado ser executado (antes da renderização de exibição).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="bcc8d-163">**OnResultExecuting**(ResultExecutingContext filterContext): antes do resultado ser executado (antes da renderização de exibição).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="bcc8d-164">**OnResultExecuted**(ResultExecutedContext filterContext): após o resultado é executado (depois que a exibição é renderizada).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="bcc8d-165">Substituindo qualquer um desses métodos em uma classe derivada, você pode executar seu próprio código de filtragem.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="bcc8d-166">Abra o **começar** solução localizado em **\Source\Ex01-LoggingActions\Begin** pasta.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="bcc8d-167">Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="bcc8d-168">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="bcc8d-169">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="bcc8d-170">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bcc8d-171">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bcc8d-172">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bcc8d-173">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="bcc8d-174">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="bcc8d-175">Adicionar uma nova classe c# para o **filtros** pasta e nomeie-o *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="bcc8d-176">Essa pasta armazena todos os filtros personalizados.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="bcc8d-177">Abra **CustomActionFilter.cs** e adicione uma referência a **System.Web.Mvc** e **MvcMusicStore.Models** namespaces:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="bcc8d-178">(Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="bcc8d-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="bcc8d-179">Herdar o **CustomActionFilter** classe **ActionFilterAttribute** e, em seguida, fazer **CustomActionFilter** implementar classe **IActionFilter** interface.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="bcc8d-180">Verifique **CustomActionFilter** classe substituir o método **OnActionExecuting** e adicione a lógica necessária para registrar a execução do filtro.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="bcc8d-181">Para fazer isso, adicione o seguinte código dentro de **CustomActionFilter** classe.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="bcc8d-182">(Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="bcc8d-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="bcc8d-183">**OnActionExecuting** usando o método **do Entity Framework** para adicionar um novo registro de ActionLog.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="bcc8d-184">Ele cria e preenche uma nova instância de entidade com as informações de contexto do **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="bcc8d-185">Você pode ler mais sobre **ControllerContext** classe em [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="bcc8d-186">Tarefa 2 - injetar um interceptador de código para a classe do controlador de armazenamento</span><span class="sxs-lookup"><span data-stu-id="bcc8d-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="bcc8d-187">Nesta tarefa, você irá adicionar o filtro personalizado injetando-lo a todas as classes do controlador e ações do controlador que serão registradas.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="bcc8d-188">Neste exercício, a classe do controlador de armazenamento terá um log.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="bcc8d-189">O método **OnActionExecuting** de **ActionLogFilterAttribute** filtro personalizado é executado quando um elemento injetado é chamado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="bcc8d-190">Também é possível interceptar um método do controlador específico.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="bcc8d-191">Abra o **StoreController** em **MvcMusicStore\Controllers** e adicione uma referência para o **filtros** namespace:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="bcc8d-192">Inserir o filtro personalizado **CustomActionFilter** em **StoreController** classe adicionando **[CustomActionFilter]** atributo antes da declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="bcc8d-193">Quando um filtro é injetado em uma classe de controlador, todas as suas ações também são injetadas.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="bcc8d-194">Se você deseja aplicar o filtro apenas para um conjunto de ações, você precisaria injetar **[CustomActionFilter]** para cada um deles:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="bcc8d-195">Tarefa 3: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bcc8d-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="bcc8d-196">Nesta tarefa, você testará que o filtro de registro em log está funcionando.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="bcc8d-197">Inicie o aplicativo e visitar a loja e, em seguida, você verificará as atividades registradas.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="bcc8d-198">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="bcc8d-199">Navegue até **/ActionLog** para ver o estado inicial de exibição de log:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="bcc8d-200">![Registrar o status do controlador antes da atividade de página](aspnet-mvc-4-custom-action-filters/_static/image3.png "registrar status do controlador antes da atividade de página")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="bcc8d-201">*Status do controlador do log antes da atividade de página*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bcc8d-202">Por padrão, ele sempre mostrará um item que é gerado quando recuperar os gêneros existentes para o menu.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="bcc8d-203">Para fins de simplicidade, está limpando o **ActionLog** sempre que o aplicativo é executado para mostrará apenas os logs de verificação da cada tarefa específica tabela.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="bcc8d-204">Você pode precisar remover o código a seguir do **sessão\_iniciar** método (no **global. asax** classe), para salvar um log de histórico para todas as ações executadas no repositório de Controlador.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="bcc8d-205">Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="bcc8d-206">Navegue até **/ActionLog** e se o log estiver vazio pressione **F5** para atualizar a página.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="bcc8d-207">Verifique se suas visitas foram rastreadas:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="bcc8d-208">![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image4.png "log de ações com atividades registradas")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="bcc8d-209">*Log de ação com atividades registradas*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="bcc8d-210">Exercício 2: Gerenciar vários filtros de ação</span><span class="sxs-lookup"><span data-stu-id="bcc8d-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="bcc8d-211">Neste exercício, você irá adicionar um segundo filtro de ação personalizada para a classe StoreController e definir da ordem específica no qual ambos os filtros serão executados.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="bcc8d-212">Em seguida, você atualizará o código para registrar o filtro global.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="bcc8d-213">Há diferentes opções para levar em conta ao definir a ordem de execução de filtros.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="bcc8d-214">Por exemplo, a propriedade de ordem e o escopo de filtros:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="bcc8d-215">Você pode definir um **escopo** para cada um dos filtros, por exemplo, você pode definir o escopo todos os filtros de ação para ser executado no **escopo controlador**e todos os filtros de autorização para executar em **escopo Global** .</span><span class="sxs-lookup"><span data-stu-id="bcc8d-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="bcc8d-216">Os escopos terão a ordem de execução definido.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="bcc8d-217">Além disso, cada filtro de ação tem uma propriedade de ordem que é usada para determinar a ordem de execução no escopo do filtro.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="bcc8d-218">Para obter mais informações sobre a ordem de execução de filtros de ação personalizada, visite este artigo do MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="bcc8d-219">Tarefa 1: Criando um novo filtro de ação personalizada</span><span class="sxs-lookup"><span data-stu-id="bcc8d-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="bcc8d-220">Nesta tarefa, você criará um novo filtro de ação personalizada para injetar na classe StoreController, aprender como gerenciar a ordem de execução dos filtros.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="bcc8d-221">Abra o **começar** solução localizado em **\Source\Ex02-ManagingMultipleActionFilters\Begin** pasta.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="bcc8d-222">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="bcc8d-223">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bcc8d-224">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="bcc8d-225">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="bcc8d-226">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="bcc8d-227">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bcc8d-228">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bcc8d-229">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="bcc8d-230">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="bcc8d-231">Adicionar uma nova classe c# para o **filtros** pasta e nomeie-o *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="bcc8d-232">Abra **MyNewCustomActionFilter.cs** e adicione uma referência a **System.Web.Mvc** e **MvcMusicStore.Models** namespace:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="bcc8d-233">(Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - o Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="bcc8d-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="bcc8d-234">Substitua a declaração de classe padrão com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="bcc8d-235">(Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - o Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="bcc8d-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="bcc8d-236">Esse filtro de ação personalizada é quase o mesmo que você criou no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="bcc8d-237">A principal diferença é que ela tem o  *&quot;registradas por&quot;*  atributo atualizado com de nome essa nova classe para identificar qual filtro registrado no log.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="bcc8d-238">Tarefa 2: Inserindo um novo interceptador de código na classe StoreController</span><span class="sxs-lookup"><span data-stu-id="bcc8d-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="bcc8d-239">Nesta tarefa, você irá adicionar um novo filtro personalizado na classe StoreController e executar a solução para verificar como ambos os filtros funcionam em conjunto.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="bcc8d-240">Abra o **StoreController** classe localizado em **MvcMusicStore\Controllers** e inserir o novo filtro personalizado **MyNewCustomActionFilter** em  **StoreController** classe como é mostrado no código a seguir.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="bcc8d-241">Agora, execute o aplicativo para ver como funcionam essas dois filtros de ação personalizada.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="bcc8d-242">Para fazer isso, pressione **F5** e aguarde até que o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="bcc8d-243">Navegue até **/ActionLog** para ver o estado inicial de exibição de log.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="bcc8d-244">![Registrar o status do controlador antes da atividade de página](aspnet-mvc-4-custom-action-filters/_static/image5.png "registrar status do controlador antes da atividade de página")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="bcc8d-245">*Status do controlador do log antes da atividade de página*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="bcc8d-246">Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="bcc8d-247">Verifique se neste momento. suas visitas eram controladas duas vezes: depois de cada um dos filtros de ação personalizada que você adicionou no **StorageController** classe.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="bcc8d-248">![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image6.png "log de ações com atividades registradas")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="bcc8d-249">*Log de ação com atividades registradas*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="bcc8d-250">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="bcc8d-251">Tarefa 3: Gerenciando a ordem do filtro</span><span class="sxs-lookup"><span data-stu-id="bcc8d-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="bcc8d-252">Nesta tarefa, você aprenderá como gerenciar a ordem de execução de filtros usando a propriedade de ordem.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="bcc8d-253">Abra o **StoreController** classe localizado em **MvcMusicStore\Controllers** e especifique o **ordem** propriedade em ambos os filtros, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="bcc8d-254">Agora, verificar como os filtros são executados dependendo do valor da propriedade sua ordem.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="bcc8d-255">Você encontrará a que o filtro com o menor valor de ordem (**CustomActionFilter**) é o primeiro que é executado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="bcc8d-256">Pressione **F5** e aguarde até que o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="bcc8d-257">Navegue até **/ActionLog** para ver o estado inicial de exibição de log.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="bcc8d-258">![Registrar o status do controlador antes da atividade de página](aspnet-mvc-4-custom-action-filters/_static/image7.png "registrar status do controlador antes da atividade de página")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="bcc8d-259">*Status do controlador do log antes da atividade de página*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="bcc8d-260">Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="bcc8d-261">Verifique se esse tempo, suas visitas eram controladas ordenados pelo valor de ordem de filtros: **CustomActionFilter** logs' primeiro.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="bcc8d-262">![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image8.png "log de ações com atividades registradas")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="bcc8d-263">*Log de ação com atividades registradas*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="bcc8d-264">Agora, você atualizar o valor de ordem de filtros e verificar como a ordem de registro em log é alterado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="bcc8d-265">No **StoreController** classe, atualize o valor de ordem de filtros como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="bcc8d-266">Execute o aplicativo novamente pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="bcc8d-267">Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="bcc8d-268">Verifique se esse tempo, os logs criadas por **MyNewCustomActionFilter** filtro aparece primeiro.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="bcc8d-269">![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image9.png "log de ações com atividades registradas")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="bcc8d-270">*Log de ação com atividades registradas*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="bcc8d-271">Tarefa 4: Registrar filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="bcc8d-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="bcc8d-272">Nesta tarefa, você atualizará a solução para registrar o novo filtro (**MyNewCustomActionFilter**) como um filtro global.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="bcc8d-273">Ao fazer isso, ele será disparado por todo a executada ações no aplicativo e não apenas em que o StoreController da tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="bcc8d-274">Em **StoreController** classe, remova **[MyNewCustomActionFilter]** atributo e a propriedade de ordem de **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="bcc8d-275">Ela deve parecer com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="bcc8d-276">Abra **global. asax** de arquivo e localize o **aplicativo\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="bcc8d-277">Observe que cada thime o aplicativo inicia está registrando os filtros globais chamando **RegisterGlobalFilters** método **FilterConfig** classe.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="bcc8d-278">![Registro de filtros globais em global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registro de filtros globais em global. asax")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="bcc8d-279">*Registro de filtros globais em global. asax*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="bcc8d-280">Abra **FilterConfig.cs** dentro do arquivo **aplicativo\_iniciar** pasta.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="bcc8d-281">Adicione uma referência ao uso de System.Web.Mvc; usando MvcMusicStore.Filters; namespace.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="bcc8d-282">Atualização **RegisterGlobalFilters** método adicionando seu filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="bcc8d-283">Para fazer isso, adicione o código realçado:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="bcc8d-284">Execute o aplicativo pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="bcc8d-285">Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="bcc8d-286">Verificação de que agora **[MyNewCustomActionFilter]** está sendo injetado em HomeController e ActionLogController muito.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="bcc8d-287">![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image11.png "log de ações com atividades registradas")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="bcc8d-288">*Log de ação com a atividade global conectada*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="bcc8d-289">Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="bcc8d-290">Resumo</span><span class="sxs-lookup"><span data-stu-id="bcc8d-290">Summary</span></span>

<span data-ttu-id="bcc8d-291">Ao concluir este laboratório prático, você aprendeu como estender um filtro de ação para executar ações personalizadas.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="bcc8d-292">Você também aprendeu como injetar qualquer filtro com os controladores de página.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="bcc8d-293">Foram usados os seguintes conceitos:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-293">The following concepts were used:</span></span>

- <span data-ttu-id="bcc8d-294">Como criar filtros de ação personalizada com a classe de ActionFilterAttribute do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bcc8d-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="bcc8d-295">Como inserir filtros em controladores de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bcc8d-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="bcc8d-296">Como gerenciar a ordenação de filtro usando a propriedade de ordem</span><span class="sxs-lookup"><span data-stu-id="bcc8d-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="bcc8d-297">Como registrar filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="bcc8d-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="bcc8d-298">Apêndice a: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="bcc8d-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="bcc8d-299">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="bcc8d-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="bcc8d-300">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="bcc8d-301">Vá para [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="bcc8d-302">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="bcc8d-303">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-303">Click on **Install Now**.</span></span> <span data-ttu-id="bcc8d-304">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="bcc8d-305">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="bcc8d-306">![Instalar Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="bcc8d-307">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="bcc8d-308">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="bcc8d-310">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="bcc8d-311">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-311">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="bcc8d-313">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-313">*Installation progress*</span></span>
6. <span data-ttu-id="bcc8d-314">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-314">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="bcc8d-316">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-316">*Installation completed*</span></span>
7. <span data-ttu-id="bcc8d-317">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="bcc8d-318">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="bcc8d-320">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="bcc8d-321">Apêndice b: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="bcc8d-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="bcc8d-322">Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="bcc8d-323">Tarefa 1: criar um novo Site do Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bcc8d-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="bcc8d-324">Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bcc8d-325">Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="bcc8d-326">Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="bcc8d-327">![Faça logon no portal do Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="bcc8d-328">*Faça logon no Portal de gerenciamento do Azure do Windows*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="bcc8d-329">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="bcc8d-330">![Criando um novo Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "criar um novo Site")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="bcc8d-331">*Criando um novo Site*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="bcc8d-332">Clique em **de computação** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="bcc8d-333">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="bcc8d-334">Forneça uma URL disponível para o novo site e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bcc8d-335">Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="bcc8d-336">A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="bcc8d-337">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="bcc8d-338">![Criando um novo Site usando a criação rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "criar um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="bcc8d-339">*Criando um novo Site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="bcc8d-340">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="bcc8d-341">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="bcc8d-342">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="bcc8d-343">![Navegando para o novo site](aspnet-mvc-4-custom-action-filters/_static/image20.png "navegando para o novo site")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="bcc8d-344">*Navegando para o novo site*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="bcc8d-345">![Site da Web em execução](aspnet-mvc-4-custom-action-filters/_static/image21.png "site em execução")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="bcc8d-346">*Site da Web em execução*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-346">*Web site running*</span></span>
6. <span data-ttu-id="bcc8d-347">Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="bcc8d-348">![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-custom-action-filters/_static/image22.png "abrir páginas de gerenciamento do site")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="bcc8d-349">*Abrir as páginas de gerenciamento do Site da Web*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="bcc8d-350">No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bcc8d-351">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="bcc8d-352">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="bcc8d-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="bcc8d-354">![Baixando o site da web de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image23.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="bcc8d-355">*Baixando o Site da Web de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="bcc8d-356">Baixe o arquivo de perfil de publicação para um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="bcc8d-357">Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="bcc8d-358">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image24.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="bcc8d-359">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="bcc8d-360">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="bcc8d-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="bcc8d-361">Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="bcc8d-362">Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="bcc8d-363">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="bcc8d-364">Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="bcc8d-365">Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="bcc8d-366">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="bcc8d-367">Não crie o banco de dados, como ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="bcc8d-368">![Painel do servidor de banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="bcc8d-369">*Painel de banco de dados do SQL Server*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="bcc8d-370">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="bcc8d-371">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) botão.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="bcc8d-373">*Adicionar endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="bcc8d-374">Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="bcc8d-376">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="bcc8d-377">Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="bcc8d-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="bcc8d-378">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="bcc8d-379">No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="bcc8d-380">![Publicar o aplicativo](aspnet-mvc-4-custom-action-filters/_static/image29.png "a publicação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="bcc8d-381">*Publicar o site*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="bcc8d-382">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="bcc8d-383">![Importar o perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image30.png "importar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="bcc8d-384">*Importar o perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="bcc8d-385">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-385">Click **Validate Connection**.</span></span> <span data-ttu-id="bcc8d-386">Quando a validação estiver concluída, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bcc8d-387">A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="bcc8d-388">![Validando a conexão](aspnet-mvc-4-custom-action-filters/_static/image31.png "validação de conexão")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="bcc8d-389">*Validação de conexão*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-389">*Validating connection*</span></span>
4. <span data-ttu-id="bcc8d-390">No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="bcc8d-391">![Configuração de implantação da Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="bcc8d-392">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="bcc8d-393">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="bcc8d-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="bcc8d-394">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="bcc8d-395">Em **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="bcc8d-396">Em **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="bcc8d-397">Digite um novo nome de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-397">Type a new database name.</span></span>

    <span data-ttu-id="bcc8d-398">![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="bcc8d-399">*Configurando a cadeia de caracteres de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="bcc8d-400">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-400">Then click **OK**.</span></span> <span data-ttu-id="bcc8d-401">Quando solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="bcc8d-402">![Criando o banco de dados](aspnet-mvc-4-custom-action-filters/_static/image34.png "criar a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="bcc8d-403">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-403">*Creating the database*</span></span>
7. <span data-ttu-id="bcc8d-404">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="bcc8d-405">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-405">Then click **Next**.</span></span>

    <span data-ttu-id="bcc8d-406">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="bcc8d-407">*Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="bcc8d-408">No **visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="bcc8d-409">![Publicar o aplicativo web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publicar o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="bcc8d-410">*Publicar o aplicativo web*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="bcc8d-411">Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="bcc8d-412">Apêndice c: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="bcc8d-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="bcc8d-413">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="bcc8d-414">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="bcc8d-415">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-custom-action-filters/_static/image37.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="bcc8d-416">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="bcc8d-417">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="bcc8d-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="bcc8d-418">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="bcc8d-419">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="bcc8d-420">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="bcc8d-421">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="bcc8d-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="bcc8d-422">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="bcc8d-423">![Comece a digitar o nome do fragmento](aspnet-mvc-4-custom-action-filters/_static/image38.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="bcc8d-424">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="bcc8d-425">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-custom-action-filters/_static/image39.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="bcc8d-426">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="bcc8d-427">![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-custom-action-filters/_static/image40.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="bcc8d-428">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="bcc8d-429">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="bcc8d-430">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="bcc8d-431">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="bcc8d-432">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="bcc8d-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="bcc8d-433">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-custom-action-filters/_static/image41.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="bcc8d-434">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="bcc8d-435">![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-custom-action-filters/_static/image42.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="bcc8d-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="bcc8d-436">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="bcc8d-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
