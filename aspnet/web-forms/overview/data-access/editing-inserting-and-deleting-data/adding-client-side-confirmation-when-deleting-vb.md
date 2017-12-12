---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: "Adicionar confirmação do lado do cliente ao excluir (VB) | Microsoft Docs"
author: rick-anderson
description: "As interfaces que criamos até agora, um usuário pode excluir acidentalmente dados clicando no botão Excluir quando eles devem clicar no botão Editar. Neste t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 3461f9ec4f139f1ea0e60a01b898e67e7ebd7f54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>Adicionar confirmação do lado do cliente ao excluir (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) ou [baixar PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> As interfaces que criamos até agora, um usuário pode excluir acidentalmente dados clicando no botão Excluir quando eles devem clicar no botão Editar. Neste tutorial, adicionaremos uma caixa de diálogo de confirmação do lado do cliente que é exibida quando o botão de exclusão é clicado.


## <a name="introduction"></a>Introdução

Sobre os últimos tutoriais vários nós ve visto como usar nosso arquitetura do aplicativo, ObjectDataSource e os dados de controles da Web em conjunto para fornecer inserindo, editar e excluir recursos. A exclusão de interfaces ve examinado até agora foram composto de uma exclusão botão que, quando clicado, causa um postback e invoca o s ObjectDataSource `Delete()` método. O `Delete()` método, em seguida, invoca o método configurado da camada de lógica de negócios, que se propaga a chamada para a camada de acesso de dados, emissão real `DELETE` instrução no banco de dados.

Enquanto essa interface do usuário permite que os visitantes excluir registros através dos controles GridView, DetailsView ou FormView, ele não tem qualquer tipo de confirmação quando o usuário clica no botão Excluir. Se um usuário acidentalmente clicar no botão Excluir quando eles devem clicar em Editar, o registro, que eles devem atualizar em vez disso, será excluído. Para ajudar a evitar isso, neste tutorial, que vamos adicionar uma caixa de diálogo de confirmação do lado do cliente que é exibida quando o botão de exclusão é clicado.

O JavaScript `confirm(string)` função exibe seu parâmetro de entrada de cadeia de caracteres como o texto dentro de uma caixa de diálogo modal que vem equipado com dois botões - Okey e Cancelar (consulte a Figura 1). O `confirm(string)` função retorna um valor booliano, dependendo de qual botão é clicado (`true`, se o usuário clica em Okey, e `false` clicando em ' Cancelar ').


![O método de confirm(string) de JavaScript exibe um Modal Messagebox do lado do cliente](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Figura 1**: O JavaScript `confirm(string)` método exibe uma caixa de mensagem Modal, no lado do cliente


Durante o envio de um formulário, se um valor de `false` é retornado de um manipulador de eventos do lado do cliente e o envio do formulário é cancelado. Usando esse recurso, podemos ter Delete botão s lado do cliente `onclick` manipulador de eventos retorna o valor de uma chamada para `confirm("Are you sure you want to delete this product?")`. Se o usuário clicar em Cancelar, `confirm(string)` retornará false e, portanto, fazendo com que o envio do formulário Cancelar. Com nenhum postback, o produto cuja exclusão foi clicada ganha t ser excluído. Se, no entanto, o usuário clica Okey na caixa de diálogo de confirmação, a postagem irá continuar sem interrupções e o produto será excluído. Consulte [s usando JavaScript `confirm()` método de envio do formulário de controle](http://www.webreference.com/programming/javascript/confirm/) para obter mais informações sobre essa técnica.

Adicionar o script necessário do lado do cliente é ligeiramente diferente se usando modelos que ao usar um CommandField. Portanto, neste tutorial examinaremos exemplo um FormView e GridView.

> [!NOTE]
> Usando técnicas de confirmação do lado do cliente, como aquelas discutidas neste tutorial, pressupõe que seus usuários estão visitando com navegadores que oferecem suporte a JavaScript e que eles tenham JavaScript habilitado. Se qualquer uma nessas suposições não forem verdadeira para um usuário específico, clicando no botão Excluir imediatamente causará um postback (não exibir uma caixa de mensagem de confirmação).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Etapa 1: Criando um FormView que dá suporte à exclusão

Comece adicionando um FormView para o `ConfirmationOnDelete.aspx` página o `EditInsertDelete` pasta, associação a um novo ObjectDataSource que extrai as informações de produto por meio de `ProductsBLL` classe s `GetProducts()` método. Configure também o ObjectDataSource para que o `ProductsBLL` classe s `DeleteProduct(productID)` método é mapeado para o s ObjectDataSource `Delete()` método; Certifique-se de que as guias INSERT e UPDATE listas suspensas são definidas como (nenhum). Finalmente, verifique a caixa de seleção Habilitar paginação na marca inteligente s FormView.

Após estas etapas, o novo ObjectDataSource s declarativo será a seguinte aparência:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Como em nossos exemplos anteriores que não usou a simultaneidade otimista, reserve um tempo para limpar o s ObjectDataSource `OldValuesParameterFormatString` propriedade.

Desde que ele foi associado a um controle ObjectDataSource que só oferece suporte a exclusão, a s FormView `ItemTemplate` oferece apenas o botão de exclusão, sem os botões novo e atualização. O FormView s declarativo, no entanto, inclui uma supérfluas `EditItemTemplate` e `InsertItemTemplate`, que pode ser removido. Reserve um tempo para personalizar o `ItemTemplate` forma que ele mostra apenas um subconjunto do produto campos de dados. Eu ve configurado para mostrar o nome do produto s em busca de um `<h3>` título acima seus nomes de categoria e fornecedor (junto com o botão de exclusão).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Com essas alterações, temos uma página da web totalmente funcional que permite que um usuário alternar entre os produtos de uma vez, com a capacidade de excluir um produto, clicando no botão Excluir. A Figura 2 mostra uma captura de tela de nosso progresso até o momento quando visualizada através de um navegador.


[![O FormView mostra informações sobre um único produto](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Figura 2**: O FormView mostra informações sobre um único produto ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Etapa 2: Chamando a função confirm(string) da excluir botões cliente-onclick do lado do evento

O FormView criado, a etapa final é configurar o botão excluir tais que, quando ele s clicado pelo visitante, o JavaScript `confirm(string)` função é invocada. Adicionar script do lado do cliente para um botão, LinkButton ou ImageButton s cliente `onclick` evento pode ser feito por meio do uso do `OnClientClick property`, que é nova para o ASP.NET 2.0. Como queremos que tem o valor de `confirm(string)` simplesmente retornado de função, defini-la:`return confirm('Are you certain that you want to delete this product?');`

Após a alteração a sintaxe declarativa de s excluir LinkButton deve ser semelhante:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Tudo lá que s é a ele! A Figura 3 mostra uma captura de tela dessa confirmação em ação. Clicando no botão Excluir abre a caixa de diálogo Confirmar. Se o usuário clicar em Cancelar, a postagem será cancelada e o produto não é excluído. Se, no entanto, o usuário clica Okey, a postagem continuará e a s ObjectDataSource `Delete()` método é chamado, culminando no registro do banco de dados que está sendo excluído.

> [!NOTE]
> A cadeia de caracteres passada para o `confirm(string)` função JavaScript é delimitada por apóstrofos (em vez de aspas). Em JavaScript, cadeias de caracteres podem ser delimitadas usando qualquer caractere. Usamos apóstrofos aqui para que os delimitadores para a cadeia de caracteres passada para `confirm(string)` não apresenta uma ambiguidade com os delimitadores usados para o `OnClientClick` o valor da propriedade.


[![Uma confirmação é agora exibido ao clicar no botão Excluir](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Figura 3**: A confirmação é agora exibido ao clicar no botão Excluir ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Etapa 3: Configurando a propriedade OnClientClick para o botão de exclusão em um CommandField

Ao trabalhar com um botão, LinkButton ou ImageButton diretamente em um modelo, uma caixa de diálogo de confirmação pode ser associada ele, simplesmente configurando seu `OnClientClick` propriedade para retornar os resultados do JavaScript `confirm(string)` função. No entanto, o CommandField - que adiciona um campo de botões excluir um GridView ou DetailsView - não tem um `OnClientClick` propriedade que pode ser definida declarativamente. Em vez disso, podemos deve referenciar programaticamente no botão Excluir no s GridView ou DetailsView apropriado `DataBound` manipulador de eventos e defina seu `OnClientClick` propriedade existe.

> [!NOTE]
> Ao definir o botão excluir s `OnClientClick` propriedade no respectivo `DataBound` manipulador de eventos, temos acesso a dados foi associados ao registro atual. Isso significa que podemos pode estender a mensagem de confirmação para incluir detalhes sobre o registro específico, como "Tem certeza de que deseja excluir o produto Chai?" Essa personalização também é possível nos modelos usando a sintaxe de associação de dados.


A configuração de prática de `OnClientClick` propriedade correspondentes a exclusão em um CommandField, s permitem adicionar um controle GridView à página. Configure este GridView para usar o controle ObjectDataSource mesmo que usa o FormView. Também limite os s GridView BoundFields para incluir apenas o nome do produto s, categoria e fornecedor. Por fim, marque a caixa de seleção Habilitar a exclusão de marca inteligente s GridView. Isso adicionará uma CommandField s a GridView `Columns` coleção com seu `ShowDeleteButton` propriedade definida como `true`.

Depois de fazer essas alterações, a marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

O CommandField contém uma única instância de excluir LinkButton que pode ser acessada por meio de programação de GridView s `RowDataBound` manipulador de eventos. Depois de referência, podemos definir seu `OnClientClick` propriedade adequadamente. Criar um manipulador de eventos para o `RowDataBound` eventos usando o seguinte código:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Este manipulador de eventos funciona com linhas de dados (aquelas que terá o botão de exclusão) e começa consultando programaticamente o botão de exclusão. Em geral, use o seguinte padrão:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* é o tipo de botão que está sendo usado por CommandField - Button, LinkButton ou ImageButton. Por padrão, o CommandField usa botões de link, mas isso pode ser personalizado por meio da s CommandField `ButtonType property`. O *commandFieldIndex* é o índice ordinal do CommandField dentro de GridView s `Columns` coleção, enquanto o *controlIndex* é o índice do botão Excluir dentro a s CommandField `Controls` coleção. O *controlIndex* valor depende da posição do botão s em relação a outros botões no CommandField. Por exemplo, se o botão somente exibido no CommandField é o excluir, use um índice de 0. Se, no entanto, há s um botão de edição que precede o botão de exclusão, use um índice de 2. O motivo pelo qual um índice de 2 é usado é como os dois controles são adicionados por CommandField antes do botão de exclusão: o botão Editar e um LiteralControl que s usado para adicionar algum espaço entre os botões Editar e excluir.

Para nosso exemplo específico, o CommandField usa botões de link e, sendo o campo mais à esquerda, tem um *commandFieldIndex* 0. Como não há nenhum outro botão, mas no botão Excluir no CommandField, usamos um *controlIndex* 0.

Depois de fazer referência a no botão Excluir no CommandField, podemos lado capturar informações sobre o produto associado à linha atual de GridView. Finalmente, definimos o botão excluir s `OnClientClick` propriedade para o JavaScript apropriado, o que inclui o nome do produto s. Desde que a cadeia de caracteres JavaScript passado para o `confirm(string)` função é delimitada usando apóstrofos nós deve escapar qualquer apóstrofos que aparecem no nome do produto s. Em particular, qualquer apóstrofos no nome do produto s são ignorados com "`\'`".

Com essas alterações completas, clicando em um botão de exclusão no GridView mostre uma caixa de diálogo de confirmação personalizado caixa (veja a Figura 4). Como com o messagebox de confirmação de FormView, se o usuário clicar em Cancelar o postback é cancelado, impedindo que a exclusão ocorra.

> [!NOTE]
> Essa técnica também pode ser usada para acessar programaticamente o botão de exclusão em CommandField em um DetailsView. De DetailsView, no entanto, você d cria um manipulador de eventos para o `DataBound` evento, como o DetailsView não tem um `RowDataBound` eventos.


[![Clique no botão de exclusão de s GridView exibe uma caixa de diálogo de confirmação personalizado](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Figura 4**: clicar o botão de exclusão do s GridView exibe uma caixa de diálogo de confirmação personalizado ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>Usando TemplateFields

Uma das desvantagens do CommandField é que seus botões devem ser acessados por meio de indexação e se o objeto resultante deve ser convertido para o tipo apropriado de botão (botão, LinkButton ou ImageButton). Usando "magic números" e tipos embutidos convida problemas que não podem ser descobertos até o tempo de execução. Por exemplo, se você ou outro desenvolvedor, adiciona novos botões CommandField em algum momento no futuro (por exemplo, um botão de edição) ou alterações a `ButtonType` propriedade, o código existente continuará compilando sem erro, mas visitar a página pode causar uma exceção ou um comportamento inesperado, dependendo de como o código foi criado e quais alterações foram feitas.

Uma abordagem alternativa é converter a GridView e DetailsView s CommandFields em TemplateFields. Isso irá gerar um TemplateField com um `ItemTemplate` que possui um LinkButton (ou botão ou ImageButton) para cada botão de CommandField. Esses botões `OnClientClick` propriedades podem ser atribuídas declarativamente, como podemos viu FormView ou podem ser acessados por meio de programação no respectivo `DataBound` manipulador de eventos usando o seguinte padrão:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Onde *controlID* é o valor do botão s `ID` propriedade. Enquanto esse padrão ainda requer um tipo embutido para a conversão, ele remove a necessidade de indexação, permitindo o layout alterar sem resultando em um erro de tempo de execução.

## <a name="summary"></a>Resumo

O JavaScript `confirm(string)` função é uma técnica normalmente usada para controlar fluxo de trabalho de envio de formulário. Quando executado, a função exibirá uma caixa de diálogo modal, no lado do cliente que inclui dois botões Okey e Cancelar. Se o usuário clica em Okey, o `confirm(string)` função retorna `true`; clicar em Cancelar volta `false`. Essa funcionalidade, juntamente com um comportamento de navegador s para cancelar o envio de um formulário se um manipulador de eventos durante o processo de envio retorna `false`, pode ser usado para exibir uma caixa de mensagem de confirmação ao excluir um registro.

O `confirm(string)` função pode ser associada um botão Web controle s cliente `onclick` manipulador de eventos por meio do controle s `OnClientClick` propriedade. Ao trabalhar com um botão de exclusão em um modelo - o em um dos modelos FormView s ou em um TemplateField no DetailsView ou GridView - essa propriedade pode ser definida declarativamente ou por meio de programação, como visto neste tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](implementing-optimistic-concurrency-vb.md)
[Próximo](limiting-data-modification-functionality-based-on-the-user-vb.md)
