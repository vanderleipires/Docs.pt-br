---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personalizando a Interface de modificação de dados (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como personalizar a interface de um GridView editável, substituindo o padrão de texto e controles de caixa de seleção com alternati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d334a86fcf2fbd1069628527c6e89f3ab655dd5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888624"
---
<a name="customizing-the-data-modification-interface-vb"></a>Personalizando a Interface de modificação de dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) ou [baixar PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> Neste tutorial, examinaremos como personalizar a interface de um GridView editável, substituindo o padrão de texto e controles de caixa de seleção com controles de Web de entrada alternativos.


## <a name="introduction"></a>Introdução

O BoundFields e CheckBoxFields usadas pelos controles GridView e DetailsView simplificam o processo de modificação de dados devido a sua capacidade de processar somente leitura, editáveis e inseríveis interfaces. Essas interfaces podem ser renderizadas sem a necessidade de adicionar qualquer marcação declarativa adicional ou código. No entanto, interfaces de BoundField e do CheckBoxField não têm a capacidade de personalização que costuma ser necessária em cenários do mundo real. Para personalizar a interface editável ou inserível em GridView ou DetailsView precisamos em vez disso, use um TemplateField.

No [tutorial anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) vimos como personalizar as interfaces de modificação de dados adicionando controles da Web de validação. Neste tutorial, examinaremos como personalizar os controles de caixa de seleção com controles de Web de entrada alternativos e controles da Web de coleção de dados reais, substituindo o BoundField e caixa de texto padrão da CheckBoxField. Em particular, criaremos um GridView editável que permite que um produto nome, categoria, fornecedor e descontinuados status seja atualizado. Ao editar uma linha específica, os campos de categoria e fornecedor serão processada como DropDownLists, que contém o conjunto de categorias disponíveis e fornecedores à sua escolha. Além disso, substituiremos padrão do CheckBoxField caixa de seleção com um controle RadioButtonList que oferece duas opções: "Ativo" e "Descontinuado".


[![Interface de edição do GridView inclui DropDownLists e botões de opção](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figura 1**: A GridView edição Interface inclui DropDownLists e botões de opção ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Etapa 1: Criando apropriada`UpdateProduct`de sobrecarga

Neste tutorial, criaremos um GridView editável que permite a edição do nome do produto, categoria, fornecedor e descontinuado status. Portanto, é necessário um `UpdateProduct` sobrecarga que aceita cinco parâmetros de entrada esses valores de quatro produto mais o `ProductID`. Como em nosso sobrecargas anteriores, esta uma será:

1. Recuperar as informações de produto do banco de dados especificado `ProductID`,
2. Atualização de `ProductName`, `CategoryID`, `SupplierID`, e `Discontinued` campos, e
3. Enviar a solicitação de atualização para a DAL por meio do TableAdapter `Update()` método.

Para resumir, para essa sobrecarga específica omiti a verificação de regra de negócios que garante que um produto que está sendo marcado como descontinuada não é o único produto oferecido por seu fornecedor. Fique à vontade para adicioná-lo no se preferir, ou, idealmente, refatorar a lógica para um método separado.

O código a seguir mostra o novo `UpdateProduct` de sobrecarga no `ProductsBLL` classe:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Etapa 2: Criar o GridView editável

Com o `UpdateProduct` sobrecarga adicionada, estamos prontos para criar nossa GridView editável. Abra o `CustomizedUI.aspx` página o `EditInsertDelete` pasta e adicionar um controle GridView para o Designer. Em seguida, crie um novo ObjectDataSource de marca inteligente do GridView. Configurar o ObjectDataSource para recuperar informações de produto por meio de `ProductBLL` da classe `GetProducts()` método e atualizar dados de produto usando o `UpdateProduct` sobrecarga que acabamos de criar. Nas guias INSERT e DELETE, selecione (nenhum) nas listas suspensas.


[![Configurar o ObjectDataSource para usar a sobrecarga de UpdateProduct recém-criado](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figura 2**: configurar o ObjectDataSource para usar o `UpdateProduct` sobrecarregar acabou de criar ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image6.png))


Como vimos em todo os tutoriais de modificação de dados, a sintaxe declarativa para ObjectDataSource criado pelo Visual Studio atribui o `OldValuesParameterFormatString` propriedade `original_{0}`. Isso, obviamente, não funcionará com nossa camada de lógica comercial desde que nossos métodos não espera que o original `ProductID` valor a ser passado. Portanto, como fizemos nos tutoriais anteriores, reserve um tempo para remover a atribuição de propriedade da sintaxe declarativa ou, em vez disso, defina o valor desta propriedade `{0}`.

Após essa alteração, declarativo do ObjectDataSource deve parecer com o seguinte:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Observe que o `OldValuesParameterFormatString` propriedade foi removida e que haja um `Parameter` no `UpdateParameters` coleção para cada um dos parâmetros de entrada esperados pelo nosso `UpdateProduct` sobrecarga.

Enquanto o ObjectDataSource está configurado para atualizar somente um subconjunto de valores de produto, GridView mostra *todos os* dos campos de produto. Reserve um tempo para editar o GridView para que:

- Incluir apenas o `ProductName`, `SupplierName`, `CategoryName` BoundFields e `Discontinued` CheckBoxField
- O `CategoryName` e `SupplierName` campos para aparecer antes (à esquerda do) a `Discontinued` CheckBoxField
- O `CategoryName` e `SupplierName` 'BoundFields `HeaderText` propriedade for definida como "Categoria" e "Fornecedor", respectivamente
- Suporte de edição é ativado (marque a caixa de seleção Habilitar edição na marca inteligente do GridView)

Após essas alterações, o Designer será semelhante à Figura 3, com sintaxe declarativa do GridView, mostrada abaixo.


[![Remover os campos desnecessários do GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figura 3**: remover os campos desnecessários do GridView ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

Neste ponto, o comportamento de somente leitura do GridView foi concluído. Ao exibir os dados, cada produto é renderizado como uma linha em GridView, mostrando o nome do produto, categoria, fornecedor e descontinuado status.


[![Interface de somente leitura do GridView está concluída](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figura 4**: A GridView Interface de somente leitura é concluída ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Conforme discutido em [tutorial uma visão geral de inserção de, atualizando e excluindo dados](an-overview-of-inserting-updating-and-deleting-data-cs.md), é extremamente importante que o GridView s estado de exibição ser habilitado (o comportamento padrão). Se você definir o GridView s `EnableViewState` propriedade `false`, você corre o risco de ter acidentalmente excluir ou editar registros de usuários simultâneos. Consulte [Aviso: problema de simultaneidade com ASP.NET 2.0 GridViews/DetailsView/FormViews que suporte a edição e/ou excluindo e cujo estado de exibição é desativado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obter mais informações.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Etapa 3: Usando DropDownList para a categoria e a edição de Interfaces de fornecedor

Lembre-se de que o `ProductsRow` objeto contém `CategoryID`, `CategoryName`, `SupplierID`, e `SupplierName` propriedades, que fornecem os valores reais de ID de chave estrangeira no `Products` banco de dados de tabela e correspondente `Name` valores de `Categories` e `Suppliers` tabelas. O `ProductRow`do `CategoryID` e `SupplierID` pode ser de leitura de e gravados, enquanto o `CategoryName` e `SupplierName` propriedades são somente leitura.

Devido ao status de somente leitura a `CategoryName` e `SupplierName` propriedades, os BoundFields correspondentes tiveram seus `ReadOnly` propriedade definida como `True`, impedindo que esses valores que está sendo modificado quando uma linha é editada. Enquanto podemos definir o `ReadOnly` propriedade `False`, renderização de `CategoryName` e `SupplierName` BoundFields como caixas de texto durante a edição, essa abordagem resultará em uma exceção quando o usuário tenta atualizar o produto porque não há nenhum `UpdateProduct` sobrecarga que recebe `CategoryName` e `SupplierName` entradas. Na verdade, não queremos criar tal uma sobrecarga por dois motivos:

- O `Products` a tabela não tem `SupplierName` ou `CategoryName` campos, mas `SupplierID` e `CategoryID`. Portanto, queremos que nosso método a ser passado esses valores de ID específicas, não valores de suas tabelas de pesquisa.
- Exigir que o usuário digite o nome do fornecedor ou categoria é inferior ao ideal, pois ela requer que o usuário precisa saber as categorias disponíveis e fornecedores e seus grafias corretas.

Os campos de categoria e fornecedor devem exibir nomes de fornecedores quando no modo somente leitura e a categoria (como acontece agora) e uma lista suspensa de opções aplicáveis ao que está sendo editado. Usando uma lista suspensa, o usuário final pode ver rapidamente quais categorias e fornecedores estão disponíveis para escolher entre e podem mais facilmente fazer sua seleção.

Para fornecer esse comportamento, precisamos converter o `SupplierName` e `CategoryName` BoundFields em TemplateFields cujo `ItemTemplate` emite o `SupplierName` e `CategoryName` valores e cujo `EditItemTemplate` usa um controle DropDownList à lista de categorias disponíveis e fornecedores.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Adicionando o`Categories`e`Suppliers`DropDownLists

Iniciar convertendo o `SupplierName` e `CategoryName` BoundFields em TemplateFields por: clicando no link Editar colunas de marcas inteligentes do GridView; selecionando o BoundField da lista na parte inferior esquerda; e clicando na "converter este campo em um Link TemplateField". O processo de conversão criará um TemplateField com ambos um `ItemTemplate` e um `EditItemTemplate`, conforme mostrado na sintaxe declarativa abaixo:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Desde que o BoundField foram marcado como somente leitura, tanto o `ItemTemplate` e `EditItemTemplate` contém um rótulo Web controle cuja `Text` propriedade está associada ao campo de dados aplicável (`CategoryName`, na sintaxe acima). É preciso modificar o `EditItemTemplate`, substituindo o controle da Web de rótulo com um controle DropDownList.

Como vimos nos tutoriais anteriores, o modelo pode ser editado por meio do Designer ou diretamente da sintaxe declarativa. Para editá-lo por meio do Designer, clique no link Editar modelos de marca inteligente do GridView e trabalhar com o campo de categoria `EditItemTemplate`. Remover o controle de rótulo Web e substitua-o por um controle DropDownList, definindo a propriedade ID do DropDownList `Categories`.


[![Remova o texto e adicione DropDownList para o EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figura 5**: Remova o texto e adicione DropDownList para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image15.png))


Em seguida, é preciso preencher DropDownList com as categorias disponíveis. Clique no link Escolher fonte de dados de marca inteligente do DropDownList e optar por criar um novo ObjectDataSource denominado `CategoriesDataSource`.


[![Criar um novo controle ObjectDataSource denominado CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figura 6**: criar um controle ObjectDataSource novo chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image18.png))


Para que este ObjectDataSource retornar todas as categorias, associá-lo para o `CategoriesBLL` da classe `GetCategories()` método.


[![Associar o ObjectDataSource ao método de GetCategories() do CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figura 7**: associar o ObjectDataSource para o `CategoriesBLL`do `GetCategories()` método ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image21.png))


Finalmente, configure as configurações do DropDownList, de modo que o `CategoryName` campo é exibido em cada DropDownList `ListItem` com o `CategoryID` usado como o valor do campo.


[![Tem o campo CategoryName exibido e o CategoryID usado como o valor](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figura 8**: tem o `CategoryName` campo exibido e o `CategoryID` usado como o valor ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image24.png))


Depois de fazer essas alterações a marcação declarativa para o `EditItemTemplate` no `CategoryName` TemplateField incluirá DropDownList e um ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList no `EditItemTemplate` devem ter seu estado de exibição habilitado. Em breve adicionaremos sintaxe de associação de dados para a sintaxe declarativa do DropDownList e comandos de associação de dados como `Eval()` e `Bind()` só pode aparecer em controles cujo estado de exibição está habilitado.


Repita essas etapas para adicionar um DropDownList denominado `Suppliers` para o `SupplierName` do TemplateField `EditItemTemplate`. Isso envolve a adição DropDownList para o `EditItemTemplate` e criar outro ObjectDataSource. O `Suppliers` ObjectDataSource do DropDownList, no entanto, deve ser configurada para invocar o `SuppliersBLL` da classe `GetSuppliers()` método. Além disso, configure o `Suppliers` DropDownList para exibir o `CompanyName` campo e use o `SupplierID` campo como o valor de seu `ListItem` s.

Depois de adicionar o DropDownLists para os dois `EditItemTemplate` s, carregar a página em um navegador e clique no botão Editar para o produto de Seasoning Cajun do chefe Anton. Como mostra a Figura 9, colunas de categoria e o fornecedor do produto são renderizadas como listas suspensas contendo as categorias disponíveis e fornecedores à sua escolha. No entanto, observe que o *primeiro* itens nas listas suspensas são selecionadas por padrão (para a categoria de bebidas) e líquidos exóticos como o fornecedor, apesar Cajun Seasoning do chefe Anton é um Condiment fornecido pelo New Orleans Cajun Delights.


[![O primeiro Item na lista suspensa mostra é selecionado por padrão](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figura 9**: O primeiro Item na lista suspensa mostra é selecionada por padrão ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image27.png))


Além disso, se você clicar em atualizar, você verá que o produto `CategoryID` e `SupplierID` valores são definidos como `NULL`. Ambos maneira indesejada comportamentos são causados porque o DropDownLists no `EditItemTemplate` s não estão associados a campos de dados dos dados subjacentes do produto.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Associação DropDownLists para o`CategoryID`e`SupplierID`campos de dados

Para ter a categoria e o fornecedor do produto editado listas suspensas definir os valores apropriados e para ter estes valores enviadas de volta para o BLL `UpdateProduct` método após clicar em atualizar, é preciso associar 'DropDownLists `SelectedValue` propriedades para o `CategoryID` e `SupplierID` campos de dados usando a associação de dados bidirecional. Para fazer isso com o `Categories` DropDownList, você pode adicionar `SelectedValue='<%# Bind("CategoryID") %>'` diretamente para a sintaxe declarativa.

Como alternativa, você pode definir databindings do DropDownList editando o modelo por meio do Designer e clique no link Editar DataBindings de marca inteligente do DropDownList. Em seguida, indicar que o `SelectedValue` propriedade deve ser associada ao `CategoryID` usando associação de dados bidirecional (consulte a Figura 10). Repita o processo declarativo ou Designer para associar o `SupplierID` campo de dados para o `Suppliers` DropDownList.


[![Associar o CategoryID a propriedade SelectedValue do DropDownList usando associação de dados bidirecional](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figura 10**: associar o `CategoryID` para a DropDownList `SelectedValue` propriedade usando bidirecional Databinding ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image30.png))


Depois que as associações foram aplicadas a `SelectedValue` propriedades de dois DropDownLists, colunas de categoria e o fornecedor do produto editado usará como padrão os valores do produto atual. Ao clicar em atualizar, o `CategoryID` e `SupplierID` valores do item de lista suspensa selecionada serão passados para o `UpdateProduct` método. A Figura 11 mostra o tutorial após terem sido adicionadas as instruções de associação de dados; Observe como os itens de lista suspensa selecionada para Seasoning do chefe Anton Cajun corretamente são Condiment e New Orleans Cajun Delights.


[![Categoria do produto editado e valores de fornecedor são selecionados por padrão](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figura 11**: O editado do produto atual de categoria e valores de fornecedor são selecionados por padrão ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Tratamento`NULL`valores

O `CategoryID` e `SupplierID` colunas o `Products` tabela pode ser `NULL`, ainda DropDownLists no `EditItemTemplate` s não incluir um item de lista para representar um `NULL` valor. Isso tem duas consequências:

- Usuário não pode usar nossa interface para alterar a categoria de um produto ou o fornecedor de não`NULL` valor para um `NULL` um
- Se um produto tem um `NULL` `CategoryID` ou `SupplierID`, clicando no botão Editar resultará em uma exceção. Isso ocorre porque o `NULL` valor retornado por `CategoryID` (ou `SupplierID`) no `Bind()` instrução não é mapeado para um valor na DropDownList (DropDownList lança uma exceção quando seu `SelectedValue` propriedade é definida como um valor *não* em sua coleção de itens de lista).

Para oferecer suporte a `NULL` `CategoryID` e `SupplierID` valores, é preciso adicionar outro `ListItem` para cada DropDownList para representar o `NULL` valor. No [filtragem de mestre/detalhes com um DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, vimos como adicionar mais `ListItem` para uma ligação de dados DropDownList, qual envolvida Configurando a DropDownList `AppendDataBoundItems` propriedade `True` e adicionando manualmente adicional `ListItem`. Neste tutorial anterior, no entanto, adicionamos uma `ListItem` com um `Value` de `-1`. A lógica de associação de dados no ASP.NET, no entanto, serão automaticamente convertidos em uma cadeia de caracteres em branco para uma `NULL` valor e vice-versa um. Portanto, para este tutorial, desejamos o `ListItem`do `Value` para ser uma cadeia de caracteres vazia.

Comece definindo 'ambos DropDownLists `AppendDataBoundItems` propriedade `True`. Em seguida, adicione o `NULL` `ListItem` adicionando o seguinte `<asp:ListItem>` elemento para cada DropDownList para que a marcação declarativa parece como:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Escolhi usar "(None)" como o valor de texto para este `ListItem`, mas você pode alterá-la para também ser uma cadeia de caracteres em branco se desejar.

> [!NOTE]
> Como vimos no *filtragem de mestre/detalhes com um DropDownList* tutorial, `ListItem` s podem ser adicionados a DropDownList por meio do Designer, clicando na DropDownList `Items` propriedade na janela Propriedades (que exibirá o `ListItem` Editor de coleção). No entanto, certifique-se de adicionar o `NULL` `ListItem` para este tutorial por meio de sintaxe declarativa. Se você usar o `ListItem` Editor de coleção, a sintaxe declarativa gerada omitirá a `Value` configuração completamente quando atribuído a uma cadeia de caracteres em branco, criar marcação declarativa como: `<asp:ListItem>(None)</asp:ListItem>`. Embora isso possa parecer inofensivo, o valor ausente faz com que o DropDownList usar o `Text` valor da propriedade em seu lugar. Isso significa que, se isso `NULL` `ListItem` é selecionada, o valor "(None)" será tentado a ser atribuído ao `CategoryID`, que resultará em uma exceção. Definindo explicitamente `Value=""`, um `NULL` valor será atribuído à `CategoryID` quando o `NULL` `ListItem` está selecionado.


Repita essas etapas para DropDownList fornecedores.

Com isso adicionais `ListItem`, a interface de edição agora pode atribuir `NULL` valores para um produto `CategoryID` e `SupplierID` campos, conforme mostrado na Figura 12.


[![Escolha (nenhum) para atribuir um valor nulo para a categoria ou o fornecedor do produto](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figura 12**: escolha (nenhum) para atribuir um `NULL` valor para a categoria de um produto ou o fornecedor ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Etapa 4: Usando botões de opção para o Status Descontinuado

Atualmente os produtos `Discontinued` campo de dados é expresso usando um CheckBoxField, que renderiza uma caixa de seleção desabilitada para as linhas de somente leitura e uma caixa de seleção habilitada para a linha que está sendo editada. Enquanto essa interface do usuário geralmente é adequado, é possível personalizá-lo se necessário usando um TemplateField. Para este tutorial, vamos alterar CheckBoxField em um TemplateField que usa um controle RadioButtonList com duas opções "Ativas" e "Descontinuado" da qual o usuário pode especificar o produto `Discontinued` valor.

Iniciar convertendo o `Discontinued` CheckBoxField em um TemplateField, que criará um TemplateField com um `ItemTemplate` e `EditItemTemplate`. Ambos os modelos incluem uma caixa de seleção com seu `Checked` propriedade associada para o `Discontinued` do campo de dados, a única diferença entre as duas é que o `ItemTemplate`da caixa de seleção `Enabled` está definida como `False`.

Substituir a caixa de seleção em ambos os `ItemTemplate` e `EditItemTemplate` com um controle RadioButtonList, definir ambos os RadioButtonLists' `ID` propriedades para `DiscontinuedChoice`. Em seguida, indicam que o RadioButtonLists devem contêm dois botões de opção, uma rotulada "ativo" com um valor de "False" e um rotulado "Descontinuado" com um valor de "True". Para fazer isso, você pode digitar o `<asp:ListItem>` elementos no diretamente usando a sintaxe declarativa ou use o `ListItem` Editor de coleção do Designer. A Figura 13 mostra o `ListItem` Editor de coleção depois que as duas opções de botão de opção especificado.


[![Adicionar opções descontinuadas e ativas a RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figura 13**: Adicionar ativa e as opções Descontinuado RadioButtonList ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image39.png))


Desde o RadioButtonList no `ItemTemplate` não devem ser editadas, definir seu `Enabled` propriedade para `False`, deixando o `Enabled` propriedade para `True` (o padrão) para RadioButtonList no `EditItemTemplate`. Isso tornará os botões de opção na linha não editado como somente leitura, mas permitirá que o usuário altere os valores de RadioButton para a linha editada.

Ainda é preciso atribuir dos controles RadioButtonList `SelectedValue` propriedades para que o botão de opção apropriado é selecionado com base do produto `Discontinued` campo de dados. Assim como acontece com o DropDownLists examinados anteriormente neste tutorial, essa sintaxe de associação de dados pode ser adicionado diretamente na marcação declarativa ou por meio do link Editar DataBindings em marcas inteligentes dos RadioButtonLists.

Depois de adicionar os dois RadioButtonLists e configurá-los, o `Discontinued` marcação declarativa do TemplateField deve parecer com:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Com essas alterações, o `Discontinued` coluna foi transformada em uma lista de caixas de seleção para uma lista de pares de botão de opção (consulte a Figura 14). Ao editar um produto, o botão de opção apropriado está selecionado e o status do produto descontinuados podem ser atualizado selecionando o botão de opção outros e clicando em Atualizar.


[![As caixas de seleção descontinuadas foram substituídas por pares de botão de opção](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figura 14**: O Descontinuado caixas de seleção foram substituídas por pares de botão de opção ([clique para exibir a imagem em tamanho normal](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Como o `Discontinued` coluna o `Products` banco de dados não pode ter `NULL` valores, não sendo necessário se preocupar com a captura `NULL` informações na interface do. Se, no entanto, `Discontinued` coluna pode conter `NULL` valores que queremos adicionar um terceiro botão à lista com seus `Value` definido como uma cadeia de caracteres vazia (`Value=""`), assim como com o fornecedor DropDownLists e categoria.


## <a name="summary"></a>Resumo

Enquanto o BoundField e CheckBoxField automaticamente renderizam interfaces somente leitura, editando e inserindo, eles não têm a capacidade de personalização. Muitas vezes, no entanto, vamos precisar personalizar a edição ou a inserção de interface, talvez adicionando controles de validação (como visto no tutorial anterior) ou personalizando a interface de usuário de coleta de dados (como visto neste tutorial). Personalizando a interface com um TemplateField pode ser somada nas etapas a seguir:

1. Adicionar um TemplateField ou converter um BoundField existente ou CheckBoxField em um TemplateField
2. Aumentar a interface, conforme necessário
3. Associar os campos de dados apropriado para os controles da Web recém-adicionado usando associação de dados bidirecional

Além de usar os controles da Web ASP.NET internos, você também pode personalizar os modelos de um TemplateField com controles de servidor personalizado, compilados e controles de usuário.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Próximo](implementing-optimistic-concurrency-vb.md)
