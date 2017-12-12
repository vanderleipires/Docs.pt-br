---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: "Atualizando e excluindo dados binários existentes (c#) | Microsoft Docs"
author: rick-anderson
description: "Nos tutoriais anteriores, vimos como o controle GridView simplifica a editar e excluir dados de texto. Neste tutorial, podemos ver como o controle GridView também fazer..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 55128faa3752a43902c17525dde3543a4a8c3997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="updating-and-deleting-existing-binary-data-c"></a>Atualizando e excluindo dados binários existentes (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) ou [baixar PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> Nos tutoriais anteriores, vimos como o controle GridView simplifica a editar e excluir dados de texto. Neste tutorial, podemos ver como o controle GridView também torna possível editar e excluir dados binários, dados binários é salvo no banco de dados ou armazenados no sistema de arquivos.


## <a name="introduction"></a>Introdução

Sobre os últimas três tutoriais é var adicionado um pouco de funcionalidade para trabalhar com dados binários. Começamos adicionando um `BrochurePath` coluna para o `Categories` de tabela e a arquitetura atualizadas. Também adicionamos métodos de camada de acesso a dados e uma camada de lógica comercial para trabalhar com a tabela s de categorias existentes `Picture` coluna que contém o s conteúdo binário de um arquivo de imagem. Criamos páginas da web para apresentar os dados binários em um GridView um link de download para a publicação, a imagem de categoria s mostrada um `<img>` elemento e adicionou um DetailsView para permitir que os usuários adicionar uma nova categoria e carregar seus dados de publicação e a imagem.

Tudo o que resta a ser implementada é a capacidade de editar e excluir as categorias existentes, podemos vai fazer neste tutorial, usando o GridView s internos de edição e exclusão de recursos. Ao editar uma categoria, o usuário será capaz de carregar uma nova imagem, opcionalmente, ou apresentam a categoria continuar a usar um existente. Para a publicação, eles ou podem optar por usar a publicação existente, para carregar uma nova publicação ou para indicar que a categoria não tem uma publicação associada a ele. Permitir que o s começar!

## <a name="step-1-updating-the-data-access-layer"></a>Etapa 1: Atualizar a camada de acesso a dados

A DAL foi gerado automaticamente `Insert`, `Update`, e `Delete` métodos, mas esses métodos foram gerados com base no `CategoriesTableAdapter` consulta principal s, que não inclui o `Picture` coluna. Portanto, o `Insert` e `Update` métodos não têm parâmetros para especificar os dados binários para a imagem de categoria s. Como foi o [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md), precisamos criar um novo método de TableAdapter para atualizar o `Categories` tabela ao especificar dados binários.

Abra o conjunto de dados tipado e, no Designer, clique duas vezes no `CategoriesTableAdapter` cabeçalho s e escolha Adicionar consulta no menu de contexto para launche o Assistente de configuração de consulta do TableAdapter. Este assistente inicia nos pedindo a como a consulta do TableAdapter deve acessar o banco de dados. Escolher as instruções SQL de uso e clique em Avançar. A próxima etapa solicitará o tipo de consulta a ser gerado. Desde que re criando uma consulta para adicionar um novo registro para o `Categories` de tabela, escolha atualizar e clique em Avançar.


[![Selecione a opção de atualização](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Figura 1**: selecione a opção de atualização ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


Agora, é necessário especificar o `UPDATE` instrução SQL. O assistente sugere automaticamente um `UPDATE` instrução corresponde à consulta principal do TableAdapter s (que atualiza o `CategoryName`, `Description`, e `BrochurePath` valores). Altere a instrução para que o `Picture` coluna está incluída junto com um `@Picture` parâmetro, da seguinte forma:


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

A última tela do assistente nos peça para nomear o novo método TableAdapter. Digite `UpdateWithPicture` e clique em Concluir.


[![Nome do novo UpdateWithPicture método TableAdapter](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Figura 2**: nomear o novo método de TableAdapter `UpdateWithPicture` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Etapa 2: Adicionando os métodos de camada de lógica de negócios

Além de atualizar a DAL, precisamos atualizar BLL para incluir os métodos para atualizar e excluir uma categoria. Esses são os métodos que serão chamados da camada de apresentação.

Para excluir uma categoria, podemos usar o `CategoriesTableAdapter` s gerado automaticamente `Delete` método. Adicione o seguinte método para o `CategoriesBLL` classe:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Para este tutorial, vamos s criar dois métodos para atualizar uma categoria - que espera que os dados de imagem binária e invoca o `UpdateWithPicture` método adicionamos apenas para o `CategoriesTableAdapter` e outra que aceita apenas o `CategoryName`, `Description`e `BrochurePath`valores e usa `CategoriesTableAdapter` classe s gerado automaticamente `Update` instrução. A lógica por trás usando dois métodos é que, em algumas circunstâncias, um usuário poderá atualizar a imagem de s categoria junto com seus outros campos, nesse caso, o usuário precisará carregar a nova imagem. Os dados binários da imagem carregada s, em seguida, podem ser usados no `UPDATE` instrução. Em outros casos, o usuário só pode estar interessado na atualização, digamos, o nome e descrição. Mas se o `UPDATE` instrução espera que os dados binários para o `Picture` coluna, podemos d precisarão fornecer essas informações também. Isso requer uma viagem extra para o banco de dados para trazer de volta os dados da imagem para o registro que está sendo editado. Portanto, queremos que dois `UPDATE` métodos. A camada de lógica de negócios determinará qual delas usar com base em se os dados da imagem são fornecidos ao atualizar a categoria.

Para facilitar isso, adicione dois métodos para o `CategoriesBLL` classe nomeadas `UpdateCategory`. O primeiro deles deve aceitar três `string` s, uma `byte` matriz e um `int` como sua entrada parâmetros; a segunda, apenas três `string` s e um `int`. O `string` são parâmetros de entrada para o nome da categoria s, descrição e caminho do arquivo de publicação, o `byte` matriz é para o conteúdo binário de imagem categoria s e o `int` identifica o `CategoryID` do registro para atualizar. Observe que a primeira sobrecarga invoca o segundo se passado `byte` matriz é `null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Etapa 3: Copiando sobre a funcionalidade de exibição e inserção

No [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md) criamos uma página chamada `UploadInDetailsView.aspx` que lista todas as categorias de um controle GridView e fornecido um DetailsView para adicionar novas categorias para o sistema. Neste tutorial, estamos estenderá GridView para incluir a edição e exclusão de suporte. Em vez de continuar a trabalhar de `UploadInDetailsView.aspx`, permitem s em vez disso, colocar alterações neste tutorial s no `UpdatingAndDeleting.aspx` página da mesma pasta, `~/BinaryData`. Copie e cole a marcação declarativa e código de `UploadInDetailsView.aspx` para `UpdatingAndDeleting.aspx`.

Comece abrindo o `UploadInDetailsView.aspx` página. Copie toda a sintaxe declarativa dentro de `<asp:Content>` elemento, conforme mostrado na Figura 3. Em seguida, abra `UpdatingAndDeleting.aspx` e cole essa marcação dentro de seu `<asp:Content>` elemento. Da mesma forma, copie o código do `UploadInDetailsView.aspx` página classe code-behind de s para `UpdatingAndDeleting.aspx`.


[![Copiar a marcação declarativa de UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Figura 3**: copiar a marcação declarativa de `UploadInDetailsView.aspx` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


Depois de copiar as declarativa marcação e código, visite `UpdatingAndDeleting.aspx`. Você verá a mesma saída e têm a mesma experiência de usuário como com `UploadInDetailsView.aspx` página do tutorial anterior.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Etapa 4: Adicionando, excluindo o suporte para o ObjectDataSource e GridView

Conforme abordado no [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView fornece recursos internos de exclusão e esses recursos podem ser habilitados em escala de uma caixa de seleção se a grade s subjacente fonte de dados oferecer suporte a exclusão. Atualmente o ObjectDataSource GridView está associada a (`CategoriesDataSource`) não oferece suporte a exclusão.

Para corrigir isso, clique na opção de configurar a fonte de dados de marca inteligente ObjectDataSource s para iniciar o assistente. A primeira tela mostra que o ObjectDataSource está configurado para trabalhar com o `CategoriesBLL` classe. Pressione próximo. Atualmente, apenas o ObjectDataSource s `InsertMethod` e `SelectMethod` propriedades são especificadas. No entanto, o assistente preenchido automaticamente as listas suspensas nas guias de UPDATE e DELETE com a `UpdateCategory` e `DeleteCategory` métodos, respectivamente. Isso ocorre porque no `CategoriesBLL` classe é marcada como esses métodos usando o `DataObjectMethodAttribute` como os métodos padrão de atualização e exclusão.

Por enquanto, definir a lista de lista suspensa atualização guia s como (nenhum), mas deixe a lista suspensa de guia s de exclusão definida como `DeleteCategory`. Retornaremos a esse assistente na etapa 6 para adicionar suporte a atualização.


[![Configurar o ObjectDataSource para usar o método DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Figura 4**: configurar o ObjectDataSource para usar o `DeleteCategory` método ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> Após concluir o assistente, o Visual Studio pode solicitar se você quiser atualizar campos e chaves, que irá regenerar os dados da Web controles campos. Selecione não, porque se você escolher Sim substituirá as personalizações de campo que tenha feito.


ObjectDataSource agora inclui um valor para o seu `DeleteMethod` propriedade, bem como a `DeleteParameter`. Lembre-se de que, ao usar o Assistente para especificar os métodos, o Visual Studio define o s ObjectDataSource `OldValuesParameterFormatString` propriedade `original_{0}`, que causa problemas com a atualização e excluir invocações de método. Portanto, limpar esta propriedade completamente ou redefini-lo para o padrão, `{0}`. Se você precisar atualizar sua memória nesta propriedade ObjectDataSource, consulte o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial.

Depois de concluir o assistente e corrigir o `OldValuesParameterFormatString`, a marcação declarativa de s ObjectDataSource deve ter aparência semelhante à seguinte:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Depois de configurar o ObjectDataSource, adicione recursos excluindo GridView, marcando a caixa de seleção Habilitar a exclusão de marca inteligente s GridView. Isso adicionará uma CommandField GridView cujo `ShowDeleteButton` está definida como `true`.


[![Habilitar o suporte para exclusão em GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Figura 5**: habilitar o suporte para exclusão em GridView ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


Reserve um momento para testar a funcionalidade de exclusão. Há uma chave estrangeira entre a `Products` tabela s `CategoryID` e `Categories` tabela s `CategoryID`, portanto, você obterá uma exceção de violação de restrição de chave estrangeira se você tentar excluir qualquer uma das primeiras oito categorias. Para testar essa funcionalidade out, adicione uma nova categoria, fornecendo uma publicação e a imagem. Categoria meu teste, mostrada na Figura 6, inclui um arquivo de publicação de teste chamado `Test.pdf` e uma imagem de teste. A Figura 7 mostra GridView, após a categoria de teste foi adicionada.


[![Adicionar uma categoria de teste com uma publicação e imagem](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Figura 6**: adicionar uma categoria de teste com uma publicação e a imagem ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![Depois de inserir a categoria de teste, ele será exibido em GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Figura 7**: depois de inserir a categoria de teste, ele será exibido em GridView ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


No Visual Studio, atualize o Gerenciador de soluções. Agora, você verá um novo arquivo no `~/Brochures` pasta `Test.pdf` (consulte a Figura 8).

Em seguida, clique no link excluir na linha de categoria de teste, fazendo com que a página de postback e `CategoriesBLL` classe s `DeleteCategory` método seja acionado. Isso chamará o s DAL `Delete` método, causando apropriada `DELETE` instrução a ser enviada para o banco de dados. Os dados, em seguida, é vinculada outra vez a GridView e a marcação é enviada de volta ao cliente com a categoria de teste não está mais presente.

Enquanto o fluxo de trabalho de exclusão removido com êxito o registro da categoria de teste do `Categories` tabela, não removeu o arquivo de publicação do sistema de arquivo do web server s. Atualize o Gerenciador de soluções e você verá que `Test.pdf` ainda está colocada no `~/Brochures` pasta.


![O arquivo Test.pdf não foi excluído do sistema de arquivo do Web Server s](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Figura 8**: O `Test.pdf` arquivo não foi excluído do sistema de arquivo do Web Server s


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Etapa 5: Remover o arquivo de publicação % s excluído categoria

Uma das desvantagens do armazenamento de dados binários externos para o banco de dados é que as etapas adicionais devem ser executadas para limpar esses arquivos quando o registro do banco de dados associado será excluído. O GridView e ObjectDataSource fornecem eventos que acionam antes e depois que o comando de exclusão foi executado. Na verdade, precisamos criar manipuladores de eventos para os eventos de pré e pós-ação. Antes do `Categories` registro é excluído, precisamos determinar seu caminho de arquivo s PDF, mas podemos don t deseja excluir o PDF antes da categoria é excluída caso há uma exceção e a categoria não é excluída.

O GridView s [ `RowDeleting` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) é acionado antes do comando de exclusão ObjectDataSource s foi chamado, enquanto seu [ `RowDeleted` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) é acionado depois. Crie manipuladores de eventos para esses dois eventos usando o seguinte código:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

No `RowDeleting` manipulador de eventos, o `CategoryID` da linha que está sendo excluído é capturado de GridView s `DataKeys` coleção, que pode ser acessada deste manipulador de eventos por meio de `e.Keys` coleção. Em seguida, o `CategoriesBLL` classe s `GetCategoryByCategoryID(categoryID)` é chamado para retornar informações sobre o registro que está sendo excluído. Se retornado `CategoriesDataRow` objeto tem uma não -`NULL``BrochurePath` valor, em seguida, ele é armazenado na variável de página `deletedCategorysPdfPath` para que o arquivo pode ser excluído a `RowDeleted` manipulador de eventos.

> [!NOTE]
> Em vez de recuperar o `BrochurePath` detalhes para o `Categories` registro sendo excluído o `RowDeleting` manipulador de eventos, pode também adicionamos a `BrochurePath` no s GridView `DataKeyNames` propriedade e acessar o valor do registro s por meio de `e.Keys` coleção. Isso seria ligeiramente aumentar o tamanho de estado de exibição do GridView s, mas seria reduzir a quantidade de código necessária e salvar uma viagem ao banco de dados.


Após o ObjectDataSource comando de exclusão s subjacente foi chamado, o s GridView `RowDeleted` manipulador de evento é acionado. Se não houve nenhuma exceção ao excluir os dados e não há um valor para `deletedCategorysPdfPath`, em seguida, o PDF é excluído do sistema de arquivos. Observe que esse código extra não será necessário para limpar os dados binários de categoria s associados sua imagem. Que s porque os dados de imagem são armazenados diretamente no banco de dados, excluindo assim o `Categories` linha também exclui esses dados de imagem de categoria s.

Depois de adicionar os manipuladores de eventos de dois, execute novamente este caso de teste. Ao excluir a categoria, o PDF associado também é excluído.

Atualizar dados binários existentes s registro associado fornece alguns desafios interessantes. O restante deste tutorial investiga adicionando recursos de atualização para a publicação e a imagem. Etapa 6 explora as técnicas para atualizar as informações de publicação durante a etapa 7 olha para atualizar a imagem.

## <a name="step-6-updating-a-category-s-brochure"></a>Etapa 6: Atualizando uma publicação de s categoria

Como discutido o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView oferece suporte interno da edição de nível de linha que pode ser implementado através de escala de uma caixa de seleção se a fonte de dados subjacente é configurado adequadamente. Atualmente, o `CategoriesDataSource` ObjectDataSource ainda não está configurado para incluir suporte, a atualização para permitem s adicioná-la no.

Clique no link configurar fonte de dados do assistente ObjectDataSource s e prosseguir para a segunda etapa. Devido a `DataObjectMethodAttribute` usados em `CategoriesBLL`, a lista suspensa de atualização deve ser preenchida automaticamente com o `UpdateCategory` sobrecarga que aceita quatro parâmetros de entrada (para todas as colunas, mas `Picture`). Altere isso para que ele usa a sobrecarga com cinco parâmetros.


[![Configurar o ObjectDataSource para usar o método UpdateCategory não inclui um parâmetro de imagem](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Figura 9**: configurar o ObjectDataSource para usar o `UpdateCategory` método que inclui um parâmetro para `Picture` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


ObjectDataSource agora inclui um valor para o seu `UpdateMethod` propriedade, bem como correspondente `UpdateParameter` s. Conforme observado na etapa 4, o Visual Studio define o s ObjectDataSource `OldValuesParameterFormatString` propriedade `original_{0}` ao usar o Assistente Configurar fonte de dados. Isso irá causar problemas com a atualização e excluir invocações de método. Portanto, limpar esta propriedade completamente ou redefini-lo para o padrão, `{0}`.

Depois de concluir o assistente e corrigir o `OldValuesParameterFormatString`, a marcação declarativa de s ObjectDataSource deve parecer com o seguinte:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Para ativar os recursos de edição GridView s internos, marque a opção de habilitar edição de marca inteligente s GridView. Isso definirá o s CommandField `ShowEditButton` propriedade `true`, resultando na adição de um botão de edição (e botões de cancelamento e de atualização para a linha que está sendo editada).


[![Configurar o GridView à edição de suporte](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Figura 10**: configurar o GridView à edição de suporte ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


Visite a página por meio de um navegador e clique em uma linha s botões de edição. O `CategoryName` e `Description` BoundFields são renderizados como caixas de texto. O `BrochurePath` TemplateField não tem um `EditItemTemplate`, portanto, ele continua mostrar seu `ItemTemplate` um link para a publicação. O `Picture` ImageField é renderizada como uma caixa de texto cuja `Text` é atribuído o valor da s ImageField propriedade `DataImageUrlField` valor, nesse caso `CategoryID`.


[![GridView não tem uma Interface de edição para BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Figura 11**: GridView não tem uma Interface de edição para `BrochurePath` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizando o`BrochurePath`s edição Interface

É preciso criar uma interface de edição para o `BrochurePath` TemplateField, que permite que o usuário seja:

- Deixe a publicação de categoria s como-é,
- Atualizar a publicação de categoria s carregando uma nova publicação, ou
- Remova completamente a publicação de categoria s (no caso de que a categoria não tem mais uma publicação associada).

Também precisamos atualizar o `Picture` interface de edição ImageField s, mas estamos obterá a isso na etapa 7.

De GridView s marca inteligente, clique no link Editar modelos e selecione o `BrochurePath` TemplateField s `EditItemTemplate` na lista suspensa. Adicionar um controle RadioButtonList Web nesse modelo, definindo seu `ID` propriedade `BrochureOptions` e seu `AutoPostBack` propriedade `true`. Na janela Propriedades, clique nas elipses no `Items` propriedade, o que abrirá o `ListItem` Editor de coleção. Adicione as três opções a seguir com `Value` s 1, 2 e 3, respectivamente:

- Usar a publicação atual
- Remover a publicação atual
- Carregar a nova publicação

Definir a primeira `ListItem` s `Selected` propriedade `true`.


![Adicionar três ListItems a RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Figura 12**: adicionar três `ListItem` s para RadioButtonList


Sob o RadioButtonList, adicione um controle de carregamento de arquivos chamado `BrochureUpload`. Definir seu `Visible` propriedade `false`.


[![Adicionar um RadioButtonList e controle de carregamento de arquivos para o EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Figura 13**: adicionar um RadioButtonList e controle de carregamento de arquivos para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


Este RadioButtonList fornece as três opções para o usuário. A ideia é que o controle de carregamento de arquivos será exibido somente se a última opção, a publicação de novos de carregamento, é selecionada. Para fazer isso, crie um manipulador de eventos para o s RadioButtonList `SelectedIndexChanged` evento e adicione o seguinte código:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Como os controles RadioButtonList e carregamento de arquivos dentro de um modelo, é necessário escrever um pouco de código para acessar esses controles de forma programática. O `SelectedIndexChanged` manipulador de eventos é passado uma referência de RadioButtonList no `sender` parâmetro de entrada. Para obter o controle de carregamento de arquivos, precisamos obter pai s RadioButtonList controle e use o `FindControl("controlID")` método de lá. Assim que tivermos uma referência a controles RadioButtonList tanto o carregamento de arquivos, o carregamento de arquivos controlam s `Visible` está definida como `true` somente se o s RadioButtonList `SelectedValue` é igual a 3, que é o `Value` para a publicação de novos de carregamento `ListItem`.

Com esse código, dedique alguns momentos para testar a interface de edição. Clique no botão Editar para uma linha. Inicialmente, o uso da opção de publicação atual deve ser selecionado. Alterar o índice selecionado causa um postback. Se a terceira opção é selecionada, o controle de carregamento de arquivos for exibido, caso contrário, ele está oculto. A Figura 14 mostra a interface de edição quando o botão de edição é clicado pela primeira vez; Figura 15 mostra a interface depois que a nova opção de publicação do carregamento é selecionada.


[![Inicialmente, a publicação atual Use que está selecionada](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Figura 14**: inicialmente, a publicação atual Use opção é selecionada ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![Escolhendo a carregamento nova publicação opção exibe o controle de carregamento de arquivos](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Figura 15**: escolhendo a publicação de carregamento nova opção exibe o controle de carregamento de arquivos ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Salvar a publicação de arquivo e atualizando o`BrochurePath`coluna

Quando o botão de atualização de s GridView é clicado, seu `RowUpdating` evento ser acionado. O comando de atualização de s é chamado de ObjectDataSource e, em seguida, o s GridView `RowUpdated` evento ser acionado. Como o fluxo de trabalho, excluir, precisamos criar manipuladores de eventos para os dois eventos. No `RowUpdating` manipulador de eventos, é necessário determinar qual ação a ser tomada com base no `SelectedValue` do `BrochureOptions` RadioButtonList:

- Se o `SelectedValue` é 1, queremos continuar usando o mesmo `BrochurePath` configuração. Portanto, precisamos definir a s ObjectDataSource `brochurePath` parâmetro existente `BrochurePath` valor do registro que está sendo atualizado. O s ObjectDataSource `brochurePath` parâmetro pode ser definido usando `e.NewValues["brochurePath"] = value`.
- Se o `SelectedValue` for 2, queremos definir o registro s `BrochurePath` valor `NULL`. Isso pode ser feito definindo o s ObjectDataSource `brochurePath` parâmetro `Nothing`, que resulta em um banco de dados `NULL` que está sendo usado no `UPDATE` instrução. Se houver um arquivo de publicação existente que está sendo removido, precisamos excluir o arquivo existente. No entanto, só queremos fazer isso se a atualização for concluída sem gerar uma exceção.
- Se o `SelectedValue` é 3, em seguida, queremos garantir que o usuário tiver carregado um arquivo PDF e, em seguida, salvá-lo para o sistema de arquivos e atualizar o registro s `BrochurePath` valor da coluna. Além disso, se houver um arquivo de publicação existente que está sendo substituído, é necessário excluir o arquivo anterior. No entanto, só queremos fazer isso se a atualização for concluída sem gerar uma exceção.

As etapas necessárias para ser concluído quando o s RadioButtonList `SelectedValue` é 3 são praticamente idênticos aos usados pelo s DetailsView `ItemInserting` manipulador de eventos. Este manipulador de eventos é executado quando um novo registro de categoria é adicionado do controle DetailsView adicionamos a [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md). Portanto, cabe a refatorar essa funcionalidade out em métodos separados. Especificamente, eu movido a funcionalidade comum em dois métodos:

- `ProcessBrochureUpload(FileUpload, out bool)`aceita como entrada uma instância de controle de carregamento de arquivos e um valor booliano de saída que especifica se a operação de exclusão ou editar deve continuar ou se ele deve ser cancelado devido a algum erro de validação. Esse método retorna o caminho para o arquivo salvo ou `null` se nenhum arquivo foi salvo.
- `DeleteRememberedBrochurePath`Exclui o arquivo especificado pelo caminho na variável de página `deletedCategorysPdfPath` se `deletedCategorysPdfPath` não é `null`.

O código para esses dois métodos a segue. Observe a semelhança entre `ProcessBrochureUpload` e s a DetailsView `ItemInserting` manipulador de eventos do tutorial anterior. Neste tutorial, atualizou os manipuladores de eventos DetailsView s para usar esses métodos de novo. Baixe o código associado com este tutorial para ver as modificações aos manipuladores de eventos de s DetailsView.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

O GridView s `RowUpdating` e `RowUpdated` manipuladores de eventos usam o `ProcessBrochureUpload` e `DeleteRememberedBrochurePath` métodos, como mostra o seguinte código:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Observe como o `RowUpdating` manipulador de eventos usa uma série de instruções condicionais para executar a ação apropriada com base no `BrochureOptions` RadioButtonList s `SelectedValue` valor da propriedade.

Com esse código, você pode editar uma categoria e tiver de usar sua publicação atual, não use nenhuma publicação ou carregar um novo. Vá em frente e experimente-o. Definir pontos de interrupção de `RowUpdating` e `RowUpdated` manipuladores de eventos para obter uma ideia do fluxo de trabalho.

## <a name="step-7-uploading-a-new-picture"></a>Etapa 7: Carregar uma nova imagem

O `Picture` s ImageField edição interface renderizada como uma caixa de texto preenchida com o valor de seu `DataImageUrlField` propriedade. Durante o fluxo de trabalho de edição, GridView passa um parâmetro para ObjectDataSource com o nome do parâmetro s o valor da s ImageField `DataImageUrlField` o valor digitado na caixa de texto na interface de edição de valor de propriedade e o parâmetro s. Esse comportamento é adequado quando a imagem é salva como um arquivo no sistema de arquivos e o `DataImageUrlField` contém a URL completa da imagem. Com essas circunstâncias, a interface de edição exibe a URL da imagem s na caixa de texto, que o usuário pode alterar e salvar no banco de dados. Concedidas, esse padrão interface permitir ao usuário carregar uma nova imagem, mas ele deixá-los a alterar a URL da imagem do valor atual para outro. Para este tutorial, no entanto, o padrão de s ImageField edição de interface não é suficiente porque a `Picture` dados binários estão sendo armazenados diretamente no banco de dados e o `DataImageUrlField` propriedade mantém apenas o `CategoryID`.

Para entender melhor o que acontece em nosso tutorial quando um usuário edita uma linha com um ImageField, considere o seguinte exemplo: um usuário edita uma linha com `CategoryID` 10, fazendo com que o `Picture` ImageField renderizar como uma caixa de texto com o valor 10. Imagine que o usuário altera o valor nesta caixa de texto para 50 e clica no botão de atualização. Ocorre um postback e GridView inicialmente cria um parâmetro chamado `CategoryID` com o valor 50. No entanto, antes de GridView envia esse parâmetro (e o `CategoryName` e `Description` parâmetros), ele adiciona os valores da `DataKeys` coleção. Portanto, ele substitui o `CategoryID` parâmetro com o atual linha s subjacente `CategoryID` valor 10. Em resumo, a s ImageField edição interface não tem nenhum efeito no fluxo de trabalho edição para este tutorial porque os nomes da s ImageField `DataImageUrlField` propriedade e a grade s `DataKey` valor são os mesmos.

Enquanto o ImageField torna mais fácil exibir uma imagem com base nos dados do banco de dados, podemos don t deseja fornecer uma caixa de texto na interface de edição. Em vez disso, queremos oferecem um controle de carregamento de arquivos que o usuário final pode usar para alterar a imagem de s categoria. Diferentemente de `BrochurePath` valor, para esses tutoriais, var decidiu requer que cada categoria deve ter uma imagem. Portanto, podemos don precisa permitir que o usuário indicar que não há nenhuma imagem associada, o usuário pode carregue uma nova imagem ou deixar a imagem atual como-é.

Para personalizar a interface de edição de ImageField s, é necessário convertê-la em um TemplateField. De GridView s marca inteligente, clique no link Editar colunas, selecione o ImageField e clique em converter este campo em um TemplateField link.


![Converter o ImageField em um TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Figura 16**: converter o ImageField em um TemplateField


Converter o ImageField em um TemplateField dessa maneira gera um TemplateField com dois modelos. Como mostra a seguinte sintaxe declarativa, o `ItemTemplate` contém uma imagem de Web controle cuja `ImageUrl` atribuído a propriedade usando a sintaxe de associação de dados com base em s ImageField `DataImageUrlField` e `DataImageUrlFormatString` propriedades. O `EditItemTemplate` contém uma caixa de texto cuja `Text` propriedade está associada ao valor especificado pelo `DataImageUrlField` propriedade.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Precisamos atualizar o `EditItemTemplate` para usar um controle de carregamento de arquivos. De GridView marca inteligente s clique em Editar modelos vincular e, em seguida, selecione o `Picture` TemplateField s `EditItemTemplate` na lista suspensa. No modelo você verá uma caixa de texto removê-lo. Em seguida, arraste um controle de carregamento de arquivos da caixa de ferramentas para a configuração do modelo, sua `ID` para `PictureUpload`. Além disso, adicione o texto para alterar a imagem de categoria s, especifique uma nova imagem. Para manter a mesma imagem categoria s, deixe o campo vazio no modelo.


[![Adicionar um controle de carregamento de arquivos para o EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Figura 17**: adicionar um controle de carregamento de arquivos para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


Depois de personalizar a interface de edição, exiba seu andamento em um navegador. Ao exibir uma linha no modo somente leitura, a imagem de s categoria é exibida como antes, mas a coluna de imagem clicando no botão de edição é renderizado como texto com um controle de carregamento de arquivos.


[![A Interface de edição inclui um controle de carregamento de arquivos](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Figura 18**: A Interface de edição inclui um controle de carregamento de arquivos ([clique para exibir a imagem em tamanho normal](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


Lembre-se de que o ObjectDataSource está configurado para chamar o `CategoriesBLL` classe s `UpdateCategory` método que aceita como entrada os dados binários para a imagem como um `byte` matriz. Se essa matriz tem um `null` valor, no entanto, a alternativa `UpdateCategory` sobrecarga é chamado, quais problemas o `UPDATE` instrução SQL que não modifica o `Picture` coluna, deixando a categoria s atual assim imagem intacta. Portanto, em GridView s `RowUpdating` manipulador de eventos, precisamos referenciar programaticamente o `PictureUpload` FileUpload controlar e determinar se um arquivo foi carregado. Se não foi carregado e, em seguida, fazemos *não* para especificar um valor para o `picture` parâmetro. Por outro lado, se um arquivo foi carregado no `PictureUpload` controle FileUpload, queremos garantir que ele seja um arquivo JPG. Se ele for, podemos enviar seu conteúdo binário para ObjectDataSource por meio de `picture` parâmetro.

Como com o código usado na etapa 6, grande parte do código necessário aqui já existe em DetailsView s `ItemInserting` manipulador de eventos. Portanto, não ve refatorado a funcionalidade comum em um novo método, `ValidPictureUpload`e atualizada a `ItemInserting` manipulador de eventos para usar esse método.

Adicione o seguinte código para o início da s GridView `RowUpdating` manipulador de eventos. Ele importante que esse código vir antes do código que salva o arquivo de publicação, já que estamos não t deseja salvar a publicação para o sistema de arquivos do web server s se um arquivo de imagem inválido for carregado.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

O `ValidPictureUpload(FileUpload)` método usa um controle de carregamento de arquivos como seu único parâmetro de entrada e verifica a extensão do arquivo carregado s para garantir que o arquivo carregado é JPG; ele é chamado apenas se um arquivo de imagem for carregado. Se nenhum arquivo é carregado, o parâmetro de imagem é não definido e, portanto, usa o valor padrão de `null`. Se uma imagem foi carregada e `ValidPictureUpload` retorna `true`, o `picture` parâmetro recebe os dados binários da imagem carregada; se o método retornar `false`, o fluxo de trabalho de atualização é cancelado, e o manipulador de eventos foi encerrado.

O `ValidPictureUpload(FileUpload)` código do método, que foi refatorado de DetailsView s `ItemInserting` manipulador de eventos segue:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Etapa 8: Substituir as imagens originais de categorias por JPGs

Lembre-se de que as imagens originais de oito categorias são arquivos de bitmap encapsulados em um cabeçalho OLE. Agora que adicionamos a capacidade de editar uma imagem de registro s existente, responda para substituir esses bitmaps JPGs. Se você quiser continuar a usar as imagens de categoria atual, você pode convertê-los para JPGs executando as etapas a seguir:

1. Salve as imagens de bitmap em seu disco rígido. Visite o `UpdatingAndDeleting.aspx` página em seu navegador e para cada um dos primeiros oito categorias, clique na imagem e optar por salvar a imagem.
2. Abra a imagem em seu editor de imagem de escolha. Você pode usar o Microsoft Paint, por exemplo.
3. Salve a imagem como uma imagem JPG.
4. Atualize a imagem de categoria s através da interface de edição, usando o arquivo JPG.

Depois de editar uma categoria e carregar a imagem JPG, a imagem não será renderizado no navegador porque o `DisplayCategoryPicture.aspx` página está retirando os primeiros 78 bytes de imagens das oito primeiras categorias. Corrigir esse problema removendo o código que executa a remoção de cabeçalho OLE. Depois de fazer isso, o `DisplayCategoryPicture.aspx``Page_Load` manipulador de eventos deve ter apenas o código a seguir:


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> O `UpdatingAndDeleting.aspx` s de página Inserir e editar interfaces poderia usar um pouco mais trabalho. O `CategoryName` e `Description` BoundFields no GridView e DetailsView TemplateFields devem ser convertidas. Como `CategoryName` não permite `NULL` valores, um RequiredFieldValidator deve ser adicionado. E o `Description` TextBox provavelmente deve ser convertido em uma caixa de texto de várias linha. Deixe esses toques como um exercício para você.


## <a name="summary"></a>Resumo

Este tutorial conclui nossa aparência como trabalhar com dados binários. Este tutorial e as três anteriores, vimos que os dados binários como podem ser armazenados no sistema de arquivos ou diretamente no banco de dados. Um usuário fornece os dados binários para o sistema selecionando um arquivo no seu disco rígido e carregá-los para o servidor web, onde ele pode ser armazenado no sistema de arquivos ou inserido no banco de dados. ASP.NET 2.0 inclui um controle de carregamento de arquivos que permite fornecer tal interface tão fácil quanto arrastar e soltar. No entanto, conforme indicado no [carregando arquivos](uploading-files-cs.md) tutorial, o controle de carregamento de arquivos só é adequado para carregamentos de arquivo relativamente pequeno, idealmente, não exceda um megabyte. Podemos também explorou como associar dados carregados com o modelo de dados subjacente, bem como editar e excluir os dados binários de registros existentes.

Nosso próximo conjunto de tutoriais explora várias técnicas de cache. Cache fornece uma forma de melhorar um aplicativo s desempenho geral por obter os resultados de operações caras e armazená-los em um local que pode ser acessado mais rapidamente.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md)
[Próximo](uploading-files-vb.md)
