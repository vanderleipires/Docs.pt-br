---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personalizando a Interface de modificação de dados (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como personalizar a interface de um GridView editável, substituindo a caixa de texto padrão e controles de caixa de seleção com alternati...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d2bf8e3b074e846f1071a94884faa40312e507b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823502"
---
<a name="customizing-the-data-modification-interface-vb"></a>Personalizando a Interface de modificação de dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) ou [baixar PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> Neste tutorial, examinaremos como personalizar a interface de um GridView editável, substituindo a caixa de texto padrão e controles de caixa de seleção com controles de Web de entrada alternativos.


## <a name="introduction"></a>Introdução

O BoundFields e CheckBoxFields usadas pelos controles GridView e DetailsView simplificar o processo de modificação de dados devido a sua capacidade de renderizar interfaces somente leitura, editáveis e inseríveis. Essas interfaces podem ser renderizadas sem a necessidade de adicionar qualquer marcação declarativa adicional ou código. No entanto, o BoundField e do CheckBoxField interfaces não têm a capacidade de personalização que muitas vezes é necessária em cenários do mundo real. Para personalizar a interface editável ou inserível em um GridView ou DetailsView precisamos em vez disso, use um TemplateField.

No [tutorial anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) , vimos como personalizar as interfaces de modificação de dados, adicionando controles de Web de validação. Neste tutorial, examinaremos como personalizar os controles de Web de coleção de dados reais, substituindo o BoundField e caixa de texto padrão da CheckBoxField e controles de caixa de seleção com controles de Web de entrada alternativos. Em particular, vamos criar um GridView editável que permite que um produto nome, categoria, fornecedor e descontinuados status ser atualizado. Ao editar uma linha específica, os campos de categoria e fornecedor serão renderizado como DropDownLists, que contém o conjunto de categorias disponíveis e fornecedores para sua escolha. Além disso, substituiremos padrão do CheckBoxField caixa de seleção com um controle RadioButtonList que oferece duas opções: "Ativo" e "Interrompido".


[![A Interface de edição do GridView inclui DropDownLists e botões de opção](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figura 1**: A GridView edição Interface inclui DropDownLists e botões de opção ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Etapa 1: Criando apropriado`UpdateProduct`de sobrecarga

Neste tutorial, criaremos um GridView editável que permite editar o nome de um produto, categoria, fornecedor e descontinuado status. Portanto, precisamos de um `UpdateProduct` sobrecarga que aceita cinco parâmetros de entrada esses valores de quatro produtos mais o `ProductID`. Como no nosso sobrecargas anteriores, este um será:

1. Recuperar as informações de produto do banco de dados especificado `ProductID`,
2. Atualizar o `ProductName`, `CategoryID`, `SupplierID`, e `Discontinued` campos, e
3. Enviar a solicitação de atualização para o DAL por meio do TableAdapter `Update()` método.

Para fins de brevidade, para essa sobrecarga específica omiti a verificação da regra de negócios que garante que um produto que está sendo marcado como Descontinuado não é o único produto oferecido pelo seu fornecedor. Fique à vontade para adicioná-lo no se preferir, ou, idealmente, refatorar a lógica para um método separado.

O código a seguir mostra o novo `UpdateProduct` sobrecarga no `ProductsBLL` classe:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Etapa 2: Criar o GridView editável

Com o `UpdateProduct` sobrecarga adicionada, estamos prontos para criar nosso GridView editável. Abra o `CustomizedUI.aspx` página no `EditInsertDelete` pasta e adicione um controle GridView para o Designer. Em seguida, crie um novo ObjectDataSource na marca inteligente do GridView. Configurar o ObjectDataSource para recuperar informações de produto por meio de `ProductBLL` da classe `GetProducts()` método e atualizar dados de produto usando o `UpdateProduct` sobrecarga que acabamos de criar. Nas guias INSERT e DELETE, selecione (nenhum) as listas de lista suspensa.


[![Configurar o ObjectDataSource para usar a sobrecarga de UpdateProduct acabou de criar](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figura 2**: configurar o ObjectDataSource para usar o `UpdateProduct` sobrecarregar acabou de criar ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image6.png))


Como vimos em todo os tutoriais de modificação de dados, a sintaxe declarativa para o ObjectDataSource criado pelo Visual Studio atribui a `OldValuesParameterFormatString` propriedade para `original_{0}`. Isso, obviamente, não funcionará com nossa camada de lógica de negócios, pois nossos métodos não espere que o original `ProductID` valor a ser passado. Portanto, assim como fizemos nos tutoriais anteriores, dedique uns momentos para remover a atribuição de propriedade da sintaxe declarativa ou, em vez disso, defina o valor da propriedade `{0}`.

Após essa alteração, a marcação declarativa do ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Observe que o `OldValuesParameterFormatString` propriedade foi removida e que não há uma `Parameter` na `UpdateParameters` coleção para cada um dos parâmetros de entrada esperados pelo nosso `UpdateProduct` de sobrecarga.

Enquanto o ObjectDataSource está configurado para atualizar apenas um subconjunto de valores de produto, no momento, mostra uma GridView *todos os* dos campos do produto. Reserve um tempo para editar o GridView, de modo que:

- Ela inclui apenas o `ProductName`, `SupplierName`, `CategoryName` BoundFields e o `Discontinued` CheckBoxField
- O `CategoryName` e `SupplierName` campos apareçam antes (à esquerda do) o `Discontinued` CheckBoxField
- O `CategoryName` e `SupplierName` BoundFields' `HeaderText` estiver definida como "Category" e "Fornecedor", respectivamente
- Suporte de edição está habilitado (Verifique a caixa de seleção Habilitar edição na marca inteligente do GridView)

Após essas alterações, o Designer será semelhante à Figura 3, com a sintaxe declarativa do GridView, mostrado abaixo.


[![Remover campos desnecessários de GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figura 3**: remover os campos desnecessários do GridView ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

Neste ponto, o comportamento de somente leitura do GridView foi concluído. Ao exibir os dados, cada produto é renderizado como uma linha em GridView, mostrando o nome do produto, categoria, fornecedor e descontinuado status.


[![Interface de somente leitura do GridView está concluída](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figura 4**: Interface do GridView a de somente leitura é concluída ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Conforme discutido em [tutorial de uma visão geral de inserção de, atualizando e excluindo dados](an-overview-of-inserting-updating-and-deleting-data-cs.md), é extremamente importante que o GridView, o estado de exibição s ser habilitada (o comportamento padrão). Se você definir o s GridView `EnableViewState` propriedade para `false`, você corre o risco de ter acidentalmente excluir ou editar registros de usuários simultâneos. Ver [Aviso: problema de simultaneidade com o ASP.NET 2.0 GridViews/DetailsView/FormViews esse suporte de edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obter mais informações.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Etapa 3: Usando DropDownList para a categoria e fornecedor Interfaces de edição

Lembre-se de que o `ProductsRow` objeto contém `CategoryID`, `CategoryName`, `SupplierID`, e `SupplierName` propriedades, que fornecem os valores reais de ID de chave estrangeira no `Products` banco de dados de tabela e correspondente `Name` os valores na `Categories` e `Suppliers` tabelas. O `ProductRow`do `CategoryID` e `SupplierID` podem tanto ser lido e gravados, enquanto o `CategoryName` e `SupplierName` propriedades são somente leitura.

Devido ao status de somente leitura a `CategoryName` e `SupplierName` propriedades, os BoundFields correspondentes tenha tido suas `ReadOnly` propriedade definida como `True`, impedindo esses valores de ser modificado quando uma linha é editada. Enquanto podemos definir o `ReadOnly` propriedade para `False`, renderização de `CategoryName` e `SupplierName` BoundFields como caixas de texto durante a edição, essa abordagem resultará em uma exceção quando o usuário tenta atualizar o produto, pois não há nenhum `UpdateProduct` sobrecarga que recebe `CategoryName` e `SupplierName` entradas. Na verdade, não queremos criar tal uma sobrecarga por dois motivos:

- O `Products` a tabela não tem `SupplierName` ou `CategoryName` campos, mas `SupplierID` e `CategoryID`. Portanto, queremos que nosso método a ser passado a esses valores de ID específicos, não os valores de suas tabelas de pesquisa.
- Exigir que o usuário digitar o nome de fornecedor ou categoria é inferior ao ideal, pois exige que o usuário precisa saber as categorias disponíveis e fornecedores e seus erros de ortografia corretos.

Os campos de categoria e fornecedor devem exibir a categoria e nomes de fornecedores no modo somente leitura (como ele faz agora) e uma lista suspensa de opções aplicáveis ao que está sendo editado. Usando uma lista suspensa, o usuário final pode ver rapidamente quais categorias e os fornecedores estão disponíveis para escolher entre e muito mais fazer sua seleção com facilidade.

Para fornecer esse comportamento, é necessário converter o `SupplierName` e `CategoryName` BoundFields em TemplateFields cujo `ItemTemplate` emite o `SupplierName` e `CategoryName` valores e cujo `EditItemTemplate` usa um controle DropDownList à lista de categorias disponíveis e fornecedores.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Adicionando o`Categories`e`Suppliers`DropDownLists

Iniciar, convertendo as `SupplierName` e `CategoryName` BoundFields em TemplateFields por: clicando no link Edit Columns na marca inteligente do GridView; selecionando o BoundField na lista na parte inferior esquerda; e clicando no "converter este campo em um Link TemplateField". O processo de conversão criará um TemplateField tanto com um `ItemTemplate` e um `EditItemTemplate`, conforme mostrado na sintaxe declarativa abaixo:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Desde que o BoundField foram marcado como somente leitura, tanto a `ItemTemplate` e `EditItemTemplate` contêm um rótulo Web controle cuja `Text` propriedade está associada ao campo de dados aplicável (`CategoryName`, na sintaxe acima). Precisamos modificar o `EditItemTemplate`, substituindo o controle da Web de rótulo com um controle DropDownList.

Como vimos nos tutoriais anteriores, o modelo pode ser editado por meio do Designer ou diretamente da sintaxe declarativa. Para editá-lo por meio do Designer, clique no link Editar modelos na marca inteligente do GridView e optar por trabalhar com o campo de categoria `EditItemTemplate`. Remover o controle de Web Label e substituí-lo com um controle DropDownList, definindo a propriedade de ID do DropDownList como `Categories`.


[![Remova a caixa de texto e adicione uma DropDownList ao EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figura 5**: Remova a caixa de texto e adicione uma DropDownList para a `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image15.png))


Em seguida, precisamos preencher DropDownList com as categorias disponíveis. Clique no link Escolher fonte de dados na marca inteligente do DropDownList e optar por criar um novo ObjectDataSource chamado `CategoriesDataSource`.


[![Criar um novo controle ObjectDataSource chamado CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figura 6**: criar um controle ObjectDataSource novo chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image18.png))


Para que este ObjectDataSource retornar todas as categorias, associá-lo para o `CategoriesBLL` da classe `GetCategories()` método.


[![Associar o ObjectDataSource GetCategories() método do CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figura 7**: associar o ObjectDataSource para a `CategoriesBLL`do `GetCategories()` método ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image21.png))


Por fim, configurar configurações do DropDownList, de modo que o `CategoryName` campo é exibido em cada DropDownList `ListItem` com o `CategoryID` usado como o valor do campo.


[![Ter o campo CategoryName exibido e o CategoryID usado como o valor](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figura 8**: tem o `CategoryName` campo exibido e o `CategoryID` usada como o valor ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image24.png))


Depois de fazer essas alterações a marcação declarativa para a `EditItemTemplate` no `CategoryName` TemplateField incluirá uma DropDownList e um ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList no `EditItemTemplate` deve ter seu estado de exibição habilitado. Em breve, adicionaremos a sintaxe de associação de dados para a sintaxe declarativa do DropDownList e comandos de vinculação de dados como `Eval()` e `Bind()` só pode aparecer em controles cujo estado de exibição está habilitado.


Repita essas etapas para adicionar uma chamada de DropDownList `Suppliers` para o `SupplierName` do TemplateField `EditItemTemplate`. Isso envolverá uma DropDownList para a adição de `EditItemTemplate` e criar outro ObjectDataSource. O `Suppliers` ObjectDataSource do DropDownList, no entanto, deve ser configurado para invocar o `SuppliersBLL` da classe `GetSuppliers()` método. Além disso, configure a `Suppliers` DropDownList para exibir o `CompanyName` campo e usar o `SupplierID` campo como o valor para seus `ListItem` s.

Depois de adicionar o DropDownLists para os dois `EditItemTemplate` s, carregue a página em um navegador e clique no botão Editar para o produto de Seasoning Cajun do chefe Anton. Como mostra a Figura 9, colunas de categoria e fornecedor do produto são renderizadas como listas suspensas que contém as categorias disponíveis e os fornecedores à sua escolha. No entanto, observe que o *primeiro* itens em ambas as listas suspensas são selecionadas por padrão (para a categoria de bebidas) e líquidos exóticos como o fornecedor, mesmo que Seasoning do chefe Anton Cajun é um Condiment fornecido pela nova Orleans Cajun Prazeres.


[![O primeiro Item no menu suspenso lista é selecionado por padrão](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figura 9**: O primeiro Item no menu suspenso lista é selecionado por padrão ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image27.png))


Além disso, se você clicar em atualizar, você descobrirá que o produto `CategoryID` e `SupplierID` valores são definidos como `NULL`. Desses dois maneira indesejada comportamentos causados porque o DropDownLists no `EditItemTemplate` s não são associados a campos de dados dos dados subjacentes do produto.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Associando a DropDownLists para o`CategoryID`e`SupplierID`campos de dados

Para ter a categoria e o fornecedor do produto editado nas listas suspensas definir os valores apropriados e para ter estes valores enviadas de volta para a BLL `UpdateProduct` método depois de clicar em atualizar, precisaremos ligar 'DropDownLists `SelectedValue` propriedades para o `CategoryID` e `SupplierID` campos de dados usando a associação de dados bidirecional. Para fazer isso com o `Categories` DropDownList, você pode adicionar `SelectedValue='<%# Bind("CategoryID") %>'` diretamente para a sintaxe declarativa.

Como alternativa, você pode definir databindings do DropDownList editando o modelo por meio do Designer e clicar no link Editar DataBindings de marca inteligente da DropDownList. Em seguida, indicar que o `SelectedValue` propriedade deve ser vinculada ao `CategoryID` campo usando vinculação de dados bidirecional (veja a Figura 10). Repita o processo declarativo ou Designer para associar o `SupplierID` campo de dados para o `Suppliers` DropDownList.


[![Associar o CategoryID a propriedade SelectedValue do DropDownList usando vinculação de dados bidirecional](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figura 10**: associar o `CategoryID` para a DropDownList `SelectedValue` propriedade usando bidirecional Databinding ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image30.png))


Depois que as associações foram aplicadas ao `SelectedValue` propriedades de dois DropDownLists, colunas de categoria e fornecedor do produto editado usará como padrão os valores do produto atual. Ao clicar em atualizar, o `CategoryID` e `SupplierID` valores do item selecionado na lista suspensa serão passados para o `UpdateProduct` método. A Figura 11 mostra o tutorial depois que as instruções de associação de dados forem adicionadas; Observe como os itens selecionados na lista suspensa para Seasoning do chefe Anton Cajun estão corretamente Condiment e Nova Orleans Cajun prazeres.


[![Categoria atual e os valores do fornecedor do produto editado são selecionadas por padrão](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figura 11**: O editado do produto atual de categoria e fornecedor valores são selecionados por padrão ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Tratamento`NULL`valores

O `CategoryID` e `SupplierID` colunas na `Products` tabela pode ser `NULL`, ainda o DropDownLists no `EditItemTemplate` s não incluem um item de lista para representar um `NULL` valor. Isso tem duas consequências:

- Usuário não pode usar nossa interface para alterar o fornecedor ou categoria de um produto de um não -`NULL` de valor para um `NULL` um
- Se um produto possui um `NULL` `CategoryID` ou `SupplierID`, clique no botão Editar resultará em uma exceção. Isso ocorre porque o `NULL` valor retornado por `CategoryID` (ou `SupplierID`) na `Bind()` instrução não é mapeado para um valor na DropDownList (DropDownList gera uma exceção quando seu `SelectedValue` estiver definida como um valor *não* em sua coleção de itens de lista).

Para dar suporte à `NULL` `CategoryID` e `SupplierID` valores, precisamos adicionar outro `ListItem` para cada DropDownList para representar o `NULL` valor. No [filtragem de mestre/detalhes com uma DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, vimos como adicionar um `ListItem` para uma associação de dados DropDownList, qual envolvida Configurando a DropDownList `AppendDataBoundItems` propriedade `True` e adicionar manualmente o adicionais `ListItem`. Nesse tutorial anterior, no entanto, adicionamos uma `ListItem` com um `Value` de `-1`. A lógica de associação de dados no ASP.NET, no entanto, serão automaticamente convertidos em uma cadeia de caracteres em branco para um `NULL` valor e vice-versa um. Portanto, para este tutorial queremos que o `ListItem`do `Value` seja uma cadeia de caracteres vazia.

Comece definindo 'dois DropDownLists `AppendDataBoundItems` propriedade para `True`. Em seguida, adicione a `NULL` `ListItem` adicionando a seguinte `<asp:ListItem>` elemento para cada DropDownList para que a marcação declarativa parece como:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Eu optei por usar "(None)" como o valor de texto para este `ListItem`, mas você pode alterá-lo para também ser uma cadeia de caracteres em branco se você desejar.

> [!NOTE]
> Como vimos na *filtragem de mestre/detalhes com uma DropDownList* tutorial, `ListItem` s podem ser adicionados a uma DropDownList por meio do Designer clicando na DropDownList `Items` propriedade na janela Propriedades (que exibirá o `ListItem` Editor de coleção). No entanto, certifique-se de adicionar o `NULL` `ListItem` para este tutorial através de sintaxe declarativa. Se você usar o `ListItem` Editor de coleção, a sintaxe declarativa gerada omitirá a `Value` completamente configuração quando atribuído a uma cadeia de caracteres em branco, a criação de uma marcação declarativa como: `<asp:ListItem>(None)</asp:ListItem>`. Embora isso possa parecer inofensivo, o valor ausente faz com que a DropDownList usar o `Text` valor da propriedade em seu lugar. Isso significa que, se isso `NULL` `ListItem` é selecionada, o valor "(None)" será tentado a ser atribuído ao `CategoryID`, que resultará em uma exceção. Definindo explicitamente `Value=""`, um `NULL` valor será atribuído à `CategoryID` quando o `NULL` `ListItem` está selecionado.


Repita essas etapas para fornecedores DropDownList.

Com isso adicionais `ListItem`, a interface de edição agora pode atribuir `NULL` valores a um produto `CategoryID` e `SupplierID` campos, conforme mostrado na Figura 12.


[![Escolha (nenhum) para atribuir um valor nulo para o fornecedor ou categoria do produto](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figura 12**: escolha (nenhum) para atribuir uma `NULL` valor para a categoria de um produto ou o fornecedor ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Etapa 4: Usando botões de opção para o Status Descontinuado

Atualmente, os produtos `Discontinued` campo de dados é expresso usando um CheckBoxField, que renderiza uma caixa de seleção desabilitada para as linhas de somente leitura e uma caixa de seleção habilitada para a linha que está sendo editada. Embora essa interface do usuário geralmente é adequado, podemos pode personalizá-lo se necessário usando um TemplateField. Para este tutorial, vamos alterar o CheckBoxField em um TemplateField que usa um controle RadioButtonList com duas opções de "Ativos" e "Descontinuado" da qual o usuário pode especificar o produto `Discontinued` valor.

Iniciar, convertendo as `Discontinued` CheckBoxField em um TemplateField, que criará um TemplateField com um `ItemTemplate` e `EditItemTemplate`. Ambos os modelos incluem uma caixa de seleção com seu `Checked` propriedade vinculada ao `Discontinued` do campo de dados, a única diferença entre os dois é que o `ItemTemplate`da caixa de seleção `Enabled` estiver definida como `False`.

Substitua a caixa de seleção em ambos os `ItemTemplate` e `EditItemTemplate` com um controle RadioButtonList, definir ambos os RadioButtonLists' `ID` propriedades a serem `DiscontinuedChoice`. Em seguida, indicam que o RadioButtonLists, cada um, deve conter dois botões de opção, uma rotulada "ativo" com um valor de "False" e uma guia rotulada "Descontinuado" com um valor "True". Para fazer isso, você pode inserir o `<asp:ListItem>` elementos no diretamente por meio de sintaxe declarativa ou use o `ListItem` Editor de coleção do Designer. A Figura 13 mostra o `ListItem` Editor de coleção depois que as duas opções do botão de opção foram especificados.


[![Adicionar opções descontinuadas e Active Directory ao RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figura 13**: Adicionar ativo e opções descontinuadas RadioButtonList ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image39.png))


Desde o RadioButtonList no `ItemTemplate` não deve ser editável, definir seu `Enabled` propriedade a ser `False`, deixando o `Enabled` propriedade a ser `True` (o padrão) para RadioButtonList no `EditItemTemplate`. Isso tornará os botões de opção na linha não editado como somente leitura, mas permitirá que o usuário altere os valores de botão de opção da linha editada.

Ainda assim será preciso atribuir controle RadioButtonList `SelectedValue` propriedades para que o botão de opção apropriado é selecionado com base em que o produto `Discontinued` campo de dados. Assim como acontece com o DropDownLists examinado neste tutorial, essa sintaxe de associação de dados pode ser adicionado diretamente na marcação declarativa ou por meio do link Editar DataBindings em marcas de inteligentes dos RadioButtonLists.

Depois de adicionar os dois RadioButtonLists e configurá-los, o `Discontinued` marcação declarativa do TemplateField deve parecer com:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Com essas alterações, o `Discontinued` coluna foi transformada em uma lista de caixas de seleção em uma lista de pares de botão de rádio (veja a Figura 14). Ao editar um produto, o botão de opção apropriado é selecionado e o status do produto descontinuado podem ser atualizado, selecionando o outro botão de rádio e clicando em Atualizar.


[![As caixas de seleção descontinuadas foram substituídas por pares de botão de rádio](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figura 14**: O Descontinuado caixas de seleção foram substituídos por pares de botão de rádio ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Desde que o `Discontinued` coluna o `Products` banco de dados não pode ter `NULL` valores, não precisamos se preocupar sobre como capturar `NULL` informações na interface do. Se, no entanto, `Discontinued` coluna poderia conter `NULL` valores que queremos adicionar um terceiro botão à lista com seus `Value` definido como uma cadeia de caracteres vazia (`Value=""`), assim como com a categoria e fornecedor DropDownLists.


## <a name="summary"></a>Resumo

Enquanto o BoundField e CheckBoxField renderizam automaticamente interfaces somente leitura, edição e inserção, eles não têm a capacidade de personalização. Muitas vezes, no entanto, precisaremos personalizar a edição ou a inserção de interface, talvez adicionando controles de validação (como vimos no tutorial anterior) ou personalizando a interface de usuário de coleta de dados (como vimos neste tutorial). Personalizando a interface com um TemplateField pode ser resumido nas etapas a seguir:

1. Adicionar um TemplateField ou converter um BoundField existente ou CheckBoxField em um TemplateField
2. Aumentar a interface conforme necessário
3. Associar os campos de dados apropriados para os controles da Web recém-adicionada usando vinculação de dados bidirecional

Além de usar os controles da Web do ASP.NET internos, você também pode personalizar os modelos de um TemplateField com controles de servidor personalizado, compilado e controles de usuário.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Próximo](implementing-optimistic-concurrency-vb.md)
