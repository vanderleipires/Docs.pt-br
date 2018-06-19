---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Classificação personalizada paginável dados (c#) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior aprendemos como implementar paginação personalizada quando presentating dados em uma página da web. Neste tutorial, podemos ver como estender o anterior...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: c30f13703ef20cd764785b00cd812ef4486e0f16
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882410"
---
<a name="sorting-custom-paged-data-c"></a>Classificação personalizada paginável dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) ou [baixar PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> No tutorial anterior aprendemos como implementar paginação personalizada quando presentating dados em uma página da web. Neste tutorial, podemos ver como estender o exemplo anterior para incluir suporte para paginação personalizada de classificação.


## <a name="introduction"></a>Introdução

Em comparação com paginação padrão, paginação personalizada pode melhorar o desempenho da paginação por meio de dados em várias ordens de magnitude, tornando personalizado paginação a escolha de implementação de paginação de fato quando a paginação grandes quantidades de dados. Implementação de paginação personalizada é mais envolvida que a implementação padrão de paginação, no entanto, especialmente ao adicionar a classificação para a combinação. Neste tutorial entenderemos o exemplo do anterior para incluir suporte para classificação *e* paginação personalizada.

> [!NOTE]
> Como este tutorial baseia aquele anterior, antes de início dedicar um tempo para copiar a sintaxe declarativa dentro a `<asp:Content>` elemento da página da web anterior do tutorial s (`EfficientPaging.aspx`) e cole-o entre o `<asp:Content>` elemento o `SortParameter.aspx` página. Voltar para a etapa 1 do [adicionando controles de validação para a edição e inserindo Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial para uma discussão mais detalhada sobre como replicar a funcionalidade de uma página ASP.NET para outra.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Etapa 1: Reavaliando a técnica de paginação personalizada

A paginação personalizada funcione corretamente, devemos implementar técnicas que com eficiência podem obter um subconjunto específico de registros fornecido aos parâmetros de índice de linha de início e o número máximo de linhas. Há várias técnicas que podem ser usadas para atingir esse objetivo. No tutorial anterior analisamos realizar isso usando o Microsoft SQL Server 2005 s novo `ROW_NUMBER()` função de classificação. Em resumo, o `ROW_NUMBER()` função de classificação atribui um número de linha para cada linha retornada por uma consulta que é classificada por uma ordem de classificação especificada. O subconjunto apropriado de registros é obtido, em seguida, retornando uma seção específica dos resultados numerados. A consulta a seguir ilustra como usar essa técnica para retornar os produtos numerados 11 a 20 quando os resultados de classificação ordem alfabeticamente pelo `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Essa técnica funciona bem para paginação usando uma ordem de classificação específicas (`ProductName` classificadas em ordem alfabética, nesse caso), mas a consulta precisa ser modificada para mostrar os resultados classificados por uma expressão de classificação diferente. Idealmente, a consulta anterior poderia ser reescrita para usar um parâmetro de `OVER` cláusula, da seguinte forma:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Infelizmente, com parâmetros `ORDER BY` cláusulas não são permitidas. Em vez disso, devemos criar um procedimento armazenado que aceita um `@sortExpression` parâmetro de entrada, mas usa uma das seguintes alternativas:

- Escrever consultas embutida para cada uma das expressões de classificação que podem ser usadas; em seguida, use `IF/ELSE` instruções T-SQL para determinar qual consulta a ser executada.
- Use um `CASE` instrução para fornecer dinâmico `ORDER BY` expressões com base no `@sortExpressio` n parâmetro de entrada; consulte usado para seção classificar resultados de consulta dinamicamente [Power de SQL `CASE` instruções](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Para obter mais informações.
- Criar a consulta apropriada, como uma cadeia de caracteres no procedimento armazenado e, em seguida, usar [o `sp_executesql` procedimento armazenado do sistema](https://msdn.microsoft.com/library/ms188001.aspx) para executar a consulta dinâmica.

Cada uma dessas soluções alternativas apresenta algumas desvantagens. A primeira opção não é como sustentável as outras duas, pois ela requer que você crie uma consulta para cada expressão de classificação possíveis. Portanto, se você decidir posteriormente adicionar campos novos e classificáveis a GridView você também precisará voltar e atualize o procedimento armazenado. A segunda abordagem tem algumas sutilezas que apresentam problemas de desempenho ao classificar por colunas de banco de dados de cadeia de caracteres não e também é prejudicada os mesmos problemas de facilidade de manutenção, como o primeiro. E a terceira opção, que usa SQL dinâmico, apresenta o risco de um ataque de injeção SQL se um invasor for capaz de executar o procedimento armazenado, passando os valores de parâmetro de entrada de sua escolha.

Embora nenhum desses procedimentos seja perfeito, acho que a terceira opção é o melhor dos três. Com o uso de SQL dinâmico, ele oferece um nível de flexibilidade para que os outros dois não. Além disso, um ataque de injeção de SQL só pode ser explorado se um invasor for capaz de executar o procedimento armazenado passando parâmetros de entrada de sua escolha. Como a DAL usa consultas parametrizadas, ADO.NET protegerá os parâmetros que são enviados para o banco de dados por meio da arquitetura, que significa que a vulnerabilidade de ataque de injeção de SQL existe somente se o invasor pode executar diretamente o procedimento armazenado.

Para implementar essa funcionalidade, crie um novo procedimento armazenado no banco de dados Northwind denominado `GetProductsPagedAndSorted`. Esse procedimento armazenado deve aceitar três parâmetros de entrada: `@sortExpression`, um parâmetro de entrada do tipo `nvarchar(100`) que especifica como os resultados devem ser classificados e é injetado diretamente após o `ORDER BY` texto no `OVER` cláusula; e `@startRowIndex` e `@maximumRows`, os mesmos parâmetros de entrada de dois inteiros do `GetProductsPaged` examinado no tutorial anterior do procedimento armazenado. Criar o `GetProductsPagedAndSorted` procedimento armazenado usando o script a seguir:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

O procedimento armazenado é iniciado, garantindo que um valor para o `@sortExpression` parâmetro foi especificado. Se ele estiver ausente, os resultados são classificados por `ProductID`. Em seguida, a consulta SQL dinâmica é construída. Observe que a consulta SQL dinâmica aqui difere ligeiramente das nossas consultas anteriores usadas para recuperar todas as linhas da tabela produtos. Nos exemplos anteriores, nós obtivemos cada categoria de produto s associada nomes de s s e fornecedor usando uma subconsulta. Essa decisão foi feita na [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) tutorial e foi feita em vez de usar `JOIN` s porque o TableAdapter não é possível criar automaticamente a inserção associada, atualizar e excluir os métodos de tal consultas. O `GetProductsPagedAndSorted` procedimento armazenado, no entanto, deve usar `JOIN` s para os resultados a ser ordenada por nomes de categoria ou fornecedor.

Essa consulta dinâmica é criada pela concatenação as partes de consulta estática e `@sortExpression`, `@startRowIndex`, e `@maximumRows` parâmetros. Como `@startRowIndex` e `@maximumRows` são parâmetros de número inteiro, eles devem ser convertidos em nvarchars para ser concatenados corretamente. Depois que essa consulta SQL dinâmica foi construída, ele é executado por meio de `sp_executesql`.

Reserve um tempo para testar esse procedimento armazenado com valores diferentes para o `@sortExpression`, `@startRowIndex`, e `@maximumRows` parâmetros. No Gerenciador de servidores, clique com botão direito no nome do procedimento armazenado e escolha Executar. Isso abrirá a caixa de diálogo Executar procedimento armazenado no qual você pode inserir os parâmetros de entrada (consulte a Figura 1). Para classificar os resultados pelo nome da categoria, use CategoryName para o `@sortExpression` valor do parâmetro; classificar pelo nome da empresa do fornecedor s, use CompanyName. Depois de fornecer os valores dos parâmetros, clique em Okey. Os resultados são exibidos na janela de saída. A Figura 2 mostra os resultados quando retornar produtos classificados 11 a 20 ao classificar pelo `UnitPrice` em ordem decrescente.


![Testar valores diferentes para os procedimento armazenado s três parâmetros de entrada](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: tente valores diferentes para o procedimento armazenado s três parâmetros de entrada


[![O procedimento armazenado s resultados são mostrados na janela de saída](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: O procedimento armazenado s resultados são mostrados na janela de saída ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Quando os resultados de classificação por especificado `ORDER BY` coluna o `OVER` cláusula, SQL Server deve classificar os resultados. Isso é uma operação rápida se houver um índice clusterizado em colunas, os resultados estão sendo ordenados por ou se houver uma cobertura de índice, mas pode ser mais caro de outra forma. Para melhorar o desempenho de consultas suficientemente grandes, considere a adição de um índice não clusterizado para a coluna pela qual os resultados são ordenados por. Consulte [desempenho no SQL Server 2005 e funções de classificação](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obter mais detalhes.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Etapa 2: Aumentando o acesso a dados e camadas de lógica de negócios

Com o `GetProductsPagedAndSorted` procedimento armazenado criado, nossa próxima etapa é fornecer um meio para executar esse procedimento armazenado por meio de nosso arquitetura do aplicativo. Isso envolve a adição de um método apropriado para a DAL e BLL. Permitir que o s comece adicionando um método para a DAL. Abra o `Northwind.xsd` conjunto de dados tipado, com o botão direito no `ProductsTableAdapter`e escolha a opção de consulta adicionar no menu de contexto. Como foi o tutorial anterior, desejamos configurar esse novo método DAL para usar um procedimento armazenado existente - `GetProductsPagedAndSorted`, nesse caso. Iniciar, indicando que você deseja o novo método de TableAdapter para usar um procedimento armazenado existente.


![Optar por usar um procedimento armazenado existente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: optar por usar um procedimento armazenado existente


Para especificar o procedimento armazenado a ser usado, selecione o `GetProductsPagedAndSorted` procedimento da lista suspensa armazenado na próxima tela.


![Use o GetProductsPagedAndSorted procedimento armazenado](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: usar o GetProductsPagedAndSorted procedimento armazenado


Esse procedimento armazenado retorna um conjunto de registros, pois seus resultados assim, na próxima tela, indicam que ele retorna dados tabulares.


![Indicar que o procedimento armazenado retorna dados tabulares](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indicar que o procedimento armazenado retorna dados tabulares


Finalmente, crie métodos DAL que usam os dois o preenchimento uma DataTable e retornar um padrões de DataTable, os métodos de nomeação `FillPagedAndSorted` e `GetProductsPagedAndSorted`, respectivamente.


![Escolha os nomes dos métodos](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: escolha os nomes dos métodos


Agora que possamos ve estendido a DAL, podemos re pronto para ativar a BLL. Abra o `ProductsBLL` arquivo de classe e adicione um novo método, `GetProductsPagedAndSorted`. Esse método precisa aceitar três parâmetros de entrada `sortExpression`, `startRowIndex`, e `maximumRows` e simplesmente deve chamar para baixo em s DAL `GetProductsPagedAndSorted` método, da seguinte forma:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Etapa 3: Configurar o ObjectDataSource para passar no parâmetro SortExpression

Tendo aumentada a DAL e BLL para incluir os métodos que utilizam o `GetProductsPagedAndSorted` procedimento armazenado, todos os que permanece é configurar o ObjectDataSource no `SortParameter.aspx` página para usar o novo método BLL e passar o `SortExpression` parâmetro baseado no coluna em que o usuário solicitou para classificar os resultados por.

Inicie alterando a s ObjectDataSource `SelectMethod` de `GetProductsPaged` para `GetProductsPagedAndSorted`. Isso pode ser feito por meio do Assistente Configurar fonte de dados, na janela Propriedades, ou diretamente por meio de sintaxe declarativa. Em seguida, é preciso fornecer um valor para o s ObjectDataSource [ `SortParameterName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Se essa propriedade for definida, o ObjectDataSource tenta passar em GridView s `SortExpression` propriedade para o `SelectMethod`. Em particular, o ObjectDataSource procura por um parâmetro de entrada cujo nome é igual ao valor da `SortParameterName` propriedade. Desde o s BLL `GetProductsPagedAndSorted` método tem o parâmetro de entrada expressão classificação denominado `sortExpression`, defina o s ObjectDataSource `SortExpression` propriedade sortExpression.

Depois de fazer essas alterações de dois, a sintaxe declarativa de s ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Como com o tutorial anterior, certifique-se de que o ObjectDataSource não *não* incluem os parâmetros de entrada sortExpression, startRowIndex ou maximumRows em sua coleção SelectParameters.


Para habilitar a classificação em GridView, basta marcar a caixa de seleção Habilitar classificação da GridView s marca inteligente, que define o GridView s `AllowSorting` propriedade `true` e fazendo com que o texto do cabeçalho para cada coluna a ser renderizado como um LinkButton. Quando o usuário final clicar em um dos botões de link do cabeçalho, um postback tem lugar e as etapas a seguir ocorrer:

1. As atualizações de GridView seu [ `SortExpression` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) para o valor da `SortExpression` do campo link cujo cabeçalho foi clicado
2. O ObjectDataSource invoca o s BLL `GetProductsPagedAndSorted` método, passando o GridView s `SortExpression` propriedade como o valor para o método s `sortExpression` parâmetro de entrada (junto com as `startRowIndex` e `maximumRows` valores de parâmetro de entrada)
3. BLL invoca o s DAL `GetProductsPagedAndSorted` método
4. A DAL executa o `GetProductsPagedAndSorted` procedimento armazenado, passando no `@sortExpression` parâmetro (juntamente com o `@startRowIndex` e `@maximumRows` valores de parâmetro de entrada)
5. O procedimento armazenado retorna o subconjunto apropriado de dados para BLL, que retorna a ObjectDataSource; Esses dados, em seguida, são associados a GridView, renderizados em HTML e enviados para o usuário final

A Figura 7 mostra a primeira página de resultados quando classificadas pelo `UnitPrice` em ordem crescente.


[![Os resultados são classificados pelo UnitPrice](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: os resultados são classificados pelo UnitPrice ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-cs/_static/image11.png))


Enquanto a implementação atual pode classificar os resultados por nome do produto, nome da categoria, quantidade por unidade e preço unitário corretamente, a tentativa de classificar os resultados pelo fornecedor resultados de nome em uma exceção de tempo de execução (consulte a Figura 8).


![Tentativa de classificar os resultados pelos resultados de fornecedor no seguinte exceção de tempo de execução](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: tentativa de classificar os resultados pelos resultados de fornecedor no seguinte exceção de tempo de execução


Essa exceção ocorre porque o `SortExpression` os s GridView `SupplierName` BoundField é definido como `SupplierName`. No entanto, o fornecedor s nome no `Suppliers` tabela, na verdade, é chamada de `CompanyName` estávamos alias esse nome de coluna como `SupplierName`. No entanto, o `OVER` cláusula usada pelo `ROW_NUMBER()` função não pode usar o alias e devem usar o nome da coluna real. Portanto, alterar o `SupplierName` BoundField s `SortExpression` de NomeDoFornecedor para CompanyName (consulte a Figura 9). Como mostra a Figura 10, após essa alteração, que os resultados podem ser classificados pelo fornecedor.


![Altere o NomeDoFornecedor BoundField s SortExpression para CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: alterar NomeDoFornecedor BoundField s SortExpression para CompanyName


[![Os resultados agora podem ser classificados por fornecedor](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: os resultados podem agora ser classificados por fornecedor ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Resumo

A implementação de paginação personalizada que examinamos no tutorial anterior necessário que a ordem pela qual os resultados foram classificadas ser especificado em tempo de design. Em resumo, isso significa que a implementação de paginação personalizada implementamos não pôde, ao mesmo tempo, fornecer recursos de classificação. Neste tutorial, estamos superou essa limitação estendendo o procedimento armazenado do primeiro para incluir um `@sortExpression` pelo qual os resultados podem ser classificados de parâmetro de entrada.

Depois de criar este procedimento armazenado e criação de novos métodos na BLL e DAL, foi capaz de implementar um GridView que oferecidos ambos os recursos de classificação e personalizadas de paginação Configurando ObjectDataSource para passar o s GridView atual `SortExpression` propriedade BLL `SelectMethod`.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Carlos Santos. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Próximo](creating-a-customized-sorting-user-interface-cs.md)
