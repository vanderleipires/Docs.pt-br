---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Uso de CascadingDropDown com um banco de dados (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 98e07764a3bd6afc8045221e9c016e57be44f5f7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832863"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Uso de CascadingDropDown com um banco de dados (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. Para isso funcionar, um serviço web especial deve ser criado.


## <a name="overview"></a>Visão geral

O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) Para isso funcionar, um serviço web especial deve ser criado.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional da instalação do Visual Studio (inclusive a express edition) e também está disponível como um download separado em [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks é parte do SQL Server 2005 exemplos e bancos de dados de exemplo (download em [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexe o `AdventureWorks.mdf` arquivo de banco de dados.

Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão. Se sua configuração for diferente, você precisa adaptar as informações de conexão para o banco de dados.

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de &lt; `form` &gt; elemento):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

Na próxima etapa, são necessários dois controles DropDownList. Neste exemplo, usamos as fornecedor e informações de contato da AdventureWorks, assim, podemos criar uma lista para os fornecedores disponíveis e outro para os contatos disponíveis:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Em seguida, dois extensores de CascadingDropDown devem ser adicionados à página. Um preenche a primeira lista (fornecedores) e o outro é preenche a segunda lista (contatos). Configure os seguintes atributos:

- `ServicePath`: A URL de um serviço web fornecendo as entradas da lista
- `ServiceMethod`: Método web fornecer as entradas da lista
- `TargetControlID`: A ID da lista suspensa
- `Category`: Informações de categoria que são enviadas para o método da web quando chamado
- `PromptText`: Texto exibido quando assincronamente Carregando dados da lista do servidor
- `ParentControlID`: (opcional) lista de lista suspensa de pai que o carregamento da lista atual de gatilhos

Dependendo da linguagem de programação usada, o nome do serviço web em questão é alterado, mas todos os outros valores de atributo são os mesmos. Aqui está o elemento CascadingDropDown para a primeira lista suspensa:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Os extensores de controle para a segunda lista precisam definir o `ParentControlID` de atributo para que a seleção de uma entrada nos gatilhos de lista de fornecedores ao carregar elementos na lista de contatos associados.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

O trabalho real, em seguida, é feito no serviço da web, que é definido da seguinte maneira. Observe que o `[ScriptService]` atributo é usado, caso contrário, o ASP.NET AJAX não é possível criar o proxy do JavaScript para acessar os métodos da web do código de script do lado do cliente.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

A assinatura dos métodos de web chamado pelo CascadingDropDown é da seguinte maneira:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Portanto, o valor de retorno deve ser uma matriz do tipo `CascadingDropDownNameValue` que é definida pelo Control Toolkit. O `GetVendors()` método é bastante fácil de implementar: O código se conecta ao banco de dados AdventureWorks e consulta os primeiros 25 fornecedores. O primeiro parâmetro na `CascadingDropDownNameValue` construtor é a legenda da entrada da lista, o segundo é seu valor (atributo de valor em do HTML &lt; `option` &gt; elemento). Aqui está o código:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Obtendo os contatos associados para um fornecedor (nome do método: `GetContactsForVendor()`) é um pouco mais complicado. Em primeiro lugar, o fornecedor que foi selecionado na lista suspensa primeiro deve ser determinado. O Kit de ferramentas de controle define um método auxiliar para a tarefa: O `ParseKnownCategoryValuesString()` método retorna um `StringDictionary` elemento com os dados de lista suspensa:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Por motivos de segurança, esses dados devem ser validados pela primeira vez. Portanto, se houver uma entrada de fornecedor (porque o `Category` do primeiro elemento CascadingDropDown estiver definida como `"Vendor"`), a ID do fornecedor selecionado poderão ser recuperada:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

O restante do método é bastante direto, em seguida. ID do fornecedor é usado como um parâmetro para uma consulta SQL que recupera todos os contatos associados para esse fornecedor. Mais uma vez, o método retorna uma matriz do tipo `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Carregar a página ASP.NET e pouco tempo depois, a lista de fornecedores é preenchida com 25 entradas. Selecione uma entrada e observe como a segunda lista suspensa é preenchida com dados.


[![A primeira lista é preenchida automaticamente](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

A primeira lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![A segunda lista é preenchida de acordo com a seleção na primeira lista](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

A segunda lista é preenchida de acordo com a seleção na primeira lista ([clique para exibir a imagem em tamanho normal](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](filling-a-list-using-cascadingdropdown-vb.md)
> [Próximo](presetting-list-entries-with-cascadingdropdown-vb.md)
