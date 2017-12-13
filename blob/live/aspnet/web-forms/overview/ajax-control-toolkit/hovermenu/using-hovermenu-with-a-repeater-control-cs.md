---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Usando HoverMenu com um controle repetidor (c#) | Microsoft Docs
author: wenz
description: "O controle HoverMenu no AJAX Control Toolkit fornece um efeito de pop-up simples: quando o ponteiro do mouse passa sobre um elemento, um pop-up é exibido em pela especificação..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aac5a26191cc633204549274c327e065578f4226
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-c"></a><span data-ttu-id="4777a-103">Usando HoverMenu com um controle repetidor (c#)</span><span class="sxs-lookup"><span data-stu-id="4777a-103">Using HoverMenu with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="4777a-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4777a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4777a-105">[Baixar o código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4777a-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span></span>

> <span data-ttu-id="4777a-106">O controle HoverMenu no AJAX Control Toolkit fornece um efeito de pop-up simples: quando o ponteiro do mouse passa sobre um elemento, um pop-up aparece em uma posição especificada.</span><span class="sxs-lookup"><span data-stu-id="4777a-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="4777a-107">Também é possível usar esse controle dentro de um repetidor.</span><span class="sxs-lookup"><span data-stu-id="4777a-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="4777a-108">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="4777a-108">Overview</span></span>

<span data-ttu-id="4777a-109">O `HoverMenu` controle no AJAX Control Toolkit fornece um efeito de pop-up simples: quando o ponteiro do mouse passa sobre um elemento, um pop-up aparece em uma posição especificada.</span><span class="sxs-lookup"><span data-stu-id="4777a-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="4777a-110">Também é possível usar esse controle dentro de um repetidor.</span><span class="sxs-lookup"><span data-stu-id="4777a-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="4777a-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="4777a-111">Steps</span></span>

<span data-ttu-id="4777a-112">Em primeiro lugar, uma fonte de dados é necessária.</span><span class="sxs-lookup"><span data-stu-id="4777a-112">First of all, a data source is required.</span></span> <span data-ttu-id="4777a-113">Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="4777a-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="4777a-114">O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="4777a-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="4777a-115">O banco de dados AdventureWorks faz parte dos bancos de dados de exemplo e exemplos do SQL Server 2005 (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="4777a-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="4777a-116">A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e conecte o `AdventureWorks.mdf` arquivo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4777a-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="4777a-117">Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="4777a-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="4777a-118">Se sua configuração for diferente, você precisa se adaptar as informações de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4777a-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="4777a-119">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="4777a-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="4777a-120">Em seguida, adicione uma fonte de dados para a página.</span><span class="sxs-lookup"><span data-stu-id="4777a-120">Then, add a data source to the page.</span></span> <span data-ttu-id="4777a-121">Para usar uma quantidade limitada de dados, podemos selecionar somente as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="4777a-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="4777a-122">Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome de tabela (`Vendor`) com `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="4777a-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="4777a-123">A marcação a seguir mostra a sintaxe correta:</span><span class="sxs-lookup"><span data-stu-id="4777a-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="4777a-124">Em seguida, adicione um painel que serve como o pop-up modal:</span><span class="sxs-lookup"><span data-stu-id="4777a-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="4777a-125">Agora, o `HoverMenuExtender` entra em cena.</span><span class="sxs-lookup"><span data-stu-id="4777a-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="4777a-126">Para que cada elemento na fonte de dados obtém seu próprio pop-up, o extensor deve ser colocado dentro do repetidor `<ItemTemplate>` seção.</span><span class="sxs-lookup"><span data-stu-id="4777a-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="4777a-127">Aqui está a marcação:</span><span class="sxs-lookup"><span data-stu-id="4777a-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="4777a-128">Agora cada item na fonte de dados exibe um pop-up para a direita (`PopupPosition` atributo) após um intervalo de 50 milissegundos (`PopDelay` atributo).</span><span class="sxs-lookup"><span data-stu-id="4777a-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="4777a-129">[![O menu de em foco é exibido ao lado de cada item no repetidor](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4777a-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="4777a-130">O menu de em foco é exibido ao lado de cada item no repetidor ([clique para exibir a imagem em tamanho normal](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4777a-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4777a-131">Avançar</span><span class="sxs-lookup"><span data-stu-id="4777a-131">Next</span></span>](using-hovermenu-with-a-repeater-control-vb.md)
