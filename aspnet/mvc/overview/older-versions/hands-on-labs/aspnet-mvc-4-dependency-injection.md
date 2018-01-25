---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Injeção de dependência do ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Observação: Este laboratório prático supõe que você tenha um conhecimento básico de filtros ASP.NET MVC e ASP.NET MVC 4. Se você não usou filtros ASP.NET MVC 4 antes, nós rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="41a61-104">Injeção de dependência do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="41a61-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="41a61-105">por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="41a61-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="41a61-106">Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC** e **filtros ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="41a61-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="41a61-107">Se você não usou **filtros ASP.NET MVC 4** antes, é recomendável que você passe **filtros de ação do ASP.NET MVC personalizado** laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="41a61-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="41a61-108">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="41a61-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<span data-ttu-id="41a61-109">Em **programação orientada a objeto** paradigma, objetos trabalham em conjunto em um modelo de colaboração em que há colaboradores e consumidores.</span><span class="sxs-lookup"><span data-stu-id="41a61-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="41a61-110">Naturalmente, esse modelo de comunicação gera as dependências entre objetos e componentes, tornando-se difíceis de gerenciar ao aumenta a complexidade.</span><span class="sxs-lookup"><span data-stu-id="41a61-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="41a61-111">![Dependências de classe e a complexidade do modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "dependências de classe e a complexidade do modelo")</span><span class="sxs-lookup"><span data-stu-id="41a61-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="41a61-112">*Dependências de classe e a complexidade do modelo*</span><span class="sxs-lookup"><span data-stu-id="41a61-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="41a61-113">Você já deve ter ouvido sobre o **padrão de fábrica** e a separação entre a interface e a implementação usando os serviços, onde os objetos de cliente geralmente são responsáveis por local do serviço.</span><span class="sxs-lookup"><span data-stu-id="41a61-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="41a61-114">O padrão de injeção de dependência é uma implementação específica de inversão de controle.</span><span class="sxs-lookup"><span data-stu-id="41a61-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="41a61-115">**Inversão de controle (IoC)** significa que os objetos não crie outros objetos que dependem para fazer seu trabalho.</span><span class="sxs-lookup"><span data-stu-id="41a61-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="41a61-116">Em vez disso, eles obtêm os objetos que precisam de uma fonte externa (por exemplo, um arquivo de configuração xml).</span><span class="sxs-lookup"><span data-stu-id="41a61-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="41a61-117">**Injeção de dependência (DI)** significa que isso é feito sem a intervenção de objeto, normalmente por um componente de estrutura que passa parâmetros do construtor e definir propriedades.</span><span class="sxs-lookup"><span data-stu-id="41a61-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="41a61-118">O padrão de Design de (DI) de injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="41a61-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="41a61-119">Em um nível alto, o objetivo de injeção de dependência é que uma classe de cliente (por exemplo, *o jogador*) precisa de algo que atenda a uma interface (por exemplo, *IClub*).</span><span class="sxs-lookup"><span data-stu-id="41a61-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="41a61-120">Ele não se importa o que é o tipo concreto (por exemplo, *WoodClub, IronClub, WedgeClub* ou *PutterClub*), que quiser que outra pessoa para lidar com isso (por exemplo, um bom *caddy*).</span><span class="sxs-lookup"><span data-stu-id="41a61-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="41a61-121">O resolvedor de dependência no ASP.NET MVC podem permitir que você registrar sua lógica de dependência em outro lugar (por exemplo, um contêiner ou um *recipiente de paus*).</span><span class="sxs-lookup"><span data-stu-id="41a61-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="41a61-122">![Diagrama de injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustração de injeção de dependência")</span><span class="sxs-lookup"><span data-stu-id="41a61-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="41a61-123">*Injeção de dependência - analogia Golfe*</span><span class="sxs-lookup"><span data-stu-id="41a61-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="41a61-124">As vantagens de usar o padrão de injeção de dependência e inversão de controle são os seguintes:</span><span class="sxs-lookup"><span data-stu-id="41a61-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="41a61-125">Reduz o acoplamento de classes</span><span class="sxs-lookup"><span data-stu-id="41a61-125">Reduces class coupling</span></span>
- <span data-ttu-id="41a61-126">Aumenta a reutilização de código</span><span class="sxs-lookup"><span data-stu-id="41a61-126">Increases code reusing</span></span>
- <span data-ttu-id="41a61-127">Melhora a capacidade de manutenção de código</span><span class="sxs-lookup"><span data-stu-id="41a61-127">Improves code maintainability</span></span>
- <span data-ttu-id="41a61-128">Melhora o teste de aplicativos</span><span class="sxs-lookup"><span data-stu-id="41a61-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="41a61-129">Injeção de dependência, às vezes, é comparada com o padrão de Design de fábrica abstrata, mas há uma pequena diferença entre as duas abordagens.</span><span class="sxs-lookup"><span data-stu-id="41a61-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="41a61-130">Injeção de dependência tem uma estrutura de trabalhar por trás de resolver dependências chamando as fábricas e os serviços registrados.</span><span class="sxs-lookup"><span data-stu-id="41a61-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="41a61-131">Agora que você entende o padrão de injeção de dependência, você aprenderá durante este laboratório para aplicá-la no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="41a61-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="41a61-132">Você irá iniciar usando a injeção de dependência no **controladores** para incluir um serviço de acesso de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="41a61-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="41a61-133">Em seguida, você aplicará a injeção de dependência de **exibições** consumir um serviço e exiba informações.</span><span class="sxs-lookup"><span data-stu-id="41a61-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="41a61-134">Por fim, você estenderá a injeção de dependência para ASP.NET MVC 4 filtros, injeção de um filtro de ação personalizada na solução.</span><span class="sxs-lookup"><span data-stu-id="41a61-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="41a61-135">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="41a61-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="41a61-136">Integrar o ASP.NET MVC 4 com Unity para injeção de dependência usando pacotes do NuGet</span><span class="sxs-lookup"><span data-stu-id="41a61-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="41a61-137">Use a injeção de dependência dentro de um controlador MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="41a61-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="41a61-138">Use a injeção de dependência dentro de uma exibição ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="41a61-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="41a61-139">Use a injeção de dependência dentro de um filtro de ação do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="41a61-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="41a61-140">Este laboratório está usando o pacote do NuGet Unity.Mvc3 para resolução de dependência, mas é possível adaptar qualquer estrutura de injeção de dependência para trabalhar com o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="41a61-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="41a61-141">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="41a61-141">Prerequisites</span></span>

<span data-ttu-id="41a61-142">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="41a61-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="41a61-143">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="41a61-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="41a61-144">Configuração</span><span class="sxs-lookup"><span data-stu-id="41a61-144">Setup</span></span>

<span data-ttu-id="41a61-145">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="41a61-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="41a61-146">Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="41a61-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="41a61-147">Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="41a61-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="41a61-148">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice b:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="41a61-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="41a61-149">Exercícios</span><span class="sxs-lookup"><span data-stu-id="41a61-149">Exercises</span></span>

<span data-ttu-id="41a61-150">Este laboratório prático é composto pelos exercícios a seguir:</span><span class="sxs-lookup"><span data-stu-id="41a61-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="41a61-151">Exercício 1: Insira um controlador</span><span class="sxs-lookup"><span data-stu-id="41a61-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="41a61-152">Exercício 2: Insira um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="41a61-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="41a61-153">Exercício 3: Injeção de filtros</span><span class="sxs-lookup"><span data-stu-id="41a61-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="41a61-154">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="41a61-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="41a61-155">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="41a61-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="41a61-156">Tempo estimado para concluir este laboratório: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="41a61-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="41a61-157">Exercício 1: Insira um controlador</span><span class="sxs-lookup"><span data-stu-id="41a61-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="41a61-158">Neste exercício, você aprenderá como usar injeção de dependência em controladores do ASP.NET MVC, integrando Unity usando um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="41a61-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="41a61-159">Por esse motivo, você incluirá os serviços em seus controladores MvcMusicStore para separar a lógica de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="41a61-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="41a61-160">Os serviços criará uma nova dependência no construtor de controlador, que será resolvido usando a injeção de dependência com a Ajuda de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="41a61-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="41a61-161">Essa abordagem mostrará como gerar menos aplicativos acoplados, que são mais flexíveis e fáceis de manter e testar.</span><span class="sxs-lookup"><span data-stu-id="41a61-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="41a61-162">Você também aprenderá como integrar o ASP.NET MVC Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="41a61-163">Sobre o serviço de StoreManager</span><span class="sxs-lookup"><span data-stu-id="41a61-163">About StoreManager Service</span></span>

<span data-ttu-id="41a61-164">O repositório de música MVC fornecida na solução começar agora inclui um serviço que gerencia os dados do controlador de armazenamento denominados **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="41a61-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="41a61-165">Abaixo você encontrará a implementação do serviço de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="41a61-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="41a61-166">Observe que todos os métodos retornam entidades de modelo.</span><span class="sxs-lookup"><span data-stu-id="41a61-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="41a61-167">**StoreController** do begin solução agora consome **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="41a61-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="41a61-168">Todas as referências de dados foram removidas do **StoreController**e agora é possível modificar o provedor de acesso de dados atual sem alterar qualquer método que consome **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="41a61-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="41a61-169">Abaixo, você encontrará o **StoreController** implementação tem uma dependência com **StoreService** dentro do construtor de classe.</span><span class="sxs-lookup"><span data-stu-id="41a61-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="41a61-170">A dependência introduzida neste exercício está relacionada à **inversão de controle** (IoC).</span><span class="sxs-lookup"><span data-stu-id="41a61-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="41a61-171">O **StoreController** construtor da classe recebe um **IStoreService** parâmetro de tipo, que é essencial para realizar chamadas de serviço de dentro da classe.</span><span class="sxs-lookup"><span data-stu-id="41a61-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="41a61-172">No entanto, **StoreController** não implementa o construtor padrão (sem nenhum parâmetro) que qualquer controlador deve ter para funcionar com o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="41a61-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="41a61-173">Para resolver a dependência, o controlador deve ser criado por uma fábrica abstrata (uma classe que retorna qualquer objeto do tipo especificado).</span><span class="sxs-lookup"><span data-stu-id="41a61-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="41a61-174">Você obterá um erro quando a classe tenta criar a StoreController sem enviar o objeto de serviço, pois não há nenhum construtor sem parâmetros declarado.</span><span class="sxs-lookup"><span data-stu-id="41a61-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="41a61-175">Tarefa 1: executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="41a61-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="41a61-176">Nesta tarefa, você executará o aplicativo de início, que inclui o serviço ao controlador de armazenamento que separa o acesso a dados da lógica do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41a61-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="41a61-177">Ao executar o aplicativo, você receberá uma exceção, como o serviço de controlador não for passado como um parâmetro por padrão:</span><span class="sxs-lookup"><span data-stu-id="41a61-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="41a61-178">Abra o **começar** solução localizada no **Controller\Begin injetando Source\Ex01**.</span><span class="sxs-lookup"><span data-stu-id="41a61-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="41a61-179">Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="41a61-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="41a61-180">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="41a61-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="41a61-181">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="41a61-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="41a61-182">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="41a61-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41a61-183">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="41a61-184">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="41a61-185">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="41a61-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="41a61-186">Pressione **Ctrl + F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="41a61-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="41a61-187">Você receberá a mensagem de erro &quot; **nenhum construtor sem parâmetros definido para este objeto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="41a61-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="41a61-188">![Erro durante a execução do aplicativo do ASP.NET MVC começar](aspnet-mvc-4-dependency-injection/_static/image3.png "erro durante a execução do aplicativo do ASP.NET MVC começar")</span><span class="sxs-lookup"><span data-stu-id="41a61-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="41a61-189">*Erro durante a execução do aplicativo do ASP.NET MVC começar*</span><span class="sxs-lookup"><span data-stu-id="41a61-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="41a61-190">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="41a61-190">Close the browser.</span></span>

<span data-ttu-id="41a61-191">As etapas a seguir, você trabalhará na solução de armazenamento de música injetar a dependência que este controlador precisa.</span><span class="sxs-lookup"><span data-stu-id="41a61-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="41a61-192">Tarefa 2 - incluindo Unity na solução MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="41a61-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="41a61-193">Nesta tarefa, você incluirá **Unity.Mvc3** pacote NuGet para a solução.</span><span class="sxs-lookup"><span data-stu-id="41a61-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="41a61-194">Unity.Mvc3 pacote foi projetado para ASP.NET MVC 3, mas ele é totalmente compatível com o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="41a61-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="41a61-195">Unity é um contêiner de injeção de dependência leve e extensível com suporte opcional para a instância e digite a interceptação.</span><span class="sxs-lookup"><span data-stu-id="41a61-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="41a61-196">É um contêiner de finalidade geral para uso em qualquer tipo de aplicativo .NET.</span><span class="sxs-lookup"><span data-stu-id="41a61-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="41a61-197">Ele fornece todos os recursos comuns encontrados nos mecanismos de injeção de dependência incluindo: criação de objeto, a abstração de requisitos com a especificação de dependências em tempo de execução e a flexibilidade, adiando a configuração do componente para o contêiner.</span><span class="sxs-lookup"><span data-stu-id="41a61-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="41a61-198">Instalar **Unity.Mvc3** pacote NuGet no **MvcMusicStore** projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="41a61-199">Para fazer isso, abra o **Package Manager Console** de **exibição** | **outras janelas**.</span><span class="sxs-lookup"><span data-stu-id="41a61-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="41a61-200">Execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="41a61-200">Run the following command.</span></span>

    <span data-ttu-id="41a61-201">PMC</span><span class="sxs-lookup"><span data-stu-id="41a61-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="41a61-202">![Instalando o pacote do NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instalando o pacote do NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="41a61-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="41a61-203">*Instalando o pacote do NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="41a61-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="41a61-204">Uma vez o **Unity.Mvc3** pacote está instalado, explore os arquivos e pastas que adiciona automaticamente para simplificar a configuração de unidade.</span><span class="sxs-lookup"><span data-stu-id="41a61-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="41a61-205">![Pacote de Unity.Mvc3 instalado](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 pacote instalado")</span><span class="sxs-lookup"><span data-stu-id="41a61-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="41a61-206">*Pacote de Unity.Mvc3 instalado*</span><span class="sxs-lookup"><span data-stu-id="41a61-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="41a61-207">Tarefa 3 - registrar Unity no aplicativo Global.asax.cs\_iniciar</span><span class="sxs-lookup"><span data-stu-id="41a61-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="41a61-208">Nesta tarefa, você atualizará o **aplicativo\_iniciar** método localizado em **Global.asax.cs** para chamar o inicializador de Bootstrapper Unity e, em seguida, atualize o registro de arquivo de Bootstrapper o serviço e o controlador usará para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="41a61-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="41a61-209">Agora, você irá se conectar o Bootstrapper que é o arquivo que inicializa o contêiner do Unity e resolvedor de dependência.</span><span class="sxs-lookup"><span data-stu-id="41a61-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="41a61-210">Para fazer isso, abra **Global.asax.cs** e adicione o seguinte código dentro de **aplicativo\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="41a61-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="41a61-211">(Código de trecho - *Unity inicializar o laboratório de injeção de dependência do ASP.NET - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="41a61-212">Abra **Bootstrapper.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="41a61-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="41a61-213">Inclua os seguintes namespaces: **MvcMusicStore.Services** e **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="41a61-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="41a61-214">(Código de trecho - *injeção de dependência ASP.NET laboratório - Ex01 - Bootstrapper adicionando Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="41a61-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="41a61-215">Substituir **BuildUnityContainer** do conteúdo com o código a seguir que registra o controlador de armazenamento e o serviço de armazenamento de método.</span><span class="sxs-lookup"><span data-stu-id="41a61-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="41a61-216">(Código de trecho - *serviço e o controlador de armazenamento de registro do ASP.NET dependência injeção laboratório - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="41a61-217">Tarefa 4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="41a61-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="41a61-218">Nesta tarefa, você executará o aplicativo para verificar que ele agora pode ser carregado depois incluindo Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="41a61-219">Pressione **F5** para executar o aplicativo, o aplicativo deve carregar agora sem mostrar qualquer mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="41a61-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="41a61-220">![Execução do aplicativo com a injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image6.png "executando o aplicativo com a injeção de dependência")</span><span class="sxs-lookup"><span data-stu-id="41a61-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="41a61-221">*Aplicativo em execução com a injeção de dependência*</span><span class="sxs-lookup"><span data-stu-id="41a61-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="41a61-222">Navegue até **/armazenamento**.</span><span class="sxs-lookup"><span data-stu-id="41a61-222">Browse to **/Store**.</span></span> <span data-ttu-id="41a61-223">Isso vai invocar **StoreController**, que é criado usando **Unity**.</span><span class="sxs-lookup"><span data-stu-id="41a61-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="41a61-224">![Repositório de música MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "repositório de música do MVC")</span><span class="sxs-lookup"><span data-stu-id="41a61-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="41a61-225">*Repositório de música do MVC*</span><span class="sxs-lookup"><span data-stu-id="41a61-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="41a61-226">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="41a61-226">Close the browser.</span></span>

<span data-ttu-id="41a61-227">Os exercícios a seguir, você aprenderá como estender o escopo de injeção de dependência para usá-lo em exibições do ASP.NET MVC e filtros de ação.</span><span class="sxs-lookup"><span data-stu-id="41a61-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="41a61-228">Exercício 2: Insira um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="41a61-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="41a61-229">Neste exercício, você aprenderá como usar injeção de dependência em uma exibição com os novos recursos do ASP.NET MVC 4 para a integração do Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="41a61-230">Para fazer isso, você chamará um serviço personalizado dentro da exibição de procurar o repositório, que mostrará uma mensagem e uma imagem abaixo.</span><span class="sxs-lookup"><span data-stu-id="41a61-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="41a61-231">Em seguida, você integrar o projeto com o Unity e criar um resolvedor de dependência personalizada para injetar as dependências.</span><span class="sxs-lookup"><span data-stu-id="41a61-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="41a61-232">Tarefa 1: criar uma exibição que consome um serviço</span><span class="sxs-lookup"><span data-stu-id="41a61-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="41a61-233">Nesta tarefa, você criará uma exibição que executa uma chamada de serviço para gerar uma nova dependência.</span><span class="sxs-lookup"><span data-stu-id="41a61-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="41a61-234">O serviço consiste em um serviço de mensagens simple incluído nesta solução.</span><span class="sxs-lookup"><span data-stu-id="41a61-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="41a61-235">Abra o **começar** solução localizada no **View\Begin injetando Source\Ex02** pasta.</span><span class="sxs-lookup"><span data-stu-id="41a61-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="41a61-236">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="41a61-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="41a61-237">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="41a61-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="41a61-238">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="41a61-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="41a61-239">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="41a61-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="41a61-240">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="41a61-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41a61-241">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="41a61-242">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="41a61-243">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="41a61-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="41a61-244">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="41a61-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="41a61-245">Incluir o **MessageService.cs** e o **IMessageService.cs** classes localizado no **\Assets de origem** pasta no **/serviços**.</span><span class="sxs-lookup"><span data-stu-id="41a61-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="41a61-246">Para fazer isso, clique com botão direito **serviços** pasta e selecione **Add Existing Item**.</span><span class="sxs-lookup"><span data-stu-id="41a61-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="41a61-247">Navegue até o local dos arquivos e incluí-los.</span><span class="sxs-lookup"><span data-stu-id="41a61-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="41a61-248">![Adicionar serviço de mensagens e Interface de serviço](aspnet-mvc-4-dependency-injection/_static/image8.png "Adicionar serviço de mensagens e Interface de serviço")</span><span class="sxs-lookup"><span data-stu-id="41a61-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="41a61-249">*Adicionar serviço de mensagens e a Interface de serviço*</span><span class="sxs-lookup"><span data-stu-id="41a61-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="41a61-250">O **IMessageService** interface define duas propriedades implementadas pelo **MessageService** classe.</span><span class="sxs-lookup"><span data-stu-id="41a61-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="41a61-251">Essas propriedades -**mensagem** e **ImageUrl**-armazenar a mensagem e a URL da imagem a ser exibida.</span><span class="sxs-lookup"><span data-stu-id="41a61-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="41a61-252">Crie a pasta **/páginas** do projeto raiz da pasta e, em seguida, adicione a classe existente **MyBasePage.cs** de **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="41a61-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="41a61-253">A página de base que herda de tem a seguinte estrutura.</span><span class="sxs-lookup"><span data-stu-id="41a61-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="41a61-254">![Pasta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "pasta páginas")</span><span class="sxs-lookup"><span data-stu-id="41a61-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="41a61-255">Abra **Browse.cshtml** exibir de **/exibições/repositório** pasta e torná-lo herdam **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="41a61-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="41a61-256">No **procurar** exibir, adicionar uma chamada para **MessageService** para exibir uma imagem e uma mensagem recuperada pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="41a61-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="41a61-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="41a61-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="41a61-258">Tarefa 2 - incluindo um resolvedor de dependência personalizada e um ativador de página de modo de exibição personalizado</span><span class="sxs-lookup"><span data-stu-id="41a61-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="41a61-259">Na tarefa anterior, é inserida uma nova dependência dentro de um modo de exibição para executar uma chamada de serviço dentro dele.</span><span class="sxs-lookup"><span data-stu-id="41a61-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="41a61-260">Agora, você resolverá essa dependência ao implementar as interfaces de injeção de dependência do ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="41a61-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="41a61-261">Você incluirá na solução de uma implementação de **IDependencyResolver** que lidará com a recuperação de serviço usando o Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="41a61-262">Em seguida, você incluirá outra implementação personalizada de **IViewPageActivator** interface que resolverá a criação dos modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="41a61-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="41a61-263">Desde o ASP.NET MVC 3, a implementação para injeção de dependência tinha simplificado as interfaces para registrar os serviços.</span><span class="sxs-lookup"><span data-stu-id="41a61-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="41a61-264">**IDependencyResolver** e **IViewPageActivator** fazem parte dos recursos do ASP.NET MVC 3 para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="41a61-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="41a61-265">**-IDependencyResolver** interface substitui o IMvcServiceLocator anterior.</span><span class="sxs-lookup"><span data-stu-id="41a61-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="41a61-266">Os implementadores de IDependencyResolver devem retornar uma instância do serviço ou uma coleção de serviço.</span><span class="sxs-lookup"><span data-stu-id="41a61-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="41a61-267">**-IViewPageActivator** interface fornece um controle mais refinado sobre como exibir páginas são instanciadas por meio de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="41a61-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="41a61-268">As classes que implementam **IViewPageActivator** interface pode criar instâncias de exibição usando informações de contexto.</span><span class="sxs-lookup"><span data-stu-id="41a61-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="41a61-269">Criar o /**fábricas** pasta na pasta raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="41a61-270">Incluir **CustomViewPageActivator.cs** à sua solução de **/fontes ativos/** para **fábricas** pasta.</span><span class="sxs-lookup"><span data-stu-id="41a61-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="41a61-271">Para fazer isso, clique com botão direito do **/Factories** pasta, selecione **adicionar | Item existente** e, em seguida, selecione **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="41a61-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="41a61-272">Essa classe implementa o **IViewPageActivator** interface para manter o contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="41a61-273">**CustomViewPageActivator** é responsável por gerenciar a criação de um modo de exibição usando um contêiner de unidade.</span><span class="sxs-lookup"><span data-stu-id="41a61-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="41a61-274">Incluir **UnityDependencyResolver.cs** arquivo **fontes/ativos** para **/Factories** pasta.</span><span class="sxs-lookup"><span data-stu-id="41a61-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="41a61-275">Para fazer isso, clique com botão direito do **/Factories** pasta, selecione **adicionar | Item existente** e, em seguida, selecione **UnityDependencyResolver.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="41a61-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="41a61-276">**UnityDependencyResolver** classe é um DependencyResolver personalizado para Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="41a61-277">Quando um serviço não foi encontrado dentro do contêiner do Unity, o resolvedor de base é invocated.</span><span class="sxs-lookup"><span data-stu-id="41a61-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="41a61-278">Na tarefa a seguir serão registradas ambas as implementações para saber o local dos serviços e os modos de exibição do modelo.</span><span class="sxs-lookup"><span data-stu-id="41a61-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="41a61-279">Tarefa 3 - registrar-se para injeção de dependência no contêiner do Unity</span><span class="sxs-lookup"><span data-stu-id="41a61-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="41a61-280">Nesta tarefa, você colocará todas as coisas anteriores para fazer a injeção de dependência de trabalho.</span><span class="sxs-lookup"><span data-stu-id="41a61-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="41a61-281">Até agora, sua solução tem os seguintes elementos:</span><span class="sxs-lookup"><span data-stu-id="41a61-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="41a61-282">Um **procurar** exibição herda de **MyBaseClass** e consome **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="41a61-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="41a61-283">Uma classe intermediária -**MyBaseClass**-que foi declarada para a interface do serviço de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="41a61-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="41a61-284">Um serviço - **MessageService** - e sua interface **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="41a61-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="41a61-285">Um resolvedor de dependência personalizada para Unity - **UnityDependencyResolver** -que lida com a recuperação de serviço.</span><span class="sxs-lookup"><span data-stu-id="41a61-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="41a61-286">Um ativador de página de exibição - **CustomViewPageActivator** -que cria a página.</span><span class="sxs-lookup"><span data-stu-id="41a61-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="41a61-287">Inserir **procurar** modo de exibição, você registrar o resolvedor de dependência personalizada no contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="41a61-288">Abra **Bootstrapper.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="41a61-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="41a61-289">Registrar uma instância de **MessageService** no contêiner do Unity para inicializar o serviço:</span><span class="sxs-lookup"><span data-stu-id="41a61-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="41a61-290">(Código de trecho - *serviço de mensagens de registro do ASP.NET dependência injeção laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="41a61-291">Adicione uma referência a **MvcMusicStore.Factories** namespace.</span><span class="sxs-lookup"><span data-stu-id="41a61-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="41a61-292">(Código de trecho - *ASP.NET dependência injeção laboratório - Ex02 - fábricas Namespace*)</span><span class="sxs-lookup"><span data-stu-id="41a61-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="41a61-293">Registrar **CustomViewPageActivator** como um ativador de página de exibição no contêiner do Unity:</span><span class="sxs-lookup"><span data-stu-id="41a61-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="41a61-294">(Código de trecho - *CustomViewPageActivator de registro do ASP.NET dependência injeção laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="41a61-295">Substitua o resolvedor de dependência padrão ASP.NET MVC 4 com uma instância do **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="41a61-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="41a61-296">Para fazer isso, substitua **inicialização** método conteúdo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="41a61-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="41a61-297">(Código de trecho - *resolvedor de dependência de atualização do ASP.NET dependência injeção laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="41a61-298">ASP.NET MVC fornece uma classe do resolvedor de dependência padrão.</span><span class="sxs-lookup"><span data-stu-id="41a61-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="41a61-299">Para trabalhar com os resolvedores de dependência personalizada que você criou para unity, esse resolvedor deve ser substituído.</span><span class="sxs-lookup"><span data-stu-id="41a61-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="41a61-300">Tarefa 4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="41a61-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="41a61-301">Nesta tarefa, você executará o aplicativo para verificar se o navegador do repositório consome o serviço e mostra a imagem e a mensagem recuperada:</span><span class="sxs-lookup"><span data-stu-id="41a61-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="41a61-302">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41a61-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="41a61-303">Clique em **Rock** dentro do Menu gêneros e veja como o **MessageService** foi inserida para o modo de exibição e carregar a mensagem de boas-vinda e a imagem.</span><span class="sxs-lookup"><span data-stu-id="41a61-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="41a61-304">Neste exemplo, podemos inserir a &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="41a61-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="41a61-305">![Repositório de música MVC - exibição injeção](aspnet-mvc-4-dependency-injection/_static/image10.png "repositório de música MVC - injeção de exibição")</span><span class="sxs-lookup"><span data-stu-id="41a61-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="41a61-306">*Repositório de música MVC - injeção de exibição*</span><span class="sxs-lookup"><span data-stu-id="41a61-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="41a61-307">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="41a61-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="41a61-308">Exercício 3: Injetando filtros de ação</span><span class="sxs-lookup"><span data-stu-id="41a61-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="41a61-309">No laboratório prático anterior **filtros de ação personalizado** você trabalhou com injeção e personalização de filtros.</span><span class="sxs-lookup"><span data-stu-id="41a61-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="41a61-310">Neste exercício, você aprenderá como inserir filtros com injeção de dependência usando o contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="41a61-311">Para fazer isso, você adicionará à solução de armazenamento de música um filtro de ação personalizada que será a atividade do site de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="41a61-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="41a61-312">Tarefa 1 – incluindo o filtro de rastreamento na solução</span><span class="sxs-lookup"><span data-stu-id="41a61-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="41a61-313">Nesta tarefa, você incluirá no repositório de música um filtro de ação personalizada para eventos de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="41a61-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="41a61-314">Como o filtro de ação personalizada conceitos já são tratados no laboratório anterior &quot;filtros de ação personalizado&quot;, inclua apenas a classe de filtro da pasta ativos deste laboratório e, em seguida, criar um provedor de filtro para Unity:</span><span class="sxs-lookup"><span data-stu-id="41a61-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="41a61-315">Abra o **começar** solução localizada no **Source\Ex03 - injetando ação Filter\Begin** pasta.</span><span class="sxs-lookup"><span data-stu-id="41a61-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="41a61-316">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="41a61-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="41a61-317">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="41a61-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="41a61-318">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="41a61-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="41a61-319">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="41a61-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="41a61-320">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="41a61-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41a61-321">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="41a61-322">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="41a61-323">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="41a61-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="41a61-324">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="41a61-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="41a61-325">Incluir **TraceActionFilter.cs** arquivo **fontes/ativos** para **/filtra** pasta.</span><span class="sxs-lookup"><span data-stu-id="41a61-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="41a61-326">Esse filtro de ação personalizada executa o rastreamento do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="41a61-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="41a61-327">Você pode verificar &quot;local do ASP.NET MVC 4 e filtros de ação dinâmicos&quot; laboratório para mais de referência.</span><span class="sxs-lookup"><span data-stu-id="41a61-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="41a61-328">Adicione a classe vazia **FilterProvider.cs** para o projeto na pasta   **/filtros.**</span><span class="sxs-lookup"><span data-stu-id="41a61-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="41a61-329">Adicionar o **System.Web.Mvc** e **Microsoft.Practices.Unity** namespaces em **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="41a61-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="41a61-330">(Código de trecho - *provedor ASP.NET dependência injeção laboratório - Ex03 - filtro adicionando Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="41a61-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="41a61-331">Tornar a classe herda de **IFilterProvider** Interface.</span><span class="sxs-lookup"><span data-stu-id="41a61-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="41a61-332">Adicionar um **IUnityContainer** propriedade o **FilterProvider** classe e, em seguida, crie um construtor de classe para atribuir o contêiner.</span><span class="sxs-lookup"><span data-stu-id="41a61-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="41a61-333">(Código de trecho - *construtor do provedor de filtro ASP.NET dependência injeção laboratório - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="41a61-334">O construtor de classe de provedor de filtro não está criando um **novo** dentro do objeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="41a61-335">O contêiner é passado como um parâmetro e a dependência é resolvida Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="41a61-336">No **FilterProvider** classe, implemente o método **GetFilters** de **IFilterProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="41a61-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="41a61-337">(Código de trecho - *GetFilters de provedor de filtro do ASP.NET dependência injeção laboratório - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="41a61-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="41a61-338">Tarefa 2: registrar e ativar o filtro</span><span class="sxs-lookup"><span data-stu-id="41a61-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="41a61-339">Nesta tarefa, você habilitará o controle do site.</span><span class="sxs-lookup"><span data-stu-id="41a61-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="41a61-340">Para fazer isso, você registrará o filtro na **Bootstrapper.cs BuildUnityContainer** método para iniciar o rastreamento:</span><span class="sxs-lookup"><span data-stu-id="41a61-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="41a61-341">Abra **Web. config** localizado na raiz do projeto e habilitar o controle de rastreamento no grupo de System. Web.</span><span class="sxs-lookup"><span data-stu-id="41a61-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="41a61-342">Abra **Bootstrapper.cs** na raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="41a61-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="41a61-343">Adicione uma referência para o **MvcMusicStore.Filters** namespace.</span><span class="sxs-lookup"><span data-stu-id="41a61-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="41a61-344">(Código de trecho - *injeção de dependência ASP.NET laboratório - Ex03 - Bootstrapper adicionando Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="41a61-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="41a61-345">Selecione o **BuildUnityContainer** método e registrar o filtro no contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="41a61-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="41a61-346">Você precisará registrar o provedor de filtro, bem como o filtro de ação.</span><span class="sxs-lookup"><span data-stu-id="41a61-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="41a61-347">(Código de trecho - *ASP.NET dependência injeção laboratório - Ex03 - Register FilterProvider e ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="41a61-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="41a61-348">Tarefa 3: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="41a61-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="41a61-349">Nesta tarefa, você executar o aplicativo e que o filtro de ação personalizado está rastreando a atividade de teste:</span><span class="sxs-lookup"><span data-stu-id="41a61-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="41a61-350">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41a61-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="41a61-351">Clique em **Rock** dentro do Menu gêneros.</span><span class="sxs-lookup"><span data-stu-id="41a61-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="41a61-352">Você pode procurar mais gêneros se desejar.</span><span class="sxs-lookup"><span data-stu-id="41a61-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="41a61-353">![Repositório de música](aspnet-mvc-4-dependency-injection/_static/image11.png "repositório de música")</span><span class="sxs-lookup"><span data-stu-id="41a61-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="41a61-354">*Loja de Música*</span><span class="sxs-lookup"><span data-stu-id="41a61-354">*Music Store*</span></span>
3. <span data-ttu-id="41a61-355">Navegue até **/Trace.axd** para ver o rastreamento de aplicativo de página e, em seguida, clique em **exibir detalhes**.</span><span class="sxs-lookup"><span data-stu-id="41a61-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="41a61-356">![Log de rastreamento de aplicativo](aspnet-mvc-4-dependency-injection/_static/image12.png "Log de rastreamento de aplicativo")</span><span class="sxs-lookup"><span data-stu-id="41a61-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="41a61-357">*Log de rastreamento de aplicativo*</span><span class="sxs-lookup"><span data-stu-id="41a61-357">*Application Trace Log*</span></span>

    <span data-ttu-id="41a61-358">![Rastreamento de aplicativo - detalhes da solicitação](aspnet-mvc-4-dependency-injection/_static/image13.png "aplicativo de rastreamento - detalhes da solicitação")</span><span class="sxs-lookup"><span data-stu-id="41a61-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="41a61-359">*Rastreamento de aplicativo - detalhes da solicitação*</span><span class="sxs-lookup"><span data-stu-id="41a61-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="41a61-360">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="41a61-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="41a61-361">Resumo</span><span class="sxs-lookup"><span data-stu-id="41a61-361">Summary</span></span>

<span data-ttu-id="41a61-362">Ao concluir este laboratório prático, você aprendeu como usar injeção de dependência no ASP.NET MVC 4, integrando Unity usando um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="41a61-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="41a61-363">Para fazer isso, você usou a injeção de dependência dentro de controladores, exibições e filtros de ação.</span><span class="sxs-lookup"><span data-stu-id="41a61-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="41a61-364">Os conceitos a seguir foram abordados:</span><span class="sxs-lookup"><span data-stu-id="41a61-364">The following concepts were covered:</span></span>

- <span data-ttu-id="41a61-365">Recursos de injeção de dependência do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="41a61-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="41a61-366">Integração do Unity usando o pacote do NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="41a61-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="41a61-367">Injeção de dependência em controladores</span><span class="sxs-lookup"><span data-stu-id="41a61-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="41a61-368">Injeção de dependência em modos de exibição</span><span class="sxs-lookup"><span data-stu-id="41a61-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="41a61-369">Injeção de dependência de filtros de ação</span><span class="sxs-lookup"><span data-stu-id="41a61-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="41a61-370">Apêndice a: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="41a61-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="41a61-371">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="41a61-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="41a61-372">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="41a61-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="41a61-373">Vá para [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="41a61-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="41a61-374">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="41a61-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="41a61-375">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="41a61-375">Click on **Install Now**.</span></span> <span data-ttu-id="41a61-376">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="41a61-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="41a61-377">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="41a61-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="41a61-378">![Instalar Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="41a61-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="41a61-379">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="41a61-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="41a61-380">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="41a61-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="41a61-382">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="41a61-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="41a61-383">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="41a61-383">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="41a61-385">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="41a61-385">*Installation progress*</span></span>
6. <span data-ttu-id="41a61-386">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="41a61-386">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="41a61-388">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="41a61-388">*Installation completed*</span></span>
7. <span data-ttu-id="41a61-389">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="41a61-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="41a61-390">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="41a61-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="41a61-392">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="41a61-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="41a61-393">Apêndice b: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="41a61-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="41a61-394">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="41a61-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="41a61-395">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="41a61-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="41a61-396">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-dependency-injection/_static/image19.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="41a61-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="41a61-397">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="41a61-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="41a61-398">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="41a61-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="41a61-399">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="41a61-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="41a61-400">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="41a61-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="41a61-401">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="41a61-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="41a61-402">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="41a61-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="41a61-403">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="41a61-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="41a61-404">![Comece a digitar o nome do fragmento](aspnet-mvc-4-dependency-injection/_static/image20.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="41a61-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="41a61-405">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="41a61-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="41a61-406">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-dependency-injection/_static/image21.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="41a61-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="41a61-407">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="41a61-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="41a61-408">![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-dependency-injection/_static/image22.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="41a61-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="41a61-409">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="41a61-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="41a61-410">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="41a61-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="41a61-411">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="41a61-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="41a61-412">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="41a61-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="41a61-413">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="41a61-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="41a61-414">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-dependency-injection/_static/image23.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="41a61-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="41a61-415">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="41a61-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="41a61-416">![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-dependency-injection/_static/image24.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="41a61-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="41a61-417">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="41a61-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
