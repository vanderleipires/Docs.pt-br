---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Executar atualizações em lote (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como criar um totalmente editáveis DataList onde todos os seus itens estão no modo de edição e cujos valores podem ser salvos, clicando em um botão 'Atualizar tudo' na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: a877bcc5b26965d59065aa17959643b66a5c4484
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388484"
---
<a name="performing-batch-updates-vb"></a>Executar atualizações em lote (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) ou [baixar PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Saiba como criar um totalmente editáveis DataList onde todos os seus itens estão no modo de edição e cujos valores podem ser salvos, clicando em um botão "Atualizar tudo" na página.


## <a name="introduction"></a>Introdução

No [tutorial anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) , examinamos como criar um DataList de nível de item. Como o GridView editável padrão, cada item no DataList incluído uma edição botão que, quando clicado, seria tornar o item editável. Embora esse nível de item edição funciona bem para dados que só são atualizadas ocasionalmente, determinados cenários de caso de uso exigem que o usuário editar vários registros. Se um usuário precise editar dezenas de registros e é forçado a clicar em Editar, fazer suas alterações e clique em atualizar para cada um deles, a quantidade de clicar em pode dificultar sua produtividade. Em tais situações, uma opção melhor é fornecer uma DataList totalmente editável, aquele em que *todos os* seus itens estão no modo de edição e cujos valores podem ser editados clicando em um botão Atualizar tudo na página (consulte a Figura 1).


[![Cada Item em uma DataList totalmente editáveis pode ser modificado.](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: cada Item em uma DataList totalmente editáveis podem ser modificado ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image3.png))


Neste tutorial, examinaremos como habilitar usuários atualizar informações de endereço de fornecedores usando uma DataList totalmente editável.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Etapa 1: Criar a Interface do usuário editável ItemTemplate s DataList

No tutorial anterior, em que estamos criando uma DataList editável padrão, o nível de item, usamos dois modelos:

- `ItemTemplate` contido a interface de usuário somente leitura (os controles da Web de rótulo para exibir cada s nome do produto e preço).
- `EditItemTemplate` contido a interface de usuário do modo de edição (os dois controles de TextBox Web).

S DataList `EditItemIndex` propriedade determina quais `DataListItem` (se houver) é renderizado utilizando o `EditItemTemplate`. Em particular, o `DataListItem` cujos `ItemIndex` valor corresponde à DataList s `EditItemIndex` propriedade é renderizada utilizando o `EditItemTemplate`. Esse modelo funciona bem quando apenas um item pode ser editado em uma hora, mas voltará distância durante a criação de um DataList totalmente editável.

Para uma DataList totalmente editável, queremos *todos os* da `DataListItem` s para processar usando a interface editável. A maneira mais simples de fazer isso é definir a interface editável no `ItemTemplate`. Para modificar as informações de endereço de fornecedores, a interface editável contém o nome de fornecedor como texto e, em seguida, caixas de texto para o endereço, cidade e valores de país.

Comece abrindo o `BatchUpdate.aspx` página, adicione um controle DataList e defina sua `ID` propriedade `Suppliers`. Da marca inteligente DataList s, optar por adicionar um novo controle ObjectDataSource chamado `SuppliersDataSource`.


[![Criar um novo ObjectDataSource chamado SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image6.png))


Configurar o ObjectDataSource para recuperar dados usando o `SuppliersBLL` classe s `GetSuppliers()` método (veja a Figura 3). Assim como acontece com o tutorial anterior, em vez de atualizar as informações de fornecedor por meio do ObjectDataSource, trabalharemos diretamente com a camada de lógica de negócios. Portanto, definir a lista suspensa como (nenhum) no guia de atualização (consulte a Figura 4).


[![Recuperar informações de fornecedor usando o método GetSuppliers()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recupere informações do fornecedor usando a `GetSuppliers()` método ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image9.png))


[![Definir a lista suspensa como (nenhum) no guia de atualização](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: defina a lista suspensa como (nenhum) no guia de atualização ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image12.png))


Depois de concluir o assistente, o Visual Studio gera automaticamente DataList s `ItemTemplate` para exibir cada campo de dados retornado pela fonte de dados em um controle de rótulo. Precisamos modificar esse modelo para que ele fornece a interface de edição em vez disso. O `ItemTemplate` pode ser personalizado por meio do Designer usando a opção de editar modelos da marca inteligente DataList s ou diretamente por meio da sintaxe declarativa.

Reserve um tempo para criar uma interface de edição que exibe o nome do fornecedor s como texto, mas inclui caixas de texto para o fornecedor s endereço, cidade e valores de país. Depois de fazer essas alterações, sua sintaxe declarativa de s de página deve ser semelhante ao seguinte:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Como com o tutorial anterior, DataList neste tutorial deverá ter seu estado de exibição habilitado.


No `ItemTemplate` eu m usando duas novas classes CSS, `SupplierPropertyLabel` e `SupplierPropertyValue`, que foram adicionado ao `Styles.css` classe e configurado para usar as mesmas configurações de estilo que o `ProductPropertyLabel` e `ProductPropertyValue` classes CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Depois de fazer essas alterações, visite esta página por meio de um navegador. Como mostra a Figura 5, cada item DataList exibe o nome do fornecedor como texto e usa caixas de texto para exibir o endereço, cidade e país.


[![Cada fornecedor no DataList é editável](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: cada fornecedor no DataList é editável ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Etapa 2: Adicionando uma atualização de todos os botões

Embora cada fornecedor na Figura 5 tem seu endereço, cidade e campos de país, exibidos em uma caixa de texto, no momento, não há nenhum botão de atualização disponível. Em vez de ter um botão de atualização por item, com DataLists totalmente editáveis normalmente há um único botão Atualizar tudo na página que, quando clicado, atualiza *todos os* dos registros no DataList. Para este tutorial, deixe s adicionar dois botões Atualizar tudo - uma na parte superior da página e outra na parte inferior (embora clicando em um dos botões terá o mesmo efeito).

Comece adicionando um controle da Web de botão acima do DataList e o conjunto de seu `ID` propriedade para `UpdateAll1`. Em seguida, adicione o segundo controle da Web de botão embaixo do DataList, definindo sua `ID` para `UpdateAll2`. Defina o `Text` propriedades para os dois botões Atualizar tudo. Por fim, criar manipuladores de eventos para ambos os botões `Click` eventos. Em vez de duplicar a lógica de atualização em cada um dos manipuladores de eventos, let s refatorar essa lógica para um terceiro método, `UpdateAllSupplierAddresses`, tendo os manipuladores de eventos simplesmente invocar esse método de terceiro.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Figura 6 mostra a página após os atualização de todos os botões foram adicionados.


[![Dois atualização todos os botões foram adicionados à página](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: dois atualização todos os botões foram adicionados à página ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Etapa 3: Atualizar todas as informações de endereço de fornecedores

Com todos os itens de s DataList exibindo a interface de edição e a adição dos botões Atualizar tudo, tudo o que resta é escrever o código para executar a atualização em lotes. Especificamente, é necessário executar um loop por meio de itens DataList s e chamar o `SuppliersBLL` classe s `UpdateSupplierAddress` método para cada um deles.

A coleção de `DataListItem` instâncias essa composição DataList pode ser acessado por meio do DataList s [ `Items` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Com uma referência a um `DataListItem`, pode capturamos correspondente `SupplierID` da `DataKeys` referência TextBox Web controles dentro de coleção e programaticamente o `ItemTemplate` como o código a seguir ilustra:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quando o usuário clica em um dos botões Atualizar tudo, o `UpdateAllSupplierAddresses` método percorre cada `DataListItem` na `Suppliers` DataList e chama o `SuppliersBLL` classe s `UpdateSupplierAddress` método, passando os valores correspondentes. Um valor não digitado para o endereço, cidade ou país passes é um valor de `Nothing` à `UpdateSupplierAddress` (em vez de uma cadeia de caracteres vazia), que resulta em um banco de dados `NULL` para os campos de registro s subjacentes.

> [!NOTE]
> Como um aprimoramento, você talvez queira adicionar um controle da Web do rótulo de status para a página que fornece alguma mensagem de confirmação após a atualização em lotes é executada.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Atualizar apenas os endereços que foram modificados

O algoritmo de atualização de lote usado para este tutorial chamadas a `UpdateSupplierAddress` método para *cada* supplier no DataList, independentemente se as informações de endereço foi alteradas. Enquanto essa blind atualiza t são inválidas normalmente um problema de desempenho, eles podem levar supérfluos registros se você está a auditoria for alterado para a tabela de banco de dados. Por exemplo, se você usar gatilhos para registrar todos os `UPDATE` s para o `Suppliers` tabela para uma tabela de auditoria, sempre que um usuário clica no botão Atualizar tudo que um novo registro de auditoria será criado para cada fornecedor no sistema, independentemente se o usuário tiver feito uma alterações.

As classes de DataTable do ADO.NET e DataAdapter são projetadas para dar suporte a atualizações em lotes em que registros novos, excluídos e modificados apenas resulta em qualquer comunicação de banco de dados. Cada linha na DataTable tem um [ `RowState` propriedade](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) que indica se a linha foi adicionada à DataTable, excluído, modificados, ou permanece inalterada. Quando um DataTable é preenchido inicialmente, todas as linhas são marcadas inalteradas. Alterar o valor de qualquer uma das colunas s linha marca a linha como modificada.

No `SuppliersBLL` classe, podemos atualizar as informações de endereço de fornecedor especificado s com a primeira leitura no registro único fornecedor em um `SuppliersDataTable` e, em seguida, defina o `Address`, `City`, e `Country` valores de coluna usando o código a seguir:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Esse código ingenuamente atribui o passado no endereço, cidade e valores de país para o `SuppliersRow` no `SuppliersDataTable` , independentemente de estarem ou não os valores foram alterados. Fazer com que essas modificações a `SuppliersRow` s `RowState` propriedade a ser marcada como modificada. Quando a camada de acesso a dados s `Update` método é chamado, ele vê que o `SupplierRow` foi modificada e, portanto, envia um `UPDATE` comando no banco de dados.

No entanto, imagine que adicionamos código para este método para atribuir apenas a passado no endereço, cidade e valores de país, se eles forem diferentes do `SuppliersRow` valores existentes de s. No caso em que o endereço, cidade e país são o mesmo que os dados existentes, nenhuma alteração será feita e o `SupplierRow` s `RowState` não podem ser esquerda marcada como alterada. O resultado líquido é que quando o s DAL `Update` método é chamado, não será feita nenhuma chamada de banco de dados porque o `SuppliersRow` não foi modificado.

Para aplicar essa alteração, substitua as instruções que atribuir cegamente o passado no endereço, cidade e valores de país com o código a seguir:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Com esse código adicionado, o s DAL `Update` método envia uma `UPDATE` instrução no banco de dados para somente os registros cujos valores de endereço foram alterados.

Como alternativa, poderíamos controlar de se houver alguma diferença entre os campos de endereço passado e do banco de dados e, se não houver nenhum, basta ignorar a chamada para o s DAL `Update` método. Essa abordagem funciona bem se você está usando o banco de dados direcionar o método, já que não seja o método direto de DB passado um `SuppliersRow` instância cujo `RowState` pode ser verificada para determinar se uma chamada de banco de dados, na verdade, é necessário.

> [!NOTE]
> Cada vez que o `UpdateSupplierAddress` método é invocado, é feita uma chamada para o banco de dados para recuperar informações sobre o registro atualizado. Em seguida, se houver alterações nos dados, outra chamada para o banco de dados é feita para atualizar a linha da tabela. Esse fluxo de trabalho poderia ser otimizado com a criação de um `UpdateSupplierAddress` sobrecarga de método que aceita um `EmployeesDataTable` instância que tem *todos os* das alterações a `BatchUpdate.aspx` página. Em seguida, ele poderia fazer uma chamada para o banco de dados para obter todos os registros da `Suppliers` tabela. Os dois conjuntos de resultados, em seguida, poderiam ser enumerados e somente os registros em que ocorreram alterações pode ser atualizados.


## <a name="summary"></a>Resumo

Neste tutorial vimos como criar uma DataList totalmente editável, que permite ao usuário modificar rapidamente as informações de endereço para vários fornecedores. Começamos definindo a interface de edição para o fornecedor s endereço, cidade e valores de país, um controle de TextBox Web no DataList s `ItemTemplate`. Em seguida, adicionamos botões Atualizar tudo acima e abaixo do DataList. Depois que um usuário tiver feito suas alterações e clicou em um dos botões Atualizar tudo, o `DataListItem` s são enumerados e uma chamada para o `SuppliersBLL` classe s `UpdateSupplierAddress` método é feito.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Zack Jones e Ken Pespisa. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Próximo](handling-bll-and-dal-level-exceptions-vb.md)
