---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Lote de atualização (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como atualizar vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, criamos um GridView onde cada linha é editável. Nos dados...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e5062898ca683571df2929eba5d824f9d77accd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833395"
---
<a name="batch-updating-vb"></a>Lote de atualização (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) ou [baixar PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Saiba como atualizar vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, criamos um GridView onde cada linha é editável. Na camada de acesso a dados, podemos encapsular as várias operações de atualização em uma transação para garantir que todas as atualizações são bem-sucedidas ou todas as atualizações serão revertidas.


## <a name="introduction"></a>Introdução

No [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md) , vimos como estender a camada de acesso a dados para adicionar suporte para transações de banco de dados. As transações de banco de dados garantem que uma série de instruções de modificação de dados será tratada como uma operação atômica, o que garante que todas as modificações apresentarão falha ou tudo será bem-sucedida. Com essa funcionalidade DAL baixo nível fora do caminho, estamos re pronto para voltar nossas atenções para a criação de interfaces de modificação de dados de lote.

Neste tutorial, criaremos um GridView onde cada linha é editável (veja a Figura 1). Como cada linha é processada na sua interface de edição, daí s sem a necessidade de uma coluna de edição, atualizar e Cancelar botões. Em vez disso, há dois botões atualizar produtos na página que, quando clicado, enumerar as linhas de GridView e atualizar o banco de dados.


[![Cada linha de GridView é editável](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figura 1**: cada linha GridView é editável ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image2.png))


Permitir que o s começar!

> [!NOTE]
> No [executando as atualizações em lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) tutorial, criamos um lote de edição da interface usando o controle DataList. Este tutorial difere do anterior em que haja usa uma GridView e a atualização em lotes é executada dentro do escopo de uma transação. Após concluir este tutorial eu recomendo que você volte para o tutorial anterior e atualizá-lo para usar a funcionalidade de relacionadas à transação de banco de dados adicionada no tutorial anterior.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examinando as etapas para fazer todas as linhas de GridView editável

Conforme discutido na [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, o GridView oferece suporte interno para editar seus dados subjacentes em uma base por linha. Internamente, o GridView notas qual linha é editável por meio de seu [ `EditIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Como o GridView está sendo associado à fonte de dados, ele verifica que cada linha para ver se o índice da linha é igual ao valor de `EditIndex`. Nesse caso, essa linha s campos são renderizados usando a edição de suas interfaces. Para BoundFields, a interface de edição é uma caixa de texto cuja `Text` propriedade recebe o valor do campo de dados especificado pelo s BoundField `DataField` propriedade. Para TemplateFields, o `EditItemTemplate` é usado em vez do `ItemTemplate`.

Lembre-se de que o fluxo de trabalho de edição começa quando um usuário clica em um botão de edição de linha s. Isso faz com que um postback, define o GridView s `EditIndex` propriedade para o índice da linha clicada s e reassociações os dados à grade. Quando um botão Cancelar de linha s é clicado, em um postback de `EditIndex` é definido como um valor de `-1` antes dos dados para a grade de reassociação. Uma vez que as linhas de s GridView começar em zero de indexação, definindo `EditIndex` para `-1` tem o efeito de exibição GridView em modo somente leitura.

O `EditIndex` propriedade funciona bem para edição por linha, mas não foi projetada para edição de lote. Para tornar todo GridView editável, precisamos ter cada linha renderizar usando sua interface de edição. A maneira mais fácil de fazer isso é criar onde cada campo editável é implementado como um TemplateField com sua interface de edição definido no `ItemTemplate`.

Ao longo dos próximos várias etapas, criaremos um GridView editável completamente. Na etapa 1 vamos começar criando o GridView e seu ObjectDataSource e converter seus BoundFields e CheckBoxField em TemplateFields. Nas etapas 2 e 3, veremos as interfaces de edição do TemplateFields `EditItemTemplate` s para seus `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Etapa 1: Exibir informações sobre o produto

Antes de nos preocupamos criando um GridView onde estão as linhas é editável, permitem que o s começa simplesmente exibindo as informações do produto. Abra o `BatchUpdate.aspx` página o `BatchData` pasta e arraste um controle GridView na caixa de ferramentas para o Designer. Definir o s GridView `ID` à `ProductsGrid` e, na marca inteligente, de escolha para associá-lo para um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para recuperar seus dados a partir de `ProductsBLL` classe s `GetProducts` método.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figura 2**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image4.png))


[![Recuperar os dados de produto usando o método GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figura 3**: recupere os dados de produto usando o `GetProducts` método ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image6.png))


Como GridView, os recursos de modificação de s de ObjectDataSource são projetados para trabalhar em uma base por linha. Para atualizar um conjunto de registros, precisaremos escrever um pouco de código em que a classe de code-behind de s de página ASP.NET que processa os dados em lotes e passa-o para a BLL. Portanto, defina as listas suspensas em s ObjectDataSource guias UPDATE, INSERT e DELETE como (nenhum). Clique em Concluir para concluir o assistente.


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figura 4**: defina a lista suspensa no UPDATE, INSERT e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, a marcação declarativa do ObjectDataSource s deve ser semelhante ao seguinte:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Concluindo o Assistente Configurar fonte de dados também faz com que o Visual Studio para criar um CheckBoxField para os campos de dados de produto e BoundFields no GridView. Para este tutorial, deixe s apenas permitir que o usuário exiba e edite o s nome do produto, categoria, preço e status descontinuados. Remover tudo, exceto os `ProductName`, `CategoryName`, `UnitPrice`, e `Discontinued` campos e renomeie o `HeaderText` propriedades dos três primeiros campos para o produto, categoria e preço, respectivamente. Por fim, marque as caixas de seleção Habilitar paginação e habilitar a classificação na marca inteligente s GridView.

Neste ponto, o GridView tem três BoundFields (`ProductName`, `CategoryName`, e `UnitPrice`) e um CheckBoxField (`Discontinued`). Precisamos converter esses quatro campos no TemplateFields e, em seguida, mova a interface de edição de s TemplateField `EditItemTemplate` para seu `ItemTemplate`.

> [!NOTE]
> Exploramos criando e personalizando TemplateFields na [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial. Vamos percorrer as etapas de conversão a BoundFields e CheckBoxField em TemplateFields e definir sua edição interfaces em seus `ItemTemplate` s, mas se você tiver problemas ou precisa de um lembrete, don t hesite em entrar para fazer referência a este tutorial anterior.


Da GridView s marca inteligente, clique no link de editar colunas para abrir a caixa de diálogo de campos. Em seguida, selecione cada campo e clique em converter este campo em um TemplateField link.


![Converter a BoundFields existente e CheckBoxField em TemplateFields](batch-updating-vb/_static/image5.gif)

**Figura 5**: converter a BoundFields existente e CheckBoxField em TemplateFields


Agora que cada campo é um TemplateField, podemos está pronto para mover a edição da interface do `EditItemTemplate` s para o `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Etapa 2: Criando o`ProductName`,`UnitPrice`, e`Discontinued`Interfaces de edição

Criando o `ProductName`, `UnitPrice`, e `Discontinued` interfaces de edição são o tópico desta etapa e são bastante simples, pois cada interface já está definido em s TemplateField `EditItemTemplate`. Criando o `CategoryName` interface de edição é um pouco mais complicado, pois precisamos criar uma DropDownList das categorias aplicáveis. Isso `CategoryName` interface de edição é abordado na etapa 3.

Permitir que o s comece com o `ProductName` TemplateField. Clique no link Editar modelos da marca inteligente s GridView e fazer drill down até o `ProductName` TemplateField s `EditItemTemplate`. Selecione a caixa de texto, copie-o para a área de transferência e, em seguida, cole-o para o `ProductName` TemplateField s `ItemTemplate`. Alterar o s TextBox `ID` propriedade para `ProductName`.

Em seguida, adicione um RequiredFieldValidator para o `ItemTemplate` para garantir que o usuário fornece um valor para cada nome de produto s. Defina as `ControlToValidate` propriedade ProductName, o `ErrorMessage` propriedade para que você deve fornecer o nome do produto. e o `Text` propriedade para \*. Depois de fazer essas adições para o `ItemTemplate`, sua tela deve ser semelhante à Figura 6.


[![O ProductName TemplateField agora inclui uma caixa de texto e um RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figura 6**: O `ProductName` TemplateField agora inclui uma caixa de texto e um RequiredFieldValidator ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image10.png))


Para o `UnitPrice` interface de edição, comece copiando-se a caixa de texto do `EditItemTemplate` para o `ItemTemplate`. Em seguida, coloque um $ na frente de caixa de texto e defina suas `ID` propriedade UnitPrice e seu `Columns` propriedade para 8.

Adicione também um CompareValidator para o `UnitPrice` s `ItemTemplate` para garantir que o valor inserido pelo usuário é um valor de moeda válidos maior que ou igual a US $0,00. Definir o s validador `ControlToValidate` propriedade UnitPrice, seu `ErrorMessage` propriedade para que você deve inserir um valor de moeda válidos. . Omitir qualquer moeda símbolos., suas `Text` propriedade para \*, sua `Type` propriedade a ser `Currency`, sua `Operator` propriedade a ser `GreaterThanEqual`e seu `ValueToCompare` propriedade como 0.


[![Adicionar um CompareValidator para garantir que o preço inserido é um valor de moeda não negativo](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figura 7**: adicionar um CompareValidator para garantir que o preço inserido é um valor de moeda não negativo ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image12.png))


Para o `Discontinued` TemplateField, você pode usar a caixa de seleção já definida no `ItemTemplate`. Basta definir seus `ID` para descontinuado e seu `Enabled` propriedade `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Etapa 3: Criando o`CategoryName`Interface de edição

A interface de edição a `CategoryName` s TemplateField `EditItemTemplate` contém uma caixa de texto que exibe o valor da `CategoryName` campo de dados. Precisamos substituir isso com uma DropDownList que lista as categorias possíveis.

> [!NOTE]
> O [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial contém uma discussão mais completa e completa sobre como personalizar um modelo para incluir uma DropDownList em vez de uma caixa de texto. Embora as etapas forem concluídas, são apresentados tersely. Para obter uma análise mais detalhada em como criar e configurar as categorias DropDownList, voltar para o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial.


Arraste uma DropDownList da caixa de ferramentas para o `CategoryName` s TemplateField `ItemTemplate`, definindo suas `ID` para `Categories`. Neste ponto, normalmente definiria a fonte de dados s DropDownLists por meio de sua marca inteligente, criando um novo ObjectDataSource. No entanto, isso adicionará o ObjectDataSource dentro de `ItemTemplate`, que resulta em uma instância de ObjectDataSource criada para cada linha de GridView. Em vez disso, deixe s criar o ObjectDataSource fora o s GridView TemplateFields. Encerrar a edição do modelo e arraste um ObjectDataSource da caixa de ferramentas para o Designer sob o `ProductsDataSource` ObjectDataSource. Nomeie o novo ObjectDataSource `CategoriesDataSource` e configurá-lo para usar o `CategoriesBLL` classe s `GetCategories` método.


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figura 8**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image14.png))


[![Recuperar os dados da categoria usando o método GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figura 9**: recupere os dados de categoria usando o `GetCategories` método ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image16.png))


Como esse ObjectDataSource é usada simplesmente para recuperar dados, defina as listas suspensas nas guias de UPDATE e DELETE como (nenhum). Clique em Concluir para concluir o assistente.


[![Conjunto de listas suspensas na atualização e exclusão guias como (nenhum)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figura 10**: defina a lista suspensa na atualização e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image18.png))


Depois de concluir o assistente, o `CategoriesDataSource` marcação declarativa de s deve parecer com o seguinte:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Com o `CategoriesDataSource` criado e configurado, retornar para o `CategoryName` TemplateField s `ItemTemplate` e, da DropDownList s marca inteligente, clique no link Escolher fonte de dados. No Assistente de configuração de fonte de dados, selecione a `CategoriesDataSource` opção na primeira lista suspensa e optar por ter `CategoryName` usado para a exibição e `CategoryID` como o valor.


[![Associar o CategoriesDataSource DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figura 11**: associar a DropDownList para a `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image20.png))


Neste ponto o `Categories` DropDownList lista todas as categorias, mas ele não ainda automaticamente seleciona a categoria apropriada para o produto associado à linha GridView. Para fazer isso, precisamos definir a `Categories` s DropDownList `SelectedValue` para o produto s `CategoryID` valor. Clique no link Editar DataBindings de marca inteligente DropDownList s e associar a `SelectedValue` propriedade com o `CategoryID` campo de dados, conforme mostrado na Figura 12.


![Associar o valor de CategoryID produto s à propriedade SelectedValue DropDownList s](batch-updating-vb/_static/image12.gif)

**Figura 12**: associar o produto s `CategoryID` valor para o s DropDownList `SelectedValue` propriedade


Uma última permanece do problema: se o produto ainda não t tiver um `CategoryID` valor, em seguida, especificado a declaração de associação de dados no `SelectedValue` resultará em uma exceção. Isso ocorre porque a DropDownList contém apenas os itens das categorias e não oferece uma opção para produtos que têm uma `NULL` banco de dados valor para `CategoryID`. Para corrigir isso, defina o s DropDownList `AppendDataBoundItems` propriedade para `True` e adicione um novo item ao DropDownList, omitindo o `Value` propriedade da sintaxe declarativa. Ou seja, certifique-se de que o `Categories` sintaxe declarativa do DropDownList s é semelhante ao seguinte:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Observe como o `<asp:ListItem Value="">` – selecione um – tem seu `Value` atributo definido explicitamente como uma cadeia de caracteres vazia. Voltar para o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial para uma discussão mais completa sobre por que esse item DropDownList adicional é necessária para lidar com o `NULL` caso e por que a atribuição do `Value` propriedade como uma cadeia de caracteres vazia é essencial.

> [!NOTE]
> Há um desempenho e escalabilidade problema potencial aqui que vale a pena mencionar. Uma vez que cada linha tem uma DropDownList que usa o `CategoriesDataSource` como sua fonte de dados, o `CategoriesBLL` classe s `GetCategories` método será chamado *n* visitam vezes por página, em que *n* é o número de linhas em um GridView. Eles *n* chama `GetCategories` resultar em *n* consultas ao banco de dados. Esse impacto no banco de dados pode ser amenizado por armazenamento em cache as categorias retornadas em um cache por solicitação ou por meio da camada de armazenamento em cache usando uma dependência ou uma expiração muito com base em curto período de tempo de cache de SQL. Para obter mais informações sobre a solicitação cache opção, consulte [ `HttpContext.Items` Store de Cache de uma solicitação por](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Etapa 4: Concluir a Interface de edição

Podemos var feitas algumas alterações nos modelos de s GridView sem pausar para exibir nosso progresso. Reserve um tempo para exibir nosso progresso através de um navegador. Como mostra a Figura 13, cada linha é renderizada utilizando seu `ItemTemplate`, que contém o s célula interface de edição.


[![Cada linha de GridView é editável](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figura 13**: cada linha de GridView é editável ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image22.png))


Há alguns problemas de formatação secundários deve cuidar de neste momento. Primeiro, observe que o `UnitPrice` valor contém quatro pontos decimais. Para corrigir esse problema, retorne para o `UnitPrice` TemplateField s `ItemTemplate` e, da caixa de texto s marca inteligente, clique no link Editar DataBindings. Em seguida, especifique que o `Text` propriedade deve ser formatada como um número.


![Formatar a propriedade Text como um número](batch-updating-vb/_static/image14.gif)

**Figura 14**: formato de `Text` propriedade como um número


Em segundo lugar, let s centralizar a caixa de seleção o `Discontinued` coluna (em vez de tê-la alinhado à esquerda). Clique em Edit Columns da marca inteligente GridView s e selecione o `Discontinued` TemplateField da lista de campos no canto inferior esquerdo. Uma busca detalhada `ItemStyle` e defina o `HorizontalAlign` propriedade Center, conforme mostrado na Figura 15.


![Centralizar na caixa de seleção descontinuada](batch-updating-vb/_static/image15.gif)

**Figura 15**: Centro o `Discontinued` caixa de seleção


Em seguida, adicione um controle ValidationSummary para a página e defina suas `ShowMessageBox` propriedade para `True` e seu `ShowSummary` propriedade `False`. Também adicione que controles de Web de botão que, quando clicado, atualizará as alterações do usuário s. Especificamente, adicione dois controles da Web de botão, um acima GridView e outro abaixo dele, definir ambos os controles `Text` propriedades aos produtos de atualização.

Desde o GridView s editando a interface é definida no seu TemplateFields `ItemTemplate` s, o `EditItemTemplate` s são supérfluos e pode ser excluído.

Depois de fazer acima mencionado alterações de formatação, adicionar os controles de botão e remover o desnecessárias `EditItemTemplate` s, sua sintaxe declarativa da página s deve parecer com o seguinte:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

A Figura 16 mostra essa página quando visualizado por meio de um navegador depois que os controles da Web de botão foram adicionados e as alterações feitas na formatação.


[![A página agora inclui dois botões de produtos de atualização](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figura 16**: A página agora inclui dois produtos de botões de atualização ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Etapa 5: Atualizar os produtos

Quando um usuário visita esta página eles fazem as modificações e, em seguida, clique em um dos dois botões de produtos de atualização. Nesse ponto, precisamos salvar alguma forma, os valores inseridos pelo usuário para cada linha em uma `ProductsDataTable` da instância e, em seguida, passá-lo para um método BLL que será, em seguida, passá-lo `ProductsDataTable` instância para o s DAL `UpdateWithTransaction` método. O `UpdateWithTransaction` método, que criamos na [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md), garante que o lote de alterações será atualizado como uma operação atômica.

Crie um método chamado `BatchUpdate` em `BatchUpdate.aspx.vb` e adicione o seguinte código:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Esse método começa obtendo todos os produtos em um `ProductsDataTable` por meio de uma chamada para o s BLL `GetProducts` método. Em seguida, enumera os `ProductGrid` s GridView [ `Rows` coleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). O `Rows` coleção contém um [ `GridViewRow` instância](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) para cada linha exibida no GridView. Uma vez que estamos mostrando no máximo dez linhas por página, o GridView s `Rows` coleção terá não mais do que dez itens.

Para cada linha a `ProductID` são capturados do `DataKeys` coleção e apropriado `ProductsRow` é selecionado do `ProductsDataTable`. Os quatro controles de entrada TemplateField são referenciados por meio de programação e seus valores atribuídos ao `ProductsRow` s propriedades da instância. Após cada GridView linha s valores foram usados para atualizar o `ProductsDataTable`, ele s passados para o s BLL `UpdateWithTransaction` método que, como vimos no tutorial anterior, simplesmente chama para baixo em s DAL `UpdateWithTransaction` método.

O algoritmo de atualização de lote usado para este tutorial atualiza cada linha no `ProductsDataTable` que corresponde a uma linha no controle GridView, independentemente se as informações do produto s foi alteradas. Enquanto essa blind atualiza t são inválidas normalmente um problema de desempenho, eles podem levar supérfluos registros se você está a auditoria for alterado para a tabela de banco de dados. Volta a [executar atualizações de lote](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) tutorial podemos explorou um lote atualizando a interface com o DataList e adicionou o código que atualizará somente os registros que realmente foram modificados pelo usuário. Fique à vontade para usar as técnicas de [executando as atualizações em lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) para atualizar o código neste tutorial, se desejado.

> [!NOTE]
> Ao associar a fonte de dados para o GridView por meio de sua marca inteligente, o Visual Studio automaticamente atribui os valores de chave primária do fonte s dados para o s GridView `DataKeyNames` propriedade. Se você não se associaram o ObjectDataSource para GridView por meio da marca inteligente do GridView s conforme descrito na etapa 1, você precisará definir manualmente o s GridView `DataKeyNames` propriedade ProductID para acessar o `ProductID` valor para cada linha por meio de `DataKeys` coleção.


O código usado na `BatchUpdate` é semelhante àquela usada em s BLL `UpdateProduct` métodos, a principal diferença é que, no `UpdateProduct` métodos somente uma única `ProductRow` instância é recuperada da arquitetura. O código que atribui as propriedades do `ProductRow` é o mesmo entre o `UpdateProducts` métodos e o código dentro de `For Each` loop na `BatchUpdate`, como é o padrão geral.

Para concluir este tutorial, é preciso ter o `BatchUpdate` método invocado quando qualquer um dos botões de produtos de atualização é clicado. Criar manipuladores de eventos para o `Click` eventos desses dois controles de botão e adicione o seguinte código nos manipuladores de eventos:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Primeiro é feita uma chamada para `BatchUpdate`. Em seguida, o [ `ClientScript` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) é usado para injetar o JavaScript que exibirá uma caixa de mensagem que lê os produtos foram atualizados.

Reserve um minuto para testar esse código. Visite `BatchUpdate.aspx` por meio de um navegador, um número de linhas de editar e clique em um dos botões de produtos de atualização. Supondo que não há nenhum erro de validação de entrada, você deve ver uma caixa de mensagem que lê que os produtos foram atualizados. Para verificar a atomicidade da atualização, considere a adição de um aleatório `CHECK` restrição, como um que não permite `UnitPrice` valores de 1234,56. Em seguida, de `BatchUpdate.aspx`, edite um número de registros, certificando-se de definir um produto s `UnitPrice` valor para o valor "proibido" (1234,56). Isso deve resultar em um erro quando clicar em produtos de atualização com as outras alterações durante essa operação em lote revertida para seus valores originais.

## <a name="an-alternativebatchupdatemethod"></a>Uma alternativa`BatchUpdate`método

O `BatchUpdate` método simplesmente examinado recupera *todas as* dos produtos de s BLL `GetProducts` método e, em seguida, atualiza apenas os registros que aparecem no GridView. Essa abordagem é ideal se GridView não usa paginação, mas se isso acontecer, pode haver centenas, milhares ou dezenas de milhares de produtos, mas apenas dez linhas em GridView. Nesse caso, obtendo todos os produtos de banco de dados somente para modificar 10 delas é inferior ao ideal.

Para esses tipos de situações, considere usar o seguinte `BatchUpdateAlternate` método em vez disso:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` começa criando um novo vazio `ProductsDataTable` chamado `products`. Em seguida, ele percorre o s GridView `Rows` coleta e, para cada linha obtém as informações de produto específico usando o s BLL `GetProductByProductID(productID)` método. Recuperada `ProductsRow` instância tem suas propriedades atualizadas da mesma forma como `BatchUpdate`, mas depois de atualizar a linha for importado para o `products` `ProductsDataTable` por meio de s DataTable [ `ImportRow(DataRow)` método](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Após o `For Each` loop estiver concluído, `products` contém um `ProductsRow` instância para cada linha de GridView. Desde que cada um da `ProductsRow` instâncias foram adicionadas para o `products` (em vez de atualizado), se cegamente de passá-lo para o `UpdateWithTransaction` método o `ProductsTableAdatper` tentará inserir cada um dos registros de banco de dados. Em vez disso, precisamos especificar que cada uma dessas linhas foi modificada (não adicionado).

Isso pode ser feito adicionando um novo método para a BLL chamada `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, conforme mostrados abaixo, conjuntos a `RowState` de cada um dos `ProductsRow` instâncias na `ProductsDataTable` para `Modified` e, em seguida, passa a `ProductsDataTable` no s DAL `UpdateWithTransaction` método.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Resumo

O GridView fornece recursos de edição de linha de interno, mas não tem suporte para a criação de interfaces totalmente editáveis. Como vimos neste tutorial, essas interfaces são possíveis, mas exigem um pouco de trabalho. Para criar um GridView onde cada linha é editável, precisamos converter os campos de s GridView em TemplateFields e definir a interface de edição dentro de `ItemTemplate` s. Além disso, atualize todos os - tipo de controles da Web de botão deve ser adicionado à página, separada do GridView. Esses botões `Click` manipuladores de eventos precisam enumerar os s GridView `Rows` coleção, armazenar as alterações em um `ProductsDataTable`e passar as informações atualizadas para o método apropriado de BLL.

No próximo tutorial, veremos como criar uma interface para exclusão em lotes. Em particular, cada linha de GridView será incluir uma caixa de seleção e, em vez de atualizar todos os-digite botões, teremos botões excluir linhas selecionadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy e David Suru. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](wrapping-database-modifications-within-a-transaction-vb.md)
> [Próximo](batch-deleting-vb.md)
