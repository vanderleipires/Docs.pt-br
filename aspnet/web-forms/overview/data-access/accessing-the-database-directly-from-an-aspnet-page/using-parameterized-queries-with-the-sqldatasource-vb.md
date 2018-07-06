---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Usando consultas parametrizadas com o SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, podemos continuar a nossa aparência para o controle SqlDataSource e saiba como definir consultas parametrizadas. Os parâmetros podem ser especificados ambos os i...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: fd4964bbdfe7662513025086a6cc3a59a6d63782
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813809"
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Usando consultas parametrizadas com o SqlDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) ou [baixar PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> Neste tutorial, podemos continuar a nossa aparência para o controle SqlDataSource e saiba como definir consultas parametrizadas. Os parâmetros podem ser especificados de forma declarativa e por meio de programação e podem ser extraídos de um número de locais como querystring, estado sessão, outros controles e muito mais.


## <a name="introduction"></a>Introdução

No tutorial anterior, vimos como usar o controle SqlDataSource para recuperar dados diretamente de um banco de dados. Usando o Assistente Configurar fonte de dados, poderíamos escolher o banco de dados e, em seguida,: selecione as colunas para retornar de uma tabela ou exibição; Insira uma instrução SQL personalizada; ou use um procedimento armazenado. Se selecionar colunas de uma tabela ou exibição ou a inserção de uma instrução SQL personalizada, o SqlDataSource controle s `SelectCommand` o SQL ad hoc resultante é atribuído a propriedade `SELECT` instrução e é isso `SELECT` instrução é executada quando o S SqlDataSource `Select()` método é invocado (programaticamente ou automaticamente de um controle da Web de dados).

O SQL `SELECT` instruções usadas no tutorial s demonstrações anteriores não tinha `WHERE` cláusulas. Em um `SELECT` instrução, o `WHERE` cláusula pode ser usada para limitar os resultados retornados. Por exemplo, para exibir os nomes dos produtos que custam mais de US $50,00, podemos pode usar a consulta a seguir:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

Normalmente, os valores usados em um `WHERE` cláusula são determinar por meio de alguma fonte externa, como um valor de cadeia de consulta, uma variável de sessão ou a entrada do usuário de um controle da Web na página. O ideal é que essas entradas são especificadas por meio do uso do *parâmetros*. Com o Microsoft SQL Server, os parâmetros são indicados usando `@parameterName`, como em:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

O SqlDataSource dá suporte a consultas parametrizadas, ambas para `SELECT` instruções e `INSERT`, `UPDATE`, e `DELETE` instruções. Além disso, os valores de parâmetro podem ser automaticamente extraídos de uma variedade de fontes de querystring, estado de sessão, os controles na página e assim por diante ou podem ser atribuídos programaticamente. Neste tutorial, veremos como definir consultas parametrizadas, bem como para especificar o parâmetro valores declarativamente e programaticamente.

> [!NOTE]
> No tutorial anterior, em comparação com o ObjectDataSource que já foi nossa ferramenta de escolha sobre os 46 primeiro tutoriais com o SqlDataSource, observando as semelhanças conceituais. Essas semelhanças também se estendem aos parâmetros. Os parâmetros do ObjectDataSource s mapeados para os parâmetros de entrada para os métodos na camada de lógica de negócios. Com o SqlDataSource, os parâmetros são definidos diretamente na consulta SQL. Ambos os controles têm coleções de parâmetros para seus `Select()`, `Insert()`, `Update()`, e `Delete()` métodos e ambos podem ter esses valores de parâmetros preenchidos a partir de fontes previamente definidos (valores de cadeia de consulta, as variáveis de sessão e assim por diante ) ou atribuídos de forma programática.


## <a name="creating-a-parameterized-query"></a>Criando uma consulta parametrizada

O Assistente para configurar a fonte de dados do SqlDataSource controle s oferece três vias para definir o comando a ser executada para recuperar os registros de banco de dados:

- Selecionando colunas de uma tabela ou exibição existente,
- Digitando uma instrução SQL personalizada, ou
- Escolhendo um procedimento armazenado

Ao escolher colunas de uma tabela existente ou exibição, os parâmetros para o `WHERE` cláusula deve ser especificada por meio de adicionar `WHERE` caixa de diálogo cláusula. Ao criar uma instrução SQL personalizada, no entanto, você pode inserir os parâmetros diretamente para o `WHERE` cláusula (usando `@parameterName` para indicar cada parâmetro). Um [procedimento armazenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) consiste em uma ou mais instruções SQL, e essas instruções podem ser parametrizadas. Os parâmetros usados em instruções SQL, no entanto, devem ser passados em como parâmetros de entrada para o procedimento armazenado.

Como criar uma consulta parametrizada depende de como o s SqlDataSource `SelectCommand` é s especificado, vamos dar uma olhada em todas as três abordagens. Para começar, abra o `ParameterizedQueries.aspx` página o `SqlDataSource` pasta, arraste um controle SqlDataSource da caixa de ferramentas para o Designer e defina seu `ID` para `Products25BucksAndUnderDataSource`. Em seguida, clique no link de configurar fonte de dados a partir da marca inteligente do controle s. Selecione o banco de dados para usar (`NORTHWINDConnectionString`) e clique em Avançar.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Etapa 1: Adicionando um`WHERE`cláusula ao separar as colunas de uma tabela ou exibição

Ao selecionar os dados a serem retornados do banco de dados com o controle SqlDataSource, o Assistente Configurar fonte de dados nos permite simplesmente escolhe as colunas para retornar de uma tabela existente ou exibir (veja a Figura 1). Fazendo isso automaticamente cria um SQL `SELECT` instrução, que é o que é enviado ao banco de dados quando o s SqlDataSource `Select()` método é invocado. Como fizemos no tutorial anterior, selecione a tabela de produtos na lista suspensa e verificar a `ProductID`, `ProductName`, e `UnitPrice` colunas.


[![Selecione as colunas para retornar de uma tabela ou exibição](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: selecione as colunas para o retorno de uma tabela ou exibição ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


Para incluir um `WHERE` cláusula em de `SELECT` instrução, clique no `WHERE` botão, o que abrirá a adicionar `WHERE` caixa de diálogo de cláusula (veja a Figura 2). Para adicionar um parâmetro para limitar os resultados retornados pelo `SELECT` da consulta, primeiro escolha a coluna para filtrar os dados por. Em seguida, escolha o operador a ser usado para filtragem (=, &lt;, &lt;=, &gt;e assim por diante). Por fim, escolha a origem do valor do parâmetro de s, como do estado de sessão ou de cadeia de consulta. Depois de configurar o parâmetro, clique no botão Adicionar para incluí-lo no `SELECT` consulta.

Neste exemplo, let s retornar apenas os resultados onde o `UnitPrice` valor é menor ou igual a $25,00. Portanto, escolha `UnitPrice` na lista suspensa coluna e &lt;= da lista suspensa do operador. Ao usar um valor de parâmetro embutido em código (por exemplo, $25,00) ou se o valor do parâmetro deve ser especificada programaticamente, selecione nenhum na lista suspensa origem. Em seguida, insira o valor do parâmetro embutido em código na caixa de texto valor 25,00 e concluir o processo clicando no botão Adicionar.


[![Limitar os resultados retornados do Adicione WHERE caixa de diálogo cláusula](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Figura 2**: limitar os resultados retornados de adicionar `WHERE` caixa de diálogo cláusula ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


Depois de adicionar o parâmetro, clique em Okey para retornar para o Assistente Configurar fonte de dados. O `SELECT` instrução na parte inferior do assistente agora deve incluir uma `WHERE` cláusula com um parâmetro chamado `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Se você especificar várias condições na `WHERE` cláusula de adicionar `WHERE` caixa de diálogo cláusula, o assistente une-as com o `AND` operador. Se você precisa incluir um `OR` no `WHERE` cláusula (como `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) e em seguida, você precisa criar o `SELECT` instrução por meio da tela de instrução SQL personalizada.


Concluir a configuração o SqlDataSource (clique em seguida, em seguida, concluir) e, em seguida, inspecione a marcação declarativa de s SqlDataSource. A marcação agora inclui um `<SelectParameters>` coleta, que esclarece as fontes para os parâmetros no `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Quando o s SqlDataSource `Select()` método é invocado, o `UnitPrice` o valor do parâmetro (25,00) é aplicado ao `@UnitPrice` parâmetro no `SelectCommand` antes de serem enviados ao banco de dados. O resultado líquido é que somente esses produtos retornados de menor ou igual a $25,00 o `Products` tabela. Para confirmar isso, adicione um controle GridView à página, associá-lo a essa fonte de dados e, em seguida, exiba a página por meio de um navegador. Você deve ver apenas os produtos listados menor ou igual a $25,00, enquanto confirma a Figura 3.


[![São exibidas apenas aqueles produtos menor que ou igual a $25,00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Figura 3**: apenas aqueles produtos menor que ou igual a $25,00 são exibidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Etapa 2: Adicionando parâmetros para uma instrução SQL personalizada

Ao adicionar uma instrução SQL personalizada que você pode inserir o `WHERE` cláusula explicitamente ou especifique um valor na célula do construtor de consulta de filtro. Para demonstrar isso, deixe s exibir apenas esses produtos em um GridView cujos preços são menores que um determinado limite. Comece adicionando uma caixa de texto o `ParameterizedQueries.aspx` página para obter esse valor de limite do usuário. Definir o s TextBox `ID` propriedade para `MaxPrice`. Adicione um controle da Web de botão e defina seu `Text` propriedade aos produtos de correspondência de exibição.

Em seguida, arraste um controle GridView à página e na marca inteligente de optar por criar um novo SqlDataSource chamado `ProductsFilteredByPriceDataSource`. No Assistente Configurar fonte de dados, continue para especificar uma instrução SQL personalizada ou tela de procedimento armazenado (veja a Figura 4) e digite a seguinte consulta:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Depois de inserir a consulta (manualmente ou por meio do construtor de consultas), clique em Avançar.


[![Retornar apenas os produtos menor ou igual a um valor de parâmetro](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Figura 4**: retorna apenas aqueles produtos menor que ou igual a um valor de parâmetro ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


Uma vez que a consulta inclui parâmetros, a próxima tela do assistente nos solicita a origem dos valores de parâmetros. Escolha o controle da lista de lista suspensa de fonte de parâmetro e `MaxPrice` (o controle de caixa de texto s `ID` valor) na lista suspensa ControlID. Você também pode inserir um valor padrão opcional para usar no caso em que o usuário não tiver inserido qualquer texto no `MaxPrice` caixa de texto. Por enquanto, não insira um valor padrão.


[![O propriedade Text do s MaxPrice TextBox é usado como a origem do parâmetro](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Figura 5**: O `MaxPrice` s da caixa de texto `Text` propriedade é usada como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


Conclua o Assistente Configurar fonte de dados clicando em Avançar e em seguida concluir. A marcação declarativa para o GridView, TextBox, Button e SqlDataSource da seguinte maneira:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Observe que o parâmetro dentro da s SqlDataSource `<SelectParameters>` seção é uma `ControlParameter`, que inclui as propriedades adicionais, como `ControlID` e `PropertyName`. Quando o s SqlDataSource `Select()` método é invocado, o `ControlParameter` captura o valor da propriedade especificada do controle de Web e o atribui ao parâmetro correspondente no `SelectCommand`. Neste exemplo, o `MaxPrice` s propriedade Text é usado como o `@MaxPrice` valor do parâmetro.

Reserve um minuto para exibir esta página por meio de um navegador. Quando o primeiro visitando a página ou sempre que o `MaxPrice` caixa de texto não tiver um valor que não há registros são exibidos em um GridView.


[![Registros não são que exibidos quando o MaxPrice caixa de texto está vazio](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Figura 6**: os registros não são exibidos quando o `MaxPrice` caixa de texto está vazia ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


O motivo pelo qual não há produtos são mostrados ocorre porque, por padrão, uma cadeia de caracteres vazia para um valor de parâmetro é convertida em um banco de dados `NULL` valor. Desde a comparação de `[UnitPrice] <= NULL` sempre é avaliada como False, nenhum resultado será retornado.

Insira um valor na caixa de texto, como 5.00 e clique no botão de produtos de correspondência de exibição. No postback, SqlDataSource informa que o GridView que uma das suas fontes de parâmetro foi alterado. Consequentemente, o GridView associa novamente para o SqlDataSource, exibindo os produtos menor ou igual a US $5,00.


[![Produtos de menor que ou igual a US $5,00 são exibidos](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Figura 7**: produtos menor que ou igual a US $5,00 são exibidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Inicialmente, exibindo todos os produtos

Em vez de não exibir nenhum produto quando a página é carregada pela primeira vez, podemos querer exibir *todos os* produtos. Uma maneira de listar todos os produtos sempre que o `MaxPrice` caixa de texto está vazia é definir o valor padrão do parâmetro s como algum valor incrivelmente alta, como 1000000, desde que ele s improvável que a Northwind Traders fará nunca ter um inventário cujo preço unitário excede US $1.000.000. No entanto, essa abordagem é falta e pode não funcionar em outras situações.

Nos tutoriais anteriores - [parâmetros declarativos](../basic-reporting/declarative-parameters-vb.md) e [filtragem de mestre/detalhes com uma DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) nós foram nos deparamos com um problema semelhante. Lá, nossa solução foi colocar essa lógica na camada de lógica de negócios. Especificamente, a BLL examinado o valor de entrada e, se ele tiver sido `NULL` ou alguns reservados de valor, a chamada foi roteada para o método DAL que todos os registros retornados. Se o valor de entrada for um valor de filtragem normal, foi feita uma chamada ao método DAL que executou uma instrução SQL que usado com um parâmetros `WHERE` cláusula com o valor fornecido.

Infelizmente, podemos ignorar a arquitetura, ao usar o SqlDataSource. Em vez disso, é preciso personalizar a instrução SQL para inteligentemente pegar todos os registros, se o `@MaximumPrice` parâmetro é `NULL` ou algum valor reservado. Para este exercício, let s tem ele então que, se o `@MaximumPrice` parâmetro for igual ao `-1.0`, em seguida, *todos os* os registros serão retornados (`-1.0` funciona como um valor reservado como nenhum produto pode ter um negativo `UnitPrice`valor). Para fazer isso, podemos usar a seguinte instrução SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Isso `WHERE` cláusula retorna *todas as* registra se o `@MaximumPrice` é igual ao parâmetro `-1.0`. Se o valor do parâmetro não for `-1.0`, apenas aqueles produtos cujo `UnitPrice` é menor que ou igual ao `@MaximumPrice` valor de parâmetro são retornados. Definindo o valor padrão a `@MaximumPrice` parâmetro para `-1.0`, o primeiro carregamento de página (ou sempre que o `MaxPrice` caixa de texto está vazia), `@MaximumPrice` terá um valor de `-1.0` e todos os produtos serão exibidos.


[![Agora, todos os produtos são exibidos quando o MaxPrice caixa de texto está vazio](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Figura 8**: agora, todos os produtos são exibidos quando o `MaxPrice` caixa de texto está vazia ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


Há algumas limitações a serem observados com essa abordagem. Primeiro, saiba que o tipo de dados do parâmetro s é inferido por ele uso na consulta SQL. Se você alterar o `WHERE` cláusula de `@MaximumPrice = -1.0` para `@MaximumPrice = -1`, o tempo de execução trata o parâmetro como um inteiro. Se você tentar atribuir o `MaxPrice` caixa de texto para um valor decimal (como 5,00), ocorrerá um erro porque ele não é possível converter 5,00 em um inteiro. Para corrigir isso, torne-se de que você use `@MaximumPrice = -1.0` no `WHERE` cláusula ou, melhor ainda, defina as `ControlParameter` objeto s `Type` propriedade em Decimal.

Em segundo lugar, adicionando a `OR @MaximumPrice = -1.0` para o `WHERE` cláusula, o mecanismo de consulta não é possível usar um índice em `UnitPrice` (supondo que exista uma), resultando em uma verificação de tabela. Isso pode afetar o desempenho se houver um número suficientemente grande de registros no `Products` tabela. Uma abordagem melhor seria mover essa lógica para um procedimento armazenado em que um `IF` instrução executaria uma `SELECT` de consulta do `Products` de tabela sem um `WHERE` cláusula quando precisam de todos os registros a serem retornados ou uma cujo `WHERE` cláusula contém apenas o `UnitPrice` critérios, para que um índice pode ser usado.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Etapa 3: Criar e usar procedimentos armazenados com parâmetros

Procedimentos armazenados podem incluir um conjunto de parâmetros de entrada que podem ser usados nas instruções SQL definidas dentro do procedimento armazenado. Ao configurar o SqlDataSource para usar um procedimento armazenado que aceita parâmetros de entrada, esses valores de parâmetro podem ser especificados usando as mesmas técnicas assim como acontece com instruções SQL ad hoc.

Para ilustrar o uso de procedimentos armazenados em SqlDataSource, let s criar um novo procedimento armazenado no banco de dados Northwind denominado `GetProductsByCategory`, que aceita um parâmetro denominado `@CategoryID` e retorna todas as colunas dos produtos cuja `CategoryID` corresponde à coluna `@CategoryID`. Para criar um procedimento armazenado, vá para o Gerenciador de servidores e Detalhar o `NORTHWND.MDF` banco de dados. (Se você don t ver o Gerenciador de servidores, ativá-la indo até o menu Exibir e selecionar a opção de Gerenciador de servidores.)

Do `NORTHWND.MDF` de banco de dados, clique com botão direito na pasta Stored Procedures, escolha Adicionar novo procedimento armazenado e digite a seguinte sintaxe:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Clique no ícone Salvar (ou Ctrl + S) para salvar o procedimento armazenado. Você pode testar o procedimento armazenado, clicando duas vezes na pasta de procedimentos armazenados e escolha Executar. Isso lhe solicitará parâmetros de procedimento armazenado s (`@CategoryID`, nesta instância), depois que os resultados serão exibidos na janela de saída.


[![O GetProductsByCategory armazenados procedimento quando executada com um @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Figura 9**: O `GetProductsByCategory` procedimento armazenado quando executada com um `@CategoryID` de 1 ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


Deixe o s usar esse procedimento armazenado para exibir todos os produtos na categoria Bebidas em um GridView. Adicione um novo GridView à página e associá-lo para um novo SqlDataSource chamado `BeverageProductsDataSource`. Continue para especificar uma instrução SQL personalizada ou tela de procedimento armazenado, selecione o botão de opção de procedimento armazenado e escolher o `GetProductsByCategory` procedimento armazenado a partir da lista suspensa.


[![Selecione o GetProductsByCategory procedimento armazenado a partir da lista suspensa](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Figura 10**: selecione o `GetProductsByCategory` procedimento armazenado na lista suspensa ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


Uma vez que o procedimento armazenado aceita um parâmetro de entrada (`@CategoryID`), clique em Avançar solicita a especificar a origem para esse valor de parâmetro s. As bebidas `CategoryID` é 1, portanto, deixe a lista de lista suspensa de origem do parâmetro com nenhum e digite 1 na caixa de texto DefaultValue.


[![Use um valor embutido em código de 1 para retornar os produtos na categoria Bebidas](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Figura 11**: Use um valor de Hard-Coded de 1 para retornar os produtos na categoria Bebidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


Como mostra a marcação declarativa seguir, ao usar um procedimento armazenado, o s SqlDataSource `SelectCommand` estiver definida como o nome do procedimento armazenado e o [ `SelectCommandType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) está definido como `StoredProcedure`, indicando que que o `SelectCommand` é o nome de um procedimento armazenado em vez de uma instrução de SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Testar a página em um navegador. Somente os produtos que pertencem à categoria de bebidas são exibidos, embora *todos os* do produto campos são exibidos desde o `GetProductsByCategory` procedimento armazenado retorna todas as colunas da `Products` tabela. É claro, estamos poderia limitar ou personalizar os campos de GridView da caixa de diálogo Editar colunas GridView s.


[![Todos as bebidas sejam exibidos](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Figura 12**: todos as bebidas sejam exibidos ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Etapa 4: Invocar programaticamente um s SqlDataSource`Select()`instrução

Os exemplos podemos ve viu até agora no tutorial anterior e este tutorial tenha associado controles SqlDataSource diretamente a um GridView. Os dados de controle s SqlDataSource, no entanto, podem ser programaticamente acessados e enumerados no código. Isso pode ser particularmente útil quando você precisa para consultar dados inspecioná-lo, mas don t preciso exibi-la. Em vez de precisar escrever todo o código do ADO.NET para se conectar ao banco de dados, especifique o comando e recuperar os resultados de texto clichê, você pode deixar o SqlDataSource lidar com esse código monótono.

Para ilustrar a trabalhar com o SqlDataSource s dados programaticamente, imagine que seu chefe abordou a você com uma solicitação para criar uma página da web que exibe o nome de uma categoria selecionada aleatoriamente e seus produtos associados. Ou seja, quando um usuário visita esta página, queremos aleatoriamente, escolha uma categoria do `Categories` de tabela, exiba o nome da categoria e, em seguida, lista os produtos que pertencem a essa categoria.

Para fazer isso, precisamos dois controles SqlDataSource aquele para captar uma categoria aleatória do `Categories` e outra tabela para obter a categoria de produtos. Vamos criar o SqlDataSource que recupera um registro de categoria aleatória nesta etapa; Etapa 5 examina elaborando o SqlDataSource que recupera os produtos a categoria.

Comece adicionando um SqlDataSource para `ParameterizedQueries.aspx` e defina sua `ID` para `RandomCategoryDataSource`. Configurá-lo para que ele use a seguinte consulta SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` Retorna os registros classificados em ordem aleatória (consulte [Using `NEWID()` aleatoriamente os registros de classificação](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` Retorna o primeiro registro do conjunto de resultados. Juntar, esta consulta retorna os `CategoryID` e `CategoryName` valores da coluna de uma categoria de única, selecionada aleatoriamente.

Para exibir a categoria s `CategoryName` de valor, adicione um controle de Web de rótulo para a página, defina sua `ID` propriedade `CategoryNameLabel`e limpe seu `Text` propriedade. Para recuperar programaticamente os dados de um controle SqlDataSource, precisamos chamar seu `Select()` método. O [ `Select()` método](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) espera um único parâmetro de entrada do tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), que especifica como os dados devem ser da mensagem antes de serem retornados. Isso pode incluir instruções sobre como classificar e filtrar os dados e é usado pelos dados de que controles da Web durante a classificação ou a paginação dos dados de um controle SqlDataSource. Para nosso exemplo, no entanto, podemos don precisa os dados a ser modificado antes de serem retornados e, portanto, passará o `DataSourceSelectArguments.Empty` objeto.

O `Select()` método retorna um objeto que implementa `IEnumerable`. O tipo exato retornado depende do valor do controle SqlDataSource s [ `DataSourceMode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Conforme discutido no tutorial anterior, essa propriedade pode ser definida como um valor de `DataSet` ou `DataReader`. Se definido como `DataSet`, o `Select()` método retorna um [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) objeto; se definido como `DataReader`, ele retorna um objeto que implementa [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Uma vez que o `RandomCategoryDataSource` SqlDataSource tem seu `DataSourceMode` propriedade definida como `DataSet` (o padrão), trabalharemos com um objeto de DataView.

O código a seguir ilustra como recuperar os registros da `RandomCategoryDataSource` SqlDataSource como uma DataView, bem como como ler o `CategoryName` valor da coluna da primeira linha da exibição de dados:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` Retorna o primeiro `DataRowView` no DataView. `randomCategoryView(0)("CategoryName")` Retorna o valor da `CategoryName` coluna nesta primeira linha. Observe que o DataView é fracamente tipado. Para fazer referência a um valor de coluna específico, precisamos passar no nome da coluna como uma cadeia de caracteres (CategoryName, neste caso). A Figura 13 mostra a mensagem exibida no `CategoryNameLabel` ao exibir a página. É claro, o nome da categoria reais exibido é selecionado aleatoriamente pelo `RandomCategoryDataSource` SqlDataSource sobre cada visita à página (incluindo postbacks).


[![O s da categoria selecionada aleatoriamente que nome é exibido](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Figura 13**: s The aleatoriamente selecionado categoria nome é exibido ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> Se o SqlDataSource controlar s `DataSourceMode` propriedade tivesse sido definida como `DataReader`, o valor de retorno a `Select()` método precisaria ser convertido em `IDataReader`. Para ler o `CategoryName` valor da coluna da primeira linha, podemos d usar um código como:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Com o SqlDataSource selecionando aleatoriamente uma categoria, podemos está pronto para adicionar o GridView que lista os produtos de categoria s.

> [!NOTE]
> Em vez de usar um controle da Web do rótulo para exibir o nome da categoria s, podemos poderia ter adicionado um FormView ou DetailsView para a página, associá-lo ao SqlDataSource. Usando o rótulo, no entanto, nos permitiu explorar como chamar programaticamente o s SqlDataSource `Select()` instrução e trabalhar com seus dados resultantes no código.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Etapa 5: Atribuir valores de parâmetro programaticamente

Todos os exemplos podemos ve viu até agora neste tutorial ter usado um valor de parâmetro embutido em código ou um obtido em uma das fontes de parâmetro predefinidos (um valor de cadeia de consulta, um controle da Web na página e assim por diante). No entanto, os parâmetros de s controle SqlDataSource podem também ser definidos programaticamente. Para concluir o nosso exemplo atual, é necessário um SqlDataSource que retorna todos os produtos que pertencem a uma categoria especificada. Este SqlDataSource terá um `CategoryID` parâmetro cujo valor precisa ser definido com base no `CategoryID` retornado pelo valor da coluna a `RandomCategoryDataSource` SqlDataSource no `Page_Load` manipulador de eventos.

Comece adicionando um GridView à página e associá-lo para um novo SqlDataSource chamado `ProductsByCategoryDataSource`. Muito como fizemos na etapa 3, configure o SqlDataSource, de modo que ele invoca o `GetProductsByCategory` procedimento armazenado. Deixe o conjunto de lista suspensa de origem do parâmetro como None, mas não insira um valor padrão, como podemos definirá esse valor padrão por meio de programação.


[![Não especificar um parâmetro de origem ou o valor padrão](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Figura 14**: não especificar uma fonte de parâmetro ou valor padrão ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


Depois de concluir o assistente SqlDataSource, a marcação declarativa resultante deve ser semelhante ao seguinte:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

Podemos atribuir a `DefaultValue` do `CategoryID` parâmetro programaticamente o `Page_Load` manipulador de eventos:


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Com esse acréscimo, a página inclui um GridView que mostra os produtos associados a categoria selecionada aleatoriamente.


[![Não especificar um parâmetro de origem ou o valor padrão](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Figura 15**: não especificar uma fonte de parâmetro ou valor padrão ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>Resumo

O SqlDataSource permite que os desenvolvedores de páginas definir consultas com parâmetros cujos valores de parâmetro podem ser embutido em código, extraídos de fontes de parâmetro predefinidos ou atribuídos por meio de programação. Neste tutorial vimos como criar uma consulta parametrizada no Assistente para configurar a fonte de dados para consultas SQL ad hoc e procedimentos armazenados. Também examinamos usando fontes de parâmetro embutido em código, um controle da Web como uma fonte de parâmetro e por meio de programação especificando o valor do parâmetro.

Como com o ObjectDataSource, SqlDataSource também fornece recursos para modificar seus dados subjacentes. No próximo tutorial, examinaremos como definir `INSERT`, `UPDATE`, e `DELETE` instruções com o SqlDataSource. Quando essas instruções foram adicionadas, podemos pode utilizar interna inserir, editar e excluir recursos inerentes aos controles GridView, DetailsView e FormView.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores líder para este tutorial foram Scott Clyde, Randell Schmidt e Ken Pespisa. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](querying-data-with-the-sqldatasource-control-vb.md)
> [Próximo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
