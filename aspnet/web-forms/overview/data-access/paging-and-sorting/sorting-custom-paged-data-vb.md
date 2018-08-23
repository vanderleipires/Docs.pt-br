---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Classificação personalizados paginados dados (VB) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior, aprendemos como implementar a paginação personalizada quando presentating dados em uma página da web. Neste tutorial, podemos ver como estender o anterior...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 63d10200300a015b35062409aba2fd7999de8c3c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832952"
---
<a name="sorting-custom-paged-data-vb"></a>Classificação personalizados paginados dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) ou [baixar PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> No tutorial anterior, aprendemos como implementar a paginação personalizada quando presentating dados em uma página da web. Neste tutorial, podemos ver como estender o exemplo anterior para incluir suporte para paginação personalizada de classificação.


## <a name="introduction"></a>Introdução

Em comparação com paginação padrão, paginação personalizada pode melhorar o desempenho da paginação por meio de dados em várias ordens de magnitude, tornando personalizado paginação a opção de implementação de paginação de fato quando a paginação de grandes quantidades de dados. Implementando a paginação personalizada é mais envolvida que a implementação padrão de paginação, no entanto, especialmente quando adicionando classificação à combinação. Neste tutorial, ampliaremos o exemplo daquele anterior para incluir suporte para classificação *e* paginação personalizada.

> [!NOTE]
> Uma vez que esse tutorial complementa aquele anterior, antes de início dedique uns momentos para copiar a sintaxe declarativa dentro a `<asp:Content>` elemento da página da web anterior do tutorial s (`EfficientPaging.aspx`) e cole-o entre o `<asp:Content>` elemento em que o `SortParameter.aspx` página. Voltar à etapa 1 dos [adicionando controles de validação para a edição e inserção de Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) tutorial para uma discussão mais detalhada sobre como replicar a funcionalidade de uma página ASP.NET para outro.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Etapa 1: Reavaliando a técnica de paginação personalizada

A paginação personalizada funcione corretamente, devemos implementar alguma técnica que com eficiência pode pegar um subconjunto específico de registros fornecido aos parâmetros de índice de linha inicial e o número máximo de linhas. Há uma série de técnicas que podem ser usadas para atingir esse objetivo. No tutorial anterior analisamos realizar isso usando o Microsoft SQL Server 2005 s novo `ROW_NUMBER()` função de classificação. Em resumo, o `ROW_NUMBER()` função de classificação atribui um número de linha para cada linha retornada por uma consulta que é classificada por uma ordem de classificação especificada. O subconjunto apropriado de registros é obtido, em seguida, retornando uma seção específica dos resultados numerados. A consulta a seguir ilustra como usar essa técnica para retornar esses produtos numerados 11 a 20 quando os resultados de classificação ordenada em ordem alfabética pelo `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Essa técnica funciona bem para paginação usando uma ordem de classificação específicas (`ProductName` classificados em ordem alfabética, neste caso), mas a consulta precisa ser modificado para mostrar os resultados classificados por uma expressão de classificação diferentes. O ideal é que a consulta anterior poderia ser reescrita para usar um parâmetro no `OVER` cláusula, da seguinte forma:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Infelizmente, com parâmetros `ORDER BY` cláusulas não são permitidas. Em vez disso, devemos criar um procedimento armazenado que aceita um `@sortExpression` parâmetro de entrada, mas usa uma das seguintes alternativas:

- Escrever consultas embutido em código para cada uma das expressões de classificação que podem ser usadas; em seguida, use `IF/ELSE` instruções T-SQL para determinar qual consulta a ser executada.
- Use uma `CASE` instrução para fornecer dinâmico `ORDER BY` expressões com base no `@sortExpressio` n parâmetro de entrada; consulte o usado para a seção classificação dinamicamente resultados da consulta em [Power do SQL `CASE` instruções](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Para obter mais informações.
- Criar a consulta apropriada como uma cadeia de caracteres no procedimento armazenado e, em seguida, use [as `sp_executesql` procedimento armazenado do sistema](https://msdn.microsoft.com/library/ms188001.aspx) para executar a consulta dinâmica.

Cada uma dessas soluções tem algumas desvantagens. A primeira opção não é tão fácil de manter como os outros dois pois ele requer que você crie uma consulta para cada expressão de classificação possíveis. Portanto, se você decidir posteriormente adicionar campos novos e classificáveis GridView. Você também precisará voltar e atualize o procedimento armazenado. A segunda abordagem tem algumas sutilezas que apresentam as preocupações de desempenho ao classificar por colunas de banco de dados de não cadeia de caracteres e também é prejudicada os mesmos problemas de facilidade de manutenção, como o primeiro. E a terceira opção, que usa SQL dinâmico, introduz o risco de um ataque de injeção de SQL, se um invasor é capaz de executar o procedimento armazenado, passando os valores de parâmetro de entrada de sua escolha.

Embora nenhuma dessas abordagens é perfeita, acho que a terceira opção é o melhor dos três. Com o uso de SQL dinâmico, ele oferece um nível de flexibilidade que não os outros dois. Além disso, um ataque de injeção de SQL pode ser explorado somente se um invasor é capaz de executar o procedimento armazenado, passando os parâmetros de entrada de sua escolha. Uma vez que a DAL usa consultas parametrizadas, o ADO.NET protegerá esses parâmetros que são enviados para o banco de dados por meio da arquitetura, que significa que a vulnerabilidade de ataque de injeção de SQL existe somente se o invasor pode executar diretamente o procedimento armazenado.

Para implementar essa funcionalidade, crie um novo procedimento armazenado no banco de dados Northwind chamado `GetProductsPagedAndSorted`. Esse procedimento armazenado deve aceitar parâmetros de entrada: `@sortExpression`, um parâmetro de entrada do tipo `nvarchar(100`) que especifica como os resultados devem ser classificados e é injetado diretamente após o `ORDER BY` texto no `OVER` cláusula; e `@startRowIndex` e `@maximumRows`, os mesmos parâmetros de entrada do inteiro de dois a `GetProductsPaged` examinado no tutorial anterior de procedimento armazenado. Criar o `GetProductsPagedAndSorted` procedimento armazenado usando o script a seguir:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

O procedimento armazenado é iniciado, garantindo que um valor para o `@sortExpression` parâmetro foi especificado. Se ele estiver ausente, os resultados são classificados por `ProductID`. Em seguida, a consulta SQL dinâmica é construída. Observe que a consulta SQL dinâmica aqui um pouco diferente de nossas consultas anteriores usadas para recuperar todas as linhas da tabela produtos. Nos exemplos anteriores, obtivemos cada categoria de produto s associada nomes s s e o fornecedor, usando uma subconsulta. Essa decisão foi tomada volta a [criando uma camada de acesso de dados](../introduction/creating-a-data-access-layer-vb.md) tutorial e foi feito em vez de usar `JOIN` s porque o TableAdapter não pode criar automaticamente a inserção associada, atualizar e excluir os métodos de tal consultas. O `GetProductsPagedAndSorted` procedimento armazenado, no entanto, deve usar `JOIN` s para os resultados para serem ordenados pelos nomes de categoria ou fornecedor.

Essa consulta dinâmica é criada concatenando as partes de consulta estática e o `@sortExpression`, `@startRowIndex`, e `@maximumRows` parâmetros. Uma vez que `@startRowIndex` e `@maximumRows` são parâmetros de inteiro, eles devem ser convertidos em nvarchars para ser concatenado corretamente. Depois que esta consulta SQL dinâmica foi construída, ele é executado por meio de `sp_executesql`.

Reserve um tempo para testar esse procedimento armazenado com valores diferentes para o `@sortExpression`, `@startRowIndex`, e `@maximumRows` parâmetros. Do Gerenciador de servidores, clique com botão direito no nome do procedimento armazenado e escolha Executar. Isso abrirá a caixa de diálogo Run Stored Procedure no qual você pode inserir os parâmetros de entrada (consulte a Figura 1). Para classificar os resultados pelo nome da categoria, use CategoryName para o `@sortExpression` valor de parâmetro; para classificar pelo nome da empresa supplier s, use o CompanyName. Depois de fornecer os valores de parâmetros, clique em Okey. Os resultados são exibidos na janela de saída. Figura 2 mostra os resultados quando retornar produtos classificados 11 a 20 ao classificar pela `UnitPrice` em ordem decrescente.


![Testar valores diferentes para os procedimento armazenado s três parâmetros de entrada](sorting-custom-paged-data-vb/_static/image1.png)

**Figura 1**: tentar valores diferentes para o procedimento armazenado s três parâmetros de entrada


[![O procedimento armazenado s resultados são mostrados na janela de saída](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Figura 2**: O procedimento armazenado s resultados são mostrados na janela de saída ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> Quando os resultados de classificação por especificado `ORDER BY` coluna o `OVER` cláusula, SQL Server deve classificar os resultados. Isso é uma operação rápida se houver um índice clusterizado pela coluna (s) os resultados estão sendo ordenados por ou se há uma cobertura de índice, mas pode ser mais caro caso contrário. Para melhorar o desempenho de consultas suficientemente grandes, considere a adição de um índice não clusterizado para a coluna pela qual os resultados são ordenados por. Consulte a [funções de classificação e o desempenho no SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obter mais detalhes.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Etapa 2: Aumentando o acesso a dados e camadas de lógica comercial

Com o `GetProductsPagedAndSorted` procedimento armazenado criado, nossa próxima etapa é fornecer um meio para executar esse procedimento armazenado por meio de nossa arquitetura de aplicativo. Isso envolve a adição de um método apropriado para a BLL e DAL. Permitir que o s comece adicionando um método para o DAL. Abra o `Northwind.xsd` tipada DataSet, o botão direito do mouse no `ProductsTableAdapter`e escolha a opção de adicionar consulta no menu de contexto. Como fizemos no tutorial anterior, que queremos configurar esse novo método DAL para usar um procedimento armazenado existente - `GetProductsPagedAndSorted`, nesse caso. Iniciar, indicando que você deseja que o novo método do TableAdapter para usar um procedimento armazenado existente.


![Optar por usar um procedimento armazenado existente](sorting-custom-paged-data-vb/_static/image5.png)

**Figura 3**: optar por usar um procedimento armazenado existente


Para especificar o procedimento armazenado para usar, selecione o `GetProductsPagedAndSorted` procedimento na lista suspensa é armazenado na próxima tela.


![Use o GetProductsPagedAndSorted procedimento armazenado](sorting-custom-paged-data-vb/_static/image6.png)

**Figura 4**: usar o GetProductsPagedAndSorted procedimento armazenado


Esse procedimento armazenado retorna um conjunto de registros, pois seus resultados dessa forma, na próxima tela, indicam que ele retorna dados tabulares.


![Indicar que o procedimento armazenado retorna dados tabulares](sorting-custom-paged-data-vb/_static/image7.png)

**Figura 5**: indicar que o procedimento armazenado retorna dados tabulares


Finalmente, crie métodos DAL que usam os dois o preenchimento uma DataTable e retornar um padrões de DataTable, os métodos de nomenclatura `FillPagedAndSorted` e `GetProductsPagedAndSorted`, respectivamente.


![Escolha os nomes de métodos](sorting-custom-paged-data-vb/_static/image8.png)

**Figura 6**: escolha os nomes de métodos


Agora que estamos ve estendido a DAL, podemos está pronto para ativar a BLL. Abra o `ProductsBLL` arquivo de classe e adicione um novo método, `GetProductsPagedAndSorted`. Esse método precisa aceitar parâmetros de entrada `sortExpression`, `startRowIndex`, e `maximumRows` e deve simplesmente chamar para baixo em s DAL `GetProductsPagedAndSorted` método, da seguinte forma:


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Etapa 3: Configurando o ObjectDataSource para passar no parâmetro SortExpression

Ter aumentado a DAL e BLL para incluir métodos que utilizam o `GetProductsPagedAndSorted` procedimento armazenado, tudo o que permanece é configurar o ObjectDataSource na `SortParameter.aspx` página para usar o novo método BLL e passar o `SortExpression` parâmetro baseado no coluna em que o usuário solicitou para classificar os resultados.

Comece alterando o s ObjectDataSource `SelectMethod` partir `GetProductsPaged` para `GetProductsPagedAndSorted`. Isso pode ser feito por meio do Assistente Configurar fonte de dados, na janela Propriedades, ou diretamente a sintaxe declarativa. Em seguida, precisamos fornecer um valor para o s ObjectDataSource [ `SortParameterName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Se essa propriedade for definida, o ObjectDataSource tenta passar em s GridView `SortExpression` propriedade para o `SelectMethod`. Em particular, o ObjectDataSource procura um parâmetro de entrada cujo nome é igual ao valor da `SortParameterName` propriedade. Desde o s BLL `GetProductsPagedAndSorted` método tem o parâmetro de entrada expressão classificação denominado `sortExpression`, defina o s ObjectDataSource `SortExpression` propriedade sortExpression.

Depois de fazer essas duas alterações, a sintaxe declarativa do ObjectDataSource s deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Como com o tutorial anterior, certifique-se de que não faz o ObjectDataSource *não* incluem os parâmetros de entrada sortExpression, startRowIndex ou maximumRows em sua coleção SelectParameters.


Para habilitar a classificação em GridView, basta marcar a caixa de seleção Habilitar classificação da GridView s marca inteligente, que define o s GridView `AllowSorting` propriedade para `true` e fazendo com que o texto do cabeçalho para cada coluna a ser renderizado como um LinkButton. Quando o usuário final clica em um dos botões de link do cabeçalho, um postback massacre e as etapas a seguir ocorrer:

1. As atualizações de GridView seus [ `SortExpression` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) para o valor da `SortExpression` do campo cujo cabeçalho link foi clicado
2. O ObjectDataSource invoca o s BLL `GetProductsPagedAndSorted` método, passando o s GridView `SortExpression` propriedade como o valor para o método s `sortExpression` parâmetro de entrada (juntamente com apropriado `startRowIndex` e `maximumRows` valores de parâmetro de entrada)
3. A BLL invoca o DAL s `GetProductsPagedAndSorted` método
4. A DAL executa o `GetProductsPagedAndSorted` procedimento armazenado, passando na `@sortExpression` parâmetro (juntamente com o `@startRowIndex` e `@maximumRows` valores de parâmetro de entrada)
5. O procedimento armazenado retornará o subconjunto apropriado de dados para a BLL, que retorna para o ObjectDataSource; Esses dados, em seguida, são associados a GridView, renderizados em HTML e enviados para o usuário final

Figura 7 mostra a primeira página de resultados quando classificadas pelo `UnitPrice` em ordem crescente.


[![Os resultados são classificados por UnitPrice](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Figura 7**: os resultados são classificados por UnitPrice ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-vb/_static/image11.png))


Embora a implementação atual corretamente pode classificar os resultados por nome do produto, nome da categoria, quantidade por unidade e o preço unitário, tentando ordenar os resultados pelo fornecedor resultados de nome em uma exceção de tempo de execução (consulte a Figura 8).


![A tentativa de classificar os resultados pelos resultados de fornecedor na seguinte exceção de tempo de execução](sorting-custom-paged-data-vb/_static/image12.png)

**Figura 8**: tentativa de classificar os resultados pelos resultados de fornecedor na seguinte exceção de tempo de execução


Essa exceção ocorre porque o `SortExpression` os s GridView `SupplierName` BoundField é definido como `SupplierName`. No entanto, o fornecedor s nome na `Suppliers` tabela, na verdade, é chamada `CompanyName` foram um alias esse nome de coluna como `SupplierName`. No entanto, o `OVER` cláusula usada pelo `ROW_NUMBER()` não é possível usar o alias de função e deve usar o nome de coluna real. Portanto, alterar o `SupplierName` BoundField s `SortExpression` de NomeDoFornecedor para CompanyName (veja a Figura 9). Como mostra a Figura 10, após essa alteração, que os resultados podem ser classificados pelo fornecedor.


![Altere o NomeDoFornecedor BoundField s SortExpression para CompanyName](sorting-custom-paged-data-vb/_static/image13.png)

**Figura 9**: alterar o NomeDoFornecedor BoundField s SortExpression para CompanyName


[![Os resultados agora podem ser classificados por fornecedor](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Figura 10**: os resultados agora podem ser classificadas por fornecedor ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>Resumo

A implementação de paginação personalizada, examinamos no tutorial anterior necessário que a ordem pela qual os resultados foram seja classificada ser especificado em tempo de design. Em resumo, isso significava que a implementação de paginação personalizada que são implementadas não pôde, ao mesmo tempo, fornecer recursos de classificação. Neste tutorial, estamos superou essa limitação, estendendo o procedimento armazenado do primeiro para incluir um `@sortExpression` parâmetro de entrada pela qual os resultados poderiam ser classificados.

Após a criação deste procedimento armazenado e criação de novos métodos na BLL e DAL, fomos capazes de implementar um GridView que oferecidos ambos os recursos de classificação e personalizados de paginação, configurando o ObjectDataSource para transmitir o GridView s atual `SortExpression` propriedade para a BLL `SelectMethod`.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Santos Carlos. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](efficiently-paging-through-large-amounts-of-data-vb.md)
> [Próximo](creating-a-customized-sorting-user-interface-vb.md)
