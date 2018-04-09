---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Carregamento de arquivos (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como permitir que os usuários carreguem arquivos binários (como documentos do Word ou PDF) para seu site da Web onde eles podem ser armazenados no sistema de arquivos do servidor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fbc4aaf80ac7e0f960e140b492055fe35cd2b6ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="uploading-files-vb"></a>Carregamento de arquivos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) ou [baixar PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Saiba como permitir que os usuários carreguem arquivos binários (como documentos do Word ou PDF) para seu site da Web onde eles podem ser armazenados no sistema de arquivos do servidor ou o banco de dados.


## <a name="introduction"></a>Introdução

Todos os tutoriais funcionaram ve examinou até o momento exclusivamente com dados de texto. No entanto, muitos aplicativos têm modelos de dados que capturem o texto e dados binários. Um site encontros on-line pode permitir que os usuários carregue uma imagem para associar ao seu perfil. Um site de recrutamento pode permitir que os usuários carregar seu resume como um documento do Microsoft Word ou PDF.

Trabalhar com dados binários adiciona um novo conjunto de desafios. É necessário decidir como os dados binários são armazenados no aplicativo. A interface usada para inserir novos registros deve ser atualizada para permitir que o usuário carregar um arquivo de seu computador e etapas adicionais que devem ser executadas para exibir ou fornecer um meio para baixar um registro s os dados binários associados. Este tutorial e as próximas três exploraremos como hurdle esses desafios. No final desses tutoriais será criamos um aplicativo totalmente funcional que associa uma imagem e a publicação PDF com cada categoria. Neste tutorial específico vamos examinar técnicas diferentes para armazenar dados binários e explorar como habilitar usuários carregar um arquivo de seu computador e tiver salvo no sistema de arquivo do web server s.

> [!NOTE]
> Dados binários que faz parte de um modelo de dados do aplicativo s às vezes são conhecidos como um [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), o acrônimo de objeto binário grande. Nesses tutoriais, optou por usar os dados binários de terminologia, embora o termo BLOB é sinônimo.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Etapa 1: Criar o trabalho com páginas da Web de dados binários

Antes de começar a explorar os desafios associados com a adição de suporte para dados binários, permitem s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial e as próximas três. Comece adicionando uma nova pasta chamada `BinaryData`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Adicionar as páginas ASP.NET para os binários tutoriais de dados relacionados](uploading-files-vb/_static/image1.gif)

**Figura 1**: adicionar as páginas ASP.NET para os binários tutoriais de dados relacionados


Como em outras pastas, `Default.aspx` no `BinaryData` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image2.png))


Por fim, adicione estas páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após o aprimorando GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para trabalhar com os tutoriais de dados binários.


![O mapa do Site agora inclui entradas para trabalhar com os tutoriais de dados binários](uploading-files-vb/_static/image3.gif)

**Figura 3**: O mapa de Site agora inclui entradas para trabalhar com os tutoriais de dados binários


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Etapa 2: Decidir onde armazenar os dados binários

Dados binários que está associados com o modelo de dados do aplicativo s podem ser armazenados em um dos dois locais: no sistema de arquivos de servidor s web com uma referência para o arquivo armazenado no banco de dados; ou diretamente no banco de dados (consulte a Figura 4). Cada abordagem tem seu próprio conjunto de prós e contras e merece uma discussão mais detalhada.


[![Dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dados](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dados ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image4.png))


Imagine que queremos estender o banco de dados Northwind para associar uma imagem de cada produto. Uma opção seria armazenar esses arquivos de imagem no sistema de arquivo do web server s e registre o caminho no `Products` tabela. Com essa abordagem, d adicionamos uma `ImagePath` coluna para o `Products` tabela de tipo `varchar(200)`, talvez. Quando um usuário carregar uma imagem de Chai, imagem poderão ser armazenada em sistema de arquivos de servidor s web no `~/Images/Tea.jpg`, onde `~` representa o caminho físico do aplicativo s. Ou seja, se o site da web raiz no caminho físico `C:\Websites\Northwind\`, `~/Images/Tea.jpg` seria equivalente a `C:\Websites\Northwind\Images\Tea.jpg`. Depois de carregar o arquivo de imagem, d atualizamos o registro Chai o `Products` tabela para que seu `ImagePath` coluna referenciada o caminho da nova imagem. Podemos usar `~/Images/Tea.jpg` ou apenas `Tea.jpg` se decidimos que todas as imagens de produto devem ser colocadas em aplicativo s `Images` pasta.

As principais vantagens de armazenar os dados binários no sistema de arquivos são:

- **Facilidade de implementação** conforme veremos em breve, armazenar e recuperar dados binários armazenados diretamente no banco de dados envolvem um pouco mais código que ao trabalhar com dados por meio do sistema de arquivos. Além disso, para que um usuário exibir ou baixar os dados binários que eles devem ser apresentados com uma URL para que os dados. Se os dados residem no sistema de arquivo do web server s, a URL é simples. Se os dados são armazenados no banco de dados, no entanto, uma página da web precisa ser criado que irá recuperar e retornar os dados do banco de dados.
- **Acesso mais amplo para os dados binários** os dados binários podem precisar ser acessível para outros serviços ou aplicativos, que não é possível extrair os dados do banco de dados. Por exemplo, as imagens associadas a cada produto podem também precisam estar disponíveis para usuários por meio de [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), caso em que podemos d deseja armazenar os dados binários no sistema de arquivos.
- **Desempenho** se os dados binários são armazenados no sistema de arquivos, o congestionamento de rede e a demanda entre o servidor de banco de dados e o servidor web será menor do que se os dados binários são armazenados diretamente no banco de dados.

A principal desvantagem de armazenar dados binários no sistema de arquivos é que ele separa os dados do banco de dados. Se um registro é excluído do `Products` tabela, o arquivo associado no sistema de arquivo do web server s não é excluída automaticamente. Precisamos escrever código extra para excluir o arquivo ou o sistema de arquivos será ficar cheia de arquivos não utilizados, órfãos. Além disso, ao fazer backup de banco de dados, é deve se certificar de fazer backups dos dados binários associados no sistema de arquivos, também. Mover o banco de dados para outro site ou servidor representa semelhante desafios.

Como alternativa, os dados binários podem ser armazenados diretamente em um banco de dados do Microsoft SQL Server 2005 por meio da criação de uma coluna do tipo `varbinary`. Como com outros tipos de dados de comprimento variável, você pode especificar um comprimento máximo dos dados binários que podem ser mantidos nesta coluna. Por exemplo, para reservar no máximo 5.000 bytes usar `varbinary(5000)`; `varbinary(MAX)` permite que o tamanho máximo de armazenamento, cerca de 2 GB.

A principal vantagem de armazenar dados binários diretamente no banco de dados é grande união entre os dados binários e o registro do banco de dados. Isso simplifica as tarefas de administração de banco de dados, como backups ou mover o banco de dados para um site ou servidor diferente. Além disso, excluir um registro exclui automaticamente os dados binários correspondentes. Também são mais sutis benefícios de armazenar os dados binários no banco de dados. Consulte [armazenar binário arquivos diretamente no banco de dados usando o ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) para obter uma discussão mais detalhada.

> [!NOTE]
> No Microsoft SQL Server 2000 e versões anteriores, o `varbinary` tipo de dados tem um limite máximo de 8.000 bytes. Para armazenar até 2 GB de dados binários a [ `image` tipo de dados](https://msdn.microsoft.com/library/ms187993.aspx) precisa ser usada em vez disso. Com a adição de `MAX` no SQL Server 2005, no entanto, o `image` tipo de dados foi preterido. -S ainda tem suporte para versões anteriores compatibilidade, mas a Microsoft anunciou que o `image` tipo de dados será removido em uma versão futura do SQL Server.


Se você estiver trabalhando com um modelo de dados mais antigo que você pode ver o `image` tipo de dados. O banco de dados Northwind s `Categories` tabela tem um `Picture` coluna que pode ser usada para armazenar os dados binários de um arquivo de imagem para a categoria. Como o banco de dados Northwind tem suas raízes no Microsoft Access e as versões anteriores do SQL Server, essa coluna é do tipo `image`.

Para este tutorial e as próximas três, vamos usar ambas as abordagens. O `Categories` tabela já tiver um `Picture` coluna para armazenar o conteúdo binário de uma imagem para a categoria. Vamos adicionar uma coluna adicional, `BrochurePath`, para armazenar um caminho para um PDF no sistema de arquivos da web server s que pode ser usado para fornecer uma visão geral de qualidade de impressão, elegante da categoria.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Etapa 3: Adicionando a`BrochurePath`coluna para o`Categories`tabela

Atualmente, a tabela de categorias tem apenas quatro colunas: `CategoryID`, `CategoryName`, `Description`, e `Picture`. Além desses campos, precisamos adicionar um novo que aponta para a publicação de categoria s (se houver). Para adicionar essa coluna, vá para o Gerenciador de servidores, fazer uma busca detalhada nas tabelas, com o botão direito no `Categories` da tabela e escolha Abrir definição de tabela (consulte a Figura 5). Se você não vir o Gerenciador de servidores, ativá-lo selecionando a opção de Gerenciador de servidores no menu Exibir, ou pressione Ctrl + Alt + S.

Adicionar um novo `varchar(200)` coluna para o `Categories` tabela denominada `BrochurePath` e permite `NULL` s e clique no ícone Salvar (ou pressione Ctrl + S).


[![Adicionar uma coluna BrochurePath à tabela de categorias](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: adicionar um `BrochurePath` coluna para o `Categories` tabela ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Etapa 4: Atualizar a arquitetura para usar o`Picture`e`BrochurePath`colunas

O `CategoriesDataTable` na dados acesso DAL (camada) atualmente tem quatro `DataColumn` definida: `CategoryID`, `CategoryName`, `Description`, e `NumberOfProducts`. Quando criamos originalmente esta DataTable no [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, o `CategoriesDataTable` tinha apenas as três primeiras colunas; o `NumberOfProducts` coluna foi adicionada a [mestre/detalhes usando um com marcadores Lista de registros mestre com DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) tutorial.

Conforme discutido em *criando uma camada de acesso a dados*, tabelas de dados no conjunto de dados tipado compõem os objetos de negócios. Os TableAdapters são responsáveis para se comunicar com o banco de dados e preencher os objetos de negócios com os resultados da consulta. O `CategoriesDataTable` é preenchida pelo `CategoriesTableAdapter`, que tem três métodos de recuperação de dados:

- `GetCategories()` executa a consulta principal do TableAdapter s e retorna o `CategoryID`, `CategoryName`, e `Description` campos de todos os registros de `Categories` tabela. A consulta principal é o que é usado pelo gerado automaticamente `Insert` e `Update` métodos.
- `GetCategoryByCategoryID(categoryID)` Retorna o `CategoryID`, `CategoryName`, e `Description` campos da categoria cujo `CategoryID` é igual a *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Retorna o `CategoryID`, `CategoryName`, e `Description` campos para todos os registros de `Categories` tabela. Também usa uma subconsulta para retornar o número de produtos associados a cada categoria.

Observe que nenhum das consultas retornam o `Categories` tabela s `Picture` ou `BrochurePath` colunas; nem faz a `CategoriesDataTable` fornecer `DataColumn` s para esses campos. Para trabalhar com a imagem e `BrochurePath` propriedades, é preciso adicioná-los ao primeiro o `CategoriesDataTable` e, em seguida, atualize o `CategoriesTableAdapter` classe para retornar essas colunas.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Adicionando o`Picture`e`BrochurePath``DataColumn` s

Comece adicionando essas duas colunas para o `CategoriesDataTable`. Clique com botão direito no `CategoriesDataTable` cabeçalho s, selecione Adicionar no menu de contexto e, em seguida, escolha a opção de coluna. Isso criará um novo `DataColumn` na DataTable nomeada `Column1`. Renomear esta coluna como `Picture`. Na janela Propriedades, defina o `DataColumn` s `DataType` propriedade `System.Byte[]` (isso não é uma opção na lista suspensa; você precisa digitá-la).


[![Criar uma imagem chamada DataColumn cujo tipo de dados é [System]](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: criar um `DataColumn` nomeado `Picture` cujo `DataType` é `System.Byte[]` ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image8.png))


Adicione outro `DataColumn` a DataTable, nomeando- `BrochurePath` usando o padrão `DataType` valor (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Retornando o`Picture`e`BrochurePath`valores do TableAdapter

Com esses dois `DataColumn` s adicionado para o `CategoriesDataTable`, podemos re pronto para atualizar o `CategoriesTableAdapter`. Poderíamos ter ambos os valores de coluna retornados na consulta TableAdapter principal, mas isso seria trazer de volta os dados binários toda vez que o `GetCategories()` método foi chamado. Em vez disso, vamos s atualizar a consulta do TableAdapter principal para trazer de volta `BrochurePath` e criar um método de recuperação de dados adicionais que retorna uma determinada categoria s `Picture` coluna.

Para atualizar a consulta do TableAdapter principal, clique duas vezes no `CategoriesTableAdapter` cabeçalho s e escolha a opção de configurar o menu de contexto. Isso abre o Assistente de configuração do adaptador de tabela, que podemos ve visto em um número de tutoriais anteriores. Atualize a consulta para trazer de volta a `BrochurePath` e clique em Concluir.


[![Atualizar a lista de colunas na instrução SELECT para retornar também BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: atualizar a lista de colunas no `SELECT` instrução para retornar também `BrochurePath` ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image10.png))


Ao usar instruções SQL ad hoc para o TableAdapter, atualizando a lista de colunas na consulta principal atualiza a lista de colunas para todos os `SELECT` métodos no TableAdapter de consulta. Isso significa que o `GetCategoryByCategoryID(categoryID)` método foi atualizado para retornar o `BrochurePath` coluna, que pode ser o que é destinado. No entanto, ela também atualizada a lista de colunas no `GetCategoriesAndNumberOfProducts()` método, removendo a subconsulta que retorna o número de produtos para cada categoria! Portanto, precisamos atualizar esse método s `SELECT` consulta. Clique duas vezes no `GetCategoriesAndNumberOfProducts()` método, escolha Configure e reverter o `SELECT` consulta para seu valor original:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Em seguida, crie um novo método de TableAdapter que retorna uma determinada categoria s `Picture` valor da coluna. Clique com botão direito no `CategoriesTableAdapter` cabeçalho s e escolha a opção de adicionar consulta para iniciar o Assistente de configuração de consulta do TableAdapter. A primeira etapa deste assistente pergunta que se quisermos para consultar dados usando uma instrução de SQL ad hoc, um novo procedimento armazenado, ou um existente. Use SQL instruções SELECT e clique em Avançar. Já que estamos será retornada uma linha, escolha o SELECT que retorna a opção de linhas da segunda etapa.


[![Selecione as opção Use SQL statements](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: selecione as opção Use SQL statements ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image12.png))


[![Desde que a consulta retornará um registro da tabela de categorias, escolha SELECT que retorna linhas](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: desde que a consulta retornará um registro da tabela de categorias, escolha Selecionar que retorna linhas ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image14.png))


Na terceira etapa, digite a seguinte consulta SQL e clique em Avançar:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

A última etapa é escolher o nome para o novo método. Use `FillCategoryWithBinaryDataByCategoryID` e `GetCategoryWithBinaryDataByCategoryID` para o preenchimento uma DataTable e retornar uma DataTable padrões, respectivamente. Clique em Concluir para concluir o assistente.


[![Escolha os nomes para os métodos TableAdapter](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: escolha os nomes para os métodos TableAdapter s ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Depois de concluir o Assistente de configuração de consulta de adaptador de tabela, você verá uma caixa de diálogo informando que o novo texto de comando retorna dados com esquema diferente do esquema da consulta principal. Em resumo, o assistente é observar que a consulta principal do TableAdapter s `GetCategories()` retorna um esquema diferente daquele que acabou de criar. Mas esse é o que queremos, portanto você pode ignorar essa mensagem.


Além disso, tenha em mente que se você estiver usando instruções SQL ad hoc e use o Assistente para alterar a consulta principal do TableAdapter s posteriormente no tempo, ele irá modificar o `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` lista de colunas de instrução s para incluir apenas as colunas das consulta principal (ou seja, ela removerá a `Picture` coluna da consulta). Você terá que atualizar manualmente a lista de colunas para retornar o `Picture` coluna, semelhante ao que fez com o `GetCategoriesAndNumberOfProducts()` método anteriormente nesta etapa.

Depois de adicionar os dois `DataColumn` s para o `CategoriesDataTable` e `GetCategoryWithBinaryDataByCategoryID` método para o `CategoriesTableAdapter`, essas classes no Designer de conjunto de dados tipado devem ser semelhante a captura de tela na Figura 11.


![O Designer de conjunto de dados inclui as novas colunas e o método](uploading-files-vb/_static/image11.gif)

**Figura 11**: O Designer de conjunto de dados inclui as novas colunas e o método


## <a name="updating-the-business-logic-layer-bll"></a>Atualizar a camada de lógica de negócios (BLL)

Com a DAL atualizada, tudo o que permanece é aumentar os negócios lógica BLL (camada) para incluir um método para o novo `CategoriesTableAdapter` método. Adicione o seguinte método para o `CategoriesBLL` classe:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Etapa 5: Carregar um arquivo do cliente para o servidor Web

Durante a coleta de dados binários, muitas vezes, esses dados são fornecidos por um usuário final. Para capturar essas informações, o usuário precisa ser capaz de carregar um arquivo de seu computador para o servidor web. Os dados carregados, então, precisam ser integrado com o modelo de dados, o que pode significar que salvar o arquivo para o sistema de arquivo do web server s e adição de um caminho para o arquivo no banco de dados ou gravar o conteúdo binário diretamente no banco de dados. Nesta etapa, examinaremos como permitir que um usuário carregar arquivos de seu computador para o servidor. O tutorial Avançar voltaremos nossa atenção para integrar o arquivo carregado com o modelo de dados.

O ASP.NET 2.0 novo [controle FileUpload Web](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) fornece um mecanismo para que os usuários enviar um arquivo do seu computador para o servidor web. O controle de carregamento de arquivos é renderizado como um `<input>` elemento cujo `type` atributo é definido como arquivo, quais navegadores são exibidos como uma caixa de texto com o botão Procurar. Clicando no botão Procurar abre uma caixa de diálogo na qual o usuário pode selecionar um arquivo. Quando o formulário é enviado de volta, o conteúdo de s do arquivo selecionado é enviado junto com a postagem. No lado do servidor, informações sobre o arquivo carregado são acessíveis por meio das propriedades de s do controle de carregamento de arquivos.

Para demonstrar o carregamento de arquivos, abra o `FileUpload.aspx` página o `BinaryData` pasta, arraste um controle de carregamento de arquivos da caixa de ferramentas para o Designer e definir o controle s `ID` propriedade `UploadTest`. Em seguida, adicione um controle de botão Web definindo seu `ID` e `Text` propriedades `UploadButton` e carregar o arquivo selecionado, respectivamente. Por fim, coloque um controle de rótulo Web sob o botão Limpar seu `Text` propriedade e defina seu `ID` propriedade `UploadDetails`.


[![Adicionar um controle de carregamento de arquivos para a página ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: adicionar um controle de carregamento de arquivos para a página ASP.NET ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image18.png))


Figura 13 mostra essa página quando visualizada através de um navegador. Observe que clicar no botão Procurar abre uma caixa de diálogo de seleção de arquivo, permitindo que o usuário escolha um arquivo de seu computador. Quando um arquivo tiver sido selecionado, clicar no botão carregar arquivo selecionado causa um postback que envia o conteúdo binário do arquivo selecionado s para o servidor web.


[![O usuário pode selecionar um arquivo para carregar do seu computador ao servidor](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: O usuário pode selecionar um arquivo para Upload de seu computador para o servidor ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image20.png))


No postback, o arquivo carregado pode ser salvo para o sistema de arquivos ou os dados binários podem ser editados diretamente por meio de um fluxo. Neste exemplo, permitir que s crie um `~/Brochures` pasta e salvar o arquivo carregado. Comece adicionando o `Brochures` pasta para o site como uma subpasta da pasta raiz. Em seguida, crie um manipulador de eventos para o `UploadButton` s `Click` evento e adicione o seguinte código:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

O controle de carregamento de arquivos fornece uma variedade de propriedades para trabalhar com os dados carregados. Por exemplo, o [ `HasFile` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica se um arquivo foi carregado pelo usuário, enquanto o [ `FileBytes` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fornece acesso aos dados binários carregados como uma matriz de bytes. O `Click` manipulador de eventos é iniciado, garantindo que um arquivo foi carregado. Se um arquivo tiver sido carregado, o rótulo mostra o nome do arquivo carregado, seu tamanho em bytes e seu tipo de conteúdo.

> [!NOTE]
> Para garantir que o usuário carrega um arquivo que você pode verificar o `HasFile` propriedade e exibir um aviso se ele s `False`, ou você pode usar o [controle RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) em vez disso.


O carregamento de arquivos s `SaveAs(filePath)` salva o arquivo carregado especificado *filePath*. *filePath* deve ser um *caminho físico* (`C:\Websites\Brochures\SomeFile.pdf`) em vez de *virtual* *caminho* (`/Brochures/SomeFile.pdf`). O [ `Server.MapPath(virtPath)` método](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) toma um caminho virtual e retorna o caminho físico correspondente. Aqui, é o caminho virtual `~/Brochures/fileName`, onde *fileName* é o nome do arquivo carregado. Consulte [usando MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) para obter mais informações sobre caminhos virtuais e físicos e usar `Server.MapPath`.

Depois de concluir o `Click` manipulador de eventos, reserve um tempo para testar a página em um navegador. Clique no botão Procurar e selecionar um arquivo de disco rígido e, em seguida, clique no botão de carregar o arquivo selecionado. A postagem enviará o conteúdo do arquivo selecionado para o servidor web, que, em seguida, exibirá informações sobre o arquivo antes de salvá-lo para o `~/Brochures` pasta. Depois de carregar o arquivo, retorne ao Visual Studio e clique no botão Atualizar no Gerenciador de soluções. Você deve ver o arquivo carregado na pasta ~/Brochures!


[![O arquivo EvolutionValley.jpg foi carregado para o servidor Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Figura 14**: O arquivo `EvolutionValley.jpg` foi carregado para o servidor Web ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg foi salvo na pasta ~/Brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` foi salvo para o `~/Brochures` pasta


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sutilezas com salvar arquivos carregados para o sistema de arquivos

Há vários sutilezas que devem ser abordadas ao salvar o carregamento de arquivos para o sistema de arquivo do web server s. Primeiro, há o problema de segurança. Para salvar um arquivo no sistema de arquivos, o contexto de segurança sob a qual a página ASP.NET está executando deve ter permissões de gravação. O servidor de Web de desenvolvimento ASP.NET é executado no contexto de sua conta de usuário atual. Se você estiver usando o Microsoft s serviços de informações da Internet (IIS) que o servidor web, o contexto de segurança depende da versão do IIS e sua configuração.

Outro desafio de salvar os arquivos para o sistema de arquivos gira em torno de nomeação de arquivos. No momento, nossa página salva todos os arquivos carregados para o `~/Brochures` diretório usando o mesmo nome do arquivo no computador cliente s. Se o usuário carrega uma publicação com o nome `Brochure.pdf`, o arquivo será salvo como `~/Brochure/Brochure.pdf`. Mas e se em algum momento posterior usuário B carrega um arquivo de publicação diferente que tem o mesmo nome de arquivo (`Brochure.pdf`)? Com o código temos agora, um arquivo s será sobrescrito com o carregamento do usuário B s do usuário.

Há várias técnicas para resolver conflitos de nome de arquivo. É uma opção Proibir o carregamento de um arquivo se já existe um com o mesmo nome. Com essa abordagem, quando o usuário B tenta carregar um arquivo chamado `Brochure.pdf`, o sistema não deve salvar seu arquivo e em vez disso, exibirá uma mensagem informando o usuário B para renomear o arquivo e tente novamente. Outra abordagem é salvar o arquivo usando um nome de arquivo exclusivo, que pode ser um [identificador global exclusivo (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) ou o valor de correspondente na coluna da chave primária s registro do banco de dados (supondo que o carregamento é associado com um linha específica no modelo de dados). O tutorial Avançar exploraremos essas opções em mais detalhes.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Desafios envolvidos com grandes quantidades de dados binários

Estes tutoriais presumem que os dados binários capturados são modestos de tamanho. Trabalhar com grandes quantidades de arquivos de dados binários que são vários megabytes ou maior apresenta novos desafios que estão além do escopo esses tutoriais. Por exemplo, por padrão ASP.NET rejeitará carregamentos de mais de 4 MB, embora isso possa ser configurado por meio de [ `<httpRuntime>` elemento](https://msdn.microsoft.com/library/e1f13641.aspx) em `Web.config`. IIS impõe limitações de tamanho de carregamento seu próprio arquivo, muito. Consulte [tamanho do arquivo de carregamento de IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) para obter mais informações. Além disso, o tempo necessário para carregar arquivos grandes pode exceder o padrão 110 segundos que ASP.NET esperará por uma solicitação. Também há problemas de memória e desempenho que surgem ao trabalhar com arquivos grandes.

O controle de carregamento de arquivos não é prático para transferências de arquivos grandes. Como o conteúdo do arquivo s é enviado ao servidor, o usuário final deve esperar pacientemente sem nenhuma confirmação de que o seu carregamento está em andamento. Isso não é um problema muito ao lidar com arquivos menores que podem ser carregados em poucos segundos, mas podem ser um problema ao lidar com arquivos maiores que podem levar minutos para carregar. Há uma variedade de arquivos de terceiros carregamento controles que são mais adequados para lidar com grandes carregamentos e muitos desses fornecedores fornecem indicadores de progresso e ActiveX carregar gerenciadores que apresentam uma experiência de usuário mais refinada.

Se seu aplicativo precisa lidar com arquivos grandes, você precisará investigar os desafios com cuidado e encontrar soluções adequadas para suas necessidades específicas.

## <a name="summary"></a>Resumo

Criando um aplicativo que precisa para capturar dados binários apresenta vários desafios. Neste tutorial, explorou os dois primeiros: decidir onde armazenar os dados binários e permitir que um usuário carregar o conteúdo binário por meio de uma página da web. Sobre os próximos três tutoriais, veremos como associar os dados carregados de um registro no banco de dados, bem como exibir os dados binários juntamente com seus campos de dados de texto.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Usando tipos de dados de valor grande](https://msdn.microsoft.com/library/ms178158.aspx)
- [Início rápido do controle de carregamento de arquivos](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [O controle de servidor ASP.NET 2.0 de carregamento de arquivos](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [O lado escuro de carregamentos de arquivos](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Teresa Murphy e Bernadette Leigh. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-and-deleting-existing-binary-data-cs.md)
> [Próximo](displaying-binary-data-in-the-data-web-controls-vb.md)
