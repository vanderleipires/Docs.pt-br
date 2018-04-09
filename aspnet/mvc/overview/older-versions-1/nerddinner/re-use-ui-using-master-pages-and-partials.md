---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Use novamente a interface do usuário usando páginas mestras e existe meio-termo | Microsoft Docs
author: microsoft
description: Etapa 7 analisa modos, que é possível aplicar 'Princípio SECA' em nossos modelos de exibição para eliminar a duplicação de código, usando modelos de exibição parcial e páginas mestras.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Reutilizar usando páginas mestras e existe meio-termo de interface do usuário
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 7 de livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 7 examina maneiras que podemos aplicar o "princípio SECA" em nossos modelos de exibição para eliminar a duplicação de código, usando modelos de exibição parcial e páginas mestras.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner etapa 7: Existe meio-termo e páginas mestras

Um das filosofias design que adota o ASP.NET MVC é o princípio "Faça não repetitivo" (conhecido como "Teste"). Um seco design ajuda a eliminar a duplicação de código e lógica, que, por fim, faz com que aplicativos mais rápidos para criar e mais fáceis de manter.

Já vimos o princípio seco aplicado em vários cenários nosso NerdDinner. Alguns exemplos: nossa lógica de validação é implementada em nossa camada de modelo, que permite que ele seja imposta em ambos os editar e criar cenários no nosso controlador; Estamos novamente usando o modelo de exibição de "NotFound" entre os métodos de ação de edição, detalhes e Delete Estamos usando um padrão de nomeação convenção - com os nossos modelos de exibição, o que elimina a necessidade de especificar explicitamente o nome quando podemos chamar o método auxiliar de View(); e podemos estiver novamente usando a classe DinnerFormViewModel para ambos os editar e criar cenários de ação.

Agora vamos examinar maneiras que podemos aplicar o "princípio SECA" em nossos modelos de exibição para eliminar a duplicação de código lá também.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Novamente, visite nosso editar e criar modelos de exibição

Atualmente estamos usando dois modelos de exibição diferente – "Edit.aspx" e "Create.aspx" – para exibir o nosso formulário de uma refeição da interface do usuário. Uma comparação visual rápida deles realça o grau de semelhança. Abaixo está a aparência do formulário de criação:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

E aqui está a aparência de nosso formulário de "Editar":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Não há muito a diferença é? Além do texto de título e o cabeçalho, os controles de layout e a entrada de formulário são idênticos.

Se abrir o "Edit.aspx" e "Create.aspx" Exibir modelos que descobriremos que elas contêm o código de controle de layout e a entrada de forma idêntica. Essa duplicação significa que podemos acabar tendo fazer alterações de duas vezes a qualquer momento, podemos apresentar ou alterar uma nova propriedade de uma refeição - não é válida.

### <a name="using-partial-view-templates"></a>Usando modelos de exibição parcial

ASP.NET MVC suporta a capacidade de definir modelos de "exibição parcial" que podem ser usados para encapsular a lógica de processamento de modo de exibição para uma subdiretório parte de uma página. "Existe meio-termo" fornece uma maneira útil para definir o modo de exibição lógica de processamento de uma vez e, em seguida, usá-lo novamente em vários locais em um aplicativo.

Para ajudar a "Simulação-up" nosso Edit.aspx e eliminação de duplicação de modelo de exibição Create.aspx, podemos criar um modelo de exibição parcial chamado "DinnerForm.ascx" que encapsula o layout de formulário e comuns em elementos de entrada. Faremos isso clicando duas vezes em nosso/exibições/jantares diretório e escolhendo a "Add -&gt;exibição" comando de menu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Isso exibirá a caixa de diálogo "Adicionar modo de exibição". Chamaremos o novo modo de exibição que deseja criar "DinnerForm", selecione a caixa de seleção "Criar uma exibição parcial" na caixa de diálogo e indicar que será passado ele uma classe DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando, clique no botão "Adicionar", o Visual Studio criará um novo modelo de exibição "DinnerForm.ascx" para que possamos dentro do diretório "\Views\Dinners".

Podemos pode, em seguida, copie/cole o layout do formulário duplicados / código de controle de nossos Edit.aspx/ Create.aspx exibir modelos de entrada em nosso novo modelo de exibição parcial "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Em seguida, podemos atualizar nosso editar e criar modelos de exibição para chamar o modelo parcial DinnerForm e eliminar a duplicação de formulário. Podemos fazer isso por chamada Html.RenderPartial("DinnerForm") em nossos modelos de exibição:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Você pode qualificar explicitamente o caminho do modelo parcial desejado ao chamar Html.RenderPartial (por exemplo: ~ Views/Dinners/DinnerForm.ascx "). Em nosso código acima, embora, estamos aproveitando o recurso baseado em convenção padrão de nomenclatura no ASP.NET MVC e apenas especificar "DinnerForm" como o nome da parte para renderizar. Quando fazemos isso ASP.NET MVC procurará primeiro no diretório baseado em convenção exibições (DinnersController seria/exibições/jantares). Se não encontrar o modelo parcial existe-lo, em seguida, procurará por ele no diretório /Views/Shared.

Quando Html.RenderPartial() é chamado com apenas o nome da exibição parcial, ASP.NET MVC passará para o modo de exibição parcial os mesmo modelo e ViewData dicionário os objetos usados pelo modelo de exibição de chamada. Como alternativa, há versões sobrecarregadas do Html.RenderPartial() que permitem que você passe um objeto de modelo alternativo e/ou dicionário ViewData para a exibição parcial a ser usado. Isso é útil para situações em que você deseja apenas passar um subconjunto do modelo completo/ViewModel.

| **Tópico de lado: Por que &lt;% %&gt; em vez de &lt;% = %&gt;?** |
| --- |
| Uma das coisas sutis que você deve ter notado com o código acima é que estamos usando um &lt;% %&gt; bloquear em vez de um &lt;% = %&gt; bloquear ao chamar Html.RenderPartial(). &lt;% = %&gt; blocos no ASP.NET indicam que um desenvolvedor deseja renderizar um valor especificado (por exemplo: &lt;% = "Hello" %&gt; geraria "Olá"). &lt;% %&gt; blocos em vez disso, indicam que o desenvolvedor quer executar o código, e que qualquer saída dentro deles renderizada deve ser feita explicitamente (por exemplo: &lt;Response.Write("Hello") %&gt;. O motivo pelo qual estamos usando um &lt;% %&gt; bloco com nosso código Html.RenderPartial acima é porque o método de Html.RenderPartial() não retorna uma cadeia de caracteres e, em vez disso gera o conteúdo diretamente para o modelo de exibição de chamada do fluxo de saída. Ele faz isso por motivos de eficiência de desempenho e isso evita a necessidade de criar um objeto de cadeia de caracteres temporária (potencialmente grande). Isso reduz o uso de memória e melhora a taxa de transferência geral do aplicativo. Um erro comum ao usar Html.RenderPartial() é esqueceu de adicionar um ponto e vírgula no final da chamada quando ele estiver em um &lt;% %&gt; bloco. Por exemplo, esse código causará um erro do compilador: &lt;Html.RenderPartial("DinnerForm") %&gt; em vez disso, você precisa gravar: &lt;% % Html.RenderPartial("DinnerForm");&gt; isso ocorre porque &lt;% %&gt; blocos são instruções de código independente e ao usar o código c# instruções precisam ser encerrada com um ponto e vírgula. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Usando modelos de exibição parcial para esclarecer o código

Nós criamos o modelo de exibição parcial "DinnerForm" para evitar a duplicação da lógica de processamento de modo de exibição em vários lugares. Este é o motivo mais comum para criar modelos de exibição parcial.

Às vezes, ele ainda faz sentido para criar exibições parciais, mesmo quando só estão sendo chamados em um único lugar. Modelos de exibição muito complicado se tornam muito mais fácil de ler quando sua lógica de processamento de modo de exibição é extraída e particionada em um ou mais bem nomeada modelos parciais.

Por exemplo, considere o trecho de código do arquivo Site.master em nosso projeto (o que podemos verá em breve) abaixo. O código é relativamente simples para ler – em parte porque a lógica para exibir um logon/logoff link na parte superior direita da tela é encapsulada em parciais de "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Sempre que você encontrará confuso tentar entender a marcação html/código dentro de um modelo de exibição, considere se ele não seria mais claro se algumas delas foi extraída e refatorado em exibições parciais bem nomeadas.

### <a name="master-pages"></a>Páginas mestras

Além de oferecer suporte a exibições parciais, o ASP.NET MVC também suporta a capacidade de criar modelos de "página mestra" que podem ser usados para definir o layout comum e html de um site de nível superior. Espaço reservado para controles, em seguida, podem ser adicionados para a página mestra para identificar regiões substituíveis que podem ser substituídos ou "preenchidos" por modos de exibição de conteúdo. Isso fornece uma maneira muito eficiente (e SECA) para aplicar um layout comum em um aplicativo.

Por padrão, os novos projetos do ASP.NET MVC têm um modelo de página mestra automaticamente adicionado a eles. Essa página mestra é chamada de "Site.master" e reside na pasta \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

O arquivo de Site.master padrão parece abaixo. Ele define o html externo do site, junto com um menu de navegação na parte superior. Ele contém dois controles de espaço reservado para conteúdo substituível – um para o título e o outro para onde o conteúdo principal de uma página deve ser substituído:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todos os modelos de exibição que criamos para nosso aplicativo NerdDinner ("List", "Detalhes", "Editar", "Criar", "NotFound", etc) baseados neste modelo Site.master. Isso é indicado por meio do atributo "MasterPageFile" que foi adicionado por padrão para a parte superior &lt;% @ Page %&gt; diretiva quando criamos nossos modos de exibição usando a caixa de diálogo "Adicionar modo de exibição":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Isso significa que podemos alterar o conteúdo de Site.master e tem as alterações automaticamente aplicadas e usadas quando podemos processar qualquer um dos nossos modelos de exibição.

Vamos atualizar a seção de cabeçalho do nosso Site.master para que o cabeçalho de nosso aplicativo é "NerdDinner" em vez de "Meu aplicativo MVC". Vamos também atualizar nosso menu de navegação para que a primeira guia é "Localizar uma refeição" (tratado pelo método de ação do HomeController Index ()) e vamos adicionar uma nova guia chamada "Host uma refeição" (tratado pelo método de ação do DinnersController Create ()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Depois de salvar o arquivo Site.master e atualizar nosso navegador veremos nosso cabeçalho alterações mostrar até em todas as exibições em nosso aplicativo. Por exemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E com o */Dinners/Editar / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Próxima etapa

Existe meio-termo e páginas mestras fornecem opções muito flexíveis que permitem organizar exibições. Você encontrará que eles ajudam a evitar a duplicação da exibição de conteúdo / código e fazer seus modelos de modo mais fácil de ler e manter.

Vamos agora revisitar o cenário de listagem que criamos anteriormente e habilitar o suporte escalável de paginação.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Próximo](implement-efficient-data-paging.md)
