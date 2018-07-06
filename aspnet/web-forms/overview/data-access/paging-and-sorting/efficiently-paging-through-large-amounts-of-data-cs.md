---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Paginação de grandes quantidades de dados (c#) com eficiência | Microsoft Docs
author: rick-anderson
description: A opção de paginação de padrão de um controle de apresentação de dados é inadequada ao trabalhar com grandes quantidades de dados, como seu retriev de controle de fonte de dados subjacente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a6d023f299d3c36e0b9f0d00f2b531d73657135c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367875"
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Paginação de grandes quantidades de dados (c#) com eficiência
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) ou [baixar PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> A opção de paginação padrão de um controle de apresentação de dados é inadequada ao trabalhar com grandes quantidades de dados, como o controle de fonte de dados subjacente recupera todos os registros, mesmo que apenas um subconjunto dos dados é exibido. Em tais circunstâncias, deve transformamos personalizados para paginação.


## <a name="introduction"></a>Introdução

Como discutimos no tutorial anterior, a paginação pode ser implementada em uma das duas maneiras:

- **Paginação padrão** pode ser implementada, simplesmente marcando a opção habilitar paginação nos dados de controle de Web s marca inteligente; no entanto, sempre que exibir uma página de dados, o ObjectDataSource recupera *todos os* de registros, até mesmo No entanto, apenas um subconjunto deles são exibidos na página
- **Paginação personalizada** melhora o desempenho de padrão de paginação, recuperando somente os registros de banco de dados que precisam ser exibido para a página específica de dados solicitados pelo usuário; no entanto, paginação personalizada envolve um pouco mais de esforço para implementar que paginação padrão

Devido à facilidade de verificação de implementação apenas uma caixa de seleção e você está pronto! paginação padrão é uma opção atraente. Sua abordagem de ve ao recuperar todos os registros, porém, torna uma opção ser inaceitável quando a paginação de suficientemente grandes quantidades de dados ou para sites com muitos usuários simultâneos. Em tais circunstâncias, podemos deve ativar a personalizado de paginação para fornecer um sistema dinâmico.

O desafio de paginação personalizada está sendo capaz de gravar uma consulta que retorna o conjunto exato de registros necessários para uma determinada página de dados. Felizmente, o Microsoft SQL Server 2005 fornece uma nova palavra-chave para resultados de classificação, que nos permite escrever uma consulta que pode recuperar com eficiência o subconjunto apropriado de registros. Neste tutorial, veremos como usar essa nova palavra-chave do SQL Server 2005 para implementar a paginação personalizada em um controle GridView. Enquanto a interface do usuário para paginação personalizada é idêntica ao que a paginação padrão, passo a passo de uma página para a próxima usando paginação personalizada pode ser várias ordens de magnitude mais rapidamente do que a paginação padrão.

> [!NOTE]
> O ganho de desempenho exato exibido pelos paginação personalizada depende do número total de registros sendo paginados por meio do e a carga que está sendo colocada no servidor de banco de dados. No final deste tutorial, examinaremos algumas métricas aproximadas que demonstram os benefícios no desempenho obtido por meio de paginação personalizada.


## <a name="step-1-understanding-the-custom-paging-process"></a>Etapa 1: Noções básicas sobre o processo de paginação personalizada

Quando a paginação por meio de dados, os registros precisos exibidos em uma página dependem da página de dados que está sendo solicitados e o número de registros exibidos por página. Por exemplo, imagine que gostaríamos de página por meio de 81 produtos, exibindo os 10 produtos por página. Ao exibir a primeira página, d queremos produtos de 1 a 10; ao exibir a segunda página d podemos estar interessado em 11 de 20 por meio de produtos e assim por diante.

Há três variáveis que determinam quais registros devem ser recuperadas e como a interface de paginação deve ser renderizada:

- **Índice de linha de início** o índice da primeira linha na página de dados a serem exibidos; isso pode índice ser calculada multiplicando o índice de página, os registros a serem exibidos por página e adição de um. Por exemplo, quando a paginação por meio de registros de 10 por vez, para a primeira página (cujo índice é 0), o índice de linha de início é 0 \* 10 + 1 ou 1; para a segunda página (cujo índice é 1), o índice de linha de início é 1 \* 10 + 1 , ou 11.
- **Máximo de linhas** o número máximo de registros a serem exibidos por página. Essa variável é conhecida como o número máximo de linhas, pois a última página lá pode ser menos registros retornados que o tamanho da página. Por exemplo, quando a paginação por meio de registros por página 81 produtos 10, a página Nona e o final terá apenas um registro. No entanto, nenhuma página mostrará mais registros que o valor máximo de linhas.
- **Contagem total de registro** o número total de registros sendo paginado por meio do. Embora essa variável não seja necessário para determinar quais registros para recuperar para uma determinada página, ele ditar a interface de paginação. Por exemplo, se houver 81 sendo paginados por meio de produtos, a interface de paginação sabe para exibir números de página nove a interface do usuário de paginação.

Com a paginação padrão, o índice de linha de início é calculado como o produto, o índice de página e o tamanho da página mais um, enquanto o máximo de linhas é simplesmente o tamanho da página. Desde que a paginação padrão recupera todos os registros de banco de dados durante a renderização de qualquer página de dados, o índice para cada linha é conhecido, tornando o movimento a linha de índice de linha de iniciar uma tarefa trivial. Além disso, a contagem Total de registros é prontamente disponível, como ela s simplesmente o número de registros na DataTable (ou qualquer objeto que está sendo usado para armazenar os resultados de banco de dados).

Considerando as variáveis de índice de linha inicial e o número máximo de linhas, uma implementação personalizada de paginação deve retornar somente o subconjunto exato de registros começando no índice de início de linha e até o número máximo de linhas de registros depois disso. Paginação personalizada fornece dois desafios:

- Podemos deve ser capazes de ser associado com eficiência um índice de linha a cada linha em todos os dados que está sendo paginados por meio de modo que podemos começar a retornar registros no índice de linha de início especificado
- É necessário fornecer o número total de registros sendo paginado por meio de

As próximas duas etapas, examinaremos o script SQL necessário para responder a esses dois desafios. Além do script SQL, também precisaremos implementar métodos na BLL e DAL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Etapa 2: Retornando o número Total de registros sendo paginado por meio de

Antes de examinar como recuperar o subconjunto exato de registros para a página que está sendo exibida, deixe s primeiro examinar como retornar o número total de registros sendo paginado por meio do. Essas informações são necessárias para configurar corretamente a interface do usuário de paginação. O número total de registros retornados por uma consulta SQL específica pode ser obtido usando o [ `COUNT` função de agregação](https://msdn.microsoft.com/library/ms175997.aspx). Por exemplo, para determinar o número total de registros no `Products` tabela, podemos usar a consulta a seguir:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Deixe o s adicionar um método a nossa DAL que retorna essas informações. Em particular, vamos criar um método DAL chamado `TotalNumberOfProducts()` que executa o `SELECT` instrução mostrada acima.

Comece abrindo o `Northwind.xsd` arquivo de conjunto de dados tipado no `App_Code/DAL` pasta. Em seguida, clique duas vezes no `ProductsTableAdapter` no Designer e escolha Add Query. Como podemos ve visto nos tutoriais anteriores, isso nos permitirá adicionar um novo método para o DAL que, quando invocada, executará uma determinada instrução SQL ou procedimento armazenado. Assim como acontece com nossos métodos TableAdapter nos tutoriais anteriores, por esse motivo optar por usar uma instrução de SQL ad hoc.


![Usar uma instrução de SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Figura 1**: usar uma instrução de SQL Ad Hoc


Na próxima tela, podemos especificar que tipo de consulta para criar. Uma vez que essa consulta retornará um único valor escalar o número total de registros na `Products` escolha tabela o `SELECT` que retorna uma opção de valor único.


![Configure a consulta para usar uma instrução SELECT que retorna um único valor](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Figura 2**: Configure a consulta para usar uma instrução SELECT que retorna um único valor


Depois que indica o tipo de consulta a ser usada, podemos deve, em seguida, especifique a consulta.


![Use o COUNT(*) SELECT da consulta de produtos](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Figura 3**: Use a contagem de SELECT (\*) FROM produtos consulta


Por fim, especifique o nome do método. Como mencionado anteriormente, vou s usar `TotalNumberOfProducts`.


![Nomeie o método DAL TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Figura 4**: Nomeie o método DAL TotalNumberOfProducts


Depois de clicar em Concluir, o assistente adicionará o `TotalNumberOfProducts` método para o DAL. Os métodos de retornados escalares no DAL retornam tipos anuláveis, no caso do resultado da consulta SQL `NULL`. Nossos `COUNT` consulta, no entanto, sempre retornará um não -`NULL` valor; independentemente disso, o método DAL retorna um inteiro anulável.

Além do método DAL, também é necessário um método na BLL. Abra o `ProductsBLL` arquivo de classe e adicione uma `TotalNumberOfProducts` método simplesmente chama para baixo para o s DAL `TotalNumberOfProducts` método:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

S o DAL `TotalNumberOfProducts` método retorna um inteiro anulável; no entanto, podemos ve criado as `ProductsBLL` classe s `TotalNumberOfProducts` , de modo que ele retorna um inteiro de padrão. Portanto, é preciso ter o `ProductsBLL` classe s `TotalNumberOfProducts` método retorna a parte do valor do inteiro que permite valor nulo retornado por s a DAL `TotalNumberOfProducts` método. A chamada para `GetValueOrDefault()` retorna o valor do inteiro que permite valor nulo, se existir; se o inteiro é `null`, no entanto, ele retorna o valor de inteiro de padrão, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Etapa 3: Retornando o subconjunto exato de registros

Nossa próxima tarefa é criar métodos na BLL que aceitam o índice de linha de início e DAL e variáveis de máximo de linhas discutido anteriormente e retornam os registros apropriados. Antes de fazer isso, let s primeiro examinar o script SQL necessário. A desafio que enfrenta a nós é que podemos deve ser capazes de atribuir com eficiência um índice para cada linha em todos os resultados sendo paginado por meio de modo que podemos pode retornar apenas os registros começando no índice de linha de início (e até o número máximo de registros de registros).

Isso não é um desafio se já houver uma coluna na tabela de banco de dados que serve como um índice de linha. À primeira vista, pode pensar que o `Products` tabela s `ProductID` campo seria suficiente, como o primeiro produto tem `ProductID` de 1, o segundo um 2, e assim por diante. No entanto, a exclusão de um produto deixa uma lacuna na sequência, anulando essa abordagem.

Há duas técnicas gerais usadas para associar com eficiência um índice de linha com os dados para a página, permitindo que o subconjunto exato de registros a serem recuperados:

- **Usando o SQL Server 2005 s `ROW_NUMBER()` palavra-chave** novo no SQL Server 2005, o `ROW_NUMBER()` palavra-chave associa uma classificação a cada registro retornado com base em alguma ordenação. Essa classificação pode ser usada como um índice de linha para cada linha.
- **Usando uma variável de tabela e `SET ROWCOUNT`**  s do SQL Server [ `SET ROWCOUNT` instrução](https://msdn.microsoft.com/library/ms188774.aspx) pode ser usado para especificar quantos registros total de uma consulta deve processar antes de encerrar; [variáveis de tabela](http://www.sqlteam.com/item.asp?ItemID=9454) são variáveis locais do T-SQL que podem conter dados tabulares, akin [tabelas temporárias](http://www.sqlteam.com/item.asp?ItemID=2029). Essa abordagem funciona igualmente bem com o Microsoft SQL Server 2005 e SQL Server 2000 (enquanto o `ROW_NUMBER()` abordagem funciona apenas com o SQL Server 2005).  
  
  A ideia aqui é criar uma variável de tabela que tem um `IDENTITY` coluna e colunas para as chaves primárias da tabela cujos dados está sendo paginados por meio do. Em seguida, o conteúdo da tabela cujos dados está sendo paginados por meio do são despejadas na variável de tabela, assim, associando um índice de linha sequenciais (via o `IDENTITY` coluna) para cada registro na tabela. Depois que a variável de tabela foi populada, um `SELECT` declaração sobre a variável de tabela unida com a tabela subjacente, pode ser executado para extrair os registros de determinado. O `SET ROWCOUNT` instrução é usada para inteligentemente limitar o número de registros que precisam ser despejadas na variável de tabela.  
  
  Essa eficiência da abordagem s baseia-se no número de página que está sendo solicitado, como o `SET ROWCOUNT` valor é atribuído o valor do índice de linha inicial mais o número máximo de linhas. Quando a paginação por meio das páginas numeradas de baixa, como o primeiro algumas páginas de dados, essa abordagem é muito eficiente. No entanto, ele exibe um padrão paginação desempenho ao recuperar uma página perto do fim.

Este tutorial implementa de personalizado de paginação usando o `ROW_NUMBER()` palavra-chave. Para obter mais informações sobre como usar a variável de tabela e `SET ROWCOUNT` técnica, consulte [método mais eficiente de um para paginação por meio de conjuntos de resultados grandes](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

O `ROW_NUMBER()` palavra-chave associadas a uma classificação com cada registro retornado em uma ordem específica usando a seguinte sintaxe:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` Retorna um valor numérico que especifica a classificação para cada registro com relação a ordem indicada. Por exemplo, para ver a classificação para cada produto, ordenado do mais caro para o menor poderia usamos a seguinte consulta:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Figura 5 mostra essa consulta resultados s quando executado por meio da janela de consulta no Visual Studio. Observe que os produtos são ordenados pelo preço, juntamente com uma classificação de preço para cada linha.


![A classificação de preço é incluída para cada registro retornado](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Figura 5**: A classificação de preço é incluída para cada registro retornado


> [!NOTE]
> `ROW_NUMBER()` apenas uma das muitas novas funções de classificação está disponível no SQL Server 2005. Para obter uma discussão mais completa da `ROW_NUMBER()`, juntamente com as outras funções de classificação, leia [retornando resultados classificados com o Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Quando os resultados de classificação por especificado `ORDER BY` coluna o `OVER` cláusula (`UnitPrice`, no exemplo acima), SQL Server deve classificar os resultados. Isso é uma operação rápida se houver um índice clusterizado pela coluna (s) os resultados estão sendo ordenados por, ou se há uma cobertura de índice, mas pode ser mais caro caso contrário. Para ajudar a melhorar o desempenho de consultas suficientemente grandes, considere a adição de um índice não clusterizado para a coluna pela qual os resultados são ordenados por. Ver [funções de classificação e o desempenho no SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obter uma visão mais detalhada das considerações de desempenho.

As informações de classificação retornadas por `ROW_NUMBER()` não pode ser usado diretamente no `WHERE` cláusula. No entanto, uma tabela derivada pode ser usada para retornar os `ROW_NUMBER()` resultado, que, em seguida, pode aparecer no `WHERE` cláusula. Por exemplo, a consulta a seguir usa uma tabela derivada para retornar as colunas ProductName e UnitPrice, juntamente com o `ROW_NUMBER()` resultados e, em seguida, usa um `WHERE` cláusula para retornar apenas os produtos cuja classificação preço está entre 11 e 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Estender esse conceito um pouco mais além, podemos pode utilizar essa abordagem para recuperar uma página específica de dados, considerando os valores de índice de linha inicial e o número máximo de linhas desejados:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Como veremos mais adiante neste tutorial, o *`StartRowIndex`* fornecido pelo ObjectDataSource é indexado começando com zero, enquanto o `ROW_NUMBER()` valor retornado pelo SQL Server 2005 é indexado começando em 1. Portanto, o `WHERE` cláusula retorna os registros em que `PriceRank` estritamente maior que *`StartRowIndex`* e menor ou igual a *`StartRowIndex`*  +  *`MaximumRows`*.


Agora que estamos ve discutido como `ROW_NUMBER()` pode ser usada para recuperar uma determinada página de dados os valores de índice de linha inicial e o número máximo de linhas de dados, agora, precisamos implementar esta lógica como métodos na BLL e DAL.

Ao criar essa consulta que devemos decidir a ordem pela qual os resultados serão classificados; Deixe o s classificar os produtos por seu nome em ordem alfabética. Isso significa que com a implementação de paginação personalizada neste tutorial, não será capaz de criar um relatório paginado personalizado que também podem ser classificados. No próximo tutorial, no entanto, veremos como essa funcionalidade pode ser fornecida.

Na seção anterior, criamos o método DAL como uma instrução de SQL ad hoc. Infelizmente, o analisador de T-SQL no Visual Studio usada pelo TableAdapter Assistente t, como o `OVER` sintaxe usada pelo `ROW_NUMBER()` função. Portanto, devemos criar esse método DAL como um procedimento armazenado. Selecione o Gerenciador de servidores do menu Exibir (ou ocorrências Ctrl + Alt + S) e expanda o `NORTHWND.MDF` nó. Para adicionar um novo procedimento armazenado, clique com botão direito no nó de procedimentos armazenados e escolha Adicionar um novo procedimento armazenado (veja a Figura 6).


![Adicionar um novo procedimento armazenado para paginação por meio de produtos](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Figura 6**: adicionar um novo procedimento armazenado para paginação por meio de produtos


Esse procedimento armazenado deve aceitar dois parâmetros de entrada de inteiro - `@startRowIndex` e `@maximumRows` e usar o `ROW_NUMBER()` função ordenados pela `ProductName` campo, retornando apenas as linhas maior que o especificado `@startRowIndex` e menor que ou igual a `@startRowIndex`  +  `@maximumRow` s. Insira o seguinte script para o novo procedimento armazenado e, em seguida, clique no ícone de salvar para adicionar o procedimento armazenado no banco de dados.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Depois de criar o procedimento armazenado, reserve um tempo para testá-lo. Clique com botão direito no `GetProductsPaged` procedimento armazenado nome no Gerenciador de servidores e escolha a opção de executar. Visual Studio então solicitará os parâmetros de entrada `@startRowIndex` e `@maximumRow` s (consulte a Figura 7). Tente valores diferentes e examinar os resultados.


![Insira um valor para o @startRowIndex e @maximumRows parâmetros](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Figura 7</strong>: insira um valor para o @startRowIndex e @maximumRows parâmetros


Depois de escolher esses valores de parâmetros de entrada, a janela de saída mostrará os resultados. Figura 8 mostra os resultados quando passando 10 para ambos os `@startRowIndex` e `@maximumRows` parâmetros.


[![Os registros que apareceriam na segunda página de dados são retornados](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Figura 8**: os registros que apareceriam na segunda página de dados são retornados ([clique para exibir a imagem em tamanho normal](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Com esse procedimento armazenado criado, podemos está pronto para criar o `ProductsTableAdapter` método. Abra o `Northwind.xsd` tipada DataSet, o botão direito do mouse no `ProductsTableAdapter`e escolha a opção de adicionar consulta. Em vez de criar a consulta usando uma instrução SQL ad hoc, crie-o usando um procedimento armazenado existente.


![Crie o método DAL usando um procedimento armazenado existente](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Figura 9**: criar o método DAL usando um procedimento armazenado existente


Em seguida, podemos serão solicitados a selecionar o procedimento armazenado para invocar. Escolher o `GetProductsPaged` procedimento armazenado a partir da lista suspensa.


![Escolha o GetProductsPaged procedimento armazenado a partir da lista suspensa](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Figura 10**: escolha o GetProductsPaged procedimento armazenado a partir da lista suspensa


A próxima tela, em seguida, solicita que o tipo de dados é retornado pelo procedimento armazenado: dados tabulares, um único valor ou nenhum valor. Uma vez que o `GetProductsPaged` procedimento armazenado pode retornar vários registros, indique que ela retorna dados tabulares.


![Indicar que o procedimento armazenado retorna dados tabulares](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Figura 11**: indicar que o procedimento armazenado retorna dados tabulares


Por fim, indicam os nomes dos métodos que você deseja criar. Assim como acontece com nossos tutoriais anteriores, vá em frente e crie métodos usando os dois o preenchimento uma DataTable e retornar uma DataTable. Nomeie o primeiro método `FillPaged` e o segundo `GetProductsPaged`.


![Nome FillPaged os métodos e GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Figura 12**: nome FillPaged os métodos e GetProductsPaged


Além ao criar um método DAL para retornar uma página específica de produtos, também precisamos fornecer tal funcionalidade na BLL. Como o método DAL, o s BLL GetProductsPaged método deve aceitar duas entradas de inteiro para especificar o índice de linha inicial e o número máximo de linhas e deve retornar apenas os registros que se enquadram dentro do intervalo especificado. Crie tal método BLL na classe ProductsBLL que simplesmente chamadas para baixo em s DAL método GetProductsPaged, desta forma:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Você pode usar qualquer nome para os parâmetros de entrada do método s do BLL, mas, como veremos daqui a pouco, optando por usar `startRowIndex` e `maximumRows` nos poupa de um arquivo extra pouco de trabalho ao configurar um ObjectDataSource para usar esse método.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Etapa 4: Configurando o ObjectDataSource para usar a paginação personalizada

Com os métodos BLL e DAL para acessar um subconjunto específico de registros completos, está pronto para criar um GridView controlamos que as páginas por meio de seus registros subjacentes usando a paginação personalizada. Comece abrindo o `EfficientPaging.aspx` página o `PagingAndSorting` pasta, adicione um controle GridView à página e configurá-lo para usar um novo controle ObjectDataSource. Em nossos tutoriais anteriores, muitas vezes tínhamos o ObjectDataSource configurado para usar o `ProductsBLL` classe s `GetProducts` método. Neste momento, no entanto, queremos usar o `GetProductsPaged` método em vez disso, desde o `GetProducts` método retorna *todos os* dos produtos no banco de dados, enquanto `GetProductsPaged` retorna apenas um subconjunto específico de registros.


![Configurar o ObjectDataSource para usar o método de GetProductsPaged ProductsBLL classe s](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Figura 13**: configurar o ObjectDataSource para usar o método de GetProductsPaged ProductsBLL classe s


Desde que estamos criando um GridView somente leitura, dedique uns momentos para definir a lista suspensa de método em INSERT, UPDATE e excluir guias como (nenhum).

Em seguida, o assistente ObjectDataSource nos solicita as fontes do `GetProductsPaged` método s `startRowIndex` e `maximumRows` valores de parâmetros de entrada. Esses parâmetros de entrada serão realmente definidos pelo GridView automaticamente, então, simplesmente deixar o conjunto de origem como None e em Concluir.


![Deixe as fontes de parâmetro de entrada como None](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Figura 14**: deixar as fontes de parâmetro de entrada como None


Depois de concluir o assistente ObjectDataSource, o GridView conterá uma BoundField ou CheckBoxField para cada um dos campos de dados do produto. Fique à vontade personalizar a aparência de s GridView conforme necessário. Eu ve optou por exibir somente o `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, e `UnitPrice` BoundFields. Além disso, configure o GridView para dar suporte à paginação, marcando a caixa de seleção Habilitar paginação em sua marca inteligente. Após essas alterações, a marcação declarativa GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Se você visitar a página por meio de um navegador, no entanto, o GridView não está em nenhum local a ser localizada.


![O GridView não está exibidos](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Figura 15**: O controle GridView não é exibidos


O GridView está ausente, porque o ObjectDataSource está usando 0 como os valores para ambas as `GetProductsPaged` `startRowIndex` e `maximumRows` parâmetros de entrada. Portanto, a consulta SQL resultante não é retornar nenhum registro e, portanto, não é exibido o GridView.

Para corrigir isso, precisamos configurar o ObjectDataSource para usar a paginação personalizada. Isso pode ser feito nas etapas a seguir:

1. **Definir o s ObjectDataSource `EnablePaging` propriedade para `true`**  isso indica para o ObjectDataSource que ele deve passar para o `SelectMethod` dois parâmetros adicionais: uma para especificar o índice de linha de início ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) e uma para especificar o número máximo de linhas ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Definir o s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` propriedades adequadamente** o `StartRowIndexParameterName` e `MaximumRowsParameterName` propriedades indicam os nomes dos parâmetros de entrada passados para o `SelectMethod` para fins de paginação personalizada. Por padrão, esses nomes de parâmetro são `startIndexRow` e `maximumRows`, que é por isso que, ao criar o `GetProductsPaged` método na BLL, usei esses valores para os parâmetros de entrada. Se você optar por usar nomes de parâmetro diferentes para o s BLL `GetProductsPaged` método, como `startIndex` e `maxRows`, por exemplo, você precisará define o s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` propriedades adequadamente (como startIndex para `StartRowIndexParameterName` e maxRows para `MaximumRowsParameterName`).
3. **Definir o s ObjectDataSource [ `SelectCountMethod` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) para o nome do método que retorna o Total número de registros que está sendo paginado por meio de (`TotalNumberOfProducts`)** Lembre-se de que o `ProductsBLL` classe s `TotalNumberOfProducts`método retorna o número total de registros sendo paginado por meio de um método DAL que executa um `SELECT COUNT(*) FROM Products` consulta. Essas informações são necessárias pelo ObjectDataSource para renderizar corretamente a interface de paginação.
4. **Remover o `startRowIndex` e `maximumRows` `<asp:Parameter>` elementos de marcação declarativa s ObjectDataSource** ao configurar o assistente ObjectDataSource, Visual Studio automaticamente adicionado duas `<asp:Parameter>` elementos para o `GetProductsPaged` método s parâmetros de entrada. Definindo `EnablePaging` à `true`, esses parâmetros serão passados automaticamente; se elas também aparecerão na sintaxe declarativa, o ObjectDataSource tenta passar *quatro* parâmetros para o `GetProductsPaged` método e dois parâmetros para o `TotalNumberOfProducts` método. Se você esquecer para removê-las `<asp:Parameter>` elementos, ao visitar a página por meio de um navegador, você receberá uma mensagem de erro como: *ObjectDataSource 'ObjectDataSource1' não foi possível encontrar um método não genérico 'TotalNumberOfProducts' que tem parâmetros: startRowIndex, maximumRows*.

Depois de fazer essas alterações, a sintaxe declarativa do ObjectDataSource s deve ser semelhante ao seguinte:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Observe que o `EnablePaging` e `SelectCountMethod` propriedades foram definidas e o `<asp:Parameter>` elementos foram removidos. A Figura 16 mostra uma captura de tela da janela Propriedades depois de fazer essas alterações.


![Para usar a paginação personalizada, Configure o controle ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Figura 16**: para usar a paginação personalizada, Configure o controle ObjectDataSource


Depois de fazer essas alterações, visite esta página por meio de um navegador. Você deve ver 10 produtos listados, classificados em ordem alfabética. Reserve um tempo para percorrer os dados em uma página por vez. Embora não haja nenhuma diferença visual da perspectiva do usuário final de s entre a paginação padrão e a paginação personalizada, paginação personalizada com mais eficiência páginas por meio de grandes quantidades de dados, como ele recupera somente os registros que precisam ser exibido para uma determinada página.


[![Os dados, ordenado pelo produto s nome, é paginado usando personalizado paginação](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Figura 17**: dados do, ordenado pelo produto s nome, é paginado usando personalizado paginação ([clique para exibir a imagem em tamanho normal](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Contagem de valor retornado por s o ObjectDataSource com paginação personalizada, a página `SelectCountMethod` é armazenado no estado de exibição GridView s. Outras variáveis de GridView a `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` coleção e assim por diante são armazenados no *estado do controle*, que é mantido independentemente do valor s o GridView `EnableViewState` propriedade. Uma vez que o `PageCount` valor é mantido em postagens usando o estado de exibição, ao usar uma interface de paginação que inclui um link para levar você a última página, é imperativo que o estado de exibição GridView s ser habilitada. (Se a sua interface de paginação não incluir um link direto para a última página, em seguida, você poderá desabilitar o estado de exibição.)


Clicando no link de página última causa um postback e instrui o GridView para atualizar seu `PageIndex` propriedade. Se o último link de página é clicado, o GridView atribui seu `PageIndex` propriedade para um valor em um menor do que seu `PageCount` propriedade. Com o estado de exibição desabilitado, o `PageCount` valor é perdido em postagens e o `PageIndex` é atribuído o valor máximo inteiro em vez disso. Em seguida, o GridView tenta determinar o índice de linha inicial multiplicando-se a `PageSize` e `PageCount` propriedades. Isso resulta em um `OverflowException` desde que o produto excede o tamanho máximo permitido de inteiros.

## <a name="implement-custom-paging-and-sorting"></a>Paginação personalizada de implementar e classificação

Nossa implementação de paginação personalizada atual requer que a ordem pela qual os dados são paginados por meio de ser estaticamente especificada ao criar o `GetProductsPaged` procedimento armazenado. No entanto, você pode ter anotado que a marca inteligente do GridView s contém uma caixa de seleção Habilitar classificação além da opção de habilitar a paginação. Infelizmente, adicionando suporte à classificação a GridView com nossa implementação de paginação personalizada atual será apenas classificar os registros na página de dados exibido no momento. Por exemplo, se você configurar o GridView para também dar suporte à paginação e em seguida, ao exibir a primeira página de dados, classificar por nome de produto em ordem decrescente, ele será inverter a ordem dos produtos na página 1. Como mostra a Figura 18, como mostra o Camarões da Malásia como o produto primeiro ao classificar em ordem alfabética inversa, que ignora os 71 outros produtos que vêm depois Camarões da Malásia, em ordem alfabética; somente os registros na primeira página são considerados na classificação.


[![Somente os dados mostrados na página atual é classificado](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Figura 18**: somente os dados mostrados na página atual é classificado ([clique para exibir a imagem em tamanho normal](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


A classificação só se aplica à página atual de dados porque a classificação está ocorrendo após os dados terem sido recuperados de s BLL `GetProductsPaged` método e esse método retorna apenas os registros para a página específica. Para implementar a classificação corretamente, precisamos passar a expressão de classificação para o `GetProductsPaged` método para que os dados podem ser classificados adequadamente antes de retornar a página específica de dados. Veremos como fazer isso em nosso próximo tutorial.

## <a name="implementing-custom-paging-and-deleting"></a>Implementação personalizada de paginação e excluindo

Se você habilitar a funcionalidade de exclusão em um GridView cujos dados são paginados usando técnicas de paginação personalizada, você descobrirá que ao excluir o último registro na última página do GridView desaparece em vez de diminuição adequadamente o s GridView `PageIndex`. Para reproduzir esse bug, habilite a exclusão para o tutorial apenas que acabamos de criar. Vá para a última página (página 9), onde você verá um único produto, pois nós são de paginação por meio de 81 produtos, 10 produtos por vez. Exclua este produto.

Depois de excluir o último produto, GridView *deve* automaticamente ir para a página oitava e essa funcionalidade é apresentada com paginação padrão. Com a paginação personalizada, no entanto, depois de excluir o último produto na última página, GridView simplesmente desaparece da tela completamente. O motivo exato *por que* isso acontecer é um pouco além do escopo deste tutorial, consulte [excluindo o último registro na última página de um GridView com paginação personalizada](http://scottonwriting.net/sowblog/posts/7326.aspx) para os detalhes de baixo nível sobre a origem do Esse problema. Em resumo-s devido a seguinte sequência de etapas que são executadas pelo GridView quando o botão Excluir é clicado:

1. Excluir o registro
2. Obter os registros apropriados para exibir especificado `PageIndex` e `PageSize`
3. Verifique se o `PageIndex` não exceda o número de páginas de dados na fonte de dados; se ele automaticamente, decrementar o GridView s `PageIndex` propriedade
4. Vincular a página apropriada de dados a GridView usando os registros obtidos na etapa 2

O problema tem origem no fato de que na etapa 2 a `PageIndex` usado quando pegar os registros a serem exibidos ainda é o `PageIndex` da última página cujo registro único apenas foi excluído. Portanto, na etapa 2, *nenhum* registros são retornados, pois essa última página de dados não contém nenhum registro. Em seguida, na etapa 3, GridView percebe que seu `PageIndex` propriedade é maior que o número total de páginas na fonte de dados (desde que criamos ve excluiu o último registro na última página) e, portanto, diminui sua `PageIndex` propriedade. Na etapa 4 GridView tenta se vincular aos dados recuperados na etapa 2; No entanto, na etapa 2, não há registros foram retornados, resultando em um GridView vazio. Com a paginação padrão, esse t do problema surgir porque na etapa 2 *todos os* registros são recuperados da fonte de dados.

Para corrigir isso, temos duas opções. A primeira é criar um manipulador de eventos para o s GridView `RowDeleted` manipulador de eventos que determina quantos registros foram exibido na página que acabou de ser excluída. Se havia apenas um registro, em seguida, o registro que acabou de ser excluído deve ter sido o último deles e precisamos decrementar o GridView s `PageIndex`. Obviamente, só queremos atualizar o `PageIndex` se a operação de exclusão foi realmente bem-sucedida, que pode ser determinado, garantindo que o `e.Exception` é de propriedade `null`.

Essa abordagem funciona porque ele atualiza o `PageIndex` após a etapa 1, mas antes da etapa 2. Portanto, na etapa 2, o conjunto apropriado de registros é retornado. Para fazer isso, use o código semelhante ao seguinte:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Uma solução alternativa é criar um manipulador de eventos para o s ObjectDataSource `RowDeleted` eventos e definir o `AffectedRows` propriedade para um valor de 1. Depois de excluir o registro na etapa 1 (mas antes de recuperar novamente os dados na etapa 2), o GridView atualiza seu `PageIndex` propriedade se uma ou mais linhas foram afetadas pela operação. No entanto, o `AffectedRows` não está definida pelo ObjectDataSource e, portanto, essa etapa for omitida. Uma maneira de ter essa etapa executada é definir manualmente o `AffectedRows` propriedade se a operação de exclusão for concluída com êxito. Isso pode ser feito usando código semelhante ao seguinte:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

O código para ambos esses manipuladores de eventos pode ser encontrado na classe code-behind do `EfficientPaging.aspx` exemplo.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Comparando o desempenho da paginação personalizada e padrão

Uma vez que a paginação personalizada recupera somente os registros necessários, enquanto que a paginação padrão retorna *todos os* os registros de cada página que está sendo visualizado, ele claro que a paginação personalizada é mais eficiente do que a paginação padrão. No entanto, somente como é muito mais eficiente é a paginação personalizada? Que tipo de ganhos de desempenho pode ser visto ao mover de paginação padrão para a paginação personalizada?

Infelizmente, s não existe nenhuma solução única todos responder aqui. O ganho de desempenho depende de vários fatores, mais proeminente dois sendo o número de pager por meio de registros e a carga sobre os canais de comunicação e o servidor de banco de dados entre o servidor web e o servidor de banco de dados. Para tabelas pequenas com apenas alguns dezenas registros, a diferença de desempenho pode ser insignificante. No entanto, para tabelas grandes, com milhares a centenas de milhares de linhas, a diferença de desempenho é importante.

Um artigo de meu, [paginação personalizada no ASP.NET 2.0 com o SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contém alguns testes de desempenho que executei para apresentar as diferenças no desempenho entre essas duas técnicas de paginação quando a paginação por meio de uma tabela de banco de dados com 50.000 registros. Nesses testes, eu examinei o tempo para executar a consulta no nível do SQL Server (usando [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) e a página do ASP.NET usando [recursos de rastreamento do ASP.NET s](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenha em mente que esses testes foram executados em minha caixa de desenvolvimento com um único usuário Active Directory e, portanto, são não-científico e imitar os padrões de carga site típico. Independentemente disso, os resultados ilustram as diferenças relativas no tempo de execução para paginação personalizada e padrão, ao trabalhar com suficientemente grandes quantidades de dados.


|  | **Média Duração (s)** | **Leituras** |
| --- | --- | --- |
| **Padrão paginação SQL Profiler** | 1.411 | 383 |
| **Profiler SQL de paginação personalizada** | 0.002 | 29 |
| **Rastreamento do ASP.NET de paginação de padrão** | 2.379 | *N/D* |
| **Rastreamento de ASP.NET paginação personalizada** | 0.029 | *N/D* |


Como você pode ver, recuperando uma determinada página de dados necessário 354 menos leituras em média e concluída em uma fração do tempo. Na página ASP.NET, personalizado a página foi capaz de renderizar em perto de 1/100<sup>ésimo</sup> do tempo necessário ao usar a paginação padrão. Ver [meu artigo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) para obter mais informações sobre esses resultados juntamente com o código e um banco de dados, você pode baixar para reproduzir esses testes em seu próprio ambiente.

## <a name="summary"></a>Resumo

Paginação padrão é muito fácil de implementar verificação apenas a caixa de seleção Habilitar paginação na marca inteligente de dados da Web controle s, mas tal simplicidade ocorre às custas do desempenho. Com a paginação padrão, quando um usuário solicita qualquer página de dados *todos os* registros são retornados, mesmo que apenas uma pequena fração deles pode ser mostrada. Para combater essa sobrecarga de desempenho, o ObjectDataSource oferece uma alternativa paginação personalizada da opção de paginação.

Enquanto a paginação personalizada aprimora padrão problemas de desempenho de s de paginação, recuperando apenas os registros que precisam ser exibidos, ele s mais complicado de implementar a paginação personalizada. Primeiro, uma consulta deve ser escrita que corretamente (e com eficiência) acessa o subconjunto específico de registros solicitados. Isso pode ser feito de várias maneiras; a examinamos neste tutorial é usar o SQL Server 2005 s nova `ROW_NUMBER()` função para classificação de resultados e, em seguida, para retornar apenas os resultados de cuja classificação estiver dentro de um intervalo especificado. Além disso, precisamos adicionar um meio para determinar o número total de registros sendo paginado por meio do. Depois de criar esses métodos DAL e BLL, também é necessário configurar o ObjectDataSource para que ele pode determinar o número total de registros estão sendo paginados por meio e corretamente podem passar os valores de índice de linha inicial e o número máximo de linhas para a BLL.

Enquanto Implementando a paginação personalizada requer várias etapas e é não quase tão simple quanto paginação padrão, a paginação personalizada é uma necessidade quando a paginação por meio de suficientemente grandes quantidades de dados. Como os resultados examinado paginação mostrou, personalizada pode lançar uma segundos fora do tempo de renderização de página ASP.NET e pode reduzir a carga no servidor de banco de dados em um ou mais ordens de magnitude.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](paging-and-sorting-report-data-cs.md)
> [Próximo](sorting-custom-paged-data-cs.md)
