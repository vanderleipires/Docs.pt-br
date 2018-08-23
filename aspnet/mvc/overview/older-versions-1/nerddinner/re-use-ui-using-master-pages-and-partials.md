---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Reutilizar interface do usuário usando páginas mestras e parciais | Microsoft Docs
author: microsoft
description: Etapa 7 examina maneiras que podemos aplicar o "princípio de DRY em nossos modelos de exibição para eliminar a duplicação de código, usando as páginas mestras e modelos de exibição parcial.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0da32e6ac38f10df6e581517989b3b1fd2f2328c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825090"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Reutilizar interface do usuário usando páginas mestras e parciais
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 7 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 7 examina de maneiras que podemos aplicar o "princípio de DRY" dentro de nossos modelos de exibição para eliminar a duplicação de código, usando as páginas mestras e modelos de exibição parcial.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>Etapa 7 do NerdDinner: Parciais e páginas mestras

Um das filosofias de design que adota o ASP.NET MVC é o princípio "Fazer não Repeat Yourself" (normalmente chamado de "Simulação"). Um design DRY ajuda a eliminar a duplicação de código e lógica, que, por fim, faz com que aplicativos mais rápidos de build e mais fácil de manter.

Já vimos o princípio DRY aplicado em vários dos nossos cenários NerdDinner. Alguns exemplos: nossa lógica de validação é implementada em nossa camada de modelo, que permite que ele ser imposta em ambos os editar e criar cenários em nosso controller; Estamos novamente usando o modelo de exibição de "NotFound" entre os métodos de ação Editar, detalhes e exclusão Estamos usando um padrão de nomenclatura convenção - com nossos modelos de exibição, o que elimina a necessidade de especificar explicitamente o nome quando chamamos o método auxiliar de View(); e podemos novamente estiver usando a classe DinnerFormViewModel para ambos os editar e criar cenários de ação.

Agora vamos analisar maneiras que podemos aplicar o "princípio de DRY" dentro de nossos modelos de exibição para eliminar a duplicação de código lá também.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Reveja nossa editar e criar modelos de exibição

No momento, estamos usando dois modelos de exibição diferente – "Edit" e "Aspx –" para exibir nosso formulário de Dinner da interface do usuário. Uma comparação visual rápida deles destaca como eles são similares. Abaixo está o que o formulário Criar se parece com:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

E aqui está a aparência de nosso formulário de "Editar":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Não muita diferença existe? Além de texto Título e o cabeçalho, os controles de entrada e o layout do formulário são idênticos.

Se abrimos o "Edit" e "Aspx" modelos de exibição que descobriremos que eles contêm código de controle de layout e a entrada de forma idêntica. Essa duplicação significa que podemos acabar tendo façam alterações duas vezes a qualquer momento podemos introduzir ou alterar uma nova propriedade de jantar - não é bom.

### <a name="using-partial-view-templates"></a>Usando modelos de exibição parcial

ASP.NET MVC suporta a capacidade para definir modelos de "modo de exibição parcial" que podem ser usados para encapsular a lógica de renderização do modo de exibição para uma parte de subpropriedades de uma página. "Parciais" fornecem uma maneira útil para definir o modo de exibição lógica de processamento de uma vez e, em seguida, usá-lo novamente em vários lugares através de um aplicativo.

Para ajudar a "DRY-up" nosso Edit e a eliminação de duplicação do modelo de exibição aspx, podemos criar um modelo de exibição parcial chamado "DinnerForm.ascx" que encapsula o layout do formulário e elementos de entrada comuns a ambos. Vamos fazer isso clicando duas vezes em nosso diretório jantares/Views/e escolhendo a "Add -&gt;exibição" comando de menu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Isso exibirá a caixa de diálogo "Adicionar exibição". Nomear a nova exibição queremos criar "DinnerForm", marque a caixa de seleção "Criar uma exibição parcial" na caixa de diálogo e indicar que será passamos a ela uma classe DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando clicamos no botão "Adicionar", o Visual Studio criará um novo modelo de exibição de "DinnerForm.ascx" para que possamos dentro do diretório "\Views\Dinners".

Podemos pode, em seguida, copiar/colar o layout do formulário duplicados / entrada de controle de código de nossos modelos de exibição Edit.aspx/ aspx em nosso novo modelo de exibição parcial de "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Em seguida, podemos atualizar nossos modelos de exibição editar e criar para chamar o modelo parcial de DinnerForm e eliminar a duplicação do formulário. Podemos fazer isso por chamada Html.RenderPartial("DinnerForm") dentro de nossos modelos de exibição:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Você pode qualificar explicitamente o caminho do modelo parcial que deseja ao chamar Html.RenderPartial (por exemplo: ~ Views/Dinners/DinnerForm.ascx "). Em nosso código acima, no entanto, estamos aproveitando o padrão de nomenclatura baseada em convenção no ASP.NET MVC e apenas especificando "DinnerForm" como o nome da parcial para renderizar. Quando fazemos isso ASP.NET MVC procurará primeiro no diretório visualizações baseado em convenção (DinnersController isso seria/Views/jantares). Se não encontrar o modelo parcial existe-lo, em seguida, procurará por ele no diretório /Views/Shared.

Quando Html.RenderPartial() for chamado com apenas o nome da exibição parcial, ASP.NET MVC passará para a exibição parcial os mesmo modelo e ViewData objetos de dicionário usados pelo modelo de exibição de chamada. Como alternativa, há versões sobrecarregadas do Html.RenderPartial() que permitem que você passe um objeto de modelo alternativo e/ou o dicionário ViewData da exibição parcial usar. Isso é útil para cenários em que você deseja apenas passar um subconjunto do modelo completo/ViewModel.

| **Tópico de lado: Por que &lt;% %&gt; em vez de &lt;% = %&gt;?** |
| --- |
| Uma das coisas sutis que você talvez tenha percebido com o código acima é que estamos usando um &lt;% %&gt; bloquear em vez de uma &lt;% = %&gt; bloquear ao chamar Html.RenderPartial(). &lt;% = %&gt; blocos no ASP.NET indicam que um desenvolvedor deseja renderizar um valor especificado (por exemplo: &lt;% = "Hello" %&gt; seriam renderizadas "Olá"). &lt;% %&gt; blocos em vez disso, indicam que o desenvolvedor deseja executar o código, e que qualquer renderizado saída dentro deles deve ser feito explicitamente (por exemplo: &lt;Response.Write("Hello") %&gt;. O motivo pelo qual estamos usando um &lt;% %&gt; bloco com nosso código Html.RenderPartial acima é porque o método Html.RenderPartial() não retorna uma cadeia de caracteres e, em vez disso, gera o conteúdo diretamente para o modelo de exibição de chamada do fluxo de saída. Ele faz isso por motivos de eficiência de desempenho e, ao fazer isso, ele evita a necessidade de criar um objeto de cadeia de caracteres temporária (potencialmente muito grande). Isso reduz o uso de memória e melhora a taxa de transferência geral do aplicativo. Um erro comum ao usar Html.RenderPartial() é se esqueça de adicionar uma ponto e vírgula no final da chamada, quando ele estiver em um &lt;% %&gt; bloco. Por exemplo, esse código fará com que um erro do compilador: &lt;% Html.RenderPartial("DinnerForm")&gt; em vez disso, você precisa escrever: &lt;% Html.RenderPartial("DinnerForm"); %&gt; isso ocorre porque &lt;% %&gt; blocos são instruções de código independente e ao usar o código c# instruções precisam ser terminada com ponto e vírgula. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Usando os modelos de exibição parcial para esclarecer o código

Criamos o modelo de exibição parcial de "DinnerForm" para evitar a duplicação de lógica de renderização do modo de exibição em vários lugares. Isso é o motivo mais comum para criar modelos de exibição parcial.

Às vezes, ele ainda faz sentido para criar exibições parciais, mesmo quando eles estão sendo chamados somente em um único lugar. Modelos de exibição muito complicado se tornam muito mais fácil de ler quando sua lógica de renderização do modo de exibição é extraída e particionada em uma ou mais bem chamada modelos parciais.

Por exemplo, considere o abaixo de trecho de código do arquivo master em nosso projeto (o que podemos verá em breve). O código é relativamente simples para ler – em parte porque a lógica para exibir um logon/logoff link na parte superior direita da tela é encapsulada em parcial "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Sempre que você se encontrar confuso tentando entender a marcação de html/código dentro de um modelo de exibição, considere se ele não seria mais claro se algumas delas foi extraído e refatorado em exibições parciais bem nomeadas.

### <a name="master-pages"></a>Páginas mestras

Além de dar suporte a exibições parciais, ASP.NET MVC também suporta a capacidade de criar modelos de "página mestra" que podem ser usados para definir o layout comum e o html de um site de nível superior. Espaço reservado para controles, em seguida, podem ser adicionados à página mestra para identificar regiões substituíveis que podem ser substituídos ou "preencher os" modos de exibição de conteúdo. Isso fornece uma maneira muito eficiente (e SECA) para aplicar um layout comum em um aplicativo.

Por padrão, os novos projetos do ASP.NET MVC têm um modelo de página mestra automaticamente adicionado a eles. Essa página mestra é chamada de "Master" e reside na pasta \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

O arquivo site Master padrão a aparência abaixo. Ele define o html externo do site, junto com um menu de navegação na parte superior. Ele contém dois controles de espaço reservado para conteúdo substituíveis – um para o título e o outro para onde o conteúdo principal de uma página deve ser substituído:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todos os modelos de exibição que criamos nosso aplicativo NerdDinner ("List", "Detalhes", "Editar", "Criar", "NotFound", etc) tem foi baseados nesse modelo de site. Isso é indicado por meio do atributo "MasterPageFile" que foi adicionado por padrão na parte superior &lt;% @ Page %&gt; diretiva quando criamos nossos modos de exibição usando a caixa de diálogo "Adicionar exibição":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Isso significa que podemos alterar o conteúdo do site, e tem as alterações automaticamente ser aplicadas e usadas quando os renderizamos qualquer um dos nossos modelos de exibição.

Vamos atualizar a seção de cabeçalho do nosso site para que o cabeçalho do nosso aplicativo é "NerdDinner" em vez de "Meu aplicativo MVC". Vamos também atualizar nosso menu de navegação para que a primeira guia é "Encontrar um jantar" (manipulado pelo método de ação do HomeController Index ()) e vamos adicionar uma nova guia chamada "Host de jantar" (manipulado pelo método de ação do DinnersController Create ()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Depois de salvar o arquivo site Master e atualizar nosso navegador veremos nosso cabeçalho alterações aparecerem em todas as exibições dentro de nosso aplicativo. Por exemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E com o */Dinners/Editar / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Próxima etapa

Parciais e páginas mestras fornecem opções muito flexíveis que permitem organizar os modos de exibição. Você encontrará o que eles ajudam a evitar a duplicação de modo de exibição de conteúdo / de código e tornam seus modelos de exibição mais fácil de ler e manter.

Vamos agora revisitar o cenário de listagem que criamos anteriormente e habilitar o suporte de paginação escalonável.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Próximo](implement-efficient-data-paging.md)
