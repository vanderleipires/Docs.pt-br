---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Usar consultas parametrizadas com SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: "Neste tutorial, vamos continuar nossa aparência no controle SqlDataSource e saiba como definir consultas parametrizadas. Os parâmetros podem ser especificados dois decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b66c68b8306b905a800465ab0ed720ae6f9d16b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Usar consultas parametrizadas com SqlDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) ou [baixar PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> Neste tutorial, vamos continuar nossa aparência no controle SqlDataSource e saiba como definir consultas parametrizadas. Os parâmetros podem ser especificados declarativamente e programaticamente e podem ser extraídos de um número de locais como querystring, sessão estado, outros controles e muito mais.


## <a name="introduction"></a>Introdução

No tutorial anterior, vimos como usar o controle SqlDataSource para recuperar dados diretamente de um banco de dados. Usando o Assistente Configurar fonte de dados, escolhemos o banco de dados e, em seguida,: selecione as colunas para retornar de uma tabela ou exibição; Insira uma instrução SQL personalizada; ou use um procedimento armazenado. Se selecionar colunas de uma tabela ou exibição ou a inserção de uma instrução SQL personalizada, o SqlDataSource controle s `SelectCommand` SQL ad hoc resultante é atribuído a propriedade `SELECT` instrução e é isso `SELECT` instrução é executada quando o S SqlDataSource `Select()` método é invocado (programaticamente ou automaticamente de um controle da Web de dados).

O SQL `SELECT` instruções usadas nas demonstrações tutorial s anterior não tinha `WHERE` cláusulas. Em um `SELECT` instrução, o `WHERE` cláusula pode ser usada para limitar os resultados retornados. Por exemplo, para exibir os nomes dos produtos que custam mais de r $50,00, podemos usar a consulta a seguir:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

Normalmente, os valores usados em um `WHERE` cláusula são determinar por alguns fonte externa, como um valor de querystring, uma variável de sessão ou a entrada do usuário de um controle da Web na página. Idealmente, essas entradas são especificadas por meio do uso de *parâmetros*. Com o Microsoft SQL Server, os parâmetros são indicados usando `@parameterName`, como em:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

O SqlDataSource dá suporte a consultas parametrizadas, tanto de `SELECT` instruções e `INSERT`, `UPDATE`, e `DELETE` instruções. Além disso, os valores de parâmetro podem ser automaticamente extraídos de uma variedade de fontes de querystring, estado de sessão, controles na página e assim por diante ou podem ser atribuídos programaticamente. Neste tutorial, veremos como definir consultas parametrizadas, bem como para especificar o parâmetro valores declarativamente e programaticamente.

> [!NOTE]
> No tutorial anterior comparamos ObjectDataSource que foi nossa ferramenta de preferência sobre os tutoriais primeiro 46 com SqlDataSource, observando as semelhanças conceituais. Também estendem essas semelhanças para parâmetros. Os parâmetros de s ObjectDataSource mapeados para os parâmetros de entrada para os métodos na camada de lógica de negócios. Com o SqlDataSource, os parâmetros são definidos diretamente dentro de uma consulta SQL. Ambos os controles tem coleções de parâmetros para seus `Select()`, `Insert()`, `Update()`, e `Delete()` métodos e ambos podem ter esses valores de parâmetro preenchidos a partir de fontes previamente definidas (valores de querystring, variáveis de sessão e assim por diante ) ou atribuídas programaticamente.


## <a name="creating-a-parameterized-query"></a>Criando uma consulta parametrizada

O Assistente para configurar a fonte de dados do SqlDataSource controle s oferece três vias para definir o comando a ser executada para recuperar os registros de banco de dados:

- Selecionando colunas de uma tabela ou exibição existente,
- Inserindo uma instrução SQL personalizada, ou
- Escolhendo um procedimento armazenado

Ao escolher colunas de uma tabela existente ou exibição, os parâmetros para o `WHERE` cláusula deve ser especificada por meio de adicionar `WHERE` caixa de diálogo cláusula. Ao criar uma instrução SQL personalizada, no entanto, você pode inserir os parâmetros diretamente para o `WHERE` cláusula (usando `@parameterName` para indicar cada parâmetro). Um [procedimento armazenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) consiste em uma ou mais instruções SQL, e essas instruções podem ser parametrizadas. Os parâmetros usados em instruções SQL, no entanto, devem ser passados em como parâmetros de entrada para o procedimento armazenado.

Como criar uma consulta parametrizada depende de como o s SqlDataSource `SelectCommand` é s especificado, permitem que dê uma olhada em todas as três abordagens. Para começar, abra o `ParameterizedQueries.aspx` página o `SqlDataSource` pasta, arraste um controle SqlDataSource da caixa de ferramentas para o Designer e defina seu `ID` para `Products25BucksAndUnderDataSource`. Em seguida, clique no link configurar fonte de dados de marca inteligente de s de controle. Selecione o banco de dados a ser usado (`NORTHWINDConnectionString`) e clique em Avançar.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Etapa 1: Adicionando um`WHERE`cláusula ao separar as colunas de uma tabela ou exibição

Ao selecionar os dados a serem retornados do banco de dados com o controle SqlDataSource, o Assistente Configurar fonte de dados permite simplesmente selecione as colunas para retornar de uma tabela existente ou (consulte a Figura 1). Fazer isso automaticamente cria um SQL `SELECT` instrução, que é o que é enviado para o banco de dados quando o s SqlDataSource `Select()` método é invocado. Como foi feito no tutorial anterior, selecione a tabela de produtos na lista suspensa e verifique o `ProductID`, `ProductName`, e `UnitPrice` colunas.


[![Selecione as colunas para retornar de uma tabela ou exibição](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: selecione as colunas para retorno de uma tabela ou exibição ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Para incluir um `WHERE` cláusula o `SELECT` instrução, clique no `WHERE` botão, que traz a adicionar `WHERE` caixa de diálogo de cláusula (consulte a Figura 2). Para adicionar um parâmetro para limitar os resultados retornados pelo `SELECT` da consulta, primeiro escolha a coluna para filtrar os dados. Em seguida, escolha o operador a ser usado para filtragem (=, &lt;, &lt;= &gt;, e assim por diante). Por fim, escolha a origem do valor do parâmetro, como do estado de sessão ou querystring. Depois de configurar o parâmetro, clique no botão Adicionar para incluí-lo no `SELECT` consulta.

Neste exemplo, vamos s retornar apenas esses resultados onde o `UnitPrice` valor é menor ou igual a $25,00. Portanto, escolha `UnitPrice` da lista suspensa de colunas e &lt;= na lista suspensa do operador. Ao usar um valor de parâmetro embutido (como $25,00) ou se o valor do parâmetro deve ser especificada programaticamente, selecione nenhum da lista suspensa de fontes. Em seguida, insira o valor de parâmetro embutido na caixa de texto valor 25,00 e concluir o processo clicando no botão Adicionar.


[![Limitar os resultados retornados pelo Adicione WHERE caixa de diálogo cláusula](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Figura 2**: limitar os resultados retornados de adicionar `WHERE` caixa de diálogo cláusula ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Depois de adicionar o parâmetro, clique em Okey para retornar para o Assistente Configurar fonte de dados. O `SELECT` declaração na parte inferior do assistente agora deve incluir um `WHERE` cláusula com um parâmetro chamado `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Se você especificar várias condições no `WHERE` cláusula de adicionar `WHERE` caixa de diálogo cláusula, o assistente une-los com o `AND` operador. Se você precisa incluir um `OR` no `WHERE` cláusula (como `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), em seguida, você precisa criar o `SELECT` instrução através da tela de instrução SQL personalizada.


Concluir a configuração SqlDataSource (clique em seguida, em seguida, concluir) e, em seguida, verifique se a marcação declarativa de s SqlDataSource. A marcação agora inclui um `<SelectParameters>` coleção, que indica as fontes para os parâmetros a `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Quando o s SqlDataSource `Select()` método é chamado, o `UnitPrice` o valor do parâmetro (25,00) é aplicado ao `@UnitPrice` parâmetro no `SelectCommand` antes de serem enviados para o banco de dados. O resultado é que apenas os produtos menor ou igual a $25,00 são retornados do `Products` tabela. Para confirmar isso, adicione um controle GridView à página, associá-lo a essa fonte de dados e, em seguida, exibir a página por meio de um navegador. Você verá somente os produtos listados que são menor ou igual a $25,00, que confirma a Figura 3.


[![Somente os produtos de menor que ou igual a $25,00 são exibidos](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Figura 3**: somente os produtos de menor que ou igual a $25,00 são exibidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Etapa 2: Adicionando parâmetros para uma instrução SQL personalizada

Ao adicionar uma instrução SQL personalizada que você pode inserir o `WHERE` cláusula explicitamente ou especifique um valor na célula de filtro do construtor de consultas. Para demonstrar isso, permitem s exibir apenas os produtos em um GridView cujos preços são menores do que um certo limite. Comece adicionando uma caixa de texto para o `ParameterizedQueries.aspx` página para obter o valor de limite do usuário. Defina a caixa de texto s `ID` propriedade `MaxPrice`. Adicionar um controle de botão Web e definir seu `Text` propriedade para produtos de correspondência de exibição.

Em seguida, arraste um controle GridView à página e na marca inteligente de optar por criar um novo SqlDataSource denominado `ProductsFilteredByPriceDataSource`. No Assistente para configurar a fonte de dados, vá para especificar uma instrução SQL personalizada ou tela de procedimento armazenado (consulte a Figura 4) e digite a seguinte consulta:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Depois de digitar a consulta (manualmente ou por meio do construtor de consultas), clique em Avançar.


[![Retornar apenas os produtos menor ou igual a um valor de parâmetro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Figura 4**: retornar somente os produtos de menor que ou igual a um valor de parâmetro ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Como a consulta inclui parâmetros, a próxima tela do assistente nos solicitará a origem dos valores de parâmetros. Escolha o controle da lista suspensa de fonte parâmetro e `MaxPrice` (o controle de caixa de texto s `ID` valor) na lista suspensa ControlID. Você também pode inserir um valor padrão opcional para usar no caso em que o usuário não insere qualquer texto no `MaxPrice` caixa de texto. Por enquanto, não insira um valor padrão.


[![O s MaxPrice TextBox que propriedade Text é usada como a origem do parâmetro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Figura 5**: O `MaxPrice` TextBox s `Text` propriedade é usada como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Conclua o Assistente Configurar fonte de dados clicando em Avançar e em seguida concluir. A marcação declarativa para o GridView, caixa de texto, botão e SqlDataSource segue:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Observe que o parâmetro dentro a s SqlDataSource `<SelectParameters>` seção é uma `ControlParameter`, que inclui as propriedades adicionais como `ControlID` e `PropertyName`. Quando o s SqlDataSource `Select()` método é chamado, o `ControlParameter` captura o valor da propriedade de controle da Web especificado e o atribui ao parâmetro correspondente no `SelectCommand`. Neste exemplo, o `MaxPrice` s propriedade Text é usado como o `@MaxPrice` o valor do parâmetro.

Reserve um minuto para exibir esta página por meio de um navegador. Quando o primeiro visitar a página ou sempre que o `MaxPrice` caixa de texto não tiver um valor que não há registros são exibidos em GridView.


[![Não há registros são que exibidos quando o MaxPrice caixa de texto está vazia](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Figura 6**: registros não são exibidos quando o `MaxPrice` caixa de texto está vazia ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


O motivo pelo qual não há produtos são mostrados ocorre porque, por padrão, uma cadeia de caracteres vazia para um valor de parâmetro é convertida em um banco de dados `NULL` valor. Desde que a comparação de `[UnitPrice] <= NULL` sempre é avaliada como False, nenhum resultado será retornado.

Insira um valor na caixa de texto, como 5,00 e clique no botão de produtos de correspondência de exibição. Em um postback, SqlDataSource informa que o GridView que uma das suas fontes de parâmetro foi alterado. Consequentemente, o GridView reconecta a SqlDataSource, exibindo os produtos menor ou igual a US $5,00.


[![Produtos de menor que ou igual a US $5,00 são exibidos](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Figura 7**: produtos menor que ou igual a US $5,00 são exibidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Inicialmente, exibindo todos os produtos

Em vez de não exibir nenhum produto quando a página for carregada pela primeira vez, podemos desejar exibir *todos os* produtos. Uma maneira de listar todos os produtos sempre que o `MaxPrice` caixa de texto está vazia é definir o valor padrão do parâmetro s para algum valor incrivelmente alta, como 1000000, pois ele s improvável que a Northwind Traders fará já tiver um inventário cujo preço unitário exceder r $1.000.000. No entanto, essa abordagem é shortsighted e pode não funcionar em outras situações.

Nos tutoriais anteriores - [parâmetros declarativos](../basic-reporting/declarative-parameters-cs.md) e [filtragem de mestre/detalhes com um DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) foram encontramos um problema semelhante. Nossa solução existe foi colocar essa lógica na camada de lógica de negócios. Especificamente, o BLL examinou o valor de entrada e, se ele foi `NULL` ou alguns reservados valor, a chamada foi roteada para o método DAL que todos os registros retornados. Se o valor de entrada era um valor de filtragem normal, foi feita uma chamada ao método DAL que executou uma instrução SQL que é usado com um parâmetros `WHERE` cláusula com o valor fornecido.

Infelizmente, podemos ignorar a arquitetura ao usar o SqlDataSource. Em vez disso, é preciso personalizar a instrução SQL para capturar inteligente todos os registros se o `@MaximumPrice` parâmetro é `NULL` ou algum valor reservado. Para este exercício, permitem s que ele seja tão que, se o `@MaximumPrice` parâmetro for igual ao `-1.0`, em seguida, *todos os* os registros devem ser retornadas (`-1.0` funciona como um valor reservado, já que nenhum produto pode ter um negativo `UnitPrice`valor). Para fazer isso, podemos usar a seguinte instrução SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Isso `WHERE` cláusula retorna *todos os* registra se o `@MaximumPrice` parâmetro for igual a `-1.0`. Se o valor do parâmetro não for `-1.0`, apenas os produtos cujo `UnitPrice` é menor que ou igual ao `@MaximumPrice` valor de parâmetro são retornados. Definindo o valor padrão de `@MaximumPrice` parâmetro para `-1.0`, o primeiro carregamento de página (ou sempre que o `MaxPrice` caixa de texto está vazia), `@MaximumPrice` terá um valor de `-1.0` e todos os produtos serão exibidos.


[![Agora todos os produtos são exibidos quando o MaxPrice caixa de texto está vazia](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Figura 8**: agora todos os produtos são exibidos quando o `MaxPrice` caixa de texto está vazia ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Há algumas limitações observar com essa abordagem. Primeiro, observe que o tipo de dados do parâmetro s é inferido por ele uso s na consulta SQL. Se você alterar o `WHERE` cláusula de `@MaximumPrice = -1.0` para `@MaximumPrice = -1`, o tempo de execução trata o parâmetro como um inteiro. Se você tentar atribuir o `MaxPrice` caixa de texto para um valor decimal (como 5.00), ocorrerá um erro porque ele não é possível converter 5,00 para um número inteiro. Para corrigir isso, torne-se de que você use `@MaximumPrice = -1.0` no `WHERE` cláusula ou, melhor ainda, defina o `ControlParameter` objeto s `Type` propriedade em Decimal.

Em segundo lugar, adicionando o `OR @MaximumPrice = -1.0` para o `WHERE` cláusula, o mecanismo de consulta não pode usar um índice em `UnitPrice` (supondo que exista uma), resultando assim em uma verificação de tabela. Isso pode afetar o desempenho se houver um número grande o suficiente de registros de `Products` tabela. Uma abordagem melhor seria mover essa lógica para um procedimento armazenado onde um `IF` instrução executaria um `SELECT` consultados o `Products` tabela sem um `WHERE` cláusula quando precisam de todos os registros a serem retornados ou uma cujo `WHERE` cláusula contém apenas o `UnitPrice` critérios, para que um índice pode ser usado.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Etapa 3: Criar e usar procedimentos armazenados com parâmetros

Procedimentos armazenados podem incluir um conjunto de parâmetros de entrada que podem ser usadas nas declarações SQL definidas no procedimento armazenado. Ao configurar o SqlDataSource para usar um procedimento armazenado que aceita parâmetros de entrada, esses valores de parâmetro podem ser especificados usando as mesmas técnicas como com instruções SQL ad hoc.

Para ilustrar o uso de procedimentos armazenados em SqlDataSource, permitem s criar um novo procedimento armazenado no banco de dados Northwind denominado `GetProductsByCategory`, que aceita um parâmetro chamado `@CategoryID` e retorna todas as colunas dos produtos cuja `CategoryID` coluna corresponde à `@CategoryID`. Para criar um procedimento armazenado, vá para o Gerenciador de servidores e Detalhar o `NORTHWND.MDF` banco de dados. (Se você não t ver o Gerenciador de servidores, colocá-lo para cima no menu de exibição e selecionando a opção de Gerenciador de servidores.)

Do `NORTHWND.MDF` de banco de dados, com o botão direito na pasta Stored Procedures, escolha Adicionar novo procedimento armazenado e digite a seguinte sintaxe:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Clique no ícone Salvar (ou Ctrl + S) para salvar o procedimento armazenado. Você pode testar o procedimento armazenado, clicando duas vezes na pasta Stored Procedures e escolha Executar. Isso solicitará os parâmetros de procedimento armazenado s (`@CategoryID`, neste exemplo), depois que os resultados serão exibidos na janela de saída.


[![O GetProductsByCategory armazenado procedimento quando executado com um @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Figura 9**: O `GetProductsByCategory` procedimento armazenado quando executado com um `@CategoryID` de 1 ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Permitir que o s usar este procedimento armazenado para exibir todos os produtos na categoria de bebidas em um controle GridView. Adicionar um novo GridView à página e associá-lo a um novo SqlDataSource denominado `BeverageProductsDataSource`. Continuar para especificar uma instrução SQL personalizada ou tela de procedimento armazenado, selecione o botão de opção de procedimento armazenado e escolha o `GetProductsByCategory` procedimento armazenado a partir da lista suspensa.


[![Selecione o GetProductsByCategory procedimento armazenado a partir da lista suspensa](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Figura 10**: selecione o `GetProductsByCategory` procedimento armazenado na lista suspensa ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Desde que o procedimento armazenado aceita um parâmetro de entrada (`@CategoryID`), clicar em Avançar solicita conosco para especificar a origem para este valor do parâmetro s. As bebidas `CategoryID` é 1, para deixar a lista de lista suspensa de origem do parâmetro em nenhum e digite 1 na caixa de texto valor padrão.


[![Use um valor embutido de 1 para retornar os produtos na categoria de bebidas](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Figura 11**: Use um valor de Hard-Coded de 1 para retornar os produtos na categoria de bebidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Como a seguir mostra marcação declarativa, ao usar um procedimento armazenado, o s SqlDataSource `SelectCommand` propriedade é definida como o nome do procedimento armazenado e o [ `SelectCommandType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) é definido como `StoredProcedure`, indicando que que o `SelectCommand` é o nome de um procedimento armazenado em vez de uma instrução de SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Testar a página em um navegador. Somente os produtos que pertencem à categoria de bebidas são exibidos, embora *todos os* do produto campos são exibidos desde o `GetProductsByCategory` procedimento armazenado retorna todas as colunas da `Products` tabela. É claro, poderia limitar ou personalizar os campos exibidos em GridView da caixa de diálogo Editar colunas do GridView s.


[![Todos as bebidas são exibidos](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Figura 12**: todas as bebidas são exibidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Etapa 4: Chamar programaticamente um s SqlDataSource`Select()`instrução

Os exemplos, var visto no tutorial anterior e este tutorial até agora tem controles de SqlDataSource associados diretamente a um controle GridView. Os dados de controle s SqlDataSource, no entanto, podem ser programaticamente acessados e enumerados no código. Isso pode ser particularmente útil quando você precisa consultar dados inspecioná-lo, mas don t precisa exibi-lo. Em vez de escrever todo o código ADO.NET para conectar-se ao banco de dados, especifique o comando e recuperar os resultados do trabalho, você pode deixar o SqlDataSource lidar com esse código monótonas.

Para ilustrar a trabalhar com o s SqlDataSource dados programaticamente, imagine que o seu chefe abordou você com uma solicitação para criar uma página da web que exibe o nome de uma categoria selecionada aleatoriamente e seus produtos associados. Ou seja, quando um usuário visita esta página, queremos aleatoriamente, escolha uma categoria do `Categories` de tabela, exibir o nome da categoria e, em seguida, lista os produtos que pertencem a essa categoria.

Para fazer isso, precisamos dois controles SqlDataSource um para obter uma categoria aleatória do `Categories` e outra tabela para obter a categoria de produtos. Criaremos SqlDataSource que recupera um registro de categoria aleatório nesta etapa; Etapa 5 examina criação SqlDataSource que recupera os produtos de categoria.

Comece adicionando um SqlDataSource para `ParameterizedQueries.aspx` e defina seu `ID` para `RandomCategoryDataSource`. Configurá-lo para que ele usa a seguinte consulta SQL:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()`Retorna os registros classificados em ordem aleatória (consulte [usando `NEWID()` aleatoriamente os registros de classificação](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1`Retorna o primeiro registro do conjunto de resultados. Resumindo, esta consulta retorna o `CategoryID` e `CategoryName` valores de coluna de uma categoria de único, selecionado aleatoriamente.

Para exibir a categoria s `CategoryName` valor, adicione um controle de rótulo à página, defina seu `ID` propriedade `CategoryNameLabel`e limpar sua `Text` propriedade. Para recuperar programaticamente os dados de um controle SqlDataSource, precisamos chamar o `Select()` método. O [ `Select()` método](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) espera um único parâmetro de entrada do tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), que especifica como os dados devem ser mensagem antes de serem retornados. Isso pode incluir instruções sobre como classificar e filtrar os dados e é usado pelos dados de que controles da Web durante a classificação ou paginação de dados de um controle SqlDataSource. Para nosso exemplo, no entanto, nós don precisa de dados a ser modificado antes de serem retornados e, portanto, passará o `DataSourceSelectArguments.Empty` objeto.

O `Select()` método retorna um objeto que implementa `IEnumerable`. O tipo exato retornado depende do valor do controle SqlDataSource s [ `DataSourceMode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Como discutido no tutorial anterior, essa propriedade pode ser definida para um valor de `DataSet` ou `DataReader`. Se definido como `DataSet`, o `Select()` método retorna um [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) objeto; se definido como `DataReader`, ele retorna um objeto que implementa [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Como o `RandomCategoryDataSource` SqlDataSource tem seu `DataSourceMode` propriedade definida como `DataSet` (o padrão), trabalharemos com um objeto de DataView.

O código a seguir demonstra como recuperar os registros da `RandomCategoryDataSource` SqlDataSource como um DataView, bem como a leitura de `CategoryName` valor de coluna da primeira linha da exibição de dados:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]`Retorna o primeiro `DataRowView` em DataView. `randomCategoryView[0]["CategoryName"]`Retorna o valor da `CategoryName` coluna nesta primeira linha. Observe que a exibição de dados é menos tipada. Para fazer referência a um valor de coluna específico, é preciso passar no nome da coluna como uma cadeia de caracteres (CategoryName, neste caso). Figura 13 mostra a mensagem exibida no `CategoryNameLabel` ao exibir a página. Naturalmente, o nome da categoria real exibido é selecionado aleatoriamente com pelo `RandomCategoryDataSource` SqlDataSource em cada visita à página (incluindo postbacks).


[![O s categoria selecionada aleatoriamente que nome é exibido](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Figura 13**: s o aleatoriamente categoria selecionada é o nome exibido ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Se o SqlDataSource controle s `DataSourceMode` propriedade tive sido definida como `DataReader`, o valor de retorno de `Select()` método seria necessário ser convertido em `IDataReader`. Para ler o `CategoryName` valor de coluna da primeira linha, d, use um código como:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Com o SqlDataSource aleatoriamente selecionando uma categoria, estamos re pronto para adicionar o GridView que lista os produtos de categoria s.

> [!NOTE]
> Em vez de usar um controle de rótulo Web para exibir o nome da categoria s, poderia adicionamos um FormView ou DetailsView para a página, associação a SqlDataSource. No entanto, usando o rótulo, permitido para explorar como chamar programaticamente o s SqlDataSource `Select()` instrução e trabalhar com seus dados resultantes no código.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Etapa 5: Atribuir valores de parâmetro programaticamente

Todos os exemplos de nós ve visto até o momento neste tutorial ter usado um valor de parâmetro embutido ou um tirado de uma das fontes de parâmetro predefinidos (um valor de querystring, um controle da Web na página e assim por diante). No entanto, os parâmetros de s controle SqlDataSource podem também ser definidos programaticamente. Para concluir nosso exemplo atual, é preciso um SqlDataSource que retorna todos os produtos que pertencem a uma categoria especificada. Este SqlDataSource terá um `CategoryID` parâmetro cujo valor deve ser definido com base no `CategoryID` retornado pelo valor da coluna a `RandomCategoryDataSource` SqlDataSource no `Page_Load` manipulador de eventos.

Comece adicionando um controle GridView à página e associá-lo a um novo SqlDataSource denominado `ProductsByCategoryDataSource`. Muito como fizemos na etapa 3, configure o SqlDataSource para que ele chama o `GetProductsByCategory` procedimento armazenado. Deixe o conjunto de lista suspensa de origem do parâmetro como None, mas não insira um valor padrão, como definir programaticamente o valor padrão.


[![Não especifique uma fonte de parâmetro ou valor padrão](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Figura 14**: não especificar um parâmetro de origem ou o valor padrão ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Depois de concluir o Assistente de SqlDataSource, a marcação declarativa resultante deve ser semelhante ao seguinte:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Podemos atribuir a `DefaultValue` do `CategoryID` parâmetro programaticamente o `Page_Load` manipulador de eventos:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Com essa adição, a página inclui um controle GridView que mostra os produtos associados a categoria selecionada aleatoriamente.


[![Não especifique uma fonte de parâmetro ou valor padrão](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Figura 15**: não especificar um parâmetro de origem ou o valor padrão ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Resumo

O SqlDataSource permite que os desenvolvedores de página definir consultas com parâmetros cujos valores de parâmetro podem ser embutida, extraídos de fontes de parâmetro predefinidos ou atribuídos programaticamente. Neste tutorial, vimos como criar uma consulta parametrizada no Assistente para configurar a fonte de dados para consultas SQL ad hoc e procedimentos armazenados. Também vimos usando fontes de parâmetro embutido, um controle da Web como uma fonte de parâmetro e por meio de programação especificando o valor do parâmetro.

Como com ObjectDataSource, SqlDataSource também fornece recursos para modificar os dados subjacentes. O seguinte tutorial, examinaremos como definir `INSERT`, `UPDATE`, e `DELETE` instruções com SqlDataSource. Depois de adicionar essas instruções, podemos pode utilizar internos inserir, editar e excluir recursos inerentes aos controles GridView, DetailsView e FormView.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Scott Clyde Randell Schmidt e Ken Pespisa. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](querying-data-with-the-sqldatasource-control-cs.md)
[Próximo](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
