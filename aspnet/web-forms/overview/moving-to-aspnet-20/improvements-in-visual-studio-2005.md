---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Melhorias no Visual Studio 2005 | Microsoft Docs
author: microsoft
description: O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de melhorias para projetos da Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="389c1-103">Melhorias no Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="389c1-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="389c1-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="389c1-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="389c1-105">O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de melhorias para projetos da Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="389c1-106">O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de melhorias para projetos da Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="389c1-107">Mesmo avançado como o Visual Studio .NET 2002 e 2003 são, houve muitas reclamações a maneira como os projetos da Web foram manipulados.</span><span class="sxs-lookup"><span data-stu-id="389c1-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="389c1-108">O Visual Studio 2005 adiciona um número significativo de novos recursos para lidar com essas reclamações.</span><span class="sxs-lookup"><span data-stu-id="389c1-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="389c1-109">Para aqueles que preferem a maneira como o Visual Studio .NET 2003 tratados compilação de aplicativos da Web, consulte [projetos de aplicativo Web](https://go.microsoft.com/fwlink/?LinkId=57870).</span><span class="sxs-lookup"><span data-stu-id="389c1-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="389c1-110">Neste módulo, também abrangem melhorias na criação do projeto da Web, gerenciamento e desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="389c1-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="389c1-111">Em um módulo posterior, também abrangem melhorias na criação de projetos da Web e implantá-los.</span><span class="sxs-lookup"><span data-stu-id="389c1-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="389c1-112">Extensões de servidor do FrontPage</span><span class="sxs-lookup"><span data-stu-id="389c1-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="389c1-113">O Visual Studio .NET 2002 e 2003 necessário extensões na caixa para criar ou criar projetos da Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="389c1-114">Os desenvolvedores tinha uma escolha entre dois modos de acesso diferentes (extensões de servidor do FrontPage ou arquivo de modo de acesso), ambos usados extensões para executar tarefas como definir a raiz do aplicativo no IIS, etc.</span><span class="sxs-lookup"><span data-stu-id="389c1-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="389c1-115">O Visual Studio 2005 remove a dependência de extensões para projetos locais.</span><span class="sxs-lookup"><span data-stu-id="389c1-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="389c1-116">Agora, o Visual Studio 2005 acessa a metabase do IIS diretamente, em vez de usar as extensões de servidor do FrontPage.</span><span class="sxs-lookup"><span data-stu-id="389c1-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="389c1-117">O Visual Studio 2005 também adiciona suporte para FTP, que permite o acesso remoto do projeto sem a necessidade de extensões de servidor do FrontPage.</span><span class="sxs-lookup"><span data-stu-id="389c1-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="389c1-118">Para os desenvolvedores que desejam usar extensões em seus projetos, a opção ainda está disponível.</span><span class="sxs-lookup"><span data-stu-id="389c1-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="389c1-119">No entanto, com base nos comentários fortes da comunidade de desenvolvedores do ASP.NET, não é um requisito.</span><span class="sxs-lookup"><span data-stu-id="389c1-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-120">Extensões de servidor do FrontPage ainda são necessárias para a criação do projeto remoto, abrir, etc.</span><span class="sxs-lookup"><span data-stu-id="389c1-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="389c1-121">ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="389c1-121">ASP.NET Development Server</span></span>

<span data-ttu-id="389c1-122">O Visual Studio 2005 vem com um novo servidor Web chamado ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="389c1-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="389c1-123">(Esse servidor Web era conhecido anteriormente como Cassini).</span><span class="sxs-lookup"><span data-stu-id="389c1-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="389c1-124">Há vários benefícios do servidor de desenvolvimento do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="389c1-125">Agora é possível para administradores desenvolver e depurar em um servidor Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="389c1-126">O ASP.NET Development Server mapeia dinamicamente os diretórios virtuais em qualquer local no sistema de arquivos, permitindo a locais de projeto flexível.</span><span class="sxs-lookup"><span data-stu-id="389c1-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="389c1-127">Os usuários no Windows XP Professional que já estiver usando o IIS agora será capazes de criar novos aplicativos Web que não afetam a estrutura de pasta ou arquivo do seu Site padrão no IIS.</span><span class="sxs-lookup"><span data-stu-id="389c1-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="389c1-128">Nenhuma configuração especial é necessária para aproveitar o ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="389c1-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="389c1-129">Quando um projeto da Web que é hospedado no sistema de arquivos é depurado ou procurado, o Visual Studio 2005 iniciará automaticamente uma instância do servidor de desenvolvimento ASP.NET em uma porta aleatória para atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="389c1-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="389c1-130">Obter mais informações serão abordadas no servidor de desenvolvimento ASP.NET neste módulo.</span><span class="sxs-lookup"><span data-stu-id="389c1-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="389c1-131">Gerenciamento de arquivos aprimorado</span><span class="sxs-lookup"><span data-stu-id="389c1-131">Improved File Management</span></span>

<span data-ttu-id="389c1-132">No Visual Studio 2002 e 2003, um arquivo de projeto (. vbproj para VB.NET) e. csproj c# informações armazenadas em todos os arquivos no aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="389c1-133">A exibição do Gerenciador de soluções baseia-se as informações do arquivo no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="389c1-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="389c1-134">Por isso, o Gerenciador de soluções geralmente exibirá informações precisas em casos onde editores externos foram usados.</span><span class="sxs-lookup"><span data-stu-id="389c1-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="389c1-135">O Visual Studio 2002 e 2003 seria geralmente substituir as alterações de arquivo ou não exibir a versão mais recente dos arquivos.</span><span class="sxs-lookup"><span data-stu-id="389c1-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="389c1-136">Visual Studio 2005 imediatamente faz com que o arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="389c1-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="389c1-137">Em vez disso, ele lê as informações de arquivo e pasta diretamente do disco, resultando em uma exibição precisa dos arquivos em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="389c1-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="389c1-138">Porque a pasta de referências no Visual Studio 2002 e 2003 não representa uma pasta real em seu aplicativo Web, o Visual Studio 2005 também remove a pasta referências no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="389c1-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="389c1-139">Para acessar as referências para o seu projeto no Visual Studio 2005, você deve usar as páginas de propriedades para o projeto.</span><span class="sxs-lookup"><span data-stu-id="389c1-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="389c1-140">Criando projetos da Web</span><span class="sxs-lookup"><span data-stu-id="389c1-140">Creating Web Projects</span></span>

<span data-ttu-id="389c1-141">Os desenvolvedores da Web tem muitas opções novas disponíveis para a criação do projeto no Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="389c1-142">Sites da Web agora podem ser criados em qualquer lugar no sistema de arquivos e, em seguida, pode ser depurados ou pesquisado usando o novo servidor de desenvolvimento do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="389c1-143">Os desenvolvedores também podem criar novos sites da Web usando FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="389c1-144">Clique aqui para exibir um vídeo passo a passo de criação de projetos Web no Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="389c1-145">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="389c1-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="389c1-146">Projetos do sistema de arquivos</span><span class="sxs-lookup"><span data-stu-id="389c1-146">File System Projects</span></span>

<span data-ttu-id="389c1-147">Como você viu no passo a passo de vídeo, você pode escolher criar sites da Web no sistema de arquivos no computador local ou em um local remoto por meio de um compartilhamento de arquivos.</span><span class="sxs-lookup"><span data-stu-id="389c1-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="389c1-148">Sites da Web que são criadas no sistema de arquivos são acessados e depurado usando o ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="389c1-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-149">O ASP.NET Development Server pode causar alguma confusão para os clientes.</span><span class="sxs-lookup"><span data-stu-id="389c1-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="389c1-150">Se um projeto da Web é criado no sistema de arquivos na estrutura de diretório IISs (por exemplo, c: inetpub/wwwroot), o site ainda será ser navegado por meio do servidor de desenvolvimento ASP.NET quando iniciado a partir do Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="389c1-151">Portanto, qualquer configuração de IIS (ou seja, métodos de autenticação) não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="389c1-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="389c1-152">O projeto da web padrão também remove muito da sobrecarga por inclui apenas uma página Default.aspx, arquivo default.cs e uma pasta de Data.</span><span class="sxs-lookup"><span data-stu-id="389c1-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="389c1-153">O Web. config e pastas especiais (por exemplo, Code) são adicionadas como eles são necessários.</span><span class="sxs-lookup"><span data-stu-id="389c1-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="389c1-154">O projeto da web inclui apenas os arquivos e pastas que você precisa.</span><span class="sxs-lookup"><span data-stu-id="389c1-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="389c1-155">Projetos HTTP</span><span class="sxs-lookup"><span data-stu-id="389c1-155">HTTP Projects</span></span>

<span data-ttu-id="389c1-156">Projetos HTTP podem ser projetos que são criados em um site da Web do IIS local ou em um site remoto.</span><span class="sxs-lookup"><span data-stu-id="389c1-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="389c1-157">O local do projeto padrão é `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="389c1-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="389c1-158">Se você clicar no botão Procurar, há duas opções de HTTP: IIS Local e o Site remoto.</span><span class="sxs-lookup"><span data-stu-id="389c1-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="389c1-159">A principal diferença nessas duas opções é o método no qual as informações do site da web são exibidas na caixa de diálogo Escolher local e em como os arquivos são copiados para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="389c1-160">A opção de Local IIS lê as informações do site da metabase no computador local e os arquivos são copiados usando o sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="389c1-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="389c1-161">A opção de Site remoto usa as extensões de servidor do FrontPage e as informações do site e os arquivos são copiados usando HTTP e chamadas de RPC de extensões de servidor do FrontPage.</span><span class="sxs-lookup"><span data-stu-id="389c1-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-162">O arquivo de vs###/_tmp.htm e get/_aspx/_ver.aspx não são usados para determinar as informações de versão.</span><span class="sxs-lookup"><span data-stu-id="389c1-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="389c1-163">A opção de HTTP padrão é o Local do IIS.</span><span class="sxs-lookup"><span data-stu-id="389c1-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="389c1-164">Essa opção lê a Metabase do IIS para determinar quais sites estão disponíveis e o local no qual criar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="389c1-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="389c1-165">Você pode selecionar uma pasta diferente ou um diretório virtual, selecionando-o na exibição de árvore.</span><span class="sxs-lookup"><span data-stu-id="389c1-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="389c1-166">Você pode também criar um novo diretório virtual, marcar pastas como aplicativos, bem como excluir diretórios virtuais existentes nessa caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="389c1-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![A escolha da caixa de diálogo local](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="389c1-168">**Figura 1**: A escolher a caixa de diálogo local</span><span class="sxs-lookup"><span data-stu-id="389c1-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="389c1-169">Ao contrário em versões anteriores do Visual Studio, se você verificar o **usar Secure Sockets Layer** caixa de seleção e o certificado SSL não coincide com a URL que você está pesquisando, você verá uma caixa de diálogo alerta de segurança, perguntando se você faria Deseja continuar.</span><span class="sxs-lookup"><span data-stu-id="389c1-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="389c1-170">Usando o Visual Studio .NET 2003, se o certificado não era uma correspondência, criando o projeto falhará.</span><span class="sxs-lookup"><span data-stu-id="389c1-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![Alerta certificado SSL sobre segurança](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="389c1-172">**Figura 2**: alerta de segurança sobre o certificado SSL</span><span class="sxs-lookup"><span data-stu-id="389c1-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="389c1-173">Observação sobre cabeçalhos de Host</span><span class="sxs-lookup"><span data-stu-id="389c1-173">Note on Host Headers</span></span>

<span data-ttu-id="389c1-174">Se você estiver criando um aplicativo da Web em um site associado a um IP específico, você precisará garantir que um cabeçalho de host é configurado.</span><span class="sxs-lookup"><span data-stu-id="389c1-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="389c1-175">Caso contrário, o Visual Studio criará o site em `http://localhost`, mas o endereço IP não será interpretado corretamente quando o site é pesquisado ou depurado de dentro do IDE.</span><span class="sxs-lookup"><span data-stu-id="389c1-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="389c1-176">Se você selecionar a opção de Site remoto, a caixa de diálogo é alterado para permitir que você insira a URL de destino para o novo site.</span><span class="sxs-lookup"><span data-stu-id="389c1-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="389c1-177">Essa URL deve ser em um servidor que tem o FrontPage Server Extensions habilitado.</span><span class="sxs-lookup"><span data-stu-id="389c1-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="389c1-178">Se você quiser trabalhar com seu servidor Web local usando as extensões de servidor do FrontPage, você pode usar a opção de Site remoto e especifique uma URL de local.</span><span class="sxs-lookup"><span data-stu-id="389c1-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![Criar um Site da Web em um servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="389c1-180">**Figura 3**: Criando um Site da Web em um servidor remoto</span><span class="sxs-lookup"><span data-stu-id="389c1-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="389c1-181">Ao criar um aplicativo em um local remoto por meio de SSL, se o certificado SSL não corresponder, a caixa de diálogo de confirmação é ligeiramente diferente do que a caixa de diálogo exibida ao usar a opção do IIS Local.</span><span class="sxs-lookup"><span data-stu-id="389c1-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![O alerta de segurança do Site remoto](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="389c1-183">**Figura 4**: O alerta de segurança do Site remoto</span><span class="sxs-lookup"><span data-stu-id="389c1-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="389c1-184">FTP</span><span class="sxs-lookup"><span data-stu-id="389c1-184">FTP</span></span>

<span data-ttu-id="389c1-185">O Visual Studio 2005 apresenta a opção para criar sites da Web por meio de FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="389c1-186">Quando você usar essa opção, o IDE cria os arquivos localmente na pasta temp de usuários e, em seguida, usa o FTP para mover os arquivos para o local de FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-187">O local da pasta temporária é c: / Documents and Settings /&lt;usuário&gt;/Local Temp/configurações/VWDWebCache/&lt;servidor&gt;/_&lt;nome do aplicativo&gt;</span><span class="sxs-lookup"><span data-stu-id="389c1-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="389c1-188">Ao usar a opção de FTP, você verá uma caixa de diálogo Escolher local.</span><span class="sxs-lookup"><span data-stu-id="389c1-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="389c1-189">Você pode inserir as informações de conexão de FTP necessárias nesta caixa de diálogo, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![A escolha da caixa de diálogo de local de FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="389c1-191">**Figura 5**: A escolher a caixa de diálogo de local de FTP</span><span class="sxs-lookup"><span data-stu-id="389c1-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="389c1-192">Laboratório: Configuração FTP do site e criar um projeto</span><span class="sxs-lookup"><span data-stu-id="389c1-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="389c1-193">As etapas a seguir configuram o site FTP para que um usuário tem um local que só eles podem carregar por meio de FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="389c1-194">Instalar o serviço de FTP</span><span class="sxs-lookup"><span data-stu-id="389c1-194">Install the FTP Service</span></span>

1. <span data-ttu-id="389c1-195">Abrir Adicionar ou remover programas, selecione Adicionar/remover componentes do Windows</span><span class="sxs-lookup"><span data-stu-id="389c1-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="389c1-196">Selecione os serviços de informações da Internet (servidor de aplicativos no Windows 2003) e clique em **detalhes**.</span><span class="sxs-lookup"><span data-stu-id="389c1-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="389c1-197">Verificar **serviço de protocolo de transferência de arquivo (FTP)** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="389c1-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="389c1-198">Clique em **próximo** para instalar o serviço FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="389c1-199">Crie uma nova pasta de conteúdo</span><span class="sxs-lookup"><span data-stu-id="389c1-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="389c1-200">No Windows Explorer, crie uma nova pasta chamada **User1** dentro de c: inetpub/wwwroot.</span><span class="sxs-lookup"><span data-stu-id="389c1-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="389c1-201">Configure as pastas e permissões nas pastas.</span><span class="sxs-lookup"><span data-stu-id="389c1-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="389c1-202">Abra o snap-in de serviços de informações da Internet em Ferramentas administrativas.</span><span class="sxs-lookup"><span data-stu-id="389c1-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="389c1-203">Agora, você terá uma pasta de Sites FTP sob o nó de nome do computador.</span><span class="sxs-lookup"><span data-stu-id="389c1-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="389c1-204">Expanda **Sites FTP**.</span><span class="sxs-lookup"><span data-stu-id="389c1-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="389c1-205">Com o botão direito o **Site FTP padrão**, selecione **novo**, em seguida, **Diretório Virtual**, em seguida, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="389c1-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="389c1-206">Digite **User1** para o nome do diretório virtual e clique **próximo**.</span><span class="sxs-lookup"><span data-stu-id="389c1-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="389c1-207">Digite **c: / inetpub/wwwroot/User1** para o caminho e clique **próximo**.</span><span class="sxs-lookup"><span data-stu-id="389c1-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="389c1-208">Clique em **próximo** e **concluir** para concluir o assistente.</span><span class="sxs-lookup"><span data-stu-id="389c1-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="389c1-209">Clique com botão direito do **User1** diretório virtual no Site FTP padrão e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="389c1-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="389c1-210">Verifique o **gravar** caixa de seleção e clique em **Okey** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="389c1-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="389c1-211">Clique com botão direito **Site FTP padrão** e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="389c1-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="389c1-212">Sobre o **contas de segurança** guia, desmarque a opção **permitir conexões anônimas**.</span><span class="sxs-lookup"><span data-stu-id="389c1-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="389c1-213">Clique em **Sim** na caixa de diálogo perguntando se deseja continuar.</span><span class="sxs-lookup"><span data-stu-id="389c1-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="389c1-214">Clique em **Okey** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="389c1-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="389c1-215">Expanda o **Default Web Site** sob o **Sites da Web** nó.</span><span class="sxs-lookup"><span data-stu-id="389c1-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="389c1-216">Clique com botão direito do **User1** diretório e selecione **propriedades**</span><span class="sxs-lookup"><span data-stu-id="389c1-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="389c1-217">No **configurações do aplicativo** seção, clique em **criar** para marcar a pasta como um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="389c1-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="389c1-218">Clique em **Okey** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="389c1-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="389c1-219">Feche o snap-in de serviços de informações da Internet.</span><span class="sxs-lookup"><span data-stu-id="389c1-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="389c1-220">Criar projeto da web</span><span class="sxs-lookup"><span data-stu-id="389c1-220">Create web project</span></span>

1. <span data-ttu-id="389c1-221">Abra o Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="389c1-222">Do **arquivo** menu, selecione **novo Site**.</span><span class="sxs-lookup"><span data-stu-id="389c1-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="389c1-223">No **local** lista suspensa, selecione **FTP**.</span><span class="sxs-lookup"><span data-stu-id="389c1-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="389c1-224">Clique em **Procurar**.</span><span class="sxs-lookup"><span data-stu-id="389c1-224">Click **Browse**.</span></span>
5. <span data-ttu-id="389c1-225">Digite **localhost** no **Server** caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="389c1-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="389c1-226">Digite **User1** na caixa de texto diretório.</span><span class="sxs-lookup"><span data-stu-id="389c1-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="389c1-227">Clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="389c1-227">Click **Open**.</span></span> <span data-ttu-id="389c1-228">O local de FTP será inserido na caixa de diálogo Novo Site da Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="389c1-229">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="389c1-229">Click **OK**.</span></span>
9. <span data-ttu-id="389c1-230">Desmarque **Anonymous Logon** na caixa de diálogo logon de FTP, insira suas credenciais e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="389c1-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="389c1-231">Qual é a URL para o projeto?</span><span class="sxs-lookup"><span data-stu-id="389c1-231">What is the URL for the project?</span></span> <span data-ttu-id="389c1-232">(A URL para o projeto será exibida no Gerenciador de soluções.)</span><span class="sxs-lookup"><span data-stu-id="389c1-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="389c1-233">Do **criar** menu, selecione **Compilar Site** ou **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="389c1-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="389c1-234">Clique duas vezes em Default.aspx no Gerenciador de soluções e selecione **exibir no navegador**.</span><span class="sxs-lookup"><span data-stu-id="389c1-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="389c1-235">Na caixa de diálogo necessário URL do Site da Web, digite `http://localhost/user1` para a URL e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="389c1-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-236">Se você receber um erro indicando a impossibilidade de carregar o tipo /_Default, certifique-se de que você está executando o ASP.NET 2.0 no seu site da Web e não uma versão anterior.</span><span class="sxs-lookup"><span data-stu-id="389c1-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="389c1-237">Você pode fazer isso na guia ASP.NET nos serviços de informações da Internet.</span><span class="sxs-lookup"><span data-stu-id="389c1-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="389c1-238">Abrindo projetos da Web</span><span class="sxs-lookup"><span data-stu-id="389c1-238">Opening Web Projects</span></span>

<span data-ttu-id="389c1-239">Abrir projetos da Web é semelhante à criação de projetos.</span><span class="sxs-lookup"><span data-stu-id="389c1-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="389c1-240">As seções a seguir chamar áreas ficar de olho-out para enquanto estiver trabalhando no IDE.</span><span class="sxs-lookup"><span data-stu-id="389c1-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="389c1-241">Ele também aborda a trabalhar com projetos da Web usando HTTP e FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="389c1-242">Para abrir um projeto da Web, selecione Abrir o Site da Web no menu arquivo.</span><span class="sxs-lookup"><span data-stu-id="389c1-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="389c1-243">Você será solicitado com a mesma caixa de diálogo Escolher local abordada anteriormente e ter as mesmas quatro opções disponíveis para você: Site remoto, IIS Local, FTP e sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="389c1-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="389c1-244">Sistema de arquivos</span><span class="sxs-lookup"><span data-stu-id="389c1-244">File System</span></span>

<span data-ttu-id="389c1-245">Como indicado anteriormente neste módulo, o Visual Studio não usa um arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="389c1-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="389c1-246">Portanto, se você optar por abrir um site do sistema de arquivos, você realmente tem a opção de escolher qualquer pasta que desejar, mesmo se a pasta que você escolher não foi criada como um projeto Web inicialmente no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="389c1-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="389c1-247">Por exemplo, você pode optar por abrir a pasta Meus documentos, como um site da Web e o Visual Studio Felizmente abri-lo e exibir os arquivos, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Meus documentos abertos como um Site da Web](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="389c1-249">**Figura 6**: *Meus documentos* aberto como um Site da Web</span><span class="sxs-lookup"><span data-stu-id="389c1-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="389c1-250">Porque somente o Visual Studio cria outros arquivos e pastas, quando necessário, sem arquivos ou pastas adicionais são adicionadas para o local em que você abrir.</span><span class="sxs-lookup"><span data-stu-id="389c1-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="389c1-251">Um efeito colateral dessa arquitetura é que ela impede que você o aninhamento de sites da Web no sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="389c1-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="389c1-252">Por exemplo, considere a seguinte estrutura de diretório.</span><span class="sxs-lookup"><span data-stu-id="389c1-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="389c1-253">Projeto da Web em c: / Meuwebsite</span><span class="sxs-lookup"><span data-stu-id="389c1-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="389c1-254">Outro projeto da web em c: / Meuwebsite/aninhadas</span><span class="sxs-lookup"><span data-stu-id="389c1-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="389c1-255">Quando você abrir o site da Web no c/Meuwebsite, a pasta aninhadas aparecerá como uma subpasta do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="389c1-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="389c1-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="389c1-256">HTTP</span></span>

<span data-ttu-id="389c1-257">Ao abrir sites da Web via HTTP, as configurações são lidas da metabase do IIS (IIS Local) ou usando o FrontPage Server Extensions (Site remoto). Se houver aplicativos web aninhadas, eles são exibidos também com um ícone que identifica como um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="389c1-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="389c1-258">Se você estiver familiarizado com a trabalhar com aplicativos web do FrontPage, o comportamento no Visual Studio 2005 é semelhante.</span><span class="sxs-lookup"><span data-stu-id="389c1-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="389c1-259">Embora o Visual Studio exibirá um ícone para aplicativos que estão aninhadas sob o aplicativo que está atualmente aberto no IDE, ele não permitirá que você expandir para ver seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="389c1-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="389c1-260">No entanto, você pode clicar duas vezes neles para abri-los.</span><span class="sxs-lookup"><span data-stu-id="389c1-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="389c1-261">Quando você fizer isso, você verá uma caixa de diálogo solicitando que você abrir o aplicativo web (e substitua a solução atualmente aberta) ou adicionar o aplicativo Web para sua solução atual.</span><span class="sxs-lookup"><span data-stu-id="389c1-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![Clicando duas vezes em um ícone de aplicativo aninhada apresenta esta caixa de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="389c1-263">**Figura 7**: clicando duas vezes em um ícone de aplicativo aninhada apresenta esta caixa de diálogo</span><span class="sxs-lookup"><span data-stu-id="389c1-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="389c1-264">Site FTP</span><span class="sxs-lookup"><span data-stu-id="389c1-264">FTP Site</span></span>

<span data-ttu-id="389c1-265">Quando você abre um site por meio de FTP, os arquivos são todos copiados localmente para a pasta temp.</span><span class="sxs-lookup"><span data-stu-id="389c1-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="389c1-266">O caminho completo para o local de armazenamento local é exibido no painel de propriedades do projeto e é criado usando o formato a seguir.</span><span class="sxs-lookup"><span data-stu-id="389c1-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="389c1-267">C: / Documents and Settings /&lt;usuário&gt;/Local Temp/configurações/VWDWebCache/&lt;servidor&gt;/_&lt;nome do aplicativo&gt;</span><span class="sxs-lookup"><span data-stu-id="389c1-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="389c1-268">Ao usar o FTP, Visual Studio precisará especificar a URL base para o seu projeto para que você pode procurar conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="389c1-269">Se você não especificar uma URL base, Visual Studio solicitará para ele na primeira vez que você tentar navegar em uma página no site da Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![Especificando uma URL Base para Sites FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="389c1-271">**Figura 8**: especificando uma URL Base para Sites FTP</span><span class="sxs-lookup"><span data-stu-id="389c1-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="389c1-272">Melhorias na compilação</span><span class="sxs-lookup"><span data-stu-id="389c1-272">Improvements in Compilation</span></span>

<span data-ttu-id="389c1-273">Trabalhar com aplicativos Web no Visual Studio 2005 é consideravelmente mais rápido do que as versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="389c1-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="389c1-274">Isso é nenhuma parte pequena devido às alterações na arquitetura de compilação.</span><span class="sxs-lookup"><span data-stu-id="389c1-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="389c1-275">No Visual Studio 2002 e 2003, os aplicativos da Web foram compilados em um assembly principal que reside na pasta /bin.</span><span class="sxs-lookup"><span data-stu-id="389c1-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="389c1-276">No Visual Studio 2005, foi adicionada a uma pasta de Code.</span><span class="sxs-lookup"><span data-stu-id="389c1-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="389c1-277">Classes e outro código de interface de usuário não são adicionadas à pasta de Code.</span><span class="sxs-lookup"><span data-stu-id="389c1-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="389c1-278">Quando o Visual Studio compila o projeto, todos os arquivos na pasta do Code são compilados em um único arquivo App/_Code.dll.</span><span class="sxs-lookup"><span data-stu-id="389c1-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="389c1-279">O resultado dessa alteração é que as compilações subsequentes são muito mais rápidas do que nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="389c1-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-280">O utilitário de linha de comando do MSBuild também pode ser usado para criar aplicativos do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="389c1-281">Essa ferramenta será abordada no módulo 9.</span><span class="sxs-lookup"><span data-stu-id="389c1-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="389c1-282">Outro aprimoramento de compilação é a nova opção de página Build no menu Build.</span><span class="sxs-lookup"><span data-stu-id="389c1-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="389c1-283">Esse recurso permite que um desenvolvedor recriar apenas a página atual (juntamente com, do curso e dependências) para que as alterações podem ser compiladas mais rapidamente.</span><span class="sxs-lookup"><span data-stu-id="389c1-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="389c1-284">Porque c# não oferece a compilação do plano de fundo para fins de atualização do IntelliSense, etc., eles serão beneficiadas imensamente desse recurso porque ele permitirá IntelliSense para ser atualizado rapidamente, simplesmente recriando uma única página.</span><span class="sxs-lookup"><span data-stu-id="389c1-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="389c1-285">As propriedades de compilação para um projeto permitem que você configure o tipo de compilação que ocorre antes da página de inicialização é executada.</span><span class="sxs-lookup"><span data-stu-id="389c1-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="389c1-286">Os desenvolvedores podem optar por criar somente a página atual para que o Visual Studio pode iniciar a depuração de aplicativos mais rapidamente após as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="389c1-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![A ação de início da página de compilação](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="389c1-288">**Figura 9**: A ação de início da página de compilação</span><span class="sxs-lookup"><span data-stu-id="389c1-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="389c1-289">Outro aprimoramento excelente para Visual Studio e a arquitetura do ASP.NET está na área de editar e continuar.</span><span class="sxs-lookup"><span data-stu-id="389c1-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="389c1-290">No Visual Studio 2005, os desenvolvedores podem iniciar a depuração de um projeto e fazer alterações de código no projeto sem desanexar o depurador.</span><span class="sxs-lookup"><span data-stu-id="389c1-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="389c1-291">Na verdade, literalmente pode iniciar a depuração de um projeto, adicione uma nova classe, adicione código para essa classe, adicione o código para a página que cria uma nova instância da classe e executar um método da classe, tudo sem desanexar o depurador.</span><span class="sxs-lookup"><span data-stu-id="389c1-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="389c1-292">Executar o novo código é literalmente mais fácil atualizar o navegador!</span><span class="sxs-lookup"><span data-stu-id="389c1-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="389c1-293">Clique aqui para ver um vídeo passo a passo de editar e continuar o recurso no Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="389c1-294">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="389c1-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="389c1-295">O robusto de edita e continuar a funcionalidade no ASP.NET 2.0 e o Visual Studio 2005 é devido a uma alteração estrutural para aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="389c1-296">No ASP.NET 1. x, os aplicativos criados no Visual Studio 2002/2003 foram compilados em um assembly principal que foi armazenado na pasta /bin.</span><span class="sxs-lookup"><span data-stu-id="389c1-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="389c1-297">Todas as classes, páginas, etc. para o aplicativo foi compilado em que uma DLL.</span><span class="sxs-lookup"><span data-stu-id="389c1-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="389c1-298">Em tempo de execução, o ASP.NET, em seguida, seria compilar todos os controles, a marcação e o código ASP.NET em páginas e copiar as DLLs na pasta temporária do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="389c1-299">No Visual Studio 2005 usando o ASP.NET 2.0, os modelos de dois compilação descrevem acima (uma para o Visual Studio) e outra para o ASP.NET em tempo de execução foram mescladas em um modelo comum de compilação.</span><span class="sxs-lookup"><span data-stu-id="389c1-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="389c1-300">Isso significa que todos os problemas de compilação agora são detectados durante a fase de desenvolvimento, em vez de em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="389c1-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="389c1-301">Ele também permite para o designer e o suporte ao IntelliSense para recursos, como páginas mestras e controles de usuário.</span><span class="sxs-lookup"><span data-stu-id="389c1-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="389c1-302">Clique aqui para ver um vídeo passo a passo de suporte de designer para controles de usuário.</span><span class="sxs-lookup"><span data-stu-id="389c1-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="389c1-303">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="389c1-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="389c1-304">Quando um controle de usuário é removido de uma página, o @Register diretiva permanece na marcação e deve ser removida manualmente para evitar erros de analisador se o controle de usuário é excluído do site.</span><span class="sxs-lookup"><span data-stu-id="389c1-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="389c1-305">Outro aprimoramento no modelo de compilação do Visual Studio é o recurso Publicar Site.</span><span class="sxs-lookup"><span data-stu-id="389c1-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="389c1-306">Porque o recurso de publicação pré-compila o site da Web, os desenvolvedores podem aproveitar o desempenho de não precisar compilar tudo sob demanda.</span><span class="sxs-lookup"><span data-stu-id="389c1-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="389c1-307">Ele também pré-compila todo o código fonte na pasta do Code em uma DLL para que nenhum código-fonte deve ser implantado.</span><span class="sxs-lookup"><span data-stu-id="389c1-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![A caixa de diálogo Publicar Web Site](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="389c1-309">**Figura 10**: A caixa de diálogo Publicar Web Site</span><span class="sxs-lookup"><span data-stu-id="389c1-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="389c1-310">O utilitário aspnet/_compile.exe também pode ser usado para pré-compilar um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="389c1-311">Essa ferramenta será abordada no módulo 9.</span><span class="sxs-lookup"><span data-stu-id="389c1-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="389c1-312">Quando você publicar um site da Web, os arquivos pré-compilado é armazenados na pasta arquivos temporários do ASP.NET, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="389c1-313">Arquivos com um *. Compiled* extensão de arquivo são arquivos XML que definem as dependências para DLLs específicos.</span><span class="sxs-lookup"><span data-stu-id="389c1-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="389c1-314">Os controles de formulário da Web ou de usuário são compilados para DLLs aleatórios que começam com *aplicativo /_Web /_*.</span><span class="sxs-lookup"><span data-stu-id="389c1-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="389c1-315">Se você deixar o *permitir que este site pré-compilado seja atualizado* caixa de seleção marcada, a marcação dentro de seus controles de formulários da Web e o usuário não será pré-compilado em uma DLL que permitem fazer alterações após a implantação.</span><span class="sxs-lookup"><span data-stu-id="389c1-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="389c1-316">Se você preferir bloquear a marcação para que não são permitidas alterações ao conteúdo implantado, desmarque essa caixa.</span><span class="sxs-lookup"><span data-stu-id="389c1-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="389c1-317">O *Use fixa assemblies de página única e nomenclatura* caixa de seleção permite que você desabilite a compilação em lote para que cada página é compilada em um assembly de nome fixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="389c1-318">Sair dessa caixa desmarcada permite aproveitar compilação em lote.</span><span class="sxs-lookup"><span data-stu-id="389c1-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="389c1-319">O *habilitar nomes fortes em assemblies pré-compilados* caixa de seleção permite nome forte seus assemblies pré-compilados.</span><span class="sxs-lookup"><span data-stu-id="389c1-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-320">No ASP.NET 1. x, assemblies de nomes fortes precisava ser instalado no Cache de Assembly Global (GAC).</span><span class="sxs-lookup"><span data-stu-id="389c1-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="389c1-321">No ASP.NET 2.0, não é necessário para instalar assemblies de nome forte no GAC.</span><span class="sxs-lookup"><span data-stu-id="389c1-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![Um arquivos de pré-compilados de aplicativos ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="389c1-323">**Figura 11**: um arquivos de pré-compilados de aplicativos ASP.NET</span><span class="sxs-lookup"><span data-stu-id="389c1-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="389c1-324">No aplicativo acima, não havia nenhum arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="389c1-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="389c1-325">Se tivesse sido, ele poderia ter sido chamado *Precompiledapp* depois de publicar Web sites processo.</span><span class="sxs-lookup"><span data-stu-id="389c1-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="389c1-326">Melhorias na implantação</span><span class="sxs-lookup"><span data-stu-id="389c1-326">Improvements in Deployment</span></span>

<span data-ttu-id="389c1-327">Como com o Visual Studio 2002 e 2003, o Visual Studio 2005 oferece um recurso de cópia de projeto.</span><span class="sxs-lookup"><span data-stu-id="389c1-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="389c1-328">No entanto, o recurso foi reforçado no Visual Studio 2005 e agora é chamado de Site da Web de cópia.</span><span class="sxs-lookup"><span data-stu-id="389c1-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="389c1-329">A caixa de diálogo do Site da Web de cópia é dividida em um quadro à esquerda e um quadro direito.</span><span class="sxs-lookup"><span data-stu-id="389c1-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="389c1-330">O quadro esquerdo é chamado de Site de origem e o quadro direito é chamado de Site remoto.</span><span class="sxs-lookup"><span data-stu-id="389c1-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="389c1-331">Uma coisa que pode confundir alguns desenvolvedores é que o site exibido no quadro à direita não é necessariamente um site remoto.</span><span class="sxs-lookup"><span data-stu-id="389c1-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="389c1-332">Pode ser um site no sistema de arquivos local ou na instância local do IIS.</span><span class="sxs-lookup"><span data-stu-id="389c1-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="389c1-333">Além disso, o site exibido no quadro esquerdo não é necessariamente o site de origem porque a caixa de diálogo permite que você publicar do site remoto *para* o site de origem.</span><span class="sxs-lookup"><span data-stu-id="389c1-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="389c1-334">Se você estiver copiando um projeto para um site remoto, esse site deve ter as extensões de servidor do FrontPage instalado nele.</span><span class="sxs-lookup"><span data-stu-id="389c1-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="389c1-335">Se não estiver, você precisará se conectar usando o FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="389c1-336">Por outro lado, se você estiver copiando um projeto para a instância local do IIS, FrontPage Server Extensions não são necessários.</span><span class="sxs-lookup"><span data-stu-id="389c1-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-337">Se você tentar criar um novo site na instância local do IIS e as extensões de servidor do FrontPage 2002 estiverem instalados, você receberá uma mensagem de erro informando que a criação de sites da Web não é suportado em um servidor do SharePoint.</span><span class="sxs-lookup"><span data-stu-id="389c1-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="389c1-338">Nesse caso, você tem a opção de instalar as extensões de servidor do FrontPage 2000 ou remover extensões de servidor do FrontPage.</span><span class="sxs-lookup"><span data-stu-id="389c1-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="389c1-339">Clique aqui para uma orientação em vídeo do recurso de cópia da Web Site.</span><span class="sxs-lookup"><span data-stu-id="389c1-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="389c1-340">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="389c1-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="389c1-341">Melhorias na depuração</span><span class="sxs-lookup"><span data-stu-id="389c1-341">Improvements in Debugging</span></span>

<span data-ttu-id="389c1-342">Há quatro principais melhorias na depuração no Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="389c1-343">Depurando localmente como um administrador não é possível fora da caixa.</span><span class="sxs-lookup"><span data-stu-id="389c1-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="389c1-344">O atributo de depuração para o elemento de compilação agora é false por padrão.</span><span class="sxs-lookup"><span data-stu-id="389c1-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="389c1-345">Instalação e configuração de depuração remota é mais fácil do que antes.</span><span class="sxs-lookup"><span data-stu-id="389c1-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="389c1-346">Agora você pode depurar um site da Web aberto por meio de um local FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="389c1-347">Depuração como não-administrador</span><span class="sxs-lookup"><span data-stu-id="389c1-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="389c1-348">A adição do servidor de desenvolvimento do ASP.NET permite que não administradores depurem facilmente aplicativos ASP.NET imediatamente.</span><span class="sxs-lookup"><span data-stu-id="389c1-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="389c1-349">Quando um aplicativo ASP.NET em execução no sistema de arquivos local é depurado, o Visual Studio inicia o ASP.NET Development Server sob o contexto do usuário conectado.</span><span class="sxs-lookup"><span data-stu-id="389c1-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="389c1-350">Esse usuário, em seguida, poderá depurar o aplicativo sem qualquer configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="389c1-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="389c1-351">Depuração é False por padrão</span><span class="sxs-lookup"><span data-stu-id="389c1-351">Debug is False by Default</span></span>

<span data-ttu-id="389c1-352">No ASP.NET 1. x, o *depurar* atributo o *compilação* elemento do arquivo Web. config foi definido como *true* por padrão.</span><span class="sxs-lookup"><span data-stu-id="389c1-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="389c1-353">Ele sempre tenha sido recomendado que os desenvolvedores definir esse atributo para *false* antes de implantar um aplicativo para produção, mas porque a maioria dos desenvolvedores não entender as consequências de deixar o atributo para depuração True, eles simplesmente da esquerda como-é.</span><span class="sxs-lookup"><span data-stu-id="389c1-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="389c1-354">O problema mais grave com o atributo de depuração que tenha definido como true é que ele desabilita o modelo de compilação em lote ASP.NETs.</span><span class="sxs-lookup"><span data-stu-id="389c1-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="389c1-355">Portanto, cada página é compilada em uma DLL separada.</span><span class="sxs-lookup"><span data-stu-id="389c1-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="389c1-356">Se uma Web aplicativo consiste em milhares de páginas (não incomum de por qualquer meio), o que significa que várias DLLs pequenos milhar serão criados por esse aplicativo.</span><span class="sxs-lookup"><span data-stu-id="389c1-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="389c1-357">Embora essas DLLs estão tamanho pequenos, elas não são carregadas em qualquer local específico na memória.</span><span class="sxs-lookup"><span data-stu-id="389c1-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="389c1-358">Portanto, eles causam fragmentação na memória do sistema e podem contribuir com OutOfMemoryException ocorrências.</span><span class="sxs-lookup"><span data-stu-id="389c1-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="389c1-359">No ASP.NET 2.0, o atributo de depuração é definido como false por padrão.</span><span class="sxs-lookup"><span data-stu-id="389c1-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="389c1-360">Como já vimos, quando um desenvolvedor depura um aplicativo ASP.NET no Visual Studio 2005, ele serão solicitados a adicionar um arquivo Web. config com depuração habilitada.</span><span class="sxs-lookup"><span data-stu-id="389c1-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="389c1-361">Isso gera as mesmo desvantagens que estavam presentes no ASP.NET 1. x, mas agora o desenvolvedor claramente é avisado de que o atributo deve ser redefinido como false antes de mover o aplicativo para produção.</span><span class="sxs-lookup"><span data-stu-id="389c1-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="389c1-362">Instalação e configuração de depuração remota</span><span class="sxs-lookup"><span data-stu-id="389c1-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="389c1-363">Depuração remota no Visual Studio 2002/2003, dependia Machine Debug Manager (mdm.exe) e o processo de vs7jit.exe.</span><span class="sxs-lookup"><span data-stu-id="389c1-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="389c1-364">Por isso, solução de problemas de depuração remotos geralmente era uma caixa preta para clientes e geralmente não era muito melhor para serviços de suporte.</span><span class="sxs-lookup"><span data-stu-id="389c1-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="389c1-365">O Visual Studio 2005 remove a dependência de processos de mdm.exe e vs7jit.exe.</span><span class="sxs-lookup"><span data-stu-id="389c1-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="389c1-366">Em vez disso, ele agora usa o serviço do Monitor de depuração remoto (msvsmon.exe).</span><span class="sxs-lookup"><span data-stu-id="389c1-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="389c1-367">O requisito para a depuração remota no Visual Studio 2005 é muito simple.</span><span class="sxs-lookup"><span data-stu-id="389c1-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="389c1-368">Você precisa executar o msvsmon.exe no servidor remoto antes da depuração.</span><span class="sxs-lookup"><span data-stu-id="389c1-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="389c1-369">Você pode instalar o Monitor de depuração remota do CD do Visual Studio ou você pode simplesmente executar msvsmon.exe de um compartilhamento sem instalar nada em todos os no servidor Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="389c1-370">Quando você executa o msvsmon.exe, é provável que ele emitirá um aviso sobre as portas que estão sendo bloqueadas para depuração remota.</span><span class="sxs-lookup"><span data-stu-id="389c1-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="389c1-371">Felizmente, você poderá desbloquear as portas da direita na caixa de diálogo de aviso facilmente conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Notificação de que o Firewall do Windows está bloqueando a depuração remota](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="389c1-373">**Figura 12**: notificação de que o Firewall do Windows está bloqueando a depuração remota</span><span class="sxs-lookup"><span data-stu-id="389c1-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="389c1-374">Depois que você tiver desbloqueado as portas necessárias para a depuração, você verá o Monitor de depuração remota conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="389c1-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="389c1-375">Essa interface, você pode monitorar as conexões e alterar permissões de depuração facilmente.</span><span class="sxs-lookup"><span data-stu-id="389c1-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![O Monitor de depuração remota](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="389c1-377">**Figura 13**: O Monitor de depuração remota</span><span class="sxs-lookup"><span data-stu-id="389c1-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="389c1-378">Também é possível depurar remotamente um aplicativo Web aberto por meio de FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="389c1-379">As etapas são as mesmas que as abordadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="389c1-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="389c1-380">No entanto, você precisará especificar a URL base para procurar o projeto FTP, conforme descrito neste módulo.</span><span class="sxs-lookup"><span data-stu-id="389c1-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="389c1-381">Laboratório 2</span><span class="sxs-lookup"><span data-stu-id="389c1-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="389c1-382">Depuração remota com o Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="389c1-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="389c1-383">Este laboratório irá orientá-lo por meio de depuração remota com o Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="389c1-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="389c1-384">Clique aqui para obter uma explicação de vídeo deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="389c1-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="389c1-385">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="389c1-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="389c1-386">Este laboratório requer que você tenha duas máquinas, uma execução Visual Studio 2005 e o outro em execução do IIS 5 ou superior.</span><span class="sxs-lookup"><span data-stu-id="389c1-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="389c1-387">Abra o Visual Studio 2005 e crie um novo site no servidor remoto.</span><span class="sxs-lookup"><span data-stu-id="389c1-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="389c1-388">Você pode criar o site da Web em uma instância remota do IIS ou por meio de FTP.</span><span class="sxs-lookup"><span data-stu-id="389c1-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="389c1-389">Do servidor Web remoto, localize o msvsmon.exe no computador de desenvolvimento usando um caminho UNC e executá-lo.</span><span class="sxs-lookup"><span data-stu-id="389c1-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="389c1-390">O local padrão para msvsmon.exe é //server/c$/Program arquivos para o Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span><span class="sxs-lookup"><span data-stu-id="389c1-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="389c1-391">Se você precisará desbloquear portas para depuração remota, faça isso.</span><span class="sxs-lookup"><span data-stu-id="389c1-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="389c1-392">Da máquina de desenvolvimento, abra o code-behind para default. aspx e defina um ponto de interrupção no método de carga.</span><span class="sxs-lookup"><span data-stu-id="389c1-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="389c1-393">Inicie a depuração da máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="389c1-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="389c1-394">Você deve atingir o ponto de interrupção conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="389c1-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="389c1-395">ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="389c1-395">ASP.NET Development Server</span></span>

<span data-ttu-id="389c1-396">Como weve já discutido, Visual Studio 2005 vem com um servidor Web chamado de servidor de desenvolvimento do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="389c1-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="389c1-397">(O servidor de desenvolvimento ASP.NET é também conhecido como Cassini.) Esse servidor Web é uma maneira conveniente de procurar e depurar aplicativos da Web em execução no sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="389c1-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="389c1-398">O servidor de desenvolvimento ASP.NET é um servidor da Web restrito.</span><span class="sxs-lookup"><span data-stu-id="389c1-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="389c1-399">Ele não permite conexões remotas, ele não permite todas as solicitações de qualquer usuário que não seja o usuário que iniciou o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="389c1-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="389c1-400">Ele também não tem a capacidade de servir páginas ASP.</span><span class="sxs-lookup"><span data-stu-id="389c1-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="389c1-401">Somente os recursos ASP.NET e recursos HTML (incluindo imagens, arquivos CSS, etc.) são atendidos.</span><span class="sxs-lookup"><span data-stu-id="389c1-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="389c1-402">O ASP.NET Development Server pode ser iniciado por meio da linha de comando executando o arquivo WebDev.WebServer.exe localizado em c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="389c1-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="389c1-403">A caixa de diálogo a seguir exibe os parâmetros que estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="389c1-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="389c1-404">**Figura 14**</span><span class="sxs-lookup"><span data-stu-id="389c1-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="389c1-405">Não há suporte para o servidor de desenvolvimento ASP.NET quando iniciado explicitamente por meio da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="389c1-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
