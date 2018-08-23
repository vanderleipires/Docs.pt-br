---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementando a simultaneidade otimista com o SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examine os conceitos básicos de controle de simultaneidade otimista e, em seguida, explore como implementá-lo usando o controle SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 7695ffad0599701840da83670af3940569e01c21
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831648"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementando a simultaneidade otimista com o SqlDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) ou [baixar PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> Neste tutorial, examine os conceitos básicos de controle de simultaneidade otimista e, em seguida, explore como implementá-lo usando o controle SqlDataSource.


## <a name="introduction"></a>Introdução

O tutorial anterior, examinamos como adicionar a inserção, atualização e exclusão de recursos para o controle SqlDataSource. Em resumo, para fornecer esses recursos que precisávamos especificar correspondente `INSERT`, `UPDATE`, ou `DELETE` instrução SQL no controle s `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` propriedades, junto com o apropriada parâmetros de `InsertParameters`, `UpdateParameters`, e `DeleteParameters` coleções. Embora essas propriedades e coleções podem ser especificadas manualmente, o botão Avançado do s de Assistente Configurar fonte de dados oferece um gerar `INSERT`, `UPDATE`, e `DELETE` caixa de seleção de instruções que irá criar automaticamente essas instruções com base no `SELECT` instrução.

Juntamente com o gere `INSERT`, `UPDATE`, e `DELETE` instruções de caixa de seleção, a caixa de diálogo Advanced SQL Generation Options inclui uma opção de simultaneidade otimista de uso (veja a Figura 1). Quando marcada, o `WHERE` cláusulas em gerada automaticamente `UPDATE` e `DELETE` instruções são modificadas para executar apenas a atualização ou exclusão se o t de realizada de dados banco de dados subjacente foi modificada desde que o usuário pela última vez carregados os dados na grade.


![Você pode adicionar suporte à simultaneidade otimista de avançada caixa de diálogo de opções de geração SQL](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Figura 1**: você pode adicionar suporte à simultaneidade otimista de avançada caixa de diálogo de opções de geração SQL


Volta a [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial, examinamos os conceitos básicos do controle de simultaneidade otimista e como adicioná-lo a ObjectDataSource. Neste tutorial, vamos Retocar essenciais do controle de simultaneidade otimista e, em seguida, explore como implementá-lo usando o SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Uma recapitulação de simultaneidade otimista

Para aplicativos web que permitem que vários usuários simultâneos para editar ou excluir os mesmos dados, existe uma possibilidade de que um usuário pode substituir acidentalmente as alterações de s outro. No [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial forneci o exemplo a seguir:

Imagine-se de que ambos dois usuários, Jisun e Sam, foram visitar uma página em um aplicativo que os visitantes para atualizar e excluir produtos por meio de um controle GridView de permissão. Ambos clique no botão Editar para Chai quase ao mesmo tempo. Jisun altera o nome do produto para Chai chá e clica no botão de atualização. O resultado é um `UPDATE` instrução é enviada para o banco de dados que define *todas as* dos campos atualizáveis produto s (mesmo que Jisun atualizadas apenas um campo, `ProductName`). Neste momento, o banco de dados tem os valores Chai chá, a categoria de bebidas, líquidos exóticos fornecedor, e assim por diante para este produto específico. No entanto, o GridView na tela do Sam s ainda mostra o nome do produto na linha de GridView editável como Chai. Alguns segundos depois de alterações de s Jisun foram confirmadas, Sam atualiza a categoria para Condimentos e clica em Atualizar. Isso resulta em uma `UPDATE` instrução enviada ao banco de dados que define o nome do produto a Chai, o `CategoryID` Condimentos categoria ID correspondente, e assim por diante. Alterações de s Jisun para o nome do produto foram substituídas.

Figura 2 ilustra essa interação.


[![Quando dois usuários simultaneamente atualizam um registro lá s potencial para um usuário s é alterada para substituir os outros s](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Figura 2**: quando dois usuários simultaneamente a atualização um registro lá s potencial para um usuário s muda para substituir os outros s ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Para impedir que esse cenário abrindo um formulário da [controle de simultaneidade](http://en.wikipedia.org/wiki/Concurrency_control) deve ser implementado. [A simultaneidade otimista](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) o foco deste tutorial funciona na suposição de que embora possa haver conflitos de simultaneidade vira e mexe, a grande maioria das vezes esses conflitos ganharam t surgir. Portanto, se ocorrer um conflito, controle de simultaneidade otimista simplesmente informa ao usuário que suas alterações pode ser salva porque outro usuário modificou os mesmos dados.

> [!NOTE]
> Para aplicativos em que ele é presumido que haja muitos conflitos de simultaneidade ou se tais conflitos não são toleráveis, em seguida, controle de simultaneidade pessimista pode ser usado em vez disso. Voltar para o [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial para uma discussão mais completa sobre o controle de simultaneidade pessimista.


Controle de simultaneidade otimista funciona, garantindo que o registro que está sendo atualizada ou excluída tem os mesmos valores de quando a atualização ou exclusão de processo iniciado. Por exemplo, ao clicar no botão Editar em um GridView editável, os valores de registro s são de leitura do banco de dados e exibidos em caixas de texto e outros controles de Web. Esses valores originais são salvos por GridView. Posteriormente, depois que o usuário faz as alterações dela e clica no botão de atualização, o `UPDATE` instrução usada deve levar em conta os valores originais e os novos valores e só atualizar o registro de banco de dados subjacente se os valores originais que o usuário iniciou a edição são idênticos aos valores ainda no banco de dados. Figura 3 ilustra essa sequência de eventos.


[![Para a atualização ou exclusão seja bem-sucedida, os valores originais devem ser iguais aos valores atuais do banco de dados](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: para Update ou Delete para forem bem-sucedidas, o Original valores deve ser igual aos valores atuais do banco de dados ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Há várias abordagens para implementar a simultaneidade otimista (consulte [Peter A. Bromberg](http://peterbromberg.net/) s [Optmistic simultaneidade atualizando lógica](http://www.eggheadcafe.com/articles/20050719.asp) para examinar uma série de opções). A técnica usada pelo SqlDataSource (bem como pelo ADO.NET digitado conjuntos de dados usados em nossa camada de acesso a dados) aumenta a `WHERE` cláusula para incluir uma comparação de todos os valores originais. O seguinte `UPDATE` instrução, por exemplo, atualiza o nome e o preço de um produto somente se os valores atuais do banco de dados são iguais aos valores que foram originalmente recuperados ao atualizar o registro em um GridView. O `@ProductName` e `@UnitPrice` parâmetros contêm os novos valores inseridos pelo usuário, enquanto `@original_ProductName` e `@original_UnitPrice` contêm os valores que foram carregados originalmente no GridView, quando o botão de edição foi clicado:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Como veremos neste tutorial, permitindo o controle de simultaneidade otimista com o SqlDataSource é tão simple quanto marcar uma caixa de seleção.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Etapa 1: Criando um SqlDataSource que dá suporte à simultaneidade otimista

Comece abrindo o `OptimisticConcurrency.aspx` página a partir de `SqlDataSource` pasta. Arraste um controle SqlDataSource da caixa de ferramentas para o Designer, as configurações de seu `ID` propriedade para `ProductsDataSourceWithOptimisticConcurrency`. Em seguida, clique no link configurar fonte de dados do que a marca inteligente do controle s. Na primeira tela do assistente, escolha para trabalhar com o `NORTHWINDConnectionString` e clique em Avançar.


[![Optar por trabalhar com o NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: escolha para trabalhar com o `NORTHWINDConnectionString` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


Para este exemplo, estaremos adicionando um GridView que permite que os usuários editem o `Products` tabela. Portanto, de configurar a tela de instrução Select, escolha o `Products` da tabela na lista suspensa e selecione o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas, conforme mostrado na Figura 5.


[![Da tabela produtos, retornar o ProductID, ProductName, UnitPrice e colunas descontinuadas](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Figura 5**: do `Products` da tabela, retornar o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colunas ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Depois de escolher as colunas, clique no botão Avançado para abrir a caixa de diálogo Advanced SQL Generation Options. Verifique o gere `INSERT`, `UPDATE`, e `DELETE` instruções e Use as caixas de seleção de simultaneidade otimista e clique em Okey (consulte novamente a Figura 1 para uma captura de tela). Conclua o assistente clicando em Avançar e em seguida concluir.

Depois de concluir o Assistente Configurar fonte de dados, reserve um tempo para examinar resultante `DeleteCommand` e `UpdateCommand` propriedades e o `DeleteParameters` e `UpdateParameters` coleções. A maneira mais fácil de fazer isso é clicar na guia origem no canto inferior esquerdo para ver a sintaxe declarativa de s de página. Lá você encontrará um `UpdateCommand` valor:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Com sete parâmetros de `UpdateParameters` coleção:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Da mesma forma, o `DeleteCommand` propriedade e `DeleteParameters` coleção deve ser semelhante ao seguinte:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Além de ampliar as `WHERE` cláusulas do `UpdateCommand` e `DeleteCommand` propriedades (e adicionando os parâmetros adicionais para as coleções de parâmetro respectivos), selecionando o uso otimista de simultaneidade opção ajusta duas outras propriedades:

- As alterações a [ `ConflictDetection` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) de `OverwriteChanges` (o padrão) para `CompareAllValues`
- As alterações a [ `OldValuesParameterFormatString` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) partir {0} (o padrão) ao original\_ {0} .

Quando os dados de controle de Web invoca o s SqlDataSource `Update()` ou `Delete()` método, ele passa os valores originais. Se o s SqlDataSource `ConflictDetection` estiver definida como `CompareAllValues`, esses valores originais são adicionados ao comando. O `OldValuesParameterFormatString` propriedade fornece o padrão de nomenclatura usado para esses parâmetros de valor original. O Assistente Configurar fonte de dados usa original\_ {0} e os nomes de cada parâmetro original na `UpdateCommand` e `DeleteCommand` propriedades e `UpdateParameters` e `DeleteParameters` coleções adequadamente.

> [!NOTE]
> Uma vez que estamos não estiver usando o controle SqlDataSource s inserindo recursos, fique à vontade remover o `InsertCommand` propriedade e seu `InsertParameters` coleção.


## <a name="correctly-handlingnullvalues"></a>Tratar corretamente`NULL`valores

Infelizmente, o aumentadas `UPDATE` e `DELETE` gerado automaticamente de instruções pelo Assistente Configurar fonte de dados ao usar a simultaneidade otimista fazer *não* funcionam com os registros que contêm `NULL` valores. Para ver o porquê, considere o nosso s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

O `UnitPrice` coluna o `Products` tabela pode ter `NULL` valores. Se um determinado registro tem um `NULL` valor para `UnitPrice`, o `WHERE` parte da cláusula `[UnitPrice] = @original_UnitPrice` serão *sempre* forem avaliadas como False, porque `NULL = NULL` sempre retorna False. Portanto, os registros que contêm `NULL` valores não podem ser editados ou excluídos, como o `UPDATE` e `DELETE` instruções `WHERE` cláusulas ganharam t retornam linhas para atualizar ou excluir.

> [!NOTE]
> Esse bug foi reportado pela primeira vez para a Microsoft em junho de 2004 na [SqlDataSource gera instruções SQL incorretas](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) e supostamente está programado para ser corrigido na próxima versão do ASP.NET.


Para corrigir isso, temos que atualizar manualmente a `WHERE` cláusulas em ambos os `UpdateCommand` e `DeleteCommand` propriedades para **todos os** colunas que pode ter `NULL` valores. Em geral, alterar `[ColumnName] = @original_ColumnName` para:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Essa modificação pode ser feita diretamente por meio de marcação declarativa, por meio das opções UpdateQuery ou DeleteQuery na janela Propriedades, ou a atualização e excluir guias na especifique uma instrução SQL personalizada ou a opção de procedimento armazenado nos dados de configurar Assistente de fonte. Novamente, essa modificação deve ser feita para *cada* coluna o `UpdateCommand` e `DeleteCommand` s `WHERE` cláusula pode conter `NULL` valores.

Aplicar isso ao nosso exemplo resulta no seguinte modificada `UpdateCommand` e `DeleteCommand` valores:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Etapa 2: Adicionando um GridView com editar e opções de exclusão

Com o SqlDataSource configurado para dar suporte à simultaneidade otimista, tudo o que resta é adicionar um controle da Web de dados para a página que utiliza esse controle de simultaneidade. Para este tutorial, deixe s um GridView que fornece de edição e a funcionalidade de exclusão. Para fazer isso, arraste um GridView Toolbox para o Designer e defina suas `ID` para `Products`. Da GridView s marca inteligente, associá-lo para o `ProductsDataSourceWithOptimisticConcurrency` controle SqlDataSource adicionado na etapa 1. Por fim, verifique as opções de permitir edição e habilitar a exclusão da marca inteligente.


[![Vincular o GridView a SqlDataSource e permitir a edição e exclusão](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Figura 6**: associar o GridView SqlDataSource e habilitar edição e exclusão ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


Depois de adicionar o GridView, configuram sua aparência, removendo o `ProductID` BoundField, alterando a `ProductName` s BoundField `HeaderText` propriedade ao produto e atualizando o `UnitPrice` BoundField, de modo que seu `HeaderText` é de propriedade simplesmente preço. O ideal é que podemos d aprimorar a interface de edição para incluir um RequiredFieldValidator para o `ProductName` valor e um CompareValidator para o `UnitPrice` valor (para assegurar que ele s um valor numérico formatado corretamente). Consulte a [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial para uma análise mais detalhada Personalizando o s GridView interface de edição.

> [!NOTE]
> O GridView, o estado de exibição s deve ser habilitado, pois os valores originais passados do GridView para o SqlDataSource são armazenados na exibição de estado.


Depois de fazer essas modificações para o GridView, a marcação declarativa GridView e SqlDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Para ver o controle de simultaneidade otimista em ação, abra duas janelas de navegador e carregar o `OptimisticConcurrency.aspx` página em ambos. Clique nos botões de edição para o primeiro produto em ambos os navegadores. Em um navegador, altere o nome do produto e clique em Atualizar. O navegador fará o postback e GridView retornará para o modo de edição previamente, mostrando o novo nome do produto para o registro que acabou de editar.

Na segunda janela do navegador, altere o preço (mas deixe o nome do produto como seu valor original) e clique em Atualizar. No postback, a grade de retorna para o modo de edição previamente, mas a alteração no preço não será registrada. O segundo navegador mostra o mesmo valor que a primeira é o novo nome de produto com o preço antigo. As alterações feitas na segunda janela do navegador foram perdidas. Além disso, as alterações foram perdidas em vez disso, no modo silencioso, pois não houve nenhuma exceção ou uma mensagem indicando que uma violação de simultaneidade ocorreu.


[![As alterações na segunda janela do navegador foram perdidas silenciosamente](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Figura 7**: as alterações no segundo navegador janela foram silenciosamente perdidas ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


O motivo por que as alterações de navegador s segunda não foram confirmadas era porque o `UPDATE` instrução s `WHERE` cláusula filtrou todos os registros e, portanto, não afetou nenhuma linha. Permitir que o s examinar o `UPDATE` novamente a instrução:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Quando a segunda janela do navegador atualiza o registro, o nome do produto original especificado no `WHERE` cláusula t correspondente com o nome de produto existente (uma vez que ele foi alterado pelo navegador primeiro). Portanto, a instrução `[ProductName] = @original_ProductName` retorna False e o `UPDATE` não afeta todos os registros.

> [!NOTE]
> Exclua funciona da mesma maneira. Com duas janelas de navegador abertas, começar a edição de um determinado produto com um e, em seguida, salvando suas alterações. Depois de salvar as alterações em um navegador, clique no botão de exclusão para o mesmo produto na outra. Uma vez que o original don valores t coincidir as `DELETE` instrução s `WHERE` cláusula, a exclusão falhará silenciosamente.


Da perspectiva do usuário final s na segunda janela do navegador, depois de clicar no botão atualizar a grade retorna ao modo de edição previamente, mas suas alterações foram perdidas. No entanto, há s sem comentários visuais que fique t de suas alterações. O ideal é que, se um alterações de usuário s são perdidas para uma violação de simultaneidade, podemos d notificá-lo e, talvez, mantenha a grade no modo de edição. Deixe o s examinar como fazer isso.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Etapa 3: Determinar quando ocorreu uma violação de simultaneidade

Uma vez que uma violação de simultaneidade rejeita as alterações feitas por um, seria bom alertar o usuário quando ocorreu uma violação de simultaneidade. Para alertar o usuário, let s adicionar um controle de Web de rótulo na parte superior da página denominada `ConcurrencyViolationMessage` cujo `Text` propriedade exibirá a seguinte mensagem: você tentou atualizar ou excluir um registro que foi atualizado simultaneamente por outro usuário. Examine as alterações de outros usuários e refaça sua atualização ou excluir. Defina o controle de rótulo s `CssClass` propriedade para aviso, que é uma classe CSS definida no arquivo `Styles.css` que exibe texto em uma fonte vermelha, itálico, negrito e grande. Por fim, defina o rótulo s `Visible` e `EnableViewState` propriedades a serem `False`. Isso ocultará o rótulo, exceto apenas essas postagens onde definimos seu `Visible` propriedade para `True`.


[![Adicionar um controle de rótulo para a página para exibir o aviso](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Figura 8**: adicionar um controle de rótulo para a página para exibir o aviso ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Ao executar uma atualização ou exclusão, o s GridView `RowUpdated` e `RowDeleted` manipuladores de eventos é acionado depois que seu controle de fonte de dados tiver efetuado solicitada update ou delete. Podemos determinar quantas linhas foram afetadas pela operação desses manipuladores de eventos. Se nenhuma linha foi afetada, queremos exibir o `ConcurrencyViolationMessage` rótulo.

Criar um manipulador de eventos para ambos os `RowUpdated` e `RowDeleted` eventos e adicione o seguinte código:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

Em ambos os manipuladores de eventos verificamos a `e.AffectedRows` propriedade e, se ele for igual a 0, defina a `ConcurrencyViolationMessage` rótulo s `Visible` propriedade `True`. No `RowUpdated` manipulador de eventos, podemos também instruir o GridView permanecer no modo de edição, definindo o `KeepInEditMode` propriedade como true. Dessa forma, é preciso associar novamente os dados à grade para que os outros dados de usuário s seja carregados para a interface de edição. Isso é feito chamando o GridView s `DataBind()` método.

Como mostra a Figura 9, com esses dois manipuladores de evento, será exibida uma mensagem muito notável sempre que ocorre uma violação de simultaneidade.


[![Será exibida uma mensagem em caso de uma violação de simultaneidade](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Figura 9**: A mensagem é exibida em caso de uma violação de simultaneidade ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Resumo

Ao criar um aplicativo da web onde simultânea de vários usuários podem estar editando os mesmos dados, é importante considerar as opções de controle de simultaneidade. Por padrão, os dados de que controles da Web do ASP.NET e controles de fonte de dados não empregar qualquer controle de simultaneidade. Como vimos neste tutorial, a implementação de controle de simultaneidade otimista com o SqlDataSource é relativamente fácil e rápido. O SqlDataSource manipula a maioria do trabalho externo para sua adição aumentada `WHERE` cláusulas para gerada automaticamente `UPDATE` e `DELETE` , mas há instruções são algumas sutilezas na manipulação de `NULL` valor colunas, conforme discutido no Tratar corretamente `NULL` valores da seção.

Esse tutorial conclui nossa análise do SqlDataSource. Nossos tutoriais restantes retornará a trabalhar com dados usando o ObjectDataSource e arquitetura em camadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
