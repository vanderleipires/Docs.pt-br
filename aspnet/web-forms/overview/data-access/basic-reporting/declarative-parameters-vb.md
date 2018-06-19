---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parâmetros declarativos (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, isso será mostrado como usar um parâmetro definido como um valor embutido para selecionar os dados a serem exibidos em um controle DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 933b7276c6dac5cce0e278fd23ff010c5b4a6fdd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877405"
---
<a name="declarative-parameters-vb"></a>Parâmetros declarativos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) ou [baixar PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> Neste tutorial, isso será mostrado como usar um parâmetro definido como um valor embutido para selecionar os dados a serem exibidos em um controle DetailsView.


## <a name="introduction"></a>Introdução

No [tutorial última](displaying-data-with-the-objectdatasource-vb.md) examinamos exibindo dados com os controles GridView, DetailsView e FormView associados a um controle ObjectDataSource que invocou o `GetProducts()` método do `ProductsBLL` classe. O `GetProducts()` método retorna um DataTable fortemente tipada preenchido com todos os registros do banco de dados Northwind `Products` tabela. O `ProductsBLL` classe contém métodos adicionais para retornar subconjuntos apenas dos produtos - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, e `GetProductsBySupplierID(supplierID)`. Esses três métodos esperam um parâmetro de entrada que indica como filtrar as informações de produto retornado.

ObjectDataSource pode ser usado para invocar métodos que esperam parâmetros de entrada, mas para isso é necessário especificar onde vêm os valores para esses parâmetros. Os valores de parâmetro pode ser embutido ou podem vir de uma variedade de fontes dinâmicos, incluindo: querystring valores, variáveis de sessão, o valor da propriedade de um controle da Web na página ou outras pessoas.

Para este tutorial vamos começar que ilustram como usar um parâmetro definido como um valor embutido. Especificamente, examinaremos adicionando um DetailsView para a página que exibe informações sobre um produto específico, ou seja, do chefe Anton mistura para Gumbo, que tem um `ProductID` de 5. Em seguida, veremos como definir o valor do parâmetro com base em um controle da Web. Em particular, vamos usar uma caixa de texto para permitir que o usuário digite em um país, após o qual ele podem clicar em um botão para ver a lista de fornecedores que residem no país.

## <a name="using-a-hard-coded-parameter-value"></a>Usando um valor de parâmetro embutido

No primeiro exemplo, comece adicionando um controle DetailsView para o `DeclarativeParams.aspx` página o `BasicReporting` pasta. Na marca inteligente de DetailsView selecione &lt;nova fonte de dados&gt; na lista suspensa lista e adicione um ObjectDataSource.


[![Adicione um ObjectDataSource à página](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: adicionar um ObjectDataSource para a página ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image3.png))


Isso iniciará o Assistente de escolher fonte de dados do controle ObjectDataSource automaticamente. Selecione o `ProductsBLL` classe na primeira tela do assistente.


[![Selecione a classe ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: selecione o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image6.png))


Desde que você deseja exibir informações sobre um produto específico, desejamos usar o `GetProductByProductID(productID)` método.


[![Escolha o método GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: escolha o `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image9.png))


Como o método selecionamos inclui um parâmetro, há uma tela de mais para que o assistente, em que são solicitados para definir o valor a ser usado para o parâmetro. A lista à esquerda mostra todos os parâmetros para o método selecionado. Para `GetProductByProductID(productID)` há apenas um `productID`. À direita, podemos especificar o valor do parâmetro selecionado. Lista de lista suspensa de parâmetros origem enumera várias fontes possíveis para o valor do parâmetro. Como queremos especificar um valor embutido de 5 para o `productID` parâmetro, deixar a fonte de parâmetro como None e digite 5 na caixa de texto valor padrão.


[![Um Hard-Coded parâmetro de valor de 5 será usado para o parâmetro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: A Hard-Coded parâmetro de valor de 5 será usado para o `productID` parâmetro ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image12.png))


Depois de concluir o Assistente Configurar fonte de dados, declarativo do controle ObjectDataSource inclui um `Parameter` objeto o `SelectParameters` coleção para cada um dos parâmetros de entrada esperados pelo método definido no `SelectMethod` propriedade. Como o método que estamos usando neste exemplo espera apenas um único parâmetro de entrada, `parameterID`, há apenas uma entrada aqui. O `SelectParameters` coleção pode conter qualquer classe que deriva de `Parameter` classe no `System.Web.UI.WebControls` namespace. Para a base de valores de parâmetro embutido `Parameter` classe é usada, mas para o outro parâmetro fonte opções um derivado `Parameter` classe é usada, você também pode criar seus próprios [tipos de parâmetro personalizado](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), se necessário.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Se você estiver acompanhando em seu próprio computador a marcação declarativa você ver neste momento pode incluir valores para o `InsertMethod`, `UpdateMethod`, e `DeleteMethod` propriedades, bem como `DeleteParameters`. Assistente de escolher fonte de dados do ObjectDataSource Especifica os métodos de automaticamente o `ProductBLL` a ser usado para inserir, atualizar e excluir, portanto, a menos que explicitamente desmarcada aqueles out, serão incluídos na marcação acima.


Ao visitar essa página, os dados de controle da Web invocará o ObjectDataSource `Select` método, que chamará o `ProductsBLL` da classe `GetProductByProductID(productID)` método usando o valor inserido no código de 5 para o `productID` parâmetro de entrada. O método retornará fortemente tipado `ProductDataTable` objeto que contém uma única linha com informações sobre a mistura de para Gumbo do chefe Anton (o produto com `ProductID` 5).


[![Para Gumbo mistura informações sobre do chefe Anton são exibidos](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: para Gumbo mistura informações sobre do chefe Anton são exibidas ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Definir o valor de parâmetro para o valor da propriedade de um controle de Web

Parâmetro do ObjectDataSource valores também podem ser definidos com base no valor de um controle da Web na página. Para ilustrar isso, vamos dar um GridView que lista todos os fornecedores que estão localizados em um país especificado pelo usuário. Para fazer isso, inicie com a adição de uma caixa de texto para a página na qual o usuário pode inserir um nome de país. Defina esse controle de caixa de texto `ID` propriedade `CountryName`. Também adicione um controle de botão de Web.


[![Adicionar uma caixa de texto para a página com ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: adicionar uma caixa de texto para a página com `ID` `CountryName` ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image18.png))


Em seguida, adicionar um controle GridView à página e, na marca inteligente, escolha Adicionar um novo ObjectDataSource. Como desejamos exibir select de informações do fornecedor a `SuppliersBLL` classe da tela de primeiro do assistente. Na segunda tela, escolha o `GetSuppliersByCountry(country)` método.


[![Escolha o método GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: escolha o `GetSuppliersByCountry(country)` método ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image21.png))


Desde o `GetSuppliersByCountry(country)` método tem um parâmetro de entrada, o assistente novamente inclui uma tela final para escolher o valor do parâmetro. Neste momento, defina a origem do parâmetro de controle. Isso preencherá lista suspensa ControlID com os nomes dos controles da página. Selecione o `CountryName` controle da lista. Quando a página for visitada primeiro o `CountryName` caixa de texto estará em branco, portanto, nenhum resultado será retornado e nada é exibido. Se você deseja exibir alguns resultados por padrão, definido adequadamente a caixa de texto do valor padrão.


[![Defina o valor de parâmetro para o valor do controle CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: definir o valor do parâmetro o `CountryName` valor de controle ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image24.png))


Marcação declarativa do ObjectDataSource difere ligeiramente do nosso primeiro exemplo, usando um [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) em vez do padrão `Parameter` objeto. Um `ControlParameter` tem propriedades adicionais para especificar o `ID` do controle da Web e o valor da propriedade a ser usado para o parâmetro (`PropertyName`). O Assistente Configurar fonte de dados foi inteligente para determinar que, para uma caixa de texto, provavelmente desejará usar o `Text` propriedade para o valor do parâmetro. Se, no entanto, você quiser usar um valor de outra propriedade do controle da Web que você pode alterar o `PropertyName` valor aqui ou clicando no link "Mostrar as propriedades avançadas" no assistente.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Ao visitar a página pela primeira vez o `CountryName` caixa de texto está vazia. O ObjectDataSource `Select` método ainda é invocado por GridView, mas um valor de `Nothing` é passado para o `GetSuppliersByCountry(country)` método. Converte o TableAdapter o `Nothing` em um banco de dados `NULL` valor (`DBNull.Value`), mas a consulta usada pelo `GetSuppliersByCountry(country)` método é gravada, de modo que ele não retorna qualquer valores quando um `NULL` valor for especificado para o `@CategoryID`parâmetro. Em resumo, nenhum fornecedores são retornados.

Depois que o visitante entra em um país, no entanto e clicar no botão Mostrar fornecedores para causar um postback, ObjectDataSource `Select` método é novamente consultado, passando o controle de caixa de texto `Text` valor como o `country` parâmetro.


[![São mostrados os fornecedores do Canadá](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: os fornecedores do Canadá são mostrados ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Mostrando todos os fornecedores por padrão

Em vez de mostrar nenhum dos fornecedores ao primeiro exibir a página pode queremos Mostrar *todos os* fornecedores primeiro, permitindo que o usuário reduzir a lista, digitando um nome de país na caixa de texto. Quando a caixa de texto estiver vazia, o `SuppliersBLL` da classe `GetSuppliersByCountry(country)` método é passado `Nothing` para seus *`country`* parâmetro de entrada. Isso `Nothing` valor, em seguida, é passado para a DAL `GetSupplierByCountry(country)` método, onde ele é convertido em um banco de dados `NULL` valor para o `@Country` parâmetro na consulta a seguir:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

A expressão `Country = NULL` sempre retorna falso, mesmo para os registros cujo `Country` coluna tem um `NULL` valor; portanto, nenhum registro será retornado.

Para retornar *todos os* fornecedores quando o caixa de texto de país está vazio, pode aumentar a `GetSuppliersByCountry(country)` método na BLL para invocar o `GetSuppliers()` método quando seu parâmetro país é `Nothing` e chamar a DAL `GetSuppliersByCountry(country)` método caso contrário. Isso terá o efeito de retornar o subconjunto apropriado de fornecedores e de todos os fornecedores quando nenhum é especificado quando o parâmetro country é incluído.

Alterar o `GetSuppliersByCountry(country)` método o `SuppliersBLL` classe para o seguinte:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Com essa alteração de `DeclarativeParams.aspx` página mostra todos os fornecedores quando visitado primeiro (ou sempre que o `CountryName` caixa de texto está vazia).


[![Todos os fornecedores são agora são mostradas por padrão](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: todos os fornecedores são agora é mostrada por padrão ([clique para exibir a imagem em tamanho normal](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Resumo

Para usar métodos com parâmetros de entrada, é necessário especificar os valores para os parâmetros no ObjectDataSource `SelectParameters` coleção. Permitir que diferentes tipos de parâmetros para o valor do parâmetro ser obtidos de fontes diferentes. O tipo de parâmetro padrão usa um valor embutido, mas apenas como facilmente (e sem uma linha de código) valores de parâmetro podem ser obtidos o querystring, variáveis de sessão, cookies e valores mesmo inseridos pelo usuário dos controles da Web na página.

Os exemplos que observamos neste tutorial ilustrado como usar valores de parâmetro declarativa. No entanto, pode haver ocasiões em que precisamos usar uma fonte de parâmetro que não está disponível, como a data e hora atuais ou, se nosso site estava usando a associação, a ID de usuário do visitante. Para esses cenários podemos definir os valores de parâmetro programaticamente antes do ObjectDataSource invocar o método do seu objeto subjacente. Veremos como fazer isso no [tutorial próximo](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-objectdatasource-vb.md)
> [Próximo](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
