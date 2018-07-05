---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Adicionando confirmação do lado do cliente ao excluir (VB) | Microsoft Docs
author: rick-anderson
description: As interfaces que criamos até agora, um usuário pode excluir acidentalmente dados clicando no botão Excluir quando pretendiam clicar no botão Editar. Nesse t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 2318018b20841038ebb7a6e4900cf9849057d941
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802481"
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>Adicionando confirmação do lado do cliente ao excluir (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) ou [baixar PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> As interfaces que criamos até agora, um usuário pode excluir acidentalmente dados clicando no botão Excluir quando pretendiam clicar no botão Editar. Neste tutorial, adicionaremos uma caixa de diálogo de confirmação do lado do cliente que é exibido quando o botão Excluir é clicado.


## <a name="introduction"></a>Introdução

Sobre os últimos tutoriais de vários nós ve viu como usar nossa arquitetura de aplicativo, ObjectDataSource e os dados de controles da Web em conjunto para fornecer a inserção, edição e exclusão de recursos. A exclusão de interfaces podemos ve examinada até agora foram composto de uma exclusão de botão que, quando clicado, faz com que um postback e invoca o s ObjectDataSource `Delete()` método. O `Delete()` método, em seguida, invoca o método configurado da camada de lógica de negócios, que propaga a chamada para baixo até a camada de acesso a dados, o valor real de emissão `DELETE` instrução no banco de dados.

Embora essa interface do usuário permite que os visitantes excluir registros pelos controles GridView, DetailsView ou FormView, ele não tem qualquer tipo de confirmação quando o usuário clica no botão Excluir. Se um usuário acidentalmente clicar no botão Excluir quando pretendiam clicar em Editar, o registro que eles devem atualizar em vez disso, será excluído. Para evitar isso, neste tutorial, adicionaremos uma caixa de diálogo de confirmação do lado do cliente que é exibido quando o botão Excluir é clicado.

O JavaScript `confirm(string)` função exibe seu parâmetro de entrada de cadeia de caracteres como o texto dentro de uma caixa de diálogo modal que vem equipado com dois botões - Okey e Cancelar (veja a Figura 1). O `confirm(string)` função retorna um valor booliano, dependendo de qual botão é clicado (`true`, se o usuário clica em Okey, e `false` clicando em ' Cancelar ').


![O método de confirm(string) de JavaScript exibe uma Modal Messagebox do lado do cliente](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Figura 1**: O JavaScript `confirm(string)` método exibe uma caixa de mensagem Modal, no lado do cliente


Durante o envio de um formulário, se o valor de `false` é retornado de um manipulador de eventos do lado do cliente, em seguida, o envio do formulário é cancelado. Usando esse recurso, podemos ter exclusão botão s lado do cliente `onclick` manipulador de eventos retornar o valor de uma chamada para `confirm("Are you sure you want to delete this product?")`. Se o usuário clicar em Cancelar, `confirm(string)` retornará false e, portanto, fazendo com que o envio do formulário Cancelar. Com nenhum postback, o produto cuja exclusão foi clicada ganhou um t ser excluído. Se, no entanto, o usuário clica Okey na caixa de diálogo de confirmação, o postback continua sem interrupções e o produto será excluído. Consultar [s usando JavaScript `confirm()` método de envio do formulário de controle](http://www.webreference.com/programming/javascript/confirm/) para obter mais informações sobre essa técnica.

Adicionar o script do lado do cliente necessário um pouco diferente se usando modelos que ao usar um CommandField. Portanto, neste tutorial vamos examinar um FormView e GridView de exemplo.

> [!NOTE]
> Usando técnicas de confirmação do lado do cliente, como aquelas discutidas neste tutorial pressupõe que seus usuários estão visitando com navegadores que oferecem suporte a JavaScript e que tenham JavaScript habilitado. Se qualquer uma dessas suposições não forem verdadeira para um usuário específico, clicando no botão Excluir imediatamente causará um postback (não exibir uma messagebox de confirmar).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Etapa 1: Criando um FormView que dá suporte à exclusão

Comece adicionando um FormView para o `ConfirmationOnDelete.aspx` página no `EditInsertDelete` pasta, associação a um novo ObjectDataSource que extrai as informações do produto por meio de `ProductsBLL` classe s `GetProducts()` método. Também configurar o ObjectDataSource para que o `ProductsBLL` classe s `DeleteProduct(productID)` método é mapeado para o s ObjectDataSource `Delete()` método; Certifique-se de que as guias INSERT e UPDATE listas suspensas são definidas como (nenhum). Por fim, marque a caixa de seleção Habilitar paginação na marca inteligente s FormView.

Após essas etapas, o novo ObjectDataSource s marcação declarativa ficará semelhante ao seguinte:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Como em nossos exemplos anteriores que não usou a simultaneidade otimista, reserve um tempo para limpar o s ObjectDataSource `OldValuesParameterFormatString` propriedade.

Uma vez que ele foi associado a um controle ObjectDataSource que dá suporte somente a exclusão, o s FormView `ItemTemplate` oferece apenas o botão de exclusão, que não tem os botões novo e atualização. No entanto, FormView s marcação declarativa, inclui um supérfluo `EditItemTemplate` e `InsertItemTemplate`, que pode ser removido. Dedique uns momentos para personalizar o `ItemTemplate` é assim que mostra os campos de dados de apenas um subconjunto do produto. Eu ve configurado meu para mostrar o nome do produto s em um `<h3>` título acima seus nomes de categoria e fornecedor (juntamente com o botão Excluir).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Com essas alterações, temos uma página da web totalmente funcional que permite que um usuário alternar entre os produtos de uma vez, com a capacidade de excluir um produto, simplesmente clicando no botão Excluir. Figura 2 mostra uma captura de tela de nosso progresso até o momento quando visualizado por meio de um navegador.


[![FormView mostra informações sobre um único produto](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Figura 2**: O FormView mostra informações sobre um único produto ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Etapa 2: Chamando a função confirm(string) da excluir botões onclick de cliente eventos

FormView criado, a etapa final é configurar o botão excluir tais que, quando ele s clicado pelo visitante, o JavaScript `confirm(string)` função é invocada. Adicionar script do lado do cliente a um botão, LinkButton ou ImageButton s lado do cliente `onclick` evento pode ser feito por meio do uso do `OnClientClick property`, que é nova para o ASP.NET 2.0. Como queremos ter o valor da `confirm(string)` simplesmente retornado da função, defina essa propriedade como: `return confirm('Are you certain that you want to delete this product?');`

Após essa alteração a sintaxe declarativa do LinkButton excluir s deve ser semelhante:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Tudo que s é a ele! Figura 3 mostra uma captura de tela dessa confirmação em ação. Clicando no botão Excluir abre a caixa de diálogo Confirmar. Se o usuário clicar em Cancelar, o postback será cancelado e o produto não é excluído. Se, no entanto, o usuário clica Okey, o postback continua e o s ObjectDataSource `Delete()` método é invocado, culminando no registro de banco de dados que está sendo excluído.

> [!NOTE]
> A cadeia de caracteres passada para o `confirm(string)` função JavaScript é delimitada com apóstrofos (em vez de aspas). No JavaScript, cadeias de caracteres podem ser delimitadas usando qualquer um dos caracteres. Usamos apóstrofos aqui para que os delimitadores para a cadeia de caracteres passada para `confirm(string)` não introduzem ambiguidade com os delimitadores usados para o `OnClientClick` valor da propriedade.


[![Uma confirmação é agora exibido ao clicar no botão Excluir](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Figura 3**: A confirmação é agora exibido ao clicar no botão Excluir ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Etapa 3: Configurando a propriedade OnClientClick do botão Delete em um CommandField

Ao trabalhar com um botão, LinkButton ou ImageButton diretamente em um modelo, uma caixa de diálogo de confirmação pode ser associada a ele simplesmente configurando seus `OnClientClick` propriedade para retornar os resultados do JavaScript `confirm(string)` função. No entanto, o CommandField - que adiciona um campo de botões de exclusão a um GridView ou DetailsView - não tem um `OnClientClick` propriedade que pode ser definida declarativamente. Em vez disso, podemos deve referenciar programaticamente no botão Excluir em s GridView ou DetailsView apropriado `DataBound` manipulador de eventos e defina seu `OnClientClick` propriedade existe.

> [!NOTE]
> Ao definir o botão excluir s `OnClientClick` propriedade no respectivo `DataBound` manipulador de eventos, temos acesso a dados foi associados ao registro atual. Isso significa que podemos estender a mensagem de confirmação para incluir detalhes sobre o registro específico, como "Tem certeza de que deseja excluir o produto Chai?" Tal personalização também é possível em modelos usando a sintaxe de associação de dados.


A configuração de prática de `OnClientClick` propriedade para a exclusão correspondentes em um CommandField, s permitem que adicione um controle GridView à página. Configure este GridView para usar o controle ObjectDataSource mesmo que usa o FormView. Também limite o s GridView BoundFields para incluir apenas o nome do produto s, categoria e fornecedor. Por fim, marque a caixa de seleção Habilitar exclusão da marca inteligente s GridView. Isso adicionará um CommandField s o GridView `Columns` coleção com seus `ShowDeleteButton` propriedade definida como `true`.

Depois de fazer essas alterações, sua marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

O CommandField contém uma única instância de excluir LinkButton que pode ser acessada por meio de programação de s GridView `RowDataBound` manipulador de eventos. Quando referenciado, podemos definir seu `OnClientClick` propriedade adequadamente. Crie um manipulador de eventos para o `RowDataBound` eventos usando o seguinte código:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Esse manipulador de eventos funciona com linhas de dados (aquelas que terá o botão de exclusão) e começa fazendo referência programaticamente no botão Excluir. Em geral, use o seguinte padrão:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* é o tipo de botão que está sendo usado por CommandField - Button, LinkButton ou ImageButton. Por padrão, o CommandField usa LinkButtons, mas isso pode ser personalizado por meio de s CommandField `ButtonType property`. O *commandFieldIndex* é o índice ordinal do CommandField dentro de s GridView `Columns` coleção, enquanto o *controlIndex* é o índice do botão Delete dentro de s CommandField `Controls` coleção. O *controlIndex* valor depende da posição do botão s em relação a outros botões no CommandField. Por exemplo, se o único botão exibido no CommandField for o botão de exclusão, use um índice de 0. Se, no entanto, há s um botão Editar que precede o botão Excluir, use um índice de 2. O motivo pelo qual um índice de 2 é usado é porque os dois controles são adicionados pelo CommandField antes do botão de exclusão: o botão de edição e uma LiteralControl que s usada para adicionar algum espaço entre os botões Editar e excluir.

Para nosso exemplo específico, o CommandField usa LinkButtons e, sendo o campo mais à esquerda, tem um *commandFieldIndex* igual a 0. Como não há nenhum outro botão, mas o botão Excluir no CommandField, usamos uma *controlIndex* igual a 0.

Depois de referenciar o botão Excluir no CommandField, em seguida capturamos informações sobre o produto associado à linha atual do GridView. Finalmente, definimos o botão excluir s `OnClientClick` propriedade para o JavaScript apropriado, o que inclui o nome do produto s. Uma vez que a cadeia de caracteres de JavaScript é passado para o `confirm(string)` função é delimitada usando apóstrofos, precisa fazer o escape de todos os apóstrofos que aparecem dentro do nome do produto s. Em particular, todos os apóstrofos no nome do produto s serão ignorados com "`\'`".

Com essas alterações completas, clicando em um botão Excluir no GridView mostre uma caixa de diálogo de confirmação personalizado caixa (veja a Figura 4). Como com messagebox confirmação de FormView, se o usuário clicar em Cancelar o postback é cancelado, impedindo assim a exclusão ocorra.

> [!NOTE]
> Essa técnica também pode ser usada para acessar programaticamente o botão Excluir no CommandField em um DetailsView. Para DetailsView, no entanto, você d cria um manipulador de eventos para o `DataBound` evento, como DetailsView não tem um `RowDataBound` eventos.


[![Clicar no botão de exclusão de s GridView exibe uma caixa de diálogo de confirmação personalizado](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Figura 4**: clicar o botão de exclusão do s GridView exibe uma caixa de diálogo de confirmação personalizado ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>Usando TemplateFields

Uma das desvantagens do CommandField é que seus botões devem ser acessados por meio de indexação e que o objeto resultante deve ser convertido para o tipo de botão apropriado (Button, LinkButton ou ImageButton). Usando tipos de embutido em código e "números mágicos" convida problemas que não podem ser descobertos até o tempo de execução. Por exemplo, se você ou outro desenvolvedor, adiciona novos botões a CommandField em algum momento no futuro (como um botão Editar) ou alterações a `ButtonType` propriedade, o código existente ainda será compilado sem erros, mas visitando a página pode causar uma exceção ou comportamento inesperado, dependendo de como seu código foi escrito e quais alterações foram feitas.

Uma abordagem alternativa é converter o GridView e DetailsView s CommandFields em TemplateFields. Isso irá gerar um TemplateField com um `ItemTemplate` que tem um LinkButton (ou botão ou ImageButton) para cada botão no CommandField. Esses botões `OnClientClick` propriedades podem ser atribuídas de forma declarativa, conforme é visto com FormView ou podem ser acessados por meio de programação no respectivo `DataBound` manipulador de eventos usando o seguinte padrão:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Em que *controlID* é o valor do botão s `ID` propriedade. Embora esse padrão ainda requer um tipo embutido em código para a conversão, ele elimina a necessidade de indexação, permitindo o layout alterar sem provocar um erro de tempo de execução.

## <a name="summary"></a>Resumo

O JavaScript `confirm(string)` função é uma técnica normalmente usada para controlar fluxo de trabalho de envio de formulário. Quando executada, a função exibe uma caixa de diálogo modal, no lado do cliente que inclui dois botões, Okey e Cancelar. Se o usuário clica em Okey, o `confirm(string)` retornos de função `true`; clique em Cancel retorna `false`. Essa funcionalidade, juntamente com um comportamento do navegador s para cancelar o envio de um formulário se retorna um manipulador de eventos durante o processo de envio `false`, pode ser usado para exibir uma caixa de mensagem de confirmação ao excluir um registro.

O `confirm(string)` função pode ser associada um cliente da Web de botão controle s lado `onclick` manipulador de eventos por meio do controle s `OnClientClick` propriedade. Ao trabalhar com um botão Excluir em um modelo: em um dos modelos FormView s ou em um TemplateField no DetailsView ou GridView - essa propriedade pode ser definida ou declarativamente por meio de programação, como vimos neste tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](implementing-optimistic-concurrency-vb.md)
> [Próximo](limiting-data-modification-functionality-based-on-the-user-vb.md)
