---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Exibindo dados binários na Web dados controles (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial vamos examinar as opções para apresentar dados binários em uma página da Web, incluindo a exibição de um arquivo de imagem e o provisionamento de um link de 'Download' f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c3e2b75b74198877b19e0be3a6428cabaf2cf25
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366764"
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>Exibindo dados binários nos controles da Web de dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) ou [baixar PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> Neste tutorial vamos examinar as opções para apresentar dados binários em uma página da Web, incluindo a exibição de um arquivo de imagem e o provisionamento de um link 'Download' para um arquivo PDF.


## <a name="introduction"></a>Introdução

No tutorial anterior, criamos explorou as duas técnicas para a associação de dados binários com um modelo de dados subjacente do aplicativo s e usamos o controle FileUpload carregar arquivos em um navegador para o sistema de arquivos do web server s. Podemos var ainda para ver como associar os dados binários carregados com o modelo de dados. Ou seja, depois que um arquivo é carregado e salvo no sistema de arquivo, um caminho para o arquivo deve ser armazenado no registro de banco de dados apropriado. Se os dados estão sendo armazenados diretamente no banco de dados, em seguida, os dados binários carregados não precisam ser salvos no sistema de arquivo, mas devem ser inseridos no banco de dados.

Antes de examinarmos associando os dados com o modelo de dados, no entanto, deixe s primeiro examinar como fornecer os dados binários para o usuário final. Apresentação de dados de texto é bastante simple, mas como dados binários devem ser apresentados? Ele depende, é claro, o tipo de dados binários. Para imagens, provavelmente queremos exibir a imagem; para PDFs, documentos do Microsoft Word, arquivos ZIP e outros tipos de dados binários, fornecendo um link de Download é provavelmente mais apropriado.

Neste tutorial, veremos como apresentar os dados binários juntamente com seus dados de texto associado usando dados de controles da Web, como GridView e DetailsView. No próximo tutorial voltaremos nossa atenção ao associar um arquivo carregado com o banco de dados.

## <a name="step-1-providingbrochurepathvalues"></a>Etapa 1: Fornecer`BrochurePath`valores

O `Picture` coluna o `Categories` tabela já contém dados binários para as diversas imagens de categoria. Especificamente, o `Picture` coluna para cada registro contém o conteúdo binário de uma imagem de bitmap granulado, baixa qualidade, 16 cores. Cada imagem de categoria é 172 pixels de largo e 120 pixels de altura e consome aproximadamente 11 KB. Novidades mais, o conteúdo binário a `Picture` coluna incluirá um byte de 78 [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) cabeçalho deve ser removido antes de exibir a imagem. Essas informações de cabeçalho estão presentes porque o banco de dados Northwind tem suas raízes no Microsoft Access. No Access, os dados binários são armazenados usando o tipo de dados do objeto OLE, que usa esse cabeçalho. Por enquanto, veremos como retirar os cabeçalhos dessas imagens de baixa qualidade para exibir a imagem. Em um futuro tutorial, criaremos uma interface para a atualização de uma categoria s `Picture` coluna e substituir essas imagens de bitmap que usam cabeçalhos OLE com as imagens JPG equivalentes sem os cabeçalhos OLE desnecessários.

No tutorial anterior, vimos como usar o controle FileUpload. Portanto, você pode prosseguir e adicionar arquivos de folheto ao sistema de arquivo do web server s. Ao fazer isso, no entanto, não atualiza o `BrochurePath` coluna o `Categories` tabela. No próximo tutorial, veremos como fazer isso, mas por enquanto, é necessário fornecer manualmente os valores dessa coluna.

Este download s do tutorial, você encontrará sete arquivos folheto PDF no `~/Brochures` pasta, um para cada uma das categorias, exceto Frutos do mar. Omiti, intencionalmente, adicionando um folheto Frutos do mar para ilustrar como lidar com cenários em que nem todos os registros têm dados binários associados. Para atualizar o `Categories` da tabela com esses valores, clique com botão direito no `Categories` nó no Gerenciador de servidores e escolha Mostrar dados da tabela. Em seguida, insira os caminhos virtuais para os arquivos de folheto para cada categoria que tem um folheto, como mostra a Figura 1. Como não há nenhum folheto para a categoria de Frutos do mar, deixe seus `BrochurePath` valor da coluna s como `NULL`.


[![Insira manualmente os valores para a coluna de BrochurePath categorias tabela s](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Figura 1**: insira manualmente os valores para o `Categories` tabela s `BrochurePath` coluna ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Etapa 2: Fornecer um Link de Download para os folhetos em um GridView

Com o `BrochurePath` valores fornecidos para o `Categories` tabela, podemos está pronto para criar um GridView que lista cada categoria, juntamente com um link para baixar o folheto categoria s. Na etapa 4, ampliaremos este GridView para também exibir a imagem de categoria s.

Comece arrastando um GridView da caixa de ferramentas para o Designer do `DisplayOrDownloadData.aspx` página o `BinaryData` pasta. Definir o s GridView `ID` para `Categories` e por meio de GridView s marca inteligente, escolha vinculá-la a uma nova fonte de dados. Especificamente, associá-lo a um ObjectDataSource denominado `CategoriesDataSource` que recupera dados usando o `CategoriesBLL` objeto s `GetCategories()` método.


[![Criar um novo ObjectDataSource chamado CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![Recuperar a lista de categorias usando o método GetCategories()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Figura 4**: recupere a lista de categorias usando o `GetCategories()` método ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio adicionará automaticamente um BoundField para o `Categories` GridView para o `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` `DataColumn` s. Vá em frente e remova os `NumberOfProducts` BoundField desde o `GetCategories()` consulta de método s não recuperar essas informações. Também remover os `CategoryID` BoundField e renomeie o `CategoryName` e `BrochurePath` BoundFields `HeaderText` propriedades à categoria e folheto, respectivamente. Depois de fazer essas alterações, sua marcação declarativa de s do GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Exibir esta página por meio de um navegador (consulte a Figura 5). Cada uma das oito categorias está listada. As sete categorias com `BrochurePath` valores têm a `BrochurePath` valor exibido no BoundField respectivo. Frutos do mar, que tem um `NULL` de valor para o seu `BrochurePath`, exibe uma célula vazia.


[![Cada categoria é o nome, descrição e o valor de BrochurePath está listada](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Figura 5**: s de cada categoria nome, descrição, e `BrochurePath` valor é listado ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


Em vez de exibir o texto do `BrochurePath` coluna, queremos criar um link para a publicação. Para fazer isso, remova o `BrochurePath` BoundField e substituí-lo com um HyperLinkField. Definir o novo s HyperLinkField `HeaderText` propriedade folheto, seu `Text` propriedade folheto do modo de exibição e seu `DataNavigateUrlFields` propriedade `BrochurePath`.


![Adicionar um HyperLinkField para BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Figura 6**: adicionar um HyperLinkField para `BrochurePath`


Isso adicionará uma coluna de links para o controle GridView, como mostra a Figura 7. Clicar em um link de exibição folheto irá exibir o PDF diretamente no navegador ou solicitar que o usuário baixe o arquivo, dependendo se um leitor de PDF está instalado e as configurações do navegador s.


[![Uma categoria s folheto pode ser exibido clicando no Link de folheto do modo de exibição](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Figura 7**: uma categoria s folheto podem ser exibidos clicando no Link de folheto do modo de exibição ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![A categoria s folheto PDF é exibida](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Figura 8**: s categoria o PDF de folheto é exibida ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ocultando o texto do folheto de modo de exibição para categorias sem um folheto

Como mostra a Figura 7, o `BrochurePath` HyperLinkField exibe seus `Text` o valor de propriedade (exibição folheto) para todos os registros, independentemente se daí s um não -`NULL` valor para `BrochurePath`. É claro que, se `BrochurePath` é `NULL`, em seguida, o link será exibido como texto, como é o caso com a categoria de Frutos do Mar (consulte novamente a Figura 7). Em vez de exibir o texto do folheto do modo de exibição, pode ser interessante ter essas categorias sem um `BrochurePath` valor exibirá algum texto alternativo, como nenhum folheto disponível.

Para fornecer esse comportamento, precisamos usar o TemplateField cujo conteúdo é gerado por meio de uma chamada para um método de página que emite a saída apropriada com base no `BrochurePath` valor. Primeiro exploramos essa formatação técnica de volta a [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial.

Ativar o HyperLinkField em um TemplateField selecionando o `BrochurePath` HyperLinkField e, em seguida, clicar em converter este campo em um TemplateField link na caixa de diálogo Editar colunas.


![Converter o HyperLinkField em um TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Figura 9**: converter o HyperLinkField em um TemplateField


Isso criará um TemplateField com um `ItemTemplate` que contém um hiperlink Web controle cuja `NavigateUrl` propriedade está associada a `BrochurePath` valor. Substitua essa marcação com uma chamada ao método `GenerateBrochureLink`, passando o valor de `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Em seguida, crie uma `protected` método no ASP.NET página classe code-behind de s denominado `GenerateBrochureLink` que retorna um `string` e aceita um `object` como um parâmetro de entrada.


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Este método determina se passado `object` valor é um banco de dados `NULL` e, nesse caso, retorna uma mensagem indicando que a categoria não tem um folheto. Caso contrário, se houver um `BrochurePath` valor, ele s exibida em um hiperlink. Observe que, se o `BrochurePath` valor é apresentá-lo s passado para o [ `ResolveUrl(url)` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Esse método resolve passado *url*, substituindo o `~` caractere com o caminho virtual apropriado. Por exemplo, se o aplicativo está enraizado no `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` retornará `/Tutorial55/Brochures/Meat.pdf`.

Figura 10 mostra a página após essas alterações foram aplicadas. Observe que a categoria de Frutos do mar s `BrochurePath` campo agora exibe o texto do folheto de nenhum disponível.


[![O texto não folheto disponíveis é exibida para as categorias sem um folheto](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Figura 10**: O texto não folheto disponíveis é exibida para as categorias sem um folheto ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Etapa 3: Adicionando uma página da Web para exibir uma imagem de categoria s

Quando um usuário visita uma página ASP.NET, eles recebem a s HTML da página do ASP.NET. O HTML recebido é somente texto e não contém quaisquer dados binários. Quaisquer dados binários adicionais, como imagens, arquivos de som, aplicativos de Macromedia Flash, vídeos do Windows Media Player incorporado e assim por diante, existem como recursos separados no servidor web. O HTML contém referências a esses arquivos, mas não inclui o conteúdo real dos arquivos.

Por exemplo, em HTML os `<img>` elemento é usado para fazer referência a uma imagem, com o `src` atributo apontando para o arquivo de imagem da seguinte forma:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Quando um navegador recebe esse HTML, ela faz outra solicitação para o servidor web para recuperar o conteúdo binário do arquivo de imagem, que então são exibidos no navegador. O mesmo conceito se aplica a quaisquer dados binários. Na etapa 2, o folheto do não foi enviado para baixo para o navegador como parte da marcação HTML da página s. Em vez disso, o HTML renderizado fornecido hiperlinks que, quando clicado, causou o navegador solicitar o documento PDF diretamente.

Para exibir ou permitir que usuários baixem dados binários que residem no banco de dados, precisamos criar uma página da web separada que retorna os dados. Em nosso aplicativo, o campo de dados binários apenas um lá s armazenado diretamente na imagem s a categoria de banco de dados. Portanto, precisamos de uma página que, quando chamado, retorna os dados da imagem para uma determinada categoria.

Adicione uma nova página ASP.NET para o `BinaryData` pasta chamada `DisplayCategoryPicture.aspx`. Ao fazer isso, deixe a caixa de seleção de página mestra selecione desmarcada. Esta página espera que um `CategoryID` valor na cadeia de consulta e retorna os dados binários da categoria s `Picture` coluna. Como essa página retorna dados binários e nada mais, ele não precisa de qualquer marcação na seção HTML. Portanto, clique na guia fonte, no canto inferior esquerdo e remova todos a marcação de página s, exceto para o `<%@ Page %>` diretiva. Ou seja, `DisplayCategoryPicture.aspx` marcação declarativa de s deve consistir em uma única linha:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Se você vir as `MasterPageFile` de atributo no `<%@ Page %>` diretiva, removê-lo.

Na classe code-behind a s de página, adicione o seguinte código para o `Page_Load` manipulador de eventos:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Esse código começa lendo o `CategoryID` valor de cadeia de consulta em uma variável chamada `categoryID`. Em seguida, os dados da imagem são recuperados por meio de uma chamada para o `CategoriesBLL` classe s `GetCategoryWithBinaryDataByCategoryID(categoryID)` método. Esses dados são retornados ao cliente usando o `Response.BinaryWrite(data)` método, mas antes que isso é chamado, o `Picture` cabeçalho da coluna valor s OLE deve ser removido. Isso é feito criando uma `byte` matriz nomeada `strippedImageData` que conterá precisamente 78 caracteres menor do que está na `Picture` coluna. O [ `Array.Copy` método](https://msdn.microsoft.com/library/z50k9bft.aspx) é usado para copiar os dados do `category.Picture` começando na posição 78 sobre a `strippedImageData`.

O `Response.ContentType` propriedade especifica o [tipo de MIME](http://en.wikipedia.org/wiki/MIME) do conteúdo que está sendo retornado para que o navegador sabe como renderizá-lo. Uma vez que o `Categories` tabela s `Picture` a coluna é uma imagem de bitmap, o tipo MIME de bitmap é usado aqui (imagem bmp). Se você omitir o tipo MIME, a maioria dos navegadores ainda exibirá a imagem corretamente porque eles podem inferir o tipo com base no conteúdo do arquivo s binário de dados de imagem. No entanto, ele s prudente incluir o MIME digite quando possível. Consulte a [site da Internet Assigned Numbers Authority](http://www.iana.org/) para uma listagem completa dos [tipos de mídia MIME](http://www.iana.org/assignments/media-types/).

Com essa página foi criada, uma imagem de determinada categoria s pode ser exibida visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. A Figura 11 mostra a imagem de categoria s Bebidas, que pode ser exibida no `DisplayCategoryPicture.aspx?CategoryID=1`.


[![O s categoria Bebidas que imagem é exibida.](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Figura 11**: s a categoria de bebidas imagem é exibida ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


Se, ao visitar `DisplayCategoryPicture.aspx?CategoryID=categoryID`, você receberá uma exceção que lê não é possível converter o objeto do tipo 'System. DBNull' para o tipo 'System. Byte []', há duas coisas que podem estar causando isso. Primeiro, o `Categories` tabela s `Picture` coluna permitir `NULL` valores. O `DisplayCategoryPicture.aspx` página, no entanto, assume que há um não -`NULL` valor presente. O `Picture` propriedade do `CategoriesDataTable` não podem ser acessados diretamente se ele tiver um `NULL` valor. Se você quiser permitir `NULL` os valores para o `Picture` coluna, d deseja incluir a seguinte condição:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

O código acima presume que existe s alguns chamado de arquivo de imagem `NoPictureAvailable.gif` no `Images` pasta que você deseja exibir para essas categorias sem uma imagem.

Essa exceção também pode ser causada se o `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrução foi revertido para a lista de coluna da consulta principal s, que pode ocorrer se você estiver usando instruções SQL ad hoc e ve você execute novamente o Assistente para o s TableAdapter consulta principal. Verifique se a `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrução ainda inclui o `Picture` coluna.

> [!NOTE]
> Sempre que o `DisplayCategoryPicture.aspx` é visitado, o banco de dados é acessado e os dados de imagem da categoria especificada s são retornados. No entanto, se t categoria s imagem tenha sido alterado desde que o usuário tiver exibido pela última vez, isso é desperdício de esforço. Felizmente, HTTP permite *condicional obtém*. Com um GET condicional, o cliente que está fazendo a solicitação HTTP envia ao longo de um [ `If-Modified-Since` cabeçalho HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) que fornece a data e hora em que o cliente recuperado pela última vez esse recurso do servidor web. Se o conteúdo não foi alterado, pois isso especificado de data, o servidor web pode responder com uma [código de status não modificado (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) e abrir mão de enviar novamente o conteúdo de recurso solicitado s. Em resumo, essa técnica alivia o servidor web de ter que enviar conteúdo para um recurso se não tiver sido modificado desde que o cliente do último acesso.


Para implementar esse comportamento, no entanto, é necessário adicionar uma `PictureLastModified` coluna para o `Categories` tabela capturar quando o `Picture` coluna foi atualizada pela última vez, assim como código para verificar se há o `If-Modified-Since` cabeçalho. Para obter mais informações sobre o `If-Modified-Since` cabeçalho e o fluxo de trabalho GET condicional, consulte [HTTP GET condicional para Hackers RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) e [uma visão mais profunda na execução de solicitações de HTTP em uma página ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Etapa 4: Exibir as imagens de categoria em um GridView

Agora que temos uma página da web para exibir uma imagem de determinada categoria s, podemos exibir usando o [controle de Web de imagem](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) ou um HTML `<img>` elemento apontando para `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Imagens cuja URL é determinado pelos dados do banco de dados podem ser exibidas no DetailsView usando o ImageField ou GridView. Contém o ImageField `DataImageUrlField` e `DataImageUrlFormatString` as propriedades que funcionam como o s HyperLinkField `DataNavigateUrlFields` e `DataNavigateUrlFormatString` propriedades.

Permitir que o s aumentar o `Categories` GridView no `DisplayOrDownloadData.aspx` adicionando um ImageField para mostrar cada imagem de categoria s. Basta adicionar o ImageField e defina suas `DataImageUrlField` e `DataImageUrlFormatString` propriedades a serem `CategoryID` e `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivamente. Isso criará uma coluna de GridView que renderiza uma `<img>` elemento cuja `src` as referências ao atributo `DisplayCategoryPicture.aspx?CategoryID={0}`, onde {0} é substituído com a linha de GridView s `CategoryID` valor.


![Adicionar um ImageField a GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Figura 12**: adicionar um ImageField a GridView


Depois de adicionar o ImageField, sua sintaxe declarativa do GridView s deve ser parecido com Acalme seguintes:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Reserve um tempo para exibir esta página por meio de um navegador. Observe como cada registro agora inclui uma imagem para a categoria.


[![A categoria s imagem é exibida para cada linha](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Figura 13**: s categoria a imagem é exibida para cada linha ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>Resumo

Neste tutorial, examinamos como apresentar dados binários. Como os dados são apresentados dependem do tipo de dados. Para os arquivos de folheto PDF, podemos ofereceram ao usuário um folheto de exibição de link que, quando clicado, levou o usuário diretamente para o arquivo PDF. Para a imagem de s de categoria, podemos criado pela primeira vez uma página para recuperar e retornar os dados binários do banco de dados e, em seguida, usado nessa página para exibir cada imagem de s categoria em um GridView.

Agora que estamos ve examinamos como exibir dados binários, podemos está pronto para examinar como executar a inserção, atualizações e exclusões no banco de dados com os dados binários. O próximo tutorial, examinaremos como associar um arquivo carregado seu registro de banco de dados correspondente. O tutorial depois disso, veremos como atualizar os dados binários existentes, bem como excluir os dados binários quando seu registro associado é removido.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy e Dave Gardner. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](uploading-files-cs.md)
> [Próximo](including-a-file-upload-option-when-adding-a-new-record-cs.md)
