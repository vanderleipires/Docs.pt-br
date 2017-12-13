---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Usando ModalPopup com um controle repetidor (VB) | Microsoft Docs
author: wenz
description: "O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. Também é possível usar esse contr...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a1e49fdebfb3ad62667ffd5a979d366730a097bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a><span data-ttu-id="3a1a3-104">Usando ModalPopup com um controle repetidor (VB)</span><span class="sxs-lookup"><span data-stu-id="3a1a3-104">Using ModalPopup with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="3a1a3-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3a1a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3a1a3-106">[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3a1a3-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span></span>

> <span data-ttu-id="3a1a3-107">O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="3a1a3-108">Também é possível usar esse controle dentro de um repetidor.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="3a1a3-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="3a1a3-109">Overview</span></span>

<span data-ttu-id="3a1a3-110">O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="3a1a3-111">Também é possível usar esse controle dentro de um repetidor.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="3a1a3-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="3a1a3-112">Steps</span></span>

<span data-ttu-id="3a1a3-113">Em primeiro lugar, uma fonte de dados é necessária.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-113">First of all, a data source is required.</span></span> <span data-ttu-id="3a1a3-114">Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="3a1a3-115">O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="3a1a3-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="3a1a3-116">O banco de dados AdventureWorks faz parte dos bancos de dados de exemplo e exemplos do SQL Server 2005 (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="3a1a3-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="3a1a3-117">A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e conecte o `AdventureWorks.mdf` arquivo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="3a1a3-118">Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="3a1a3-119">Se sua configuração for diferente, você precisa se adaptar as informações de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="3a1a3-120">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="3a1a3-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="3a1a3-121">Em seguida, adicione uma fonte de dados para a página.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-121">Then, add a data source to the page.</span></span> <span data-ttu-id="3a1a3-122">Para usar uma quantidade limitada de dados, podemos selecionar somente as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="3a1a3-123">Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome de tabela (`Vendor`) com `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="3a1a3-124">A marcação a seguir mostra a sintaxe correta:</span><span class="sxs-lookup"><span data-stu-id="3a1a3-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="3a1a3-125">Em seguida, adicione um painel que serve como o pop-up modal.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="3a1a3-126">Ele contém um `Button` controle para fechar o janela pop-up novamente:</span><span class="sxs-lookup"><span data-stu-id="3a1a3-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="3a1a3-127">Para tornar o pop-up funcionam no repetidor, o `ModalPopupExtender` controle deve ser colocado dentro de `<ItemTemplate>` seção do repetidor.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="3a1a3-128">Portanto, o painel está fora do repetidor, mas o extensor está dentro.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="3a1a3-129">Aqui está a marcação para repetidor:</span><span class="sxs-lookup"><span data-stu-id="3a1a3-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="3a1a3-130">Em seguida, todos os itens na fonte de dados é exibido com um botão ao lado dele que dispara o popup modal.</span><span class="sxs-lookup"><span data-stu-id="3a1a3-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="3a1a3-131">[![O pop-up modal pode ser acionado para cada entrada de fonte de dados](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3a1a3-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="3a1a3-132">O pop-up modal pode ser acionado para cada entrada de fonte de dados ([clique para exibir a imagem em tamanho normal](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3a1a3-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3a1a3-133">[Anterior](launching-a-modal-popup-window-from-server-code-vb.md)
[Próximo](handling-postbacks-from-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3a1a3-133">[Previous](launching-a-modal-popup-window-from-server-code-vb.md)
[Next](handling-postbacks-from-a-modalpopup-vb.md)</span></span>
