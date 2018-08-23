---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Incluindo um arquivo de opção Carregar ao adicionar um novo registro (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como criar uma interface da Web que permite que o usuário digite dados de texto e carregar arquivos binários. Para ilustrar o t disponível de opções...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: c3887f920126d70b300de5a0d6e09474fd33c332
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830614"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Incluindo uma opção de Upload de arquivo ao adicionar um novo registro (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) ou [baixar PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Este tutorial mostra como criar uma interface da Web que permite que o usuário digite dados de texto e carregar arquivos binários. Para ilustrar as opções disponíveis para armazenar dados binários, um arquivo será salvo no banco de dados enquanto o outro é armazenado no sistema de arquivos.


## <a name="introduction"></a>Introdução

Nos tutoriais anteriores dois exploramos técnicas para armazenar dados binários que está associados com o modelo de dados de s do aplicativo, examinado como usar o controle FileUpload para enviar os arquivos do cliente para o servidor web e viu como apresentar esses dados binários em um dado W controle EB. Podemos var ainda para falar sobre como associar dados carregados com o modelo de dados, no entanto.

Neste tutorial, criaremos uma página da web para adicionar uma nova categoria. Além de caixas de texto para o nome da categoria s e a descrição, essa página será necessário incluir dois controles FileUpload para a nova imagem de categoria s e outro para o folheto do. A imagem carregada será armazenada diretamente no novo registro de s `Picture` coluna, enquanto o folheto será salvo para o `~/Brochures` pasta com o caminho para o arquivo salvo no novo registro s `BrochurePath` coluna.

Antes de criar essa nova página da web, precisaremos atualizar a arquitetura. O `CategoriesTableAdapter` consulta principal de s não recuperar o `Picture` coluna. Consequentemente, o autogerado `Insert` método só tem entradas para o `CategoryName`, `Description`, e `BrochurePath` campos. Portanto, precisamos criar um método adicional no TableAdapter que solicita todos os quatro `Categories` campos. O `CategoriesBLL` classe na camada de lógica de negócios também precisam ser atualizados.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Etapa 1: Adicionando um`InsertWithPicture`método para o`CategoriesTableAdapter`

Quando criamos o `CategoriesTableAdapter` volta a [criando uma camada de acesso de dados](../introduction/creating-a-data-access-layer-cs.md) tutorial, estamos configurados para gerar automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções com base em consulta principal. Além disso, nós o instruímos a empregar a abordagem direta de banco de dados, o que criou os métodos TableAdapter `Insert`, `Update`, e `Delete`. Esses métodos são executados geradas automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções e, consequentemente, aceitar parâmetros de entrada com base nas colunas retornadas pela consulta principal. No [carregar arquivos](uploading-files-cs.md) tutorial aumentássemos a `CategoriesTableAdapter` a consulta principal s para usar o `BrochurePath` coluna.

Uma vez que o `CategoriesTableAdapter` não faz referência a consulta principal de s a `Picture` coluna, podemos não pode adicionar um novo registro nem atualizar um registro existente com um valor para o `Picture` coluna. Para capturar essas informações, podemos pode criar um novo método no TableAdapter que é usado especificamente para inserir um registro com dados binários ou podemos personalizar geradas automaticamente `INSERT` instrução. O problema com a personalização geradas automaticamente `INSERT` instrução é que podemos o risco de ter nossas personalizações substituídas pelo assistente. Por exemplo, imagine que estamos personalizada a `INSERT` instrução para incluir o uso do `Picture` coluna. Isso atualizará o TableAdapter s `Insert` método para incluir um parâmetro de entrada adicional para os dados binários de imagem s s da categoria. Em seguida, criamos um método na camada de lógica de negócios para usar esse método DAL e invocar esse método BLL por meio da camada de apresentação, e tudo o que funcionaria brilhante. Ou seja, até a próxima vez, configuramos o TableAdapter o Assistente de configuração do TableAdapter. Assim que o assistente for concluído, nosso personalizações para o `INSERT` instrução seria substituída, o `Insert` método seria revertida para sua forma antiga, e nosso código não seria mais compilado!

> [!NOTE]
> Essa mensagem inconveniente não é um-problema ao usar procedimentos armazenados, em vez de instruções SQL ad hoc. Um tutorial futuro explorará usando procedimentos armazenados no lugar de instruções SQL ad hoc na camada de acesso a dados.


Para evitar essa possibilidade dor de cabeça, em vez de como personalizar as instruções de SQL gerado automaticamente permitem s em vez disso, crie um novo método para o TableAdapter. Esse método, chamado `InsertWithPicture`, aceite os valores para o `CategoryName`, `Description`, `BrochurePath`, e `Picture` colunas e executar um `INSERT` instrução que armazena todos os quatro valores em um novo registro.

Abra o conjunto de dados tipado e, no Designer, clique com botão direito no `CategoriesTableAdapter` cabeçalho s e escolha Add Query no menu de contexto. Isso inicia o Assistente de configuração de consulta do TableAdapter, que começa com a pergunta como a consulta do TableAdapter deve acessar o banco de dados. Escolha usar instruções SQL e clique em Avançar. A próxima etapa solicitará o tipo de consulta a ser gerado. Desde que criamos re criando uma consulta para adicionar um novo registro para o `Categories` de tabela, escolha Inserir e clique em Avançar.


[![Selecione a opção de inserção](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Figura 1**: selecione a opção de inserir ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Agora, precisamos especificar o `INSERT` instrução SQL. O assistente sugere automaticamente um `INSERT` instrução corresponde à consulta principal do TableAdapter s. Nesse caso, ele s uma `INSERT` instrução que insere a `CategoryName`, `Description`, e `BrochurePath` valores. A instrução Update para que o `Picture` coluna está incluída junto com um `@Picture` parâmetro, da seguinte forma:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

A tela final do assistente nos pede para nomear o novo método do TableAdapter. Insira `InsertWithPicture` e clique em Concluir.


[![Nomeie o novo InsertWithPicture de método do TableAdapter](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Figura 2**: Nomeie o novo método do TableAdapter `InsertWithPicture` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Etapa 2: Atualizar a camada de lógica de negócios

Uma vez que a camada de apresentação devem fazer interface com a camada de lógica de negócios em vez de ignorar nele para ir diretamente para a camada de acesso a dados, precisamos criar um método BLL que invoca o método DAL que acabamos de criar (`InsertWithPicture`). Para este tutorial, crie um método na `CategoriesBLL` classe denominada `InsertWithPicture` que aceita como entrada três `string` s e um `byte` matriz. O `string` parâmetros de entrada são para o nome da categoria s, descrição e caminho do arquivo de folheto, enquanto o `byte` matriz é para o conteúdo binário de imagem categoria s. Como mostra o código a seguir, esse método BLL invoca o método DAL correspondente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Certifique-se de que você salvou o conjunto de dados tipado antes de adicionar o `InsertWithPicture` método para a BLL. Uma vez que o `CategoriesTableAdapter` código da classe é gerado automaticamente com base em um DataSet tipado, se don t primeiro salvar suas alterações para o conjunto de dados tipado a `Adapter` propriedade ganhou um t saber sobre o `InsertWithPicture` método.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Etapa 3: Listando as categorias existentes e seus dados binários

Neste tutorial vamos criar uma página que permite que um usuário final adicionar uma nova categoria para o sistema, fornecendo uma imagem e folheto para a nova categoria. No [tutorial anterior](displaying-binary-data-in-the-data-web-controls-cs.md) usamos um GridView com um TemplateField e ImageField para exibir cada nome de categoria s, descrição, imagem e um link para baixar seu folheto. Deixe o s replicar essa funcionalidade para este tutorial, criando uma página que lista todas as categorias existentes e permite que novos processos a serem criados.

Comece abrindo o `DisplayOrDownload.aspx` página a partir de `BinaryData` pasta. Vá para a exibição da fonte e copie a GridView e ObjectDataSource s sintaxe declarativa, colando-o dentro de `<asp:Content>` elemento no `UploadInDetailsView.aspx`. Além disso, não se esqueça de copiar ao longo de `GenerateBrochureLink` a classe code-behind do método `DisplayOrDownload.aspx` para `UploadInDetailsView.aspx`.


[![Copie e cole a sintaxe declarativa do DisplayOrDownload.aspx para UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Figura 3**: copie e cole a sintaxe declarativa do `DisplayOrDownload.aspx` à `UploadInDetailsView.aspx` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Depois de copiar a sintaxe declarativa e `GenerateBrochureLink` método de failover para o `UploadInDetailsView.aspx` de página, exibir a página por meio de um navegador para garantir que tudo o que foi copiado corretamente sobre. Você deve ver um GridView as oito categorias de listagem que inclui um link para baixar o folheto, bem como a imagem de categoria s.


[![Agora você deve ver cada categoria, juntamente com seus dados binários](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Figura 4**: você deve agora veja cada categoria juntamente com seus dados binários ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Etapa 4: Configurando o`CategoriesDataSource`à inserção de suporte

O `CategoriesDataSource` ObjectDataSource usado pelo `Categories` GridView atualmente não fornece a capacidade de inserir dados. Para dar suporte à inserção desse controle de fonte de dados, precisamos mapear suas `Insert` método para um método em seu objeto subjacente, `CategoriesBLL`. Em particular, queremos mapeá-lo para o `CategoriesBLL` método que adicionamos novamente na etapa 2, `InsertWithPicture`.

Inicie clicando no link configurar fonte de dados a partir da marca inteligente do ObjectDataSource s. A primeira tela mostra o objeto de fonte de dados está configurada para funcionar com, `CategoriesBLL`. Deixe essa configuração como-está e clique em Next para ir para a tela Definir métodos de dados. Mover para a guia Inserir e escolha o `InsertWithPicture` método na lista suspensa. Clique em Concluir para concluir o assistente.


[![Configurar o ObjectDataSource para usar o método InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `InsertWithPicture` método ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Após a conclusão do assistente, o Visual Studio poderá solicitar se você quiser atualizar campos e as chaves, que irá gerar novamente os dados da Web controla os campos. Selecione não, porque se você escolher Sim, substituirá todas as personalizações de campo que tenha feito.


Depois de concluir o assistente, o ObjectDataSource agora incluem um valor para seus `InsertMethod` propriedade, bem como `InsertParameters` para as colunas de quatro categorias, como a seguinte marcação declarativa ilustra:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Etapa 5: Criar a Interface de inserção

Como inicialmente abordados os [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), o controle DetailsView fornece uma interface de inserção internos que pode ser utilizada ao trabalhar com um controle de fonte de dados que dá suporte à inserção. Permitir que o s adicione um controle DetailsView a esta página acima GridView que processará permanentemente sua interface de inserção, que permite ao usuário adicionar rapidamente uma nova categoria. Após a adição de uma nova categoria em DetailsView, GridView abaixo dele será automaticamente atualizar e exibir a nova categoria.

Início arrastando um DetailsView da caixa de ferramentas para o Designer acima GridView, definindo sua `ID` propriedade para `NewCategory` e apagando o `Height` e `Width` valores de propriedade. Da DetailsView s marca inteligente, associá-lo ao existente `CategoriesDataSource` e, em seguida, marque a caixa de seleção Permitir inserção.


[![Associar o CategoriesDataSource DetailsView e ativar inserir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Figura 6**: associar DetailsView para o `CategoriesDataSource` e permitir inserção ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Para processar permanentemente DetailsView na sua interface de inserção, defina suas `DefaultMode` propriedade para `Insert`.

Observe que o DetailsView tem cinco BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` Embora o `CategoryID` BoundField não é renderizado na interface de inserção porque seu `InsertVisible` propriedade é definida como `false`. Esses BoundFields existe porque eles são as colunas retornadas pelo `GetCategories()` método, que é o que o ObjectDataSource invoca para recuperar seus dados. Para inserir, no entanto, podemos don t deseja permitir que o usuário especifique um valor para `NumberOfProducts`. Além disso, é necessário permitir que ele carregue uma imagem para a nova categoria, bem como carregar um PDF para o folheto do.

Remover o `NumberOfProducts` BoundField do DetailsView totalmente e atualize o `HeaderText` propriedades da `CategoryName` e `BrochurePath` BoundFields à categoria e folheto, respectivamente. Em seguida, converter o `BrochurePath` BoundField em um TemplateField e adicionar um novo TemplateField para a imagem, dando a esse novo TemplateField um `HeaderText` valor da imagem. Mover o `Picture` TemplateField para que ele fique entre a `BrochurePath` TemplateField e CommandField.


![Associar o CategoriesDataSource DetailsView e ativar inserir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Figura 7**: associar DetailsView para o `CategoriesDataSource` e ativar inserir


Se você tiver convertido a `BrochurePath` o TemplateField de BoundField em um TemplateField por meio da caixa de diálogo Editar campos, inclui um `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. Somente o `InsertItemTemplate` é necessário, no entanto, fique à vontade remover os dois outros modelos. Neste ponto, sua sintaxe declarativa do DetailsView s deve ser semelhante ao seguinte:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Adicionando controles de FileUpload para o folheto e campos de imagem

No momento, o `BrochurePath` s TemplateField `InsertItemTemplate` contém uma caixa de texto, enquanto o `Picture` TemplateField não contém todos os modelos. É necessário atualizar essas duas s TemplateField `InsertItemTemplate` s para usar controles FileUpload.

Da DetailsView s marca inteligente, escolha a opção de editar modelos e, em seguida, selecione a `BrochurePath` TemplateField s `InsertItemTemplate` na lista suspensa. Remover a caixa de texto e, em seguida, arraste um controle FileUpload da caixa de ferramentas para o modelo. Defina o controle FileUpload s `ID` para `BrochureUpload`. Da mesma forma, adicione um controle FileUpload para o `Picture` TemplateField s `InsertItemTemplate`. Definir esse controle FileUpload s `ID` para `PictureUpload`.


[![Adicionar um controle FileUpload para o InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Figura 8**: adicionar um controle FileUpload para o `InsertItemTemplate` ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Depois de fazer essas adições, a sintaxe declarativa do dois TemplateField s será:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Quando um usuário adiciona uma nova categoria, queremos garantir que o folheto e a imagem são do tipo de arquivo correto. Para criar o folheto, o usuário deve fornecer um PDF. Para a imagem, é necessário que o usuário carregue um arquivo de imagem, mas podemos permitir *qualquer* apenas os arquivos de imagem de um tipo específico, como GIFs ou JPGs ou de arquivo de imagem? Para permitir diferentes tipos de arquivo, d precisamos estender a `Categories` esquema para incluir uma coluna que captura o tipo de arquivo para que esse tipo pode ser enviado ao cliente por meio `Response.ContentType` em `DisplayCategoryPicture.aspx`. Já que estamos não t tem uma coluna desse tipo, seria prudente restringir os usuários para fornecer apenas um tipo de arquivo de imagem específico. O `Categories` imagens existentes da tabela s são bitmaps, mas JPGs estão um formato de arquivo mais adequado para imagens atendidas pela web.

Se um usuário carrega um tipo de arquivo incorreto, é necessário cancelar a inserção e exibir uma mensagem indicando que o problema. Adicione um controle de Web de rótulo abaixo DetailsView. Definir seus `ID` propriedade para `UploadWarning`, desmarque out seu `Text` propriedade, defina o `CssClass` propriedade para aviso e o `Visible` e `EnableViewState` propriedades para `false`. O `Warning` classe CSS é definida em `Styles.css` e renderiza o texto em uma fonte grande, vermelho, em itálico e negrito.

> [!NOTE]
> O ideal é que o `CategoryName` e `Description` BoundFields seria convertida em TemplateFields e suas interfaces de inserção personalizadas. O `Description` inserir interface, por exemplo, seria provavelmente mais adequado por meio de uma caixa de texto de várias linha. E, como o `CategoryName` coluna não aceita `NULL` valores, um RequiredFieldValidator deve ser adicionado para garantir que o usuário fornece um valor para o novo nome da categoria s. Essas etapas são deixadas como um exercício para o leitor. Consultar novamente [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) para um olhar detalhado sobre aumentando as interfaces de modificação de dados.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Etapa 6: Salvar o folheto carregado para o sistema de arquivos do Web Server s

Quando o usuário insere os valores para uma nova categoria e clica no botão de inserção, ocorre um postback e o fluxo de trabalho de inserção é revelado. Primeiro, o s DetailsView [ `ItemInserting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) é acionado. Em seguida, o s ObjectDataSource `Insert()` método é invocado, o que resulta em um novo registro que está sendo adicionado para o `Categories` tabela. Depois disso, o s DetailsView [ `ItemInserted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) é acionado.

Antes do s ObjectDataSource `Insert()` método é invocado, devemos primeiro certifique-se de que os tipos de arquivos apropriados foram carregados pelo usuário e, em seguida, salvar o folheto do PDF para o sistema de arquivos do servidor s da web. Criar um manipulador de eventos para o s DetailsView `ItemInserting` eventos e adicione o seguinte código:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

O manipulador de eventos começa fazendo referência a `BrochureUpload` controle FileUpload dos modelos de s DetailsView. Em seguida, se um folheto tiver sido carregado, a extensão do arquivo carregado s é examinada. Se a extensão não é. PDF, em seguida, um aviso é exibido, a inserção é cancelada e termina a execução do manipulador de eventos.

> [!NOTE]
> Contar com a extensão de s do arquivo carregado não é uma técnica segura para garantir que o arquivo carregado é um documento em PDF. O usuário pode ter um documento válido de PDF com a extensão `.Brochure`, ou poderia ter levado um documento não PDF e deu a ela um `.pdf` extensão. O conteúdo binário do arquivo s precisariam ser examinado por meio de programação para a mais conclusivamente Verifique se o tipo de arquivo. Essas abordagens completas, no entanto, geralmente são um exagero; Verificando a extensão é suficiente para a maioria dos cenários.


Conforme discutido na [carregar arquivos](uploading-files-cs.md) tutorial, é necessário ter cuidado ao salvamento de arquivos para o sistema de arquivos para esse carregamento de um usuário s não substitua s outro. Para este tutorial, estamos tentará usar o mesmo nome como o arquivo carregado. Se já existe um arquivo no `~/Brochures` diretório com esse mesmo nome de arquivo, no entanto, podemos irá acrescentar um número ao final até encontra um nome exclusivo. Por exemplo, se o usuário carrega um arquivo de folheto denominado `Meats.pdf`, mas já existe um arquivo chamado `Meats.pdf` na `~/Brochures` pasta, vamos alterar o nome do arquivo salvo para `Meats-1.pdf`. Se o que existe, tentaremos `Meats-2.pdf`e assim por diante, até encontra um nome de arquivo exclusivo.

O código a seguir usa o [ `File.Exists(path)` método](https://msdn.microsoft.com/library/system.io.file.exists.aspx) para determinar se um arquivo já existe com o nome de arquivo especificado. Nesse caso, ele continua experimentar os novos nomes de arquivo para o folheto até que nenhum conflito seja encontrado.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Depois que um nome de arquivo válido foi encontrado, o arquivo precisa ser salvo no sistema de arquivos e os s ObjectDataSource `brochurePath``InsertParameter` valor precisa ser atualizado para que esse nome de arquivo é gravado no banco de dados. Como vimos na *carregar arquivos* tutorial, o arquivo pode ser salvo usando o controle FileUpload s `SaveAs(path)` método. Para atualizar o s ObjectDataSource `brochurePath` parâmetro, use o `e.Values` coleção.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Etapa 7: Salvar a imagem carregada no banco de dados

Para armazenar a imagem carregada no novo `Categories` registro, precisamos atribuir o conteúdo binário carregado para o s ObjectDataSource `picture` parâmetro em s DetailsView `ItemInserting` eventos. Antes de fazermos essa atribuição, no entanto, precisamos primeiro certifique-se de que a imagem carregada é um JPG e não outro tipo de imagem. Como na etapa 6, deixe s para usar a extensão de arquivo imagem carregada s para determinar seu tipo.

Enquanto o `Categories` table permite `NULL` os valores para o `Picture` coluna, todas as categorias atualmente tiver uma imagem. Deixe o s forçar o usuário a fornecer uma imagem ao adicionar uma nova categoria por meio desta página. O código a seguir verifica para garantir que uma imagem foi carregada e se ele tem uma extensão apropriada.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Esse código deve ser colocado *antes de* o código da etapa 6 para que se houver um problema com o carregamento de imagem, o manipulador de eventos será encerrado antes que o arquivo de folheto é salvo no sistema de arquivos.

Supondo que um arquivo apropriado tiver sido carregado, atribua o conteúdo binário carregado para o valor de parâmetro da imagem s com a seguinte linha de código:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Completo`ItemInserting`manipulador de eventos

Para fins de integridade, aqui está o `ItemInserting` manipulador de eventos em sua totalidade:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Etapa 8: Corrigindo o`DisplayCategoryPicture.aspx`página

Let s dedique uns momentos para testar a interface de inserção e `ItemInserting` manipulador de eventos que foi criado durante os últimos algumas etapas. Visite o `UploadInDetailsView.aspx` página por meio de um navegador e tentar adicionar uma categoria, mas omita a imagem, ou especificar uma imagem não JPG ou um folheto não PDF. Em qualquer um desses casos, será exibida uma mensagem de erro e o fluxo de trabalho de inserção cancelada.


[![Uma mensagem de aviso será exibido se um tipo de arquivo inválido é carregado](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Figura 9**: uma mensagem de aviso é exibido se um tipo de arquivo inválido é carregado ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Uma vez você verificou que a página requer uma imagem a ser carregado e t ganha aceitar arquivos não-PDF ou não JPG, adicionar uma nova categoria com imagem válida JPG, deixando o campo de folheto vazio. Depois de clicar no botão de inserção, a página fará o postback e será adicionado um novo registro para o `Categories` tabela com o conteúdo binário de imagem carregada s armazenados diretamente no banco de dados. O GridView é atualizado e mostra uma linha para a categoria recém adicionada, mas, como mostra a Figura 10, a nova imagem s categoria não é renderizada corretamente.


[![A nova categoria s que imagem não é exibida.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Figura 10**: s nova categoria a imagem não é exibida ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


O motivo pelo qual a nova imagem não é exibida é porque o `DisplayCategoryPicture.aspx` página que retorna uma imagem de categoria especificada s está configurada para processar bitmaps que possuem um cabeçalho OLE. Esse cabeçalho de 78 bytes é tirado do `Picture` conteúdo binário de coluna s antes de serem enviadas de volta para o cliente. Mas o arquivo JPG que acabou de carregar para a nova categoria não tem esse cabeçalho OLE; Portanto, bytes válidos, necessárias estão sendo removidos dos dados binários de s de imagem.

Já que agora são ambos os bitmaps com cabeçalhos OLE e JPGs na `Categories` tabela, precisamos atualizar `DisplayCategoryPicture.aspx` para que ele não o cabeçalho OLE colocação para oito categorias originais e ignora essa colocação para os registros mais recentes de categoria. Em nosso próximo tutorial, examinaremos como atualizar uma imagem existente do registro s e atualizaremos todas as imagens de categoria antigo para que eles sejam JPGs. Por enquanto, porém, use o seguinte código no `DisplayCategoryPicture.aspx` para retirar os cabeçalhos OLE apenas para essas categorias de oito originais:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Com essa alteração, a imagem JPG agora é renderizada corretamente em GridView.


[![As imagens JPG para novas categorias são renderizados corretamente](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Figura 11**: as imagens JPG para novas categorias são renderizados corretamente ([clique para exibir a imagem em tamanho normal](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Etapa 9: Excluir o folheto diante de uma exceção

Um dos desafios de armazenar dados binários no sistema de arquivo do web server s é que ela introduz uma desconexão entre o modelo de dados e seus dados binários. Portanto, sempre que um registro é excluído, os dados binários correspondentes no sistema de arquivos devem também ser removidos. Isso pode surgir quando são inseridos, também. Considere o seguinte cenário: um usuário adiciona uma nova categoria, especificando um folheto e uma imagem válida. Após clicar no botão de inserção, ocorre um postback e o s DetailsView `ItemInserting` evento é acionado, salvando o folheto do sistema de arquivos do servidor s da web. Em seguida, o s ObjectDataSource `Insert()` método é invocado, que chama o `CategoriesBLL` classe s `InsertWithPicture` método, que chama o `CategoriesTableAdapter` s `InsertWithPicture` método.

Agora, o que acontece se o banco de dados estiver offline ou se houver um erro no `INSERT` instrução SQL? Claramente o INSERT falhará, portanto, nenhuma nova linha de categoria será adicionada ao banco de dados. Mas, ainda temos no arquivo do folheto carregados no sistema de arquivo do web server s! Esse arquivo precisa ser excluído diante de uma exceção durante a inserção de fluxo de trabalho.

Como discutido anteriormente a [tratamento BLL - e exceções de nível DAL em uma página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial, quando uma exceção é lançada de dentro a fundo a arquitetura é aparecido por meio de várias camadas. Na camada de apresentação, podemos determinar se ocorreu uma exceção de s DetailsView `ItemInserted` eventos. Esse manipulador de eventos também fornece os valores de s o ObjectDataSource `InsertParameters`. Portanto, podemos criar um manipulador de eventos para o `ItemInserted` evento verifica se houve uma exceção e, nesse caso, exclui o arquivo especificado por s o ObjectDataSource `brochurePath` parâmetro:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Resumo

Há uma série de etapas que devem ser realizadas para fornecer uma interface baseada na web para adicionar os registros que incluem dados binários. Se os dados binários estão sendo armazenados diretamente no banco de dados, provavelmente que você precisará atualizar a arquitetura, a adição de métodos específicos para manipular o caso em que os dados binários está sendo inseridos. Depois que a arquitetura tiver sido atualizada, a próxima etapa é criar a interface de inserção, que pode ser feita usando um DetailsView que foi personalizado para incluir um controle FileUpload para cada campo de dados binários. Os dados carregados podem ser salvo no sistema de arquivo do web server s ou atribuídos a um parâmetro de fonte de dados em s DetailsView `ItemInserting` manipulador de eventos.

Salvar dados binários para o sistema de arquivos requer mais planejamento que salvar os dados diretamente no banco de dados. Um esquema de nomenclatura deve ser escolhido para evitar um upload de usuário s substituindo s outro. Além disso, etapas adicionais devem ficar para excluir o arquivo carregado, se a inserção de banco de dados falhará.

Agora temos a capacidade de adicionar novas categorias para o sistema com um folheto e imagem, mas podemos ve ainda para observar como atualizar um existente dados binários de s da categoria ou como remover corretamente os dados binários para uma categoria excluído. Vamos explorar esses dois tópicos no próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores de avanço para este tutorial foram Dave Gardner, Teresa Murphy e Bernadette Leigh. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-binary-data-in-the-data-web-controls-cs.md)
> [Próximo](updating-and-deleting-existing-binary-data-cs.md)
