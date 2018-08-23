---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Uso de HoverMenu com um controle repetidor (VB) | Microsoft Docs
author: wenz
description: 'O controle de HoverMenu no AJAX Control Toolkit fornece um efeito de pop-up simples: quando o ponteiro do mouse passa sobre um elemento, um pop-up é exibido em pela especificação...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ef2481b93a8bbe16b79edb8c93c02e24fc9890f3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832464"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Uso de HoverMenu com um controle repetidor (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> O controle de HoverMenu no AJAX Control Toolkit fornece um efeito de pop-up simples: quando o ponteiro do mouse passa sobre um elemento, um pop-up aparece em uma posição especificada. Também é possível usar esse controle em um repetidor.


## <a name="overview"></a>Visão geral

O `HoverMenu` controle no AJAX Control Toolkit fornece um efeito de pop-up simples: quando o ponteiro do mouse passa sobre um elemento, um pop-up aparece em uma posição especificada. Também é possível usar esse controle em um repetidor.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional da instalação do Visual Studio (inclusive a express edition) e também está disponível como um download separado em [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks é parte do SQL Server 2005 exemplos e bancos de dados de exemplo (download em [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexe o `AdventureWorks.mdf` arquivo de banco de dados.

Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão. Se sua configuração for diferente, você precisa adaptar as informações de conexão para o banco de dados.

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Em seguida, adicione uma fonte de dados para a página. Para usar uma quantidade limitada de dados, vamos selecionar apenas as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks. Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome da tabela (`Vendor`) com `Purchasing`. A marcação a seguir mostra a sintaxe correta:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Em seguida, adicione um painel que serve como o popup modal:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Agora, o `HoverMenuExtender` entra em cena. Para que todos os elementos na fonte de dados obtém seu próprio pop-up, o extensor deve ser colocado dentro do repetidor `<ItemTemplate>` seção. Aqui está a marcação:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Agora cada item na fonte de dados exibe um pop-up para a direita (`PopupPosition` atributo) após um atraso de 50 milissegundos (`PopDelay` atributo).


[![O menu de foco é exibido ao lado de cada item no repetidor](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

O menu de foco é exibido ao lado de cada item no repetidor ([clique para exibir a imagem em tamanho normal](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-hovermenu-with-a-repeater-control-cs.md)
