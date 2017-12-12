---
uid: whitepapers/denied-access-to-iis-directories
title: "ASP.NET negado acesso a diretórios IIS | Microsoft Docs"
author: rick-anderson
description: "Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro \"acesso negado ao diretório DirectoryName. Falha ao s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="1e25e-104">Acesso negado para diretórios do IIS do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1e25e-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="1e25e-105">Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro "acesso negado ao *DirectoryName* directory.</span><span class="sxs-lookup"><span data-stu-id="1e25e-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="1e25e-106">Falha ao iniciar o monitoramento de diretório chaanges."</span><span class="sxs-lookup"><span data-stu-id="1e25e-106">Failed to start monitoring directory chaanges."</span></span>
> 
> <span data-ttu-id="1e25e-107">Aplica-se ao ASP.NET 1.0 e 1.1 do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e25e-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="1e25e-108">RTM do ASP.NET V1 agora é executada usando um menor com privilégios de conta do windows - registrada como a conta "ASPNET" em um computador local.</span><span class="sxs-lookup"><span data-stu-id="1e25e-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="1e25e-109">Em alguns bloqueados sistemas, essa conta pode não por padrão têm acesso de leitura segurança para diretórios de conteúdo do site, o diretório raiz do aplicativo ou o diretório raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1e25e-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="1e25e-110">Nesse caso, você receberá o seguinte erro ao solicitar páginas de um determinado aplicativo web:</span><span class="sxs-lookup"><span data-stu-id="1e25e-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="1e25e-111">Para corrigir isso, você precisará alterar as permissões de segurança nos diretórios apropriados.</span><span class="sxs-lookup"><span data-stu-id="1e25e-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="1e25e-112">Especificamente, o ASP.NET exige leitura, executar e lista de acesso para a conta ASPNET para a raiz do site da web (por exemplo: c:\inetpub\wwwroot ou qualquer diretório de site alternativo, talvez você tenha configurado no IIS), o diretório de conteúdo e o diretório raiz do aplicativo para monitorar as alterações de arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="1e25e-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="1e25e-113">A raiz do aplicativo corresponde ao caminho da pasta associado com o diretório virtual do aplicativo na ferramenta de administração do IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="1e25e-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="1e25e-114">Por exemplo, considere a seguinte hierarquia de aplicativo na pasta wwwroot.</span><span class="sxs-lookup"><span data-stu-id="1e25e-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="1e25e-115">Neste exemplo, a conta ASPNET precisa de permissões de leitura definidas acima para o conteúdo de myapp e o diretório wwwroot.</span><span class="sxs-lookup"><span data-stu-id="1e25e-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="1e25e-116">Uma ACL herdada único na pasta raiz também pode ser usada opcionalmente para ambos os diretórios se ele estão aninhados.</span><span class="sxs-lookup"><span data-stu-id="1e25e-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="1e25e-117">Para adicionar permissões para um diretório, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="1e25e-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="1e25e-118">Usando o Windows explorer, navegue até o diretório</span><span class="sxs-lookup"><span data-stu-id="1e25e-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="1e25e-119">Clique com o botão direito na pasta do diretório e escolha "Propriedades"</span><span class="sxs-lookup"><span data-stu-id="1e25e-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="1e25e-120">Navegue até a guia "Segurança" na caixa de diálogo de propriedade</span><span class="sxs-lookup"><span data-stu-id="1e25e-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="1e25e-121">Clique no botão "Adicionar" e insira o nome do computador seguido pelo nome da conta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="1e25e-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="1e25e-122">Por exemplo, em um computador chamado "webdev", você poderia insira webdev\ASPNET e ocorrências de "Okey".</span><span class="sxs-lookup"><span data-stu-id="1e25e-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="1e25e-123">Verifique se a conta ASPNET tem de "leitura &amp; Execute", "Listar conteúdo da pasta" e "Leitura" caixas de seleção marcadas.</span><span class="sxs-lookup"><span data-stu-id="1e25e-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="1e25e-124">Acertos Okey para fechar a caixa de diálogo e salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="1e25e-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="1e25e-125">Se desejar, essas alterações podem ser automatizadas usando scripts ou a ferramenta "cacls.exe" que é fornecido com o Windows.</span><span class="sxs-lookup"><span data-stu-id="1e25e-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="1e25e-126">Para obter mais informações sobre a conta ASPNET, consulte o [documento perguntas frequentes sobre](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="1e25e-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="1e25e-127">Se um determinado aplicativo web depende de ter gravar ou modificar permissões para um arquivo ou pasta específica, isso pode receber o mesmo procedimento a seguir e marcando as caixas de seleção "Gravação" e/ou "Modificar".</span><span class="sxs-lookup"><span data-stu-id="1e25e-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="1e25e-128">Em computadores que permitem que todos, ou o acesso de leitura do grupo de usuários para esses diretórios (que é a configuração padrão), nenhum problema será encontrado e as etapas acima não será necessárias.</span><span class="sxs-lookup"><span data-stu-id="1e25e-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
