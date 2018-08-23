---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parâmetros declarativos (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial podemos ilustrar como usar um parâmetro definido como um valor embutido em código para selecionar os dados a serem exibidos em um controle DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 10ed3a4f019cd78033ecdd61499b1914f403414e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831716"
---
<a name="declarative-parameters-vb"></a>Parâmetros declarativos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) ou [baixar PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> Neste tutorial podemos ilustrar como usar um parâmetro definido como um valor embutido em código para selecionar os dados a serem exibidos em um controle DetailsView.


## <a name="introduction"></a>Introdução

No [último tutorial](displaying-data-with-the-objectdatasource-vb.md) analisamos exibindo dados com os controles GridView, DetailsView e FormView associados a um controle ObjectDataSource que invocou o `GetProducts()` método da `ProductsBLL` classe. O `GetProducts()` método retorna uma tabela de dados fortemente tipados preenchida com todos os registros do banco de dados Northwind `Products` tabela. O `ProductsBLL` classe contém métodos adicionais para retornados subconjuntos apenas dos produtos - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, e `GetProductsBySupplierID(supplierID)`. Esses três métodos esperam um parâmetro de entrada que indica como filtrar as informações de produto retornado.

O ObjectDataSource pode ser usado para invocar métodos que esperam parâmetros de entrada, mas para isso, podemos deve especificar de onde vêm os valores para esses parâmetros. Os valores de parâmetro pode ser embutido em código ou podem vir de uma variedade de origens dinâmicas, incluindo: valores de querystring, variáveis de sessão, o valor da propriedade de um controle de Web na página ou outras pessoas.

Para este tutorial vamos começar que ilustram como usar um parâmetro definido como um valor embutido em código. Especificamente, vamos examinar adicionando um DetailsView para a página que exibe informações sobre um produto específico, ou seja, do chefe Anton mistura para Gumbo, que tem um `ProductID` de 5. Em seguida, veremos como definir o valor do parâmetro com base em um controle da Web. Em particular, usaremos uma caixa de texto para permitir que o usuário digite em um país, após o qual eles podem clicar um botão para ver a lista de fornecedores que residem nesse país.

## <a name="using-a-hard-coded-parameter-value"></a>Usando um valor de parâmetro embutido em código

Para o primeiro exemplo, comece adicionando um controle DetailsView para o `DeclarativeParams.aspx` página o `BasicReporting` pasta. Na marca inteligente de DetailsView, selecione &lt;nova fonte de dados&gt; na lista suspensa lista e escolha Adicionar um ObjectDataSource.


[![Adicionar um ObjectDataSource para a página](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: adicionar um ObjectDataSource para a página ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image3.png))


Isso iniciará o Assistente de escolher fonte de dados do controle ObjectDataSource automaticamente. Selecione o `ProductsBLL` classe desde a primeira tela do assistente.


[![Selecione a classe ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: selecione o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image6.png))


Como queremos exibir informações sobre um produto específico que queremos usar o `GetProductByProductID(productID)` método.


[![Escolha o método GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: escolha o `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image9.png))


Uma vez que o método selecionamos inclui um parâmetro, há uma tela de mais para que o assistente, em que somos solicitados a definir o valor a ser usado para o parâmetro. A lista à esquerda mostra todos os parâmetros para o método selecionado. Para `GetProductByProductID(productID)` há apenas um `productID`. À direita, podemos especificar o valor do parâmetro selecionado. A lista de lista suspensa de origem do parâmetro enumera as várias fontes possíveis para o valor do parâmetro. Como queremos especificar um valor embutido em código de 5 para o `productID` parâmetro, deixar a fonte de parâmetro como None e digitar 5, na caixa de texto DefaultValue.


[![Um Hard-Coded parâmetro de valor de 5 será usado para o parâmetro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: A Hard-Coded parâmetro de valor de 5 será usado para o `productID` parâmetro ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image12.png))


Depois de concluir o Assistente Configurar fonte de dados, a marcação de declarativa do controle ObjectDataSource inclui um `Parameter` do objeto na `SelectParameters` coleção para cada um dos parâmetros de entrada esperados pelo método definido no `SelectMethod` propriedade. Uma vez que o método que estamos usando neste exemplo espera apenas um único parâmetro de entrada, `parameterID`, há apenas uma entrada aqui. O `SelectParameters` coleção pode conter qualquer classe que deriva de `Parameter` classe no `System.Web.UI.WebControls` namespace. Para a base de valores de parâmetro embutido `Parameter` classe é usada, mas para o outro parâmetro de origem opções um derivada `Parameter` classe é usada; você também pode criar seus próprios [tipos de parâmetro personalizado](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), se necessário.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Se você estiver acompanhando em seu próprio computador a marcação declarativa você vê neste momento pode incluir valores para o `InsertMethod`, `UpdateMethod`, e `DeleteMethod` propriedades, bem como `DeleteParameters`. Assistente de escolher fonte de dados do ObjectDataSource especifica automaticamente os métodos a partir de `ProductBLL` a ser usado para inserção, atualização e exclusão, portanto, a menos que você desmarcou explicitamente aqueles out, serão incluídos na marcação acima.


Ao visitar essa página, os dados de controle de Web invocará o ObjectDataSource `Select` método, que chamará o `ProductsBLL` da classe `GetProductByProductID(productID)` método usando o valor embutido em código de 5 para o `productID` parâmetro de entrada. O método retornará um fortemente tipadas `ProductDataTable` objeto que contém uma única linha com informações sobre a mistura de para Gumbo do chefe Anton (o produto com `ProductID` 5).


[![Para Gumbo mistura informações sobre do chefe Anton são exibidos](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: para Gumbo mistura informações sobre do chefe Anton são exibidas ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Definindo o valor do parâmetro como o valor da propriedade de um controle de Web

Parâmetros do ObjectDataSource valores também podem ser definidos com base no valor de um controle da Web na página. Para ilustrar isso, vamos dar uma GridView que lista todos os fornecedores que estão localizados em um país especificado pelo usuário. Para realizar este guia de início, adicionando uma caixa de texto para a página na qual o usuário pode inserir um nome de país. Definir esse controle de caixa de texto `ID` propriedade para `CountryName`. Também adicione um controle da Web de botão.


[![Adicione uma caixa de texto para a página com ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: Adicione uma caixa de texto para a página com `ID` `CountryName` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image18.png))


Em seguida, adicionar um controle GridView à página e, na marca inteligente, escolha Adicionar um novo ObjectDataSource. Como queremos exibir select de informações do fornecedor a `SuppliersBLL` classe da primeira de tela do assistente. Na segunda tela, escolha o `GetSuppliersByCountry(country)` método.


[![Escolha o método GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: escolha o `GetSuppliersByCountry(country)` método ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image21.png))


Uma vez que o `GetSuppliersByCountry(country)` método tem um parâmetro de entrada, o assistente inclui mais uma vez uma tela final para escolher o valor do parâmetro. Neste momento, defina a origem do parâmetro de controle. Isso populará a lista suspensa de ControlID com os nomes dos controles na página; Selecione o `CountryName` controle da lista. Quando a página é visitada primeiro o `CountryName` caixa de texto estará em branco, portanto, nenhum resultado será retornado e nada é exibido. Se você quiser exibir algum resultado por padrão, defina a caixa de texto DefaultValue adequadamente.


[![Defina o valor de parâmetro para o valor do controle CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: defina o valor de parâmetro para o `CountryName` o valor de controle ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image24.png))


Marcação declarativa do ObjectDataSource difere um pouco nosso primeiro exemplo, usando um [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) em vez do padrão `Parameter` objeto. Um `ControlParameter` tem propriedades adicionais para especificar o `ID` do controle da Web e o valor da propriedade a ser usado para o parâmetro (`PropertyName`). O Assistente Configurar fonte de dados foi inteligente o suficiente para determinar que, para uma caixa de texto, provavelmente desejará usar o `Text` propriedade para o valor do parâmetro. Se, no entanto, você quiser usar um valor de propriedade diferente de controle da Web que você pode alterar o `PropertyName` valor aqui ou clicando no link "Mostrar propriedades avançadas" no assistente.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Ao visitar a página pela primeira vez o `CountryName` caixa de texto está vazia. O ObjectDataSource `Select` método ainda é chamado pelo GridView, mas um valor de `Nothing` é passado para o `GetSuppliersByCountry(country)` método. Converte o TableAdapter o `Nothing` em um banco de dados `NULL` valor (`DBNull.Value`), mas a consulta usada pelo `GetSuppliersByCountry(country)` método é escrito, de modo que ele não retorna qualquer valores quando um `NULL` valor for especificado para o `@CategoryID`parâmetro. Em resumo, não há nenhum fornecedor é retornados.

Depois que o visitante entra em um país, no entanto e clica no botão Mostrar fornecedores para fazer com que um postback, o ObjectDataSource `Select` método é novamente consultado, passando o controle de caixa de texto `Text` de valor como o `country` parâmetro.


[![Esses fornecedores do Canadá são mostrados](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: os fornecedores do Canadá são mostrados ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Mostrando todos os fornecedores por padrão

Em vez disso, que mostrar nenhum dos fornecedores ao primeiro exibir a página podemos querer mostrar *todos os* fornecedores a princípio, permitindo que o usuário diminuir a lista digitando um nome de país na caixa de texto. Quando a caixa de texto está vazia, o `SuppliersBLL` da classe `GetSuppliersByCountry(country)` método é passado `Nothing` para sua *`country`* parâmetro de entrada. Isso `Nothing` valor é então passado para baixo para a DAL `GetSupplierByCountry(country)` método, onde ele é convertido em um banco de dados `NULL` de valor para o `@Country` parâmetro na consulta a seguir:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

A expressão `Country = NULL` sempre retorna falso, mesmo para os registros cujo `Country` coluna tem um `NULL` valor; portanto, nenhum registro será retornado.

Para retornar *todos os* fornecedores quando o país de caixa de texto está vazio, nós ampliamos o `GetSuppliersByCountry(country)` método na BLL para invocar o `GetSuppliers()` método quando o parâmetro de país for `Nothing` e para chamar a DAL `GetSuppliersByCountry(country)` Caso contrário do método. Isso terá o efeito de retornar todos os fornecedores quando nenhum país é especificado e o subconjunto apropriado de fornecedores, quando o parâmetro de país é incluído.

Alterar o `GetSuppliersByCountry(country)` método no `SuppliersBLL` classe para o seguinte:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Com essa alteração de `DeclarativeParams.aspx` página mostra todos os fornecedores quando visitado pela primeira vez (ou sempre que o `CountryName` caixa de texto está vazia).


[![Todos os fornecedores estão agora são mostradas por padrão](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: todos os fornecedores estão agora são mostradas por padrão ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Resumo

Para usar métodos com parâmetros de entrada, precisamos especificar os valores para os parâmetros no ObjectDataSource `SelectParameters` coleção. Permitem tipos diferentes de parâmetros para o valor do parâmetro a ser obtidos de fontes diferentes. O tipo de parâmetro padrão usa um valor embutido em código, mas tão facilmente (e sem uma linha de código) os valores de parâmetro podem ser obtidos de querystring, variáveis de sessão, cookies e até mesmo valores inseridos pelo usuário dos controles da Web na página.

Os exemplos que vimos neste tutorial ilustrado como usar os valores de parâmetro declarativa. No entanto, poderá haver ocasiões em que podemos precisa usar uma fonte de parâmetro que não está disponível, como a data e hora atuais, ou, se estiver usando a associação, a ID de usuário do visitante nosso site. Para esses cenários podemos definir os valores de parâmetro por meio de programação antes do ObjectDataSource invocação de método do seu objeto subjacente. Veremos como fazer isso na [próximo tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-objectdatasource-vb.md)
> [Próximo](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
