---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Executando Scripts do Windows PowerShell de arquivos de projeto do MSBuild | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, no b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362889"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="a92f9-104">Executando Scripts do Windows PowerShell de arquivos de projeto do MSBuild</span><span class="sxs-lookup"><span data-stu-id="a92f9-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="a92f9-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a92f9-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a92f9-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="a92f9-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a92f9-107">Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação.</span><span class="sxs-lookup"><span data-stu-id="a92f9-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="a92f9-108">Você pode executar um script localmente (em outras palavras, no servidor de compilação) ou remotamente, como em um servidor web de destino ou o servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a92f9-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="a92f9-109">Há muitas razões por que você talvez queira executar um script de pós-implantação do Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a92f9-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="a92f9-110">Por exemplo, é possível:</span><span class="sxs-lookup"><span data-stu-id="a92f9-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="a92f9-111">Adicione uma fonte de evento personalizado para o registro.</span><span class="sxs-lookup"><span data-stu-id="a92f9-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="a92f9-112">Gere um diretório de sistema de arquivos para carregamentos.</span><span class="sxs-lookup"><span data-stu-id="a92f9-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="a92f9-113">Limpe os diretórios de compilação.</span><span class="sxs-lookup"><span data-stu-id="a92f9-113">Clean up build directories.</span></span>
> - <span data-ttu-id="a92f9-114">Gravar entradas em um arquivo de log personalizado.</span><span class="sxs-lookup"><span data-stu-id="a92f9-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="a92f9-115">Envie emails para convidar usuários para um aplicativo web recentemente provisionado.</span><span class="sxs-lookup"><span data-stu-id="a92f9-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="a92f9-116">Crie contas de usuário com as permissões apropriadas.</span><span class="sxs-lookup"><span data-stu-id="a92f9-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="a92f9-117">Configure a replicação entre instâncias do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a92f9-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="a92f9-118">Este tópico mostra como executar scripts do Windows PowerShell local e remotamente de um destino personalizado em um arquivo de projeto do Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="a92f9-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="a92f9-119">Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a92f9-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="a92f9-120">O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build.</span><span class="sxs-lookup"><span data-stu-id="a92f9-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="a92f9-121">No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.</span><span class="sxs-lookup"><span data-stu-id="a92f9-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="a92f9-122">Visão geral da tarefa</span><span class="sxs-lookup"><span data-stu-id="a92f9-122">Task Overview</span></span>

<span data-ttu-id="a92f9-123">Para executar um script do Windows PowerShell como parte de um processo de implantação automatizada ou passo a passo, você precisará concluir essas tarefas de alto nível:</span><span class="sxs-lookup"><span data-stu-id="a92f9-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="a92f9-124">Adicione o script do Windows PowerShell à sua solução e ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="a92f9-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="a92f9-125">Crie um comando que invoca o script do Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a92f9-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="a92f9-126">Todos os caracteres XML reservados em seu comando de escape.</span><span class="sxs-lookup"><span data-stu-id="a92f9-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="a92f9-127">Criar um destino no arquivo de projeto personalizado do MSBuild e usar o **Exec** tarefa para executar o comando.</span><span class="sxs-lookup"><span data-stu-id="a92f9-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="a92f9-128">Este tópico mostra como executar esses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="a92f9-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="a92f9-129">As tarefas e instruções passo a passo neste tópico pressupõem que você já esteja familiarizado com as propriedades e destinos do MSBuild e que você compreenda como usar um arquivo de projeto personalizado do MSBuild para orientar um processo de compilação e implantação.</span><span class="sxs-lookup"><span data-stu-id="a92f9-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="a92f9-130">Para obter mais informações, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="a92f9-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="a92f9-131">Criando e adicionando Scripts do Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="a92f9-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="a92f9-132">As tarefas neste tópico usam um exemplo de script do Windows PowerShell nomeado **LogDeploy.ps1** para ilustrar como executar scripts do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a92f9-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="a92f9-133">O **LogDeploy.ps1** script contém uma função simples que grava uma entrada de linha única para um arquivo de log:</span><span class="sxs-lookup"><span data-stu-id="a92f9-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="a92f9-134">O **LogDeploy.ps1** script aceita dois parâmetros.</span><span class="sxs-lookup"><span data-stu-id="a92f9-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="a92f9-135">O primeiro parâmetro representa o caminho completo para o arquivo de log para o qual você deseja adicionar uma entrada e o segundo parâmetro representa o destino de implantação que você deseja gravar no arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="a92f9-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="a92f9-136">Quando você executa o script, ele adiciona uma linha ao arquivo de log neste formato:</span><span class="sxs-lookup"><span data-stu-id="a92f9-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="a92f9-137">Para tornar o **LogDeploy.ps1** script disponível para o MSBuild, você precisa:</span><span class="sxs-lookup"><span data-stu-id="a92f9-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="a92f9-138">Adicione o script ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="a92f9-138">Add the script to source control.</span></span>
- <span data-ttu-id="a92f9-139">Adicione o script para sua solução no Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a92f9-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="a92f9-140">Você não precisa implantar o script com o conteúdo de solução, independentemente se você planeja executar o script no servidor de compilação ou em um computador remoto.</span><span class="sxs-lookup"><span data-stu-id="a92f9-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="a92f9-141">É uma opção Adicionar o script para uma pasta de solução.</span><span class="sxs-lookup"><span data-stu-id="a92f9-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="a92f9-142">O exemplo Contact Manager, como você deseja usar o script do Windows PowerShell como parte do processo de implantação, faz sentido para adicionar o script para a pasta de solução de publicação.</span><span class="sxs-lookup"><span data-stu-id="a92f9-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="a92f9-143">O conteúdo das pastas de solução é copiado para os servidores de compilação como material de origem.</span><span class="sxs-lookup"><span data-stu-id="a92f9-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="a92f9-144">No entanto, eles formam nenhuma parte de qualquer saída do projeto.</span><span class="sxs-lookup"><span data-stu-id="a92f9-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="a92f9-145">Executar um Script do Windows PowerShell no servidor de compilação</span><span class="sxs-lookup"><span data-stu-id="a92f9-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="a92f9-146">Em alguns cenários, você talvez queira executar scripts do Windows PowerShell no computador que compila seus projetos.</span><span class="sxs-lookup"><span data-stu-id="a92f9-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="a92f9-147">Por exemplo, você pode usar um script do Windows PowerShell para limpeza de pastas de compilação ou gravar entradas em um arquivo de log personalizado.</span><span class="sxs-lookup"><span data-stu-id="a92f9-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="a92f9-148">Em termos de sintaxe, executar um script do Windows PowerShell de um arquivo de projeto do MSBuild é o mesmo que executar um script do Windows PowerShell em um prompt de comando regular.</span><span class="sxs-lookup"><span data-stu-id="a92f9-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="a92f9-149">Você precisa invocar o powershell.exe executável e usar o **– comando** switch para fornecer os comandos que você deseja que o Windows PowerShell para executar.</span><span class="sxs-lookup"><span data-stu-id="a92f9-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="a92f9-150">(No Windows PowerShell v2, você também pode usar o **– arquivo** alternar).</span><span class="sxs-lookup"><span data-stu-id="a92f9-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="a92f9-151">O comando deve levar este formato:</span><span class="sxs-lookup"><span data-stu-id="a92f9-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="a92f9-152">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a92f9-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="a92f9-153">Se o caminho para o seu script contiver espaços, você precisa colocar o caminho do arquivo entre aspas, precedidas por um e comercial.</span><span class="sxs-lookup"><span data-stu-id="a92f9-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="a92f9-154">É possível usar aspas duplas, porque você já tiver usado-los para incluir o comando:</span><span class="sxs-lookup"><span data-stu-id="a92f9-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="a92f9-155">Há algumas considerações adicionais quando você invoca este comando do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a92f9-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="a92f9-156">Primeiro, você deve incluir a **– NonInteractive** sinalizador para garantir que o script é executado no modo silencioso.</span><span class="sxs-lookup"><span data-stu-id="a92f9-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="a92f9-157">Em seguida, você deve incluir a **ExecutionPolicy –** sinalizador com um valor de argumento apropriado.</span><span class="sxs-lookup"><span data-stu-id="a92f9-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="a92f9-158">Isso especifica a política de execução que o Windows PowerShell será aplicada ao seu script e permite que você substitua a política de execução padrão, que pode impedir a execução do script.</span><span class="sxs-lookup"><span data-stu-id="a92f9-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="a92f9-159">Você pode escolher entre esses valores de argumento:</span><span class="sxs-lookup"><span data-stu-id="a92f9-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="a92f9-160">Um valor de **irrestrito** permitirá que o Windows PowerShell executar o script, independentemente se o script está assinado.</span><span class="sxs-lookup"><span data-stu-id="a92f9-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="a92f9-161">Um valor de **RemoteSigned** permitirá que o Windows PowerShell executar scripts não assinados que foram criados no computador local.</span><span class="sxs-lookup"><span data-stu-id="a92f9-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="a92f9-162">No entanto, os scripts que foram criados em outro lugar devem ser assinados.</span><span class="sxs-lookup"><span data-stu-id="a92f9-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="a92f9-163">(Na prática, é muito improvável que tenha criado um script do PowerShell do Windows localmente em um servidor de compilação).</span><span class="sxs-lookup"><span data-stu-id="a92f9-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="a92f9-164">Um valor de **AllSigned** permitirá que o Windows PowerShell executar somente scripts assinados.</span><span class="sxs-lookup"><span data-stu-id="a92f9-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="a92f9-165">A política de execução padrão é **Restricted**, que impede que o Windows PowerShell executando qualquer arquivo de script.</span><span class="sxs-lookup"><span data-stu-id="a92f9-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="a92f9-166">Por fim, você precisará escapar qualquer caracteres XML reservados que ocorrem em seu comando do Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a92f9-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="a92f9-167">Substituir aspas com  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="a92f9-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="a92f9-168">Substitua as aspas duplas com  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="a92f9-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="a92f9-169">Substitua "e" comercial com  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="a92f9-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="a92f9-170">Quando você fizer essas alterações, o comando será parecida com isto:</span><span class="sxs-lookup"><span data-stu-id="a92f9-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="a92f9-171">Dentro de seu arquivo de projeto personalizado do MSBuild, você pode criar um novo destino e usar o **Exec** tarefa para executar este comando:</span><span class="sxs-lookup"><span data-stu-id="a92f9-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="a92f9-172">Neste exemplo, observe que:</span><span class="sxs-lookup"><span data-stu-id="a92f9-172">In this example, note that:</span></span>

- <span data-ttu-id="a92f9-173">Todas as variáveis, como valores de parâmetro e o local do executável do Windows PowerShell, são declaradas como propriedades do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a92f9-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="a92f9-174">As condições são incluídas para permitir que os usuários substituir esses valores da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="a92f9-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="a92f9-175">O **MSDeployComputerName** propriedade é declarada em outro lugar no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="a92f9-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="a92f9-176">Quando você executar esse destino como parte do processo de compilação, o Windows PowerShell execute o comando e gravar uma entrada de log para o arquivo especificado.</span><span class="sxs-lookup"><span data-stu-id="a92f9-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="a92f9-177">Executar um Script do Windows PowerShell em um computador remoto</span><span class="sxs-lookup"><span data-stu-id="a92f9-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="a92f9-178">Windows PowerShell é capaz de executar scripts em computadores remotos por meio [gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="a92f9-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="a92f9-179">Para fazer isso, você precisa usar o [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a92f9-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="a92f9-180">Isso permite que você executar o script em um ou mais computadores remotos sem copiar o script para os computadores remotos.</span><span class="sxs-lookup"><span data-stu-id="a92f9-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="a92f9-181">Todos os resultados são retornados ao computador local do qual você executou o script.</span><span class="sxs-lookup"><span data-stu-id="a92f9-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="a92f9-182">Antes de usar o **Invoke-Command** scripts de cmdlet para executar o Windows PowerShell em um computador remoto, você precisará configurar um ouvinte de WinRM para aceitar mensagens remotas.</span><span class="sxs-lookup"><span data-stu-id="a92f9-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="a92f9-183">Você pode fazer isso executando o comando **winrm quickconfig** no computador remoto.</span><span class="sxs-lookup"><span data-stu-id="a92f9-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="a92f9-184">Para obter mais informações, consulte [instalação e configuração para gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="a92f9-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="a92f9-185">Em uma janela do Windows PowerShell, você usaria essa sintaxe para executar o **LogDeploy.ps1** script em um computador remoto:</span><span class="sxs-lookup"><span data-stu-id="a92f9-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="a92f9-186">Há várias outras maneiras de usar **Invoke-Command** para executar um script do arquivo, mas essa abordagem é mais simples quando você precisa fornecer valores de parâmetro e gerenciar caminhos com espaços.</span><span class="sxs-lookup"><span data-stu-id="a92f9-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="a92f9-187">Quando você executar isso em um prompt de comando, você precisará chamar o executável de PowerShell do Windows e usar o **– comando** parâmetro para fornecer suas instruções:</span><span class="sxs-lookup"><span data-stu-id="a92f9-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="a92f9-188">Como antes, você precisará fornecer algumas opções adicionais e todos os caracteres XML reservados de escape quando você executa o comando do MSBuild:</span><span class="sxs-lookup"><span data-stu-id="a92f9-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="a92f9-189">Por fim, assim como antes, você pode usar o **Exec** tarefa dentro de um destino MSBuild personalizado para executar o comando:</span><span class="sxs-lookup"><span data-stu-id="a92f9-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="a92f9-190">Quando você executar esse destino como parte do processo de compilação, o Windows PowerShell executará seu script no computador especificado na **– computername** argumento.</span><span class="sxs-lookup"><span data-stu-id="a92f9-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a92f9-191">Conclusão</span><span class="sxs-lookup"><span data-stu-id="a92f9-191">Conclusion</span></span>

<span data-ttu-id="a92f9-192">Este tópico descreveu como executar um script do Windows PowerShell de um arquivo de projeto MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a92f9-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="a92f9-193">Você pode usar essa abordagem para executar um script do Windows PowerShell, localmente ou em um computador remoto, como parte de um processo de compilação e implantação automatizado ou passo a passo.</span><span class="sxs-lookup"><span data-stu-id="a92f9-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a92f9-194">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="a92f9-194">Further Reading</span></span>

<span data-ttu-id="a92f9-195">Para obter orientação sobre como assinar scripts do Windows PowerShell e gerenciar políticas de execução, consulte [executando Scripts do Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="a92f9-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="a92f9-196">Para obter orientação sobre como executar comandos do Windows PowerShell de um computador remoto, consulte [executando comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="a92f9-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="a92f9-197">Para obter mais informações sobre como usar arquivos de projeto personalizados do MSBuild para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="a92f9-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a92f9-198">[Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Próximo](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="a92f9-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
