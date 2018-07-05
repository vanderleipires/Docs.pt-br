---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Scaffolding do ASP.NET é um novo recurso que está incluído no Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: f014ef1439376349f49f10f1f4063945ab81e7c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369086"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="54c57-103">Scaffolding do ASP.NET no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54c57-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="54c57-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="54c57-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="54c57-105">Scaffolding do ASP.NET é um novo recurso que está incluído no Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="54c57-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="54c57-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="54c57-106">Overview</span></span>

<span data-ttu-id="54c57-107">Scaffolding do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="54c57-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="54c57-108">Visual Studio 2013 inclui geradores de código previamente instalados para projetos MVC e API da Web.</span><span class="sxs-lookup"><span data-stu-id="54c57-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="54c57-109">Adicionar o scaffolding ao projeto quando você deseja adicionar rapidamente o código que interage com os modelos de dados.</span><span class="sxs-lookup"><span data-stu-id="54c57-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="54c57-110">Usando o scaffolding pode reduzir a quantidade de tempo para desenvolver as operações de dados padrão em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="54c57-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="54c57-111">Por padrão, o Visual Studio 2013 não oferece suporte a geração de código para um projeto de Web Forms, mas você pode usar o scaffolding com Web Forms Adicionando dependências MVC ao projeto ou instalar uma extensão.</span><span class="sxs-lookup"><span data-stu-id="54c57-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="54c57-112">Ambas as abordagens são mostradas abaixo.</span><span class="sxs-lookup"><span data-stu-id="54c57-112">Both approaches are shown below.</span></span>

<span data-ttu-id="54c57-113">Visual Studio 2013 atualização 2 (atualmente RC) fornece a capacidade de estender o Scaffolding do ASP.NET para atender aos requisitos do seu cenário.</span><span class="sxs-lookup"><span data-stu-id="54c57-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="54c57-114">Com essa funcionalidade, você pode criar um modelo de scaffolding personalizado e adicioná-lo à caixa de diálogo Adicionar Scaffold novo.</span><span class="sxs-lookup"><span data-stu-id="54c57-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="54c57-115">Dentro do modelo personalizado, você deve especificar o código que é gerado ao adicionar um item de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="54c57-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="54c57-116">Para obter mais informações, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="54c57-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54c57-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="54c57-117">Prerequisites</span></span>

<span data-ttu-id="54c57-118">Para usar o Scaffolding do ASP.NET, você deve ter:</span><span class="sxs-lookup"><span data-stu-id="54c57-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="54c57-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54c57-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="54c57-120">Ferramentas de desenvolvedor da Web (parte da instalação padrão do Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="54c57-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="54c57-121">Estruturas da Web do ASP.NET e Tools 2013 (parte da instalação padrão do Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="54c57-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="54c57-122">Adicionar um item de scaffolding MVC ou API da Web</span><span class="sxs-lookup"><span data-stu-id="54c57-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="54c57-123">Para adicionar um scaffold, clique com botão direito no projeto ou uma pasta dentro do projeto e selecione **Add** – **Novo Item de Scaffold**, conforme mostrado na imagem a seguir.</span><span class="sxs-lookup"><span data-stu-id="54c57-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Adicionar item andaime](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="54c57-125">Dos **adicionar Scaffold** janela, selecione o tipo de scaffold para adicionar.</span><span class="sxs-lookup"><span data-stu-id="54c57-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Selecione o tipo de scaffold](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="54c57-127">O **Adicionar controlador** janela lhe dá a oportunidade de selecionar opções para gerar o controlador, incluindo se você deseja usar os novos recursos assíncronos do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="54c57-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Adicionar controlador](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="54c57-129">As classes relevantes e as páginas são criadas para seu cenário.</span><span class="sxs-lookup"><span data-stu-id="54c57-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="54c57-130">Por exemplo, a imagem a seguir mostra o controlador MVC e exibições que foram criadas por meio de scaffolding para uma classe de modelo chamada filmes.</span><span class="sxs-lookup"><span data-stu-id="54c57-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Os arquivos criados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="54c57-132">Adicionar um item de scaffolding para Web Forms</span><span class="sxs-lookup"><span data-stu-id="54c57-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="54c57-133">Para adicionar o scaffolding que gera o código de Web Forms, você deve instalar uma extensão para o Visual Studio ou adicionar dependências do MVC.</span><span class="sxs-lookup"><span data-stu-id="54c57-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="54c57-134">Ambas as abordagens são mostradas abaixo, mas você só precisa fazer uma dessas abordagens.</span><span class="sxs-lookup"><span data-stu-id="54c57-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="54c57-135">Extensão de Scaffolding de Web Forms</span><span class="sxs-lookup"><span data-stu-id="54c57-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="54c57-136">Você pode instalar uma extensão do Visual Studio que permitem que você usar o scaffolding com um projeto de Web Forms.</span><span class="sxs-lookup"><span data-stu-id="54c57-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="54c57-137">No Visual Studio, selecione **ferramentas** e, em seguida **extensões e atualizações**.</span><span class="sxs-lookup"><span data-stu-id="54c57-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="54c57-138">Nessa caixa de diálogo Pesquisar Galeria do Visual Studio para **Scaffolding dos Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="54c57-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![instalar o web forms do scaffolding](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="54c57-140">Para obter mais informações, consulte [Scaffolding dos Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="54c57-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="54c57-141">Dependências do MVC</span><span class="sxs-lookup"><span data-stu-id="54c57-141">MVC Dependencies</span></span>

<span data-ttu-id="54c57-142">Para adicionar dependências do MVC, selecione **Add** - **Novo Item de Scaffold**.</span><span class="sxs-lookup"><span data-stu-id="54c57-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="54c57-143">Na janela Adicionar Scaffold, selecione **dependências MVC**, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="54c57-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Adicionar dependências MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="54c57-145">Há duas opções para scaffolding MVC; Mínimo e completo.</span><span class="sxs-lookup"><span data-stu-id="54c57-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="54c57-146">Se você selecionar no mínimo, apenas os pacotes do NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="54c57-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="54c57-147">Se você selecionar a opção completa, as dependências mínimas são adicionadas, bem como os arquivos de conteúdo necessários para um projeto do MVC.</span><span class="sxs-lookup"><span data-stu-id="54c57-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="54c57-148">Para usar facilmente o scaffolding, selecione dependências completas.</span><span class="sxs-lookup"><span data-stu-id="54c57-148">To easily use scaffolding, select Full dependencies.</span></span>

![Selecionar dependências completas](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="54c57-150">Depois de adicionar as dependências, você verá uma **Readme. txt** arquivo.</span><span class="sxs-lookup"><span data-stu-id="54c57-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="54c57-151">Cuidadosamente, siga as instruções nesse arquivo para garantir que seu projeto funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="54c57-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="54c57-152">Quando você tiver concluído as etapas no arquivo readme. txt, você pode adicionar um novo item com Scaffold, conforme mostrado na seção anterior sobre o MVC e API da Web.</span><span class="sxs-lookup"><span data-stu-id="54c57-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="54c57-153">Os modos de exibição gerados automaticamente e o controlador funcionará corretamente dentro de seu projeto.</span><span class="sxs-lookup"><span data-stu-id="54c57-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="54c57-154">Tutoriais</span><span class="sxs-lookup"><span data-stu-id="54c57-154">Tutorials</span></span>

<span data-ttu-id="54c57-155">Para criar um scaffolder personalizado, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="54c57-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="54c57-156">Para personalizar os arquivos gerados, consulte [como personalizar os arquivos gerados na caixa de diálogo Novo Item de Scaffold](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="54c57-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="54c57-157">Para obter um exemplo de como usar o scaffolding com **desenvolvimento de banco de dados primeiro**, consulte [First do EF banco de dados com o ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="54c57-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="54c57-158">Para obter um exemplo de como usar o scaffolding em uma **MVC** de projeto, consulte [Introdução ao ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="54c57-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="54c57-159">Para obter um exemplo de como usar o scaffolding em uma **API Web** de projeto, consulte [criar uma API REST com roteamento de atributo na API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="54c57-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
