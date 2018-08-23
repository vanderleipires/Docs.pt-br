---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Adicionando mais colunas DataTable (c#) | Microsoft Docs
author: rick-anderson
description: Ao usar o Assistente do TableAdapter para criar um conjunto de dados tipado, DataTable correspondente contém as colunas retornadas pela consulta de banco de dados principal. Lá, mas...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 059538d3196aaa1fe3a70d9c02565e4e7af36881
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831053"
---
<a name="adding-additional-datatable-columns-c"></a>Adicionando mais colunas DataTable (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) ou [baixar PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Ao usar o Assistente do TableAdapter para criar um conjunto de dados tipado, DataTable correspondente contém as colunas retornadas pela consulta de banco de dados principal. Mas há ocasiões em que o DataTable precisa incluir colunas adicionais. Neste tutorial, saiba por que os procedimentos armazenados são recomendados quando precisarmos de mais colunas DataTable.


## <a name="introduction"></a>Introdução

Ao adicionar um TableAdapter a um conjunto de dados tipado, o esquema de s DataTable correspondente é determinado pela consulta TableAdapter s principal. Por exemplo, se a consulta principal retorna campos de dados *um*, *B*, e *C*, a DataTable terá três colunas correspondentes denominadas *um*, *B*, e *C*. Além de sua consulta principal, um TableAdapter pode incluir consultas adicionais que retornam, talvez, um subconjunto dos dados com base em algum parâmetro. Por exemplo, em além com o `ProductsTableAdapter` consulta principal s, que retorna informações sobre todos os produtos, ela também contém métodos como `GetProductsByCategoryID(categoryID)` e `GetProductByProductID(productID)`, que retornam informações de produto específico com base em um parâmetro fornecido.

O modelo de ter o esquema de DataTable s refletem a consulta principal do TableAdapter s funciona bem se todos os métodos TableAdapter s retornam o mesmo ou menos campos de dados daquelas especificadas na consulta principal. Se precisar de um método do TableAdapter retornar campos de dados adicionais, podemos deve expandir o esquema de DataTable s adequadamente. No [mestre/detalhes usando uma lista com marcadores de registros de mestre com um DataList de detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial adicionamos um método para o `CategoriesTableAdapter` que retornou a `CategoryID`, `CategoryName`, e `Description` campos de dados definidos no a consulta principal adição `NumberOfProducts`, um campo de dados adicionais que relatou o número de produtos associados a cada categoria. Manualmente, adicionamos uma nova coluna para o `CategoriesDataTable` para capturar o `NumberOfProducts` valor desse novo método do campo de dados.

Conforme discutido na [carregar arquivos](../working-with-binary-files/uploading-files-cs.md) tutorial, grande cuidado com TableAdapters que usam instruções SQL ad hoc e têm métodos cujos campos de dados não coincidem com precisão a consulta principal. Se o Assistente de configuração do TableAdapter é executado novamente, ele atualizará todos os métodos TableAdapter s para que corresponda a sua lista de campos de dados da consulta principal. Consequentemente, quaisquer métodos com listas de coluna personalizada serão revertido para a lista de colunas da consulta principal s e não retornar os dados esperados. Esse problema não ocorre ao usar procedimentos armazenados.

Neste tutorial vamos examinar como estender o esquema de s uma DataTable para incluir colunas adicionais. Devido a fragilidade do TableAdapter ao usar instruções SQL ad hoc, neste tutorial usaremos procedimentos armazenados. Consulte a [criando novos procedimentos armazenados para o s TableAdapters do conjunto de dados tipado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) e [usando procedimentos armazenados existentes para o s TableAdapters do conjunto de dados tipado](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) tutoriais para saber mais sobre Configurando um TableAdapter para usar procedimentos armazenados.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Etapa 1: Adicionando um`PriceQuartile`coluna para o`ProductsDataTable`

No *criando novos procedimentos armazenados para o s TableAdapters do conjunto de dados tipado* tutorial, criamos um DataSet tipado chamado `NorthwindWithSprocs`. Atualmente, esse conjunto de dados contém duas DataTables: `ProductsDataTable` e `EmployeesDataTable`. O `ProductsTableAdapter` tem três métodos a seguir:

- `GetProducts` -a consulta principal, que retorna todos os registros do `Products` tabela
- `GetProductsByCategoryID(categoryID)` -Retorna todos os produtos com a especificada *categoryID*.
- `GetProductByProductID(productID)` -Retorna o produto específico com a especificada *productID*.

A consulta principal e os dois métodos adicionais retornam o mesmo conjunto de campos de dados, ou seja, todas as colunas da `Products` tabela. Não há nenhum subconsultas correlacionadas ou `JOIN` s, extraindo dados relacionados do `Categories` ou `Suppliers` tabelas. Portanto, o `ProductsDataTable` tem uma coluna correspondente para cada campo no `Products` tabela.

Para este tutorial, vamos s adicionar um método para o `ProductsTableAdapter` chamado `GetProductsWithPriceQuartile` que retorna todos os produtos. Além dos campos de dados padrão do produto, `GetProductsWithPriceQuartile` também incluirá uma `PriceQuartile` campo de dados que indica sob quais quartil o preço do produto s cai. Por exemplo, os produtos cujos preços estão em 25% mais caro terá um `PriceQuartile` valor de 1, enquanto aquelas cujos preços se enquadram na parte inferior 25% terá um valor de 4. Antes de nos preocupamos criando o procedimento armazenado para retornar essa informação, no entanto, primeiro precisamos atualizar o `ProductsDataTable` para incluir uma coluna para conter a `PriceQuartile` resultados quando o `GetProductsWithPriceQuartile` método é usado.

Abra o `NorthwindWithSprocs` conjunto de dados e clique duas vezes no `ProductsDataTable`. Escolha Adicionar no menu de contexto e, em seguida, escolha a coluna.


[![Adicionar uma nova coluna para o ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: adicionar uma nova coluna para o `ProductsDataTable` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image3.png))


Isso adicionará uma nova coluna à DataTable chamado Column1 do tipo `System.String`. Precisamos atualizar esse nome de coluna s PriceQuartile e seu tipo como `System.Int32` , pois ele será usado para manter um número entre 1 e 4. Selecione a coluna recentemente adicionada na `ProductsDataTable` e, na janela Propriedades, defina a `Name` propriedade PriceQuartile e o `DataType` propriedade `System.Int32`.


[![Definir as propriedades de tipo de dados e o nome da nova coluna s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: definir a nova coluna s `Name` e `DataType` propriedades ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image6.png))


Como mostra a Figura 2, há propriedades adicionais que podem ser definidas, como se os valores na coluna devem ser exclusivos, se a coluna for uma coluna de incremento automático, ou não de banco de dados `NULL` valores são permitidos e assim por diante. Deixe esses valores definidos como seus padrões.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Etapa 2: Criando o`GetProductsWithPriceQuartile`método

Agora que o `ProductsDataTable` foi atualizado para incluir os `PriceQuartile` coluna, estamos prontos para criar o `GetProductsWithPriceQuartile` método. Inicie o botão direito do mouse no TableAdapter e selecionando Add Query no menu de contexto. Isso abre o Assistente de configuração de consulta do TableAdapter, que primeiro solicita sobre se desejamos usar instruções SQL ad hoc ou um procedimento armazenado de novo ou existente. Já que estamos don t ainda tem um procedimento armazenado que retorna os dados de preço quartil, permitem que s permitem que o TableAdapter criar esse procedimento armazenado para nós. Selecione a opção de procedimento armazenado criar novo e clique em Avançar.


[![Instruir o Assistente para criar o procedimento armazenado para que possamos TableAdapter](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: instruir o Assistente para criar o armazenado procedimento para nós TableAdapter ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image9.png))


A tela subsequente, mostrada na Figura 4, o assistente solicita a nós que tipo de consulta para adicionar. Uma vez que o `GetProductsWithPriceQuartile` método retornará todas as colunas e registros da `Products` da tabela, selecione SELECT que retorna linhas de opção e clique em Avançar.


[![Nossa consulta será uma instrução SELECT que retorna várias linhas](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: nossa consulta será um `SELECT` instrução que retorna várias linhas ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image12.png))


Em seguida, será solicitado a fornecer o `SELECT` consulta. Insira a seguinte consulta no Assistente:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

A consulta acima usa o SQL Server 2005 s novo [ `NTILE` função](https://msdn.microsoft.com/library/ms175126.aspx) para dividir os resultados em quatro grupos em que os grupos são determinados pelo `UnitPrice` valores classificados em ordem decrescente.

Infelizmente, o construtor de consultas não sabem como analisar o `OVER` palavra-chave e exibirá um erro ao analisar a consulta acima. Portanto, insira a consulta acima diretamente na caixa de texto no assistente sem usar o construtor de consultas.

> [!NOTE]
> Para obter mais informações sobre s NTILE e SQL Server 2005 outras funções de classificação, consulte [retornando resultados classificados com o Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) e o [seção funções de classificação](https://msdn.microsoft.com/library/ms189798.aspx) do [SQL Manuais Online do Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).


Depois de inserir o `SELECT` consulta e clique em Avançar, o assistente solicita que possamos fornecer um nome para o procedimento armazenado será criado. Nomeie o novo procedimento armazenado `Products_SelectWithPriceQuartile` e clique em Avançar.


[![Nome do procedimento armazenado Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: nomear o procedimento armazenado `Products_SelectWithPriceQuartile` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image15.png))


Por fim, podemos serão solicitados a nomear os métodos do TableAdapter. Deixe os dois o preenchimento uma DataTable e retornar uma DataTable de caixas de seleção marcadas e o nome os métodos `FillWithPriceQuartile` e `GetProductsWithPriceQuartile`.


[![Os métodos TableAdapter s de nome e clique em Concluir](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: nomear os métodos do TableAdapter s e clique em Concluir ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image18.png))


Com o `SELECT` consulta especificada e o procedimento armazenado e métodos do TableAdapter nomeados, clique em Concluir para concluir o assistente. Neste ponto você pode receber um aviso ou dois do Assistente de dizendo que o `OVER` instrução ou construção SQL não tem suporte. Esses avisos podem ser ignorados.

Depois de concluir o assistente, o TableAdapter deve incluir a `FillWithPriceQuartile` e `GetProductsWithPriceQuartile` métodos e o banco de dados devem incluir um procedimento armazenado denominado `Products_SelectWithPriceQuartile`. Reserve um tempo para verificar que o TableAdapter, de fato, contém esse novo método e que o procedimento armazenado foi adicionado corretamente ao banco de dados. Durante a verificação de banco de dados, se você não vir o procedimento armazenado try clicando com botão direito na pasta Stored Procedures e escolher atualizar.


![Verifique se que foi adicionado um novo método ao TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: Verifique se que foi adicionado um novo método ao TableAdapter


[![Certifique-se de que o banco de dados contém o Products_SelectWithPriceQuartile procedimento armazenado](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: Certifique-se de que o banco de dados que contém o `Products_SelectWithPriceQuartile` procedimento armazenado ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Um dos benefícios do uso de procedimentos armazenados, em vez de instruções SQL ad hoc é que a executar novamente o Assistente de configuração do TableAdapter não modificará a coluna de lista de procedimentos armazenados. Verifique isso clicando duas vezes no TableAdapter, escolhendo a opção de configurar no menu de contexto para iniciar o assistente e, em seguida, clicando em Concluir para concluí-lo. Em seguida, vá para o banco de dados e exibir o `Products_SelectWithPriceQuartile` procedimento armazenado. Observe que sua lista de colunas não foi modificada. Estavam estamos usando instruções SQL ad hoc, executar novamente o Assistente de configuração do TableAdapter seria revertida essa lista de colunas de consulta s para corresponder à lista de coluna da consulta principal, removendo, assim, a instrução de NTILE de consulta usada pelo `GetProductsWithPriceQuartile` método.


Quando a camada de acesso a dados s `GetProductsWithPriceQuartile` método é invocado, o TableAdapter executa o `Products_SelectWithPriceQuartile` procedimento armazenado e adiciona uma linha para o `ProductsDataTable` para cada registro de retorno. Os campos de dados retornados pelo procedimento armazenado são mapeados para o `ProductsDataTable` colunas s. Já que há um `PriceQuartile` campo de dados retornados pelo procedimento armazenado, seu valor é atribuído para o `ProductsDataTable` s `PriceQuartile` coluna.

Para esses métodos do TableAdapter cujas consultas não retornam um `PriceQuartile` campo de dados, o `PriceQuartile` o valor da coluna s é o valor especificado por seu `DefaultValue` propriedade. Como mostra a Figura 2, esse valor é definido como `DBNull`, o padrão. Se você desejar um valor padrão diferente, basta definir o `DefaultValue` propriedade adequadamente. Apenas certifique-se de que o `DefaultValue` o valor é válido, dada a coluna s `DataType` (ou seja, `System.Int32` para o `PriceQuartile` coluna).

Neste ponto, fizemos as etapas necessárias para adicionar uma coluna adicional a um DataTable. Para verificar se essa coluna adicional funciona conforme o esperado, deixe s criar uma página ASP.NET que exibe cada s nome do produto, preço e quartil de preço. Antes de fazer isso, no entanto, primeiro precisamos atualizar a camada de lógica de negócios para incluir um método que chama para baixo para o s DAL `GetProductsWithPriceQuartile` método. Vamos atualizar a BLL, em seguida, na etapa 3 e, em seguida, criar a página ASP.NET na etapa 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Etapa 3: Aumentando a camada de lógica de negócios

Antes de usarmos o novo `GetProductsWithPriceQuartile` método da camada de apresentação, podemos deve primeiro adicionar um método correspondente para a BLL. Abra o `ProductsBLLWithSprocs` arquivo de classe e adicione o seguinte código:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Como os outros métodos de recuperação de dados no `ProductsBLLWithSprocs`, o `GetProductsWithPriceQuartile` método simplesmente chama o DAL s correspondente `GetProductsWithPriceQuartile` método e retorna os resultados.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Etapa 4: Exibindo as informações de preço quartil em uma página da Web ASP.NET

Com a adição de BLL conclua, está pronto para criar uma página ASP.NET que mostra o quartil de preço para cada produto. Abra o `AddingColumns.aspx` página na `AdvancedDAL` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` propriedade para `Products`. Da GridView s marca inteligente, associá-lo a um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe s `GetProductsWithPriceQuartile` método. Já que isso será uma grade somente leitura, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image25.png))


[![Recuperar informações de produto do método GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperar informações de produto a `GetProductsWithPriceQuartile` método ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image28.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio adicionará automaticamente um BoundField ou CheckBoxField a GridView para cada um dos campos de dados retornados pelo método. Um desses campos de dados é `PriceQuartile`, que é a coluna são adicionadas a `ProductsDataTable` na etapa 1.

Edite os campos de s GridView, remoção de tudo, exceto os `ProductName`, `UnitPrice`, e `PriceQuartile` BoundFields. Configurar o `UnitPrice` BoundField para formatar seu valor como uma moeda e ter o `UnitPrice` e `PriceQuartile` BoundFields e center alinhado à direita, respectivamente. Por fim, atualize o BoundFields restantes `HeaderText` propriedades para o produto, preço e preço quartil, respectivamente. Além disso, verifique a caixa de seleção Habilitar classificação da marca inteligente s GridView.

Após essas modificações, GridView e ObjectDataSource s marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Figura 11 mostra essa página quando acessadas por meio de um navegador. Observe que, inicialmente, os produtos são ordenados por seu preço em ordem decrescente com cada produto atribuído apropriado `PriceQuartile` valor. É claro esses dados podem ser classificados por outros critérios com o valor da coluna preço quartil ainda refletindo a classificação de produto s em relação ao preço (veja a Figura 12).


[![Os produtos são ordenados por seus preços](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: os produtos são ordenados por seus preços ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image31.png))


[![Os produtos são ordenados por seus nomes](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: os produtos são ordenados por seus nomes ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Com algumas linhas de código nós poderia ampliamos o GridView, de modo que ele coloridos as linhas de produto com base em suas `PriceQuartile` valor. Podemos pode colorir os produtos em que o primeiro quartil uma luz verde, aqueles em que o segundo quartil um amarelo-claro e assim por diante. Eu recomendo que você reserve um tempo para adicionar essa funcionalidade. Se você precisar de um lembrete sobre a formatação de um GridView, consulte o [formatação com base em dados personalizados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Uma abordagem alternativa - criando outro TableAdapter

Como vimos neste tutorial, quando adicionar um método para um TableAdapter que retorna os campos de dados que não sejam aqueles indicados claramente pela consulta principal, podemos adicionar colunas correspondentes à DataTable. Essa abordagem, entretanto, funciona bem apenas se há um pequeno número de métodos no TableAdapter que retornam campos de dados diferentes e se esses campos de dados alternativo não variam muito da consulta principal.

Em vez de adicionar colunas à tabela de dados, você pode adicionar outro TableAdapter ao conjunto de dados que contém os métodos do TableAdapter primeiro que retornam campos de dados diferentes. Para este tutorial, em vez de adicionar o `PriceQuartile` coluna para o `ProductsDataTable` (onde ele é usado somente pela `GetProductsWithPriceQuartile` método), poderia que adicionamos um TableAdapter adicional ao conjunto de dados denominado `ProductsWithPriceQuartileTableAdapter` que é usado o `Products_SelectWithPriceQuartile` armazenados procedimento como sua consulta principal. Páginas ASP.NET que são necessárias para obter informações sobre o produto com o quartil preço seriam usar a `ProductsWithPriceQuartileTableAdapter`, enquanto aqueles que não foi possível continuar a usar o `ProductsTableAdapter`.

Adicionando um novo TableAdapter, as tabelas de dados permanecem untarnished e suas colunas refletem com precisão os campos de dados retornados por seus métodos do TableAdapter s. No entanto, TableAdapters adicionais pode apresentar funcionalidade e as tarefas repetitivas. Por exemplo, se esses de páginas ASP.NET que é exibido o `PriceQuartile` coluna também necessário para fornecer a inserção, atualização e exclusão suporte, o `ProductsWithPriceQuartileTableAdapter` precisaria ter seu `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades corretamente configurado. Embora essas propriedades espelharia o `ProductsTableAdapter` s, essa configuração apresenta uma etapa extra. Além disso, agora há duas maneiras de atualizar, excluir ou adicionar um produto no banco de dados - por meio de `ProductsTableAdapter` e `ProductsWithPriceQuartileTableAdapter` classes.

O download para este tutorial inclui um `ProductsWithPriceQuartileTableAdapter` classe o `NorthwindWithSprocs` conjunto de dados que ilustra essa abordagem alternativa.

## <a name="summary"></a>Resumo

Na maioria dos cenários, todos os métodos em um TableAdapter retornará o mesmo conjunto de campos de dados, mas há vezes quando um método específico ou dois podem precisar retornar um campo adicional. Por exemplo, no [mestre/detalhes usando uma lista com marcadores de registros de mestre com um DataList de detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial adicionamos um método para o `CategoriesTableAdapter` que, além dos campos de dados de consulta principal s retornados um `NumberOfProducts` campo que relatou o número de produtos associados a cada categoria. Neste tutorial vimos adicionando um método na `ProductsTableAdapter` que retornou um `PriceQuartile` campo além dos campos de dados de consulta principal s. Para capturar dados adicionais campos retornados pelos métodos TableAdapter s, precisamos adicionar colunas correspondentes à DataTable.

Se você planeja manualmente adicionando colunas à tabela de dados, é recomendável que o TableAdapter usar procedimentos armazenados. Se o TableAdapter usa instruções SQL ad hoc, sempre que o Assistente de configuração do TableAdapter é executado todos os métodos de listas de campo de dados é revertido para os campos de dados retornados pela consulta principal. Esse problema não se estende aos procedimentos armazenados, por isso, eles são recomendados e foram usados neste tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores líder para este tutorial foram Randy Schmidt, Goor Jacky, Bernadette Leigh e Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-the-tableadapter-to-use-joins-cs.md)
> [Próximo](working-with-computed-columns-cs.md)
