---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "Usar o AJAX para fornecer atualizações dinâmicas | Microsoft Docs"
author: microsoft
description: "Etapa 10 implementa suporte usuários conectados ao RSVP seu interesse em participando de uma refeição, usando uma abordagem baseada em Ajax integrada aos detalhes de uma refeição..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Usar o AJAX para fornecer atualizações dinâmicas
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 10 de livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 10 implementa suporte para os usuários autorizados para RSVP seu interesse em participando de uma refeição, usando uma abordagem baseada em Ajax integrada dentro da página de detalhes de uma refeição.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner etapa 10: Aceita de AJAX habilitando RSVPs

Agora, vamos implementar suporte para os usuários autorizados para RSVP seu interesse em participando de uma refeição. É isso habilitará a usar uma abordagem baseada em AJAX integrada dentro da página de detalhes de uma refeição.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Que indica se o usuário for confirmado a presença

Os usuários podem visitar o */Dinners/detalhes / [id*] URL para ver os detalhes sobre uma refeição específico:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

O método de ação é implementado de Details() da seguinte forma:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

A primeira etapa para implementar o suporte RSVP seria adicionar um método auxiliar de "IsUserRegistered(username)" ao nosso objeto refeição (dentro da classe parcial do Dinner.cs que criamos anteriormente). Esse método auxiliar retorna true ou false dependendo se o usuário for confirmado presença atualmente o diagrama:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Podemos, em seguida, adicionar o código a seguir ao nosso modelo de exibição Details.aspx para exibir uma mensagem apropriada que indica se o usuário está registrado ou não para o evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

E agora quando um usuário acessa uma refeição que eles são registrados para ele verá esta mensagem:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

E quando eles visitam uma refeição não estão registrados para ele verá a mensagem abaixo de:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementando o método de ação de registro

Agora vamos adicionar a funcionalidade necessária para permitir que os usuários RSVP um diagrama da página de detalhes.

Para implementar isso, vamos criar uma nova classe de "RSVPController" clicando duas vezes no diretório \Controllers e escolhendo Adicionar -&gt;comando de menu do controlador.

Implementaremos um método de ação de "Registro" em nova classe RSVPController que usa uma id para uma refeição como um argumento, recupera o objeto refeição apropriado, verifica se o usuário que fez logon já está na lista de usuários que se inscreveram para ele e se não adiciona um objeto RSVP para eles:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Observe acima como estamos retornando uma cadeia de caracteres simple como a saída do método de ação. Podemos ter inserido esta mensagem em um modelo de exibição – mas pois ela é tão pequena apenas usaremos o método auxiliar Content() na classe base do controlador e retornar uma mensagem de cadeia de caracteres, como acima.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Chamar o método de ação RSVPForEvent usando AJAX

Vamos usar AJAX para invocar o método de ação de registro da visão de detalhes. Implementar isso é muito fácil. Primeiro, vamos adicionar duas referências de biblioteca do script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

A primeira biblioteca faz referência a biblioteca de script do lado do cliente do ASP.NET AJAX principal. Esse arquivo é aproximadamente 24 KB de tamanho (compactado) e contém a funcionalidade de AJAX do lado do cliente principal. A segunda biblioteca contém funções de utilitário que se integram ao AJAX auxiliar métodos internos de ASP.NET MVC (que será usado em breve).

Podemos atualizar o código de modelo de exibição adicionamos anteriormente para que, em vez de outputing uma mensagem "Você não estiver registrado para este evento", em vez disso, renderizar um link que quando enviada por push executará uma chamada AJAX que invoca o método de ação nosso RSVPForEvent em nosso controlador RSVP e RSVPs o usuário:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

O método auxiliar de Ajax.ActionLink() usado acima é integrado ao ASP.NET MVC e é similar ao método auxiliar Html.ActionLink(), exceto que, em vez de executar uma navegação padrão faz uma chamada AJAX para o método de ação quando o link é clicado. Acima estamos chamando o método de ação de "Registro" em controlador "RSVP" e transmitindo o DinnerID como o parâmetro "id" a ele. O parâmetro AjaxOptions final, passa indica que desejamos levar o conteúdo retornado do método de ação e atualize o HTML &lt;div&gt; elemento na página cuja id é "rsvpmsg".

E agora, quando um usuário navega para uma refeição eles não estão registrados para ainda ele verá um link para RSVP para ele:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Se clicar no link "RSVP para este evento" farão uma chamada AJAX para o método de ação de registro no controlador RSVP e quando ela é concluída eles verá uma mensagem atualizada como abaixo:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

A largura de banda de rede e o tráfego envolvidos ao fazer essa chamada AJAX é leves. Quando o usuário clica no link "RSVP para este evento", é feita uma solicitação de rede pequena do HTTP POST para o */Dinners/Register/1* URL parece abaixo na conexão:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E a resposta do nosso método de ação de registro é simplesmente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Essa chamada leve é rápida e funcionará mesmo em uma rede lenta.

### <a name="adding-a-jquery-animation"></a>Adicionar um animação jQuery

A funcionalidade AJAX implementamos funciona bem e rápida. Às vezes, isso pode acontecer muito rapidamente, no entanto, se um usuário poderá não perceber que o link RSVP foi substituído pelo novo texto. Para tornar o resultado um pouco mais óbvio que podemos adicionar uma animação simples para chamar atenção para a mensagem de atualização.

O modelo de projeto do ASP.NET MVC padrão inclui uma biblioteca de JavaScript do código-fonte aberto excelente (e muito popular) que também é suportada pela Microsoft – jQuery. jQuery fornece uma série de recursos, incluindo uma biblioteca de seleção e efeitos de HTML DOM adequada.

Para usar o jQuery primeiro vamos adicionar uma referência de script para ele. Porque estamos usando o jQuery dentro de uma variedade de locais em nosso site, vamos adicionar a referência de script no arquivo de página mestra nosso Site.master para que todas as páginas podem usá-lo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Dica: Verifique se que você instalou o hotfix do intellisense do JavaScript para VS 2008 SP1 que habilita o suporte mais amplo de intellisense para arquivos JavaScript (incluindo jQuery). Você pode baixá-lo do: http://tinyurl.com/vs2008javascripthotfix*

Código gravado usando JQuery geralmente usa um global "$ ()" método de JavaScript que recupera um ou mais elementos HTML usando um seletor CSS. Por exemplo, *$("#rsvpmsg")* seleciona qualquer elemento HTML com a id de rsvpmsg, enquanto *$(".something")* selecione todos os elementos com "algo" CSS nome de classe. Você também pode escrever consultas mais avançadas, como "retornar todos os botões de opção de ativação" usando uma consulta de seletor como: *$("entrada [@type= opção] [@checked]")*.

Depois de selecionar elementos, você pode chamar métodos neles tome uma ação, como ocultá-las: *$("#rsvpmsg").hide();*

Em nosso cenário RSVP, vamos definir uma função JavaScript simples chamada "AnimateRSVPMessage" que seleciona "rsvpmsg" &lt;div&gt; e anima o tamanho de seu conteúdo de texto. O código abaixo inicia o texto pequeno e, em seguida, faz com que ele a aumentar ao longo de um período de tempo de 400 milissegundos:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Podemos pode, em seguida, durante a transmissão a essa função JavaScript a ser chamado após conclusão bem-sucedida da nossa chamada AJAX transmitindo seu nome para nosso método auxiliar de Ajax.ActionLink() (por meio de AjaxOptions "OnSuccess" propriedade de evento):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

E agora quando se clica no link "RSVP para este evento" e nossa chamada AJAX concluído com êxito, a conteúdo mensagem enviada back será animar e aumentar:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Além de fornecer um evento "OnSuccess", o objeto AjaxOptions expõe OnBegin, OnFailure e OnComplete eventos que você pode manipular (junto com uma variedade de outras propriedades e opções úteis).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Limpeza - refatorar um modo de exibição de parcial RSVP

Nosso modelo de exibição de detalhes está começando a ficar um pouco longos, quais extras tornará um pouco mais difícil de entender. Para ajudar a melhorar a legibilidade do código, vamos finalizar, criando uma exibição parcial – RSVPStatus.ascx – que encapsulam a todo o código de exibição RSVP para nossa página de detalhes.

Podemos fazer isso clicando duas vezes na pasta \Views\Dinners e, em seguida, escolhendo a adicionar -&gt;exibir comando de menu. Teremos-lo a tirar um objeto de uma refeição como seu ViewModel fortemente tipada. Podemos pode, em seguida, copiar/colar o conteúdo RSVP do nosso exibição Details.aspx nele.

Depois que tiver feito isso, vamos também criar outra exibição parcial – EditAndDeleteLinks.ascx - que encapsula o nosso código de exibição de link de editar e excluir. Também teremos ele tirar um objeto de uma refeição como seu ViewModel fortemente tipada e copiar/colar a lógica de edição e exclusão de nosso exibição Details.aspx nele.

Nosso detalhes exibir modelo e incluem apenas duas chamadas de método Html.RenderPartial() na parte inferior:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Isso torna o código de limpeza de ler e manter.

### <a name="next-step"></a>Próxima etapa

Agora, vamos ver como podemos usar AJAX ainda mais e adicionar suporte a mapeamento interativa ao nosso aplicativo.

>[!div class="step-by-step"]
[Anterior](secure-applications-using-authentication-and-authorization.md)
[Próximo](use-ajax-to-implement-mapping-scenarios.md)
