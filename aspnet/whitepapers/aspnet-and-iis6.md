---
uid: whitepapers/aspnet-and-iis6
title: Executando o ASP.NET 1.1 com o IIS 6.0 | Microsoft Docs
author: rick-anderson
description: "Enquanto o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6.0 um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="c1212-104">Executando o ASP.NET 1.1 com o IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="c1212-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="c1212-105">Enquanto o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão.</span><span class="sxs-lookup"><span data-stu-id="c1212-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="c1212-106">Este white paper descreve como habilitar o IIS 6.0 e ASP.NET 1.1 e recomenda várias definições de configuração para obter o desempenho ideal do IIS e ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c1212-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="c1212-107">Aplica-se a ASP.NET 1.1 e o IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="c1212-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="c1212-108">ASP.NET 1.1 é fornecido com o Windows Server 2003, que também inclui a versão mais recente do Internet Information Server (IIS) versão 6.0.</span><span class="sxs-lookup"><span data-stu-id="c1212-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="c1212-109">IIS 6.0 e ASP.NET 1.1 são projetados para integrar-se perfeitamente e o ASP.NET padrão agora é o novo modelo de processo de trabalho do IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="c1212-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="c1212-110">ASP.NET 1.1 não está instalado por padrão</span><span class="sxs-lookup"><span data-stu-id="c1212-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="c1212-111">Diferentemente das versões anteriores dos sistemas operacionais da Microsoft, o servidor de informações da Internet (IIS) não está habilitado por padrão. nem ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="c1212-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="c1212-112">Há duas opções para habilitar o IIS:</span><span class="sxs-lookup"><span data-stu-id="c1212-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="c1212-113">Habilitar o IIS, a opção #1 - o Assistente para configurar o servidor</span><span class="sxs-lookup"><span data-stu-id="c1212-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="c1212-114">Windows Server 2003 fornece um novo 'Assistente' para ajudá-lo a configurar adequadamente o servidor no modo desejado.</span><span class="sxs-lookup"><span data-stu-id="c1212-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="c1212-115">Para iniciar o assistente - Observe que, para executar o assistente, você deve ser conectado como um administrador - acesse: Iniciar | Programas | Ferramentas administrativas e selecione 'Configurar o servidor'.</span><span class="sxs-lookup"><span data-stu-id="c1212-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="c1212-116">Uma vez selecionada, você deverá ver a tela de abertura 'Assistente':</span><span class="sxs-lookup"><span data-stu-id="c1212-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="c1212-117">Clique em ' Avançar &gt;':</span><span class="sxs-lookup"><span data-stu-id="c1212-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="c1212-118">Clique em ' Avançar &gt;'</span><span class="sxs-lookup"><span data-stu-id="c1212-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="c1212-119">Nessa tela, será necessário selecionar ' servidor de aplicativos (IIS, ASP.NET) como as opções para configurar.</span><span class="sxs-lookup"><span data-stu-id="c1212-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="c1212-120">Clique em ' Avançar &gt;'.</span><span class="sxs-lookup"><span data-stu-id="c1212-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="c1212-121">Depois de selecionar esta opção para configurar o servidor como um servidor de aplicativos, a tela será exibida solicitando que recursos adicionais devem ser instalados.</span><span class="sxs-lookup"><span data-stu-id="c1212-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="c1212-122">Nenhuma opção é selecionada por padrão.</span><span class="sxs-lookup"><span data-stu-id="c1212-122">Neither option is selected by default.</span></span> <span data-ttu-id="c1212-123">Para habilitar o ASP.NET automaticamente, você precisa selecionar ' Habilitar ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="c1212-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="c1212-124">Clique em ' Avançar &gt;'.</span><span class="sxs-lookup"><span data-stu-id="c1212-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="c1212-125">Essa tela exibe quais opções estão a ser instalado.</span><span class="sxs-lookup"><span data-stu-id="c1212-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="c1212-126">Clique em ' Avançar &gt;'.</span><span class="sxs-lookup"><span data-stu-id="c1212-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="c1212-127">Você verá esta tela enquanto as opções que você selecionou estão sendo instaladas.</span><span class="sxs-lookup"><span data-stu-id="c1212-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="c1212-128">É normal para ver outros diálogo caixas aparecem como serviços estão sendo instalados.</span><span class="sxs-lookup"><span data-stu-id="c1212-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="c1212-129">Além disso, você pode solicitado para o local do CD de instalação do Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="c1212-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="c1212-130">Clique em ' Avançar &gt;' quando concluir.</span><span class="sxs-lookup"><span data-stu-id="c1212-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="c1212-131">Clique em 'Concluir' - o Windows Server 2003 está configurado para dar suporte a IIS 6.0 e ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="c1212-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="c1212-132">Habilitar o IIS, a opção #2 - configurar manualmente o IIS e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c1212-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="c1212-133">Se você não quiser usar o 'Assistente' opcionalmente você pode instalar o IIS 6.0 e ASP.NET 1.1 usando 'Adicionar ou remover programas' no painel de controle.</span><span class="sxs-lookup"><span data-stu-id="c1212-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="c1212-134">Primeiro, abra o painel de controle:</span><span class="sxs-lookup"><span data-stu-id="c1212-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="c1212-135">Em seguida, clique em 'Adicionar ou remover componentes do Windows' que abrirá o 'Assistente de componentes do Windows':</span><span class="sxs-lookup"><span data-stu-id="c1212-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="c1212-136">Realce e verifique o 'servidor de aplicativos ' e, em seguida, clique em "Detalhes"?</span><span class="sxs-lookup"><span data-stu-id="c1212-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="c1212-137">botão:</span><span class="sxs-lookup"><span data-stu-id="c1212-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="c1212-138">Para instalar o ASP.NET, verifique ' ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="c1212-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="c1212-139">Clique em 'Okey' para retornar ao Assistente de componentes do Windows.</span><span class="sxs-lookup"><span data-stu-id="c1212-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="c1212-140">Clique em ' Avançar &gt;' do Assistente de componentes do Windows para começar a instalação:</span><span class="sxs-lookup"><span data-stu-id="c1212-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="c1212-141">É normal para ver outros diálogo caixas aparecem como serviços estão sendo instalados.</span><span class="sxs-lookup"><span data-stu-id="c1212-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="c1212-142">Além disso, você pode solicitado para o local do CD de instalação do Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="c1212-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="c1212-143">Quando a instalação for concluída, você verá a última tela do Assistente de componentes do Windows:</span><span class="sxs-lookup"><span data-stu-id="c1212-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="c1212-144">IIS 6.0 e ASP.NET 1.1 estão configurados e disponíveis.</span><span class="sxs-lookup"><span data-stu-id="c1212-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="c1212-145">Configurações recomendadas</span><span class="sxs-lookup"><span data-stu-id="c1212-145">Recommended Settings</span></span>

<span data-ttu-id="c1212-146">Ao executar o ASP.NET 1.1 com o IIS 6.0, há várias configurações que são recomendadas para obter o desempenho ideal do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="c1212-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="c1212-147">Configurar limites de memória de processo de trabalho</span><span class="sxs-lookup"><span data-stu-id="c1212-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="c1212-148">Configurando a reciclagem do processo de trabalho</span><span class="sxs-lookup"><span data-stu-id="c1212-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="c1212-149">Configurar limites de memória de processo de trabalho</span><span class="sxs-lookup"><span data-stu-id="c1212-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="c1212-150">Por padrão o IIS 6.0 não definir um limite na quantidade de memória que o IIS tem permissão para usar.</span><span class="sxs-lookup"><span data-stu-id="c1212-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="c1212-151">AS PÁGINAS ASP. O recurso de Cache do NET se baseia em um limite de memória para que o Cache de forma proativa remover itens não usados da memória.</span><span class="sxs-lookup"><span data-stu-id="c1212-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="c1212-152">É recomendável que você configure a recurso do IIS 6.0 de reciclagem de memória.</span><span class="sxs-lookup"><span data-stu-id="c1212-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="c1212-153">Para configurar este abrir Gerenciador de serviços de informações da Internet (Iniciar | Programas | Ferramentas administrativas | Serviços de informações da Internet).</span><span class="sxs-lookup"><span data-stu-id="c1212-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="c1212-154">Quando aberto, expanda a pasta de 'Pools de aplicativos ':</span><span class="sxs-lookup"><span data-stu-id="c1212-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="c1212-155">Para cada pool de aplicativos:</span><span class="sxs-lookup"><span data-stu-id="c1212-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="c1212-156">Clique com botão direito no pool de aplicativos, por exemplo 'DefaultAppPool' e selecione 'Propriedades':</span><span class="sxs-lookup"><span data-stu-id="c1212-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="c1212-157">Em seguida, habilitar a reciclagem de memória clicando em um ' máximo de memória usada (em megabytes):'.</span><span class="sxs-lookup"><span data-stu-id="c1212-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="c1212-158">O valor não deve ser maior que a quantidade de memória física de (não virtual) no servidor, uma boa aproximação é 60% da memória física, ou seja, para um servidor com 512MB de memória física, selecione 310.</span><span class="sxs-lookup"><span data-stu-id="c1212-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="c1212-159">Também é recomendável que o máximo não exceda 800MB ao usar um espaço de endereço de 2GB.</span><span class="sxs-lookup"><span data-stu-id="c1212-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="c1212-160">Se o espaço de endereço de memória do servidor é 3GB, o limite máximo de memória do processo de trabalho pode ser de até 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="c1212-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="c1212-161">Clique em 'Aplicar' e 'Okey' para sair da caixa de diálogo Propriedades.</span><span class="sxs-lookup"><span data-stu-id="c1212-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="c1212-162">Repita essa etapa para todos os pools de aplicativos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="c1212-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="c1212-163">Configurando o trabalho de reciclagem</span><span class="sxs-lookup"><span data-stu-id="c1212-163">Configuring worker recycling</span></span>

<span data-ttu-id="c1212-164">Por padrão, o IIS 6.0 é configurado para reciclar o processo de trabalho cada 29 horas.</span><span class="sxs-lookup"><span data-stu-id="c1212-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="c1212-165">Este é um pouco agressivo para um aplicativo em execução ASP.NET e é recomendável que a reciclagem do processo de trabalho automático está desativado.</span><span class="sxs-lookup"><span data-stu-id="c1212-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="c1212-166">Para desativar a reciclagem de processo de trabalho automático, primeiro abra o Gerenciador de serviços de informações da Internet (Iniciar | Programas | Ferramentas administrativas | Serviços de informações da Internet).</span><span class="sxs-lookup"><span data-stu-id="c1212-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="c1212-167">Quando aberto, expanda a pasta de 'Pools de aplicativos ':</span><span class="sxs-lookup"><span data-stu-id="c1212-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="c1212-168">Para cada pool de aplicativos:</span><span class="sxs-lookup"><span data-stu-id="c1212-168">For each application pool:</span></span>

1. <span data-ttu-id="c1212-169">Clique com botão direito no pool de aplicativos, por exemplo 'DefaultAppPool' e selecione 'Propriedades':</span><span class="sxs-lookup"><span data-stu-id="c1212-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="c1212-170">Desmarque a opção ' Reciclar processos do operador (em minutos):':</span><span class="sxs-lookup"><span data-stu-id="c1212-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="c1212-171">Clique em 'Aplicar' e 'Okey' para sair da caixa de diálogo Propriedades.</span><span class="sxs-lookup"><span data-stu-id="c1212-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="c1212-172">Repita essa etapa para todos os pools de aplicativos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="c1212-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="c1212-173">Concedendo acesso de gravação para o sistema de arquivos</span><span class="sxs-lookup"><span data-stu-id="c1212-173">Granting write access to the file system</span></span>

<span data-ttu-id="c1212-174">Se seu aplicativo requer acesso de gravação para o sistema de arquivos e está usando o NTFS você precisará modificar uma lista de controle de acesso (ACL) no arquivo ou pasta para conceder acesso ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c1212-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="c1212-175">Por exemplo, conceder ASP.NET acesso de gravação para o c:\inetpub\wwwroot primeiro abra o explorer e navegue até o diretório:</span><span class="sxs-lookup"><span data-stu-id="c1212-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="c1212-176">Em seguida, clique no diretório, por exemplo, 'wwwroot' e selecione Propriedades.</span><span class="sxs-lookup"><span data-stu-id="c1212-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="c1212-177">Depois de abre a caixa de diálogo de propriedades, selecione a guia 'Segurança':</span><span class="sxs-lookup"><span data-stu-id="c1212-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="c1212-178">O diretório de c:\inetpub\wwwroot\ é um diretório especial em que o grupo especial do IIS 6.0 ' IIS\_WPG' já foi concedida a leitura &amp; permissões de execução, Listar conteúdo da pasta e leitura.</span><span class="sxs-lookup"><span data-stu-id="c1212-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="c1212-179">No entanto, para conceder permissão de gravação, você precisa clicar na caixa de seleção Permitir para gravação:</span><span class="sxs-lookup"><span data-stu-id="c1212-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="c1212-180">Agora, o IIS 6.0 tem permissão de gravação nessa pasta.</span><span class="sxs-lookup"><span data-stu-id="c1212-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="c1212-181">Para conceder permissões de gravação em outras pastas, execute as seguintes etapas - Observe que talvez seja necessário adicionar o IIS\_grupo WPG se ele ainda não existir.</span><span class="sxs-lookup"><span data-stu-id="c1212-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="c1212-182">Conceder permissão de gravação para o IIS\_WPG permitirá que qualquer aplicativo ASP.NET gravar nesse diretório.</span><span class="sxs-lookup"><span data-stu-id="c1212-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="c1212-183">Com suporte a autenticação integrada com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="c1212-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="c1212-184">Permite a autenticação integrada do SQL Server aproveitar a autenticação do Windows NT para validar as contas de logon do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1212-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="c1212-185">Isso permite que o usuário ignore o processo de logon padrão do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1212-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="c1212-186">Com essa abordagem, um usuário de rede pode acessar um banco de dados do SQL Server sem fornecer uma senha ou a identificação de logon separadas, porque o SQL Server obtém as informações de usuário e senha do processo de segurança de rede do Windows NT.</span><span class="sxs-lookup"><span data-stu-id="c1212-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="c1212-187">Escolher a autenticação integrada para aplicativos ASP.NET é uma boa opção porque nenhuma credencial nunca é armazenadas na cadeia de caracteres de conexão para o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1212-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="c1212-188">Em vez disso, a cadeia de conexão usada para se conectar ao SQL será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="c1212-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="c1212-189">Essa cadeia de caracteres de conexão informa ao SQL Server para usar as credenciais do Windows do aplicativo tentar acessar o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1212-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="c1212-190">No caso do ASP.NET /IIS 6, isso seria uma conta no IIS\_grupo WPG.</span><span class="sxs-lookup"><span data-stu-id="c1212-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="c1212-191">Para habilitar a autenticação integrada entre o SQL Server e o ASP.NET, você precisará primeiro certifique-se de que o SQL Server está configurado para autenticação integrada ou a autenticação de modo misto - Verifique com seu DBA para determinar isso.</span><span class="sxs-lookup"><span data-stu-id="c1212-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="c1212-192">Se o SQL Server estiver em um destes dois modos, você pode usar a autenticação integrada.</span><span class="sxs-lookup"><span data-stu-id="c1212-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="c1212-193">Abra o SQL Server Enterprise Manager (Iniciar | Programas | Microsoft SQL Server | Enterprise Manager), selecione o servidor apropriado e expanda a pasta de segurança:</span><span class="sxs-lookup"><span data-stu-id="c1212-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="c1212-194">Se ' BUILTINT\IIS\_WPG' grupo não estiver listado, clique em logons e selecione 'Novo logon':</span><span class="sxs-lookup"><span data-stu-id="c1212-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="c1212-195">No ' nome:' caixa de texto Digite ' \IIS [nome do domínio do servidor]\_WPG' ou clique no botão de reticências para abrir o selecionador de usuário/grupo do Windows NT:</span><span class="sxs-lookup"><span data-stu-id="c1212-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="c1212-196">Selecione IIS a máquina atual\_WPG grupo e clique em 'Adicionar' e Okey para fechar o seletor.</span><span class="sxs-lookup"><span data-stu-id="c1212-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="c1212-197">Em seguida, você precisa também definir o banco de dados padrão e as permissões para acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1212-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="c1212-198">Para definir o banco de dados padrão escolha na lista suspensa lista, por exemplo, a Northwind abaixo é selecionado:</span><span class="sxs-lookup"><span data-stu-id="c1212-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="c1212-199">Em seguida, clique na guia acesso de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="c1212-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="c1212-200">Clique na caixa de seleção para cada banco de dados que você deseja permitir o acesso à permissão.</span><span class="sxs-lookup"><span data-stu-id="c1212-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="c1212-201">Você também precisará selecionar funções de banco de dados, a verificação de banco de dados\_proprietário garantirá que o logon tem todas as permissões necessárias para gerenciar e usar o banco de dados selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1212-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="c1212-202">Clique em Okey para sair da caixa de diálogo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="c1212-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="c1212-203">Seu aplicativo ASP.NET agora está configurado para dar suporte à autenticação integrada do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1212-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="c1212-204">ASP.NET 1.0 não são executados em modo nativo do IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="c1212-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="c1212-205">Somente há suporte para o ASP.NET 1.0 no IIS 6.0 no modo de compatibilidade do IIS 5.</span><span class="sxs-lookup"><span data-stu-id="c1212-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="c1212-206">Para configurar o ASP.NET 1.0 para ser executado no modo de compatibilidade do IIS 5.0, abra o Gerenciador de serviços de Internet e Sites da Web de clique do botão direito e selecione Propriedades:</span><span class="sxs-lookup"><span data-stu-id="c1212-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="c1212-207">Alternar para a guia de serviço e verificar? Execute o serviço da WWW no modo de isolamento 5.0 do IIS?:</span><span class="sxs-lookup"><span data-stu-id="c1212-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
