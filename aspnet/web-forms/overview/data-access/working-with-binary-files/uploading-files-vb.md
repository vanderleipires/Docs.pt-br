---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Carregando arquivos (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como permitir que usuários carreguem arquivos binários (como documentos do Word ou PDF) para seu site da Web onde eles podem ser armazenados no sistema de arquivos do servidor...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fdf7b351152d9c64b0ac4b1bc9d558962e704543
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834930"
---
<a name="uploading-files-vb"></a>Carregando arquivos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) ou [baixar PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Saiba como permitir que usuários carreguem arquivos binários (como documentos do Word ou PDF) para seu site da Web onde eles podem ser armazenados no sistema de arquivos do servidor ou o banco de dados.


## <a name="introduction"></a>Introdução

Todos os tutoriais, ve examinei até agora tenha trabalhado exclusivamente com dados de texto. No entanto, muitos aplicativos têm modelos de dados que capturam o texto e dados binários. Um site de encontros online pode permitir que os usuários carreguem uma imagem para associar ao seu perfil. Um site de recrutamento pode permitir que os usuários carregar seu resume como um documento do Microsoft Word ou PDF.

Trabalhar com dados binários adiciona um novo conjunto de desafios. Devemos decidir como os dados binários são armazenados no aplicativo. A interface usada para inserir novos registros deve ser atualizado para permitir que o usuário carregue um arquivo de seu computador e etapas adicionais devem ser executadas para exibir ou fornecer um meio para fazer o download de um registro s associados a dados binários. Este tutorial e os próximos três, exploraremos como obstáculo esses desafios. No final desses tutoriais vai criamos um aplicativo totalmente funcional que associa uma imagem e um folheto PDF com cada categoria. Neste tutorial específico vamos examinar as técnicas diferentes para armazenar dados binários e explore como habilitar usuários carregar um arquivo de seu computador e tiver salvo no sistema de arquivo do web server s.

> [!NOTE]
> Dados binários que faz parte de um modelo de dados do aplicativo s às vezes são conhecidos como um [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), um acrônimo para objeto binário grande. Nesses tutoriais, escolhi usar os dados binários de terminologia, embora o termo BLOB é sinônimo.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Etapa 1: Criando o trabalho com páginas da Web de dados binários

Antes de começar a explorar os desafios associados com a adição de suporte para dados binários, permitem que s levar alguns instantes para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial e os próximos três pela primeira vez. Comece adicionando uma nova pasta chamada `BinaryData`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais de binários e dados relacionados](uploading-files-vb/_static/image1.gif)

**Figura 1**: adicionar as páginas do ASP.NET para que os tutoriais de binários e dados relacionados


Como em outras pastas `Default.aspx` no `BinaryData` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image2.png))


Por fim, adicione essas páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a marcação a seguir após o Enhancing GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para o trabalho com os tutoriais de dados binários.


![O mapa do Site agora inclui entradas para o trabalho com os tutoriais de dados binários](uploading-files-vb/_static/image3.gif)

**Figura 3**: O mapa do Site agora inclui entradas para o trabalho com os tutoriais de dados binários


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Etapa 2: Decidir onde Store os dados binários

Dados binários que está associados com o modelo de dados do aplicativo s podem ser armazenados em um dos dois locais: no sistema de arquivos de servidor s da web com uma referência para o arquivo armazenado no banco de dados; ou diretamente dentro do próprio banco de dados (veja a Figura 4). Cada abordagem tem seu próprio conjunto de prós e contras e merece uma discussão mais detalhada.


[![Dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dados](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: dados binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dados ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image4.png))


Imagine que gostaríamos de estender o banco de dados Northwind para associar uma imagem de cada produto. Uma opção seria armazenar esses arquivos de imagem no sistema de arquivos da web do servidor s e registre o caminho no `Products` tabela. Com essa abordagem, d adicionamos uma `ImagePath` coluna para o `Products` tabela de tipo `varchar(200)`, talvez. Quando um usuário carregar uma imagem de Chai, essa imagem pode ser armazenada no sistema de arquivos de s do servidor web no `~/Images/Tea.jpg`, onde `~` representa o caminho físico do aplicativo s. Ou seja, se o site da web está enraizada no caminho físico `C:\Websites\Northwind\`, `~/Images/Tea.jpg` seria equivalente a `C:\Websites\Northwind\Images\Tea.jpg`. Depois de carregar o arquivo de imagem, d atualizamos o registro Chai na `Products` tabela, de modo que seu `ImagePath` coluna referenciada o caminho da nova imagem. Poderíamos usar `~/Images/Tea.jpg` ou apenas `Tea.jpg` se, decidimos que todas as imagens de produto devem ser colocadas em aplicativo s `Images` pasta.

As principais vantagens de armazenar os dados binários no sistema de arquivos são:

- **Facilidade de implementação** como você verá em breve, armazenar e recuperar dados binários armazenados diretamente no banco de dados envolvem um pouco mais código que ao trabalhar com dados por meio do sistema de arquivos. Além disso, para que um usuário exibir ou baixar dados binários devem ser apresentada a eles uma URL para que os dados. Se os dados residem no sistema de arquivo do web server s, a URL é simples. Se os dados são armazenados no banco de dados, no entanto, uma página da web precisa ser criado que irá recuperar e retornar os dados do banco de dados.
- **Acesso mais amplo aos dados binários** os dados binários talvez precise ser acessíveis a outros serviços ou aplicativos, os que não é possível efetuar pull dos dados do banco de dados. Por exemplo, as imagens associadas a cada produto podem também precisam estar disponíveis aos usuários por meio [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), nesse caso, estamos d deseja armazenar os dados binários no sistema de arquivos.
- **Desempenho** se os dados binários são armazenados no sistema de arquivos, o congestionamento de rede e por demanda entre o servidor de banco de dados e o servidor web será menor se os dados binários são armazenados diretamente no banco de dados.

A principal desvantagem de armazenar dados binários no sistema de arquivos é que ele separa os dados do banco de dados. Se um registro é excluído do `Products` tabela, o arquivo associado no sistema de arquivos da web do servidor s não é excluída automaticamente. Podemos deve gravar código extra para excluir o arquivo ou o sistema de arquivos será fiquem desorganizado com arquivos não utilizados, órfãos. Além disso, ao fazer backup de banco de dados, podemos deve se certificar de fazer backups dos dados binários associados no sistema de arquivos, também. Movendo o banco de dados para outro site ou servidor influenciarem semelhante.

Como alternativa, os dados binários podem ser armazenados diretamente em um banco de dados do Microsoft SQL Server 2005 com a criação de uma coluna do tipo `varbinary`. Como com outros tipos de dados de comprimento variável, você pode especificar um comprimento máximo dos dados binários que podem ser mantidos nesta coluna. Por exemplo, para reservar no máximo 5.000 bytes usar `varbinary(5000)`; `varbinary(MAX)` permite que o tamanho máximo de armazenamento, cerca de 2 GB.

A principal vantagem de armazenar dados binários diretamente no banco de dados é o acoplamento rígido entre os dados binários e o registro de banco de dados. Isso simplifica bastante a tarefas de administração de banco de dados, como backups ou mover o banco de dados para um site ou servidor diferente. Além disso, a exclusão de um registro automaticamente exclui os dados binários correspondentes. Também há mais benefícios sutis de armazenar os dados binários no banco de dados. Ver [armazenar binários arquivos diretamente no banco de dados usando o ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) para uma discussão mais detalhada.

> [!NOTE]
> No Microsoft SQL Server 2000 e versões anteriores, o `varbinary` tipo de dados tem um limite máximo de 8.000 bytes. Para armazenar até 2 GB de dados binários a [ `image` tipo de dados](https://msdn.microsoft.com/library/ms187993.aspx) precisa ser usado em vez disso. Com a adição de `MAX` no SQL Server 2005, no entanto, o `image` tipo de dados foi preterido. -S ainda com compatibilidade, mas a Microsoft anunciou que o `image` tipo de dados será removido em uma versão futura do SQL Server.


Se você estiver trabalhando com um modelo de dados mais antigo que você pode ver o `image` tipo de dados. O banco de dados Northwind s `Categories` a tabela tem um `Picture` coluna que pode ser usada para armazenar os dados binários de um arquivo de imagem para a categoria. Como o banco de dados Northwind tem suas raízes no Microsoft Access e versões anteriores do SQL Server, essa coluna é do tipo `image`.

Para este tutorial e os próximos três, vamos usar ambas as abordagens. O `Categories` tabela já tem um `Picture` coluna para armazenar o conteúdo binário de uma imagem para a categoria. Vamos adicionar uma coluna adicional, `BrochurePath`, para armazenar um caminho para um PDF no sistema de arquivos s do servidor web que pode ser usado para fornecer uma visão geral de qualidade de impressão e sofisticada da categoria.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Etapa 3: Adicionando a`BrochurePath`coluna para o`Categories`tabela

Atualmente, a tabela de categorias tem apenas quatro colunas: `CategoryID`, `CategoryName`, `Description`, e `Picture`. Além desses campos, precisamos adicionar um novo que irá apontar para o folheto categoria s (se houver). Para adicionar essa coluna, vá para o Gerenciador de servidores, fazer uma busca detalhada nas tabelas, clique com botão direito no `Categories` da tabela e escolha Abrir definição de tabela (consulte a Figura 5). Se você não vir o Gerenciador de servidores, ativá-la selecionando a opção de Gerenciador de servidores, no menu Exibir, ou pressione Ctrl + Alt + S.

Adicione um novo `varchar(200)` coluna para o `Categories` tabela denominada `BrochurePath` e permite `NULL` s e clique no ícone Salvar (ou pressione Ctrl + S).


[![Adicionar uma coluna de BrochurePath à tabela de categorias](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: adicionar um `BrochurePath` coluna para o `Categories` tabela ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Etapa 4: Atualizando a arquitetura para usar o`Picture`e`BrochurePath`colunas

O `CategoriesDataTable` na camada de acesso a dados (DAL) atualmente tem quatro `DataColumn` s definidas: `CategoryID`, `CategoryName`, `Description`, e `NumberOfProducts`. Quando projetamos originalmente essa DataTable na [criando uma camada de acesso de dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, o `CategoriesDataTable` tinha apenas as três primeiras colunas; o `NumberOfProducts` coluna foi adicionada no [mestre/detalhes usando um com marcadores Lista de registros mestre com um DataList de detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) tutorial.

Conforme discutido em *criando uma camada de acesso a dados*, tabelas de dados no conjunto de dados tipado compõem os objetos de negócios. Os TableAdapters são responsáveis por comunicar-se com o banco de dados e preencher os objetos de negócios com os resultados da consulta. O `CategoriesDataTable` é preenchida pelo `CategoriesTableAdapter`, que tem três métodos de recuperação de dados:

- `GetCategories()` executa a consulta principal do TableAdapter s e retorna o `CategoryID`, `CategoryName`, e `Description` campos de todos os registros no `Categories` tabela. A consulta principal é o que é usado pelo geradas automaticamente `Insert` e `Update` métodos.
- `GetCategoryByCategoryID(categoryID)` Retorna o `CategoryID`, `CategoryName`, e `Description` campos da categoria cujas `CategoryID` é igual a *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Retorna o `CategoryID`, `CategoryName`, e `Description` campos para todos os registros no `Categories` tabela. Também usa uma subconsulta para retornar o número de produtos associados a cada categoria.

Observe que nenhuma dessas consultas retornam os `Categories` tabela s `Picture` ou `BrochurePath` colunas; nem faz a `CategoriesDataTable` fornecer `DataColumn` s para esses campos. Para trabalhar com a imagem e `BrochurePath` propriedades, precisamos primeiro adicioná-los para o `CategoriesDataTable` e, em seguida, atualize o `CategoriesTableAdapter` classe para retornar essas colunas.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Adicionando o`Picture`e`BrochurePath``DataColumn` s

Comece por adicionar essas duas colunas para o `CategoriesDataTable`. Clique com botão direito no `CategoriesDataTable` cabeçalho s, selecione Adicionar no menu de contexto e, em seguida, escolha a opção de coluna. Isso criará um novo `DataColumn` na DataTable chamado `Column1`. Renomeie esta coluna como `Picture`. Na janela Propriedades, defina as `DataColumn` s `DataType` propriedade `System.Byte[]` (isso não é uma opção na lista suspensa; você precisa digitá-lo em).


[![Criar uma imagem chamada DataColumn cujo tipo de dados é [System]](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: criar um `DataColumn` nomeados `Picture` cujo `DataType` está `System.Byte[]` ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image8.png))


Adicione outro `DataColumn` para DataTable, nomeando- `BrochurePath` usando o padrão `DataType` valor (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Retornando o`Picture`e`BrochurePath`valores do TableAdapter

Com esses dois `DataColumn` s adicionado para o `CategoriesDataTable`, podemos está pronto para atualizar o `CategoriesTableAdapter`. Poderíamos ter ambos os valores de coluna retornados na consulta principal do TableAdapter, mas isso seria trazer de volta os dados binários sempre o `GetCategories()` método foi invocado. Em vez disso, let s atualize a consulta principal do TableAdapter para trazer de volta `BrochurePath` e crie um método de recuperação de dados adicionais que retorna uma determinada categoria s `Picture` coluna.

Para atualizar a consulta do TableAdapter principal, clique com botão direito no `CategoriesTableAdapter` cabeçalho s e escolha a opção de configurar o menu de contexto. Isso abre o Assistente de configuração do adaptador de tabela, que podemos ve visto em vários tutoriais anteriores. Atualize a consulta para trazer de volta a `BrochurePath` e clique em Concluir.


[![Atualizar a lista de colunas na instrução SELECT para retornar também BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: atualizar a lista de colunas em de `SELECT` instrução para retornar também `BrochurePath` ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image10.png))


Ao usar instruções SQL ad hoc para o TableAdapter, atualizando a lista de colunas na consulta principal atualiza a lista de colunas para todos os `SELECT` métodos TableAdapter de consulta. Isso significa que o `GetCategoryByCategoryID(categoryID)` método foi atualizado para retornar o `BrochurePath` coluna, que pode ser o que podemos pretendia. No entanto, ela também atualizada a lista de colunas no `GetCategoriesAndNumberOfProducts()` método, remover a subconsulta que retorna o número de produtos para cada categoria! Portanto, precisamos atualizar esse método s `SELECT` consulta. Clique com botão direito no `GetCategoriesAndNumberOfProducts()` método, escolha Configure e reverter a `SELECT` consulta de volta para seu valor original:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Em seguida, crie um novo método do TableAdapter que retorna uma determinada categoria s `Picture` valor da coluna. Clique com botão direito no `CategoriesTableAdapter` cabeçalho s e escolha a opção Add Query para iniciar o Assistente de configuração de consulta do TableAdapter. A primeira etapa deste assistente pergunta que se quisermos para consultar dados usando uma instrução de SQL ad hoc, um novo procedimento armazenado, ou um existente. Selecione usar instruções SQL e clique em Avançar. Como podemos será retornada uma linha, escolha o SELECT que retorna a opção de linhas da segunda etapa.


[![Selecione as opção Use SQL statements](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: selecione as opção Use SQL statements ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image12.png))


[![Uma vez que a consulta retornará um registro da tabela de categorias, escolha SELECT que retorna linhas](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: uma vez que a consulta retornará um registro da tabela de categorias, escolha Selecionar que retorna linhas ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image14.png))


Na terceira etapa, insira a seguinte consulta SQL e clique em Avançar:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

A última etapa é escolher o nome para o novo método. Use `FillCategoryWithBinaryDataByCategoryID` e `GetCategoryWithBinaryDataByCategoryID` para o preenchimento uma DataTable e retornar uma DataTable padrões, respectivamente. Clique em Concluir para concluir o assistente.


[![Escolha os nomes dos métodos do TableAdapter s](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: escolha os nomes para os métodos do TableAdapter s ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Depois de concluir o Assistente de configuração de consulta de adaptador de tabela, você poderá ver uma caixa de diálogo informando que o novo texto de comando retorna dados com esquema diferente do esquema da consulta principal. Em resumo, o assistente é observar que a consulta principal do TableAdapter s `GetCategories()` retorna um esquema diferente daquele que acabamos de criar. Mas isso é o que queremos, portanto, você pode ignorar essa mensagem.


Além disso, tenha em mente que se você estiver usando instruções SQL ad hoc e use o Assistente para alterar a consulta principal do TableAdapter s em algum momento posterior no tempo, ele modificará o `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` lista de colunas de instrução s para incluir apenas as colunas das consulta principal (ou seja, ele removerá o `Picture` coluna da consulta). Você terá que atualizar manualmente a lista de colunas para retornar os `Picture` coluna, semelhante ao que fizemos com o `GetCategoriesAndNumberOfProducts()` método no início desta etapa.

Depois de adicionar os dois `DataColumn` s para o `CategoriesDataTable` e o `GetCategoryWithBinaryDataByCategoryID` método para o `CategoriesTableAdapter`, essas classes no Designer de conjunto de dados tipado devem se parecer com a captura de tela na Figura 11.


![O Designer de conjunto de dados inclui as novas colunas e o método](uploading-files-vb/_static/image11.gif)

**Figura 11**: O Designer de conjunto de dados inclui as novas colunas e o método


## <a name="updating-the-business-logic-layer-bll"></a>Atualizando a camada de lógica de negócios (BLL)

Com a DAL atualizada, tudo o que resta é aumentar os negócios lógica BLL (camada) para incluir um método para o novo `CategoriesTableAdapter` método. Adicione o seguinte método para o `CategoriesBLL` classe:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Etapa 5: Carregar um arquivo do cliente para o servidor Web

Durante a coleta de dados binários, muitas vezes, esses dados são fornecidos por um usuário final. Para capturar essas informações, o usuário precisa ser capaz de carregar um arquivo de seu computador para o servidor web. Os dados carregados, em seguida, precisam ser integrada com o modelo de dados, o que pode significar que salvar o arquivo para o sistema de arquivos do web server s e adicionar um caminho para o arquivo no banco de dados ou gravar o conteúdo binário diretamente no banco de dados. Nesta etapa, examinaremos como permitir que um usuário carrega arquivos do seu computador para o servidor. No próximo tutorial voltaremos nossa atenção para integrar o arquivo carregado com o modelo de dados.

O ASP.NET 2.0 s novos [controle FileUpload Web](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) fornece um mecanismo para que os usuários enviem um arquivo de seu computador para o servidor web. O controle FileUpload renderiza como um `<input>` elemento cujo `type` atributo é definido como o arquivo, que navegadores exibem como uma caixa de texto com um botão de procura. Clique no botão Procurar abre uma caixa de diálogo na qual o usuário pode selecionar um arquivo. Quando o formulário é postado novamente, o conteúdo do arquivo selecionado s é enviado junto com o postback. No lado do servidor, informações sobre o arquivo carregado são acessíveis por meio das propriedades de s controle FileUpload.

Para demonstrar um carregamento de arquivos, abra o `FileUpload.aspx` página o `BinaryData` pasta, arraste um controle FileUpload da caixa de ferramentas para o Designer e defina o controle s `ID` propriedade para `UploadTest`. Em seguida, adicione um controle da Web de botão definindo sua `ID` e `Text` propriedades a serem `UploadButton` e carregue o arquivo selecionado, respectivamente. Por fim, coloque um controle de rótulo Web sob o botão Limpar seu `Text` propriedade e defina sua `ID` propriedade para `UploadDetails`.


[![Adicionar um controle FileUpload para a página ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: adicionar um controle FileUpload para a página ASP.NET ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image18.png))


Figura 13 mostra essa página quando visualizado por meio de um navegador. Observe que clicar no botão Procurar abre uma caixa de diálogo de seleção de arquivo, permitindo que o usuário escolha um arquivo de seu computador. Depois que um arquivo tiver sido selecionado, clicar no botão carregar arquivo selecionado faz com que um postback que envia o conteúdo binário de s de arquivo selecionado para o servidor web.


[![O usuário pode selecionar um arquivo para carregar do seu computador ao servidor](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: O usuário pode selecionar um arquivo para Upload de seu computador ao servidor ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image20.png))


No postback, o arquivo carregado pode ser salvo para o sistema de arquivos ou seus dados binários podem ser trabalhados diretamente por meio de um Stream. Neste exemplo, permitir que s criem um `~/Brochures` pasta e salve o arquivo carregado nela. Comece adicionando o `Brochures` pasta para o site como uma subpasta da pasta raiz. Em seguida, crie um manipulador de eventos para o `UploadButton` s `Click` eventos e adicione o seguinte código:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

O controle FileUpload fornece uma variedade de propriedades para trabalhar com os dados carregados. Por exemplo, o [ `HasFile` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica se um arquivo foi carregado pelo usuário, enquanto o [ `FileBytes` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fornece acesso aos dados binários carregados como uma matriz de bytes. O `Click` manipulador de eventos é iniciado, garantindo que um arquivo foi carregado. Se um arquivo tiver sido carregado, o rótulo mostra o nome do arquivo carregado, seu tamanho em bytes e seu tipo de conteúdo.

> [!NOTE]
> Para garantir que o usuário carrega um arquivo que você pode verificar a `HasFile` propriedade e exibir um aviso se ele s `False`, ou você pode usar o [controle RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) em vez disso.


O s FileUpload `SaveAs(filePath)` salva o arquivo carregado especificado *filePath*. *filePath* deve ser um *caminho físico* (`C:\Websites\Brochures\SomeFile.pdf`) em vez de um *virtual* *caminho* (`/Brochures/SomeFile.pdf`). O [ `Server.MapPath(virtPath)` método](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) toma um caminho virtual e retorna o caminho físico correspondente. Aqui, é o caminho virtual `~/Brochures/fileName`, onde *fileName* é o nome do arquivo carregado. Ver [usando o Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) para obter mais informações sobre caminhos virtuais e físicos e usar `Server.MapPath`.

Depois de concluir o `Click` manipulador de eventos, reserve um tempo para testar a página em um navegador. Clique no botão Procurar e selecionar um arquivo de disco rígido e, em seguida, clique no botão de carregar o arquivo selecionado. O postback enviará o conteúdo do arquivo selecionado para o servidor web, que, em seguida, exibe informações sobre o arquivo antes de salvá-lo para o `~/Brochures` pasta. Depois de carregar o arquivo, retorne ao Visual Studio e clique no botão Atualizar no Gerenciador de soluções. Você deve ver o arquivo que você acabou de carregar na pasta ~/Brochures!


[![EvolutionValley.jpg o arquivo foi carregado para o servidor Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Figura 14**: O arquivo ini `EvolutionValley.jpg` foi carregado para o servidor Web ([clique para exibir a imagem em tamanho normal](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg foi salvo na pasta ~/Brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` foi salvo para o `~/Brochures` pasta


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sutilezas com relação a salvar arquivos carregados para o sistema de arquivos

Há várias sutilezas que devem ser abordadas ao salvar a carregar arquivos para o sistema de arquivos do web server s. Primeiro, daí o problema de segurança. Para salvar um arquivo no sistema de arquivos, o contexto de segurança sob a qual a página do ASP.NET está em execução deve ter permissões de gravação. O servidor de Web de desenvolvimento do ASP.NET é executado sob o contexto da conta do usuário atual. Se você estiver usando s Microsoft Internet Information Services (IIS) como o servidor web, o contexto de segurança depende da versão do IIS e sua configuração.

Outro desafio de salvar arquivos no sistema de arquivos gira em torno de nomear os arquivos. No momento, nossa página salva todos os arquivos carregados para o `~/Brochures` diretório usando o mesmo nome que o arquivo no computador cliente s. Se o usuário A carrega um folheto com o nome `Brochure.pdf`, o arquivo será salvo como `~/Brochure/Brochure.pdf`. Mas e se o usuário B em algum momento posterior carrega um arquivo de folheto diferente que tem o mesmo nome de arquivo (`Brochure.pdf`)? Com o código agora, nós temos usuário um s de arquivo será substituído com o carregamento do usuário B s.

Há várias técnicas para resolver conflitos de nome de arquivo. Uma opção é proibir o carregamento de um arquivo se já existir um com o mesmo nome. Com essa abordagem, quando o usuário B tenta carregar um arquivo chamado `Brochure.pdf`, o sistema não seria salvar seu arquivo e exibirá uma mensagem informando o usuário B para renomear o arquivo e tente novamente. Outra abordagem é salvar o arquivo usando um nome de arquivo exclusivo, que pode ser um [identificador global exclusivo (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) ou o valor de correspondente s registros colunas de chave primária do banco de dados (supondo que o upload está associado com um linha específica no modelo de dados). No próximo tutorial vamos explorar essas opções mais detalhadamente.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Desafios envolvidos com grandes quantidades de dados binários

Estes tutoriais presumem que os dados binários capturados são modestos em tamanho. Trabalhando com grandes quantidades de arquivos de dados binários que estão vários megabytes ou maior apresenta novos desafios que estão além do escopo destes tutoriais. Por exemplo, por padrão ASP.NET rejeitará carregamentos de mais de 4 MB, embora isso possa ser configurado por meio de [ `<httpRuntime>` elemento](https://msdn.microsoft.com/library/e1f13641.aspx) em `Web.config`. IIS impõe limitações de tamanho de upload de seu próprio arquivo, também. Ver [tamanho de arquivo de carregamento de IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) para obter mais informações. Além disso, o tempo necessário para carregar arquivos grandes pode exceder o padrão ASP.NET irá esperar para uma solicitação de 110 segundos. Também há problemas de memória e desempenho que surgem ao trabalhar com arquivos grandes.

O controle FileUpload é impraticável para transferências de arquivos grandes. Como o conteúdo do arquivo s é enviado ao servidor, o usuário final deve aguardará pacientemente sem nenhuma confirmação que seu upload está em andamento. Isso não é tanto um problema ao lidar com arquivos menores que podem ser carregados em poucos segundos, mas podem ser um problema ao lidar com arquivos maiores que podem levar minutos para carregar. Há uma variedade de arquivos de terceiros upload controles que são mais adequados para lidar com grandes carregamentos e muitos desses fornecedores fornecem indicadores de progresso e ActiveX carregar gerenciadores que apresentam uma experiência de usuário muito mais refinada.

Se seu aplicativo precisa lidar com arquivos grandes, você precisará investigar os desafios com cuidado e encontrar soluções adequadas para suas necessidades específicas.

## <a name="summary"></a>Resumo

Criando um aplicativo que precisa para capturar dados binários apresenta uma série de desafios. Neste tutorial, exploramos os dois primeiros: decidir onde armazenar os dados binários e que permite ao usuário carregar o conteúdo binário por meio de uma página da web. Sobre os próximos três tutoriais, vamos ver como associar os dados carregados de um registro no banco de dados, bem como exibir os dados binários juntamente com seus campos de dados de texto.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Usando tipos de dados de valor grande](https://msdn.microsoft.com/library/ms178158.aspx)
- [Inícios rápidos de Controle fileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [O controle de servidor do ASP.NET 2.0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [O lado obscuro das transferências de arquivos](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy e Bernadette Leigh. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-and-deleting-existing-binary-data-cs.md)
> [Próximo](displaying-binary-data-in-the-data-web-controls-vb.md)
