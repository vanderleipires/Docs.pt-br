---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Usando um ConfirmButton em um repetidor (c#) | Microsoft Docs
author: wenz
description: "O Kit de ferramentas de controle AJAX do extensor de ConfirmButton cria um Sim/não pop-up quando o usuário clica em um botão (incluindo controle LinkButton). Somente se Sim for..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 12fd3e4e58e6a73720ade38372caff496f7f2c97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a><span data-ttu-id="77648-104">Usando um ConfirmButton em um repetidor (c#)</span><span class="sxs-lookup"><span data-stu-id="77648-104">Using a ConfirmButton In a Repeater (C#)</span></span>
====================
<span data-ttu-id="77648-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="77648-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="77648-106">[Baixar o código](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="77648-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span></span>

> <span data-ttu-id="77648-107">O Kit de ferramentas de controle AJAX do extensor de ConfirmButton cria um Sim/não pop-up quando o usuário clica em um botão (incluindo controle LinkButton).</span><span class="sxs-lookup"><span data-stu-id="77648-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="77648-108">Somente se Sim for selecionada, a ação do botão é executada, caso contrário cancelada.</span><span class="sxs-lookup"><span data-stu-id="77648-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="77648-109">Isso também é possível em um repetidor.</span><span class="sxs-lookup"><span data-stu-id="77648-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="77648-110">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="77648-110">Overview</span></span>

<span data-ttu-id="77648-111">O Kit de ferramentas de controle AJAX do extensor de ConfirmButton cria um Sim/não pop-up quando o usuário clica em um botão (incluindo controle LinkButton).</span><span class="sxs-lookup"><span data-stu-id="77648-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="77648-112">Somente se Sim for selecionada, a ação do botão é executada, caso contrário cancelada.</span><span class="sxs-lookup"><span data-stu-id="77648-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="77648-113">Isso também é possível em um repetidor.</span><span class="sxs-lookup"><span data-stu-id="77648-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="77648-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="77648-114">Steps</span></span>

<span data-ttu-id="77648-115">Em primeiro lugar, uma fonte de dados é necessária.</span><span class="sxs-lookup"><span data-stu-id="77648-115">First of all, a data source is required.</span></span> <span data-ttu-id="77648-116">Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="77648-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="77648-117">O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="77648-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="77648-118">O banco de dados AdventureWorks faz parte dos bancos de dados de exemplo e exemplos do SQL Server 2005 (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="77648-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="77648-119">A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e conecte o `AdventureWorks.mdf` arquivo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77648-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="77648-120">Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="77648-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="77648-121">Se sua configuração for diferente, você precisa se adaptar as informações de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77648-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="77648-122">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="77648-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

<span data-ttu-id="77648-123">Em seguida, uma fonte de dados é necessária.</span><span class="sxs-lookup"><span data-stu-id="77648-123">Then, a data source is required.</span></span> <span data-ttu-id="77648-124">Para simplificar, somente as primeiras cinco entradas na tabela de fornecedores de AdventureWorks são recuperadas.</span><span class="sxs-lookup"><span data-stu-id="77648-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="77648-125">Observe que ao usar o Assistente do Visual Studio para criar a fonte de dados, o nome da tabela (`Vendors`) atualmente não está corretamente prefixado com `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="77648-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="77648-126">A seguinte marcação está correto:</span><span class="sxs-lookup"><span data-stu-id="77648-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

<span data-ttu-id="77648-127">Esta fonte de dados, em seguida, pode ser usada dentro de um repetidor.</span><span class="sxs-lookup"><span data-stu-id="77648-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="77648-128">Como de costume, o `DataBinder.Eval()` método recupera dados da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="77648-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="77648-129">O `ConfirmButtonExtender` controle deve ser colocado dentro de `<ItemTemplate>` seção do repetidor para que ele apareça para cada entrada na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="77648-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


<span data-ttu-id="77648-130">[![Botão Confirmar aparece ao lado de cada entrada da fonte de dados](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="77648-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span></span>

<span data-ttu-id="77648-131">Botão Confirmar aparece ao lado de cada entrada da fonte de dados ([clique para exibir a imagem em tamanho normal](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="77648-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="77648-132">Avançar</span><span class="sxs-lookup"><span data-stu-id="77648-132">Next</span></span>](using-a-confirmbutton-in-a-repeater-vb.md)
