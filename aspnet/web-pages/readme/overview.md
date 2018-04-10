---
uid: web-pages/readme/overview
title: Leiame do WebMatrix | Microsoft Docs
author: rick-anderson
description: O WebMatrix e o Leiame do ASP.NET páginas da Web (Razor) versão 1.0
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="webmatrix-readme"></a><span data-ttu-id="bf554-103">Leiame do WebMatrix</span><span class="sxs-lookup"><span data-stu-id="bf554-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="bf554-104">13 de janeiro de 2011</span><span class="sxs-lookup"><span data-stu-id="bf554-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="bf554-105">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="bf554-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="bf554-106">Este arquivo Leiame aplica-se para a 1.0 versão do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bf554-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="bf554-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="bf554-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="bf554-108">Instalação</span><span class="sxs-lookup"><span data-stu-id="bf554-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="bf554-109">Como publicar aplicativos</span><span class="sxs-lookup"><span data-stu-id="bf554-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="bf554-110">Problemas e alterações</span><span class="sxs-lookup"><span data-stu-id="bf554-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="bf554-111">Instalação do WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="bf554-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="bf554-112">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf554-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="bf554-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="bf554-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="bf554-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="bf554-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="bf554-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="bf554-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="bf554-116">Instalando aplicativos</span><span class="sxs-lookup"><span data-stu-id="bf554-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="bf554-117">Publicação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="bf554-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="bf554-118">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="bf554-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="bf554-119">Visão geral</span><span class="sxs-lookup"><span data-stu-id="bf554-119">Overview</span></span>

> <span data-ttu-id="bf554-120">Microsoft WebMatrix 1.0 é uma pilha de desenvolvimento gratuito da web que é instalado em minutos.</span><span class="sxs-lookup"><span data-stu-id="bf554-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="bf554-121">Ela integra um servidor web com o banco de dados e estruturas para criar uma única experiência integrada de programação.</span><span class="sxs-lookup"><span data-stu-id="bf554-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="bf554-122">Você pode usar o WebMatrix para simplificar a maneira de código, testar e publicar o seu próprio site ASP.NET ou PHP, ou você pode usar o WebMatrix para iniciar um novo site usando aplicativos de código aberto populares como DotNetNuke, Umbraco, WordPress ou Joomla.</span><span class="sxs-lookup"><span data-stu-id="bf554-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="bf554-123">O WebMatrix usa o mesmo servidor de aplicativos web, o mecanismo de banco de dados e o ambiente de estruturas que executará o seu site na internet, o que faz a transição do desenvolvimento para produção simples e direta.</span><span class="sxs-lookup"><span data-stu-id="bf554-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="bf554-124">Instalação</span><span class="sxs-lookup"><span data-stu-id="bf554-124">Installation</span></span>

> <span data-ttu-id="bf554-125">Para instalar o WebMatrix 1.0, você deve primeiro instalar o [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="bf554-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="bf554-126">Depois de instalar o Web Platform Installer, você pode usá-lo para instalar o WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bf554-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="bf554-127">Se você tiver problemas durante a instalação, consulte [Solucionando problemas com o Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="bf554-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="bf554-128">Como publicar aplicativos</span><span class="sxs-lookup"><span data-stu-id="bf554-128">How to Publish Applications</span></span>

> <span data-ttu-id="bf554-129">Consulte [instruções passo a passo para publicar aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="bf554-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="bf554-130">Problemas e alterações</span><span class="sxs-lookup"><span data-stu-id="bf554-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="bf554-131">Problemas de instalação 1.0 do WebMatrix</span><span class="sxs-lookup"><span data-stu-id="bf554-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="bf554-132">Problema: O WebMatrix 1.0 está disponível somente nas plataformas que dão suporte ao Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="bf554-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="bf554-133">O .NET Framework versão 4 é necessário para o WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bf554-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="bf554-134">Em alguns casos, o instalador do WebMatrix 1.0 permitirá tente instalar em uma plataforma que não faz parte do conjunto de configuração com suporte.</span><span class="sxs-lookup"><span data-stu-id="bf554-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="bf554-135">Em particular, o Windows Vista sem a atualização do SP1 permitirá que você iniciar a instalação do WebMatrix, mas o componente do .NET Framework 4 falhará e bloquear a instalação.</span><span class="sxs-lookup"><span data-stu-id="bf554-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="bf554-136">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-136">**Workaround**</span></span>  
> <span data-ttu-id="bf554-137">Instale em uma plataforma com suporte, que inclui:</span><span class="sxs-lookup"><span data-stu-id="bf554-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="bf554-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="bf554-138">Windows 7</span></span>
> - <span data-ttu-id="bf554-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="bf554-139">Windows Server 2008</span></span>
> - <span data-ttu-id="bf554-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="bf554-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="bf554-141">Windows Vista SP1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="bf554-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="bf554-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="bf554-142">Windows XP SP3</span></span>
> - <span data-ttu-id="bf554-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="bf554-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="bf554-144">Problema: Não é possível instalar o WebMatrix 1.0 se o Microsoft Visual Studio 2008 está instalado sem o Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="bf554-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="bf554-145">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-145">**Workaround**</span></span>  
> <span data-ttu-id="bf554-146">Instalar [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) do Centro de Download da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf554-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="bf554-147">Problema: Alguns assemblies para o SQL Server Compact 4.0 não estão instalados no GAC</span><span class="sxs-lookup"><span data-stu-id="bf554-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="bf554-148">Os assemblies gerenciados para o SQL Server Compact 4.0 não são colocados no cache de assembly global (GAC) quando você instala o SQL Server Compact 4.0 em um computador de 64 bits e o computador tem apenas o .NET Framework 3.5 SP1 Client Profile instalado.</span><span class="sxs-lookup"><span data-stu-id="bf554-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="bf554-149">Os módulos gerenciados que não estão instalados no GAC são:</span><span class="sxs-lookup"><span data-stu-id="bf554-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="bf554-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span><span class="sxs-lookup"><span data-stu-id="bf554-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="bf554-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="bf554-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="bf554-152">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-152">**Workaround**</span></span>  
> <span data-ttu-id="bf554-153">Desinstalar o SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="bf554-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="bf554-154">Baixe e instale a versão completa do .NET Framework 3.5 SP1 no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="bf554-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="bf554-155">Microsoft .NET Framework 3.5 Service pack 1 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="bf554-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="bf554-156">Em seguida, reinstale o SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="bf554-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="bf554-157">Problema: Não é possível desinstalar o SQL Server Compact usando a linha de comando</span><span class="sxs-lookup"><span data-stu-id="bf554-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="bf554-158">A desinstalação do SQL Server Compact usando opções de linha de comando não funciona nesta versão.</span><span class="sxs-lookup"><span data-stu-id="bf554-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="bf554-159">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-159">**Workaround**</span></span>  
> <span data-ttu-id="bf554-160">Use *programas e recursos* no painel de controle para desinstalar o Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="bf554-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="bf554-161">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf554-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="bf554-162">Esta seção do documento descreve novos recursos, alterações e problemas conhecidos com a 1.0 versão do ASP.NET Web Pages com sintaxe do Razor.</span><span class="sxs-lookup"><span data-stu-id="bf554-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="bf554-163">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="bf554-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="bf554-164">Alterações</span><span class="sxs-lookup"><span data-stu-id="bf554-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="bf554-165">Problemas</span><span class="sxs-lookup"><span data-stu-id="bf554-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="bf554-166">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="bf554-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="bf554-167">Novo: Configuração adicionado para desabilitar o Gerenciador de pacotes</span><span class="sxs-lookup"><span data-stu-id="bf554-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="bf554-168">Um novo `asp:AdminManagerEnabled` chave está disponível para o `<appSettings>` elemento o *Web. config* arquivo, que permite que você desabilite totalmente o Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="bf554-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="bf554-169">O valor padrão para esse elemento for true, o que significa que, se ele não está incluído no *Web. config* arquivo, o Gerenciador de pacotes está habilitado.</span><span class="sxs-lookup"><span data-stu-id="bf554-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="bf554-170">Para desabilitar o Gerenciador de pacote, adicione o seguinte elemento para o *Web. config* arquivo na raiz do site:</span><span class="sxs-lookup"><span data-stu-id="bf554-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="bf554-171">Alterações</span><span class="sxs-lookup"><span data-stu-id="bf554-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="bf554-172">Alterar: a chave de "webPages:AdminFolderVirtualPath" renomeada para "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="bf554-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="bf554-173">O `webPages:AdminFolderVirtualPath` chave que pode ser adicionado para o *Web. config* foi renomeado um arquivo para especificar o local do Gerenciador de pacotes para usar o `asp:` namespace em vez do `webPages` namespace.</span><span class="sxs-lookup"><span data-stu-id="bf554-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="bf554-174">Se você usou esse elemento, você deverá renomeá-la no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="bf554-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="bf554-175">Problemas conhecidos</span><span class="sxs-lookup"><span data-stu-id="bf554-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="bf554-176">Problema: Senhas para usuários de associação não reconhecidos</span><span class="sxs-lookup"><span data-stu-id="bf554-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="bf554-177">O algoritmo para criar e armazenar senhas de membros (logon) foi alterado para ser mais seguro.</span><span class="sxs-lookup"><span data-stu-id="bf554-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="bf554-178">Como resultado, as senhas armazenadas para membros (usuários) criados em versões Beta do ASP.NET Razor não serão reconhecidas.</span><span class="sxs-lookup"><span data-stu-id="bf554-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="bf554-179">**Solução alternativa** se o site ainda não foi colocado em produção, remova os registros de usuário do banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="bf554-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="bf554-180">Se o banco de dados ao vivo, programaticamente gerar novamente as senhas existentes no banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="bf554-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="bf554-181">Problema: Um comportamento inesperado quando uma tabela de usuário personalizada para associação</span><span class="sxs-lookup"><span data-stu-id="bf554-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="bf554-182">Para inicializar o provedor de associação para um site ASP.NET Razor, você deve chamar o `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="bf554-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="bf554-183">(No WebMatrix, o modelo de Site inicial inclui uma chamada para este método no  *\_AppStart.cshtml* arquivo.) Se o `autoCreateTables` parâmetro deste método é definido como true (por padrão, ele é definido como true no modelo de Site inicial), e se um nome de tabela não reconhecido é passado para o método (o segundo parâmetro), o método não gerará um erro.</span><span class="sxs-lookup"><span data-stu-id="bf554-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="bf554-184">Em vez disso, ele cria automaticamente a tabela.</span><span class="sxs-lookup"><span data-stu-id="bf554-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="bf554-185">Isso pode ser um problema se você pretende usar uma tabela de usuário personalizado para associação, mas passar o nome da tabela incorreta para o `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="bf554-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="bf554-186">Como o método não por padrão gera um erro se a tabela que você especificar não existir, e em vez disso, ele cria uma nova tabela, o aplicativo pode parecem estar funcionando.</span><span class="sxs-lookup"><span data-stu-id="bf554-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="bf554-187">Entretanto, o código do aplicativo que se baseia em sua tabela de usuário personalizada (e em campos), eventualmente, pode falhar com erros inesperados.</span><span class="sxs-lookup"><span data-stu-id="bf554-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="bf554-188">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-188">**Workaround**</span></span>  
> <span data-ttu-id="bf554-189">Certifique-se de que o nome passado a `InitializeDatabaseConnection` correspondências de método, o perfil de usuário no banco de dados de associação de tabela, ou verifique se o `autoCreateTables` parâmetro for definido como false.</span><span class="sxs-lookup"><span data-stu-id="bf554-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="bf554-190">Problema: Mensagem de erro "o módulo de administração requer acesso ao ~/App\_dados"</span><span class="sxs-lookup"><span data-stu-id="bf554-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="bf554-191">Em algumas circunstâncias, tentando criar os usuários ou trabalhar com o sistema de associação ASP.NET pode causar a página para exibir o erro *o módulo de administração requer acesso ao ~/App\_dados*.</span><span class="sxs-lookup"><span data-stu-id="bf554-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="bf554-192">Isso ocorre se a conta que o IIS ou IIS Express está sendo executado em não tem permissões para criar e gravar o *aplicativo\_dados* pasta sob a raiz do site.</span><span class="sxs-lookup"><span data-stu-id="bf554-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="bf554-193">**Solução alternativa** criar manualmente um *aplicativo\_dados* pasta para o site.</span><span class="sxs-lookup"><span data-stu-id="bf554-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="bf554-194">Certifique-se de que a conta do Windows que o aplicativo é executado (normalmente o serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo em subpastas, como aplicativo\_dados.</span><span class="sxs-lookup"><span data-stu-id="bf554-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="bf554-195">Informações mais detalhadas estão disponíveis no artigo da Base de Conhecimento [problemas com o SQL Server Express instanciação de usuário e a projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="bf554-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="bf554-196">Problema: "Falha ao gerar uma instância de usuário do SQL Server" Erro</span><span class="sxs-lookup"><span data-stu-id="bf554-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="bf554-197">Se um aplicativo Web do WebMatrix usa o SQL Server Express e IIS 7.5 está em execução no Windows 7 ou Windows Server 2008 R2, você poderá ver um erro que indica que o SQL Server não pode recuperar caminho do aplicativo local do usuário em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="bf554-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="bf554-198">**Solução alternativa** Certifique-se de que a conta do Windows que o aplicativo é executado (normalmente o serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e das subpastas como *aplicativo\_dados*.</span><span class="sxs-lookup"><span data-stu-id="bf554-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="bf554-199">Informações mais detalhadas estão disponíveis no artigo da Base de Conhecimento [problemas com o SQL Server Express instanciação de usuário e a projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="bf554-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="bf554-200">Problema: Os arquivos que contém os recursos do Gerenciador de pacotes ou senhas do Gerenciador de pacote são servable no IIS 6.0 e versões anteriores</span><span class="sxs-lookup"><span data-stu-id="bf554-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="bf554-201">Se você implantar um aplicativo de páginas da Web do ASP.NET (Razor) que foi criado com a versão RC2, e se o aplicativo contém um *Password* ou *packagesources.txt* de arquivos em */App\_ Dados/admin*, IIS 6.0 servirá o arquivo se solicitado, potencialmente expor as senhas para sua instância do Gerenciador de pacote.</span><span class="sxs-lookup"><span data-stu-id="bf554-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="bf554-202">**Solução alternativa** renomear o *Password* ou *packagesources.txt* o arquivo para *password.config* ou *packagesources.config*. Por padrão, o IIS 6.0 não atende aos arquivos que têm o *. config* extensão.</span><span class="sxs-lookup"><span data-stu-id="bf554-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="bf554-203">(No IIS 7, não há arquivos de *aplicativo\_dados* pasta são atendidos, portanto você não precisa renomear os arquivos.)</span><span class="sxs-lookup"><span data-stu-id="bf554-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="bf554-204">Problema: Desinstalar pacotes instalados usando a versão Beta 3 não remove completamente os componentes de pacote</span><span class="sxs-lookup"><span data-stu-id="bf554-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="bf554-205">Se você instalou um pacote usando o Gerenciador de pacote na versão Beta 3 e, em seguida, tente desinstalá-lo usando a versão atual, o pacote não foi completamente desinstalado.</span><span class="sxs-lookup"><span data-stu-id="bf554-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="bf554-206">Usando o Gerenciador de pacote **desinstalação** botão remove alguns componentes, mas deixa o código da biblioteca do pacote e não atualiza o *package.config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="bf554-207">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-207">**Workaround** </span></span>  
> <span data-ttu-id="bf554-208">Execute estas etapas:</span><span class="sxs-lookup"><span data-stu-id="bf554-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="bf554-209">Excluir o *aplicativo\_Data\packages* pasta.</span><span class="sxs-lookup"><span data-stu-id="bf554-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="bf554-210">Isso remove todos os pacotes.</span><span class="sxs-lookup"><span data-stu-id="bf554-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="bf554-211">Excluir o *Packages* arquivo na raiz do site.</span><span class="sxs-lookup"><span data-stu-id="bf554-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="bf554-212">Problema: No Visual Studio, chamar o Gerenciador de pacote baseado na web usa o aplicativo offline</span><span class="sxs-lookup"><span data-stu-id="bf554-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="bf554-213">Se você estiver trabalhando no Visual Studio (não o WebMatrix) e usar o  *\_admin* funcionalidade para iniciar o Gerenciador de pacotes, o Visual Studio usa o aplicativo offline e envia o *aplicativo\_ offline.htm* para a raiz do site, que interrompe a capacidade de usar o Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="bf554-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="bf554-214">Embora geralmente você veria esse comportamento ao usar a interface do Gerenciador de pacote baseado na web, o mesmo comportamento ocorre se você adiciona, remove ou modificar arquivos de *aplicativo\_dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="bf554-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="bf554-215">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-215">**Workaround** </span></span>  
> <span data-ttu-id="bf554-216">Para trabalhar com pacotes no Visual Studio, use a extensão do NuGet em vez do Gerenciador de pacote baseado na web.</span><span class="sxs-lookup"><span data-stu-id="bf554-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="bf554-217">Para obter informações, consulte o [NuGet documentação](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="bf554-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="bf554-218">Se você estiver trabalhando com outros arquivos de *aplicativo\_dados* pasta, considere manter os arquivos em outro lugar para evitar esse problema.</span><span class="sxs-lookup"><span data-stu-id="bf554-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="bf554-219">Se não for prático, exclua o *aplicativo\_offline.htm* arquivo manualmente ou esperar até que o site de volta a ficar online automaticamente (por padrão, após 30 segundos).</span><span class="sxs-lookup"><span data-stu-id="bf554-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="bf554-220">Problema: Visual Studio IntelliSense e projeto modelos disponíveis apenas no ASP.NET MVC versão 3</span><span class="sxs-lookup"><span data-stu-id="bf554-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="bf554-221">Instalar o ASP.NET Web Pages não também instala ferramentas para Visual Studio, como modelos de projeto e IntelliSense para aplicativos ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="bf554-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="bf554-222">**Solução alternativa** para usar os modelos de projeto e IntelliSense para aplicativos de páginas da Web ASP.NET no Visual Studio, instalar o ASP.NET MVC 3 RC por meio do Web Platform Installer ou [instalador autônomo](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="bf554-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="bf554-223">Problema: Ler feeds ou outros dados externos por meio de um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="bf554-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="bf554-224">Se o servidor que executa o site estiver em um servidor proxy, você talvez precise configurar informações de proxy de *Web. config* arquivo a fim de ser capaz de ler as informações que vêm de fora do site.</span><span class="sxs-lookup"><span data-stu-id="bf554-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="bf554-225">Por exemplo, se você usar o `ReCaptcha` auxiliar, o auxiliar se comunica com o serviço de reCAPTCHA, mas pode ser bloqueado por seu servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="bf554-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="bf554-226">Da mesma forma, os feeds que são usados em páginas de Web do ASP.NET, como o feed usado pelo Gerenciador de pacote, podem exigir a configuração de proxy.</span><span class="sxs-lookup"><span data-stu-id="bf554-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="bf554-227">Se você tiver problemas para trabalhar com um serviço externo ou trabalhar com o pacote de feed, coloque os seguintes elementos raiz do aplicativo *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="bf554-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="bf554-228">Para obter mais informações sobre como configurar um servidor proxy, consulte [ &lt;proxy&gt; (configurações de rede) do elemento](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="bf554-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="bf554-229">Problema: Desinstalar o .NET Framework versão 4 desabilita a ASP.NET Web Pages com sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="bf554-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="bf554-230">Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, o ASP.NET Web Pages com sintaxe do Razor são desabilitados.</span><span class="sxs-lookup"><span data-stu-id="bf554-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="bf554-231">As páginas com a *. cshtml* extensão não são executados corretamente.</span><span class="sxs-lookup"><span data-stu-id="bf554-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="bf554-232">Páginas da Web ASP.NET registra um assembly na raiz da máquina *Web. config* arquivo e remover o .NET Framework remove esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="bf554-233">Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly de páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf554-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="bf554-234">**Solução alternativa** após a reinstalação do .NET Framework, reinstale o ASP.NET Web Pages com sintaxe do Razor.</span><span class="sxs-lookup"><span data-stu-id="bf554-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="bf554-235">Isso adiciona o elemento a seguir para o *Web. config* arquivo na raiz do computador, que normalmente é no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="bf554-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="bf554-236">Problema: URLs sem extensão não localizar arquivos de.cshtml/.vbhtml no IIS 7 ou IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="bf554-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="bf554-237">No IIS 7 ou IIS 7.5, solicitações com uma URL semelhante à seguinte não serão possível localizar as páginas que têm o *. cshtml* ou *. vbhtml* extensão:</span><span class="sxs-lookup"><span data-stu-id="bf554-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="bf554-238">O problema ocorre porque a regravação de URL não está habilitado por padrão para IIS 7 ou IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="bf554-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="bf554-239">O cenário mais provável é que você não vir o problema ao testar localmente usando o IIS Express, mas você enfrentar ao implantar seu site em um site de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="bf554-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="bf554-240">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="bf554-241">Se você tem controle sobre o computador do servidor, no computador do servidor de instalar a atualização descrita no [uma atualização está disponível que permite que determinados manipuladores do IIS 7.0 ou IIS 7.5 para tratar as solicitações cujas URLs não terminam com um período](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="bf554-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="bf554-242">Se você não tem controle sobre o computador do servidor (por exemplo, você estiver implantando em um site de hospedagem), adicione o seguinte para o site *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="bf554-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="bf554-243">Problema: Implantando um aplicativo em um computador que não tenha o SQL Server Compact instalado</span><span class="sxs-lookup"><span data-stu-id="bf554-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="bf554-244">Aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador em que o SQL Server Compact não está instalado.</span><span class="sxs-lookup"><span data-stu-id="bf554-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="bf554-245">Microsoft WebMatrix 1.0 executa apropriada e copia esses binários para você automaticamente *Web. config* transformações de arquivos.</span><span class="sxs-lookup"><span data-stu-id="bf554-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="bf554-246">**Solução alternativa** se você precisa copiar esses arquivos e fazer a *Web. config* alterações de arquivo manualmente, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="bf554-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="bf554-247">Copie os assemblies do mecanismo de banco de dados para o *Bin* pasta (e subpastas) do aplicativo no computador de destino:</span><span class="sxs-lookup"><span data-stu-id="bf554-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="bf554-248">Cópia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="bf554-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="bf554-249">**to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="bf554-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="bf554-250">Cópia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>para</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="bf554-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="bf554-251">Cópia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>para</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="bf554-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="bf554-252">Na pasta raiz do site, crie ou abra um *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="bf554-253">(No WebMatrix 1.0, esse tipo de arquivo está disponível se você clicar em **todos os** no **escolher um tipo de arquivo** caixa de diálogo.)</span><span class="sxs-lookup"><span data-stu-id="bf554-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="bf554-254">Adicione o seguinte elemento como um filho de `<configuration>` elemento (não dentro a `<system.web>` elemento):</span><span class="sxs-lookup"><span data-stu-id="bf554-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="bf554-255">Problema: "Database" e "WebGrid" auxiliares não funcionam no nível de confiança média no Visual Basic</span><span class="sxs-lookup"><span data-stu-id="bf554-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="bf554-256">Se você estiver usando o Visual Basic (criando *. vbhtml* arquivos), o `Database` e `WebGrid` auxiliares não funcionará se o aplicativo está definido para usar o nível de confiança média.</span><span class="sxs-lookup"><span data-stu-id="bf554-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="bf554-257">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-257">**Workaround**</span></span>  
> <span data-ttu-id="bf554-258">Se você usar o Visual Studio 2010, você pode resolver esse problema instalando a versão do Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="bf554-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="bf554-259">Até que a versão final da versão do SP1 está disponível, você pode baixar a versão Beta do SP1 a partir de [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) página no Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="bf554-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="bf554-260">Se isso não é prático, ou se você não usar o Visual Studio 2010, você pode temporariamente definido o aplicativo para usar a confiança total.</span><span class="sxs-lookup"><span data-stu-id="bf554-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="bf554-261">Problema: Os recursos de "ApplicationPart" são acessíveis</span><span class="sxs-lookup"><span data-stu-id="bf554-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="bf554-262">Se um assembly contém objetos que deriva de `ApplicationPart` classe, que os recursos do assembly são expostos pelo `ResourceRouteHandler` classe.</span><span class="sxs-lookup"><span data-stu-id="bf554-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="bf554-263">Por exemplo, considere a seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="bf554-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="bf554-264">Essa solicitação baixa todas as cadeias de caracteres de recurso no *System.Web.WebPages.Administration.dll* assembly.</span><span class="sxs-lookup"><span data-stu-id="bf554-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="bf554-265">Todos os recursos incorporados (mesmo aqueles que não se destina a ser usado como o conteúdo estático) são baixados.</span><span class="sxs-lookup"><span data-stu-id="bf554-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="bf554-266">Se os recursos inseridos contêm informações confidenciais, isso pode representar um risco à segurança.</span><span class="sxs-lookup"><span data-stu-id="bf554-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="bf554-267">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-267">**Workaround** </span></span>  
> <span data-ttu-id="bf554-268">Se você criar um **ApplicationPart** de objeto, certifique-se de que os recursos incorporados associado que **ApplicationPart** assembly do objeto não contêm informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="bf554-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="bf554-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="bf554-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="bf554-270">Para obter informações sobre problemas de instalação para o WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.</span><span class="sxs-lookup"><span data-stu-id="bf554-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="bf554-271">Esta seção do documento descreve problemas conhecidos para o ambiente de desenvolvimento do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bf554-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="bf554-272">Problema: As alterações no nome de usuário ou senha de uma cadeia de caracteres de conexão de banco de dados em um arquivo Web. config não são refletidas no espaço de trabalho de bancos de dados</span><span class="sxs-lookup"><span data-stu-id="bf554-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="bf554-273">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="bf554-274">No *Web. config* de arquivo, altere o nome do banco de dados na cadeia de conexão (por exemplo, adicione "1" a ele).</span><span class="sxs-lookup"><span data-stu-id="bf554-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="bf554-275">Salve o *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="bf554-276">Clique em **bancos de dados** e atualizar.</span><span class="sxs-lookup"><span data-stu-id="bf554-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="bf554-277">Alterar o nome do banco de dados na cadeia de conexão do *Web. config* arquivo para o nome do banco de dados original.</span><span class="sxs-lookup"><span data-stu-id="bf554-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="bf554-278">Salve o *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="bf554-279">Clique em **bancos de dados** e atualizar.</span><span class="sxs-lookup"><span data-stu-id="bf554-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="bf554-280">Problema: Pastas criadas pelo WebMatrix não podem ser excluídas</span><span class="sxs-lookup"><span data-stu-id="bf554-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="bf554-281">Se o WebMatrix é executado usando permissões elevadas (ou seja, você iniciou o WebMatrix usando o **executar como administrador** opção no Windows), as pastas criadas pelo WebMatrix não podem ser excluídas usando o Windows Explorer.</span><span class="sxs-lookup"><span data-stu-id="bf554-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="bf554-282">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-282">**Workaround**</span></span>  
> <span data-ttu-id="bf554-283">Execute o Windows Explorer usando permissões elevadas.</span><span class="sxs-lookup"><span data-stu-id="bf554-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="bf554-284">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="bf554-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="bf554-285">No Windows, clique em **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="bf554-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="bf554-286">Digite "Windows Explorer" e clique na entrada de **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="bf554-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="bf554-287">Clique em **executar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="bf554-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="bf554-288">Você pode excluir as pastas.</span><span class="sxs-lookup"><span data-stu-id="bf554-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="bf554-289">Problema: O WebMatrix 1.0 é capaz de realizar certas tarefas que exigem elevação</span><span class="sxs-lookup"><span data-stu-id="bf554-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="bf554-290">O WebMatrix 1.0 é capaz de realizar certas tarefas que exigem elevação, como ao instalar componentes adicionais nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="bf554-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="bf554-291">No Windows Vista ou Windows 7, você está conectado com uma conta que não tem privilégios administrativos e controle de conta de usuário (UAC) está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="bf554-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="bf554-292">Você está usando o Microsoft Windows XP ou Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="bf554-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="bf554-293">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-293">**Workaround**</span></span>  
> <span data-ttu-id="bf554-294">A maioria das tarefas no WebMatrix 1.0 não requerem permissão administrativa.</span><span class="sxs-lookup"><span data-stu-id="bf554-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="bf554-295">Para aqueles que fazer, você pode executar a operação como um administrador ou siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="bf554-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="bf554-296">No Windows Vista ou Windows 7, habilite o UAC.</span><span class="sxs-lookup"><span data-stu-id="bf554-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="bf554-297">No Windows XP, adicione o usuário ao grupo de segurança Administradores.</span><span class="sxs-lookup"><span data-stu-id="bf554-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="bf554-298">Problema: "Da Galeria de Web Site" está desabilitada</span><span class="sxs-lookup"><span data-stu-id="bf554-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="bf554-299">O **Site da Galeria de Web** opção será desabilitada se o Web Platform Installer 3.0 não está instalado.</span><span class="sxs-lookup"><span data-stu-id="bf554-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="bf554-300">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-300">**Workaround**</span></span>  
> <span data-ttu-id="bf554-301">Instalar o [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="bf554-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="bf554-302">Problema: O Google Chrome não está disponível como uma opção de execução</span><span class="sxs-lookup"><span data-stu-id="bf554-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="bf554-303">Google Chrome não é exibido na lista de navegadores em **executar** no **início** guia.</span><span class="sxs-lookup"><span data-stu-id="bf554-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="bf554-304">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-304">**Workaround**</span></span>  
> <span data-ttu-id="bf554-305">Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão do Windows.</span><span class="sxs-lookup"><span data-stu-id="bf554-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="bf554-306">Como alternativa, inicie o Google Chrome, clique no *personalizar e controle Google Chrome* menu, clique em *opções*e, em seguida, clique em *Verifique Google Chrome navegador padrão*.</span><span class="sxs-lookup"><span data-stu-id="bf554-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="bf554-307">Problema: A caixa de diálogo "Foreign Key" não permite a inserção de uma chave primária</span><span class="sxs-lookup"><span data-stu-id="bf554-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="bf554-308">O **chave estrangeira** caixa de diálogo não permitem que você insira o nome da chave primário da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="bf554-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="bf554-309">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-309">**Workaround**</span></span>  
> <span data-ttu-id="bf554-310">Isso é intencional.</span><span class="sxs-lookup"><span data-stu-id="bf554-310">This is intentional.</span></span> <span data-ttu-id="bf554-311">Você não precisa inserir o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="bf554-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="bf554-312">Problema: O IntelliSense não está disponível no WebMatrix para o Razor sintaxe, c# ou Visual Basic</span><span class="sxs-lookup"><span data-stu-id="bf554-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="bf554-313">Há suporte para o IntelliSense para HTML e CSS no WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bf554-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="bf554-314">No entanto, não está disponível para outros idiomas.</span><span class="sxs-lookup"><span data-stu-id="bf554-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="bf554-315">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-315">**Workaround** </span></span>  
> <span data-ttu-id="bf554-316">nenhuma.</span><span class="sxs-lookup"><span data-stu-id="bf554-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="bf554-317">Problema: O IntelliSense para HTML e CSS sugere elementos que não são apropriados contextualmente</span><span class="sxs-lookup"><span data-stu-id="bf554-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="bf554-318">IntelliSense para marcação no WebMatrix dá suporte a HTML usando o [esquema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e CSS usando o [esquema CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="bf554-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="bf554-319">Como IntelliSense baseia-se nesses esquemas específicos, determinadas marcas, atributos ou propriedades podem sugeridas que não são apropriados para a definição de estilo ou página atual.</span><span class="sxs-lookup"><span data-stu-id="bf554-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="bf554-320">Para HTML, ele também pode causar sugestões inesperados no conteúdo que pode ser interpretada como XHTML malformado (por exemplo, quando as marcas não forem fechadas).</span><span class="sxs-lookup"><span data-stu-id="bf554-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="bf554-321">Esse problema pode ser mais perceptível se o ponto de inserção está dentro de uma marca incompleta; Nesse caso, IntelliSense pode sugerir novas marcas de abertura ou oferecer outras sugestões incorretos.</span><span class="sxs-lookup"><span data-stu-id="bf554-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="bf554-322">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-322">**Workaround** </span></span>  
> <span data-ttu-id="bf554-323">Para HTML, certifique-se de que você está trabalhando em uma página XHTML bem formada e completa.</span><span class="sxs-lookup"><span data-stu-id="bf554-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="bf554-324">Para CSS, não há nenhuma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="bf554-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="bf554-325">Problema: O IntelliSense não é invocado durante a digitação</span><span class="sxs-lookup"><span data-stu-id="bf554-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="bf554-326">Às vezes, IntelliSense não pode ser chamado como HTML ou CSS está sendo inserido no editor.</span><span class="sxs-lookup"><span data-stu-id="bf554-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="bf554-327">Em particular, isso pode acontecer quando o ponto de inserção é diretamente ao lado do outro elemento ou no final de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="bf554-328">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-328">**Workaround** </span></span>  
> <span data-ttu-id="bf554-329">Certifique-se de que há espaço em branco em torno do ponto de inserção e se o ponto de inserção não está no final de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="bf554-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="bf554-330">Você também pode invocar IntelliSense manualmente, pressionando Ctrl + espaço.</span><span class="sxs-lookup"><span data-stu-id="bf554-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="bf554-331">Problema: Nenhuma interface do usuário está disponível para desabilitar o IntelliSense</span><span class="sxs-lookup"><span data-stu-id="bf554-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="bf554-332">O WebMatrix 1.0 não fornece nenhuma interface do usuário ou o gesto para desativar o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="bf554-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="bf554-333">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="bf554-333">**Workaround** </span></span>  
> <span data-ttu-id="bf554-334">Inicie o WebMatrix usando o comando a seguir, que inclui uma opção que desativa o IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="bf554-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="bf554-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="bf554-335">IIS Express</span></span>

<span data-ttu-id="bf554-336">O IIS Express tem seu próprio arquivo Leiame, que está disponível na seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="bf554-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="bf554-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="bf554-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="bf554-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="bf554-338">SQL Server Compact</span></span>

<span data-ttu-id="bf554-339">SQL Server Compact tem seu próprio arquivo Leiame, que está disponível na seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="bf554-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="bf554-340">Para obter informações sobre problemas que envolvem a instalação do SQL Server Compact como parte do WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.</span><span class="sxs-lookup"><span data-stu-id="bf554-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="bf554-341">Instalando aplicativos</span><span class="sxs-lookup"><span data-stu-id="bf554-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="bf554-342">Problema: Instalação de um aplicativo pode levar muito tempo se a pasta Meus documentos do usuário é redirecionada para um compartilhamento de rede</span><span class="sxs-lookup"><span data-stu-id="bf554-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="bf554-343">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-343">**Workaround**</span></span>  
> <span data-ttu-id="bf554-344">nenhuma.</span><span class="sxs-lookup"><span data-stu-id="bf554-344">None.</span></span> <span data-ttu-id="bf554-345">O aplicativo pode demorar um pouco para ser instalado, mas será instalado corretamente.</span><span class="sxs-lookup"><span data-stu-id="bf554-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="bf554-346">Publicação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="bf554-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="bf554-347">Problema: "necessário não foi possível adquirir as permissões" Erro ao publicar um banco de dados do SQL Compact</span><span class="sxs-lookup"><span data-stu-id="bf554-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="bf554-348">O WebMatrix não suporta completamente Implantando binários suporte para SQL Server Compact em um servidor que executa o .NET Framework versão 3.5 com uma configuração de confiança média.</span><span class="sxs-lookup"><span data-stu-id="bf554-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="bf554-349">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-349">**Workaround**</span></span>  
> <span data-ttu-id="bf554-350">É a solução preferencial instalar o .NET Framework 4 no servidor.</span><span class="sxs-lookup"><span data-stu-id="bf554-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="bf554-351">Como alternativa, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="bf554-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="bf554-352">Adicione os seguintes elementos para o `SecurityClasses` seção *Web\_MediumTrust.config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="bf554-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="bf554-353">Criar um novo conjunto de permissões no *Web\_MediumTrust.config* arquivo com as seguintes permissões:</span><span class="sxs-lookup"><span data-stu-id="bf554-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="bf554-354">Aplicar o conjunto de permissões para o SQL Server Compact, colocando os seguintes elementos *Web\_MediumTrust.config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="bf554-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="bf554-355">Problema: Aplicativos da web galeria e PhpBB exibem um erro "O serviço não está disponível" após a publicação</span><span class="sxs-lookup"><span data-stu-id="bf554-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="bf554-356">Em algumas circunstâncias, a publicação de um aplicativo faz com que um erro "serviço indisponível".</span><span class="sxs-lookup"><span data-stu-id="bf554-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="bf554-357">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-357">**Workaround**</span></span>  
> <span data-ttu-id="bf554-358">No WebMatrix, adicionar uma barra invertida (\) ao final do nome do servidor no **configurações de publicação** janela e, em seguida, publicar o aplicativo novamente.</span><span class="sxs-lookup"><span data-stu-id="bf554-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="bf554-359">Problema: Links e layout do site o Moodle são desfeitos após a publicação</span><span class="sxs-lookup"><span data-stu-id="bf554-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="bf554-360">Depois de publicar um aplicativo o Moodle, o aplicativo não funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="bf554-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="bf554-361">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-361">**Workaround**</span></span>  
> <span data-ttu-id="bf554-362">No WebMatrix, adicione uma barra (/) ao final do **nome do Site** campo o **configurações de publicação** janela e, em seguida, publicar o aplicativo novamente.</span><span class="sxs-lookup"><span data-stu-id="bf554-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="bf554-363">Problema: Publicar nopCommerce falhará com um erro de banco de dados</span><span class="sxs-lookup"><span data-stu-id="bf554-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="bf554-364">Publicação nopCommerce falhar e relata um erro de banco de dados como "inserir o nop\_tabela de log falhou."</span><span class="sxs-lookup"><span data-stu-id="bf554-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="bf554-365">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="bf554-366">No WebMatrix, clique em **executar** para iniciar o nopCommerce localmente.</span><span class="sxs-lookup"><span data-stu-id="bf554-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="bf554-367">Faça logon na página Administração.</span><span class="sxs-lookup"><span data-stu-id="bf554-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="bf554-368">Clique o **sistema** menu.</span><span class="sxs-lookup"><span data-stu-id="bf554-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="bf554-369">Clique o **Log** opção.</span><span class="sxs-lookup"><span data-stu-id="bf554-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="bf554-370">Clique o **Limpar Log** botão.</span><span class="sxs-lookup"><span data-stu-id="bf554-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="bf554-371">Publica nopCommerce novamente.</span><span class="sxs-lookup"><span data-stu-id="bf554-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="bf554-372">Problema: O Silverstripe CMS exibe um "Erro HTTP 500 PHP FCGI" quando você baixar um site publicado</span><span class="sxs-lookup"><span data-stu-id="bf554-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="bf554-373">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-373">**Workaround**</span></span>  
> <span data-ttu-id="bf554-374">Depois de clicar em **site publicado do Download**, ignorar `silverstripe-cache/manifest_main` na **Publicar visualização**.</span><span class="sxs-lookup"><span data-stu-id="bf554-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="bf554-375">Esse arquivo é usado para fins de cache e é específico para cada computador.</span><span class="sxs-lookup"><span data-stu-id="bf554-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="bf554-376">Problema: O Subtext exibe "Erro de servidor no aplicativo '/'" quando você baixar um site publicado</span><span class="sxs-lookup"><span data-stu-id="bf554-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="bf554-377">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-377">**Workaround**</span></span>  
> <span data-ttu-id="bf554-378">Abra o site *Web. config* de arquivo e substitua a ID de usuário e senha na cadeia de conexão de banco de dados com as credenciais de administrador do SQL Server (as credenciais "sa").</span><span class="sxs-lookup"><span data-stu-id="bf554-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="bf554-379">Como alternativa, siga estas etapas para conceder a conta de usuário que você está conectado com `db_owner` permissões:</span><span class="sxs-lookup"><span data-stu-id="bf554-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="bf554-380">Instale o SQL Server Management Studio usando o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="bf554-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="bf554-381">Conecte-se à instância local do SQL Server Express (por padrão, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="bf554-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="bf554-382">Clique em **bancos de dados** &gt; *[localSubtextDatabase]* &gt; **segurança** &gt; **usuários** &gt; *[localSubtextUser*] (o padrão é `subtextuser`], com o botão direito e clique em **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="bf554-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="bf554-383">Selecione **db\_proprietário** na seção de associação de função.</span><span class="sxs-lookup"><span data-stu-id="bf554-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="bf554-384">Problema: Os sites podem não funcionar após a publicação se o campo "Destino URL" não é prefixado com http:// ou https://</span><span class="sxs-lookup"><span data-stu-id="bf554-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="bf554-385">No **configurações de publicação** caixa de diálogo, se a URL de destino não começa com `http://` ou `https://`, o site pode não funcionar após a implantação.</span><span class="sxs-lookup"><span data-stu-id="bf554-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="bf554-386">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-386">**Workaround**</span></span>  
> <span data-ttu-id="bf554-387">Verifique se antes de publicar um site, a URL de destino no **configurações de publicação** caixa de diálogo começa com `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="bf554-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="bf554-388">Problema: Publicar um banco de dados MySQL falha com o erro "Falha ao publicar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf554-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="bf554-389">Isso pode acontecer se o banco de dados remoto não é possível executar o script."</span><span class="sxs-lookup"><span data-stu-id="bf554-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="bf554-390">O erro pode ocorrer por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="bf554-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="bf554-391">Um motivo que você pode ver esse erro é se o script de banco de dados contém um caractere de aspas simples (') e o conjunto de caracteres de padrão de destino MySQL do banco de dados não é UTF-8.</span><span class="sxs-lookup"><span data-stu-id="bf554-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="bf554-392">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-392">**Workaround**</span></span>  
> <span data-ttu-id="bf554-393">Defina o caractere padrão definido para o banco de dados remoto do MySQL para UTF-8.</span><span class="sxs-lookup"><span data-stu-id="bf554-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="bf554-394">Problema: Alguns links não são visíveis no DotNetNuke depois de publicar ou baixando o site</span><span class="sxs-lookup"><span data-stu-id="bf554-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="bf554-395">Se você publica ou baixar um site DotNetNuke, talvez seja necessário limpar o cache para obter novos links para aparecer no site.</span><span class="sxs-lookup"><span data-stu-id="bf554-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="bf554-396">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="bf554-397">Faça logon como "Host".</span><span class="sxs-lookup"><span data-stu-id="bf554-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="bf554-398">Vá para o menu de host e selecione **configurações de Host**.</span><span class="sxs-lookup"><span data-stu-id="bf554-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="bf554-399">Role para baixo e, em **configurações avançadas**, expanda **as configurações de desempenho**.</span><span class="sxs-lookup"><span data-stu-id="bf554-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="bf554-400">Clique o **Limpar Cache** links para páginas.</span><span class="sxs-lookup"><span data-stu-id="bf554-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="bf554-401">Vá até a parte inferior da página e reiniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf554-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="bf554-402">Problema: Alguns links no AtomSite são quebrados após você fazer o download de um site publicado</span><span class="sxs-lookup"><span data-stu-id="bf554-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="bf554-403">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-403">**Workaround**</span></span>  
> <span data-ttu-id="bf554-404">No *service.config* arquivo, *users.config* arquivo e todos os *. XML* arquivos, substitua a cadeia de caracteres de URL (por exemplo, `http://myhost.com/atomsite`) com um local (por exemplo, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="bf554-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="bf554-405">Problema: Aplicativos como o WordPress MySQL falharem publicar e relatar um erro de banco de dados</span><span class="sxs-lookup"><span data-stu-id="bf554-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="bf554-406">Por padrão, o WebMatrix instala MySQL com o conjunto de caracteres UTF-8.</span><span class="sxs-lookup"><span data-stu-id="bf554-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="bf554-407">Se você instalar o MySQL por conta própria, e o conjunto de caracteres não é UTF-8 (por exemplo, é Latin1), o processo de publicação para bancos de dados pode falhar.</span><span class="sxs-lookup"><span data-stu-id="bf554-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="bf554-408">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="bf554-409">Altere o conjunto de caracteres para MySQL para UTF-8.</span><span class="sxs-lookup"><span data-stu-id="bf554-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="bf554-410">(Para obter detalhes, consulte [o conjunto de caracteres do servidor e o agrupamento](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) no site do MySQL.)</span><span class="sxs-lookup"><span data-stu-id="bf554-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="bf554-411">Reinstale o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf554-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="bf554-412">Publicar novamente o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf554-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="bf554-413">Problema: "Download publicados site" falhar para aplicativos que têm a instalação baseada em navegador</span><span class="sxs-lookup"><span data-stu-id="bf554-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="bf554-414">Alguns aplicativos (por exemplo, o Kentico CMS) exigem que você iniciá-los no navegador para executar o programa de instalação após a instalação, como criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf554-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="bf554-415">Se você publicar um aplicativo como este, sem concluir a instalação baseada em navegador, a tentativa de fazer o download do mesmo site de um servidor remoto falhará.</span><span class="sxs-lookup"><span data-stu-id="bf554-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="bf554-416">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-416">**Workaround**</span></span>  
> <span data-ttu-id="bf554-417">Conclua a instalação baseada em navegador antes de publicar o site.</span><span class="sxs-lookup"><span data-stu-id="bf554-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="bf554-418">Problema: "Download publicados site" falhará com um erro de banco de dados para DotNetNuke e Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="bf554-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="bf554-419">Se você tentar baixar um aplicativo de um servidor e ter credenciais de administrador na cadeia de conexão de banco de dados no **configurações de publicação** caixa de diálogo, você poderá ver o seguinte erro no log de publicação:</span><span class="sxs-lookup"><span data-stu-id="bf554-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="bf554-420">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="bf554-420">**Workaround**</span></span>  
> <span data-ttu-id="bf554-421">Se for prático, publicar novamente o site (ou publicá-lo) usando credenciais de não-administrador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf554-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="bf554-422">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="bf554-422">For More Information</span></span>

<span data-ttu-id="bf554-423">Para obter mais informações sobre o WebMatrix 1.0, consulte os seguintes sites:</span><span class="sxs-lookup"><span data-stu-id="bf554-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="bf554-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="bf554-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="bf554-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf554-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="bf554-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="bf554-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="bf554-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="bf554-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="bf554-428">Todos os direitos reservados.</span><span class="sxs-lookup"><span data-stu-id="bf554-428">All Rights Reserved.</span></span> <span data-ttu-id="bf554-429">[Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf554-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
