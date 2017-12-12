---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: "Associação de dados com Accordion (VB) | Microsoft Docs"
author: wenz
description: "O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são normalmente declaradas w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: aeff732e4daed6ed22fd5f3b6adcdeb6082aae53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-vb"></a>Associação de dados com Accordion (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis geralmente são declarados dentro da página em si, mas a associação a uma fonte de dados oferece mais flexibilidade.


## <a name="overview"></a>Visão Geral

O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis geralmente são declarados dentro da página em si, mas a associação a uma fonte de dados oferece mais flexibilidade.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks faz parte dos bancos de dados de exemplo e exemplos do SQL Server 2005 (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e conecte o `AdventureWorks.mdf` arquivo de banco de dados.

Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão. Se sua configuração for diferente, você precisa se adaptar as informações de conexão para o banco de dados.

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

Em seguida, adicione uma fonte de dados para a página. Para usar uma quantidade limitada de dados, podemos selecionar somente as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks. Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome de tabela (`Vendor`) com `Purchasing`. A marcação a seguir mostra a sintaxe correta:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Lembre-se o nome (ID) da fonte de dados. Essa identificação muito deve ser usada no `DataSourceID` propriedade do controle Accordion:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

Dentro do controle Accordion, você pode fornecer modelos de várias partes do controle, incluindo o cabeçalho (`<HeaderTemplate>`) e o conteúdo (`<ContentTemplate>`). Dentro desses elementos, exibir apenas os dados dos dados de origem, usando o `DataBinder.Eval()` método:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

Quando a página for carregada, a fonte de dados deve ser vinculada a accordion com este código do lado do servidor:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Para concluir este exemplo, você precisa definir duas classes CSS que são referenciadas no controle Accordion (em suas propriedades `HeaderCssClass` e `ContentCssClass`). Coloque a seguinte marcação no `<head>` seção da página:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Os dados de accordion vêm diretamente da fonte de dados](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Os dados de accordion vêm diretamente da fonte de dados ([clique para exibir a imagem em tamanho normal](databinding-to-an-accordion-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](dynamically-adding-an-accordion-pane-cs.md)
[Próximo](dynamically-adding-an-accordion-pane-vb.md)
