---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: "Exibindo dados binários na Web dados controles (VB) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, examine as opções para apresentar dados binários em uma página da Web, incluindo a exibição de um arquivo de imagem e o fornecimento de um link de 'Baixar' f..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: df79748bf5734ffcb9eb81ca089aeded0e63bdc5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Exibindo dados binários nos controles da Web de dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) ou [baixar PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> Neste tutorial, examine as opções para apresentar dados binários em uma página da Web, incluindo a exibição de um arquivo de imagem e o fornecimento de um link de 'Download' para um arquivo PDF.


## <a name="introduction"></a>Introdução

No tutorial anterior, explorados duas técnicas para a associação de dados binários com um modelo de dados subjacente do aplicativo s e usou o controle de carregamento de arquivos para carregar arquivos em um navegador para o sistema de arquivo do web server s. Podemos ver ainda para ver como associar os dados binários carregados com o modelo de dados. Ou seja, depois que um arquivo foi carregado e salvo no sistema de arquivo, um caminho para o arquivo deve armazenado no registro do banco de dados apropriado. Se os dados estão sendo armazenados diretamente no banco de dados, em seguida, os dados binários carregados não precisam ser salvos no sistema de arquivo, mas devem ser inseridos no banco de dados.

Antes de examinarmos associando os dados com o modelo de dados, no entanto, deixe-s primeiro examinar como fornecer os dados binários para o usuário final. Apresentação de dados de texto é simple o suficiente, mas como dados binários devem ser apresentados? Depende, é claro, o tipo de dados binários. Para imagens, provavelmente queremos exibir a imagem; para PDFs, documentos do Microsoft Word, arquivos ZIP e outros tipos de dados binários, fornecendo um link de Download é mais apropriado.

Neste tutorial, de que examinaremos como apresentar os dados binários juntamente com seus dados de texto associada usando dados controles da Web como o GridView e DetailsView. O tutorial Avançar voltaremos nossa atenção ao associar um arquivo carregado com o banco de dados.

## <a name="step-1-providingbrochurepathvalues"></a>Etapa 1: Fornecendo`BrochurePath`valores

O `Picture` coluna o `Categories` tabela já contém dados binários para as várias imagens de categoria. Especificamente, o `Picture` coluna para cada registro contém o conteúdo binário de uma imagem de bitmap granulados, baixa qualidade, 16 cores. Cada imagem de categoria é 172 pixels de largura e 120 pixels de altura e consome aproximadamente 11 KB. O que é mais, o conteúdo binário a `Picture` coluna inclui um 78 bytes [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) cabeçalho que deve ser removido antes de exibir a imagem. Essas informações de cabeçalho estão presentes porque o banco de dados Northwind tem suas raízes no Microsoft Access. No Access, os dados binários são armazenados usando o tipo de dados do objeto OLE, move sobre este cabeçalho. Por enquanto, veremos como Tire os cabeçalhos dessas imagens de baixa qualidade para exibir a imagem. Em um futuro tutorial, criaremos uma interface para a atualização de uma categoria s `Picture` coluna e substituir essas imagens de bitmap que usam cabeçalhos OLE com equivalentes imagens JPG sem o cabeçalho OLE desnecessário.

No tutorial anterior, vimos como usar o controle de carregamento de arquivos. Portanto, você pode prosseguir e adicionar arquivos de publicação para o sistema de arquivo do web server s. Ao fazer isso, no entanto, não atualiza o `BrochurePath` coluna o `Categories` tabela. No tutorial de Avançar, veremos como fazer isso, mas agora é preciso fornecer manualmente os valores dessa coluna.

Este download tutorial s, você encontrará sete arquivos de publicação PDF no `~/Brochures` pasta, um para cada categoria exceto Frutos do mar. Eu intencionalmente omitido adicionando uma publicação de Frutos do mar para ilustrar como lidar com cenários em que nem todos os registros com dados binários associados. Para atualizar o `Categories` da tabela com esses valores, com o botão direito no `Categories` nó do Gerenciador de servidores e escolha Mostrar dados da tabela. Em seguida, insira os caminhos virtuais para os arquivos de publicação para cada categoria que tenha uma publicação, como mostra a Figura 1. Como não há nenhuma publicação para a categoria de Frutos do mar, deixe seu `BrochurePath` valor da coluna s como `NULL`.


[![Insira manualmente os valores para a coluna de BrochurePath tabela s categorias](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Figura 1**: inserir manualmente os valores para o `Categories` tabela s `BrochurePath` coluna ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Etapa 2: Fornecer um Link de Download para os folhetos em um GridView

Com o `BrochurePath` valores fornecidos para o `Categories` tabela, podemos re pronto para criar um controle GridView que lista cada categoria junto com um link para baixar a publicação de categoria s. Na etapa 4, estenderemos esse GridView para também exibir a imagem de categoria s.

Iniciar arrastando um GridView da caixa de ferramentas para o Designer do `DisplayOrDownloadData.aspx` página o `BinaryData` pasta. Definir o GridView s `ID` para `Categories` e por meio de GridView s marca inteligente, escolha para associá-lo para uma nova fonte de dados. Especificamente, associá-lo a um ObjectDataSource denominado `CategoriesDataSource` que recupera dados usando o `CategoriesBLL` objeto s `GetCategories()` método.


[![Criar um novo ObjectDataSource denominado CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Recuperar a lista de categorias usando o método GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Figura 4**: recuperar a lista de categorias usando o `GetCategories()` método ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio adicionará automaticamente um BoundField para o `Categories` GridView para o `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` `DataColumn` s. Vá em frente e remover o `NumberOfProducts` BoundField desde o `GetCategories()` consulta método s não recuperar essas informações. Também removerá o `CategoryID` BoundField e renomear o `CategoryName` e `BrochurePath` BoundFields `HeaderText` propriedades para a categoria e a publicação, respectivamente. Depois de fazer essas alterações, a marcação de declarativa s GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Exibir esta página por meio de um navegador (consulte a Figura 5). Cada um dos oito categorias está listada. As sete categorias com `BrochurePath` valores têm o `BrochurePath` valor exibido do respectivo BoundField. Frutos do mar, que tem um `NULL` valor para seu `BrochurePath`, exibe uma célula vazia.


[![Cada categoria é o nome, descrição e o valor de BrochurePath está listada](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Figura 5**: s de cada categoria nome, descrição, e `BrochurePath` valor é listado ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Em vez de exibir o texto do `BrochurePath` coluna, quer criar um link para a publicação. Para fazer isso, remova o `BrochurePath` BoundField e substitua-o por um HyperLinkField. Definir o novo s HyperLinkField `HeaderText` propriedade de publicação, seu `Text` propriedade para exibir a publicação e sua `DataNavigateUrlFields` propriedade `BrochurePath`.


![Adicionar um HyperLinkField para BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Figura 6**: adicionar um HyperLinkField para`BrochurePath`


Isso adicionará uma coluna de links GridView, como mostra a Figura 7. Clicar em um link Exibir publicação irá exibir o PDF diretamente no navegador ou solicitar o usuário para baixar o arquivo, dependendo se um leitor de PDF está instalado e as configurações do navegador s.


[![Uma categoria s publicação pode ser exibida clicando no Link de publicação do modo de exibição](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Figura 7**: A categoria s publicação podem ser exibidos clicando no Link de publicação do modo de exibição ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![A categoria s PDF de publicação é exibida](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Figura 8**: A categoria s PDF de publicação é exibido ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ocultando o texto de exibição da publicação para categorias sem uma publicação

Como mostra a Figura 7, o `BrochurePath` HyperLinkField exibe seu `Text` o valor de propriedade (exibição de publicação) para todos os registros, independentemente se há s não`NULL` valor para `BrochurePath`. É claro que, se `BrochurePath` é `NULL`, em seguida, o link será exibido como texto, como é o caso com a categoria de Frutos do Mar (consulte novamente a Figura 7). Em vez de exibir o texto de exibição de publicação, talvez seja interessante ter nessas categorias sem um `BrochurePath` valor exibir um texto alternativo, como não há publicação disponível.

Para fornecer esse comportamento, precisamos usar um TemplateField cujo conteúdo é gerado por meio de uma chamada para um método de página que emite a saída apropriada com base no `BrochurePath` valor. Podemos primeiro explorados isso formatação técnica novamente o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial.

Ativar o HyperLinkField em um TemplateField selecionando o `BrochurePath` HyperLinkField e, em seguida, clicando em converter este campo em um TemplateField link na caixa de diálogo Editar colunas.


![Converter o HyperLinkField em um TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Figura 9**: converter o HyperLinkField em um TemplateField


Isso criará um TemplateField com um `ItemTemplate` que contém um hiperlink Web controle cuja `NavigateUrl` propriedade está vinculada a `BrochurePath` valor. Substitua essa marcação com uma chamada ao método `GenerateBrochureLink`, passando o valor de `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Em seguida, crie um `Protected` método no ASP.NET página classe code-behind de s denominado `GenerateBrochureLink` que retorna um `String` e aceita um `Object` como um parâmetro de entrada.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Este método determina se passado `Object` valor é um banco de dados `NULL` e, em caso afirmativo, retorna uma mensagem indicando que a categoria não tem uma publicação. Caso contrário, se houver um `BrochurePath` valor, ele s exibido em um hiperlink. Observe que, se o `BrochurePath` valor é apresentá-lo s passado para o [ `ResolveUrl(url)` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Este método resolve transmitido *url*, substituindo o `~` caractere com o caminho virtual apropriado. Por exemplo, se o aplicativo tem raiz em `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` retornará `/Tutorial55/Brochures/Meat.pdf`.

A Figura 10 mostra a página após essas alterações foram aplicadas. Observe que a categoria de Frutos do mar s `BrochurePath` campo agora exibe o texto não há publicação disponível.


[![O texto não há publicação disponível é exibida para as categorias sem uma publicação](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Figura 10**: O texto não há publicação disponível é exibida para as categorias sem uma publicação ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Etapa 3: Adicionando uma página da Web para exibir uma imagem de categoria s

Quando um usuário acessa uma página ASP.NET, eles recebem a s HTML da página do ASP.NET. O HTML recebido é somente texto e não contém quaisquer dados binários. Quaisquer dados binários adicionais, como imagens, arquivos de som, aplicativos do Macromedia Flash, vídeos do Windows Media Player incorporado e assim por diante, existam como recursos separados no servidor web. O HTML contém referências a esses arquivos, mas não inclui o conteúdo real dos arquivos.

Por exemplo, em HTML a `<img>` elemento é usado para fazer referência a uma imagem, com o `src` atributo apontando para o arquivo de imagem da seguinte forma:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Quando um navegador recebe esse HTML, faz outra solicitação para o servidor web para recuperar o conteúdo binário do arquivo de imagem, que então são exibidos no navegador. O mesmo conceito se aplica a todos os dados binários. Na etapa 2, a publicação não foi enviada para baixo para o navegador como parte da marcação HTML da página s. Em vez disso, o HTML renderizado fornecido hiperlinks que, quando clicado, causou o navegador solicitar o documento PDF diretamente.

Para exibir ou permitir que usuários baixem dados binários que residem no banco de dados, é preciso criar uma página da web separado que retorna os dados. Em nosso aplicativo, o campo de dados binários apenas um lá s armazenado diretamente na imagem s a categoria de banco de dados. Portanto, precisamos de uma página que, quando chamado, retorna os dados da imagem para uma determinada categoria.

Adicione uma nova página ASP.NET para o `BinaryData` pasta denominada `DisplayCategoryPicture.aspx`. Ao fazer isso, marque a caixa de seleção Selecionar página mestra. Esta página espera um `CategoryID` valor na querystring e retorna os dados binários da categoria s `Picture` coluna. Como essa página retorna dados binários e nada mais, ele não precisa de qualquer marcação na seção de HTML. Portanto, clique na guia fonte, no canto inferior esquerdo e remova todos de marcação da página s, exceto para o `<%@ Page %>` diretiva. Ou seja, `DisplayCategoryPicture.aspx` declarativo s deve consistir em uma única linha:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Se você vir o `MasterPageFile` atributo o `<%@ Page %>` diretiva, removê-lo.

Na classe code-behind de páginas, adicione o seguinte código para o `Page_Load` manipulador de eventos:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Esse código inicia lendo o `CategoryID` querystring valor em uma variável chamada `categoryID`. Em seguida, os dados da imagem são recuperados por meio de uma chamada para o `CategoriesBLL` classe s `GetCategoryWithBinaryDataByCategoryID(categoryID)` método. Esses dados são retornados para o cliente usando o `Response.BinaryWrite(data)` método, mas antes que isso é chamado, o `Picture` cabeçalho da coluna valor s OLE deve ser removido. Isso é feito criando um `Byte` matriz chamada `strippedImageData` que armazenará precisamente 78 caracteres menor do que no `Picture` coluna. O [ `Array.Copy` método](https://msdn.microsoft.com/library/z50k9bft.aspx) é usado para copiar os dados de `category.Picture` começando na posição 78 sobre a `strippedImageData`.

O `Response.ContentType` propriedade especifica o [tipo MIME](http://en.wikipedia.org/wiki/MIME) do conteúdo que está sendo retornado de modo que o navegador sabe como processá-lo. Como o `Categories` tabela s `Picture` a coluna é uma imagem de bitmap, o tipo MIME de bitmap é usado aqui (imagem bmp). Se você omitir o tipo MIME, a maioria dos navegadores ainda exibirá a imagem corretamente porque eles podem inferir o tipo com base no conteúdo do arquivo s binário de dados de imagem. No entanto, ele prudente incluem MIME tipo quando possível. Consulte o [site de s Internet Assigned Numbers Authority](http://www.iana.org/) para uma lista completa de [tipos de mídia MIME](http://www.iana.org/assignments/media-types/).

Com esta página foi criada, uma imagem de categoria específico s pode ser exibida visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. A Figura 11 mostra a imagem de categoria s Bebidas, que pode ser exibida no `DisplayCategoryPicture.aspx?CategoryID=1`.


[![O s de categoria de bebidas que imagem é exibida](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Figura 11**: A categoria de bebidas s imagem é exibida ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Se, ao visitar `DisplayCategoryPicture.aspx?CategoryID=categoryID`, você receberá uma exceção que lê não é possível converter o objeto do tipo 'System. DBNull' para o tipo 'System. Byte []', há duas coisas que podem ter causado isso. Primeiro, o `Categories` tabela s `Picture` coluna permitir `NULL` valores. O `DisplayCategoryPicture.aspx` página, no entanto, assume que há não`NULL` valor presente. O `Picture` propriedade o `CategoriesDataTable` não pode ser acessado diretamente se ele tem um `NULL` valor. Se você quiser permitir `NULL` valores para o `Picture` coluna d deseja incluir a seguinte condição:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

O código acima presume que existem s alguns imagem arquivo chamado `NoPictureAvailable.gif` no `Images` pasta que você deseja exibir para essas categorias sem uma imagem.

Essa exceção também pode ser causada se o `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrução voltou a lista de colunas de consulta principal s, que pode ocorrer se você estiver usando instruções SQL ad hoc e que você execute novamente o Assistente para o s TableAdapter consulta principal. Verifique se que `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrução ainda inclui o `Picture` coluna.

> [!NOTE]
> Toda vez que o `DisplayCategoryPicture.aspx` é visitada, o banco de dados é acessado e os dados de imagem de categoria especificada s são retornados. Se t categoria s imagem tenha sido alterado desde que o usuário tem última exibida, no entanto, esse é inútil esforço. Felizmente, HTTP permite *condicional obtém*. Com GET condicional, o cliente faz a solicitação HTTP envia ao longo de um [ `If-Modified-Since` cabeçalho HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) que fornece a data e hora que o cliente recuperou última esse recurso do servidor web. Se o conteúdo não foi alterado desde a data especificado, o servidor web pode responder com uma [código de status não modificado (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) e abrir mão retornando o conteúdo do recurso solicitado s. Em resumo, essa técnica libera o servidor web de ter que enviar conteúdo para um recurso se ele não tiver sido modificado desde que o cliente do último acesso.


Para implementar esse comportamento, no entanto, é necessário adicionar um `PictureLastModified` coluna para o `Categories` tabela da qual capturar quando o `Picture` coluna da última atualização, bem como o código para verificar a `If-Modified-Since` cabeçalho. Para obter mais informações sobre o `If-Modified-Since` cabeçalho e o fluxo de trabalho GET condicional, consulte [HTTP GET condicional para Hackers RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) e [um mais profunda examinar executar solicitações HTTP em uma página ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Etapa 4: Exibir as imagens de categoria em um controle GridView

Agora que temos uma página da web para exibir uma imagem de categoria específico s, podemos exibir usando o [controle Image Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) ou um HTML `<img>` elemento apontando para `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Imagens cuja URL é determinado pelo banco de dados podem ser exibidas no GridView ou usando o ImageField de DetailsView. Contém o ImageField `DataImageUrlField` e `DataImageUrlFormatString` propriedades que funcionam como os s HyperLinkField `DataNavigateUrlFields` e `DataNavigateUrlFormatString` propriedades.

Permitir que o s aumentar o `Categories` GridView no `DisplayOrDownloadData.aspx` adicionando um ImageField para mostrar cada imagem de categoria s. Basta adicionar o ImageField e definir seu `DataImageUrlField` e `DataImageUrlFormatString` propriedades `CategoryID` e `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivamente. Isso criará uma coluna de GridView que renderiza uma `<img>` elemento cujo `src` as referências ao atributo `DisplayCategoryPicture.aspx?CategoryID={0}`, onde {0} é substituído com a linha GridView s `CategoryID` valor.


![Adicionar um ImageField a GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Figura 12**: adicionar um ImageField a GridView


Depois de adicionar o ImageField, sua sintaxe declarativa do GridView s deve ser parecido com soothe seguintes:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Dedicar um tempo para exibir esta página por meio de um navegador. Observe como cada registro agora inclui uma imagem para a categoria.


[![A categoria s imagem será exibida para cada linha](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Figura 13**: s categoria a imagem é exibida para cada linha ([clique para exibir a imagem em tamanho normal](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Resumo

Neste tutorial, examinamos como apresentar dados binários. Como os dados são apresentados depende do tipo de dados. Para os arquivos de publicação PDF, podemos ofereceram ao usuário uma publicação de exibição de link que, quando clicado, levou o usuário diretamente para o arquivo PDF. Para a imagem de s categoria, é criado pela primeira vez uma página para recuperar e retornar os dados binários do banco de dados e usado, em seguida, essa página para exibir cada imagem de s categoria em um controle GridView.

Agora que possamos ve examinamos como exibir dados binários, podemos re pronto para examinar como executar insert, atualizações e exclusões no banco de dados com os dados binários. O seguinte tutorial, examinaremos como associar um arquivo carregado com o seu registro de banco de dados correspondente. O tutorial depois disso, veremos como atualizar dados binários existentes, bem como excluir os dados binários quando seu registro associado é removido.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Teresa Murphy e Dave Gardner. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](uploading-files-vb.md)
[Próximo](including-a-file-upload-option-when-adding-a-new-record-vb.md)
