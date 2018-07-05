---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Uso de ModalPopup com um controle repetidor (c#) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. Também é possível usar esse contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e6136bac48b7c1b0931cada66501d303be8e927
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371071"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>Uso de ModalPopup com um controle repetidor (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. Também é possível usar esse controle em um repetidor.


## <a name="overview"></a>Visão geral

O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. Também é possível usar esse controle em um repetidor.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional da instalação do Visual Studio (inclusive a express edition) e também está disponível como um download separado em [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks é parte do SQL Server 2005 exemplos e bancos de dados de exemplo (download em [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexe o `AdventureWorks.mdf` arquivo de banco de dados. Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão. Se sua configuração for diferente, você precisa adaptar as informações de conexão para o banco de dados. Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Em seguida, adicione uma fonte de dados para a página. Para usar uma quantidade limitada de dados, vamos selecionar apenas as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks. Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome da tabela (`Vendor`) com `Purchasing`. A marcação a seguir mostra a sintaxe correta:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

Em seguida, adicione um painel que serve como o popup modal. Ele contém um `Button` controle para fechar o janela pop-up novamente:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Para fazer com que o pop-up funcionar dentro de repeater, o `ModalPopupExtender` controle deve ser colocada dentro a `<ItemTemplate>` seção do repetidor. Portanto, o painel está fora do repetidor, mas o extensor está dentro. Aqui está a marcação para o repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Em seguida, cada item na fonte de dados é exibido com um botão ao lado dele que dispara o popup modal.


[![O popup modal pode ser disparado para cada entrada da fonte de dados](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

O popup modal pode ser disparado para cada entrada da fonte de dados ([clique para exibir a imagem em tamanho normal](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](launching-a-modal-popup-window-from-server-code-cs.md)
> [Próximo](handling-postbacks-from-a-modalpopup-cs.md)
