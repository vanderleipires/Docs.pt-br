---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: Paginação com eficiência grandes quantidades de dados (VB) | Microsoft Docs
author: rick-anderson
description: A opção de paginação de padrão de um controle de apresentação de dados é inadequada ao trabalhar com grandes quantidades de dados, como seu retriev de controle de fonte de dados subjacente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 00057f9bfd9b1c479e500ac591db694388a5d358
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890691"
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Paginação com eficiência grandes quantidades de dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) ou [baixar PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> A opção de paginação de padrão de um controle de apresentação de dados é inadequada ao trabalhar com grandes quantidades de dados, como o controle de fonte de dados subjacente recupera todos os registros, mesmo que apenas um subconjunto de dados é exibido. Em tais circunstâncias, deve desativamos personalizados para paginação.


## <a name="introduction"></a>Introdução

Conforme abordado no tutorial anterior, a paginação pode ser implementada em uma de duas maneiras:

- **Paginação padrão** pode ser implementada, simplesmente marcando a opção habilitar paginação nos dados de controle de Web s marca inteligente; no entanto, sempre que exibir uma página de dados, o ObjectDataSource recupera *todos os* dos registros, até mesmo Embora somente um subconjunto delas são exibidas na página
- **Paginação personalizada** melhora o desempenho de padrão de paginação, recuperando somente os registros do banco de dados que precisam ser exibidos para a página específica de dados solicitado pelo usuário; no entanto, paginação personalizada envolve um pouco mais esforço para implementar de paginação padrão

Devido à facilidade de implementação Verifique a caixa de seleção e você está concluído! paginação padrão é uma opção atraente. Sua abordagem de var na recuperar todos os registros, torna uma opção a ser inaceitável quando a paginação suficientemente grandes quantidades de dados ou para sites com muitos usuários simultâneos. Em tais circunstâncias, deve desativamos para personalizado para fornecer um sistema de resposta de paginação.

O desafio de paginação personalizada está sendo capaz de gravar uma consulta que retorna o conjunto exato de registros necessários para uma página específica de dados. Felizmente, Microsoft SQL Server 2005 oferece uma nova palavra-chave para resultados de classificação, que permite escrever uma consulta que pode recuperar o subconjunto apropriado de registros de maneira eficiente. Neste tutorial, veremos como usar essa nova palavra-chave do SQL Server 2005 para implementar a paginação personalizada em um controle GridView. Enquanto a interface do usuário para paginação personalizada é idêntica à paginação padrão, passo a passo de uma página para a próxima usando paginação personalizada pode ser mais rápido do que a paginação padrão várias ordens de magnitude.

> [!NOTE]
> O ganho de desempenho exato exibido pelos paginação personalizada depende do número total de pager por meio de registros e a carga no servidor de banco de dados. No final deste tutorial, examinaremos algumas métricas aproximadas que mostram os benefícios de desempenho obtido por meio de paginação personalizada.


## <a name="step-1-understanding-the-custom-paging-process"></a>Etapa 1: Noções básicas sobre o processo de paginação personalizada

Quando a paginação de dados, os registros precisos exibidos em uma página dependem da página de dados que está sendo solicitadas e o número de registros exibidos por página. Por exemplo, imagine que queremos ver 81 produtos, exibindo os 10 produtos por página. Ao exibir a primeira página, d queremos produtos 1 a 10; ao exibir a segunda página d que esteja interessado em produtos 20 por meio de 11 e assim por diante.

Há três variáveis que determinam quais registros devem ser recuperadas e como a interface de paginação deve ser renderizada:

- **Índice de linha de início** o índice da primeira linha na página de dados a serem exibidos; isso pode índice ser calculado multiplicando o índice de página de registros a serem exibidos por página e adição de um. Por exemplo, quando a paginação por meio de registros de 10 por vez, para a primeira página (cujo índice é 0), o índice de linha de início é 0 \* 10 + 1 ou 1; para a segunda página (cujo índice é 1), o índice de linha de início é 1 \* 10 + 1 , ou 11.
- **Máximo de linhas** o número máximo de registros a serem exibidos por página. Essa variável é chamada como o número máximo de linhas desde a última página pode ser menos registros retornados que o tamanho da página. Por exemplo, quando a paginação os registros de 81 produtos 10 por página, a página Nona e final terá apenas um registro. No entanto, nenhuma página mostrará mais registros que o valor máximo de linhas.
- **Contagem total de registro** o número total de registros por meio de pager. Enquanto essa variável não é necessária para determinar quais registros para recuperar de uma determinada página, ele determinam a interface de paginação. Por exemplo, se houver 81 sendo enviada via paginados por meio de produtos, a interface de paginação sabe para exibir números de página nove na interface do usuário de paginação.

Com paginação padrão, o índice de início de linha é calculado como o produto o índice de página e o tamanho da página, mais um, enquanto o máximo de linhas é simplesmente o tamanho da página. Desde que a paginação padrão recupera todos os registros de banco de dados durante a renderização de qualquer página de dados, o índice para cada linha é conhecido, tornando-se mover para a linha de índice de linha de iniciar uma tarefa simples. Além disso, estão disponível, como a contagem Total de registros s simplesmente o número de registros no DataTable (ou qualquer objeto que está sendo usado para armazenar os resultados de banco de dados).

Considerando as variáveis de iniciar o índice de linha e o número máximo de linhas, uma implementação de paginação personalizada deve retornar somente o subconjunto preciso de registros, começando no índice de início de linha e até o número máximo de linhas de registros depois que. Paginação personalizada fornece dois desafios:

- Nós deve ser capazes de associar com eficiência um índice de linha a cada linha em todos os dados sendo enviada via paginados por meio de forma que podemos começar a retornar registros no índice de linha de início especificado
- É preciso fornecer o número total de registros sendo enviada via pager por meio de

As próximas duas etapas, examinaremos o script SQL necessário para responder a esses dois desafios. Além de script SQL, podemos também precisará implementar métodos na BLL e DAL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Etapa 2: Retornando o número Total de registros sendo enviada via pager por meio de

Antes de examinar como recuperar o subconjunto preciso de registros para a página que está sendo exibido, permitem que s primeiro examinar como retornar o número total de registros por meio de pager. Essas informações são necessárias para configurar corretamente a interface do usuário de paginação. O número total de registros retornados por uma consulta específica de SQL pode ser obtido usando o [ `COUNT` função de agregação](https://msdn.microsoft.com/library/ms175997.aspx). Por exemplo, para determinar o número total de registros de `Products` tabela, podemos usar a consulta a seguir:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Permitir que o s adicionar um método para nosso DAL que retorna essas informações. Em particular, vamos criar um método DAL chamado `TotalNumberOfProducts()` que executa o `SELECT` instrução mostrada acima.

Comece abrindo o `Northwind.xsd` arquivo de conjunto de dados tipado no `App_Code/DAL` pasta. Em seguida, clique duas vezes no `ProductsTableAdapter` no Designer e escolha Adicionar consulta. Como podemos ve visto nos tutoriais anteriores, isso nos permitirá adicionar um novo método para a DAL que, quando chamado, executará uma determinada instrução SQL ou procedimento armazenado. Assim como acontece com nossos métodos TableAdapter nos tutoriais anteriores, para esta optar por usar uma instrução de SQL ad hoc.


![Use uma instrução de SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Figura 1**: usar uma instrução de SQL Ad Hoc


Na próxima tela, podemos especificar o tipo de consulta para criar. Desde que essa consulta retorna um único valor escalar o número total de registros a `Products` Escolher tabela de `SELECT` que retorna uma opção de valor único.


![Configure a consulta para usar uma instrução SELECT que retorna um único valor](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Figura 2**: Configure a consulta para usar uma instrução SELECT que retorna um único valor


Depois que indica o tipo de consulta para usar, podemos deve, em seguida, especifique a consulta.


![Use o COUNT(*) SELECT da consulta de produtos](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Figura 3**: usam a contagem de SELECT (\*) de consulta de produtos


Por fim, especifique o nome do método. Como mencionado anteriormente, permitem s usar `TotalNumberOfProducts`.


![Nome do método DAL TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Figura 4**: nome do método DAL TotalNumberOfProducts


Depois de clicar em Concluir, o assistente adicionará o `TotalNumberOfProducts` método DAL. Os métodos de retorno escalares em DAL retornam tipos anuláveis, no caso do resultado da consulta SQL é `NULL`. Nosso `COUNT` consulta, no entanto, sempre retornará não`NULL` valor; independentemente, o método DAL retorna um inteiro anulável.

Além do método DAL, também é necessário um método na BLL. Abra o `ProductsBLL` arquivo de classe e adicione um `TotalNumberOfProducts` método simplesmente chama para baixo para o s DAL `TotalNumberOfProducts` método:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

A DAL s `TotalNumberOfProducts` método retorna um inteiro anulável; no entanto, podemos ve criado o `ProductsBLL` classe s `TotalNumberOfProducts` método para que ele retorna um inteiro padrão. Portanto, é preciso ter o `ProductsBLL` classe s `TotalNumberOfProducts` método retorna a parte do valor do inteiro permite valor nulo retornado por s a DAL `TotalNumberOfProducts` método. A chamada para `GetValueOrDefault()` retorna o valor do inteiro anulável, se existir; se o inteiro anulável é `null`, no entanto, ela retorna o valor de inteiro padrão, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Etapa 3: Retornando o subconjunto preciso de registros

Nossa próxima tarefa é criar métodos no DAL e BLL que aceitam o índice de linha de início e variáveis de máximo de linhas discutido anteriormente e retornam os registros adequados. Antes de fazermos isso, vamos s primeiro examinar o script necessário do SQL. O desafio voltada para nós é que podemos deve ser capazes de atribuir um índice para cada linha em todos os resultados sejam chamados por meio de forma que podemos pode retornar apenas os registros iniciando no índice de linha de início (e até o número máximo de registros de registros).

Isso não é um desafio se já houver uma coluna na tabela de banco de dados que serve como um índice de linha. À primeira vista pode achamos que o `Products` tabela s `ProductID` campo seria suficiente, como o primeiro produto tem `ProductID` de 1, 2, o segundo e assim por diante. No entanto, a exclusão de um produto deixa uma lacuna na sequência, anulando essa abordagem.

Existem duas técnicas gerais usadas para associar um índice de linha com eficiência os dados para a página, habilitando o subconjunto preciso de registros a serem recuperados:

- **Usando o SQL Server 2005 s `ROW_NUMBER()` palavra-chave** novo para o SQL Server 2005, o `ROW_NUMBER()` palavra-chave associa uma classificação a cada registro retornado com base na ordenação. Essa classificação pode ser usada como um índice de linha para cada linha.
- **Usando uma variável de tabela e `SET ROWCOUNT`**  s do SQL Server [ `SET ROWCOUNT` instrução](https://msdn.microsoft.com/library/ms188774.aspx) pode ser usado para especificar o número total de registros deve processar uma consulta antes de encerrar; [variáveis de tabela](http://www.sqlteam.com/item.asp?ItemID=9454) são variáveis locais do T-SQL que podem conter dados tabulares, akin para [tabelas temporárias](http://www.sqlteam.com/item.asp?ItemID=2029). Essa abordagem funciona igualmente bem com o Microsoft SQL Server 2005 e SQL Server 2000 (enquanto o `ROW_NUMBER()` abordagem só funciona com o SQL Server 2005).  
  
  A ideia aqui é criar uma variável de tabela que tem um `IDENTITY` coluna e colunas para as chaves primárias da tabela cujos dados estão sendo enviada via pager por meio de. Em seguida, o conteúdo da tabela cujos dados estão sendo enviada via pager por meio de é despejada na variável de tabela, assim, a associação de um índice de linha sequenciais (por meio de `IDENTITY` coluna) para cada registro na tabela. Depois que a variável de tabela foi preenchida, um `SELECT` instrução na variável de tabela, associado à tabela subjacente, podem ser executadas para destacar os registros específico. O `SET ROWCOUNT` instrução é usada de forma inteligente limitar o número de registros que precisam ser despejada na variável de tabela.  
  
  Essa eficiência abordagem s é baseada no número de página que está sendo solicitado, como o `SET ROWCOUNT` será atribuído o valor de índice de linha iniciar com o máximo de linhas. Quando a paginação páginas numeradas baixa, como o primeiro algumas páginas de dados, essa abordagem é muito eficiente. No entanto, ele exibe o desempenho de paginação como padrão ao recuperar uma página no final.

Este tutorial implementa paginação personalizado usando o `ROW_NUMBER()` palavra-chave. Para obter mais informações sobre como usar a variável de tabela e `SET ROWCOUNT` técnica, consulte [método mais eficiente de um para paginação por meio de conjuntos de resultados grandes](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

O `ROW_NUMBER()` palavra-chave associadas a uma classificação com cada registro retornado em uma ordem específica usando a seguinte sintaxe:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` Retorna um valor numérico que especifica a classificação para cada registro em relação a ordem indicada. Por exemplo, para ver a classificação para cada produto, ordenado do mais caro para o menor, podemos usar a consulta a seguir:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

A Figura 5 mostra essa consulta resultados s quando executado através da janela de consulta no Visual Studio. Observe que os produtos são ordenados por preço, juntamente com uma classificação de preço para cada linha.


![A classificação de preço é incluída para cada registro retornado](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Figura 5**: A classificação de preço é incluída para cada registro retornado


> [!NOTE]
> `ROW_NUMBER()` apenas uma das muitas novas funções de classificação está disponível no SQL Server 2005. Para obter uma discussão mais completa de `ROW_NUMBER()`, junto com as outras funções de classificação, ler [retornando resultados com o Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Quando os resultados de classificação por especificado `ORDER BY` coluna o `OVER` cláusula (`UnitPrice`, no exemplo acima), SQL Server deve classificar os resultados. Isso é uma operação rápida se houver um índice clusterizado em colunas, os resultados estão sendo ordenados por, ou se houver uma cobertura de índice, mas pode ser mais caro de outra forma. Para ajudar a melhorar o desempenho de consultas suficientemente grandes, considere a adição de um índice não clusterizado para a coluna pela qual os resultados são ordenados por. Consulte [desempenho no SQL Server 2005 e funções de classificação](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obter informações mais detalhada sobre as considerações de desempenho.

As informações de classificação retornadas por `ROW_NUMBER()` não pode ser usado diretamente no `WHERE` cláusula. No entanto, uma tabela derivada pode ser usada para retornar o `ROW_NUMBER()` resultado, o que, em seguida, pode aparecer no `WHERE` cláusula. Por exemplo, a consulta a seguir usa uma tabela derivada para retornar as colunas ProductName e UnitPrice, juntamente com o `ROW_NUMBER()` resultados e, em seguida, usa um `WHERE` cláusula para retornar somente os produtos cuja posição de preço está entre 11 e 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Estender esse conceito um pouco mais, podemos utilizar essa abordagem para recuperar uma página específica de dados de acordo com os valores de índice de linha de início e o número máximo de linhas desejados:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Como veremos mais adiante neste tutorial, o *`StartRowIndex`* fornecido por ObjectDataSource é indexado começando com zero, enquanto o `ROW_NUMBER()` valor retornado pelo SQL Server 2005 é indexada começando em 1. Portanto, o `WHERE` cláusula retorna os registros onde `PriceRank` é estritamente maior que *`StartRowIndex`* e menor ou igual a *`StartRowIndex`*  +  *`MaximumRows`*.


Agora que possamos ve discutido como `ROW_NUMBER()` pode ser usada para recuperar uma página específica de acordo com os valores de índice de linha de início e o número máximo de linhas de dados, que agora precisamos implementar esta lógica como métodos na BLL e DAL.

Ao criar essa consulta, decida a ordem pela qual os resultados serão classificados; permitir que o s classificar os produtos por seu nome em ordem alfabética. Isso significa que com a implementação de paginação personalizada neste tutorial, não será capaz de criar um relatório paginado personalizado que também podem ser classificados. No tutorial de Avançar, porém, veremos como essa funcionalidade pode ser fornecida.

Na seção anterior, criamos o método DAL como uma instrução de SQL ad hoc. Infelizmente, o analisador de T-SQL no Visual Studio usado pelo TableAdapter Assistente t como o `OVER` sintaxe usada pelo `ROW_NUMBER()` função. Portanto, devemos criar esse método DAL como um procedimento armazenado. Selecione o Gerenciador de servidores do menu Exibir (ou ocorrências Ctrl + Alt + S) e expanda o `NORTHWND.MDF` nó. Para adicionar um novo procedimento armazenado, com o botão direito no nó de procedimentos armazenados e escolha Adicionar um novo procedimento armazenado (veja a Figura 6).


![Adicionar um novo procedimento armazenado para paginação por meio de produtos](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Figura 6**: adicionar um novo procedimento armazenado para paginação por meio de produtos


Esse procedimento armazenado deve aceitar dois parâmetros de entrada de inteiro - `@startRowIndex` e `@maximumRows` e usar o `ROW_NUMBER()` função ordenados pelo `ProductName` campo, retornando apenas as linhas superiores especificado `@startRowIndex` e menor que ou igual a `@startRowIndex`  +  `@maximumRow` s. Insira o seguinte script para o novo procedimento armazenado e, em seguida, clique no ícone de salvar para adicionar o procedimento armazenado no banco de dados.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Depois de criar o procedimento armazenado, dedique alguns momentos para testá-lo. Clique com botão direito no `GetProductsPaged` procedimento armazenado nome no Gerenciador de servidores e escolha a opção de executar. O Visual Studio solicitará que você para os parâmetros de entrada, `@startRowIndex` e `@maximumRow` s (consulte a Figura 7). Tente valores diferentes e examine os resultados.


![Insira um valor para o @startRowIndex e @maximumRows parâmetros](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>Figura 7</strong>: insira um valor para o @startRowIndex e @maximumRows parâmetros


Depois de escolher esses valores de parâmetros de entrada, a janela Saída exibirá os resultados. Figura 8 mostra os resultados quando passando 10 para ambos os `@startRowIndex` e `@maximumRows` parâmetros.


[![Os registros que seria exibido na segunda página de dados são retornados](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Figura 8**: os registros que seria exibido na segunda página de dados são retornados ([clique para exibir a imagem em tamanho normal](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Com esse procedimento armazenado criado, podemos re pronto para criar o `ProductsTableAdapter` método. Abra o `Northwind.xsd` conjunto de dados tipado, com o botão direito no `ProductsTableAdapter`e escolha a opção de adicionar consulta. Em vez de criar a consulta usando uma instrução SQL ad hoc, crie-o usando um procedimento armazenado existente.


![Crie o método DAL usando um procedimento armazenado existente](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Figura 9**: criar o método DAL usando um procedimento armazenado existente


Em seguida, podemos serão solicitados a selecionar o procedimento armazenado para invocar. Escolha o `GetProductsPaged` procedimento armazenado a partir da lista suspensa.


![Escolha o GetProductsPaged procedimento armazenado a partir da lista suspensa](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Figura 10**: escolha o GetProductsPaged procedimento armazenado a partir da lista suspensa


A próxima tela, em seguida, solicita que tipo de dados é retornado pelo procedimento armazenado: dados de tabela, um único valor ou nenhum valor. Desde o `GetProductsPaged` procedimento armazenado pode retornar vários registros, indique que ela retorna dados tabulares.


![Indicar que o procedimento armazenado retorna dados tabulares](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Figura 11**: indicar que o procedimento armazenado retorna dados tabulares


Por fim, indicam os nomes dos métodos que você deseja criar. Como com os tutoriais anteriores, vá em frente e crie métodos usando o preenchimento ambos um DataTable e retornar uma DataTable. Nomeie o primeiro método `FillPaged` e a segunda `GetProductsPaged`.


![Nome de métodos FillPaged e GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Figura 12**: nome de métodos FillPaged e GetProductsPaged


Além para criar um método DAL para retornar uma página específica de produtos, também precisamos fornecer essa funcionalidade na BLL. Como o método DAL, a s BLL GetProductsPaged método deve aceitar duas entradas de inteiro para especificar o número máximo de linhas e iniciar o índice de linha e deve retornar apenas os registros que estão dentro do intervalo especificado. Crie tal método BLL na classe ProductsBLL que simplesmente chamadas para baixo em s DAL método GetProductsPaged, da seguinte forma:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Você pode usar qualquer nome para os parâmetros de entrada do método s do BLL, mas, conforme veremos em breve, escolher usar `startRowIndex` e `maximumRows` economiza de um extra pouco de trabalho ao configurar um ObjectDataSource para usar esse método.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Etapa 4: Configurar o ObjectDataSource para usar a paginação personalizada

Com os métodos BLL e DAL para acessar um subconjunto específico de registros completos, podemos re pronto para criar um controle GridView controlar que as páginas em seus registros subjacentes usando a paginação personalizada. Comece abrindo o `EfficientPaging.aspx` página o `PagingAndSorting` pasta, adicionar um controle GridView à página e configurá-lo para usar um novo controle ObjectDataSource. Em nossos tutoriais anteriores, geralmente tínhamos ObjectDataSource configurado para usar o `ProductsBLL` classe s `GetProducts` método. Neste momento, no entanto, queremos usar o `GetProductsPaged` método em vez disso, desde o `GetProducts` método retorna *todos os* dos produtos no banco de dados enquanto `GetProductsPaged` retorna apenas um subconjunto específico de registros.


![Configurar o ObjectDataSource para usar o método de GetProductsPaged ProductsBLL classe s](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Figura 13**: configurar o ObjectDataSource para usar o método de GetProductsPaged ProductsBLL classe s


Desde que esteja criando um GridView somente leitura, reserve um tempo para definir a lista suspensa de método em INSERT, UPDATE e excluir guias como (nenhum).

Em seguida, o assistente ObjectDataSource solicitará nas fontes do `GetProductsPaged` método s `startRowIndex` e `maximumRows` valores de parâmetros de entrada. Esses parâmetros de entrada serão realmente definidos por GridView automaticamente, portanto simplesmente deixam o conjunto de origem como None e clique em Concluir.


![Deixar as fontes de parâmetro de entrada como None](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Figura 14**: deixar as fontes de parâmetro de entrada como None


Depois de concluir o assistente ObjectDataSource, o GridView conterá um BoundField ou CheckBoxField para cada um dos campos de dados de produto. Fique à vontade para personalizar a aparência do GridView s como desejar. Posso salvar optou por exibir somente o `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, e `UnitPrice` BoundFields. Além disso, configure o GridView para oferecer suporte a paginação, marcando a caixa de seleção Habilitar paginação em sua marca inteligente. Após essas alterações, a marcação declarativa GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Se você visitar a página por meio de um navegador, no entanto, GridView não está em nenhum local a ser localizado.


![GridView não é exibido](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Figura 15**: A GridView não é exibido


GridView está ausente, porque o ObjectDataSource está usando atualmente 0 como valores para ambos os `GetProductsPaged` `startRowIndex` e `maximumRows` parâmetros de entrada. Portanto, a consulta SQL resultante está retornando registros não e, portanto, não será exibido o GridView.

Para corrigir isso, é preciso configurar ObjectDataSource para usar a paginação personalizada. Isso pode ser feito nas etapas a seguir:

1. **Definir o s ObjectDataSource `EnablePaging` propriedade `true`**  indica a ObjectDataSource que ele deve passar para o `SelectMethod` dois parâmetros adicionais: uma para especificar o índice de linha de início ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) e uma para especificar o número máximo de linhas ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Definir o s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` propriedades adequadamente** o `StartRowIndexParameterName` e `MaximumRowsParameterName` propriedades indicam os nomes dos parâmetros de entrada passados para o `SelectMethod` para fins de paginação personalizada. Por padrão, esses nomes de parâmetro são `startIndexRow` e `maximumRows`, que é por isso que, ao criar o `GetProductsPaged` método na BLL, usei esses valores para os parâmetros de entrada. Se você optar por usar nomes de parâmetro diferentes para o s BLL `GetProductsPaged` método como `startIndex` e `maxRows`para o exemplo, você precisa define a s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` propriedades adequadamente (como startIndex para `StartRowIndexParameterName` e maxRows para `MaximumRowsParameterName`).
3. **Definir o s ObjectDataSource [ `SelectCountMethod` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) para o nome do método que retorna o Total número de registros sendo paginável a (`TotalNumberOfProducts`)** Lembre-se de que o `ProductsBLL` classe s `TotalNumberOfProducts`método retorna o número total de registros sendo enviada via pager usando um método DAL que executa um `SELECT COUNT(*) FROM Products` consulta. Essas informações são necessárias por ObjectDataSource para processar corretamente a interface de paginação.
4. **Remover o `startRowIndex` e `maximumRows` `<asp:Parameter>` elementos de marcação declarativa ObjectDataSource s** ao configurar o ObjectDataSource por meio do assistente, o Visual Studio automaticamente adicionados dois `<asp:Parameter>` elementos para o `GetProductsPaged` método s parâmetros de entrada. Definindo `EnablePaging` para `true`, esses parâmetros serão passados automaticamente; se elas também aparecerão na sintaxe declarativa, ObjectDataSource tentará passar *quatro* parâmetros para o `GetProductsPaged` método e dois parâmetros para o `TotalNumberOfProducts` método. Se você se esqueça de remover esses `<asp:Parameter>` elementos, quando visitar a página por meio de um navegador, você receberá uma mensagem de erro como: *ObjectDataSource 'ObjectDataSource1' não foi possível encontrar um método não genérico 'TotalNumberOfProducts' que tem parâmetros: startRowIndex, maximumRows*.

Depois de fazer essas alterações, a sintaxe declarativa de s ObjectDataSource deve parecer com o seguinte:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Observe que o `EnablePaging` e `SelectCountMethod` propriedades foram definidas e `<asp:Parameter>` elementos foram removidos. A Figura 16 mostra uma captura de tela da janela Propriedades depois que essas alterações foram feitas.


![Para usar a paginação personalizada, configurar o controle ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Figura 16**: para usar a paginação personalizada, configurar o controle ObjectDataSource


Depois de fazer essas alterações, visite esta página por meio de um navegador. Você deve ver os 10 produtos listados, classificados em ordem alfabética. Dedique alguns momentos para percorrer os dados em uma página por vez. Embora não haja nenhuma diferença visual sob a perspectiva do usuário final s entre paginação padrão e paginação personalizada, paginação personalizada com mais eficiência páginas com grandes quantidades de dados, ele recupera somente os registros que precisam ser exibidos para uma determinada página.


[![Os dados, ordenado pelo produto s nome, é paginado usando personalizado paginação](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Figura 17**: dados do, ordenado pelo produto s nome, é paginado usando personalizado paginação ([clique para exibir a imagem em tamanho normal](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> Contagem de valor retornado pela s ObjectDataSource com paginação personalizada, a página `SelectCountMethod` é armazenada no estado de exibição GridView s. Outras variáveis de GridView o `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` coleção e assim por diante são armazenados em *controlar o estado*, que é mantido independentemente do valor de GridView s `EnableViewState` propriedade. Desde o `PageCount` valor é mantido em postagens usando o estado de exibição, ao usar uma interface de paginação que inclui um link para levá-lo para a última página, é fundamental que o estado de exibição do GridView s ser habilitado. (Se a interface de paginação não incluir um link direto para a última página, em seguida, você poderá desabilitar o estado de exibição.)


Clique no link de página última causa um postback e instrui o GridView para atualizar seu `PageIndex` propriedade. Se o último link de página for selecionado, o GridView atribui seu `PageIndex` propriedade para um valor um menor do que seu `PageCount` propriedade. Com o estado de exibição desabilitado, o `PageCount` valor é perdido em postagens e `PageIndex` é atribuído o valor máximo inteiro em vez disso. Em seguida, GridView tentará determinar o índice da linha inicial, multiplicando o `PageSize` e `PageCount` propriedades. Isso resulta em um `OverflowException` desde que o produto excede o tamanho máximo permitido de inteiros.

## <a name="implement-custom-paging-and-sorting"></a>Implementação de paginação personalizada e classificação

Nossa implementação de paginação personalizada atual requer que a ordem pela qual os dados são paginados por meio de ser especificado estaticamente ao criar o `GetProductsPaged` procedimento armazenado. No entanto, você pode observou que a marca inteligente de s GridView contém uma caixa de seleção Habilitar classificação além da opção de habilitar a paginação. Infelizmente, a adição de suporte à classificação GridView com nossa implementação de paginação personalizada atual só classificará os registros na página de dados exibido no momento. Por exemplo, se você configurar o GridView para também oferecem suporte à paginação e, em seguida, ao exibir a primeira página de dados, classificar por nome em ordem decrescente, ele será inverter a ordem dos produtos na página 1. Como mostra a Figura 18, como mostra o Camarões da Malásia como o produto primeiro ao classificar em ordem alfabética inversa, que ignora os 71 outros produtos que vêm depois Camarões da Malásia, em ordem alfabética; somente os registros na primeira página são considerados na classificação.


[![Somente os dados mostrados na página atual for classificado](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Figura 18**: somente os dados mostrados na página atual é classificado ([clique para exibir a imagem em tamanho normal](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


A classificação só se aplica à página atual de dados como a classificação ocorre depois que os dados são recuperados de s BLL `GetProductsPaged` método e esse método retorna apenas os registros para a página específica. Para implementar a classificação corretamente, é preciso passar a expressão de classificação para o `GetProductsPaged` método para que os dados podem ser classificados adequadamente antes de retornar a página específica de dados. Veremos como fazer isso no nosso tutorial Avançar.

## <a name="implementing-custom-paging-and-deleting"></a>Implementação personalizada de paginação e excluindo

Se você habilitar a funcionalidade de exclusão em um GridView cujos dados são paginados usando técnicas personalizadas de paginação, você descobrirá que ao excluir o último registro de última página, GridView desaparece em vez de diminuição adequadamente o s GridView `PageIndex`. Para reproduzir esse bug, habilite a exclusão do tutorial apenas que acabamos de criar. Vá para a última página (9), onde você verá um único produto como nós são paginação 81 produtos, 10 produtos por vez. Exclua este produto.

Após a exclusão do último produto, GridView *devem* automaticamente ir para a página oitava e essa funcionalidade é apresentada com paginação padrão. Com a paginação personalizada, no entanto, após a exclusão do último produto na última página, GridView simplesmente desaparece da tela completamente. O motivo exato *por* isso acontece um bit está além do escopo deste tutorial, consulte [excluindo o último registro na última página de um GridView com paginação personalizada](http://scottonwriting.net/sowblog/posts/7326.aspx) para os detalhes de baixo nível sobre a origem do Esse problema. Em resumo-s devido a seguinte sequência de etapas que são executadas pelo GridView quando clicar no botão de exclusão:

1. Excluir o registro
2. Obter os registros apropriados para exibir especificado `PageIndex` e `PageSize`
3. Verifique se que o `PageIndex` não exceder o número de páginas de dados na fonte de dados; se ele automaticamente, diminuir o GridView s `PageIndex` propriedade
4. Vincular a página apropriada de dados a GridView usando os registros obtidos na etapa 2

O problema deriva do fato de que na etapa 2 do `PageIndex` usado quando a captura de registros a serem exibidos ainda é o `PageIndex` da última página cujo registro único apenas foi excluído. Portanto, na etapa 2, *sem* registros são retornados desde a última página de dados não contém quaisquer registros. Em seguida, na etapa 3, GridView percebe que seu `PageIndex` propriedade é maior do que o número total de páginas na fonte de dados (desde ve excluiu o último registro na última página) e, portanto, diminui sua `PageIndex` propriedade. Na etapa 4 GridView tenta se vincular aos dados recuperados na etapa 2; No entanto, na etapa 2, nenhum registro foi retornado, resultando em um GridView vazio. Com a paginação de padrão de t de esse problema superfície porque na etapa 2 *todos os* registros são recuperados da fonte de dados.

Para corrigir isso, temos duas opções. A primeira é criar um manipulador de eventos para o s GridView `RowDeleted` manipulador de eventos que determina quantos registros foram exibidos na página que acabou de ser excluída. Se houve apenas um registro, o registro excluído apenas deve ter sido último e é necessário diminuir o GridView s `PageIndex`. Obviamente, queremos apenas atualizar o `PageIndex` se a operação de exclusão foi bem-sucedida na verdade, que pode ser determinado, garantindo que o `e.Exception` é de propriedade `null`.

Essa abordagem funciona porque ele atualiza o `PageIndex` após a etapa 1, mas antes da etapa 2. Portanto, na etapa 2, o conjunto apropriado de registros é retornado. Para fazer isso, use um código como o seguinte:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Uma solução alternativa é criar um manipulador de eventos para o s ObjectDataSource `RowDeleted` eventos e definir o `AffectedRows` propriedade para um valor de 1. Depois de excluir o registro na etapa 1 (mas antes de recuperar novamente os dados na etapa 2), o GridView atualiza seu `PageIndex` propriedade se uma ou mais linhas foram afetadas pela operação. No entanto, o `AffectedRows` propriedade não está definida por ObjectDataSource e, portanto, esta etapa é omitida. Uma maneira para que esta etapa executada é definir manualmente o `AffectedRows` propriedade se a operação de exclusão for concluída com êxito. Isso pode ser feito usando código semelhante ao seguinte:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

O código de ambos esses manipuladores de eventos pode ser encontrado na classe code-behind do `EfficientPaging.aspx` exemplo.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Comparando o desempenho da paginação personalizada e padrão

Como a paginação personalizada recupera somente os registros necessários, enquanto paginação padrão retorna *todos os* os registros de cada página que está sendo exibido, ele claro que a paginação personalizada é mais eficiente do que a paginação padrão. Mas apenas como muito mais eficiente é paginação personalizada? Que tipo de ganhos de desempenho pode ser visto por mover de paginação padrão para paginação personalizada?

Infelizmente, não há s sem um tamanho serve para responder a todos os aqui. O ganho de desempenho depende de vários fatores, mais proeminente dois sendo o número de pager por meio de registros e a carga sobre os canais de comunicação e o servidor de banco de dados entre o servidor web e o servidor de banco de dados. Para tabelas pequenas com apenas alguns registros de dezenas, a diferença de desempenho pode ser insignificante. No entanto, para tabelas grandes, com milhares a centenas de milhares de linhas, a diferença de desempenho é acento aguda.

Um artigo meu, [paginação personalizada no ASP.NET 2.0 com o SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contém alguns testes de desempenho que executei para exibir as diferenças de desempenho entre essas duas técnicas de paginação quando paginação por meio de uma tabela de banco de dados com 50.000 registros. Nesses testes, examinou o tempo para executar a consulta no nível do SQL Server (usando [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) e a página ASP.NET usando [recursos de rastreamento do ASP.NET s](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenha em mente que esses testes foram executados em minha caixa de desenvolvimento com um único usuário ativo, portanto não-científico e são não simular padrões de carga do site da Web típica. Independente disso, os resultados ilustram as diferenças relativas no tempo de execução para paginação personalizada e padrão, ao trabalhar com suficientemente grandes quantidades de dados.


|  | **Média Duração (s)** | **Leituras** |
| --- | --- | --- |
| **Criador de perfil SQL de paginação padrão** | 1.411 | 383 |
| **Criador de perfil SQL paginação personalizada** | 0.002 | 29 |
| **Rastreamento padrão do ASP.NET de paginação** | 2.379 | *N/A* |
| **Rastreamento de ASP.NET paginação personalizada** | 0.029 | *N/A* |


Como você pode ver, recuperar uma página específica de dados necessário 354 menos leituras em média e concluídas em uma fração do tempo. Na página do ASP.NET, personalizado a página foi capaz de renderizar em perto de 1/100<sup>th</sup> do tempo necessário ao usar a paginação padrão. Consulte [meu artigo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) para obter mais informações sobre esses resultados junto com o código e um banco de dados, você pode baixar para reproduzir esses testes no seu ambiente.

## <a name="summary"></a>Resumo

Paginação padrão é muito fácil de implementar Verifique a caixa de seleção Habilitar paginação na marca inteligente de dados Web controle s, mas tal simplicidade ocorre às custas do desempenho. Com paginação padrão, quando um usuário solicita qualquer página de dados *todos os* registros são retornados, mesmo que apenas uma pequena fração deles pode ser mostrada. Para combater essa sobrecarga de desempenho, o ObjectDataSource oferece uma alternativa paginação personalizada da opção de paginação.

Enquanto a paginação personalizada aprimora padrão paginação problemas de desempenho s recuperando somente os registros que precisam ser exibidos, ele s mais envolvida implementar a paginação personalizada. Primeiro, uma consulta deve ser escrita que corretamente (e com eficiência) acessa o subconjunto específico de registros solicitado. Isso pode ser feito de várias maneiras; o examinamos neste tutorial é usar o SQL Server 2005 s nova `ROW_NUMBER()` resultados de função para classificação e, em seguida, para retornar apenas os resultados de cuja classificação está em um intervalo especificado. Além disso, precisamos adicionar um meio para determinar o número total de registros por meio de pager. Depois de criar esses métodos DAL e BLL, também é preciso configurar ObjectDataSource para que ele pode determinar o número total de registros estão sendo enviada via pager por meio e corretamente podem passar os valores de índice de linha de início e o número máximo de linhas para BLL.

Ao implementar paginação personalizada exigem um número de etapas e é não quase tão simple quanto paginação padrão, a paginação personalizada é uma necessidade quando a paginação suficientemente grandes quantidades de dados. Como os resultados examinados paginação personalizada, mostrada pode lançar uma segundos fora do tempo de renderização de página ASP.NET e pode reduzir a carga no servidor de banco de dados em uma ou mais ordens de magnitude.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](paging-and-sorting-report-data-vb.md)
> [Próximo](sorting-custom-paged-data-vb.md)
