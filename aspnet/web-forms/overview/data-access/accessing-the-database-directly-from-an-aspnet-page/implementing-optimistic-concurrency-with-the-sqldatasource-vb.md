---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: "Implementando a concorrência otimista com SqlDataSource (VB) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, examine os conceitos básicos de controle de simultaneidade otimista e, em seguida, explore como implementá-la usando o controle SqlDataSource."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 974ea50a0d12aae09107470815214b20068ea553
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementando a concorrência otimista com SqlDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) ou [baixar PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> Neste tutorial, examine os conceitos básicos de controle de simultaneidade otimista e, em seguida, explore como implementá-la usando o controle SqlDataSource.


## <a name="introduction"></a>Introdução

No tutorial anterior, examinamos como adicionar inserindo, atualizando e excluindo recursos para o controle SqlDataSource. Resumindo, para fornecer esses recursos, precisávamos especificar correspondente `INSERT`, `UPDATE`, ou `DELETE` a instrução SQL no controle s `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` propriedades, junto com apropriada parâmetros de `InsertParameters`, `UpdateParameters`, e `DeleteParameters` coleções. Apesar dessas propriedades e coleções podem ser especificadas manualmente, o botão Avançado do assistente s de configurar fonte de dados oferece um gerar `INSERT`, `UPDATE`, e `DELETE` caixa de seleção de instruções que irá criar automaticamente essas instruções se baseia a `SELECT` instrução.

Junto com a gerar `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção, a caixa de diálogo Advanced SQL Generation Options inclui uma opção de simultaneidade otimista de uso (consulte a Figura 1). Quando marcada, o `WHERE` cláusulas gerada automaticamente `UPDATE` e `DELETE` instruções são modificadas para executar apenas a atualização ou exclusão se o banco de dados dados tenha sido t subjacente foi modificada desde que o usuário carregada pela última vez os dados na grade.


![Você pode adicionar suporte a simultaneidade otimista de avançada caixa de diálogo de opções de geração de SQL](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Figura 1**: você pode adicionar suporte a simultaneidade otimista de avançada caixa de diálogo de opções de geração de SQL


Volta o [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial examinamos os fundamentos do controle de simultaneidade otimista e como adicioná-lo a ObjectDataSource. Este tutorial, vamos Retocar sobre os conceitos básicos de controle de simultaneidade otimista e, em seguida, explore como implementá-la usando o SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Uma recapitulação de simultaneidade otimista

Para aplicativos web que permitem que vários usuários simultâneos para editar ou excluir os mesmos dados, existe a possibilidade de que um usuário pode substituir acidentalmente outro alterações s. No [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial fornecido o exemplo a seguir:

Imagine que dois usuários, Jisun e Sam, foram ambas de visita uma página em um aplicativo que permitido visitantes atualizar e excluir produtos por meio de um controle GridView. Ambos clique no botão Editar para Chai ao mesmo tempo. Jisun altera o nome do produto para Chá Chai e clica no botão de atualização. O resultado é um `UPDATE` instrução é enviada para o banco de dados, que define *todos os* dos campos atualizáveis produto s (embora Jisun atualizado apenas um campo, `ProductName`). Neste momento, o banco de dados tem os valores Chai chá, a categoria de bebidas, líquidos exóticas fornecedor, e assim por diante para este produto específico. No entanto, o GridView na tela do Sam s ainda mostrará o nome do produto na linha GridView editável como Chai. Alguns segundos depois que as alterações de s Jisun foram confirmadas, Sam atualiza a categoria para Condimentos e clica em Atualizar. Isso resulta em uma `UPDATE` instrução enviada para o banco de dados que define o nome do produto para Chai, o `CategoryID` para a ID da categoria Condimentos correspondente e assim por diante. Alterações de s Jisun para o nome do produto foram substituídas.

A Figura 2 ilustra essa interação.


[![Quando dois usuários simultaneamente atualizar um registro existe potencial s para um usuário s muda para substituir os outros s](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Figura 2**: alterações de um registro existe s potencial para um usuário s quando dois usuários simultaneamente a atualização para substituir os outros s ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Para impedir que esse cenário abrindo uma forma de [controle de simultaneidade](http://en.wikipedia.org/wiki/Concurrency_control) devem ser implementados. [Simultaneidade otimista](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) o foco deste tutorial funciona na suposição de que embora possa haver conflitos de simultaneidade ocasionalmente, a maioria do tempo esses conflitos ganha t surgem. Portanto, se ocorrer um conflito, controle de simultaneidade otimista simplesmente informa ao usuário que suas alterações pode ser salva porque outro usuário modificou os mesmos dados.

> [!NOTE]
> Para aplicativos em que supõe-se que haja muitos conflitos de simultaneidade ou se esses conflitos não são toleráveis, em seguida, controle de simultaneidade pessimista pode ser usado. Voltar para o [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial para uma discussão mais completa sobre o controle de simultaneidade pessimista.


Controle de simultaneidade otimista funciona, garantindo que o registro que está sendo atualizada ou excluída tem os mesmos valores de quando a atualização ou exclusão de processo iniciado. Por exemplo, ao clicar no botão de edição em um GridView editável, os valores de registro s são de leitura do banco de dados e exibidos em caixas de texto e outros controles da Web. Esses valores originais são salvos por GridView. Posteriormente, depois que o usuário faz alterações e clica no botão de atualização, o `UPDATE` instrução usada deve levar em conta os valores originais e os novos valores e apenas atualizar o registro do banco de dados subjacente se original valores que o usuário iniciou a edição são idênticos aos valores ainda no banco de dados. A Figura 3 mostra esta sequência de eventos.


[![Para a atualização ou exclusão seja bem-sucedida, os valores originais devem ser iguais para os valores atuais do banco de dados](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: For Update ou Delete para sucesso, o Original valores deve ser igual para os valores atuais do banco de dados ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Há várias abordagens para implementar a simultaneidade otimista (consulte [Peter A. Bromberg](http://peterbromberg.net/) s [lógica de atualização de simultaneidade de Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) para examinar uma série de opções). A técnica usada por SqlDataSource (bem como o ADO.NET digitado os conjuntos de dados usados em nossa camada de acesso a dados) aumenta a `WHERE` cláusula para incluir uma comparação de todos os valores originais. O seguinte `UPDATE` instrução, por exemplo, atualiza o nome e o preço de um produto somente se os valores atuais do banco de dados são iguais aos valores originalmente recuperados ao atualizar o registro em GridView. O `@ProductName` e `@UnitPrice` parâmetros contêm os novos valores inseridos pelo usuário, enquanto `@original_ProductName` e `@original_UnitPrice` contêm os valores que foram carregados originalmente em GridView, quando o botão de edição foi clicado:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Como veremos neste tutorial, permitindo que o controle de simultaneidade otimista com SqlDataSource é tão simple quanto marcar uma caixa de seleção.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Etapa 1: Criando um SqlDataSource que dá suporte à simultaneidade otimista

Comece abrindo o `OptimisticConcurrency.aspx` página a partir de `SqlDataSource` pasta. Arraste um controle SqlDataSource da caixa de ferramentas para o Designer, as configurações de seu `ID` propriedade `ProductsDataSourceWithOptimisticConcurrency`. Em seguida, clique no link configurar fonte de dados de marca inteligente de s de controle. Na primeira tela do assistente, escolha para trabalhar com o `NORTHWINDConnectionString` e clique em Avançar.


[![Escolha para trabalhar com o NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: escolha para trabalhar com o `NORTHWINDConnectionString` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


Para este exemplo incluiremos um GridView que permite que os usuários editem o `Products` tabela. Portanto, de configurar a tela de instrução Select, escolha o `Products` tabela da lista suspensa e selecione o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas, conforme mostrado na Figura 5.


[![Da tabela produtos, retornar o ProductID, ProductName, UnitPrice e colunas descontinuadas](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Figura 5**: da `Products` da tabela, retornar o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Depois de escolher as colunas, clique no botão Avançado para exibir a caixa de diálogo Advanced SQL Generation Options. Verifique a gerar `INSERT`, `UPDATE`, e `DELETE` instruções e Use as caixas de seleção de simultaneidade otimista e clique em Okey (consulte novamente a Figura 1 para uma captura de tela). Conclua o assistente clicando em Avançar e em seguida concluir.

Depois de concluir o Assistente Configurar fonte de dados, reserve um tempo para examinar resultante `DeleteCommand` e `UpdateCommand` propriedades e o `DeleteParameters` e `UpdateParameters` coleções. A maneira mais fácil de fazer isso é clicar na guia fonte, no canto inferior esquerdo para ver a sintaxe declarativa de s de página. Lá você encontrará uma `UpdateCommand` valor:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Com sete parâmetros o `UpdateParameters` coleção:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Da mesma forma, o `DeleteCommand` propriedade e `DeleteParameters` coleção deve ser semelhante ao seguinte:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Além de aumentar o `WHERE` cláusulas do `UpdateCommand` e `DeleteCommand` propriedades (e adicionando os parâmetros adicionais para as coleções de parâmetro do respectivos), selecionando o uso otimista de simultaneidade opção ajusta dois outros propriedades:

- Alterações de [ `ConflictDetection` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) de `OverwriteChanges` (o padrão) para`CompareAllValues`
- Alterações de [ `OldValuesParameterFormatString` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) de {0} (o padrão) original\_{0}.

Quando os dados de controle da Web chama a s SqlDataSource `Update()` ou `Delete()` método, ele passa os valores originais. Se o s SqlDataSource `ConflictDetection` está definida como `CompareAllValues`, esses valores originais são adicionados ao comando. O `OldValuesParameterFormatString` propriedade fornece o padrão de nomenclatura usado para esses parâmetros de valor original. O Assistente Configurar fonte de dados usa original\_{0} e nomes de cada parâmetro original a `UpdateCommand` e `DeleteCommand` propriedades e `UpdateParameters` e `DeleteParameters` coleções adequadamente.

> [!NOTE]
> Desde que não estiver usando o controle SqlDataSource s inserindo recursos, fique à vontade para remover o `InsertCommand` propriedade e seu `InsertParameters` coleção.


## <a name="correctly-handlingnullvalues"></a>Lidar corretamente com`NULL`valores

Infelizmente, o aumentada `UPDATE` e `DELETE` é gerado automaticamente de instruções pelo Assistente Configurar fonte de dados ao usar a simultaneidade otimista *não* trabalhar com registros que contêm `NULL` valores. Para ver o porquê, considere nosso s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

O `UnitPrice` coluna o `Products` tabela pode ter `NULL` valores. Se um determinado registro tem um `NULL` valor para `UnitPrice`, o `WHERE` parte da cláusula `[UnitPrice] = @original_UnitPrice` será *sempre* retornam False porque `NULL = NULL` sempre retorna False. Portanto, os registros que contêm `NULL` valores não podem ser editados ou excluídos, como o `UPDATE` e `DELETE` instruções `WHERE` cláusulas ganha t retornam linhas para atualizar ou excluir.

> [!NOTE]
> Esse erro foi relatado pela primeira vez à Microsoft em junho de 2004 em [SqlDataSource gera instruções SQL incorretas](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) e supostamente está programado para ser corrigido na próxima versão do ASP.NET.


Para corrigir isso, temos que atualizar manualmente o `WHERE` cláusulas em ambos o `UpdateCommand` e `DeleteCommand` propriedades para **todos os** colunas que pode ter `NULL` valores. Em geral, alterar `[ColumnName] = @original_ColumnName` para:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Essa modificação pode ser feita diretamente por meio de marcação declarativa, por meio de opções UpdateQuery ou DeleteQuery na janela de propriedades, ou a atualização e excluir guias em especificar uma opção de procedimento armazenado em Configurar dados ou uma instrução SQL personalizada Assistente de origem. Novamente, essa modificação deve ser feita para *cada* coluna o `UpdateCommand` e `DeleteCommand` s `WHERE` cláusula que pode conter `NULL` valores.

Aplicar isso em nosso exemplo resulta no seguinte modificado `UpdateCommand` e `DeleteCommand` valores:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Etapa 2: Adicionando um GridView com editar e excluir opções

Com o SqlDataSource configurado para dar suporte a simultaneidade otimista, tudo o que permanece é adicionar um controle da Web de dados para a página que utiliza esse controle de simultaneidade. Para este tutorial, permitem s adicionar um controle GridView que fornece os dois editar e excluir funcionalidade. Para fazer isso, arraste um controle GridView da caixa de ferramentas para o Designer e defina seu `ID` para `Products`. De GridView s marca inteligente, associá-lo para o `ProductsDataSourceWithOptimisticConcurrency` controle SqlDataSource adicionado na etapa 1. Finalmente, verifique as opções de marca inteligente habilitar edição e habilitar a exclusão.


[![Vincular GridView a SqlDataSource e permitir a edição e exclusão](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Figura 6**: associar o GridView SqlDataSource e habilitar edição e exclusão ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


Depois de adicionar o GridView, configure sua aparência, removendo o `ProductID` BoundField, alterando o `ProductName` BoundField s `HeaderText` propriedade ao produto e atualizando o `UnitPrice` BoundField para que seu `HeaderText` é de propriedade simplesmente preço. Idealmente, d, aprimorar a interface de edição para incluir um RequiredFieldValidator para o `ProductName` valor e um CompareValidator para o `UnitPrice` valor (para garantir que ele s um valor numérico formatado corretamente). Consulte o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial para uma análise mais detalhada Personalizando o s GridView interface de edição.

> [!NOTE]
> O GridView no estado de exibição s deve ser habilitado porque os valores originais passados do GridView para o SqlDataSource são armazenados na exibição de estado.


Depois de fazer essas modificações GridView, a marcação declarativa GridView e SqlDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Para ver o controle de simultaneidade otimista em ação, abra duas janelas de navegador e carregar o `OptimisticConcurrency.aspx` página em ambos. Clique nos botões de edição do produto primeiro em ambos os navegadores. Em um navegador, altere o nome do produto e clique em Atualizar. O navegador será postback e GridView retornará para o modo de edição previamente, mostrando o novo nome do produto para o registro que acabou de editar.

Na segunda janela do navegador, altere o preço (mas não o nome do produto como seu valor original) e clique em Atualizar. No postback, a grade retorna para o modo de edição previamente, mas a alteração no preço não será registrada. O segundo navegador mostra o mesmo valor que o primeiro o novo nome de produto com o preço antigo. As alterações feitas na segunda janela do navegador foram perdidas. Além disso, as alterações foram perdidas em vez disso, silenciosamente, porque não houve nenhuma exceção ou uma mensagem indicando que uma violação de concorrência ocorreu.


[![As alterações na segunda janela do navegador foram perdidas silenciosamente](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Figura 7**: as alterações no segundo navegador janela foram silenciosamente perdidas ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


O motivo por que as alterações de navegador s segundo não foram confirmadas foi porque o `UPDATE` instrução s `WHERE` cláusula filtrou todos os registros e, portanto, não afetou nenhuma linha. Permitir que o s examinar o `UPDATE` novamente a instrução:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Quando a segunda janela do navegador atualiza o registro, o nome do produto original especificado no `WHERE` cláusula t correspondente com o nome do produto existente (desde que ele foi alterado pelo navegador primeiro). Portanto, a instrução `[ProductName] = @original_ProductName` retorna False e o `UPDATE` não afeta todos os registros.

> [!NOTE]
> A exclusão funciona da mesma maneira. Com duas janelas de navegador abertas, inicie pela edição de um determinado produto com um e, em seguida, salvar suas alterações. Depois de salvar as alterações em um navegador, clique no botão de exclusão para o mesmo produto em outro. Desde que o original don valores t correspondentes no `DELETE` instrução s `WHERE` cláusula, a exclusão silenciosamente falha.


Da perspectiva do usuário final s na segunda janela do navegador, depois de clicar no botão atualizar a grade retorna para o modo de edição previamente, mas suas alterações foram perdidas. No entanto, há s sem comentários visuais que fique de t de suas alterações. Idealmente, se alterações um usuário s são perdidas para uma violação de concorrência, podemos d notificá-lo e, talvez, mantenha a grade no modo de edição. Permitir que o s examinar como fazer isso.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Etapa 3: Determinar quando ocorreu uma violação de concorrência

Como uma violação de concorrência rejeitará as alterações feitas por um, seria interessante alertar o usuário quando ocorreu uma violação de concorrência. Para alertar o usuário, permitem s adicionar um controle de Web de rótulo na parte superior da página chamada `ConcurrencyViolationMessage` cujo `Text` propriedade exibirá a seguinte mensagem: você tentou atualizar ou excluir um registro que foi atualizado simultaneamente por outro usuário. Revise as alterações do outro usuário e refazer a atualização ou excluir. Definir o controle de rótulo s `CssClass` propriedade para aviso, que é uma classe CSS definida no `Styles.css` que exibe o texto em uma fonte vermelha, itálico, negrito e grande. Por fim, defina o rótulo s `Visible` e `EnableViewState` propriedades `False`. Isso ocultará o rótulo, exceto apenas essas postagens onde podemos definir explicitamente seu `Visible` propriedade `True`.


[![Adicionar um controle de rótulo para a página para exibir o aviso](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Figura 8**: adicionar um controle de rótulo para a página para exibir o aviso ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Ao executar uma atualização ou exclusão, o s GridView `RowUpdated` e `RowDeleted` manipuladores de eventos acionam depois que o controle de fonte de dados executou solicitada update ou delete. É possível determinar quantas linhas foram afetadas pela operação desses manipuladores de eventos. Se zero linhas foram afetadas, desejamos exibir o `ConcurrencyViolationMessage` rótulo.

Criar um manipulador de eventos para ambos os `RowUpdated` e `RowDeleted` eventos e adicione o seguinte código:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

Em ambos os manipuladores de eventos verificamos o `e.AffectedRows` propriedade e, se ele for igual a 0, defina o `ConcurrencyViolationMessage` rótulo s `Visible` propriedade `True`. No `RowUpdated` manipulador de eventos, podemos também instruir o GridView permanecer no modo de edição, definindo o `KeepInEditMode` propriedade como true. Dessa forma, é preciso associar novamente os dados à grade para que os outros dados de usuário s são carregados para a interface de edição. Isso é feito chamando a s GridView `DataBind()` método.

Como mostra a Figura 9, com esses manipuladores de eventos de dois, será exibida uma mensagem perceptível sempre que ocorre uma violação de concorrência.


[![Será exibida uma mensagem caso ocorra uma violação de concorrência](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Figura 9**: A mensagem é exibida no caso de uma violação de concorrência ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Resumo

Ao criar um aplicativo da web onde simultânea de vários usuários podem editar os mesmos dados, é importante considerar opções de controle de simultaneidade. Por padrão, os dados de que controles da Web do ASP.NET e controles de fonte de dados não usar qualquer controle de simultaneidade. Como vimos neste tutorial, implementar o controle de simultaneidade otimista com SqlDataSource é relativamente fácil e rápido. SqlDataSource lidará com a maioria do trabalho a externo de sua adição aumentada `WHERE` cláusulas para gerada automaticamente `UPDATE` e `DELETE` instruções mas há são alguns sutilezas na manipulação de `NULL` valor colunas, conforme discutido no Lidar corretamente com `NULL` valores de seção.

Este tutorial conclui nossa análise do SqlDataSource. Nossos tutoriais restantes retornará a trabalhar com dados usando o ObjectDataSource e arquitetura hierárquica.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
