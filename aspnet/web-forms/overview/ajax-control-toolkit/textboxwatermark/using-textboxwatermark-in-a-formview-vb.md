---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Usando TextBoxWatermark em um FormView (VB) | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 5789ccc4b2c6385f3476857dce139a8b47c5e5e8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872527"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a><span data-ttu-id="da0d2-104">Usando TextBoxWatermark em um FormView (VB)</span><span class="sxs-lookup"><span data-stu-id="da0d2-104">Using TextBoxWatermark in a FormView (VB)</span></span>
====================
<span data-ttu-id="da0d2-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="da0d2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="da0d2-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="da0d2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)</span></span>

> <span data-ttu-id="da0d2-107">O controle TextBoxWatermark AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="da0d2-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="da0d2-108">Quando um usuário clica na caixa, é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="da0d2-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="da0d2-109">Se o usuário deixar a caixa sem inserir texto, o texto pré-preenchidos reaparecerá.</span><span class="sxs-lookup"><span data-stu-id="da0d2-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="da0d2-110">Isso também é possível em um controle FormView.</span><span class="sxs-lookup"><span data-stu-id="da0d2-110">This is also possible within a FormView control.</span></span>


## <a name="overview"></a><span data-ttu-id="da0d2-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="da0d2-111">Overview</span></span>

<span data-ttu-id="da0d2-112">O `TextBoxWatermark` controle AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="da0d2-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="da0d2-113">Quando um usuário clica na caixa, é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="da0d2-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="da0d2-114">Se o usuário deixar a caixa sem inserir texto, o texto pré-preenchidos reaparecerá.</span><span class="sxs-lookup"><span data-stu-id="da0d2-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="da0d2-115">Isso também é possível em um `FormView` controle.</span><span class="sxs-lookup"><span data-stu-id="da0d2-115">This is also possible within a `FormView` control.</span></span>

## <a name="steps"></a><span data-ttu-id="da0d2-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="da0d2-116">Steps</span></span>

<span data-ttu-id="da0d2-117">Em primeiro lugar, uma fonte de dados é necessária.</span><span class="sxs-lookup"><span data-stu-id="da0d2-117">First of all, a data source is required.</span></span> <span data-ttu-id="da0d2-118">Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="da0d2-118">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="da0d2-119">O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição express) e também está disponível como um download separado em [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="da0d2-119">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="da0d2-120">O banco de dados AdventureWorks faz parte dos bancos de dados de exemplo e exemplos do SQL Server 2005 (download em [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="da0d2-120">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="da0d2-121">A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e conecte o `AdventureWorks.mdf` arquivo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="da0d2-121">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="da0d2-122">Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="da0d2-122">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="da0d2-123">Se sua configuração for diferente, você precisa se adaptar as informações de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="da0d2-123">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="da0d2-124">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="da0d2-124">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

<span data-ttu-id="da0d2-125">Em seguida, adicione uma fonte de dados para a página que oferece suporte a `DELETE`, `INSERT` e `UPDATE` instruções SQL.</span><span class="sxs-lookup"><span data-stu-id="da0d2-125">Then, add a data source to the page which supports the `DELETE`, `INSERT` and `UPDATE` SQL statements.</span></span> <span data-ttu-id="da0d2-126">Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome de tabela (`Vendor`) com `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="da0d2-126">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="da0d2-127">A marcação a seguir mostra a sintaxe correta:</span><span class="sxs-lookup"><span data-stu-id="da0d2-127">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

<span data-ttu-id="da0d2-128">Lembrar o nome (`ID`) da fonte de dados, pois ele será usado o `DataSourceID` propriedade do `FormView` controle.</span><span class="sxs-lookup"><span data-stu-id="da0d2-128">Remember the name (`ID`) of the data source, since it will be used in the `DataSourceID` property of the `FormView` control.</span></span> <span data-ttu-id="da0d2-129">O `<InsertItemTemplate>` seção o `FormView` contém uma caixa de texto que será estendida a `TextBoxWatermarkExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="da0d2-129">The `<InsertItemTemplate>` section of the `FormView` contains a textbox which is extended by the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="da0d2-130">Certifique-se de que o extensor reside dentro do modelo e não fora dele.</span><span class="sxs-lookup"><span data-stu-id="da0d2-130">Make sure that the extender resides within the template and not outside of it.</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

<span data-ttu-id="da0d2-131">Agora quando o usuário altera para o modo de inserção do `FormView` controle, o campo de texto para o novo fornecedor é previamente preenchido graças a `TextBoxWatermarkExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="da0d2-131">Now when the user changes into the insert mode of the `FormView` control, the text field for the new vendor is prefilled thanks to the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="da0d2-132">Um clique dentro da caixa de texto permite que o texto de preenchimento desaparecer.</span><span class="sxs-lookup"><span data-stu-id="da0d2-132">A click inside the textbox lets the filler text disappear.</span></span>


<span data-ttu-id="da0d2-133">[![A marca d'água no campo é proveniente do extensor](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="da0d2-133">[![The watermark in the field comes from the extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)</span></span>

<span data-ttu-id="da0d2-134">A marca d'água no campo é proveniente do extensor ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-in-a-formview-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="da0d2-134">The watermark in the field comes from the extender ([Click to view full-size image](using-textboxwatermark-in-a-formview-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="da0d2-135">[Anterior](using-textboxwatermark-with-validation-controls-cs.md)
> [Próximo](using-textboxwatermark-with-validation-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="da0d2-135">[Previous](using-textboxwatermark-with-validation-controls-cs.md)
[Next](using-textboxwatermark-with-validation-controls-vb.md)</span></span>
