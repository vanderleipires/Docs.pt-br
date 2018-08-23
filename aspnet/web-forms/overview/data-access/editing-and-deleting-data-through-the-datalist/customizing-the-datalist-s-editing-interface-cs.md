---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Personalizar DataList de edição de Interface (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos uma interface mais rica de edição para DataList, inclui DropDownLists e uma caixa de seleção.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f7723895dd50b1923de49ca4a3a7055bbad5fe4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825084"
---
<a name="customizing-the-datalists-editing-interface-c"></a>Personalizando a Interface de edição do DataList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) ou [baixar PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> Neste tutorial, criaremos uma interface mais rica de edição para DataList, inclui DropDownLists e uma caixa de seleção.


## <a name="introduction"></a>Introdução

A marcação e controles da Web no DataList s `EditItemTemplate` definir sua interface editável. Em todos os exemplos de DataList editáveis podemos ve examinada até o momento, o editável interface tem sido composto de controles da Web de caixa de texto. No [tutorial anterior](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) melhoramos a experiência do usuário de tempo de edição adicionando controles de validação.

O `EditItemTemplate` pode ser expandido para incluir controles da Web que não seja a caixa de texto, como DropDownLists, RadioButtonLists, calendários e assim por diante. Assim como acontece com caixas de texto, ao personalizar a interface de edição para incluir outros controles da Web, emprega as seguintes etapas:

1. Adicionar o controle da Web para o `EditItemTemplate`.
2. Use a sintaxe de associação de dados para atribuir o valor do campo de dados correspondente para a propriedade apropriada.
3. No `UpdateCommand` controlar o valor de manipulador de eventos, acesso por meio de programação da Web e passá-la para o método apropriado de BLL.

Neste tutorial, criaremos uma interface mais rica de edição para DataList, inclui DropDownLists e uma caixa de seleção. Em particular, vamos criar uma DataList que lista informações sobre o produto e permite que o nome do produto s, fornecedor, categoria e descontinuados status ser atualizado (veja a Figura 1).


[![A Interface de edição inclui uma caixa de seleção de uma caixa de texto e dois DropDownLists](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: A Interface de edição inclui uma caixa de seleção de uma caixa de texto e dois DropDownLists ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Etapa 1: Exibir informações sobre o produto

Antes de criarmos a interface do DataList s editável, primeiro precisamos criar a interface somente leitura. Comece abrindo o `CustomizedUI.aspx` página do `EditDeleteDataList` pasta e, no Designer, adicione uma DataList para a página, definindo seu `ID` propriedade para `Products`. A partir do DataList s marca inteligente, crie um novo ObjectDataSource. Nomeie esse novo ObjectDataSource `ProductsDataSource` e configurá-lo para recuperar dados, o `ProductsBLL` classe s `GetProducts` método. Como com os tutoriais do DataList editáveis anteriores, vamos atualizar as informações do produto editado s indo diretamente para a camada de lógica de negócios. Da mesma forma, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Defina as listas suspensas UPDATE, INSERT e DELETE guias como (nenhum)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: defina a atualização, inserção e excluir guias menu suspenso lista como (nenhum) ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))


Depois de configurar o ObjectDataSource, o Visual Studio criará um padrão `ItemTemplate` de DataList que lista o nome e valor para cada um dos campos de dados retornado. Modificar a `ItemTemplate` para que o modelo de lista o nome do produto em um `<h4>` elemento juntamente com o nome da categoria, nome do fornecedor, preço e status descontinuados. Além disso, adicione um botão de edição, certificando-se de que seu `CommandName` estiver definida como edição. A marcação declarativa para meu `ItemTemplate` segue:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

A marcação acima apresenta as informações de produto usando um &lt;h4&gt; título para o nome do produto s e quatro colunas `<table>` para os campos restantes. O `ProductPropertyLabel` e `ProductPropertyValue` classes CSS, definidas em `Styles.css`, foram discutidos nos tutoriais anteriores. Figura 3 mostra nosso progresso quando visualizado por meio de um navegador.


[![O nome, fornecedor, categoria, Status descontinuado e preço de cada produto é exibido](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: O nome, fornecedor, categoria, Status descontinuado e preço de cada produto é exibida ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Etapa 2: Adicionando controles da Web para a Interface de edição

A primeira etapa na criação do DataList personalizado, interface de edição é adicionar os controles da Web necessários para o `EditItemTemplate`. Em particular, é necessário uma DropDownList para a categoria, outro para o fornecedor e uma caixa de seleção para o estado descontinuado. Como o preço do produto s não é editável neste exemplo, podemos continuar a exibi-la usando um controle da Web do rótulo.

Para personalizar a interface de edição, clique no link Editar modelos da marca inteligente DataList s e escolha o `EditItemTemplate` opção na lista suspensa. Adicionar uma DropDownList para a `EditItemTemplate` e defina sua `ID` para `Categories`.


[![Adicionar uma DropDownList para as categorias](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: adicionar uma DropDownList das categorias ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))


Em seguida, da DropDownList s marca inteligente, selecione a opção de escolher fonte de dados e criar um novo ObjectDataSource chamado `CategoriesDataSource`. Configurar este ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories()` método (consulte a Figura 5). Em seguida, o s DropDownList Data Source Configuration Wizard solicitará os campos de dados a ser usado para cada `ListItem` s `Text` e `Value` propriedades. Ter a exibição DropDownList a `CategoryName` campo de dados e use o `CategoryID` como o valor, conforme mostrado na Figura 6.


[![Criar um novo ObjectDataSource chamado CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: criar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))


[![Configurar a exibição de s DropDownList e campos de valor](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: Configure os campos de valor e a DropDownList s exibição ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))


Repita esta série de etapas para criar uma DropDownList para os fornecedores. Defina as `ID` para este DropDownList para `Suppliers` e nomeie seu ObjectDataSource `SuppliersDataSource`.

Depois de adicionar duas DropDownLists, adicione uma caixa de seleção para o estado descontinuado e uma caixa de texto para o nome do produto s. Defina as `ID` s para a caixa de seleção e a caixa de texto `Discontinued` e `ProductName`, respectivamente. Adicione um RequiredFieldValidator para garantir que o usuário fornece um valor para o nome do produto s.

Por fim, adicione os botões Cancelar e de atualização. Lembre-se de que esses dois botões é fundamental que suas `CommandName` propriedades são definidas para atualizar e Cancelar, respectivamente.

Fique à vontade para dispor a interface de edição desejado. Eu ve optou por usar os mesmos quatro colunas `<table>` ilustra o layout da interface somente leitura, como a seguinte sintaxe declarativa e a captura de tela:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![A Interface de edição é disposto horizontalmente, como a Interface somente leitura](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Figura 7**: A Interface de edição é disposto horizontalmente, como a Interface somente leitura ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Etapa 3: Criando o EditCommand e manipuladores de eventos CancelCommand

Atualmente, não há nenhuma sintaxe de associação de dados na `EditItemTemplate` (exceto para o `UnitPriceLabel`, que foi copiado literalmente do `ItemTemplate`). Vamos adicionar momentaneamente a sintaxe de associação de dados, mas primeiro deixar s criar os manipuladores de eventos para s DataList `EditCommand` e `CancelCommand` eventos. Lembre-se de que a responsabilidade do `EditCommand` manipulador de eventos é renderizar a interface de edição para o item de DataList cuja edição botão foi clicado, enquanto o `CancelCommand` s cabe a você retornar DataList ao seu estado de edição previamente.

Criar esses dois manipuladores de evento e usem o código a seguir:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Com esses dois manipuladores de eventos no local, clique no botão Editar exibe a interface de edição e clicando no botão Cancelar retorna o item editado para o modo somente leitura. Figura 8 mostra DataList após ser clicado no botão Editar para s chefe Anton mistura para Gumbo. Desde que criamos ve ainda para adicionar qualquer sintaxe de vinculação de dados para a interface de edição, o `ProductName` caixa de texto é em branco, o `Discontinued` caixa de seleção desmarcada e os primeiros itens selecionados do `Categories` e `Suppliers` DropDownLists.


[![Clicar o botão de edição exibe a Interface de edição](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Figura 8**: clique no botão Editar exibe a Interface de edição ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Etapa 4: Adicionando a sintaxe de associação de dados para a Interface de edição

Para que a interface de edição para exibir os valores atuais de s do produto, precisamos usar a sintaxe de vinculação de dados para atribuir os valores de campo de dados para os valores de controle da Web apropriados. A sintaxe de vinculação de dados pode ser aplicada por meio do Designer, vá para a tela Editar modelos e selecionar o link Editar DataBindings da Web controla as marcas inteligentes. Como alternativa, a sintaxe de vinculação de dados pode ser adicionada diretamente à marcação declarativa.

Atribuir a `ProductName` valor ao campo de dados a `ProductName` s da caixa de texto `Text` propriedade, o `CategoryID` e `SupplierID` valores de campo de dados a `Categories` e `Suppliers` DropDownLists `SelectedValue` propriedades e o `Discontinued` campo de dados de valor para o `Discontinued` caixa de seleção s `Checked` propriedade. Depois de fazer essas alterações, por meio do Designer ou diretamente por meio de marcação declarativa, revisita a página por meio de um navegador e clique no botão Editar para s chefe Anton mistura para Gumbo. Como mostra a Figura 9, a sintaxe de associação de dados tiver adicionado os valores atuais para a caixa de texto, DropDownLists e caixa de seleção.


[![Clicar o botão de edição exibe a Interface de edição](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Figura 9**: clique no botão Editar exibe a Interface de edição ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Etapa 5: Salvar as alterações do usuário s no manipulador de eventos UpdateCommand

Quando o usuário edita um produto e clica no botão de atualização, ocorre um postback e DataList s `UpdateCommand` evento é acionado. No evento manipulador, precisamos ler os valores dos controles da Web a `EditItemTemplate` e a interface com a BLL para atualizar o produto no banco de dados. Como podemos ve visto nos tutoriais anteriores, o `ProductID` do produto atualizado é acessível por meio de `DataKeys` coleção. Os campos inseridos pelo usuário são acessados referenciando programaticamente os controles da Web usando `FindControl("controlID")`, como mostra o código a seguir:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

O código começa consultando o `Page.IsValid` propriedade para garantir que todos os controles de validação na página são válidos. Se `Page.IsValid` está `True`, o produto editado s `ProductID` valor é lido do `DataKeys` coleta e a entrada de dados a controles da Web no `EditItemTemplate` são referenciados por meio de programação. Em seguida, os valores desses controles da Web são lidos em variáveis que são então passadas em apropriado `UpdateProduct` de sobrecarga. Depois de atualizar os dados, DataList é retornado para seu estado de edição previamente.

> [!NOTE]
> Eu ve omitido a lógica adicionada de manipulação de exceção de [tratamento BLL - e exceções de nível DAL](handling-bll-and-dal-level-exceptions-cs.md) tutorial para manter o código e este exemplo se concentra. Como um exercício, adicione essa funcionalidade depois de concluir este tutorial.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Etapa 6: CategoryID de tratamento de nulos e valores SupplierID

Permite que o banco de dados Northwind `NULL` os valores para o `Products` tabela s `CategoryID` e `SupplierID` colunas. No entanto, nossa edição t da interface atualmente acomodar `NULL` valores. Se tentar editar um produto que tenha um `NULL` valor para o seu `CategoryID` ou `SupplierID` colunas, falaremos sobre uma `ArgumentOutOfRangeException` com uma mensagem de erro semelhante à: *'Categorias' tem um SelectedValue que é inválido porque ele faz não existe na lista de itens.* Além disso, s não existe atualmente nenhuma maneira de alterar uma categoria de produto s ou o fornecedor de valor de um não -`NULL` de valor para um `NULL` um.

Para dar suporte à `NULL` valores para a categoria e fornecedor DropDownLists, precisamos adicionar um `ListItem`. Eu ve optado por usar (nenhum) como o `Text` valor para este `ListItem`, mas você pode alterá-lo para algo diferente (como uma cadeia de caracteres vazia) se você d gosta. Por fim, lembre-se de definir o DropDownLists `AppendDataBoundItems` à `True`; se você se esquecer de fazer isso, as categorias e fornecedores associados a DropDownList substituirá o-adicionados estaticamente `ListItem`.

Depois de fazer essas alterações, a marcação DropDownLists no DataList s `EditItemTemplate` deve ser semelhante ao seguinte:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Estático `ListItem` s podem ser adicionados a uma DropDownList por meio do Designer ou diretamente por meio da sintaxe declarativa. Ao adicionar um item de DropDownList para representar um banco de dados `NULL` de valor, certifique-se de adicionar o `ListItem` através de sintaxe declarativa. Se você usar o `ListItem` Editor de coleção no Designer, a sintaxe declarativa gerada omitirá os `Value` completamente configuração quando atribuído a uma cadeia de caracteres em branco, a criação de uma marcação declarativa como: `<asp:ListItem>(None)</asp:ListItem>`. Embora isso pareça inofensivo, ausentes `Value` faz com que a DropDownList usar o `Text` valor da propriedade em seu lugar. Isso significa que, se isso `NULL` `ListItem` é selecionada, o valor (nenhum) será tentado a ser atribuído ao campo de dados do produto (`CategoryID` ou `SupplierID`, neste tutorial), que resulta em uma exceção. Definindo explicitamente `Value=""`, um `NULL` valor será atribuído ao produto do campo de dados quando o `NULL` `ListItem` está selecionado.


Reserve um tempo para exibir nosso progresso através de um navegador. Observe que, ao editar um produto, o `Categories` e `Suppliers` DropDownLists os dois têm um (nenhum) opção no início da DropDownList.


[![As categorias e fornecedores DropDownLists incluem um (nenhum) opção](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Figura 10**: O `Categories` e `Suppliers` DropDownLists incluem um (nenhum) opção ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))


Salvar (nenhum) opção como um banco de dados `NULL` valor, é necessário retornar para o `UpdateCommand` manipulador de eventos. Alterar o `categoryIDValue` e `supplierIDValue` variáveis sejam inteiros que permitem valor nulos e atribuí-los um valor diferente de `Nothing` somente se o s DropDownList `SelectedValue` não é uma cadeia de caracteres vazia:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Com essa alteração, um valor de `Nothing` será passado para o `UpdateProduct` método BLL se o usuário selecionado (nenhum) opção de qualquer um das listas suspensas, que corresponde a um `NULL` valor do banco de dados.

## <a name="summary"></a>Resumo

Neste tutorial vimos como criar uma interface de edição de DataList mais complexa que incluiu três controles de Web de entrada diferentes, uma caixa de texto, duas DropDownLists e uma caixa de seleção junto com os controles de validação. Ao criar a interface de edição, as etapas são as mesmas, independentemente dos controles da Web que está sendo usados: começar pela adição de controles da Web à DataList s `EditItemTemplate`; use a sintaxe de associação de dados para atribuir os valores de campo de dados correspondentes com a Web apropriada Propriedades do controle; e, além de `UpdateCommand` manipulador de eventos, acessar programaticamente os controles da Web e suas propriedades apropriadas, passando os valores para a BLL.

Ao criar uma interface de edição, se ele s composto por apenas caixas de texto ou uma coleção de controles da Web diferentes, certifique-se de lidar corretamente com o banco de dados `NULL` valores. Quando contabilizar `NULL` s, é imperativo que você não está corretamente apenas exibir uma existente `NULL` valor na interface de edição, mas também que você oferecem um meio para marcar um valor como `NULL`. Para DropDownLists em DataLists, que normalmente significa adicionar um estático `ListItem` cujos `Value` estiver explicitamente definida como uma cadeia de caracteres vazia (`Value=""`) e adicionando um pouco de código para o `UpdateCommand` manipulador de eventos para determinar ou não o `NULL``ListItem` foi selecionado.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Dennis Patterson, David Suru e Randy Schmidt. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [Próximo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
