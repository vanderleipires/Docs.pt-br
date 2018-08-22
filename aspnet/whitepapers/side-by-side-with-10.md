---
uid: whitepapers/side-by-side-with-10
title: Execução do .NET Framework 1.0 e 1.1 do ASP.NET lado a lado | Microsoft Docs
author: rick-anderson
description: Este white paper descreve como instalar o .NET 1.0 e 1.1 do .NET em seu computador, permitindo que um aplicativo Web ASP.NET ser executado em qualquer versão do enquadrar...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: b5457a62e127ba555674fbe3b9f75552cad041c7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824354"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="0747d-103">Execução do ASP.NET lado a lado do .NET Framework 1.0 e 1.1</span><span class="sxs-lookup"><span data-stu-id="0747d-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="0747d-104">Este white paper descreve como instalar o .NET 1.0 e 1.1 do .NET em seu computador, permitindo que um aplicativo Web ASP.NET seja executado em qualquer versão do framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="0747d-105">Aplica-se para o ASP.NET 1.0 e ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="0747d-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="0747d-106">No ASP.NET, aplicativos devem estar em execução lado a lado quando eles são instalados no mesmo computador, mas usam diferentes versões do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="0747d-107">O tópico a seguir descreve como configurar aplicativos do ASP.NET para execução lado a lado e fornece etapas detalhadas para:</span><span class="sxs-lookup"><span data-stu-id="0747d-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="0747d-108">Manter o mapeamento de seu aplicativo Web para o .NET Framework versão 1.0 durante a instalação</span><span class="sxs-lookup"><span data-stu-id="0747d-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="0747d-109">Mapa de um aplicativo Web para uma versão específica do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="0747d-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="0747d-110">Localizar a versão do .NET Framework que está usando um site da Web</span><span class="sxs-lookup"><span data-stu-id="0747d-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="0747d-111">Tradicionalmente, quando um componente ou aplicativo é atualizado em um computador, a versão mais antiga é removida e substituída pela versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="0747d-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="0747d-112">Se a nova versão não é compatível com a versão anterior, isso geralmente afeta outros aplicativos que usam o componente ou aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0747d-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="0747d-113">O .NET Framework fornece suporte para execução lado a lado, o que permite que várias versões de um assembly ou aplicativo a ser instalado no mesmo computador ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="0747d-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="0747d-114">Como várias versões podem ser instaladas simultaneamente, os aplicativos gerenciados podem selecionar qual versão usar sem afetar os aplicativos que usam uma versão diferente.</span><span class="sxs-lookup"><span data-stu-id="0747d-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="0747d-115">Por padrão, durante a instalação do .NET Framework versão 1.1, todos os aplicativos ASP.NET existentes são automaticamente reconfigurados para usar a versão mais recente do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="0747d-116">Se você não quiser seus aplicativos ASP.NET 1.1 do .NET Framework por padrão, clique em [aqui](#1) para saber como evitar isso durante a instalação.</span><span class="sxs-lookup"><span data-stu-id="0747d-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="0747d-117">Se você atualizar seu servidor Web para o .NET Framework 1.1 e deseja que um ou mais aplicativos Web para executar o .NET Framework 1.0, você precisará atualizar o mapa de Script do Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="0747d-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="0747d-118">O mapeamento de script é o mecanismo para mapear a extensão de arquivo. aspx para um aplicativo Web específico para uma versão do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="0747d-119">Clique em [aqui](#2) para aprender a mapear um aplicativo Web para uma versão específica do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="0747d-120">Você pode usar o Gerenciador de informações da Internet ou a ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe) para localizar qual versão do .NET Framework está em execução em um aplicativo Web específico.</span><span class="sxs-lookup"><span data-stu-id="0747d-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="0747d-121">Clique em [aqui](#3) para aprender a localizar a versão do .NET Framework que um site da Web está usando.</span><span class="sxs-lookup"><span data-stu-id="0747d-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="0747d-122">Uma consideração de importação ao migrar para o .NET Framework 1.1 é que cada versão do .NET Framework usa seu próprio arquivo Machine. config.</span><span class="sxs-lookup"><span data-stu-id="0747d-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="0747d-123">Como resultado, se um administrador da Web tiver feito alterações no arquivo Machine. config, essas alterações devem ser migradas para o arquivo Machine. config do .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="0747d-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="0747d-124">Manter o mapeamento de seu aplicativo Web para o .NET Framework 1.0 durante a instalação</span><span class="sxs-lookup"><span data-stu-id="0747d-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="0747d-125">Por padrão, todos os aplicativos ASP.NET existentes são automaticamente reconfigurados durante a instalação para usar a versão mais recente do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="0747d-126">Usando a versão mais recente do .NET Framework, os aplicativos podem tirar total proveito das melhorias e novos recursos incluídos na nova versão.</span><span class="sxs-lookup"><span data-stu-id="0747d-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="0747d-127">Ao mesmo tempo, o administrador da Web, que talvez queiram controle granular sobre quais aplicativos são atualizados, pode impedir que o remapeamento automático de todos os aplicativos ASP.NET existentes durante a instalação do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="0747d-128">Para evitar o remapeamento automático de todo o aplicativo ASP.NET para a versão mais recente do .NET Framework, o administrador da Web pode usar a opção de linha de comando /noaspupgrade com o programa de instalação Dotnetfx.exe.</span><span class="sxs-lookup"><span data-stu-id="0747d-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="0747d-129">**Para evitar o remapeamento total do aplicativo ASP.NET para a versão mais recente**</span><span class="sxs-lookup"><span data-stu-id="0747d-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="0747d-130">Vá para **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="0747d-130">Go to **Start**.</span></span>
2. <span data-ttu-id="0747d-131">Clique em **executar**.</span><span class="sxs-lookup"><span data-stu-id="0747d-131">Click on **run**.</span></span>
3. <span data-ttu-id="0747d-132">Digite **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0747d-132">Type **cmd**.</span></span>
4. <span data-ttu-id="0747d-133">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="0747d-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="0747d-134">No prompt de comando, digite a seguinte linha para iniciar a instalação do .NET Framework: **/c Dotnetfx.exe: "instalar /noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="0747d-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="0747d-135">Clique em **Sim** na instalação do Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="0747d-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="0747d-136">Isso iniciará o processo de instalação do .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="0747d-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="0747d-137">Mapa de um aplicativo Web para uma versão específica do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="0747d-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="0747d-138">Cada versão do .NET Framework inclui uma versão da ferramenta de registro de IIS do ASP.NET (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="0747d-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="0747d-139">Essa ferramenta permite aos administradores especificar que um aplicativo Web ser executado em uma versão específica do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="0747d-140">Isso é chamado de mapeamento de um aplicativo Web para uma versão do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0747d-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="0747d-141">Os administradores devem selecionar o Aspnet\_regiis.exe que corresponde à versão do .NET Framework que será associado com o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="0747d-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="0747d-142">Por exemplo, um administrador que deseja especificar que um site da Web usa o .NET Framework 1.1 deve usar o Aspnet\_regiis.exe que vem com o .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="0747d-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="0747d-143">O Aspnet\_regiis.exe para a versão 1.0 está localizado em:</span><span class="sxs-lookup"><span data-stu-id="0747d-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="0747d-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="0747d-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="0747d-145">O Aspnet\_regiis.exe para a versão 1,1 está localizado em:</span><span class="sxs-lookup"><span data-stu-id="0747d-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="0747d-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="0747d-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="0747d-147">O Aspnet\_regiis.exe fornece duas opções para um aplicativo Web de mapeamento de script:</span><span class="sxs-lookup"><span data-stu-id="0747d-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="0747d-148">**-s** define o mapa de script no caminho e no seu filho diretórios.</span><span class="sxs-lookup"><span data-stu-id="0747d-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="0747d-149">**-sn** define o mapa de script no caminho apenas.</span><span class="sxs-lookup"><span data-stu-id="0747d-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="0747d-150">O caminho define o caminho da Web aplicativo IIS metadados, que é definido na forma de W3SVC/raiz / {WebSiteNumber} / {aplicativo\_nome}.</span><span class="sxs-lookup"><span data-stu-id="0747d-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="0747d-151">Por exemplo, para um aplicativo Web chamado Portal localizado no site da Web padrão, o caminho da metabase é W3SVC/1/raiz/Portal.</span><span class="sxs-lookup"><span data-stu-id="0747d-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="0747d-152">Observe que você também pode usar uma ferramenta chamada Editor de Metabase para obter o caminho de metabase.</span><span class="sxs-lookup"><span data-stu-id="0747d-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="0747d-153">Você pode baixar essa ferramenta no site da Microsoft Support em [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="0747d-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="0747d-154">Executar Aspnet\_regiis.exe -s W3SVC/1/raiz/Portal para atualizar o portal do IIS de mapa e seu subapplication script.</span><span class="sxs-lookup"><span data-stu-id="0747d-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="0747d-155">Executar Aspnet\_regiis.exe -sn W3SVC/1/raiz/Portal para atualizar o script do IIS portal mapear, sem afetar os aplicativos no portal? subdiretórios s.</span><span class="sxs-lookup"><span data-stu-id="0747d-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="0747d-156">Localizar a versão do .NET Framework que está usando um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="0747d-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="0747d-157">Um administrador pode usar o Gerenciador de serviços de Internet para encontrar qual versão do .NET Framework é executado em um site da Web.</span><span class="sxs-lookup"><span data-stu-id="0747d-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="0747d-158">Versões diferentes do sistema operacional inicie o Gerenciador de serviços de Internet de forma diferente.</span><span class="sxs-lookup"><span data-stu-id="0747d-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="0747d-159">Para iniciar o Gerenciador de serviço, siga as etapas listadas abaixo.</span><span class="sxs-lookup"><span data-stu-id="0747d-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="0747d-160">**Para iniciar o Gerenciador de serviço de Internet**</span><span class="sxs-lookup"><span data-stu-id="0747d-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="0747d-161">Vá para **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="0747d-161">Go to **Start**.</span></span>
2. <span data-ttu-id="0747d-162">Clique em **executar**.</span><span class="sxs-lookup"><span data-stu-id="0747d-162">Click on **run**.</span></span>
3. <span data-ttu-id="0747d-163">Tipo de **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="0747d-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="0747d-164">No Gerenciador de serviço de Internet, selecione o aplicativo Web cuja versão do .NET Framework que você deseja saber.</span><span class="sxs-lookup"><span data-stu-id="0747d-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="0747d-165">Clique com botão direito no aplicativo Web e, em seguida, clique em **propriedades.**</span><span class="sxs-lookup"><span data-stu-id="0747d-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="0747d-166">Na janela Propriedades, selecione **configuração.**</span><span class="sxs-lookup"><span data-stu-id="0747d-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="0747d-167">Na tabela de mapeamento do aplicativo, selecione **. aspx**e clique em **editar**.</span><span class="sxs-lookup"><span data-stu-id="0747d-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="0747d-168">Dos **executável** caixa de texto, examine o diretório de versão ao rolar.</span><span class="sxs-lookup"><span data-stu-id="0747d-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="0747d-169">Se o diretório de versão é v.1.1.4322, o aplicativo é mapeado para o .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="0747d-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="0747d-170">Por outro lado, se o diretório de versão é v1.0.3705, o aplicativo é mapeado para o .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="0747d-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
