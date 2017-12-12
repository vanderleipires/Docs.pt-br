---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Adicionando colunas da DataTable adicionais (c#) | Microsoft Docs
author: rick-anderson
description: "Ao usar o Assistente de TableAdapter para criar um conjunto de dados tipado, DataTable correspondente contém as colunas retornadas pela consulta de banco de dados principal. Não há mas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b1fe8d2e376065aed8d94b1267910bd1f7e5bd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-additional-datatable-columns-c"></a>Adicionando colunas da DataTable adicionais (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) ou [baixar PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Ao usar o Assistente de TableAdapter para criar um conjunto de dados tipado, DataTable correspondente contém as colunas retornadas pela consulta de banco de dados principal. Mas há ocasiões em que a DataTable precisa incluir colunas adicionais. Neste tutorial aprendemos por procedimentos armazenados são recomendados quando precisamos de mais colunas da DataTable.


## <a name="introduction"></a>Introdução

Ao adicionar um TableAdapter para um conjunto de dados tipado, o esquema de s DataTable correspondente é determinado pela consulta TableAdapter s principal. Por exemplo, se a consulta principal retorna campos de dados *um*, *B*, e *C*, DataTable terá três colunas correspondentes denominadas *um*, *B*, e *C*. Além de sua consulta principal, um TableAdapter pode incluir consultas adicionais que retornam, talvez, um subconjunto dos dados com base em algum parâmetro. Por exemplo, em adicionais para o `ProductsTableAdapter` consulta principal s, que retorna informações sobre todos os produtos, ele também contém métodos, como `GetProductsByCategoryID(categoryID)` e `GetProductByProductID(productID)`, que retorna informações de produto específico com base em um parâmetro fornecido.

O modelo de ter o esquema de DataTable s refletem a consulta principal do TableAdapter s funciona bem se todos os métodos TableAdapter s retornam o mesmo ou menor de campos de dados que os especificados na consulta principal. Se precisar de um método do TableAdapter retornar os campos de dados adicionais, podemos deve expandir o esquema de DataTable s adequadamente. No [mestre/detalhes usando uma lista com marcadores de registros de mestre com DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial adicionamos um método para o `CategoriesTableAdapter` que retornou o `CategoryID`, `CategoryName`, e `Description` campos de dados definidos no a consulta principal adição `NumberOfProducts`, um campo de dados adicionais que relatou o número de produtos associados a cada categoria. Manualmente, adicionamos uma nova coluna para o `CategoriesDataTable` para capturar o `NumberOfProducts` valor desse novo método de campo de dados.

Como discutido o [carregando arquivos](../working-with-binary-files/uploading-files-cs.md) tutorial, excelente cuidado com TableAdapters que usam instruções SQL ad hoc e têm métodos cujos campos de dados não coincidem com precisão a consulta principal. Se o Assistente de configuração do TableAdapter for executado novamente, ele atualizará todos os métodos TableAdapter s para que corresponda a sua lista de campo de dados da consulta principal. Consequentemente, qualquer métodos com listas de coluna personalizada serão revertido para a lista de colunas da consulta principal s e não retornar os dados esperados. Esse problema não ocorre ao usar procedimentos armazenados.

Este tutorial examinaremos como estender o esquema de s uma DataTable para incluir colunas adicionais. Devido a fragilidade do TableAdapter ao usar instruções SQL ad hoc, neste tutorial usaremos procedimentos armazenados. Consulte o [criando novos procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) e [usando existente procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) tutoriais para obter mais informações sobre Configurando um TableAdapter para usar procedimentos armazenados.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Etapa 1: Adicionando um`PriceQuartile`coluna para o`ProductsDataTable`

No *criando novos procedimentos armazenados para o conjunto de dados tipado de s TableAdapters* tutorial criamos um conjunto de dados tipado chamado `NorthwindWithSprocs`. Atualmente, esse conjunto de dados contém duas tabelas de dados: `ProductsDataTable` e `EmployeesDataTable`. O `ProductsTableAdapter` tem três métodos a seguir:

- `GetProducts`-a consulta principal, que retorna todos os registros da `Products` tabela
- `GetProductsByCategoryID(categoryID)`-Retorna todos os produtos com especificado *categoryID*.
- `GetProductByProductID(productID)`-Retorna o produto específico com especificado *productID*.

A consulta principal e os dois métodos adicionais retornam o mesmo conjunto de campos de dados, ou seja, todas as colunas da `Products` tabela. Não há nenhum subconsultas correlacionadas ou `JOIN` s dados relacionados entre o `Categories` ou `Suppliers` tabelas. Portanto, o `ProductsDataTable` tem uma coluna correspondente para cada campo de `Products` tabela.

Para este tutorial, vamos s adicionar um método para o `ProductsTableAdapter` chamado `GetProductsWithPriceQuartile` que retorna todos os produtos. Além dos campos de dados padrão do produto, `GetProductsWithPriceQuartile` também incluirá um `PriceQuartile` campo de dados que indica em qual quartil o preço do produto s cai. Por exemplo, os produtos cujos preços são em 25% mais caro terá um `PriceQuartile` valor de 1, enquanto aquelas cujos preços se enquadram na parte inferior 25% terá um valor de 4. Antes que se preocupar sobre como criar o procedimento armazenado para retornar essas informações, no entanto, primeiro precisamos atualizar o `ProductsDataTable` para incluir uma coluna para manter o `PriceQuartile` resultados quando o `GetProductsWithPriceQuartile` método é usado.

Abra o `NorthwindWithSprocs` conjunto de dados e clique duas vezes no `ProductsDataTable`. Escolha Adicionar no menu de contexto e, em seguida, escolha a coluna.


[![Adicionar uma nova coluna para o ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: adicionar uma nova coluna para o `ProductsDataTable` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image3.png))


Isso adicionará uma nova coluna à tabela de dados denominada Column1 do tipo `System.String`. Precisamos atualizar esse nome de coluna s PriceQuartile e seu tipo como `System.Int32` pois ele será usado para manter um número entre 1 e 4. Selecione a coluna adicionada recentemente no `ProductsDataTable` e, na janela Propriedades, defina o `Name` propriedade PriceQuartile e o `DataType` propriedade `System.Int32`.


[![Definir as propriedades de tipo de dados e o nome da nova coluna s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: definir a nova coluna s `Name` e `DataType` propriedades ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image6.png))


Como mostra a Figura 2, há propriedades adicionais que podem ser definidas, como se os valores na coluna devem ser exclusivos, se a coluna é uma coluna de incremento automático, o banco de dados ou não `NULL` valores são permitidos e assim por diante. Deixe esses valores definidos como seus padrões.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Etapa 2: Criando o`GetProductsWithPriceQuartile`método

Agora que o `ProductsDataTable` foi atualizado para incluir o `PriceQuartile` coluna, você está pronto para criar o `GetProductsWithPriceQuartile` método. Iniciar clicando duas vezes no TableAdapter e escolhendo Adicionar consulta no menu de contexto. Isso abre o Assistente de configuração de consulta do TableAdapter, que primeiro solicita nos quanto se queremos usar instruções SQL ad hoc ou um procedimento armazenado existente ou novo. Já que estamos don t ainda tem um procedimento armazenado que retorna os dados de preço quartil, permitem s permitem que o TableAdapter criar esse procedimento armazenado para nós. Selecione a opção de procedimento armazenado criar novo e clique em Avançar.


[![Instrui o Assistente para criar o procedimento armazenado para que possamos TableAdapter](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: instruir o Assistente para criar o armazenados procedimento para nos TableAdapter ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image9.png))


Na tela de subsequentes, mostrada na Figura 4, o assistente perguntará nos que tipo de consulta a ser adicionada. Como o `GetProductsWithPriceQuartile` método retornará todas as colunas e registros da `Products` da tabela, selecione SELECT que retorna linhas opção e clique em Avançar.


[![A consulta será uma instrução SELECT que retorna várias linhas](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: nosso consulta será um `SELECT` instrução que retorna várias linhas ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image12.png))


Em seguida, serão solicitados para o `SELECT` consulta. Digite a seguinte consulta no Assistente:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

A consulta acima usa s do SQL Server 2005 novo [ `NTILE` função](https://msdn.microsoft.com/en-us/library/ms175126.aspx) para dividir os resultados em quatro grupos em que os grupos são determinados pelo `UnitPrice` valores classificados em ordem decrescente.

Infelizmente, o construtor de consultas não sabe como analisar o `OVER` palavra-chave e exibirá um erro ao analisar a consulta anterior. Portanto, digite a consulta acima diretamente na caixa de texto do assistente sem usar o construtor de consultas.

> [!NOTE]
> Para obter mais informações sobre o SQL Server 2005 e NTILE s outras funções de classificação, consulte [retornando resultados com o Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) e [seção funções de classificação](https://msdn.microsoft.com/en-us/library/ms189798.aspx) do [SQL Manuais Online do Server 2005](https://msdn.microsoft.com/en-us/library/ms189798.aspx).


Depois de inserir o `SELECT` consulta e clicar em Avançar, o assistente perguntará fornecer um nome para o procedimento armazenado será criado. Nomeie o novo procedimento armazenado `Products_SelectWithPriceQuartile` e clique em Avançar.


[![Nome do procedimento armazenado Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: nome do procedimento armazenado `Products_SelectWithPriceQuartile` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image15.png))


Por fim, podemos serão solicitados a nomear os métodos TableAdapter. Deixe os dois o preenchimento uma DataTable e retornar uma DataTable caixas de seleção marcadas e os métodos `FillWithPriceQuartile` e `GetProductsWithPriceQuartile`.


[![Os métodos TableAdapter s do nome e clique em Concluir](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: nomear os métodos TableAdapter e clique em Concluir ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image18.png))


Com o `SELECT` consulta especificada e o procedimento armazenado e os métodos TableAdapter nomeados, clique em Concluir para concluir o assistente. Neste ponto, você pode receber um aviso ou dois do assistente dizendo que o `OVER` instrução ou construção SQL não tem suporte. Esses avisos devem ser ignorados.

Depois de concluir o assistente, o TableAdapter deve incluir o `FillWithPriceQuartile` e `GetProductsWithPriceQuartile` métodos e o banco de dados devem incluir um procedimento armazenado denominado `Products_SelectWithPriceQuartile`. Dedique alguns momentos para verificar que o TableAdapter realmente contém esse novo método e que o procedimento armazenado foi adicionado corretamente para o banco de dados. Ao verificar o banco de dados, se você não vir o procedimento armazenado que tente clicando na pasta Stored Procedures e escolhendo a atualização.


![Verifique se um novo método foi adicionado ao TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: Verifique se um novo método foi adicionado ao TableAdapter


[![Verifique se o banco de dados contém o Products_SelectWithPriceQuartile procedimento armazenado](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: Certifique-se de que o banco de dados contém o `Products_SelectWithPriceQuartile` procedimento armazenado ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Um dos benefícios do uso de procedimentos armazenados em vez de instruções SQL ad hoc é executar novamente o Assistente de configuração do TableAdapter não irá modificar a coluna de lista de procedimentos armazenados. Verificar isso clicando duas vezes no TableAdapter, escolher a opção de configurar no menu de contexto para iniciar o assistente e, em seguida, clicar em Concluir para concluí-la. Em seguida, vá para o banco de dados e exibir o `Products_SelectWithPriceQuartile` procedimento armazenado. Observe que sua lista de colunas não foi modificada. Estava estamos usando instruções SQL ad hoc, executar novamente o Assistente de configuração do TableAdapter seria revertida esta lista de coluna de consulta s para corresponder à lista de coluna de consulta principal, eliminando a instrução NTILE da consulta usada pelo `GetProductsWithPriceQuartile` método.


Quando a camada de acesso a dados s `GetProductsWithPriceQuartile` método é chamado, o TableAdapter executa o `Products_SelectWithPriceQuartile` procedimento armazenado e adiciona uma linha para o `ProductsDataTable` para cada registro de retornado. Os campos de dados retornados pelo procedimento armazenado são mapeados para o `ProductsDataTable` colunas s. Como há um `PriceQuartile` campo de dados retornado pelo procedimento armazenado, seu valor é atribuído para o `ProductsDataTable` s `PriceQuartile` coluna.

Para os métodos TableAdapter cujas consultas não retornam um `PriceQuartile` campo de dados, o `PriceQuartile` coluna s é o valor especificado pelo seu `DefaultValue` propriedade. Como mostra a Figura 2, esse valor é definido como `DBNull`, o padrão. Se você preferir um valor padrão diferente, basta definir o `DefaultValue` propriedade adequadamente. Apenas certifique-se de que o `DefaultValue` valor é válido, dada a coluna s `DataType` (ou seja, `System.Int32` para o `PriceQuartile` coluna).

Neste ponto, fizemos as etapas necessárias para adicionar uma coluna adicional a uma DataTable. Para verificar se essa coluna adicional funciona conforme o esperado, permitem s criar uma página ASP.NET que exibe cada s nome do produto, preço e quartil de preço. Antes de fazer isso, no entanto, primeiro precisamos atualizar a camada de lógica de negócios para incluir um método que as chamadas para baixo para o s DAL `GetProductsWithPriceQuartile` método. Vamos atualizar BLL em seguida, na etapa 3 e, em seguida, criar a página ASP.NET na etapa 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Etapa 3: Aumentando a camada de lógica de negócios

Antes que podemos usar o novo `GetProductsWithPriceQuartile` método da camada de apresentação, deve primeiro adicionamos um método correspondente ao BLL. Abra o `ProductsBLLWithSprocs` arquivo de classe e adicione o seguinte código:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Como os outros métodos de recuperação de dados em `ProductsBLLWithSprocs`, o `GetProductsWithPriceQuartile` método simplesmente chama o DAL s correspondente `GetProductsWithPriceQuartile` método e retorna seus resultados.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Etapa 4: Exibindo as informações de preços quartil em uma página da Web

Com a adição de BLL conclua é re pronto para criar uma página ASP.NET que mostra o quartil de preço para cada produto. Abra o `AddingColumns.aspx` página o `AdvancedDAL` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` propriedade `Products`. De GridView s marca inteligente, associá-lo a um novo ObjectDataSource denominado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe s `GetProductsWithPriceQuartile` método. Já que isso será uma grade somente leitura, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image25.png))


[![Recuperar informações de produto do método GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperar informações de produto o `GetProductsWithPriceQuartile` método ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image28.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio adiciona automaticamente um BoundField ou CheckBoxField a GridView para cada um dos campos de dados retornados pelo método. Um desses campos de dados é `PriceQuartile`, que é a coluna são adicionadas a `ProductsDataTable` na etapa 1.

Edite os campos de s GridView, remover tudo, exceto o `ProductName`, `UnitPrice`, e `PriceQuartile` BoundFields. Configurar o `UnitPrice` BoundField formatar seu valor como uma moeda e ter o `UnitPrice` e `PriceQuartile` BoundFields e center-alinhado à direita, respectivamente. Por fim, atualizar o restante BoundFields `HeaderText` propriedades para o produto, preço e preço quartil, respectivamente. Além disso, verifique a caixa de seleção Habilitar classificação de marca inteligente s GridView.

Após essas modificações, a marcação de declarativa s GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

A Figura 11 mostra essa página quando visitado por meio de um navegador. Observe que, inicialmente, os produtos são ordenados por seu preço em ordem decrescente com cada produto atribuído adequados `PriceQuartile` valor. Obviamente esses dados podem ser classificados por outro critério com o valor da coluna preço quartil ainda reflete a classificação de produto s em relação ao preço (veja a Figura 12).


[![Os produtos são ordenados por seus preços](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: os produtos são ordenados por seus preços ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image31.png))


[![Os produtos são ordenados por seus nomes](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: os produtos são ordenados por seus nomes ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Com algumas linhas de código pode aumentar o GridView para que ele coloridas as linhas de produto com base em seus `PriceQuartile` valor. Podemos pode cor esses produtos no primeiro quartil um verde-claro, aqueles em que o segundo quartil um amarelo-claro e assim por diante. Recomendo que você reserve um tempo para adicionar essa funcionalidade. Se você precisar de uma atualização sobre um GridView de formatação, consulte o [personalizado formatação com base em dados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Uma abordagem alternativa - criar outro TableAdapter

Como vimos neste tutorial, ao adicionar um método para um TableAdapter que retorna os campos de dados diferentes dos descritos pela consulta principal, podemos adicionar colunas correspondentes em DataTable. Essa abordagem, entretanto, funciona bem somente se houver um pequeno número de métodos no TableAdapter que retornar campos de dados diferentes e se os campos de dados alternativo não variam muito da consulta principal.

Em vez de adicionar colunas à tabela de dados, em vez disso, você pode adicionar outra TableAdapter ao conjunto de dados que contém os métodos do TableAdapter primeiro retornam campos de dados diferentes. Para este tutorial, em vez de adicionar o `PriceQuartile` coluna para o `ProductsDataTable` (onde ele é usado somente pelo `GetProductsWithPriceQuartile` método), foi, adicionamos um TableAdapter adicional para o conjunto de dados chamado `ProductsWithPriceQuartileTableAdapter` utilizada a `Products_SelectWithPriceQuartile` armazenados procedimento principal de sua consulta. Páginas ASP.NET que são necessárias para obter informações sobre o produto com o quartil preço usaria o `ProductsWithPriceQuartileTableAdapter`, enquanto aquelas que não foi possível continuar a usar o `ProductsTableAdapter`.

Adicionando um novo TableAdapter, as tabelas de dados permanecem untarnished e suas colunas refletem com precisão os campos de dados retornados por seus métodos TableAdapter. No entanto, os TableAdapters adicionais podem introduzir funcionalidade e as tarefas repetitivas. Por exemplo, se esses ASP.NET páginas que são exibidos o `PriceQuartile` coluna também necessário para fornecer a inserção, atualização e exclusão suporte, o `ProductsWithPriceQuartileTableAdapter` precisa ter seu `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades corretamente configurado. Enquanto essas propriedades deve espelhar o `ProductsTableAdapter` s, essa configuração apresenta uma etapa extra. Além disso, agora há duas maneiras de atualizar, excluir ou adicionar um produto no banco de dados - por meio de `ProductsTableAdapter` e `ProductsWithPriceQuartileTableAdapter` classes.

O download para este tutorial inclui um `ProductsWithPriceQuartileTableAdapter` classe no `NorthwindWithSprocs` conjunto de dados que mostra essa abordagem alternativa.

## <a name="summary"></a>Resumo

Na maioria dos cenários, todos os métodos em um TableAdapter retornará o mesmo conjunto de campos de dados, mas há momentos em que um método específico ou dois talvez precise retornar um campo adicional. Por exemplo, no [mestre/detalhes usando uma lista com marcadores de registros de mestre com DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial adicionamos um método para o `CategoriesTableAdapter` que, além dos campos de dados de consulta principal s, retornados um `NumberOfProducts` campo que relatou o número de produtos associados a cada categoria. Neste tutorial vimos adicionando um método no `ProductsTableAdapter` que retornaram um `PriceQuartile` campo além dos campos de dados de consulta principal s. Para capturar dados adicionais campos retornados pelos métodos TableAdapter s que precisamos adicionar colunas correspondentes em DataTable.

Se você planeja manualmente adicionando colunas à tabela de dados, é recomendável que o TableAdapter usar procedimentos armazenados. Se o TableAdapter usa instruções SQL ad hoc, sempre que o Assistente de configuração do TableAdapter é executado todos os métodos reverter listas de campo de dados para os campos de dados retornados pela consulta principal. Esse problema não se estende a procedimentos armazenados, por isso, eles são recomendados e foram usados neste tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Randy Schmidt, Goor Jacky, Bernadette Leigh e Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](updating-the-tableadapter-to-use-joins-cs.md)
[Próximo](working-with-computed-columns-cs.md)
