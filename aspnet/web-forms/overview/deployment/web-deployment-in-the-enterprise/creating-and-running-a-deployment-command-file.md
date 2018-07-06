---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Criando e executando uma implantação de arquivo de comando | Microsoft Docs
author: jrjlee
description: Este tópico descreve como criar um arquivo de comando que lhe permitirá executar uma implantação usando os arquivos de projeto do Microsoft Build Engine (MSBuild) como uma única etapa, re...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830296"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="c8f84-103">Criando e executando um arquivo de comando de implantação</span><span class="sxs-lookup"><span data-stu-id="c8f84-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="c8f84-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="c8f84-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="c8f84-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="c8f84-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="c8f84-106">Este tópico descreve como criar um arquivo de comando que permitirá que você execute uma implantação usando arquivos de projeto do Microsoft Build Engine (MSBuild) como um processo passo a passo e reproduzível.</span><span class="sxs-lookup"><span data-stu-id="c8f84-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="c8f84-107">Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [Contact Manager](the-contact-manager-solution.md) solução&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8f84-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="c8f84-108">O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o processo de compilação](understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build.</span><span class="sxs-lookup"><span data-stu-id="c8f84-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="c8f84-109">No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.</span><span class="sxs-lookup"><span data-stu-id="c8f84-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="c8f84-110">Visão geral do processo</span><span class="sxs-lookup"><span data-stu-id="c8f84-110">Process Overview</span></span>

<span data-ttu-id="c8f84-111">Neste tópico, você aprenderá como criar e executar um arquivo de comando que usa esses arquivos de projeto para executar uma implantação repetível em seu ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="c8f84-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="c8f84-112">Essencialmente, o arquivo de comando simplesmente precisa conter um comando do MSBuild que:</span><span class="sxs-lookup"><span data-stu-id="c8f84-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="c8f84-113">Informa ao MSBuild para executar o ambiente independente *Publish.proj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="c8f84-114">Informa o *Publish.proj* arquivo qual arquivo contém as configurações de projeto específicas do ambiente e onde encontrá-lo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="c8f84-115">Criar um comando do MSBuild</span><span class="sxs-lookup"><span data-stu-id="c8f84-115">Create an MSBuild Command</span></span>

<span data-ttu-id="c8f84-116">Conforme descrito em [Noções básicas sobre o processo de compilação](understanding-the-build-process.md), o arquivo de projeto específicas do ambiente&#x2014;por exemplo, *Env Dev.proj*&#x2014;é projetado para ser importado para o ambiente independente *Publish.proj* arquivo no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="c8f84-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="c8f84-117">Juntos, esses dois arquivos fornecem um conjunto completo de instruções que dizem ao MSBuild como compilar e implantar sua solução.</span><span class="sxs-lookup"><span data-stu-id="c8f84-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="c8f84-118">O *Publish.proj* arquivo usa uma **importar** elemento para importar o arquivo de projeto específicas do ambiente.</span><span class="sxs-lookup"><span data-stu-id="c8f84-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="c8f84-119">Dessa forma, quando você usa MSBuild.exe para compilar e implantar a solução do Contact Manager, você precisa:</span><span class="sxs-lookup"><span data-stu-id="c8f84-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="c8f84-120">Executar MSBuild.exe na *Publish.proj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="c8f84-121">Especifique o local do arquivo de projeto específicas do ambiente, fornecendo um parâmetro de linha de comando chamado **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="c8f84-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="c8f84-122">Para fazer isso, seu comando de MSBuild deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="c8f84-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="c8f84-123">A partir daqui, é uma etapa simple para mover para uma implantação repetível, passo a passo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="c8f84-124">Tudo que você precisa fazer é adicionar o comando de MSBuild em um arquivo. cmd.</span><span class="sxs-lookup"><span data-stu-id="c8f84-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="c8f84-125">Na solução do Contact Manager, a pasta de publicação inclui um arquivo chamado *Dev.cmd publicar* que faz exatamente isso.</span><span class="sxs-lookup"><span data-stu-id="c8f84-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="c8f84-126">O **/fl** opção instrui o MSBuild para criar um arquivo de log chamado *MSBuild* no diretório de trabalho em que MSBuild.exe foi invocado.</span><span class="sxs-lookup"><span data-stu-id="c8f84-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="c8f84-127">Para implantar ou reimplantar a solução de Gerenciador de contatos, tudo o que você precisa fazer é executar o *Dev.cmd publicar* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="c8f84-128">Quando você executar o arquivo, o MSBuild irá:</span><span class="sxs-lookup"><span data-stu-id="c8f84-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="c8f84-129">Compile todos os projetos na solução.</span><span class="sxs-lookup"><span data-stu-id="c8f84-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="c8f84-130">Gere pacotes de web implantável para projetos de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c8f84-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="c8f84-131">Gere arquivos .dbschema e .deploymanifest para os projetos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8f84-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="c8f84-132">Implante os pacotes da web para o servidor web.</span><span class="sxs-lookup"><span data-stu-id="c8f84-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="c8f84-133">Implante o banco de dados no servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8f84-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="c8f84-134">Executar a implantação</span><span class="sxs-lookup"><span data-stu-id="c8f84-134">Run the Deployment</span></span>

<span data-ttu-id="c8f84-135">Quando você tiver criado um arquivo de comando para o seu ambiente de destino, você poderá concluir a implantação inteira, simplesmente executando o arquivo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="c8f84-136">**Para implantar a solução de Gerenciador de contatos em seu ambiente de teste**</span><span class="sxs-lookup"><span data-stu-id="c8f84-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="c8f84-137">Em sua estação de trabalho do desenvolvedor, abra o Windows Explorer e, em seguida, navegue até o local do *Dev.cmd publicar* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="c8f84-138">Clique duas vezes no arquivo para executá-lo.</span><span class="sxs-lookup"><span data-stu-id="c8f84-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="c8f84-139">Se um **abrir arquivo – Aviso de segurança** caixa de diálogo for exibida, clique em **executar**.</span><span class="sxs-lookup"><span data-stu-id="c8f84-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="c8f84-140">Se suas definições de configuração e servidores de teste estão configurados corretamente, a janela de Prompt de comando mostrará um **Build bem-sucedido** mensagem ao MSBuild terminou de processar os arquivos de projeto.</span><span class="sxs-lookup"><span data-stu-id="c8f84-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="c8f84-141">Se essa for a primeira vez em que você implantou a solução para esse ambiente, você precisará adicionar a conta de máquina de servidor do teste da web para o **db\_datawriter** e **db\_datareader**funções do **ContactManager** banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8f84-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="c8f84-142">Esse procedimento é descrito na [configurar um servidor de banco de dados para publicação de implantação na Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c8f84-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8f84-143">Você só precisa atribuir essas permissões ao criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8f84-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="c8f84-144">Por padrão, o processo de compilação não recriará o banco de dados em todas as implantações&#x2014;em vez disso, ele comparará o banco de dados existente para o esquema mais recente e fazer somente as alterações necessárias.</span><span class="sxs-lookup"><span data-stu-id="c8f84-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="c8f84-145">Como resultado, você só será necessário mapear essas funções de banco de dados na primeira vez que você implantar a solução.</span><span class="sxs-lookup"><span data-stu-id="c8f84-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="c8f84-146">Abra o Internet Explorer e navegue até a URL do aplicativo Gerenciador de contatos (por exemplo, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="c8f84-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="c8f84-147">Verifique se o aplicativo funciona conforme o esperado e você é capaz de adicionar contatos.</span><span class="sxs-lookup"><span data-stu-id="c8f84-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="c8f84-148">Conclusão</span><span class="sxs-lookup"><span data-stu-id="c8f84-148">Conclusion</span></span>

<span data-ttu-id="c8f84-149">Criar um arquivo de comando que contém as instruções do MSBuild fornece uma maneira rápida e fácil de criar e implantar uma solução com vários projetos em um ambiente de destino específico.</span><span class="sxs-lookup"><span data-stu-id="c8f84-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="c8f84-150">Se você precisar implantar repetidamente sua solução em vários ambientes de destino, você pode criar vários arquivos de comando.</span><span class="sxs-lookup"><span data-stu-id="c8f84-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="c8f84-151">Em cada arquivo de comando, o comando MSBuild criará o mesmo arquivo de projeto universal, mas ele especificará um arquivo de projeto diferente de específicas do ambiente.</span><span class="sxs-lookup"><span data-stu-id="c8f84-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="c8f84-152">Por exemplo, um arquivo de comando para publicar para um desenvolvedor ou ambiente de teste pode conter este comando do MSBuild:</span><span class="sxs-lookup"><span data-stu-id="c8f84-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="c8f84-153">Um arquivo de comando para publicar em um ambiente de preparo pode conter este comando do MSBuild:</span><span class="sxs-lookup"><span data-stu-id="c8f84-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="c8f84-154">Para obter orientação sobre como personalizar os arquivos de projeto de ambiente específicas para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="c8f84-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="c8f84-155">Você também pode personalizar o processo de compilação para cada ambiente substituindo propriedades ou definindo várias outras opções em seu comando de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="c8f84-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="c8f84-156">Para obter mais informações, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="c8f84-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c8f84-157">[Anterior](deploying-database-projects.md)
> [Próximo](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="c8f84-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
