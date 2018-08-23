---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Atualizando e excluindo dados binários existentes (c#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, vimos como o controle GridView torna simples a editar e excluir dados de texto. Neste tutorial, podemos ver como o controle GridView também tornar...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7ab615585da1fb324f0740c1626c70dd7e99df
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831729"
---
<a name="updating-and-deleting-existing-binary-data-c"></a>Atualizando e excluindo dados binários existentes (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) ou [baixar PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> Nos tutoriais anteriores, vimos como o controle GridView torna simples a editar e excluir dados de texto. Neste tutorial, podemos ver como o controle GridView também torna possível editar e excluir dados binários, se esses dados binários é salva no banco de dados ou armazenados no sistema de arquivos.


## <a name="introduction"></a>Introdução

Sobre os últimos tutoriais de três nós adicionou um pouco de funcionalidade para trabalhar com dados binários. Começamos adicionando um `BrochurePath` coluna para o `Categories` de tabela e atualizada a arquitetura adequadamente. Também adicionamos métodos de camada de acesso a dados e camada de lógica comercial para trabalhar com categorias tabela s existente `Picture` coluna, que contém o s conteúdo binário de um arquivo de imagem. Criamos páginas da web para apresentar os dados binários em um GridView um link de download para o folheto, com a imagem de categoria s mostrada em um `<img>` elemento e tiver adicionado um DetailsView para permitir que os usuários adicionar uma nova categoria e carregar seus dados de folheto e imagem.

Tudo o que resta a ser implementada é a capacidade de editar e excluir as categorias existentes, vamos executar este tutorial usando o GridView s internos de edição e exclusão de recursos. Ao editar uma categoria, o usuário poderá, opcionalmente, carregar uma nova imagem ou tenham a categoria continue a usar o já existente. Para criar o folheto, eles podem optar por usar folheto existente, para carregar um novo folheto ou para indicar que a categoria não tem mais um folheto associado a ele. Permitir que o s começar!

## <a name="step-1-updating-the-data-access-layer"></a>Etapa 1: Atualizar a camada de acesso a dados

A DAL tem gerado automaticamente `Insert`, `Update`, e `Delete` métodos, mas esses métodos foram gerados com base no `CategoriesTableAdapter` consulta principal s, que não inclui o `Picture` coluna. Portanto, o `Insert` e `Update` métodos não têm parâmetros para especificar os dados binários para a imagem de categoria s. Como fizemos [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md), precisamos criar um novo método do TableAdapter para atualizar o `Categories` ao especificar dados binários de tabela.

Abra o conjunto de dados tipado e, no Designer, clique com botão direito no `CategoriesTableAdapter` cabeçalho s e escolha Add Query no menu de contexto para launche o Assistente de configuração de consulta do TableAdapter. Este assistente inicia nos pedindo a como a consulta do TableAdapter deve acessar o banco de dados. Escolha usar instruções SQL e clique em Avançar. A próxima etapa solicitará o tipo de consulta a ser gerado. Desde que criamos re criando uma consulta para adicionar um novo registro para o `Categories` de tabela, escolha a atualização e clique em Avançar.


[![Selecione a opção de atualização](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Figura 1**: selecione a opção de atualização ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


Agora, precisamos especificar o `UPDATE` instrução SQL. O assistente sugere automaticamente um `UPDATE` instrução corresponde à consulta principal do TableAdapter s (que atualiza o `CategoryName`, `Description`, e `BrochurePath` valores). Altere a instrução para que o `Picture` coluna está incluída junto com um `@Picture` parâmetro, da seguinte forma:


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

A tela final do assistente nos pede para nomear o novo método do TableAdapter. Insira `UpdateWithPicture` e clique em Concluir.


[![Nomeie o novo UpdateWithPicture de método do TableAdapter](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Figura 2**: Nomeie o novo método do TableAdapter `UpdateWithPicture` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Etapa 2: Adicionando os métodos de camada de lógica de negócios

Além de atualizar a DAL, precisamos atualizar a BLL para incluir métodos para atualizar e excluir uma categoria. Esses são os métodos que serão invocados da camada de apresentação.

Para excluir uma categoria, podemos usar o `CategoriesTableAdapter` s autogerado `Delete` método. Adicione o seguinte método para o `CategoriesBLL` classe:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Para este tutorial, vamos s criar dois métodos para atualizar uma categoria - uma que se espera que os dados da imagem binária e invoca o `UpdateWithPicture` método que acabamos de adicionar para o `CategoriesTableAdapter` e outra que aceita apenas o `CategoryName`, `Description`e `BrochurePath`valores e usa `CategoriesTableAdapter` classe s autogerado `Update` instrução. A lógica por trás do uso de dois métodos é que em algumas circunstâncias, um usuário pode desejar atualizar a imagem de s categoria juntamente com seus outros campos, na qual o caso, o usuário terá que carregar a nova imagem. Os dados binários da imagem carregada s, em seguida, podem ser usados no `UPDATE` instrução. Em outros casos, o usuário só pode estar interessado na atualização, digamos, o nome e descrição. Porém, se o `UPDATE` instrução espera que os dados binários para o `Picture` também a coluna, podemos d precisarão fornecer essa informação também. Isso exigiria uma viagem extra no banco de dados para trazer de volta os dados da imagem para o registro que está sendo editado. Portanto, queremos que dois `UPDATE` métodos. A camada de lógica de negócios determinará qual delas usar com base em se os dados da imagem são fornecidos ao atualizar a categoria.

Para facilitar isso, adicione dois métodos para o `CategoriesBLL` classe nomeadas `UpdateCategory`. O primeiro deles deve aceitar três `string` s, uma `byte` array e um `int` como sua entrada parâmetros; a segunda, apenas três `string` s e um `int`. O `string` parâmetros de entrada são para o nome da categoria s, descrição e caminho do arquivo de folheto, o `byte` matriz é para o conteúdo binário de imagem categoria s e o `int` identifica o `CategoryID` do registro a atualizar. Observe que a primeira sobrecarga invoca o segundo se passado `byte` matriz é `null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Etapa 3: Copiar sobre a funcionalidade de exibição e inserção

No [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md) criamos uma página chamada `UploadInDetailsView.aspx` que listadas todas as categorias em um GridView e fornecido um DetailsView para adicionar novas categorias para o sistema. Neste tutorial, estenderemos GridView para incluir a edição e exclusão de suporte. Em vez de continuar a trabalhar a partir `UploadInDetailsView.aspx`, em vez disso, let s colocar alterações neste tutorial s no `UpdatingAndDeleting.aspx` página da mesma pasta, `~/BinaryData`. Copie e cole a marcação declarativa e código do `UploadInDetailsView.aspx` para `UpdatingAndDeleting.aspx`.

Comece abrindo o `UploadInDetailsView.aspx` página. Copie toda a sintaxe declarativa dentro a `<asp:Content>` elemento, conforme mostrado na Figura 3. Em seguida, abra `UpdatingAndDeleting.aspx` e cole essa marcação dentro de seu `<asp:Content>` elemento. Da mesma forma, copie o código do `UploadInDetailsView.aspx` página de classe code-behind de s para `UpdatingAndDeleting.aspx`.


[![Copiar a marcação declarativa de UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Figura 3**: Copie a marcação declarativa de `UploadInDetailsView.aspx` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


Depois de copiar sobre a marcação declarativa e código, visite `UpdatingAndDeleting.aspx`. Você deve ver a mesma saída e têm a mesma experiência do usuário assim como acontece com `UploadInDetailsView.aspx` página do tutorial anterior.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Etapa 4: Adicionando, excluindo o suporte para o ObjectDataSource e GridView

Como discutimos na [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, o GridView fornece recursos internos de exclusão e esses recursos podem ser habilitados na escala de uma caixa de seleção se grade s subjacente fonte de dados oferecer suporte a exclusão. Atualmente, o ObjectDataSource GridView está associado a (`CategoriesDataSource`) não oferece suporte a exclusão.

Para corrigir isso, clique na opção de configurar fonte de dados da marca inteligente s ObjectDataSource para iniciar o assistente. A primeira tela mostra que o ObjectDataSource está configurado para trabalhar com o `CategoriesBLL` classe. Clique em Avançar. Atualmente, apenas o ObjectDataSource s `InsertMethod` e `SelectMethod` propriedades são especificadas. No entanto, o assistente preenchida automaticamente as listas suspensas nas guias de UPDATE e DELETE com a `UpdateCategory` e `DeleteCategory` métodos, respectivamente. Isso ocorre porque na `CategoriesBLL` classe é marcada como esses métodos usando o `DataObjectMethodAttribute` como os métodos padrão para atualizar e excluir.

Por enquanto, a lista de suspensa atualização guia s como (nenhum), mas deixe a lista suspensa de guia s de exclusão definida como `DeleteCategory`. Vamos voltar para este assistente na etapa 6 para adicionar suporte à atualização.


[![Configurar o ObjectDataSource para usar o método DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Figura 4**: configurar o ObjectDataSource para usar o `DeleteCategory` método ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> Após a conclusão do assistente, o Visual Studio poderá solicitar se você quiser atualizar campos e as chaves, que irá gerar novamente os dados da Web controla os campos. Selecione não, porque se você escolher Sim, substituirá todas as personalizações de campo que tenha feito.


O ObjectDataSource agora incluirá um valor para seus `DeleteMethod` propriedade, bem como a `DeleteParameter`. Lembre-se de que, ao usar o Assistente para especificar os métodos, o Visual Studio define o s ObjectDataSource `OldValuesParameterFormatString` propriedade para `original_{0}`, que causa problemas com a atualização e exclua as invocações de método. Portanto, limpar essa propriedade completamente ou redefini-lo para o padrão, `{0}`. Se você precisar atualizar sua memória nessa propriedade ObjectDataSource, consulte o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial.

Depois de concluir o assistente e corrigindo o `OldValuesParameterFormatString`, a marcação declarativa do ObjectDataSource s deve ter aparência semelhante à seguinte:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Depois de configurar o ObjectDataSource, adicione recursos excluindo o GridView, marcando a caixa de seleção Habilitar exclusão da marca inteligente s GridView. Isso adicionará um CommandField GridView cujos `ShowDeleteButton` estiver definida como `true`.


[![Habilitar o suporte para exclusão em GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Figura 5**: habilitar o suporte para exclusão no GridView ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


Reserve um tempo para testar a funcionalidade de exclusão. Há uma chave estrangeira entre a `Products` tabela s `CategoryID` e o `Categories` tabela s `CategoryID`, portanto, você obterá uma exceção de violação de restrição de chave estrangeira se você tentar excluir qualquer uma das oito primeiras categorias. Para testar essa funcionalidade horizontalmente, adicione uma nova categoria, fornecendo um folheto e a imagem. Categoria meu teste, mostrada na Figura 6, inclui um arquivo de folheto de teste chamado `Test.pdf` e uma imagem de teste. Figura 7 mostra o GridView depois que a categoria de teste foi adicionada.


[![Adicionar uma categoria de teste com um folheto e imagem](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Figura 6**: adicionar uma categoria de teste com um folheto e a imagem ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![Depois de inserir a categoria de teste, ele é exibido em GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Figura 7**: depois de inserir a categoria de teste, ele é exibido no GridView ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


No Visual Studio, atualize o Gerenciador de soluções. Agora, você verá um novo arquivo na `~/Brochures` pasta, `Test.pdf` (consulte a Figura 8).

Em seguida, clique no link excluir a linha de categoria de teste, fazendo com que a página para o postback e o `CategoriesBLL` classe s `DeleteCategory` método a ser acionado. Isso invocará o s DAL `Delete` método, causando apropriado `DELETE` instrução a ser enviada ao banco de dados. Os dados, em seguida, são reassociados ao GridView e a marcação é enviada de volta ao cliente com a categoria de teste não está mais presente.

Enquanto o fluxo de trabalho de exclusão removido com êxito o registro da categoria de teste do `Categories` tabela, ele não removeu seu arquivo de folheto do sistema de arquivos da web do servidor s. Atualize o Gerenciador de soluções e você verá que `Test.pdf` ainda esteja no `~/Brochures` pasta.


![O arquivo Test.pdf não foi excluído do sistema de arquivo do Web Server s](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Figura 8**: O `Test.pdf` arquivo não foi excluído do sistema de arquivos da Web do servidor s


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Etapa 5: Removendo o arquivo de folheto s categoria excluído

Uma das desvantagens de armazenar dados binários externos ao banco de dados é que as etapas adicionais devem ser executadas para limpar esses arquivos quando o registro de banco de dados associado será excluído. O GridView e ObjectDataSource fornecem eventos que são disparados antes e depois que o comando de exclusão foi executado. Na verdade, precisamos criar manipuladores de eventos para os eventos de pré e pós-ação. Antes do `Categories` registro é excluído, precisamos determinar seu caminho de arquivo s PDF, mas podemos don t deseja excluir o PDF antes que a categoria é excluída caso há alguma exceção e a categoria não é excluída.

O s GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) é acionado antes que o comando de exclusão do ObjectDataSource s foi invocado, enquanto seu [ `RowDeleted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) é acionado depois. Crie manipuladores de eventos para esses dois eventos usando o seguinte código:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

No `RowDeleting` manipulador de eventos, o `CategoryID` da linha que está sendo excluído é capturado de s GridView `DataKeys` coleção, que pode ser acessada no manipulador de eventos por meio do `e.Keys` coleção. Em seguida, o `CategoriesBLL` classe s `GetCategoryByCategoryID(categoryID)` é chamado para retornar informações sobre o registro que está sendo excluído. Se retornado `CategoriesDataRow` objeto tem um não -`NULL``BrochurePath` e em seguida, ele é armazenado na variável de página de valor `deletedCategorysPdfPath` para que o arquivo pode ser excluído no `RowDeleted` manipulador de eventos.

> [!NOTE]
> Em vez de recuperar o `BrochurePath` detalhes para o `Categories` registro sendo excluído na `RowDeleting` manipulador de eventos, pode também adicionamos o `BrochurePath` para o s GridView `DataKeyNames` propriedade e acessado o valor do registro s por meio de `e.Keys` coleção. Isso seria um pouco aumentar o tamanho de estado de exibição do GridView s, mas seria reduzir a quantidade de código necessário e salvar uma viagem ao banco de dados.


Após o comando de exclusão s subjacente foi invocado, o ObjectDataSource s o GridView `RowDeleted` manipulador do evento é acionado. Se não houve nenhuma exceção na exclusão de dados e não há um valor para `deletedCategorysPdfPath`, em seguida, o PDF é excluído do sistema de arquivos. Observe que esse código extra não é necessária para limpar os dados binários de categoria s associados com sua imagem. Que s porque os dados de imagem são armazenados diretamente no banco de dados, portanto, excluindo o `Categories` linha também exclui esses dados de imagem de categoria s.

Depois de adicionar os dois manipuladores de evento, execute novamente este caso de teste. Ao excluir a categoria, seu PDF associado também é excluído.

Atualizar dados binários existentes s registros associados fornece alguns desafios interessantes. O restante deste tutorial investiga adicionando recursos de atualização para o folheto e a imagem. Etapa 6 explora as técnicas para atualizar as informações de folheto enquanto a etapa 7 examina atualizando a imagem.

## <a name="step-6-updating-a-category-s-brochure"></a>Etapa 6: Atualizando um folheto de s de categoria

Conforme discutido na [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, o GridView oferece suporte interno de edição de nível de linha que pode ser implementado através de escala de uma caixa de seleção se a fonte de dados subjacente é configurado adequadamente. Atualmente, o `CategoriesDataSource` ObjectDataSource ainda não está configurado para incluir a atualização de suporte, para que o permitem s adicioná-la no.

Clique no link configurar fonte de dados do Assistente de s ObjectDataSource e prossiga para a segunda etapa. Devido a `DataObjectMethodAttribute` usado na `CategoriesBLL`, a lista suspensa de atualização deve ser preenchida automaticamente com o `UpdateCategory` sobrecarga que aceita quatro parâmetros de entrada (para todas as colunas, mas `Picture`). Altere isso para que ele usa a sobrecarga com cinco parâmetros.


[![Configurar o ObjectDataSource para usar o método UpdateCategory não que inclui um parâmetro de imagem](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Figura 9**: configurar o ObjectDataSource para usar o `UpdateCategory` que inclui um parâmetro para o método `Picture` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


O ObjectDataSource agora incluirá um valor para seus `UpdateMethod` propriedade, bem como correspondente `UpdateParameter` s. Conforme observado na etapa 4, o Visual Studio define o s ObjectDataSource `OldValuesParameterFormatString` propriedade para `original_{0}` ao usar o Assistente Configurar fonte de dados. Isso causar problemas com a atualização e excluirá as invocações de método. Portanto, limpar essa propriedade completamente ou redefini-lo para o padrão, `{0}`.

Depois de concluir o assistente e corrigindo o `OldValuesParameterFormatString`, a marcação declarativa do ObjectDataSource s deve ser semelhante ao seguinte:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Para ativar os recursos de edição GridView s internos, verifique a opção Habilitar edição da marca inteligente s GridView. Isso definirá o s CommandField `ShowEditButton` propriedade para `true`, resultando na adição de um botão Editar (e botões de atualização e cancelamento para a linha que está sendo editada).


[![Configurar o GridView à edição de suporte](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Figura 10**: configurar o GridView para suporte de edição ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


Visite a página por meio de um navegador e clique em um da linha s botões de edição. O `CategoryName` e `Description` BoundFields são renderizados como caixas de texto. O `BrochurePath` TemplateField não tem um `EditItemTemplate`, portanto, ele continua mostrar seu `ItemTemplate` um link para a publicação. O `Picture` ImageField é renderizado como uma caixa de texto cuja `Text` propriedade recebe o valor da s ImageField `DataImageUrlField` de valor, nesse caso, `CategoryID`.


[![O GridView não tem uma Interface de edição para BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Figura 11**: O GridView não tem uma Interface de edição para `BrochurePath` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizando o`BrochurePath`s Interface de edição

É necessário criar uma interface de edição para o `BrochurePath` TemplateField, que permite que o usuário para qualquer um:

- Deixe o folheto categoria s como-está,
- Atualizar o folheto categoria s carregando um folheto novo, ou
- Remova completamente o folheto categoria s (no caso de que a categoria não tem mais um folheto associado).

Também precisamos atualizar o `Picture` interface de edição ImageField s, mas podemos obterá a isso na etapa 7.

No GridView s marca inteligente, clique no link Editar modelos e selecione o `BrochurePath` TemplateField s `EditItemTemplate` na lista suspensa. Adicionar um controle RadioButtonList Web para este modelo, definindo sua `ID` propriedade para `BrochureOptions` e seu `AutoPostBack` propriedade para `true`. Na janela Propriedades, clique nas elipses na `Items` propriedade, que abrirá o `ListItem` Editor de coleção. Adicione as três opções a seguir com `Value` s 1, 2 e 3, respectivamente:

- Usar folheto atual
- Remover folheto atual
- Carregar novo folheto

Definir a primeira `ListItem` s `Selected` propriedade `true`.


![Adicione três ListItems ao RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Figura 12**: Adicione três `ListItem` s para RadioButtonList


Sob o RadioButtonList, adicione um controle FileUpload chamado `BrochureUpload`. Defina suas `Visible` propriedade para `false`.


[![Adicionar um RadioButtonList e o controle FileUpload ao EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Figura 13**: adicionar um RadioButtonList e o controle FileUpload para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


Este RadioButtonList fornece três opções para o usuário. A ideia é que o controle FileUpload será exibido somente se a última opção, o folheto novo de Upload, está selecionada. Para fazer isso, crie um manipulador de eventos para o s RadioButtonList `SelectedIndexChanged` eventos e adicione o seguinte código:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Como os controles RadioButtonList e FileUpload são dentro de um modelo, precisamos escrever um pouco de código para acessar esses controles de forma programática. O `SelectedIndexChanged` manipulador de eventos é passado uma referência de RadioButtonList no `sender` parâmetro de entrada. Para obter o controle FileUpload, precisamos obter o pai de s RadioButtonList controle e use o `FindControl("controlID")` método a partir daí. Assim que tivermos uma referência a controles de RadioButtonList e FileUpload, o FileUpload controlar s `Visible` estiver definida como `true` somente se o s RadioButtonList `SelectedValue` é igual a 3, que é o `Value` para o folheto do novo Upload `ListItem`.

Com esse código funcionando, reserve um tempo para testar a interface de edição. Clique no botão Editar para uma linha. Inicialmente, a opção de folheto usar atual deve ser selecionada. Alterar o índice selecionado faz com que um postback. Se a terceira opção é selecionada, o controle FileUpload for exibido, caso contrário, ele está oculto. A Figura 14 mostra a interface de edição quando o botão Editar é clicado pela primeira vez; Figura 15 mostra a interface depois que a nova opção de folheto do Upload é selecionada.


[![Inicialmente, o uso atual folheto de que opção está selecionada](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Figura 14**: inicialmente, o uso atual folheto de opção é selecionada ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![Escolhendo o folheto do Upload nova opção exibe o controle FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Figura 15**: escolhendo o folheto do Upload nova opção exibe o controle FileUpload ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Salvando o folheto do arquivo e atualizando o`BrochurePath`coluna

Quando o botão de atualização de s GridView é clicado, seu `RowUpdating` evento é acionado. O ObjectDataSource comando de atualização de s é invocado e, em seguida, o GridView s `RowUpdated` evento é acionado. Como com o fluxo de trabalho de exclusão, é necessário criar manipuladores de eventos para os dois eventos. No `RowUpdating` manipulador de eventos, precisamos determinar qual ação será tomada com base em de `SelectedValue` da `BrochureOptions` RadioButtonList:

- Se o `SelectedValue` for 1, queremos continuar a usar o mesmo `BrochurePath` configuração. Portanto, precisamos definir o s ObjectDataSource `brochurePath` parâmetro existente `BrochurePath` valor do registro que está sendo atualizado. O s ObjectDataSource `brochurePath` parâmetro pode ser definido usando `e.NewValues["brochurePath"] = value`.
- Se o `SelectedValue` é 2 e, em seguida, queremos definir o registro s `BrochurePath` valor `NULL`. Isso pode ser feito definindo o s ObjectDataSource `brochurePath` parâmetro para `Nothing`, que resulta em um banco de dados `NULL` que está sendo usado no `UPDATE` instrução. Se houver um arquivo existente de folheto que está sendo removido, é necessário excluir o arquivo existente. No entanto, só queremos fazer isso, se a atualização for concluída sem gerar uma exceção.
- Se o `SelectedValue` for 3, em seguida, queremos garantir que o usuário tiver carregado um arquivo PDF e, em seguida, salvá-la no sistema de arquivos e atualizar o registro s `BrochurePath` valor da coluna. Além disso, se houver um arquivo existente de folheto que está sendo substituído, precisamos excluir o arquivo anterior. No entanto, só queremos fazer isso, se a atualização for concluída sem gerar uma exceção.

As etapas necessárias para ser concluído quando o s RadioButtonList `SelectedValue` é 3 são praticamente idênticos aos usados pelo s DetailsView `ItemInserting` manipulador de eventos. Esse manipulador de eventos é executado quando um novo registro de categoria é adicionado do controle DetailsView adicionamos a [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md). Portanto, cabe a nós para refatorar essa funcionalidade out em métodos separados. Especificamente, movi a funcionalidade comum em dois métodos:

- `ProcessBrochureUpload(FileUpload, out bool)` aceita como entrada uma instância de controle FileUpload e um valor de saída booliano que especifica se a operação de exclusão ou editar deve continuar ou se ele deve ser cancelado devido a algum erro de validação. Esse método retorna o caminho para o arquivo salvo ou `null` se nenhum arquivo foi salvo.
- `DeleteRememberedBrochurePath` Exclui o arquivo especificado pelo caminho em que a variável de página `deletedCategorysPdfPath` se `deletedCategorysPdfPath` não é `null`.

Segue o código para esses dois métodos. Observe a semelhança entre `ProcessBrochureUpload` e o s DetailsView `ItemInserting` manipulador de eventos do tutorial anterior. Neste tutorial ter atualizado os manipuladores de eventos DetailsView s para usar esses novos métodos. Baixe o código associado com este tutorial para ver as modificações para manipuladores de eventos DetailsView s.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

O s GridView `RowUpdating` e `RowUpdated` manipuladores de eventos usam o `ProcessBrochureUpload` e `DeleteRememberedBrochurePath` métodos, como mostra o código a seguir:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Observe como o `RowUpdating` manipulador de eventos usa uma série de instruções condicionais para executar a ação apropriada com base em de `BrochureOptions` RadioButtonList s `SelectedValue` valor da propriedade.

Com esse código em vigor, você pode editar uma categoria de e -lo usar seu folheto atual, não use nenhum folheto ou carregar um novo. Vá em frente e experimente-o. Definir pontos de interrupção a `RowUpdating` e `RowUpdated` manipuladores de eventos para obter uma ideia do fluxo de trabalho.

## <a name="step-7-uploading-a-new-picture"></a>Etapa 7: Carregar uma nova imagem

O `Picture` s ImageField renderizações de interface de edição como uma caixa de texto preenchida com o valor de seu `DataImageUrlField` propriedade. Durante o fluxo de trabalho de edição, GridView passa um parâmetro para o ObjectDataSource com o nome do parâmetro s o valor da s ImageField `DataImageUrlField` o valor inserido na caixa de texto na interface de edição de valor de propriedade e o parâmetro s. Esse comportamento é adequado quando a imagem é salva como um arquivo no sistema de arquivos e o `DataImageUrlField` contém a URL completa da imagem. Com tais circunstâncias, a interface de edição exibe a URL da imagem s na caixa de texto, o que o usuário pode alterar e salvar no banco de dados. Tudo bem, esse padrão interface permitir ao usuário carregar uma nova imagem, mas o deixamos a alterar a URL da imagem do valor atual para outro. Para este tutorial, no entanto, o padrão de s ImageField interface de edição não é suficiente porque o `Picture` dados binários que estão sendo armazenados diretamente no banco de dados e o `DataImageUrlField` propriedade mantém apenas o `CategoryID`.

Para entender melhor o que acontece em nosso tutorial, quando um usuário edita uma linha com um ImageField, considere o seguinte exemplo: um usuário edita uma linha com `CategoryID` 10, fazendo com que o `Picture` ImageField renderizar como uma caixa de texto com o valor 10. Imagine que o usuário altera o valor nesta caixa de texto para 50 e clica no botão de atualização. Ocorre um postback e GridView inicialmente cria um parâmetro chamado `CategoryID` com o valor 50. No entanto, antes do GridView envia esse parâmetro (e o `CategoryName` e `Description` parâmetros), ele adiciona os valores da `DataKeys` coleção. Portanto, ele substitui o `CategoryID` parâmetro com a linha s subjacente atual `CategoryID` valor 10. Em resumo, o s ImageField editando a interface não tem nenhum efeito sobre o fluxo de trabalho de edição para este tutorial porque os nomes os s ImageField `DataImageUrlField` propriedade e a grade s `DataKey` valor são os mesmos.

Enquanto o ImageField torna mais fácil de exibir uma imagem com base nos dados do banco de dados, podemos don t deseja fornecer uma caixa de texto na interface de edição. Em vez disso, queremos oferecer um controle FileUpload que o usuário final pode usar para alterar a imagem de s de categoria. Ao contrário de `BrochurePath` valor, para esses tutoriais, ve decidiu exigir que cada categoria deve ter uma imagem. Portanto, podemos don precisa permitir que o usuário indicar que não há nenhuma imagem associada, o usuário pode carregue uma nova imagem ou deixar a imagem atual como-está.

Para personalizar a interface de edição ImageField s, é necessário convertê-lo em um TemplateField. Da GridView s marca inteligente, clique no link Editar colunas, selecionar o ImageField e clique em converter este campo em um TemplateField link.


![Converter o ImageField em um TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Figura 16**: converter o ImageField em um TemplateField


Converter o ImageField em um TemplateField dessa maneira gera um TemplateField com dois modelos. Como mostra a seguinte sintaxe declarativa, o `ItemTemplate` contém uma imagem de Web controle cuja `ImageUrl` atribuído a propriedade usando a sintaxe de associação de dados com base em s ImageField `DataImageUrlField` e `DataImageUrlFormatString` propriedades. O `EditItemTemplate` contém uma caixa de texto cuja `Text` propriedade está associada ao valor especificado pelo `DataImageUrlField` propriedade.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Precisamos atualizar o `EditItemTemplate` para usar um controle FileUpload. De GridView de marca inteligente do s clique em Editar modelos vincular e, em seguida, selecione a `Picture` TemplateField s `EditItemTemplate` na lista suspensa. No modelo você deve ver uma caixa de texto removê-lo. Em seguida, arraste um controle FileUpload da caixa de ferramentas para a configuração do modelo, suas `ID` para `PictureUpload`. Também adicione o texto para alterar a imagem de categoria s, especifique uma nova imagem. Para manter a imagem de categoria s o mesmo, deixe o campo vazio para o modelo.


[![Adicionar um controle FileUpload ao EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Figura 17**: adicionar um controle FileUpload para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


Depois de personalizar a interface de edição, exiba seu progresso em um navegador. Ao exibir uma linha no modo somente leitura, a imagem de s de categoria é mostrada como era antes, mas clicando no botão Editar renderiza a coluna de imagem como texto com um controle FileUpload.


[![A Interface de edição inclui um controle FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Figura 18**: A Interface de edição inclui um controle FileUpload ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


Lembre-se de que o ObjectDataSource está configurado para chamar o `CategoriesBLL` classe s `UpdateCategory` método que aceita como entrada os dados binários para a imagem como um `byte` matriz. Se essa matriz tem um `null` de valor, no entanto, a alternativa `UpdateCategory` sobrecarga é chamado, quais problemas o `UPDATE` instrução SQL que não modifica o `Picture` coluna, sem afetar a categoria s atual de imagens intactas. Portanto, em s GridView `RowUpdating` manipulador de eventos, precisamos referenciar programaticamente o `PictureUpload` FileUpload controlar e determinar se um arquivo foi carregado. Se não foi carregado e, em seguida, fazemos *não* para especificar um valor para o `picture` parâmetro. Por outro lado, se um arquivo foi carregado no `PictureUpload` controle FileUpload, queremos garantir que ele seja um arquivo JPG. Se ele for, podemos enviar seu conteúdo binário para o ObjectDataSource por meio de `picture` parâmetro.

Como com o código usado na etapa 6, grande parte do código necessário aqui já existe no s DetailsView `ItemInserting` manipulador de eventos. Portanto, eu ve refatorou a funcionalidade comum em um novo método, `ValidPictureUpload`e atualizada a `ItemInserting` manipulador de eventos para usar esse método.

Adicione o seguinte código ao início da s GridView `RowUpdating` manipulador de eventos. Ele importante que esse código vêm antes do código que salva o arquivo de folheto, já que estamos não t deseja salvar o folheto do sistema de arquivos do servidor s web se um arquivo de imagem inválido for carregado.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

O `ValidPictureUpload(FileUpload)` método usa um controle FileUpload como seu único parâmetro de entrada e verifica a extensão do arquivo carregado s para garantir que o arquivo carregado é um JPG; ele é chamado apenas se um arquivo de imagem for carregado. Se nenhum arquivo for carregado, o parâmetro de imagem é não definido e, portanto, usa o valor padrão de `null`. Se uma imagem foi carregada e `ValidPictureUpload` retorna `true`, o `picture` parâmetro receberá os dados binários da imagem carregada; se o método retornar `false`, o fluxo de trabalho de atualização é cancelado, e o manipulador de eventos foi encerrado.

O `ValidPictureUpload(FileUpload)` código do método, o que foi refatorado de s DetailsView `ItemInserting` segue o manipulador de eventos:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Etapa 8: Substituir as imagens originais de categorias por JPGs

Lembre-se de que as imagens originais de oito categorias são arquivos de bitmap encapsulados em um cabeçalho OLE. Agora que adicionamos a capacidade de editar uma imagem existente do registro s, levar alguns instantes para substituir esses bitmaps com JPGs. Se você quiser continuar a usar as imagens de categoria atual, você pode convertê-las para JPGs executando as seguintes etapas:

1. Salve as imagens de bitmap em seu disco rígido. Visite o `UpdatingAndDeleting.aspx` no seu navegador e para cada uma das oito primeiras categorias de página, clique com botão direito na imagem e optar por salvar a imagem.
2. Abra a imagem em seu editor de imagem de escolha. Você pode usar o Microsoft Paint, por exemplo.
3. Salve o bitmap como uma imagem JPG.
4. Atualize a imagem de categoria s por meio da interface de edição, usando o arquivo JPG.

Depois de editar uma categoria e carregar a imagem JPG, a imagem não será renderizado no navegador porque o `DisplayCategoryPicture.aspx` os primeiros 78 bytes do que as imagens das oito primeiras categorias de remoção de página. Corrigir esse problema removendo o código que executa a remoção de cabeçalho OLE. Depois de fazer isso, o `DisplayCategoryPicture.aspx``Page_Load` manipulador de eventos deve ter apenas o código a seguir:


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> O `UpdatingAndDeleting.aspx` s de página inserindo e editando interfaces poderia usar um pouco mais trabalhoso. O `CategoryName` e `Description` BoundFields no GridView e DetailsView TemplateFields devem ser convertidas. Uma vez que `CategoryName` não permite `NULL` valores, um RequiredFieldValidator deve ser adicionado. E o `Description` TextBox provavelmente deve ser convertido em uma caixa de texto de várias linha. Vou deixar esses toques finais como um exercício para você.


## <a name="summary"></a>Resumo

Esse tutorial conclui nossa como trabalhar com dados binários. Neste tutorial e as três anteriores, vimos que dados como binários podem ser armazenados no sistema de arquivos ou diretamente no banco de dados. Um usuário fornece os dados binários no sistema selecionando um arquivo no seu disco rígido e carregá-lo no servidor web, onde podem ser armazenado no sistema de arquivos ou inserido no banco de dados. O ASP.NET 2.0 inclui um controle FileUpload que torna o fornecimento de tal interface tão fácil como arrastar e soltar. No entanto, como observado na [carregar arquivos](uploading-files-cs.md) tutorial, o controle FileUpload só é adequado para carregamentos de arquivos relativamente pequeno, o ideal é que não exceda um megabyte. Também exploramos como associar dados carregados com o modelo de dados subjacente, bem como editar e excluir os dados binários de registros existentes.

Nosso próximo conjunto de tutoriais explora várias técnicas de armazenamento em cache. O cache fornece um meio para aprimorar um aplicativo s utilizando os resultados de operações caras e armazená-los em um local que pode ser acessado mais rapidamente o desempenho geral.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md)
> [Próximo](uploading-files-vb.md)
