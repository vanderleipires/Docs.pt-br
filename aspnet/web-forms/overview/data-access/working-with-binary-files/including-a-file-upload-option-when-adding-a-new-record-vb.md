---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Incluindo um arquivo de opção de carregar ao adicionar um novo registro (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como criar uma interface Web que permite que o usuário ao inserir dados de texto e carregar arquivos binários. Para ilustrar o t disponíveis opções...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 39608eef7cc88be56ef6e21820e4afcfaa4ffd8d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888647"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Incluindo uma opção de carregamento de arquivo ao adicionar um novo registro (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) ou [baixar PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Este tutorial mostra como criar uma interface Web que permite que o usuário ao inserir dados de texto e carregar arquivos binários. Para ilustrar as opções disponíveis para armazenar dados binários, um arquivo será salva no banco de dados enquanto outros é armazenado no sistema de arquivos.


## <a name="introduction"></a>Introdução

Nos tutoriais anteriores dois nós explorados técnicas para armazenar dados binários que está associados com o modelo de dados s aplicativo, examinado como usar o controle de carregamento de arquivos para enviar os arquivos do cliente para o servidor web e viu como apresentar esses dados binários em dados W controle de Web. Podemos ver ainda para falar sobre como associar dados carregados com o modelo de dados, embora.

Este tutorial, vamos criar uma página da web para adicionar uma nova categoria. Além de caixas de texto para o nome da categoria s e a descrição, essa página será necessário incluir dois controles de carregamento de arquivos para a nova imagem de categoria s e outro para a publicação. A imagem carregada será armazenada diretamente no novo registro s `Picture` coluna, enquanto a publicação serão salvas a `~/Brochures` pasta com o caminho para o arquivo salvo no novo registro s `BrochurePath` coluna.

Antes de criar essa nova página da web, é necessário atualizar a arquitetura. O `CategoriesTableAdapter` consulta principal s não recuperar o `Picture` coluna. Consequentemente, o gerado automaticamente `Insert` método só tem entradas para o `CategoryName`, `Description`, e `BrochurePath` campos. Portanto, precisamos criar um método adicional no TableAdapter que solicita todos os quatro `Categories` campos. O `CategoriesBLL` classe na camada de lógica de negócios também precisam ser atualizados.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Etapa 1: Adicionando um`InsertWithPicture`método para o`CategoriesTableAdapter`

Quando criamos o `CategoriesTableAdapter` novamente o [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, nós configuramos para gerar automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções com base na consulta principal. Além disso, podemos instruiu o TableAdapter empregar a abordagem direta do banco de dados, que criou os métodos `Insert`, `Update`, e `Delete`. Esses métodos execute o gerado automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções e, consequentemente, aceitar parâmetros de entrada com base nas colunas retornadas pela consulta principal. No [carregando arquivos](uploading-files-vb.md) tutorial são aumentadas a `CategoriesTableAdapter` a consulta principal s para usar o `BrochurePath` coluna.

Como o `CategoriesTableAdapter` consulta principal s não faz referência a `Picture` coluna, não pode adicionar um novo registro nem atualiza um registro existente com um valor para o `Picture` coluna. Para capturar essas informações, pode criar um novo método no TableAdapter que é usado especificamente para inserir um registro com dados binários ou podemos personalizar o gerado automaticamente `INSERT` instrução. O problema com a personalização de gerado automaticamente `INSERT` instrução é que podemos risco nosso personalizações substituídas pelo assistente. Por exemplo, imagine que estamos personalizada a `INSERT` instrução para incluir o uso do `Picture` coluna. Isso atualizará o TableAdapter s `Insert` método para incluir um parâmetro de entrada adicional para os dados binários de imagem s categoria s. Pode, em seguida, criamos um método na camada de lógica de negócios para usar esse método DAL e chamar esse método BLL por meio da camada de apresentação e tudo o que funcionaria muito. Ou seja, até a próxima vez que configuramos o TableAdapter por meio do Assistente de configuração do TableAdapter. Assim que o assistente for concluído, nosso personalizações para o `INSERT` instrução seria substituída, o `Insert` método serão revertidas para sua forma antiga e nosso código seria não compilar mais!

> [!NOTE]
> Essa mensagem inconveniente é um problema ao usar procedimentos armazenados em vez de instruções SQL ad hoc. Um tutorial futuras explorará usando procedimentos armazenados em vez de instruções SQL ad hoc na camada de acesso a dados.


Para evitar essa possibilidade dor, em vez de personalizando instruções SQL geradas automaticamente permitem s em vez disso, crie um novo método para o TableAdapter. Esse método, chamado `InsertWithPicture`, aceita valores para o `CategoryName`, `Description`, `BrochurePath`, e `Picture` colunas e executar um `INSERT` instrução que armazena todos os quatro valores em um novo registro.

Abra o conjunto de dados tipado e, no Designer, clique duas vezes no `CategoriesTableAdapter` cabeçalho s e escolha Adicionar consulta no menu de contexto. Isso inicia o Assistente de configuração de consulta do TableAdapter, que inicia nos pedindo a como a consulta do TableAdapter deve acessar o banco de dados. Escolher as instruções SQL de uso e clique em Avançar. A próxima etapa solicitará o tipo de consulta a ser gerado. Desde que re criando uma consulta para adicionar um novo registro para o `Categories` de tabela, escolha Inserir e clique em Avançar.


[![Selecione a opção de inserção](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Figura 1**: selecione a opção de inserção ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


Agora, é necessário especificar o `INSERT` instrução SQL. O assistente sugere automaticamente um `INSERT` instrução corresponde à consulta principal do TableAdapter s. Nesse caso, ele s uma `INSERT` instrução que insere o `CategoryName`, `Description`, e `BrochurePath` valores. A instrução Update para que o `Picture` coluna está incluída junto com um `@Picture` parâmetro, da seguinte forma:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

A última tela do assistente nos peça para nomear o novo método TableAdapter. Digite `InsertWithPicture` e clique em Concluir.


[![Nome do novo InsertWithPicture método TableAdapter](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Figura 2**: nomear o novo método de TableAdapter `InsertWithPicture` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Etapa 2: Atualizando a camada de lógica de negócios

Desde que a camada de apresentação deve interface somente com a camada de lógica de negócios em vez de utilizando-o para ir diretamente para a camada de acesso a dados, é preciso criar um método BLL que invoca o método DAL que acabamos de criar (`InsertWithPicture`). Para este tutorial, crie um método no `CategoriesBLL` classe denominada `InsertWithPicture` que aceita como entrada três `String` s e um `Byte` matriz. O `String` são parâmetros de entrada para o nome da categoria s, descrição e caminho do arquivo de publicação, enquanto o `Byte` matriz é para o conteúdo binário de imagem categoria s. Como mostra o código a seguir, este método BLL invoca o método DAL correspondente:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Certifique-se de que você salvou o conjunto de dados tipado antes de adicionar o `InsertWithPicture` método BLL. Como o `CategoriesTableAdapter` código da classe é gerado automaticamente com base no conjunto de dados tipado, se você não t primeiro salvar as alterações para o conjunto de dados tipado o `Adapter` propriedade ganha t conhecer o `InsertWithPicture` método.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Etapa 3: Lista as categorias existentes e seus dados binários

Este tutorial criaremos uma página que permite que um usuário final adicionar uma nova categoria para o sistema, fornecendo uma imagem e a publicação para a nova categoria. No [tutorial anterior](displaying-binary-data-in-the-data-web-controls-vb.md) usamos um GridView com um TemplateField e ImageField para exibir cada nome de categoria s, descrição, imagem e um link para baixar sua publicação. Permitir que o s replicar essa funcionalidade para este tutorial, criando uma página que lista todas as categorias existentes e permite que novos a ser criado.

Comece abrindo o `DisplayOrDownload.aspx` página a partir de `BinaryData` pasta. Ir para a exibição de fonte e copie a GridView e ObjectDataSource s sintaxe declarativa, colá-los dentro de `<asp:Content>` elemento em `UploadInDetailsView.aspx`. Além disso, não se esqueça copiar o `GenerateBrochureLink` método da classe por trás do código de `DisplayOrDownload.aspx` para `UploadInDetailsView.aspx`.


[![Copie e cole a sintaxe declarativa de DisplayOrDownload.aspx para UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Figura 3**: copiar e colar a sintaxe declarativa de `DisplayOrDownload.aspx` para `UploadInDetailsView.aspx` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


Depois de copiar a sintaxe declarativa e `GenerateBrochureLink` método sobre o `UploadInDetailsView.aspx` página, exibir a página por meio de um navegador para garantir que tudo o que foi copiado corretamente sobre. Você deve ver um GridView oito categorias de listagem que inclui um link para baixar a publicação, bem como a imagem de categoria s.


[![Agora você verá cada categoria junto com seus dados binários](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Figura 4**: você deve agora consulte cada categoria junto com seus dados binários ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Etapa 4: Configurar o`CategoriesDataSource`à inserção de suporte

O `CategoriesDataSource` ObjectDataSource usado pelo `Categories` GridView atualmente não fornece a capacidade de inserir dados. Para dar suporte à inserção por este controle de fonte de dados, é preciso mapear seu `Insert` método a um método em seu objeto subjacente, `CategoriesBLL`. Em particular, queremos mapeá-la para o `CategoriesBLL` método adicionamos novamente na etapa 2, `InsertWithPicture`.

Iniciar clicando no link configurar fonte de dados de marca inteligente ObjectDataSource s. A primeira tela mostra o objeto de fonte de dados está configurada para funcionar com, `CategoriesBLL`. Deixe essa configuração como-está e clique em Avançar para a tela de definir métodos de dados. Mover para a guia Inserir e escolha o `InsertWithPicture` método na lista suspensa. Clique em Concluir para concluir o assistente.


[![Configurar o ObjectDataSource para usar o método InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `InsertWithPicture` método ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> Após concluir o assistente, o Visual Studio pode solicitar se você quiser atualizar campos e chaves, que irá regenerar os dados da Web controles campos. Selecione não, porque se você escolher Sim substituirá as personalizações de campo que tenha feito.


Depois de concluir o assistente, o ObjectDataSource agora incluem um valor para o seu `InsertMethod` propriedade, bem como `InsertParameters` para as colunas de quatro categorias, como a seguinte marcação declarativa mostra:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Etapa 5: Criar a Interface de inserção

Como primeiro abordados o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), o controle DetailsView fornece uma interface de inserção interna que pode ser utilizada ao trabalhar com um controle de fonte de dados que dá suporte à inserção. Permitir que o s adicionar um controle DetailsView a esta página acima GridView que permanentemente processará sua interface inserindo, permitindo que um usuário adicionar rapidamente uma nova categoria. Após adicionar uma nova categoria de DetailsView, GridView abaixo dela automaticamente atualizar e exibir a nova categoria.

Comece a arrastar um DetailsView da caixa de ferramentas para o Designer acima GridView, definindo seu `ID` propriedade `NewCategory` e limpar o `Height` e `Width` valores de propriedade. De DetailsView s marca inteligente, associá-lo a existente `CategoriesDataSource` e, em seguida, marque a caixa de seleção Permitir inserção.


[![Vincular DetailsView a CategoriesDataSource e habilitar a inserção de](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Figura 6**: associar o DetailsView para o `CategoriesDataSource` e permitir inserção ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


Para processar permanentemente DetailsView em sua interface de inserção, defina seu `DefaultMode` propriedade `Insert`.

Observe que o DetailsView tem cinco BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` Embora o `CategoryID` BoundField não será renderizado na interface inserindo porque seu `InsertVisible` propriedade está definida como `False`. Essas BoundFields existe porque eles são as colunas retornadas pelo `GetCategories()` método, que é o que o ObjectDataSource invoca para recuperar seus dados. Para inserir, no entanto, podemos don t deseja permitir que o usuário especifique um valor para `NumberOfProducts`. Além disso, é necessário permitir que eles carregar uma imagem para a nova categoria, bem como carregar um PDF para a publicação.

Remover o `NumberOfProducts` BoundField do DetailsView totalmente e atualize o `HeaderText` propriedades do `CategoryName` e `BrochurePath` BoundFields categoria e publicação, respectivamente. Em seguida, converter o `BrochurePath` BoundField em um TemplateField e adicionar um TemplateField novo para a imagem, dando a esse novo TemplateField um `HeaderText` valor de imagem. Mover o `Picture` TemplateField para que ela seja entre o `BrochurePath` TemplateField e CommandField.


![Vincular DetailsView a CategoriesDataSource e habilitar a inserção de](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Figura 7**: associar o DetailsView para o `CategoriesDataSource` e habilitar a inserção de


Se você converteu o `BrochurePath` BoundField em um TemplateField através da caixa de diálogo Editar campos, o TemplateField inclui um `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. Somente o `InsertItemTemplate` é necessário, no entanto, portanto fique à vontade para remover os dois modelos. Neste ponto, sua sintaxe declarativa de s DetailsView deve ser semelhante ao seguinte:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Adicionando controles de carregamento de arquivos para a publicação e campos de imagem

Atualmente, o `BrochurePath` TemplateField s `InsertItemTemplate` contém uma caixa de texto, enquanto o `Picture` TemplateField não contém todos os modelos. É necessário atualizar esses dois s TemplateField `InsertItemTemplate` s para usar os controles de carregamento de arquivos.

De DetailsView s marca inteligente, escolha a opção de editar modelos e, em seguida, selecione o `BrochurePath` TemplateField s `InsertItemTemplate` na lista suspensa. Remover a caixa de texto e, em seguida, arraste um controle de carregamento de arquivos da caixa de ferramentas para o modelo. Definir o controle de carregamento de arquivos s `ID` para `BrochureUpload`. Da mesma forma, adicionar um controle de carregamento de arquivos para o `Picture` TemplateField s `InsertItemTemplate`. Defina esse controle FileUpload s `ID` para `PictureUpload`.


[![Adicionar um controle de carregamento de arquivos para o InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Figura 8**: adicionar um controle de carregamento de arquivos para o `InsertItemTemplate` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


Depois de fazer essas adições, a sintaxe declarativa do dois TemplateField s será:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Quando um usuário adiciona uma nova categoria, queremos garantir que a publicação e a imagem do tipo de arquivo correto. Para a publicação, o usuário deve fornecer um PDF. Para a imagem, precisamos que o usuário para carregar um arquivo de imagem, mas podemos permitir *qualquer* a imagem do arquivo ou apenas os arquivos de imagem de um determinado tipo, como GIFs ou JPGs? Para permitir diferentes tipos de arquivo, d é necessário estender o `Categories` esquema para incluir uma coluna que captura o tipo de arquivo para que esse tipo pode ser enviado ao cliente por meio de `Response.ContentType` em `DisplayCategoryPicture.aspx`. Já que estamos não t tem uma coluna desse tipo, seria aconselhável restringir os usuários para fornecer apenas um tipo de arquivo de imagem específico. O `Categories` imagens existentes da tabela s são bitmaps, mas JPGs um formato de arquivo mais adequado para imagens servido pela web.

Se um usuário carrega um tipo de arquivo incorreto, é preciso cancelar a inserção e exibirá uma mensagem indicando o problema. Adicione um controle de rótulo Web abaixo DetailsView. Definir seu `ID` propriedade `UploadWarning`, desmarque-out seu `Text` propriedade, definida a `CssClass` propriedade para aviso e o `Visible` e `EnableViewState` propriedades para `False`. O `Warning` classe CSS é definida em `Styles.css` e renderiza o texto em uma fonte grande, vermelho, em itálico, negrito.

> [!NOTE]
> Idealmente, o `CategoryName` e `Description` BoundFields será convertida em TemplateFields e suas interfaces de inserção personalizadas. O `Description` inserir interface, por exemplo, seria provavelmente se ajustem melhor por meio de uma caixa de texto de várias linha. E, como o `CategoryName` coluna não aceitar `NULL` valores, um RequiredFieldValidator deve ser adicionado para garantir que o usuário fornece um valor para o novo nome de categoria s. Essas etapas são deixadas como um exercício para o leitor. Voltar ao [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para uma Visão aprofundada de aumentar as interfaces de modificação de dados.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Etapa 6: Salvar a publicação carregada para o sistema de arquivo do Web Server s

Quando o usuário insere os valores para uma nova categoria e clica no botão de inserção, ocorre um postback e inserindo o fluxo de trabalho é revelado. Primeiro, o s DetailsView [ `ItemInserting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) é acionado. Em seguida, o s ObjectDataSource `Insert()` método é chamado, o que resulta em um novo registro que está sendo adicionado para o `Categories` tabela. Depois disso, o s DetailsView [ `ItemInserted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) é acionado.

Antes do s ObjectDataSource `Insert()` método é invocado, deve primeiro, certifique-se de que os tipos de arquivo apropriado foram carregados pelo usuário e, em seguida, salvar a publicação PDF para o sistema de arquivos do servidor s da web. Criar um manipulador de eventos para o s DetailsView `ItemInserting` evento e adicione o seguinte código:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

O manipulador de eventos começa consultando o `BrochureUpload` controle de carregamento de arquivos nos modelos de s DetailsView. Em seguida, se uma publicação tiver sido carregada, a extensão do arquivo carregado s é examinada. Se a extensão não está. PDF, em seguida, um aviso é exibido, a inserção será cancelada e termina a execução do manipulador de eventos.

> [!NOTE]
> Contar com a extensão do arquivo carregado s não é uma técnica segura para garantir que o arquivo carregado é um documento PDF. O usuário pode ter um documento PDF válido com a extensão `.Brochure`, ou pode ter levado um documento PDF não e deu a ela uma `.pdf` extensão. O conteúdo binário do arquivo s precisa ser examinado por meio de programação para verificar mais definitivamente o tipo de arquivo. Essas abordagens completa, porém, costumam ser um exagero; Verificando a extensão é suficiente para a maioria dos cenários.


Como discutido o [carregando arquivos](uploading-files-vb.md) tutorial, deve ter cuidado ao salvar arquivos para o sistema de arquivos para esse carregamento de um usuário s não substitua s outro. Para este tutorial, tentará usar o mesmo nome como o arquivo carregado. Se já existe um arquivo de `~/Brochures` diretório com esse mesmo nome de arquivo, no entanto, podemos vai acrescentar um número no final até encontra um nome exclusivo. Por exemplo, se o usuário carrega um arquivo de publicação nomeado `Meats.pdf`, mas já existe um arquivo chamado `Meats.pdf` no `~/Brochures` pasta, vamos alterar o nome do arquivo salvo para `Meats-1.pdf`. Se existente, tentaremos `Meats-2.pdf`, e assim por diante, até encontra um nome de arquivo exclusivo.

O código a seguir usa o [ `File.Exists(path)` método](https://msdn.microsoft.com/library/system.io.file.exists.aspx) para determinar se um arquivo já existe com o nome de arquivo especificado. Nesse caso, ele continuará a tentar novos nomes de arquivo para a publicação até que nenhum conflito foi encontrado.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Depois que um nome de arquivo válido foi encontrado, o arquivo precisa ser salvo para o sistema de arquivos e os s ObjectDataSource `brochurePath``InsertParameter` valor precisa ser atualizado para que este nome de arquivo é gravado no banco de dados. Como vimos no *carregando arquivos* tutorial, o arquivo pode ser salvo usando o controle FileUpload s `SaveAs(path)` método. Para atualizar o s ObjectDataSource `brochurePath` parâmetro, use o `e.Values` coleção.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Etapa 7: Salvar a imagem carregada no banco de dados

Para armazenar a imagem carregada no novo `Categories` registro, é preciso atribuir o conteúdo binário carregado para o s ObjectDataSource `picture` parâmetro em DetailsView s `ItemInserting` eventos. Antes de fazermos essa atribuição, no entanto, é necessário primeiro certificar-se de que a imagem carregada é JPG e não outro tipo de imagem. Como na etapa 6, permitem s para usar a extensão do arquivo carregado imagem s para determinar seu tipo.

Enquanto o `Categories` tabela permite `NULL` valores para o `Picture` coluna, todas as categorias atualmente tem uma imagem. Permitir que o s forçar o usuário a fornecer uma imagem ao adicionar uma nova categoria por meio desta página. O código a seguir verifica para garantir que uma imagem foi carregada e que ele tem uma extensão apropriada.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Esse código deve ser colocado *antes de* o código da etapa 6 para que se houver um problema com o carregamento de imagem, o manipulador de eventos será encerrado antes que o arquivo de publicação é salvo no sistema de arquivos.

Supondo que um arquivo apropriado foi carregado, atribua o conteúdo binário carregado para o valor de parâmetro da imagem s com a seguinte linha de código:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>Completo`ItemInserting`manipulador de eventos

Para fins de exatidão, aqui está o `ItemInserting` manipulador de eventos em sua totalidade:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Etapa 8: Corrigir o`DisplayCategoryPicture.aspx`página

Permitem s dedicar um tempo para testar a interface de inserção e `ItemInserting` manipulador de eventos que foi criado nas últimas etapas. Visite o `UploadInDetailsView.aspx` por meio de um navegador e a tentativa de adicionar uma categoria, mas omita a imagem da página, ou especificar uma imagem JPG não ou uma publicação não PDF. Em qualquer um desses casos, uma mensagem de erro será exibida e o fluxo de trabalho de inserção cancelada.


[![Uma mensagem de aviso será exibido se um tipo de arquivo inválido é carregado](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Figura 9**: A mensagem de aviso é exibido se um tipo de arquivo inválido é carregado ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


Uma vez você verificou que a página requer uma imagem a ser carregado e t ganha aceitar arquivos PDF não ou não JPG, adicionar uma nova categoria com imagem válida JPG, deixar o campo de publicação vazio. Depois de clicar no botão de inserção, a página fará o postback e será adicionado um novo registro para o `Categories` tabela com o conteúdo binário de imagem carregada s diretamente no banco de dados. GridView é atualizada e mostra uma linha para a categoria adicionada recentemente, mas, como mostra a Figura 10, a nova imagem s categoria não será renderizada corretamente.


[![A nova categoria s que imagem não é exibida.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Figura 10**: s a nova categoria imagem não é exibida ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


O motivo pelo qual não é exibida a nova imagem é porque o `DisplayCategoryPicture.aspx` página que retorna uma imagem de categoria especificada s está configurada para processar bitmaps que têm um cabeçalho OLE. Esse cabeçalho 78 bytes é tirado do `Picture` conteúdo binário de coluna s antes de serem enviados de volta ao cliente. Mas o arquivo JPG que acabou de ser carregados para a nova categoria não tem esse cabeçalho OLE; Portanto, bytes válidos, é necessários estão sendo removidos dos dados binários s da imagem.

Como agora são ambos os bitmaps com cabeçalhos OLE e JPEGs no `Categories` tabela, precisamos atualizar `DisplayCategoryPicture.aspx` para que ele não o cabeçalho OLE retirando para as categorias de oito originais e ignora esta colocação para os registros mais recentes de categoria. Nossa próxima tutorial, examinaremos como atualizar uma imagem existente do registro s e vamos atualizar todas as imagens de categoria antiga para que eles sejam JPGs. Por enquanto, no entanto, usar o seguinte código em `DisplayCategoryPicture.aspx` para Tire os cabeçalhos OLE apenas para essas categorias de oito originais:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Com essa alteração, a imagem JPG agora é renderizada corretamente em GridView.


[![As imagens JPG para novas categorias são renderizados corretamente](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Figura 11**: as imagens JPG para novas categorias são renderizados corretamente ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Etapa 9: Excluindo a publicação no caso de uma exceção

Um dos desafios de armazenar dados binários no sistema de arquivo do web server s é que ela apresenta uma desconexão entre o modelo de dados e seus dados binários. Portanto, sempre que um registro é excluído, os dados binários correspondentes no sistema de arquivos também deverão ser removidos. Isso pode entra em jogo quando inserir, também. Considere o seguinte cenário: um usuário adiciona uma nova categoria, a especificação de uma imagem válida e a publicação. Após clicar no botão de inserção, ocorre um postback e s a DetailsView `ItemInserting` evento ser acionado e salvar a publicação para o sistema de arquivos do servidor s da web. Em seguida, o s ObjectDataSource `Insert()` método é chamado, o que chama o `CategoriesBLL` classe s `InsertWithPicture` método, que chama o `CategoriesTableAdapter` s `InsertWithPicture` método.

Agora, o que acontece se o banco de dados estiver offline ou se houver um erro no `INSERT` instrução SQL? Claramente a inserção falhará, portanto, nenhuma nova linha categoria será adicionada ao banco de dados. Mas, ainda temos o arquivo de publicação carregados no sistema de arquivos da web server s! Este arquivo deve ser excluído caso ocorra uma exceção durante a inserção do fluxo de trabalho.

Como discutido anteriormente o [tratamento BLL e DAL nível exceções em uma página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial, quando uma exceção de dentro do fundo da arquitetura do qual ele é transferido backup por meio de várias camadas. Na camada de apresentação, é possível determinar se ocorreu uma exceção de DetailsView s `ItemInserted` eventos. Este manipulador de eventos também fornece os valores da s ObjectDataSource `InsertParameters`. Portanto, podemos criar um manipulador de eventos para o `ItemInserted` evento verifica se houve uma exceção e, nesse caso, exclui o arquivo especificado para o s ObjectDataSource `brochurePath` parâmetro:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Resumo

Há uma série de etapas que devem ser realizadas para fornecer uma interface baseada na web para adicionar os registros que incluem dados binários. Se os dados binários estão sendo armazenados diretamente no banco de dados, é provável que você precisará atualizar a arquitetura, a adição de métodos específicos para lidar com o caso em que os dados binários estão sendo inseridos. Depois que a arquitetura tiver sido atualizada, a próxima etapa é criar a interface de inserção, que pode ser feita usando um DetailsView que foi personalizado para incluir um controle de carregamento de arquivos para cada campo de dados binários. Os dados carregados podem ser salvo no sistema de arquivo do web server s ou atribuídos a um parâmetro de fonte de dados em DetailsView s `ItemInserting` manipulador de eventos.

Salvar dados binários para o sistema de arquivos requer mais planejamento que salvar os dados diretamente no banco de dados. Um esquema de nomenclatura deve ser escolhido para evitar um carregamento de usuário s substituindo s outro. Além disso, etapas adicionais devem ser executadas para excluir o arquivo carregado se a inserção de banco de dados falhará.

Agora temos a capacidade de adicionar novas categorias para o sistema com uma publicação e a imagem, mas estamos ve ainda para observar como atualizar um categoria s binário os dados existentes ou remover corretamente os dados binários de uma categoria excluído. Vamos explorar esses dois tópicos no tutorial Avançar.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Dave Gardner Teresa Murphy e Bernadette Leigh. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-binary-data-in-the-data-web-controls-vb.md)
> [Próximo](updating-and-deleting-existing-binary-data-vb.md)
