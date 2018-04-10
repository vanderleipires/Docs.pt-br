---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Executar atualizações em lotes (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como criar um totalmente editável DataList em que todos os seus itens estão no modo de edição e cujos valores podem ser salvos, clicando em um botão 'Atualizar tudo' em de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d28a431c2b09de8c46079e888aa191017de4e30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="performing-batch-updates-vb"></a>Executar atualizações em lotes (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) ou [baixar PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Saiba como criar um totalmente editável DataList em que todos os seus itens estão no modo de edição e cujos valores podem ser salvos, clicando em um botão "Atualizar tudo" na página.


## <a name="introduction"></a>Introdução

No [tutorial anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) examinamos como criar um nível de item DataList. Como o GridView editável padrão, cada item em DataList incluída uma edição de botão que, quando clicado, faça o item editável. Enquanto esse nível de item edição funciona bem para dados que só são atualizados ocasionalmente, determinados cenários de caso de uso exigem que o usuário editar vários registros. Se um usuário precisa editar dezenas de registros e será forçado a clique em Editar, faça as alterações e clique em atualizar para cada um, a quantidade de clicar em pode dificultar sua produtividade. Em tais situações, a melhor opção é fornecer uma DataList totalmente editável, um onde *todos os* de seus itens estão em modo de edição e cujos valores podem ser editados clicando em um botão Atualizar tudo na página (consulte a Figura 1).


[![Cada Item em DataList totalmente editável pode ser modificado.](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: cada Item em DataList totalmente editável pode ser modificado ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image3.png))


Neste tutorial, examinaremos como habilitar usuários atualizar informações de endereço de fornecedores usando uma DataList totalmente editável.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Etapa 1: Criar a Interface do usuário editável ItemTemplate s DataList

No tutorial anterior, em que estamos criando uma DataList editável padrão, o nível de item, usamos dois modelos:

- `ItemTemplate` continha a interface de usuário somente leitura (os controles de rótulo Web para exibir cada s nome do produto e preço).
- `EditItemTemplate` continha a interface de usuário de modo de edição (os dois controles de caixa de texto Web).

DataList s `EditItemIndex` propriedade determina quais `DataListItem` (se houver) é processado usando o `EditItemTemplate`. Em particular, o `DataListItem` cujo `ItemIndex` valor corresponde o DataList s `EditItemIndex` propriedade é processada usando o `EditItemTemplate`. Esse modelo funciona bem quando somente um item pode ser editado por um tempo, mas fica distância ao criar uma DataList totalmente editável.

Para uma DataList totalmente editável, queremos *todos os* do `DataListItem` s para processar usando a interface editável. A maneira mais simples de fazer isso é definir a interface editável no `ItemTemplate`. Para modificar as informações de endereço de fornecedores, a interface editável contém o nome do fornecedor como texto e, em seguida, caixas de texto para o endereço, cidade e valores de país.

Comece abrindo o `BatchUpdate.aspx` página, adicione um controle DataList e defina seu `ID` propriedade `Suppliers`. Marca inteligente DataList s, optar por adicionar um novo controle ObjectDataSource chamado `SuppliersDataSource`.


[![Criar um novo ObjectDataSource denominado SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image6.png))


Configurar o ObjectDataSource para recuperar dados usando o `SuppliersBLL` classe s `GetSuppliers()` método (consulte a Figura 3). Com o tutorial anterior, ao invés de atualizar as informações de fornecedor por ObjectDataSource, trabalharemos diretamente com a camada de lógica de negócios. Portanto, definir a lista suspensa como (nenhum) no guia de atualização (consulte a Figura 4).


[![Recuperar informações de fornecedor usando o método GetSuppliers()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recuperar informações do fornecedor usando o `GetSuppliers()` método ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image9.png))


[![Definir a lista suspensa como (nenhum) no guia de atualização](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: definir a lista suspensa como (nenhum) no guia de atualização ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image12.png))


Depois de concluir o assistente, o Visual Studio gera automaticamente o DataList s `ItemTemplate` para exibir cada campo de dados retornado pela fonte de dados em um controle de rótulo. É preciso modificar este modelo para que ele fornece a interface de edição em vez disso. O `ItemTemplate` pode ser personalizado por meio do Designer usando a opção de editar modelos de marca inteligente DataList s ou diretamente a sintaxe declarativa.

Dedicar um tempo para criar uma interface de edição que exibe o nome do fornecedor s como texto, mas inclui caixas de texto para o fornecedor s endereço, cidade e valores de país. Depois de fazer essas alterações, a sintaxe declarativa s da página deve ser semelhante ao seguinte:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Como com o tutorial anterior, DataList neste tutorial deve ter seu estado de exibição habilitado.


No `ItemTemplate` m usando duas novas classes CSS, `SupplierPropertyLabel` e `SupplierPropertyValue`, que foram adicionado para o `Styles.css` classe e configurado para usar as mesmas configurações de estilo a `ProductPropertyLabel` e `ProductPropertyValue` classes CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Depois de fazer essas alterações, visite esta página por meio de um navegador. Como mostra a Figura 5, cada item de DataList exibe o nome do fornecedor como texto e usa caixas de texto para exibir o endereço, cidade e país.


[![Cada fornecedor em DataList é editável](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: cada fornecedor em DataList é editável ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Etapa 2: Adicionando uma atualização de todos os botões

Embora cada fornecedor na Figura 5, tem seu endereço, cidade e país campos exibidos em uma caixa de texto, atualmente não há nenhum botão de atualização disponível. Em vez de um botão de atualização por item, com DataLists totalmente editáveis normalmente há um único botão Atualizar tudo na página que, quando clicado, atualiza *todos os* dos registros em DataList. Para este tutorial, permitem s adicionar dois botões Atualizar tudo - uma na parte superior da página e outra na parte inferior (embora qualquer botão terá o mesmo efeito).

Comece a adicionar um controle de botão Web acima de DataList e defina seu `ID` propriedade `UpdateAll1`. Em seguida, adicionar o segundo controle de botão Web abaixo DataList, definindo seu `ID` para `UpdateAll2`. Definir o `Text` propriedades para os dois botões Atualizar tudo. Por fim, criar manipuladores de eventos para ambos os botões `Click` eventos. Em vez de duplicar a lógica de atualização em cada um dos manipuladores de eventos, permitem s refatorar essa lógica para um método de terceiro, `UpdateAllSupplierAddresses`, com os manipuladores de eventos, basta invocar esse método de terceiro.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Figura 6 mostra a página após os atualização de todos os botões foram adicionados.


[![Atualização de todos os botões de duas foram adicionados à página](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: dois todos os botões de atualização foram adicionados para a página ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Etapa 3: Atualizar todas as informações de endereço de fornecedores

Com todos os itens de s DataList exibindo a interface de edição e a adição dos botões Atualizar tudo, tudo o que permanece está gravando o código para executar a atualização em lotes. Especificamente, é necessário percorrer os itens de DataList s e a chamada a `SuppliersBLL` classe s `UpdateSupplierAddress` método para cada um.

A coleção de `DataListItem` instâncias que composição DataList pode ser acessado por meio do DataList s [ `Items` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Com uma referência a um `DataListItem`, podemos pode obter correspondente `SupplierID` do `DataKeys` referência TextBox Web controles em coleção e programaticamente o `ItemTemplate` como o código a seguir ilustra:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quando o usuário clica em um dos botões Atualizar tudo, o `UpdateAllSupplierAddresses` método itera em cada `DataListItem` no `Suppliers` DataList e chama o `SuppliersBLL` classe s `UpdateSupplierAddress` método, passando os valores correspondentes. Um valor não digitado para o endereço, cidade ou país passa é um valor de `Nothing` para `UpdateSupplierAddress` (em vez de uma cadeia de caracteres vazia), o que resulta em um banco de dados `NULL` para os campos de registro s subjacentes.

> [!NOTE]
> Como um aprimoramento, convém adicionar um controle da Web do rótulo de status para a página que fornece algumas mensagens de confirmação após a atualização em lotes é executada.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Atualizar apenas os endereços que foram modificados

O algoritmo de atualização do lote usado para este tutorial chamadas a `UpdateSupplierAddress` método *cada* fornecedor em DataList, independentemente se suas informações de endereço foi alteradas. Enquanto esse blind atualiza t são geralmente um problema de desempenho, elas podem gerar registros supérfluos se re auditoria as alterações na tabela de banco de dados. Por exemplo, se você usar gatilhos para registrar todos os `UPDATE` s para o `Suppliers` tabela para uma tabela de auditoria sempre que um usuário clica no botão Atualizar tudo que um novo registro de auditoria será criado para cada fornecedor no sistema, independentemente se o usuário fez qualquer alterações.

As classes de DataTable do ADO.NET e DataAdapter são projetadas para oferecer suporte a atualizações em lote em que os registros novos, excluídos e modificados apenas resulta em qualquer comunicação de banco de dados. Cada linha na DataTable tem um [ `RowState` propriedade](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) que indica se a linha foi adicionada à DataTable, excluído, modificados, ou permanece inalterada. Quando uma tabela de dados é inicialmente preenchida, todas as linhas são marcadas como inalteradas. Alterar o valor de qualquer uma das colunas s linha marca a linha como modificadas.

No `SuppliersBLL` classe podemos atualizar as informações de endereço do fornecedor especificado s lendo primeiro o registro do fornecedor único em uma `SuppliersDataTable` e, em seguida, defina o `Address`, `City`, e `Country` valores de coluna usando o seguinte código:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Esse código ingenuamente atribui o passado no endereço, cidade e valores de país para o `SuppliersRow` no `SuppliersDataTable` , independentemente de estarem ou não os valores foram alterados. Essas modificações causam o `SuppliersRow` s `RowState` propriedade a ser marcada como modificada. Quando a camada de acesso a dados s `Update` método é chamado, ele vê que o `SupplierRow` foi modificado e, portanto, envia um `UPDATE` comando no banco de dados.

No entanto, imagine que adicionamos código para este método para atribuir somente a passado no endereço, cidade e valores de país se eles forem diferentes do `SuppliersRow` s valores existentes. No caso em que o endereço, cidade e país são o mesmo que os dados existentes, nenhuma alteração será feita e o `SupplierRow` s `RowState` será ser marcado como de permanecem inalteradas. O resultado é que quando o s DAL `Update` método é chamado, não será feita nenhuma chamada de banco de dados porque o `SuppliersRow` não foi modificado.

Para aplicar essa alteração, substitua as declarações que cegamente atribuir a passado no endereço, cidade e valores de país com o código a seguir:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Com isso adicionado código, a DAL s `Update` método envia uma `UPDATE` instrução no banco de dados para somente os registros cujos valores de endereço foram alterados.

Como alternativa, poderíamos se há quaisquer diferenças entre os campos de endereço no passado e do banco de dados e, se não houver nenhum, simplesmente ignorar a chamada para o s DAL `Update` método. Essa abordagem funciona bem se você está usando o banco de dados direta método, desde que não seja o método direto o banco de dados passado um `SuppliersRow` instância cujo `RowState` pode ser verificada para determinar se uma chamada de banco de dados é realmente necessária.

> [!NOTE]
> Cada vez que o `UpdateSupplierAddress` método é invocado, é feita uma chamada para o banco de dados para recuperar informações sobre o registro atualizado. Em seguida, se houver alterações nos dados, outra chamada para o banco de dados é feita para atualizar a linha de tabela. Este fluxo de trabalho pode ser otimizado, criando um `UpdateSupplierAddress` sobrecarga do método que aceita um `EmployeesDataTable` instância tem *todos os* das alterações do `BatchUpdate.aspx` página. Em seguida, ele pode fazer uma chamada para o banco de dados para obter todos os registros da `Suppliers` tabela. Os dois conjuntos de resultados, em seguida, podem ser enumerados e somente os registros onde ocorreram alterações podem ser atualizados.


## <a name="summary"></a>Resumo

Neste tutorial, vimos como criar uma DataList totalmente editável, permitindo que um usuário modificar rapidamente as informações de endereço de vários fornecedores. Começamos definindo a interface de edição um controle TextBox Web para o fornecedor s endereço, cidade e valores de país em DataList s `ItemTemplate`. Em seguida, adicionamos atualizar todos os botões acima e abaixo do DataList. Depois que um usuário tiver feito suas alterações e clicou em um dos botões Atualizar tudo, o `DataListItem` s são enumeradas e uma chamada para o `SuppliersBLL` classe s `UpdateSupplierAddress` método é feito.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Zack Jones e Ken Pespisa. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Próximo](handling-bll-and-dal-level-exceptions-vb.md)
