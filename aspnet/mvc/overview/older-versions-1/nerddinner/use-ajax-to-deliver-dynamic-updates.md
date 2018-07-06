---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar o AJAX para fornecer atualizações dinâmicas | Microsoft Docs
author: microsoft
description: Etapa 10 implementa suporte os usuários conectados ao RSVP seu interesse em participar de um jantar, usando uma abordagem baseada em Ajax integrada em detalhes o jantar...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825195"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Usar o AJAX para fornecer atualizações dinâmicas
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 10 de um livre [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 10 implementa suporte para os usuários conectados ao RSVP seu interesse em participar de um jantar, usando uma abordagem baseada em Ajax integrada dentro da página de detalhes do jantar.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner etapa 10: Aceita de AJAX habilitando RSVPs

Agora, vamos implementar o suporte para os usuários conectados ao RSVP seu interesse em participar de um jantar. Nós a Habilitaremos isso usando uma abordagem baseada em AJAX integrada dentro da página de detalhes do jantar.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Que indica se o usuário é RSVP'd

Os usuários podem visitar o */Dinners/detalhes / [id*] URL para ver detalhes sobre uma refeição específica:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

O Details() método de ação é implementado da seguinte forma:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Nossa primeira etapa para implementar o suporte RSVP será adicionar um método auxiliar de "IsUserRegistered(username)" ao nosso objeto de jantar (dentro da classe parcial do Dinner.cs que criamos anteriormente). Esse método auxiliar retorna true ou false dependendo se o usuário atualmente é RSVP'd do jantar:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Em seguida, podemos adicionar o código a seguir para nosso modelo de exibição details. aspx para exibir uma mensagem apropriada indicando se o usuário está registrado ou não para o evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

E agora quando um usuário visita um jantar, eles são registrados para ele verá esta mensagem:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

E quando eles visitam um jantar não estão registrados para que eles verão a mensagem abaixo:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementando o método de ação Register

Agora, vamos adicionar a funcionalidade necessária para permitir que os usuários confirme presença em um jantar da página de detalhes.

Para implementar isso, vamos criar uma nova classe "RSVPController" clicando duas vezes no diretório \Controllers e escolhendo Add -&gt;comando de menu do controlador.

Implementaremos um método de ação dentro da classe RSVPController novo que usa uma id em um jantar como um argumento, recupera o objeto apropriado de jantar, verifica se o usuário que fez logon estiver atualmente na lista de usuários que se registraram para ele e se "Registrar" não adiciona um objeto RSVP para eles:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Observe acima como estamos retornando uma cadeia de caracteres simple como a saída do método de ação. Podemos ter inserido esta mensagem dentro de um modelo de exibição – mas, pois ele é bem pequeno, usaremos apenas o método auxiliar Content() na classe base de controlador e retornar uma mensagem de cadeia de caracteres como acima.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Chamar o método de ação RSVPForEvent usando AJAX

Vamos usar AJAX para chamar o método de ação Register da nossa exibição de detalhes. Implementar isso é muito fácil. Primeiro, vamos adicionar duas referências de biblioteca de script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

A primeira biblioteca faz referência a biblioteca de script do lado do cliente do AJAX do ASP.NET core. Esse arquivo é de aproximadamente 24k de tamanho (compactado) e contém a funcionalidade de AJAX do lado do cliente principal. A segunda biblioteca contém funções de utilitário que se integram internos AJAX métodos auxiliares de ASP.NET MVC (que usaremos em breve).

Podemos pode então o código do modelo de exibição que adicionamos anteriormente para que, em vez de outputing uma mensagem "Você não estiver registrado para este evento", em vez disso, renderizamos um link que quando enviada por push de atualização realiza uma chamada ao AJAX que invoca nosso método de ação RSVPForEvent no nosso controlador RSVP e RSVPs o usuário:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

O método auxiliar de Ajax.ActionLink() usado acima é integrado ao ASP.NET MVC e é semelhante ao método auxiliar Html.ActionLink(), exceto que em vez de executar uma navegação padrão faz uma chamada AJAX para o método de ação quando o link é clicado. Acima estamos chamando o método de ação de "Registrar" em controlador "RSVP" e passando o DinnerID como o parâmetro de "id" para ele. O parâmetro AjaxOptions final, estamos passando indica que queremos levar o conteúdo retornado do método de ação e atualize o HTML &lt;div&gt; elemento na página cuja id é "rsvpmsg".

E agora, quando um usuário navega até um jantar eles não estão registrados para ainda eles verão um link para o RSVP para ele:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Se eles clicarem no link "RSVP para este evento" fará uma chamada AJAX para o método de ação Register no controlador de RSVP, e quando ele for concluído, eles verão uma mensagem atualizada como abaixo:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

A largura de banda de rede e o tráfego envolvidos ao fazer essa chamada AJAX é realmente simples. Quando o usuário clica no link "RSVP para este evento", é feita uma solicitação de rede pequena do HTTP POST para o */Dinners/Register/1* URL se parece com abaixo durante a transmissão:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E a resposta do nosso método de ação Register é simplesmente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Essa chamada leve é rápida e funcionarão mesmo em uma rede lenta.

### <a name="adding-a-jquery-animation"></a>Adicionar uma animação de jQuery

A funcionalidade AJAX que implementamos funciona bem e rápida. Às vezes, isso pode acontecer muito rapidamente, no entanto, se um usuário não poderá notar que o link RSVP foi substituído pelo novo texto. Para tornar o resultado um pouco mais óbvio que podemos adicionar uma animação simples para chamar atenção para a mensagem de atualização.

O modelo de projeto do ASP.NET MVC padrão inclui uma biblioteca de JavaScript excelentes (e muito popular) de software livre que também é suportada pela Microsoft – jQuery. jQuery fornece inúmeros recursos, incluindo uma boa biblioteca de efeitos e seleção HTML DOM.

Para usar o jQuery primeiro vamos adicionar uma referência de script para ele. Já que vamos usar jQuery dentro de uma variedade de locais dentro do nosso site, vamos adicionar a referência de script dentro do nosso arquivo de página mestra do site para que todas as páginas podem usá-lo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Dica: Verifique se que você instalou o hotfix do intellisense do JavaScript para o VS 2008 SP1 que habilita o suporte mais amplo de intellisense para arquivos JavaScript (incluindo jQuery). Você pode baixá-lo de: http://tinyurl.com/vs2008javascripthotfix*

Código escrito usando o JQuery geralmente usa um global "$ ()" método de JavaScript que recupera um ou mais elementos HTML usando um seletor de CSS. Por exemplo, <em>$("#rsvpmsg")</em> seleciona qualquer elemento HTML com a id de rsvpmsg, enquanto <em>$(".something")</em> selecionará todos os elementos com o "algo" CSS nome de classe. Você também pode escrever consultas mais avançadas, como "retornar todos os botões de opção selecionado" usando uma consulta de seletor como: <em>$("entrada [@type= rádio] [@checked]")</em>.

Depois de selecionar elementos, você pode chamar métodos neles para tomar uma ação, como ocultá-las: *$("#rsvpmsg").hide();*

Para nosso cenário RSVP, vamos definir uma função de JavaScript simples chamada "AnimateRSVPMessage" que seleciona "rsvpmsg" &lt;div&gt; e anima o tamanho do seu conteúdo de texto. O código abaixo inicia o texto pequeno e, em seguida, faz com que ele aumentará ao longo de um período de tempo de 400 milissegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Podemos pode, em seguida, wire-up essa função JavaScript a ser chamado depois que nossa chamada AJAX é concluída com êxito, passando seu nome para nosso método auxiliar de Ajax.ActionLink() (via o AjaxOptions "OnSuccess" propriedade de evento):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

E agora quando se clica no link "RSVP para este evento" e nossa chamada AJAX é concluída com êxito, a mensagem conteúda enviada back será animar e ficar grande:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Além de fornecer um evento de "OnSuccess", o objeto de AjaxOptions expõe eventos OnBegin, OnFailure e OnComplete que você pode manipular (juntamente com uma variedade de outras propriedades e opções úteis).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpeza - refatorar-out de uma exibição parcial RSVP

Nosso modelo de exibição de detalhes está começando a ficar um pouco longos, ao longo do tempo que irá torná-lo um pouco mais difícil de entender. Para ajudar a melhorar a legibilidade do código, vamos terminam com a criação de uma exibição parcial – RSVPStatus.ascx – que encapsulam a todo o código de modo de exibição RSVP para nossa página de detalhes.

Podemos fazer isso clicando duas vezes na pasta \Views\Dinners e, em seguida, escolher Add -&gt;exibir comando de menu. Teremos que ele utilizam um objeto de jantar como seu ViewModel fortemente tipada. Podemos pode, em seguida, copie/cole o conteúdo RSVP da nossa exibição details. aspx para ele.

Depois que nós já fizemos isso, vamos também criar outra exibição parcial – EditAndDeleteLinks.ascx - que encapsula o nosso código de exibição de link de edição e exclusão. Também teremos ele utilizam um objeto de jantar como seu ViewModel fortemente tipados e copiar/colar a lógica de edição e exclusão de nossa exibição details. aspx para ele.

Nossos detalhes exibir modelo poderá então apenas incluem duas chamadas de método Html.RenderPartial() na parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Isso torna o código mais limpo para ler e manter.

### <a name="next-step"></a>Próxima etapa

Agora vejamos como podemos usar o AJAX ainda mais e adicionar suporte a mapeamento interativa ao nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](secure-applications-using-authentication-and-authorization.md)
> [Próximo](use-ajax-to-implement-mapping-scenarios.md)
