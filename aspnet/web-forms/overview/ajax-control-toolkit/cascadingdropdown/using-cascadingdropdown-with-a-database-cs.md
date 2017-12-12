---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Usando CascadingDropDown com um banco de dados (c#) | Microsoft Docs
author: wenz
description: "O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d6b62259573433cff7054d50cc299da9e4f372
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-c"></a>Usando CascadingDropDown com um banco de dados (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. Em ordem para este trabalho, um serviço web especial deve ser criado.


## <a name="overview"></a>Visão Geral

O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) Em ordem para este trabalho, um serviço web especial deve ser criado.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks faz parte dos bancos de dados de exemplo e exemplos do SQL Server 2005 (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e conecte o `AdventureWorks.mdf` arquivo de banco de dados.

Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão. Se sua configuração for diferente, você precisa se adaptar as informações de conexão para o banco de dados.

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do &lt; `form` &gt; elemento):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

Na próxima etapa, são necessários dois controles DropDownList. Neste exemplo, usamos as fornecedor e informações de contato da AdventureWorks, portanto, podemos criar uma lista de fornecedores disponíveis e uma para os contatos disponíveis:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Em seguida, dois extensores de CascadingDropDown devem ser adicionados à página. Um preenche a primeira lista (fornecedores) e o outro um preenche a segunda lista (contatos). Configure os seguintes atributos:

- `ServicePath`: A URL de um serviço web fornecendo as entradas da lista
- `ServiceMethod`: Método da web fornecendo as entradas da lista
- `TargetControlID`: A ID da lista suspensa
- `Category`: Informações de categoria que são enviadas ao método da web quando chamado
- `PromptText`: Texto exibido quando assincronamente Carregando dados de lista do servidor
- `ParentControlID`: (opcional) pai suspensa lista que o carregamento da lista atual de gatilhos

Dependendo da linguagem de programação usada, o nome do serviço da web em questão é alterado, mas todos os outros valores de atributo são os mesmos. Aqui está o elemento CascadingDropDown para a primeira lista suspensa:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

Os extensores de controle para a segunda lista precisam definir o `ParentControlID` atributo para que a seleção de uma entrada nos gatilhos de lista de fornecedores carregar associados elementos na lista de contatos.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

O trabalho real, em seguida, é feito no serviço da web, que é definido da seguinte maneira. Observe que o `[ScriptService]` atributo é usado, caso contrário, o ASP.NET AJAX não é possível criar o proxy JavaScript para acessar os métodos da web do código de script do lado do cliente.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

A assinatura dos métodos de web chamado pelo CascadingDropDown é da seguinte maneira:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Para que o valor de retorno deve ser uma matriz do tipo `CascadingDropDownNameValue` que é definido pelo Kit de ferramentas. O `GetVendors()` método é muito fácil de implementar: O código se conecta ao banco de dados AdventureWorks e consulta os 25 primeiros fornecedores. O primeiro parâmetro de `CascadingDropDownNameValue` construtor é a legenda da entrada de lista, o outro valor (atributo de valor em HTML &lt; `option` &gt; elemento). Aqui está o código:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Obtendo os contatos associados para um fornecedor (nome do método: `GetContactsForVendor()`) é um pouco mais complicado. Em primeiro lugar, o fornecedor que foi selecionado na lista suspensa primeiro deve ser determinado. O Kit de ferramentas define um método auxiliar para a tarefa: O `ParseKnownCategoryValuesString()` método retorna um `StringDictionary` elemento com os dados da lista suspensa:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Por motivos de segurança, esses dados devem ser validados primeiro. Portanto, se houver uma entrada de fornecedor (porque o `Category` propriedade do primeiro elemento CascadingDropDown está definida como `"Vendor"`), a ID do fornecedor selecionado pode ser recuperada:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

O restante do método é bastante direta, então. ID do fornecedor é usado como um parâmetro para uma consulta SQL que recupera todos os contatos associados para esse fornecedor. Uma vez, o método retorna uma matriz do tipo `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Carregar a página do ASP.NET, e depois de um curto tempo a lista de fornecedores é preenchida com 25 entradas. Selecione uma entrada e observe como a segunda lista suspensa é preenchida com dados.


[![A primeira lista é preenchida automaticamente](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

A primeira lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![Na segunda lista é preenchida de acordo com a seleção na primeira lista](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

Na segunda lista é preenchida de acordo com a seleção na primeira lista ([clique para exibir a imagem em tamanho normal](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

>[!div class="step-by-step"]
[Anterior](filling-a-list-using-cascadingdropdown-cs.md)
[Próximo](presetting-list-entries-with-cascadingdropdown-cs.md)
