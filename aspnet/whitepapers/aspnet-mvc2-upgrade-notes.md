---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Atualizando um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento descreve como atualizar manualmente e com um Assistente de um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2. Este documento também está disponível para d...
ms.author: aspnetcontent
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: fe1696cd8f98f2ff253d385b62a6bcd74b536d33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823865"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="ce4bc-104">Atualizando um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="ce4bc-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="ce4bc-105">Este documento descreve como atualizar manualmente e com um Assistente de um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="ce4bc-106">Este documento também está disponível para [baixar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce4bc-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="ce4bc-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="ce4bc-107">Introduction</span></span>

<span data-ttu-id="ce4bc-108">ASP.NET MVC 2 pode ser instalado lado a lado com o ASP.NET MVC 1.0 no mesmo servidor.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="ce4bc-109">Isso fornece flexibilidade de desenvolvedores de aplicativo na escolha quando atualizar um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="ce4bc-110">Visual Studio 2010 inclui um assistente que atualizações de projetos do ASP.NET MVC 1.0 existentes criados com o Visual Studio 2008 para o ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="ce4bc-111">O Assistente de atualização é iniciado, abrindo um projeto ASP.NET MVC 1.0 no Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="ce4bc-112">Atualizar Assistente para ASP.NET MVC 1.0 no Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ce4bc-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="ce4bc-113">Para atualizar um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2 no Visual Studio 2008 SP1, use o aplicativo MvcAppConverter (sem suporte).</span><span class="sxs-lookup"><span data-stu-id="ce4bc-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="ce4bc-114">Você pode baixar este aplicativo da seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="ce4bc-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="ce4bc-115">Atualizar manualmente um projeto ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="ce4bc-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="ce4bc-116">Para atualizar manualmente um aplicativo ASP.NET MVC 1.0 existente para a versão 2, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ce4bc-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="ce4bc-117">Faça um backup de um projeto existente.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="ce4bc-118">Em um editor de texto, abra o arquivo de projeto (o arquivo com a extensão de arquivo. csproj ou. vbproj) e localize o elemento ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="ce4bc-119">Como o valor desse elemento, substitua o GUID {603c0e0b-db56-11dc-be95-000d561079b0} com {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="ce4bc-120">Quando você terminar, o valor desse elemento deve ser da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ce4bc-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="ce4bc-121">Na pasta raiz do aplicativo Web, edite o arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="ce4bc-122">Procure System.Web.Mvc, versão = 1.0.0.0 e substitua todas as instâncias com System.Web.Mvc, versão = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="ce4bc-123">Repita a etapa anterior para o arquivo Web. config localizado na pasta modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="ce4bc-124">Abra o projeto usando o Visual Studio e, na **Gerenciador de soluções**, expanda o **referências** nó.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="ce4bc-125">Exclua a referência ao System.Web.Mvc (que aponta para o assembly de versão 1.0).</span><span class="sxs-lookup"><span data-stu-id="ce4bc-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="ce4bc-126">Adicione uma referência ao System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="ce4bc-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="ce4bc-127">Adicione o seguinte elemento de bindingRedirect para o arquivo Web. config na raiz do aplicativo sob a seção de configuração:</span><span class="sxs-lookup"><span data-stu-id="ce4bc-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="ce4bc-128">Crie um novo aplicativo ASP.NET MVC 2 vazio.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="ce4bc-129">Copie os arquivos da pasta Scripts do novo aplicativo para a pasta de Scripts do aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="ce4bc-130">Atualizar o existente â €™ arquivo CSS de s com as definições de estilo CSS no arquivo CSS.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="ce4bc-131">Compilar o aplicativo e executá-lo.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-131">Compile the application and run it.</span></span> <span data-ttu-id="ce4bc-132">Se ocorrerem erros, consulte a seção alterações significativas do [o que há de novo no ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) página.</span><span class="sxs-lookup"><span data-stu-id="ce4bc-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
