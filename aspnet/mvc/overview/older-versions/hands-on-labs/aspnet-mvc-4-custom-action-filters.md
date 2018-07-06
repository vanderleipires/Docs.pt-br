---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de ação personalizada do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fornece filtros de ação para executar a lógica de filtragem antes ou depois que um método de ação é chamado. Filtros de ação são tha de atributos personalizados...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: c435fef624d526ceb01dbc370c5df52e2a1e8350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807929"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="c5374-104">Filtros de ação personalizada do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c5374-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="c5374-105">por [Web Camps equipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c5374-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c5374-106">Baixe o Kit de treinamento do Web Camps</span><span class="sxs-lookup"><span data-stu-id="c5374-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="c5374-107">ASP.NET MVC fornece filtros de ação para executar a lógica de filtragem antes ou depois que um método de ação é chamado.</span><span class="sxs-lookup"><span data-stu-id="c5374-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="c5374-108">Filtros de ação são os atributos personalizados que fornecem o meio declarativo para adicionar o comportamento de ação pré e pós-ação aos métodos de ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="c5374-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="c5374-109">Este laboratório prático, você criará um atributo de filtro de ação personalizada à solução MvcMusicStore para capturar solicitações do controlador e a atividade de um site em uma tabela de banco de dados de log.</span><span class="sxs-lookup"><span data-stu-id="c5374-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="c5374-110">Você poderá adicionar o filtro de log pela injeção a qualquer controlador ou ação.</span><span class="sxs-lookup"><span data-stu-id="c5374-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="c5374-111">Por fim, você verá o modo de exibição de log que mostra a lista de visitantes.</span><span class="sxs-lookup"><span data-stu-id="c5374-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="c5374-112">Este laboratório prático pressupõe que você tenha um conhecimento básico dos **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="c5374-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="c5374-113">Se você não usou **ASP.NET MVC** antes, recomendamos que você passe **conceitos básicos do ASP.NET MVC 4** laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="c5374-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="c5374-114">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível no site da [versões Web/Microsoft-WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="c5374-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="c5374-115">O projeto específico para este laboratório está disponível em [filtros de ação personalizado do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="c5374-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c5374-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="c5374-116">Objectives</span></span>

<span data-ttu-id="c5374-117">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="c5374-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="c5374-118">Criar um atributo de filtro de ação personalizada para estender os recursos de filtragem</span><span class="sxs-lookup"><span data-stu-id="c5374-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="c5374-119">Aplicar um atributo de filtro personalizado pela injeção de um nível específico</span><span class="sxs-lookup"><span data-stu-id="c5374-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="c5374-120">Registre-se que uma ação personalizada filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="c5374-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c5374-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c5374-121">Prerequisites</span></span>

<span data-ttu-id="c5374-122">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="c5374-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c5374-123">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="c5374-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c5374-124">Configuração</span><span class="sxs-lookup"><span data-stu-id="c5374-124">Setup</span></span>

<span data-ttu-id="c5374-125">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="c5374-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="c5374-126">Para sua conveniência, grande parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5374-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c5374-127">Para instalar os trechos de código executados **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="c5374-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c5374-128">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5374-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c5374-129">Exercícios</span><span class="sxs-lookup"><span data-stu-id="c5374-129">Exercises</span></span>

<span data-ttu-id="c5374-130">Este laboratório prático é composto pelos seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="c5374-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c5374-131">Exercício 1: O log de ações</span><span class="sxs-lookup"><span data-stu-id="c5374-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="c5374-132">Exercício 2: Gerenciar vários filtros de ação</span><span class="sxs-lookup"><span data-stu-id="c5374-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="c5374-133">Tempo estimado para concluir este laboratório: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="c5374-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="c5374-134">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="c5374-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c5374-135">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="c5374-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="c5374-136">Exercício 1: O log de ações</span><span class="sxs-lookup"><span data-stu-id="c5374-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="c5374-137">Neste exercício, você aprenderá como criar um filtro de log de ação personalizada por meio de provedores de filtro ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c5374-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="c5374-138">Para essa finalidade, você aplicará um filtro de log para o site MusicStore que registra todas as atividades em controladores de selecionado.</span><span class="sxs-lookup"><span data-stu-id="c5374-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="c5374-139">O filtro estenderá **ActionFilterAttributeClass** e substituir **OnActionExecuting** método Capture cada solicitação e, em seguida, execute as ações de registro em log.</span><span class="sxs-lookup"><span data-stu-id="c5374-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="c5374-140">As informações de contexto sobre solicitações HTTP, executar métodos, resultados e parâmetros será fornecido pelo ASP.NET MVC **ActionExecutingContext** classe **.**</span><span class="sxs-lookup"><span data-stu-id="c5374-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="c5374-141">Além disso, o ASP.NET MVC 4 tem provedores de filtros de padrão, você pode usar sem criar um filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="c5374-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="c5374-142">ASP.NET MVC 4 fornece os seguintes tipos de filtros:</span><span class="sxs-lookup"><span data-stu-id="c5374-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="c5374-143">**Autorização** filtrar, que toma decisões de segurança sobre a possibilidade de executar um método de ação, como executar a autenticação ou validação de propriedades da solicitação.</span><span class="sxs-lookup"><span data-stu-id="c5374-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="c5374-144">**Ação** filtro, que envolve a execução do método de ação.</span><span class="sxs-lookup"><span data-stu-id="c5374-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="c5374-145">Esse filtro pode executar processamento adicional, como fornecendo dados adicionais para o método de ação, inspecionando o valor de retorno ou cancelar a execução do método da ação</span><span class="sxs-lookup"><span data-stu-id="c5374-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="c5374-146">**Resultado** filtro, que envolve a execução do objeto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="c5374-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="c5374-147">Esse filtro pode executar processamento adicional do resultado, como modificar a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5374-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="c5374-148">**Exceção** filtro, que é executado se houver uma exceção sem tratamento gerada em algum lugar no método de ação, começando com os filtros de autorização e terminando com a execução do resultado.</span><span class="sxs-lookup"><span data-stu-id="c5374-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="c5374-149">Filtros de exceção podem ser usados para tarefas como registro em log ou exibir uma página de erro.</span><span class="sxs-lookup"><span data-stu-id="c5374-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="c5374-150">Para obter mais informações sobre os provedores de filtros, visite este link do MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="c5374-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="c5374-151">Sobre o recurso de log de aplicativo de Store de música do MVC</span><span class="sxs-lookup"><span data-stu-id="c5374-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="c5374-152">Essa solução do Music Store tem uma nova tabela de modelo de dados para o log de site **ActionLog**, com os seguintes campos: nome do controlador que recebeu uma solicitação, a ação de chamado, o IP do cliente e o carimbo de data / hora.</span><span class="sxs-lookup"><span data-stu-id="c5374-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="c5374-153">![Modelo de dados. Tabela ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de dados. Tabela ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="c5374-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="c5374-154">*Modelo de dados – tabela ActionLog*</span><span class="sxs-lookup"><span data-stu-id="c5374-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="c5374-155">A solução oferece um modo de exibição do ASP.NET MVC para o log de ações que pode ser encontrado em **MvcMusicStores/exibições/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="c5374-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="c5374-156">![Modo de exibição de Log de ação](aspnet-mvc-4-custom-action-filters/_static/image2.png "modo de exibição do Log de ações")</span><span class="sxs-lookup"><span data-stu-id="c5374-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="c5374-157">*Modo de exibição de Log de ação*</span><span class="sxs-lookup"><span data-stu-id="c5374-157">*Action Log view*</span></span>

<span data-ttu-id="c5374-158">Com isso considerando a estrutura, todo o trabalho estará concentrado em interromper a solicitação do controlador e executar o registro em log usando a filtragem personalizada.</span><span class="sxs-lookup"><span data-stu-id="c5374-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="c5374-159">Tarefa 1 - criar um filtro personalizado para capturar a solicitação de um controlador</span><span class="sxs-lookup"><span data-stu-id="c5374-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="c5374-160">Nesta tarefa, você criará uma classe de atributo de filtro personalizado que contém a lógica de registro em log.</span><span class="sxs-lookup"><span data-stu-id="c5374-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="c5374-161">Para essa finalidade, você ampliará ASP.NET MVC **ActionFilterAttribute** classe e implementar a interface **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="c5374-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="c5374-162">O **ActionFilterAttribute** é a classe base para todos os filtros de atributo.</span><span class="sxs-lookup"><span data-stu-id="c5374-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="c5374-163">Ele fornece os seguintes métodos para executar uma lógica específica após e antes da execução da ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="c5374-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="c5374-164">**OnActionExecuting**(ActionExecutingContext filterContext): apenas antes da ação de método é chamado.</span><span class="sxs-lookup"><span data-stu-id="c5374-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="c5374-165">**OnActionExecuted**(ActionExecutedContext filterContext): depois que o método de ação é chamado e antes do resultado é executado (antes da renderização do modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="c5374-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="c5374-166">**OnResultExecuting**(ResultExecutingContext filterContext): antes do resultado é executado (antes da renderização do modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="c5374-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="c5374-167">**OnResultExecuted**(ResultExecutedContext filterContext): depois que o resultado é executado (depois que a exibição é renderizada).</span><span class="sxs-lookup"><span data-stu-id="c5374-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="c5374-168">Substituindo qualquer um desses métodos em uma classe derivada, você pode executar seu próprio código de filtragem.</span><span class="sxs-lookup"><span data-stu-id="c5374-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="c5374-169">Abra o **começar** solução localizado em **\Source\Ex01-LoggingActions\Begin** pasta.</span><span class="sxs-lookup"><span data-stu-id="c5374-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="c5374-170">Você precisará baixar alguns pacotes do NuGet ausentes, antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c5374-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="c5374-171">Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.</span><span class="sxs-lookup"><span data-stu-id="c5374-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c5374-172">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="c5374-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c5374-173">Por fim, compile a solução clicando **construir** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="c5374-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c5374-174">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="c5374-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c5374-175">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="c5374-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c5374-176">É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="c5374-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="c5374-177">Para obter mais informações, consulte este artigo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c5374-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c5374-178">Adicionar uma nova classe c# para o **filtros** pasta e nomeie-o *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="c5374-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="c5374-179">Essa pasta armazena todos os filtros personalizados.</span><span class="sxs-lookup"><span data-stu-id="c5374-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="c5374-180">Abra **CustomActionFilter.cs** e adicione uma referência ao **System.Web.Mvc** e **MvcMusicStore.Models** namespaces:</span><span class="sxs-lookup"><span data-stu-id="c5374-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="c5374-181">(Código de trecho de código – *filtros de ação personalizada do ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="c5374-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="c5374-182">Herdar o **CustomActionFilter** classe **ActionFilterAttribute** e, em seguida, faça **CustomActionFilter** classe implemente **IActionFilter** interface.</span><span class="sxs-lookup"><span data-stu-id="c5374-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="c5374-183">Tornar **CustomActionFilter** classe substituir o método **OnActionExecuting** e adicione a lógica necessária para registrar a execução do filtro.</span><span class="sxs-lookup"><span data-stu-id="c5374-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="c5374-184">Para fazer isso, adicione o seguinte código realçado dentro **CustomActionFilter** classe.</span><span class="sxs-lookup"><span data-stu-id="c5374-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="c5374-185">(Código de trecho de código – *filtros de ação personalizada do ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="c5374-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="c5374-186">**OnActionExecuting** usando o método **Entity Framework** para adicionar um novo registro de ActionLog.</span><span class="sxs-lookup"><span data-stu-id="c5374-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="c5374-187">Ele cria e preenche uma nova instância de entidade com as informações de contexto do **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="c5374-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="c5374-188">Você pode ler mais sobre **ControllerContext** classe no [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5374-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="c5374-189">Tarefa 2 - injetando um Interceptor de código para a classe de controlador Store</span><span class="sxs-lookup"><span data-stu-id="c5374-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="c5374-190">Nesta tarefa, você adicionará o filtro personalizado, injetando-lo para todas as classes de controlador e ações do controlador que serão registradas.</span><span class="sxs-lookup"><span data-stu-id="c5374-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="c5374-191">Neste exercício, a classe de controlador Store terá um log.</span><span class="sxs-lookup"><span data-stu-id="c5374-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="c5374-192">O método **OnActionExecuting** partir **ActionLogFilterAttribute** filtro personalizado é executado quando um elemento injetado é chamado.</span><span class="sxs-lookup"><span data-stu-id="c5374-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="c5374-193">Também é possível interceptar um método de controlador específico.</span><span class="sxs-lookup"><span data-stu-id="c5374-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="c5374-194">Abra o **StoreController** no **MvcMusicStore\Controllers** e adicione uma referência para o **filtros** namespace:</span><span class="sxs-lookup"><span data-stu-id="c5374-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="c5374-195">Injetar o filtro personalizado **CustomActionFilter** em **StoreController** classe adicionando **[CustomActionFilter]** atributo antes da declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="c5374-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="c5374-196">Quando um filtro é injetado em uma classe de controlador, todas as suas ações também são injetadas.</span><span class="sxs-lookup"><span data-stu-id="c5374-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="c5374-197">Se você quiser aplicar o filtro apenas para um conjunto de ações, você teria que injetar **[CustomActionFilter]** para cada um deles:</span><span class="sxs-lookup"><span data-stu-id="c5374-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="c5374-198">Tarefa 3: executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c5374-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="c5374-199">Nesta tarefa, você irá testar se o filtro de log está funcionando.</span><span class="sxs-lookup"><span data-stu-id="c5374-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="c5374-200">Inicie o aplicativo e visitar a loja e, em seguida, você verificará se há atividades registradas.</span><span class="sxs-lookup"><span data-stu-id="c5374-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="c5374-201">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c5374-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c5374-202">Navegue até **/ActionLog** para ver o estado inicial do modo de exibição de log:</span><span class="sxs-lookup"><span data-stu-id="c5374-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="c5374-203">![Status do controlador antes da atividade de página do log](aspnet-mvc-4-custom-action-filters/_static/image3.png "registrar o status do controlador antes da atividade de página")</span><span class="sxs-lookup"><span data-stu-id="c5374-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c5374-204">*Status do rastreador de log antes da atividade de página*</span><span class="sxs-lookup"><span data-stu-id="c5374-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="c5374-205">Por padrão, ele sempre mostra um item que é gerado ao recuperar os gêneros existentes para o menu.</span><span class="sxs-lookup"><span data-stu-id="c5374-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="c5374-206">Para fins de simplicidade estamos excluindo os **ActionLog** sempre que o aplicativo é executado, portanto, ele mostrará apenas os logs de verificação da cada tarefa específica tabela.</span><span class="sxs-lookup"><span data-stu-id="c5374-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="c5374-207">Talvez você precise remover o código a seguir da **sessão\_começar** método (no **global. asax** classe), para salvar um log de histórico para todas as ações executadas dentro do Store Controlador.</span><span class="sxs-lookup"><span data-stu-id="c5374-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="c5374-208">Clique em um dos **gêneros** no menu e executar algumas ações, como navegar de um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="c5374-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="c5374-209">Navegue até **/ActionLog** e se o log estiver vazia press **F5** para atualizar a página.</span><span class="sxs-lookup"><span data-stu-id="c5374-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="c5374-210">Verifique que as suas visitas foram controladas:</span><span class="sxs-lookup"><span data-stu-id="c5374-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="c5374-211">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image4.png "log de ações com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="c5374-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c5374-212">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="c5374-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="c5374-213">Exercício 2: Gerenciar vários filtros de ação</span><span class="sxs-lookup"><span data-stu-id="c5374-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="c5374-214">Neste exercício você adicionar um segundo filtro de ação personalizada para a classe StoreController e definir a ordem específica em que ambos os filtros serão executados.</span><span class="sxs-lookup"><span data-stu-id="c5374-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="c5374-215">Em seguida, você irá atualizar o código para registrar o filtro globalmente.</span><span class="sxs-lookup"><span data-stu-id="c5374-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="c5374-216">Há diferentes opções para levar em conta ao definir a ordem de execução de filtros.</span><span class="sxs-lookup"><span data-stu-id="c5374-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="c5374-217">Por exemplo, a propriedade Order e escopo de filtros:</span><span class="sxs-lookup"><span data-stu-id="c5374-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="c5374-218">Você pode definir um **escopo** para cada um dos filtros, por exemplo, você pode definir o escopo de todos os filtros de ação para ser executado nos **escopo do controlador**e todos os filtros de autorização para executar no **escopo Global** .</span><span class="sxs-lookup"><span data-stu-id="c5374-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="c5374-219">Os escopos têm uma ordem de execução definido.</span><span class="sxs-lookup"><span data-stu-id="c5374-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="c5374-220">Além disso, cada filtro de ação tem uma propriedade de pedido, que é usada para determinar a ordem de execução no escopo do filtro.</span><span class="sxs-lookup"><span data-stu-id="c5374-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="c5374-221">Para obter mais informações sobre a ordem de execução de filtros de ação personalizada, visite este artigo do MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="c5374-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="c5374-222">Tarefa 1: Criando um novo filtro de ação personalizada</span><span class="sxs-lookup"><span data-stu-id="c5374-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="c5374-223">Nesta tarefa, você criará um novo filtro de ação personalizada para injetar a classe StoreController, aprender como gerenciar a ordem de execução dos filtros.</span><span class="sxs-lookup"><span data-stu-id="c5374-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="c5374-224">Abra o **começar** solução localizado em **\Source\Ex02-ManagingMultipleActionFilters\Begin** pasta.</span><span class="sxs-lookup"><span data-stu-id="c5374-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="c5374-225">Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="c5374-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c5374-226">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c5374-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c5374-227">Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.</span><span class="sxs-lookup"><span data-stu-id="c5374-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c5374-228">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="c5374-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c5374-229">Por fim, compile a solução clicando **construir** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="c5374-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c5374-230">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="c5374-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c5374-231">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="c5374-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c5374-232">É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="c5374-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="c5374-233">Para obter mais informações, consulte este artigo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c5374-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c5374-234">Adicionar uma nova classe c# para o **filtros** pasta e nomeie-o *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="c5374-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="c5374-235">Abra **MyNewCustomActionFilter.cs** e adicione uma referência ao **System.Web.Mvc** e o **MvcMusicStore.Models** namespace:</span><span class="sxs-lookup"><span data-stu-id="c5374-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="c5374-236">(Código de trecho de código – *filtros de ação personalizada do ASP.NET MVC 4 - o Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="c5374-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="c5374-237">Substitua a declaração de classe padrão com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c5374-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="c5374-238">(Código de trecho de código – *filtros de ação personalizada do ASP.NET MVC 4 - o Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="c5374-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="c5374-239">Esse filtro de ação personalizado é quase o mesmo que aquele que você criou no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="c5374-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="c5374-240">A principal diferença é que ela tem o *&quot;registradas pelo&quot;* atributo atualizado com de nome essa nova classe para identificar o filtro registrado no log.</span><span class="sxs-lookup"><span data-stu-id="c5374-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="c5374-241">Tarefa 2: Injetando um novo interceptador de código para a classe StoreController</span><span class="sxs-lookup"><span data-stu-id="c5374-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="c5374-242">Nesta tarefa, você irá adicionar um novo filtro personalizado na classe StoreController e executar a solução para verificar como os dois filtros funcionam em conjunto.</span><span class="sxs-lookup"><span data-stu-id="c5374-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="c5374-243">Abra o **StoreController** classe localizado em **MvcMusicStore\Controllers** e inserir o novo filtro personalizado **MyNewCustomActionFilter** em  **StoreController** classe, como é mostrado no código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c5374-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="c5374-244">Agora, execute o aplicativo para ver como esses dois filtros de ação personalizado funcionam.</span><span class="sxs-lookup"><span data-stu-id="c5374-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="c5374-245">Para fazer isso, pressione **F5** e aguarde até que o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="c5374-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="c5374-246">Navegue até **/ActionLog** para ver o estado inicial do modo de exibição de log.</span><span class="sxs-lookup"><span data-stu-id="c5374-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="c5374-247">![Status do controlador antes da atividade de página do log](aspnet-mvc-4-custom-action-filters/_static/image5.png "registrar o status do controlador antes da atividade de página")</span><span class="sxs-lookup"><span data-stu-id="c5374-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c5374-248">*Status do rastreador de log antes da atividade de página*</span><span class="sxs-lookup"><span data-stu-id="c5374-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="c5374-249">Clique em um dos **gêneros** no menu e executar algumas ações, como navegar de um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="c5374-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="c5374-250">Verifique se neste momento. suas visitas foram acompanhadas duas vezes: depois de cada um dos filtros de ação personalizado que você adicionou na **StorageController** classe.</span><span class="sxs-lookup"><span data-stu-id="c5374-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="c5374-251">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image6.png "log de ações com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="c5374-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c5374-252">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="c5374-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="c5374-253">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="c5374-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="c5374-254">Tarefa 3: Gerenciar a ordenação de filtro</span><span class="sxs-lookup"><span data-stu-id="c5374-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="c5374-255">Nesta tarefa, você aprenderá como gerenciar a ordem de execução de filtros usando a propriedade de ordem.</span><span class="sxs-lookup"><span data-stu-id="c5374-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="c5374-256">Abra o **StoreController** classe localizado em **MvcMusicStore\Controllers** e especifique a **ordem** como de propriedade em ambos os filtros conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c5374-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="c5374-257">Agora, verifique como os filtros são executados, dependendo do valor da propriedade do seu pedido.</span><span class="sxs-lookup"><span data-stu-id="c5374-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="c5374-258">Você encontrará a que o filtro com o menor valor de ordem (**CustomActionFilter**) é o primeiro que é executado.</span><span class="sxs-lookup"><span data-stu-id="c5374-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="c5374-259">Pressione **F5** e aguarde até que o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="c5374-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="c5374-260">Navegue até **/ActionLog** para ver o estado inicial do modo de exibição de log.</span><span class="sxs-lookup"><span data-stu-id="c5374-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="c5374-261">![Status do controlador antes da atividade de página do log](aspnet-mvc-4-custom-action-filters/_static/image7.png "registrar o status do controlador antes da atividade de página")</span><span class="sxs-lookup"><span data-stu-id="c5374-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c5374-262">*Status do rastreador de log antes da atividade de página*</span><span class="sxs-lookup"><span data-stu-id="c5374-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="c5374-263">Clique em um dos **gêneros** no menu e executar algumas ações, como navegar de um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="c5374-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="c5374-264">Verificação de que esse tempo, suas visitas foram acompanhadas ordenados pelo valor de ordem de filtros: **CustomActionFilter** logs' primeiro.</span><span class="sxs-lookup"><span data-stu-id="c5374-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="c5374-265">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image8.png "log de ações com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="c5374-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c5374-266">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="c5374-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="c5374-267">Agora, você irá atualizar o valor de ordem de filtros e verificar como a ordem de registro em log é alterado.</span><span class="sxs-lookup"><span data-stu-id="c5374-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="c5374-268">No **StoreController** de classe, atualize o valor de ordem de filtros, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c5374-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="c5374-269">Execute o aplicativo novamente pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="c5374-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="c5374-270">Clique em um dos **gêneros** no menu e executar algumas ações, como navegar de um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="c5374-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="c5374-271">Verifique se esse tempo, os logs criado pelo **MyNewCustomActionFilter** filtro aparece primeiro.</span><span class="sxs-lookup"><span data-stu-id="c5374-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="c5374-272">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image9.png "log de ações com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="c5374-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c5374-273">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="c5374-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="c5374-274">Tarefa 4: Registrar filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="c5374-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="c5374-275">Nesta tarefa, você atualizará a solução para registrar o novo filtro (**MyNewCustomActionFilter**) como um filtro global.</span><span class="sxs-lookup"><span data-stu-id="c5374-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="c5374-276">Ao fazer isso, ele será disparado por todos os executadas as ações no aplicativo e não apenas no StoreController aqueles da tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="c5374-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="c5374-277">Na **StoreController** classe, remova **[MyNewCustomActionFilter]** atributo e a propriedade de ordem de **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="c5374-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="c5374-278">Ele deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="c5374-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="c5374-279">Abra **global. asax** do arquivo e localize o **aplicativo\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="c5374-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="c5374-280">Observe que cada vez que o aplicativo é iniciado ele está registrando os filtros globais chamando **RegisterGlobalFilters** método dentro **FilterConfig** classe.</span><span class="sxs-lookup"><span data-stu-id="c5374-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="c5374-281">![Registrando os filtros globais no global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrando os filtros globais no global. asax")</span><span class="sxs-lookup"><span data-stu-id="c5374-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="c5374-282">*Registrando os filtros globais no global. asax*</span><span class="sxs-lookup"><span data-stu-id="c5374-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="c5374-283">Abra **FilterConfig.cs** dentro do arquivo **App\_iniciar** pasta.</span><span class="sxs-lookup"><span data-stu-id="c5374-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="c5374-284">Adicionar uma referência ao usando System.Web.Mvc; usando MvcMusicStore.Filters; namespace.</span><span class="sxs-lookup"><span data-stu-id="c5374-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="c5374-285">Atualização **RegisterGlobalFilters** método adicionando seu filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="c5374-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="c5374-286">Para fazer isso, adicione o código realçado:</span><span class="sxs-lookup"><span data-stu-id="c5374-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="c5374-287">Execute o aplicativo pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="c5374-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="c5374-288">Clique em um dos **gêneros** no menu e executar algumas ações, como navegar de um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="c5374-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="c5374-289">Verificação de que agora **[MyNewCustomActionFilter]** está sendo injetados no HomeController e ActionLogController muito.</span><span class="sxs-lookup"><span data-stu-id="c5374-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="c5374-290">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image11.png "log de ações com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="c5374-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c5374-291">*Log de ação com a atividade global conectada*</span><span class="sxs-lookup"><span data-stu-id="c5374-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="c5374-292">Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="c5374-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c5374-293">Resumo</span><span class="sxs-lookup"><span data-stu-id="c5374-293">Summary</span></span>

<span data-ttu-id="c5374-294">Ao concluir este laboratório prático, você aprendeu a estender um filtro de ação para executar ações personalizadas.</span><span class="sxs-lookup"><span data-stu-id="c5374-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="c5374-295">Você também aprendeu como injetar qualquer filtro aos controladores de página.</span><span class="sxs-lookup"><span data-stu-id="c5374-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="c5374-296">Foram usados os seguintes conceitos:</span><span class="sxs-lookup"><span data-stu-id="c5374-296">The following concepts were used:</span></span>

- <span data-ttu-id="c5374-297">Como criar filtros de ação personalizada com a classe de ActionFilterAttribute do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c5374-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="c5374-298">Como injetar filtros nos controladores do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c5374-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="c5374-299">Como gerenciar a ordenação de filtro usando a propriedade de ordem</span><span class="sxs-lookup"><span data-stu-id="c5374-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="c5374-300">Como registrar filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="c5374-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c5374-301">Apêndice a: instalação do Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="c5374-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c5374-302">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="c5374-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c5374-303">As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="c5374-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c5374-304">Vá para [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c5374-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c5374-305">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c5374-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c5374-306">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="c5374-306">Click on **Install Now**.</span></span> <span data-ttu-id="c5374-307">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="c5374-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c5374-308">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="c5374-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c5374-309">![Instalar o Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalar o Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c5374-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c5374-310">*Instalar o Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c5374-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c5374-311">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="c5374-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="c5374-313">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="c5374-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c5374-314">Aguarde até que o processo de download e instalação for concluído.</span><span class="sxs-lookup"><span data-stu-id="c5374-314">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="c5374-316">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="c5374-316">*Installation progress*</span></span>
6. <span data-ttu-id="c5374-317">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="c5374-317">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="c5374-319">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="c5374-319">*Installation completed*</span></span>
7. <span data-ttu-id="c5374-320">Clique em **Exit** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="c5374-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c5374-321">Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="c5374-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco da Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="c5374-323">*VS Express para o bloco da Web*</span><span class="sxs-lookup"><span data-stu-id="c5374-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c5374-324">Apêndice b: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="c5374-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c5374-325">Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o ambiente de laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c5374-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="c5374-326">Tarefa 1 - criar um novo Site do Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c5374-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="c5374-327">Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="c5374-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5374-328">Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego aumenta.</span><span class="sxs-lookup"><span data-stu-id="c5374-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c5374-329">Você pode se inscrever [aqui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c5374-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c5374-330">![Faça logon no portal do Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c5374-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c5374-331">*Faça logon no Portal de gerenciamento do Azure do Windows*</span><span class="sxs-lookup"><span data-stu-id="c5374-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="c5374-332">Clique em **New** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c5374-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c5374-333">![Criando um novo Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "criando um novo Site")</span><span class="sxs-lookup"><span data-stu-id="c5374-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c5374-334">*Criando um novo Site*</span><span class="sxs-lookup"><span data-stu-id="c5374-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c5374-335">Clique em **Compute** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="c5374-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c5374-336">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="c5374-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="c5374-337">Forneça uma URL disponível para o novo site da web e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="c5374-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5374-338">Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="c5374-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c5374-339">A opção criação rápida permite que você implantar um aplicativo web completo para o Windows Azure Site de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="c5374-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="c5374-340">Ele não inclui as etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c5374-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c5374-341">![Criando um novo Site usando a criação rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "criando um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="c5374-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c5374-342">*Criando um novo Site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="c5374-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c5374-343">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="c5374-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c5374-344">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="c5374-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c5374-345">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="c5374-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c5374-346">![Navegando até o novo site](aspnet-mvc-4-custom-action-filters/_static/image20.png "navegando até o novo site da web")</span><span class="sxs-lookup"><span data-stu-id="c5374-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c5374-347">*Navegando até o novo site da web*</span><span class="sxs-lookup"><span data-stu-id="c5374-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c5374-348">![Site da Web em execução](aspnet-mvc-4-custom-action-filters/_static/image21.png "site da Web em execução")</span><span class="sxs-lookup"><span data-stu-id="c5374-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="c5374-349">*Site da Web em execução*</span><span class="sxs-lookup"><span data-stu-id="c5374-349">*Web site running*</span></span>
6. <span data-ttu-id="c5374-350">Volte para o portal e clique no nome do site da web sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="c5374-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c5374-351">![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-custom-action-filters/_static/image22.png "abrir as páginas de gerenciamento do site da web")</span><span class="sxs-lookup"><span data-stu-id="c5374-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c5374-352">*Abrir as páginas de gerenciamento do Site da Web*</span><span class="sxs-lookup"><span data-stu-id="c5374-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c5374-353">No **Dashboard** página na **visão geral rápida** seção, clique no **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="c5374-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5374-354">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="c5374-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="c5374-355">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para conectar e autenticar em relação a cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="c5374-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c5374-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web nos sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c5374-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="c5374-357">![Baixando o site da web de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image23.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="c5374-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c5374-358">*Baixando o Site da Web de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="c5374-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c5374-359">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="c5374-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c5374-360">Ainda mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5374-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="c5374-361">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image24.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="c5374-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c5374-362">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="c5374-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c5374-363">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="c5374-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c5374-364">Se seu aplicativo faz uso do SQL Server você precisará criar um servidor de banco de dados SQL de bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="c5374-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c5374-365">Se você quiser implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="c5374-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c5374-366">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c5374-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c5374-367">Você pode exibir os servidores de banco de dados SQL na sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**.</span><span class="sxs-lookup"><span data-stu-id="c5374-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c5374-368">Se você não tiver um servidor criado, você pode criar uma usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c5374-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c5374-369">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="c5374-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c5374-370">Não crie o banco de dados ainda, ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="c5374-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c5374-371">![Painel do servidor de banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="c5374-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c5374-372">*Painel de banco de dados do SQL Server*</span><span class="sxs-lookup"><span data-stu-id="c5374-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c5374-373">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="c5374-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c5374-374">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço de IP do cliente atual** e cole-o na **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique em de ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) botão.</span><span class="sxs-lookup"><span data-stu-id="c5374-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="c5374-376">*Adicionar endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="c5374-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c5374-377">Uma vez a **endereço IP do cliente** é adicionado para endereços IP, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="c5374-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="c5374-379">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="c5374-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c5374-380">Tarefa 3 – publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="c5374-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c5374-381">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c5374-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c5374-382">No **Gerenciador de soluções**, clique com botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c5374-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c5374-383">![O aplicativo de publicação](aspnet-mvc-4-custom-action-filters/_static/image29.png "publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="c5374-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="c5374-384">*Publicar o site da web*</span><span class="sxs-lookup"><span data-stu-id="c5374-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="c5374-385">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="c5374-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c5374-386">![Importando o perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image30.png "importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="c5374-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c5374-387">*Importando o perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="c5374-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="c5374-388">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="c5374-388">Click **Validate Connection**.</span></span> <span data-ttu-id="c5374-389">Depois que a validação estiver concluída, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="c5374-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5374-390">A validação é concluída quando você vir uma marca de seleção verde aparecer ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="c5374-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c5374-391">![Validar conexão](aspnet-mvc-4-custom-action-filters/_static/image31.png "validar conexão")</span><span class="sxs-lookup"><span data-stu-id="c5374-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="c5374-392">*Validando a conexão*</span><span class="sxs-lookup"><span data-stu-id="c5374-392">*Validating connection*</span></span>
4. <span data-ttu-id="c5374-393">No **as configurações** página na **bancos de dados** seção, clique no botão ao lado da caixa de texto do sua conexão banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c5374-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c5374-394">![Configuração de implantação da Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="c5374-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c5374-395">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="c5374-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c5374-396">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c5374-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="c5374-397">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="c5374-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="c5374-398">Na **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="c5374-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="c5374-399">Na **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="c5374-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="c5374-400">Digite um novo nome de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c5374-400">Type a new database name.</span></span>

     <span data-ttu-id="c5374-401">![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="c5374-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="c5374-402">*Configurando a cadeia de caracteres de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="c5374-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c5374-403">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5374-403">Then click **OK**.</span></span> <span data-ttu-id="c5374-404">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="c5374-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c5374-405">![Criando o banco de dados](aspnet-mvc-4-custom-action-filters/_static/image34.png "criando a cadeia de caracteres de banco de dados")</span><span class="sxs-lookup"><span data-stu-id="c5374-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="c5374-406">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="c5374-406">*Creating the database*</span></span>
7. <span data-ttu-id="c5374-407">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="c5374-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c5374-408">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="c5374-408">Then click **Next**.</span></span>

    <span data-ttu-id="c5374-409">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="c5374-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c5374-410">*Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="c5374-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c5374-411">No **versão prévia** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c5374-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c5374-412">![Publicando o aplicativo web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publicando o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="c5374-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="c5374-413">*Publicar o aplicativo da web*</span><span class="sxs-lookup"><span data-stu-id="c5374-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="c5374-414">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="c5374-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="c5374-415">Apêndice c: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="c5374-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="c5374-416">Com trechos de código, você tem todo o código que necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="c5374-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c5374-417">O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="c5374-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c5374-418">![Usar trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-custom-action-filters/_static/image37.png "trechos de código usando o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="c5374-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c5374-419">*Usar trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="c5374-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c5374-420">***Para adicionar um trecho de código usando o teclado (somente c#)***</span><span class="sxs-lookup"><span data-stu-id="c5374-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c5374-421">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="c5374-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c5374-422">Comece a digitar o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="c5374-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c5374-423">Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="c5374-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c5374-424">Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).</span><span class="sxs-lookup"><span data-stu-id="c5374-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c5374-425">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="c5374-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c5374-426">![Comece a digitar o nome do trecho](aspnet-mvc-4-custom-action-filters/_static/image38.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="c5374-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c5374-427">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="c5374-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="c5374-428">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-custom-action-filters/_static/image39.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="c5374-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c5374-429">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="c5374-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c5374-430">![Pressione Tab novamente e o trecho de código serão expandido](aspnet-mvc-4-custom-action-filters/_static/image40.png "pressione Tab novamente e o trecho de código serão expandido")</span><span class="sxs-lookup"><span data-stu-id="c5374-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c5374-431">*Pressione Tab novamente e o trecho de código serão expandido*</span><span class="sxs-lookup"><span data-stu-id="c5374-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c5374-432">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c5374-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c5374-433">Clique com botão direito no qual você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="c5374-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c5374-434">Selecione **Inserir trecho** seguido **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="c5374-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c5374-435">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="c5374-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c5374-436">![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")</span><span class="sxs-lookup"><span data-stu-id="c5374-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c5374-437">*Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet*</span><span class="sxs-lookup"><span data-stu-id="c5374-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c5374-438">![Escolher o trecho relevante na lista, clicando nele](aspnet-mvc-4-custom-action-filters/_static/image42.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="c5374-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c5374-439">*Escolher o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="c5374-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
