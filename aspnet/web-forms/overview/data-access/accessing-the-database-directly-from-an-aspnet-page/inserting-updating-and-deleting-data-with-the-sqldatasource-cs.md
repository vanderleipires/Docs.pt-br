---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Inserindo, atualizando e excluindo dados com SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores aprendemos como o controle ObjectDataSource permitido para inserir, atualizar e excluir dados. O controle SqlDataSource dá suporte a t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 25dab0292aefa183a1abc2615a7ba8e7a512346d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877275"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Inserindo, atualizando e excluindo dados com SqlDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) ou [baixar PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Nos tutoriais anteriores aprendemos como o controle ObjectDataSource permitido para inserir, atualizar e excluir dados. O controle SqlDataSource suporta as mesmas operações, mas a abordagem é diferente, e este tutorial mostra como configurar o SqlDataSource para inserir, atualizar e excluir dados.


## <a name="introduction"></a>Introdução

Conforme discutido em [uma visão geral de inserção, atualização e exclusão de](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), o controle GridView fornece interna de atualizar e excluir recursos, enquanto os controles de DetailsView e FormView incluem inserir suportam juntamente com Editar e excluir a funcionalidade. Essas capacidades de modificação de dados podem ser conectadas diretamente a um controle de fonte de dados sem uma linha de código que precisam ser gravados. [Uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) examinado usando ObjectDataSource para facilitar inserindo, atualizando e excluindo com os controles GridView, DetailsView e FormView. Como alternativa, o SqlDataSource pode ser usado no lugar de ObjectDataSource.

Lembre-se de que para dar suporte a inserindo, atualizando e excluindo com ObjectDataSource é necessária para especificar os métodos de camada de objeto a ser invocado para executar a inserção, atualização ou exclusão de ação. Com o SqlDataSource, é preciso fornecer `INSERT`, `UPDATE`, e `DELETE` SQL instruções (ou procedimentos armazenados) para executar. Como veremos neste tutorial, essas instruções podem ser criadas manualmente ou podem ser geradas automaticamente pelo assistente SqlDataSource s configurar fonte de dados.

> [!NOTE]
> Desde a ve já discutidos a inserir, editar e excluir recursos de GridView, DetailsView e FormView controla, este tutorial se concentrará para configurar o controle SqlDataSource para dar suporte a essas operações. Se você precisar aprimorar os conhecimentos sobre como implementar esses recursos em GridView, DetailsView e FormView, retornar aos tutoriais de edição, inserir e excluir dados, começando com [uma visão geral de inserção, atualização e exclusão de](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Etapa 1: Especificar`INSERT`,`UPDATE`, e`DELETE`instruções

Como podemos ve visto nos últimos dois tutoriais, para recuperar dados de um controle SqlDataSource, precisamos definir duas propriedades:

1. `ConnectionString`, que especifica o banco de dados para enviar a consulta, e
2. `SelectCommand`, que especifica o nome do procedimento armazenado a ser executada para retornar os resultados ou instrução de SQL ad hoc.

Para `SelectCommand` valores com parâmetros, o parâmetro os valores são especificados por meio de s SqlDataSource `SelectParameters` coleta e pode incluir valores embutidos, valores de fonte de parâmetros comuns (querystring campos, variáveis de sessão, valores de controle da Web, e assim por diante), ou pode ser atribuído por meio de programação. Quando o SqlDataSource controlar s `Select()` método é invocado por meio de programação ou automaticamente de um controle da Web de dados é estabelecida uma conexão ao banco de dados, os valores de parâmetro são atribuídos à consulta e o comando é shuttled off para o banco de dados. Os resultados são retornados como um conjunto de dados ou um DataReader, dependendo do valor do controle s `DataSourceMode` propriedade.

Junto com a seleção de dados, o controle SqlDataSource pode ser usado para inserir, atualizar e excluir dados fornecendo `INSERT`, `UPDATE`, e `DELETE` instruções SQL em quase da mesma forma. Basta atribuir o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades de `INSERT`, `UPDATE`, e `DELETE` instruções SQL para executar. Se as instruções têm parâmetros (como mais sempre serão), incluí-los no `InsertParameters`, `UpdateParameters`, e `DeleteParameters` coleções.

Uma vez um `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` valor foi especificado, a opção Habilitar inserindo, habilitar edição ou exclusão de habilitar os dados correspondentes marca inteligente do controle s Web estará disponível. Para ilustrar isso, vamos s veja um exemplo da `Querying.aspx` página criada no [consultando dados com o controle SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) tutorial e incrementá-lo para incluir excluir recursos.

Comece abrindo o `InsertUpdateDelete.aspx` e `Querying.aspx` páginas do `SqlDataSource` pasta. No Designer no `Querying.aspx` , selecione o SqlDataSource e GridView do primeiro exemplo (o `ProductsDataSource` e `GridView1` controles). Depois de selecionar os dois controles, vá para o menu Editar e escolha ' Copiar ' (ou apenas ocorrências Ctrl + C). Em seguida, vá para o Designer de `InsertUpdateDelete.aspx` e cole os controles. Depois que você tiver movido dois controles para `InsertUpdateDelete.aspx`, teste a página em um navegador. Você deve ver os valores da `ProductID`, `ProductName`, e `UnitPrice` colunas para todos os registros no `Products` tabela de banco de dados.


[![Todos os produtos são listados, ordenados por ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: todos os produtos são listados, ordenados por `ProductID` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Adicionando a s SqlDataSource`DeleteCommand`e`DeleteParameters`propriedades

Neste ponto, temos um SqlDataSource que retorna todos os registros de simplesmente o `Products` tabela e GridView que processa esses dados. Nosso objetivo é estender esse exemplo para permitir que o usuário exclua produtos por meio de GridView. Para fazer isso é necessário especificar valores para o controle SqlDataSource s `DeleteCommand` e `DeleteParameters` propriedades e, em seguida, configure o GridView para oferecer suporte a exclusão.

O `DeleteCommand` e `DeleteParameters` propriedades podem ser especificadas de várias maneiras:

- Por meio de sintaxe declarativa
- Na janela de propriedades no Designer
- Da tela de procedimento armazenado no Assistente para configurar a fonte de dados ou especificar uma instrução SQL personalizada
- Por meio do botão Avançado nas especificar colunas de uma tabela de tela de exibição no Assistente para configurar a fonte de dados, que, na verdade, gerará automaticamente o `DELETE` coleção de instruções e parâmetros SQL usadas no `DeleteCommand` e `DeleteParameters` Propriedades

Vamos examinar como automaticamente tem o `DELETE` instrução criada na etapa 2. Por enquanto, permitem s usar a janela Propriedades no Designer, embora o Assistente Configurar fonte de dados ou a opção de sintaxe declarativa funcionaria bem.

No Designer de `InsertUpdateDelete.aspx`, clique no `ProductsDataSource` SqlDataSource e, em seguida, exibir a janela de propriedades (no menu Exibir, escolha a janela Propriedades ou simplesmente atingir F4). Selecione a propriedade DeleteQuery, o que abrirá um conjunto de reticências.


![Selecione a propriedade DeleteQuery na janela de propriedades](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Figura 2**: selecione a propriedade DeleteQuery na janela de propriedades


> [!NOTE]
> O SqlDataSource t tem uma propriedade DeleteQuery. Em vez disso, DeleteQuery é uma combinação da `DeleteCommand` e `DeleteParameters` propriedades e só será listado na janela Propriedades, ao exibir a janela por meio do Designer. Se você estiver procurando na janela Propriedades, na exibição da fonte, você encontrará o `DeleteCommand` propriedade em vez disso.


Clique nas reticências na propriedade DeleteQuery para abrir a caixa de diálogo Editor de comando e parâmetro caixa (veja a Figura 3). Nessa caixa de diálogo, você pode especificar o `DELETE` instrução SQL e especifique os parâmetros. Digite a seguinte consulta para o `DELETE` caixa de texto de comando (seja manualmente ou usando o construtor de consultas, se você preferir):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Em seguida, clique no botão Atualizar parâmetros para adicionar o `@ProductID` parâmetro à lista de parâmetros abaixo.


[![Selecione a propriedade DeleteQuery na janela de propriedades](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: selecione a propriedade DeleteQuery na janela de propriedades ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Fazer *não* fornecer um valor para esse parâmetro (deixe seu parâmetro de origem em nenhum). Depois que adicionamos suporte excluindo a GridView, GridView fornecerá automaticamente esse valor de parâmetro, usando o valor de seu `DataKeys` coleção para a linha cuja exclusão foi clicada.

> [!NOTE]
> O nome do parâmetro usado no `DELETE` consulta *deve* ser o mesmo que o nome do `DataKeyNames` valor o GridView, DetailsView ou FormView. Ou seja, o parâmetro no `DELETE` instrução intencionalmente chamada `@ProductID` (em vez de, digamos, `@ID`), porque o nome de coluna de chave primária da tabela de produtos (e, portanto, o valor DataKeyNames em GridView) é `ProductID`.


Se o nome do parâmetro e `DataKeyNames` correspondência de valor de t, GridView não pode atribuir automaticamente o parâmetro do valor da `DataKeys` coleção.

Depois de inserir as informações relacionadas ao excluir na caixa de diálogo Editor de comando e parâmetro clique Okey e vá para o modo de exibição de fonte para examinar a marcação declarativa resultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Observe a adição do `DeleteCommand` propriedade, bem como a `<DeleteParameters>` seção e o objeto de parâmetro nomeado `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurando o GridView para exclusão

Com o `DeleteCommand` propriedade adicionada, a marca inteligente do GridView s agora contém a opção de habilitar a exclusão. Vá em frente e marque esta caixa de seleção. Conforme discutido em [uma visão geral de inserção, atualização e exclusão de](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), isso faz com que o GridView adicionar um CommandField com seu `ShowDeleteButton` propriedade definida como `true`. Como a Figura 4 mostra, quando a página for visitada por meio de um navegador, um botão de exclusão é incluído. Teste esta saída de página, excluindo alguns produtos.


[![Cada linha GridView agora inclui um botão de exclusão](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: cada linha GridView agora inclui um botão Excluir ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Após clicar em um botão de exclusão, ocorre um postback, GridView atribui o `ProductID` o valor do parâmetro do `DataKeys` valor de coleção para a linha cujo botão Excluir foi clicado e invoca o s SqlDataSource `Delete()` método. O controle SqlDataSource, em seguida, conecta-se ao banco de dados e executa o `DELETE` instrução. Em seguida, reconecta GridView para o SqlDataSource, recuperar e exibir o conjunto atual de produtos (que não inclui o registro excluído apenas).

> [!NOTE]
> Desde que o GridView usa seu `DataKeys` coleção para preencher os parâmetros de SqlDataSource, ele s vital que o GridView s `DataKeyNames` propriedade ser definida para as colunas que constituem a chave primária e que o s SqlDataSource `SelectCommand` retorna Essas colunas. Além disso, ele importante que o parâmetro de nome em SqlDataSource s `DeleteCommand` é definido como `@ProductID`. Se o `DataKeyNames` propriedade não está definida ou o parâmetro não é chamado `@ProductsID`, clicando no botão Excluir fará com que um postback, mas não ganha realmente exclui qualquer registro.


Figura 5 mostra essa interação graficamente. Voltar para o [examinando os eventos associados inserindo, atualizando e excluindo](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial para uma discussão mais detalhada sobre a cadeia de eventos associados ao inserir, atualizar e excluir de um controle da Web de dados.


![Clicar no botão de exclusão em GridView invoca o método do SqlDataSource s Delete)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Figura 5**: clicar no botão de exclusão em GridView invoca o s SqlDataSource `Delete()` método


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Etapa 2: Gerar automaticamente o`INSERT`,`UPDATE`, e`DELETE`instruções

Como etapa 1 examinados, `INSERT`, `UPDATE`, e `DELETE` instruções SQL podem ser especificadas com a janela Propriedades ou a sintaxe declarativa de controle s. No entanto, essa abordagem requer que podemos manualmente gravar as instruções SQL manualmente, que pode ser monótonas e propenso a erros. Felizmente, o Assistente Configurar fonte de dados fornece uma opção para fazer o `INSERT`, `UPDATE`, e `DELETE` instruções geradas automaticamente quando usar as especificar colunas de uma tabela de tela de exibição.

Permitir que o s explorar essa opção de geração automática. Adicionar um DetailsView ao Designer no `InsertUpdateDelete.aspx` e defina seu `ID` propriedade `ManageProducts`. Em seguida, de DetailsView s marca inteligente, optar por criar uma nova fonte de dados e criar um SqlDataSource denominado `ManageProductsDataSource`.


[![Criar um novo SqlDataSource denominado ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Figura 6**: criar um novo SqlDataSource nomeado `ManageProductsDataSource` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


No Assistente para configurar a fonte de dados, optar por usar o `NORTHWINDConnectionString` conexão de cadeia de caracteres e clique em Avançar. De configurar a tela de instrução Select, deixe as especificar colunas usando um botão de opção de tabela ou exibição selecionado e escolha o `Products` tabela da lista suspensa. Selecione o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas na lista da caixa de seleção.


[![Usando a tabela de produtos, retornar o ProductID, ProductName, UnitPrice e colunas descontinuadas](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Figura 7**: usando o `Products` da tabela, retornar o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Para gerar automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções com base na tabela selecionada e colunas, clique no botão Avançado e verificar a gerar `INSERT`, `UPDATE`, e `DELETE` caixa de seleção de instruções.


![Marque as caixa de seleção de instruções Generate INSERT, UPDATE e DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Figura 8**: verificar a gerar `INSERT`, `UPDATE`, e `DELETE` instruções caixa de seleção


Generate `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção só será selecionável se a tabela selecionada tem uma chave primária e a coluna de chave primária (ou colunas) são incluídas na lista de colunas retornadas. A caixa de seleção de simultaneidade otimista de uso, que poderá ser selecionada uma vez a gerar `INSERT`, `UPDATE`, e `DELETE` caixa de seleção de instruções foram verificados, será aumentar o `WHERE` cláusulas resultante `UPDATE` e `DELETE` instruções para fornecer controle de simultaneidade otimista. Por enquanto, deixe essa caixa de seleção desmarcada; Vamos examinar a simultaneidade otimista com o controle SqlDataSource no tutorial Avançar.

Depois de verificar a gerar `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção, clique em Okey para retornar à tela de configurar a instrução Select, em seguida, clique em Avançar e, em seguida, Concluir para concluir o Assistente Configurar fonte de dados. Após concluir o assistente, o Visual Studio irá adicionar BoundFields a DetailsView para o `ProductID`, `ProductName`, e `UnitPrice` colunas e um CheckBoxField para o `Discontinued` coluna. De DetailsView s marca inteligente, marque a opção habilitar paginação para que o usuário visitar essa página pode percorrer os produtos. Limpar o s DetailsView também `Width` e `Height` propriedades.

Observe que a marca inteligente tem as opções Habilitar inserindo, habilitar edição e exclusão de habilitar disponíveis. Isso ocorre porque o SqlDataSource contém valores para seus `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, como mostra a seguinte sintaxe declarativa:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Observe como o controle SqlDataSource teve valores definidos automaticamente para sua `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades. O conjunto de colunas referenciadas no `InsertCommand` e `UpdateCommand` propriedades são baseadas no `SELECT` instrução. Isto é, em vez de *cada* coluna produtos no `InsertCommand` e `UpdateCommand`, há somente as colunas especificadas no `SelectCommand` (menos `ProductID`, que é omitido porque ele s um [ `IDENTITY` coluna](http://www.sqlteam.com/item.asp?ItemID=102), cujo valor não pode ser alterado quando editado e que é atribuído automaticamente ao inserir). Além disso, para cada parâmetro a `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades de parâmetros correspondentes no `InsertParameters`, `UpdateParameters`, e `DeleteParameters` coleções.

Para ativar os recursos de modificação de dados de s DetailsView, verifique permitir inserção, habilitar edição e habilitar a exclusão opções nessa marca inteligente. Isso adiciona um CommandField com seu `ShowInsertButton`, `ShowEditButton`, e `ShowDeleteButton` propriedades definidas como `true`.

Visite a página em um navegador e observe o editar, excluir e novos botões incluídos em DetailsView. Clicar no botão Editar DetailsView se transforma em modo de edição, que exibe cada BoundField cujo `ReadOnly` está definida como `false` (o padrão) como uma caixa de texto e CheckBoxField como uma caixa de seleção.


[![O s DetailsView padrão Interface de edição](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Figura 9**: s DetailsView a Interface de edição padrão ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Da mesma forma, você pode excluir o produto selecionado no momento ou adicionar um novo produto no sistema. Como o `InsertCommand` instrução só funciona com o `ProductName`, `UnitPrice`, e `Discontinued` colunas, as outras colunas ou têm `NULL` ou seu valor padrão atribuído pelo banco de dados em insert. Assim como com o ObjectDataSource, se o `InsertCommand` está faltando a qualquer tabela de banco de dados permitem colunas que não t `NULL` s e não t tem um valor padrão, ocorrerá um erro SQL ao tentar executar o `INSERT` instrução.

> [!NOTE]
> O s DetailsView inserir e editar as interfaces não têm qualquer tipo de validação ou personalização. Para adicionar controles de validação ou personalizar as interfaces, você precisa converter o BoundFields TemplateFields. Consulte o [adicionar controles de validação para a edição e inserindo Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutoriais para obter mais informações.


Além disso, tenha em mente que, para atualizar e excluir, DetailsView usa o produto atual s `DataKey` valor, que está presente somente se o `DataKeyNames` propriedade é configurada. Se a edição ou exclusão parece não ter nenhum efeito, certifique-se de que o `DataKeyNames` está definida.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitações de geração automática de instruções SQL

Desde o gerar `INSERT`, `UPDATE`, e `DELETE` opção instruções só está disponível quando escolher colunas de uma tabela, para consultas mais complexas será necessário escrever sua própria `INSERT`, `UPDATE`, e `DELETE` instruções de como fizemos na etapa 1. Normalmente, o SQL `SELECT` instruções usam `JOIN` s para trazer de volta os dados de uma ou mais tabelas de pesquisa para fins de exibição (como recuperar o `Categories` tabela s `CategoryName` campo ao exibir informações sobre o produto). Ao mesmo tempo, poderíamos desejar permitir que o usuário editar, atualizar ou inserir dados na tabela principal (`Products`, nesse caso).

Enquanto o `INSERT`, `UPDATE`, e `DELETE` instruções podem ser inseridos manualmente, considere a seguinte dica de economia de tempo. Configurar inicialmente o SqlDataSource para que ele extrai dados apenas a partir de `Products` tabela. Use as configurar fonte de dados Assistente s especificar colunas para uma tabela ou exibição de tela para que você pode gerar automaticamente o `INSERT`, `UPDATE`, e `DELETE` instruções. Em seguida, após concluir o assistente, optar por configurar SelectQuery na janela de propriedades (ou, Alternativamente, volte para a opção de procedimento armazenado ou o Assistente para configurar a fonte de dados, mas use especificar uma instrução SQL personalizada). Em seguida, atualize o `SELECT` instrução para incluir o `JOIN` sintaxe. Essa técnica oferece os benefícios de economia de tempo das instruções SQL geradas automaticamente e permite mais personalizada `SELECT` instrução.

Outra limitação de gerar automaticamente o `INSERT`, `UPDATE`, e `DELETE` instruções que são as colunas no `INSERT` e `UPDATE` instruções baseiam-se as colunas retornadas pelo `SELECT` instrução. É necessário atualizar ou inserir os campos de mais ou menos, no entanto. Por exemplo, no exemplo a partir da etapa 2, talvez desejemos tem o `UnitPrice` BoundField ser somente leitura. Nesse caso, ele deveria aparece no `UpdateCommand`. Ou, talvez queira definir o valor de um campo de tabela que não aparece em GridView. Por exemplo, ao adicionar um novo registro devemos o `QuantityPerUnit` valor definido como um TODO.

Se essas personalizações são necessárias, você precisa torná-los manualmente, usando a janela Propriedades, especifique uma instrução SQL personalizada ou opção de procedimento armazenado no assistente, ou por meio de sintaxe declarativa.

> [!NOTE]
> Ao adicionar parâmetros que não têm campos correspondentes nos dados de controle de Web, tenha em mente que esses valores de parâmetros precisam ser atribuídos valores de alguma maneira. Esses valores podem ser: embutida diretamente no `InsertCommand` ou `UpdateCommand`; podem vir de alguma origem predefinida (o querystring estado da sessão, controles da Web na página e assim por diante); ou pode ser atribuído por meio de programação, como visto no tutorial anterior.


## <a name="summary"></a>Resumo

Em ordem para os dados de que controles da Web para utilizar suas inserindo internas, editar e excluir recursos, o controle de fonte de dados que estejam associados ao oferecer essa funcionalidade. Para o SqlDataSource, isso significa que `INSERT`, `UPDATE`, e `DELETE` instruções SQL devem ser atribuídas para o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades. Essas propriedades e as coleções de parâmetros correspondentes, podem ser adicionadas manualmente ou geradas automaticamente por meio do Assistente Configurar fonte de dados. Neste tutorial, examinamos ambas as técnicas.

Examinamos usando simultaneidade otimista com ObjectDataSource no [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial. O controle SqlDataSource também fornece suporte à simultaneidade otimista. Conforme observado na etapa 2, ao gerar automaticamente o `INSERT`, `UPDATE`, e `DELETE` instruções, o assistente oferece uma opção de simultaneidade otimista de uso. Como você verá o seguinte tutorial, usando a simultaneidade otimista com SqlDataSource modifica o `WHERE` cláusulas o `UPDATE` e `DELETE` instruções para garantir que os valores para as outras colunas ainda t alterada desde que os dados estavam último exibido na página.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Próximo](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
