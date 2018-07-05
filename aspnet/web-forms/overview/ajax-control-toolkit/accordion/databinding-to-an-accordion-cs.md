---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Associação de dados com um Accordion (c#) | Microsoft Docs
author: wenz
description: O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis são normalmente declaradas w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 8441eb15d369085b93297ae896d595a40d4099fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394006"
---
<a name="databinding-to-an-accordion-c"></a>Associação de dados com um Accordion (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis geralmente são declarados dentro da página em si, mas a associação a uma fonte de dados oferece mais flexibilidade.


## <a name="overview"></a>Visão geral

O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis geralmente são declarados dentro da página em si, mas a associação a uma fonte de dados oferece mais flexibilidade.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional da instalação do Visual Studio (inclusive a express edition) e também está disponível como um download separado em [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks é parte do SQL Server 2005 exemplos e bancos de dados de exemplo (download em [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexe o `AdventureWorks.mdf` arquivo de banco de dados.

Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão. Se sua configuração for diferente, você precisa adaptar as informações de conexão para o banco de dados.

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Em seguida, adicione uma fonte de dados para a página. Para usar uma quantidade limitada de dados, vamos selecionar apenas as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks. Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome da tabela (`Vendor`) com `Purchasing`. A marcação a seguir mostra a sintaxe correta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Lembre-se o nome (ID) da fonte de dados. Essa identificação muita deve ser usada no `DataSourceID` propriedade do controle Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Dentro do controle Accordion, você pode fornecer modelos para várias partes do controle, incluindo o cabeçalho (`<HeaderTemplate>`) e o conteúdo (`<ContentTemplate>`). Dentro desses elementos, apenas dados de saída os partir dos dados de origem, usando o `DataBinder.Eval()` método:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Quando a página é carregada, a fonte de dados deve ser vinculada a Acordeão com esse código do lado do servidor:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Para concluir este exemplo, você precisa definir as duas classes CSS que são referenciadas no controle Accordion (em suas propriedades `HeaderCssClass` e `ContentCssClass`). Coloque a seguinte marcação no `<head>` seção da página:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Os dados no acordeão vêm diretamente da fonte de dados](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Os dados no acordeão vêm diretamente da fonte de dados ([clique para exibir a imagem em tamanho normal](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](dynamically-adding-an-accordion-pane-cs.md)
