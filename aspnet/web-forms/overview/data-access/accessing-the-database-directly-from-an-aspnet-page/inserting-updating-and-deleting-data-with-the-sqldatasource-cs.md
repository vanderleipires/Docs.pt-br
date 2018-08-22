---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Inserindo, atualizando e excluindo dados com o SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, aprendemos como o controle ObjectDataSource permitido para inserção, atualização e exclusão de dados. O controle SqlDataSource dá suporte a t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c52fcf746d80899d7ea568c8110c4dfa610224c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825494"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Inserindo, atualizando e excluindo dados com o SqlDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) ou [baixar PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Nos tutoriais anteriores, aprendemos como o controle ObjectDataSource permitido para inserção, atualização e exclusão de dados. O controle SqlDataSource suporta as mesmas operações, mas a abordagem é diferente, e este tutorial mostra como configurar o SqlDataSource para inserir, atualizar e excluir dados.


## <a name="introduction"></a>Introdução

Conforme discutido em [uma visão geral de inserção, atualização e exclusão de](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), o controle GridView fornece atualização interna e excluir recursos, enquanto os controles DetailsView e FormView incluem inserir suportam juntamente com edição e exclusão de funcionalidade. Esses recursos de modificação de dados podem ser conectados diretamente a um controle de fonte de dados sem uma linha de código que precisam ser gravados. [Uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) examinado usando o ObjectDataSource para facilitar a inserção, atualização e exclusão com os controles GridView, DetailsView e FormView. Como alternativa, o SqlDataSource pode ser usado no lugar do ObjectDataSource.

Lembre-se de que para dar suporte à inserção, atualização e exclusão, com o ObjectDataSource é necessário para especificar os métodos de camada de objeto para chamar para executar a inserção, atualização ou exclusão ação. Com o SqlDataSource, precisamos fornecer `INSERT`, `UPDATE`, e `DELETE` SQL instruções (ou procedimentos armazenados) para executar. Como veremos neste tutorial, essas instruções podem ser criadas manualmente ou podem ser automaticamente geradas pelo Assistente Configurar fonte de dados SqlDataSource s.

> [!NOTE]
> Desde que criamos ve já discutidos a inserir, editar e excluir recursos de GridView, DetailsView e FormView controla, este tutorial se concentrará em como configurar o controle SqlDataSource para dar suporte a essas operações. Se você precisar recordar implementando esses recursos no retorno de GridView, DetailsView e FormView, os tutoriais de edição, inserção e exclusão de dados, começando com [uma visão geral de inserção, atualização e exclusão de](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Etapa 1: Especificar`INSERT`,`UPDATE`, e`DELETE`instruções

Como podemos ve visto nos últimos dois tutoriais, para recuperar dados de um controle SqlDataSource, precisamos definir duas propriedades:

1. `ConnectionString`, que especifica qual banco de dados para enviar a consulta, e
2. `SelectCommand`, que especifica a instrução de SQL ad hoc ou o nome do procedimento armazenado a ser executada para retornar os resultados.

Para `SelectCommand` valores com parâmetros, o parâmetro de valores são especificados por meio de s SqlDataSource `SelectParameters` coleta e pode incluir valores embutidos, valores de fonte de parâmetros comuns (querystring campos, variáveis de sessão, valores de controle da Web, e assim por diante), ou podem ser atribuídos de forma programática. Quando o controle s SqlDataSource `Select()` método é invocado por meio de programação ou automaticamente de um controle da Web de dados é estabelecida uma conexão ao banco de dados, os valores de parâmetro são atribuídos à consulta e o comando é shuttled desativado para o banco de dados. Os resultados são retornados como um conjunto de dados ou um DataReader, dependendo do valor do controle s `DataSourceMode` propriedade.

Além de selecionar os dados, o controle SqlDataSource pode ser usado para inserir, atualizar e excluir dados por meio do fornecimento `INSERT`, `UPDATE`, e `DELETE` instruções SQL da mesma forma. Basta atribuir o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades do `INSERT`, `UPDATE`, e `DELETE` instruções SQL para executar. Se as instruções têm parâmetros (como mais sempre serão), incluí-los de `InsertParameters`, `UpdateParameters`, e `DeleteParameters` coleções.

Uma vez um `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` valor tiver sido especificado, a opção de habilitar a inserção, habilitar edição ou habilitar exclusão nos dados correspondentes marca inteligente do controle s Web estará disponível. Para ilustrar isso, let s tomar um exemplo do `Querying.aspx` página que criamos na [consultando dados com o controle SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) tutorial e incrementar o que ele inclua excluir recursos.

Comece abrindo o `InsertUpdateDelete.aspx` e `Querying.aspx` páginas do `SqlDataSource` pasta. No Designer na `Querying.aspx` , selecione o SqlDataSource e GridView do primeiro exemplo (o `ProductsDataSource` e `GridView1` controles). Depois de selecionar os dois controles, vá para o menu Editar e escolha Copiar (ou apenas pressione Ctrl + C). Em seguida, vá para o Designer de `InsertUpdateDelete.aspx` e colar nos controles. Depois de ter de mudar os dois controles para `InsertUpdateDelete.aspx`, testar a página em um navegador. Você deve ver os valores da `ProductID`, `ProductName`, e `UnitPrice` colunas para todos os registros no `Products` tabela de banco de dados.


[![Todos os produtos são listados, ordenados por ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: todos os produtos são listados, ordenados por `ProductID` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Adicionando o s SqlDataSource`DeleteCommand`e`DeleteParameters`propriedades

Neste ponto, temos um SqlDataSource que simplesmente retorna todos os registros da `Products` tabela e um GridView que processa esses dados. Nosso objetivo é estender esse exemplo para permitir que o usuário exclua produtos por meio de GridView. Para fazer isso, precisamos especificar valores para o controle SqlDataSource s `DeleteCommand` e `DeleteParameters` propriedades e, em seguida, configure o GridView para oferecer suporte a exclusão.

O `DeleteCommand` e `DeleteParameters` propriedades podem ser especificadas de várias maneiras:

- Através de sintaxe declarativa
- Na janela Propriedades no Designer
- Da tela de procedimento armazenado no Assistente Configurar fonte de dados ou especificar uma instrução SQL personalizada
- Por meio do botão Avançado nas especificar colunas de uma tabela de tela de visualização no Assistente Configurar fonte de dados, que irá gerar automaticamente, na verdade, o `DELETE` coleção de instruções e parâmetros SQL usadas no `DeleteCommand` e `DeleteParameters` Propriedades

Vamos examinar como ter automaticamente o `DELETE` demonstrativo criado na etapa 2. Por enquanto, deixe s use a janela Propriedades no Designer, embora o Assistente Configurar fonte de dados ou a opção de sintaxe declarativa funcionaria muito bem.

No Designer ou na `InsertUpdateDelete.aspx`, clique no `ProductsDataSource` SqlDataSource e, em seguida, abrir a janela de propriedades (no menu Exibir, escolha a janela Propriedades ou simplesmente pressione F4). Selecione a propriedade DeleteQuery, que abre um conjunto de reticências.


![Selecione a propriedade DeleteQuery na janela Propriedades](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Figura 2**: selecione a propriedade DeleteQuery na janela Propriedades


> [!NOTE]
> O SqlDataSource tem uma propriedade DeleteQuery. Em vez disso, DeleteQuery é uma combinação da `DeleteCommand` e `DeleteParameters` propriedades e só será listado na janela Propriedades quando exibir a janela por meio do Designer. Se você estiver observando a janela de propriedades na exibição da fonte, você encontrará o `DeleteCommand` propriedade em vez disso.


Clique nas reticências na propriedade DeleteQuery para abrir a caixa de diálogo Editor de comando e parâmetro caixa (veja a Figura 3). Nessa caixa de diálogo, você pode especificar o `DELETE` instrução SQL e especifique os parâmetros. Insira a seguinte consulta para o `DELETE` caixa de texto de comando (seja manualmente ou usando o construtor de consultas, se você preferir):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Em seguida, clique no botão Atualizar parâmetros para adicionar o `@ProductID` parâmetro à lista de parâmetros abaixo.


[![Selecione a propriedade DeleteQuery na janela Propriedades](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: selecione a propriedade DeleteQuery na janela Propriedades ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Fazer *não* fornecer um valor para esse parâmetro (deixe seu parâmetro de origem em nenhum). Depois de adicionar suporte a exclusão para o GridView, GridView fornecerá automaticamente esse valor de parâmetro, usando o valor de seu `DataKeys` coleção para a linha cuja exclusão foi clicada.

> [!NOTE]
> O nome do parâmetro usado na `DELETE` consulta *deve* ser o mesmo que o nome da `DataKeyNames` valor o GridView, DetailsView ou FormView. Ou seja, o parâmetro na `DELETE` instrução propositadamente é denominada `@ProductID` (em vez de, digamos, `@ID`), pois é o nome da coluna de chave primária da tabela de produtos (e, portanto, ao valor DataKeyNames no GridView) `ProductID`.


Se o nome do parâmetro e `DataKeyNames` ainda não correspondência de valor de t, GridView não pode atribuir automaticamente o parâmetro o valor da `DataKeys` coleção.

Depois de inserir as informações relacionadas a excluir na caixa de diálogo Editor de comando e parâmetro clique Okey e vá para a exibição da fonte para examinar a marcação declarativa resultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Observe a adição do `DeleteCommand` propriedade, bem como a `<DeleteParameters>` seção e o objeto de parâmetro nomeado `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurando o GridView para exclusão

Com o `DeleteCommand` propriedade adicionada, a marca inteligente do GridView s agora contém a opção de habilitar a exclusão. Vá em frente e marque essa caixa de seleção. Conforme discutido em [uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), isso faz com que o GridView adicionar um CommandField com seus `ShowDeleteButton` propriedade definida como `true`. Como a Figura 4 mostra, quando a página for visitada por meio de um navegador em um botão Excluir é incluído. Essa saída de página de teste, excluindo alguns produtos.


[![Cada linha de GridView agora inclui um botão Excluir](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: cada linha de GridView agora inclui um botão Delete ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Ao clicar em um botão de exclusão, ocorre um postback, GridView atribui a `ProductID` o valor do parâmetro do `DataKeys` valor de coleção para a linha cujo botão Excluir foi clicado e invoca o s SqlDataSource `Delete()` método. O controle SqlDataSource, em seguida, conecta-se ao banco de dados e executa o `DELETE` instrução. O GridView, em seguida, associa novamente ao SqlDataSource, obter de volta e exibindo o conjunto atual de produtos (que não inclui mais o registro excluído apenas).

> [!NOTE]
> Uma vez que usa GridView seu `DataKeys` coleção para preencher os parâmetros do SqlDataSource, ele s vital que s o GridView `DataKeyNames` propriedade ser definida como as colunas que constituem a chave primária e que o s SqlDataSource `SelectCommand` retorna Essas colunas. Além disso, ele importante que o parâmetro de nome em s SqlDataSource `DeleteCommand` é definido como `@ProductID`. Se o `DataKeyNames` não está definida ou o parâmetro não é chamado `@ProductsID`, clicando no botão Excluir fará com que um postback, mas t ganha exclui qualquer registro.


Figura 5 ilustra essa interação graficamente. Voltar para o [examinando os eventos associados inserindo, atualizando e excluindo](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial para uma discussão mais detalhada sobre a cadeia de eventos associados à inserção, atualização e exclusão de um controle da Web de dados.


![Clicando no botão Excluir no GridView invoca o método do SqlDataSource s Delete)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Figura 5**: clicando no botão Excluir no GridView invoca o s SqlDataSource `Delete()` método


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Etapa 2: Gerar automaticamente a`INSERT`,`UPDATE`, e`DELETE`instruções

Como etapa examinado, de 1 `INSERT`, `UPDATE`, e `DELETE` instruções SQL podem ser especificadas por meio da janela Properties ou a sintaxe declarativa do controle s. No entanto, essa abordagem requer que podemos manualmente gravar as instruções SQL manualmente, que pode ser monótonas e propensas a erro. Felizmente, o Assistente Configurar fonte de dados fornece uma opção para que o `INSERT`, `UPDATE`, e `DELETE` instruções geradas automaticamente ao usar as especificar colunas de uma tabela de tela de exibição.

Deixe o s explorar essa opção de geração automática. Adicionar um DetailsView para o Designer no `InsertUpdateDelete.aspx` e defina sua `ID` propriedade `ManageProducts`. Em seguida, da DetailsView s marca inteligente, optar por criar uma nova fonte de dados e criar um SqlDataSource chamado `ManageProductsDataSource`.


[![Criar um novo SqlDataSource chamado ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Figura 6**: criar um novo SqlDataSource nomeado `ManageProductsDataSource` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


No Assistente Configurar fonte de dados, optar por usar o `NORTHWINDConnectionString` conexão da cadeia de caracteres e clique em Avançar. De configurar a tela de instrução Select, deixe as especificar colunas usando um botão de opção de tabela ou exibição selecionado e escolha o `Products` tabela na lista suspensa. Selecione o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas da lista de caixa de seleção.


[![Usando a tabela de produtos, retornar o ProductID, ProductName, UnitPrice e colunas descontinuadas](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Figura 7**: usando o `Products` da tabela, retornar o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Para gerar automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções com base na tabela selecionada e colunas, clique no botão Avançado e verifique a gerar `INSERT`, `UPDATE`, e `DELETE` caixa de seleção de instruções.


![Marque as caixa de seleção de instruções Generate INSERT, UPDATE e DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Figura 8**: Verifique Generate `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção


Generate `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção só será selecionável; se a tabela selecionada tem uma chave primária e a coluna de chave primária (ou colunas) são incluídas na lista de colunas retornadas. A caixa de seleção de simultaneidade otimista de uso, que se torna selecionável uma vez Generate `INSERT`, `UPDATE`, e `DELETE` caixa de seleção de instruções foram verificados, irá aumentar os `WHERE` cláusulas em resultante `UPDATE` e `DELETE` instruções para fornecer controle de simultaneidade otimista. Por enquanto, deixe essa caixa de seleção desmarcada; Vamos examinar a simultaneidade otimista com o controle SqlDataSource no próximo tutorial.

Depois de verificar Generate `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção, clique em Okey para retornar à tela Configurar a instrução Select, em seguida, clique em Avançar e em seguida Concluir para concluir o Assistente Configurar fonte de dados. Após a conclusão do assistente, o Visual Studio adicionará BoundFields a DetailsView para o `ProductID`, `ProductName`, e `UnitPrice` colunas e um CheckBoxField para o `Discontinued` coluna. Na DetailsView s marca inteligente, verifique a opção habilitar paginação para que o usuário visitar esta página pode percorrer os produtos. Limpar também o s DetailsView `Width` e `Height` propriedades.

Observe que a marca inteligente tem as opções de inserção de habilitar, habilitar edição e habilitar exclusão disponíveis. Isso ocorre porque o SqlDataSource contém valores para seus `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, como mostra a seguinte sintaxe declarativa:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Observe como o controle SqlDataSource teve valores definidos automaticamente para seus `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades. O conjunto de colunas referenciadas na `InsertCommand` e `UpdateCommand` propriedades baseadas no `SELECT` instrução. Ou seja, em vez de precisar *cada* coluna de produtos no `InsertCommand` e `UpdateCommand`, existem apenas as colunas especificadas no `SelectCommand` (menos `ProductID`, que é omitido porque ele s um [ `IDENTITY` coluna](http://www.sqlteam.com/item.asp?ItemID=102), cujo valor não pode ser alterado quando editado e qual é atribuído automaticamente ao inserir). Além disso, para cada parâmetro na `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades não existem parâmetros correspondentes no `InsertParameters`, `UpdateParameters`, e `DeleteParameters` coleções.

Para ativar os recursos de modificação de dados de s DetailsView, verifique a habilitar a inserção, habilitar edição e habilitar exclusão opções nessa marca inteligente. Isso adiciona um CommandField com seu `ShowInsertButton`, `ShowEditButton`, e `ShowDeleteButton` propriedades definidas como `true`.

Visite a página em um navegador e observe a edição, exclusão e novos botões incluídos em DetailsView. Clique no botão Editar transforma DetailsView em modo de edição, que exibe cada BoundField cujos `ReadOnly` estiver definida como `false` (o padrão) como uma caixa de texto e o CheckBoxField como uma caixa de seleção.


[![O s DetailsView padrão de Interface de edição](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Figura 9**: s DetailsView a Interface de edição padrão ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Da mesma forma, você pode excluir o produto selecionado no momento ou adicionar um novo produto no sistema. Uma vez que o `InsertCommand` instrução só funciona com o `ProductName`, `UnitPrice`, e `Discontinued` colunas, as colunas têm tanto `NULL` ou seu valor padrão atribuído pelo banco de dados em insert. Assim como ocorre com o ObjectDataSource, se o `InsertCommand` não tem nenhuma tabela de banco de dados permitem colunas que não t `NULL` s e don t tem um valor padrão, ocorrerá um erro SQL ao tentar executar o `INSERT` instrução.

> [!NOTE]
> O s DetailsView inserindo e editando as interfaces não têm qualquer tipo de personalização ou validação. Para adicionar controles de validação ou personalizar as interfaces, você precisa converter o BoundFields TemplateFields. Consulte a [adicionando controles de validação para a edição e inserção de Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutoriais para obter mais informações.


Além disso, tenha em mente que, para atualizar e excluir, DetailsView usa o produto atual s `DataKey` valor, que estará presente somente se o `DataKeyNames` propriedade é configurada. Se a edição ou exclusão parece não ter nenhum efeito, certifique-se de que o `DataKeyNames` propriedade está definida.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitações de geração automática de instruções SQL

Desde o gere `INSERT`, `UPDATE`, e `DELETE` opção instruções só está disponível quando escolher colunas de uma tabela, para consultas mais complexas, você terá que escrever seu próprio `INSERT`, `UPDATE`, e `DELETE` instruções de como fizemos na etapa 1. Normalmente, o SQL `SELECT` instruções usam `JOIN` s para trazer de volta os dados de uma ou mais tabelas de pesquisa para fins de exibição (como trazer de volta a `Categories` tabela s `CategoryName` campo ao exibir as informações de produto). Ao mesmo tempo, queremos permitir que o usuário editar, atualizar ou inserir dados na tabela core (`Products`, nesse caso).

Enquanto o `INSERT`, `UPDATE`, e `DELETE` instruções podem ser inseridos manualmente, considere a seguinte dica de economia de tempo. Configurar inicialmente o SqlDataSource, de modo que ele volta extrai dados apenas do `Products` tabela. Use as configurar fonte de dados Assistente s especificar colunas para uma tela de tabela ou exibição para que você possa gerar automaticamente a `INSERT`, `UPDATE`, e `DELETE` instruções. Em seguida, depois de concluir o assistente, optar por configurar o SelectQuery na janela Propriedades (ou, Alternativamente, volte para a opção de procedimento armazenado ou o Assistente Configurar fonte de dados, mas use a especificar uma instrução SQL personalizada). Em seguida, atualize o `SELECT` instrução para incluir o `JOIN` sintaxe. Essa técnica oferece os benefícios de economia de tempo das instruções SQL geradas automaticamente e permite mais personalizada `SELECT` instrução.

Outra limitação da geração automática do `INSERT`, `UPDATE`, e `DELETE` instruções que são as colunas no `INSERT` e `UPDATE` instruções são baseadas nas colunas retornadas pelo `SELECT` instrução. Talvez seja necessário atualizar ou inserir campos mais ou menos, no entanto. Por exemplo, no exemplo a partir da etapa 2, talvez desejemos têm o `UnitPrice` BoundField ser somente leitura. Nesse caso, ele t não deve aparecer no `UpdateCommand`. Ou talvez queiramos definir o valor de um campo da tabela que não aparece no GridView. Por exemplo, ao adicionar um novo registro pode queremos o `QuantityPerUnit` valor definido como um TODO.

Se tais personalizações são necessárias, você precisa torná-los manualmente, por meio da janela Propriedades, a especificar uma instrução SQL personalizada ou a opção de procedimento armazenado no assistente, ou por meio da sintaxe declarativa.

> [!NOTE]
> Ao adicionar parâmetros que não têm campos correspondentes nos dados de controle de Web, tenha em mente que esses valores de parâmetros precisam ser atribuídos valores de alguma maneira. Esses valores podem ser: embutido em código diretamente na `InsertCommand` ou `UpdateCommand`; podem vir de alguma origem predefinida (a querystring, estado de sessão, controles da Web na página e assim por diante); ou podem ser atribuídos de forma programática, como vimos no tutorial anterior.


## <a name="summary"></a>Resumo

Para que os dados de controles da Web para utilizar seus internos de inserção, editar e excluir recursos, o controle de fonte de dados que estejam associados ao deve oferecer essa funcionalidade. Para o SqlDataSource, isso significa que `INSERT`, `UPDATE`, e `DELETE` instruções SQL devem ser atribuídas para o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades. Essas propriedades e as coleções de parâmetros correspondentes, podem ser adicionadas manualmente ou geradas automaticamente pelo Assistente Configurar fonte de dados. Neste tutorial, examinamos as duas técnicas.

Examinamos usando a simultaneidade otimista com o ObjectDataSource na [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) tutorial. O controle SqlDataSource também fornece suporte à simultaneidade otimista. Conforme observado na etapa 2, ao gerar automaticamente o `INSERT`, `UPDATE`, e `DELETE` instruções, o assistente oferece uma opção de simultaneidade otimista de uso. Como veremos no próximo tutorial, usando a simultaneidade otimista com o SqlDataSource modifica o `WHERE` cláusulas na `UPDATE` e `DELETE` instruções para garantir que os valores para as outras colunas foram t alterada desde que os dados estavam último exibido na página.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Próximo](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
