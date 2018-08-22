---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar o AJAX para fornecer atualizações dinâmicas | Microsoft Docs
author: microsoft
description: Etapa 10 implementa suporte os usuários conectados ao RSVP seu interesse em participar de um jantar, usando uma abordagem baseada em Ajax integrada em detalhes o jantar...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: e902881d3dab6a902cb747a197a32f317d199723
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824781"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="e4573-103">Usar o AJAX para fornecer atualizações dinâmicas</span><span class="sxs-lookup"><span data-stu-id="e4573-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="e4573-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e4573-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="e4573-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="e4573-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="e4573-106">Esta é a etapa 10 de um livre [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="e4573-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="e4573-107">Etapa 10 implementa suporte para os usuários conectados ao RSVP seu interesse em participar de um jantar, usando uma abordagem baseada em Ajax integrada dentro da página de detalhes do jantar.</span><span class="sxs-lookup"><span data-stu-id="e4573-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="e4573-108">Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.</span><span class="sxs-lookup"><span data-stu-id="e4573-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="e4573-109">NerdDinner etapa 10: Aceita de AJAX habilitando RSVPs</span><span class="sxs-lookup"><span data-stu-id="e4573-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="e4573-110">Agora, vamos implementar o suporte para os usuários conectados ao RSVP seu interesse em participar de um jantar.</span><span class="sxs-lookup"><span data-stu-id="e4573-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="e4573-111">Nós a Habilitaremos isso usando uma abordagem baseada em AJAX integrada dentro da página de detalhes do jantar.</span><span class="sxs-lookup"><span data-stu-id="e4573-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="e4573-112">Que indica se o usuário é RSVP'd</span><span class="sxs-lookup"><span data-stu-id="e4573-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="e4573-113">Os usuários podem visitar o */Dinners/detalhes / [id*] URL para ver detalhes sobre uma refeição específica:</span><span class="sxs-lookup"><span data-stu-id="e4573-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="e4573-114">O Details() método de ação é implementado da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="e4573-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="e4573-115">Nossa primeira etapa para implementar o suporte RSVP será adicionar um método auxiliar de "IsUserRegistered(username)" ao nosso objeto de jantar (dentro da classe parcial do Dinner.cs que criamos anteriormente).</span><span class="sxs-lookup"><span data-stu-id="e4573-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="e4573-116">Esse método auxiliar retorna true ou false dependendo se o usuário atualmente é RSVP'd do jantar:</span><span class="sxs-lookup"><span data-stu-id="e4573-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="e4573-117">Em seguida, podemos adicionar o código a seguir para nosso modelo de exibição details. aspx para exibir uma mensagem apropriada indicando se o usuário está registrado ou não para o evento:</span><span class="sxs-lookup"><span data-stu-id="e4573-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="e4573-118">E agora quando um usuário visita um jantar, eles são registrados para ele verá esta mensagem:</span><span class="sxs-lookup"><span data-stu-id="e4573-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="e4573-119">E quando eles visitam um jantar não estão registrados para que eles verão a mensagem abaixo:</span><span class="sxs-lookup"><span data-stu-id="e4573-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="e4573-120">Implementando o método de ação Register</span><span class="sxs-lookup"><span data-stu-id="e4573-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="e4573-121">Agora, vamos adicionar a funcionalidade necessária para permitir que os usuários confirme presença em um jantar da página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="e4573-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="e4573-122">Para implementar isso, vamos criar uma nova classe "RSVPController" clicando duas vezes no diretório \Controllers e escolhendo Add -&gt;comando de menu do controlador.</span><span class="sxs-lookup"><span data-stu-id="e4573-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="e4573-123">Implementaremos um método de ação dentro da classe RSVPController novo que usa uma id em um jantar como um argumento, recupera o objeto apropriado de jantar, verifica se o usuário que fez logon estiver atualmente na lista de usuários que se registraram para ele e se "Registrar" não adiciona um objeto RSVP para eles:</span><span class="sxs-lookup"><span data-stu-id="e4573-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="e4573-124">Observe acima como estamos retornando uma cadeia de caracteres simple como a saída do método de ação.</span><span class="sxs-lookup"><span data-stu-id="e4573-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="e4573-125">Podemos ter inserido esta mensagem dentro de um modelo de exibição – mas, pois ele é bem pequeno, usaremos apenas o método auxiliar Content() na classe base de controlador e retornar uma mensagem de cadeia de caracteres como acima.</span><span class="sxs-lookup"><span data-stu-id="e4573-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="e4573-126">Chamar o método de ação RSVPForEvent usando AJAX</span><span class="sxs-lookup"><span data-stu-id="e4573-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="e4573-127">Vamos usar AJAX para chamar o método de ação Register da nossa exibição de detalhes.</span><span class="sxs-lookup"><span data-stu-id="e4573-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="e4573-128">Implementar isso é muito fácil.</span><span class="sxs-lookup"><span data-stu-id="e4573-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="e4573-129">Primeiro, vamos adicionar duas referências de biblioteca de script:</span><span class="sxs-lookup"><span data-stu-id="e4573-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="e4573-130">A primeira biblioteca faz referência a biblioteca de script do lado do cliente do AJAX do ASP.NET core.</span><span class="sxs-lookup"><span data-stu-id="e4573-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="e4573-131">Esse arquivo é de aproximadamente 24k de tamanho (compactado) e contém a funcionalidade de AJAX do lado do cliente principal.</span><span class="sxs-lookup"><span data-stu-id="e4573-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="e4573-132">A segunda biblioteca contém funções de utilitário que se integram internos AJAX métodos auxiliares de ASP.NET MVC (que usaremos em breve).</span><span class="sxs-lookup"><span data-stu-id="e4573-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="e4573-133">Podemos pode então o código do modelo de exibição que adicionamos anteriormente para que, em vez de outputing uma mensagem "Você não estiver registrado para este evento", em vez disso, renderizamos um link que quando enviada por push de atualização realiza uma chamada ao AJAX que invoca nosso método de ação RSVPForEvent no nosso controlador RSVP e RSVPs o usuário:</span><span class="sxs-lookup"><span data-stu-id="e4573-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="e4573-134">O método auxiliar de Ajax.ActionLink() usado acima é integrado ao ASP.NET MVC e é semelhante ao método auxiliar Html.ActionLink(), exceto que em vez de executar uma navegação padrão faz uma chamada AJAX para o método de ação quando o link é clicado.</span><span class="sxs-lookup"><span data-stu-id="e4573-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="e4573-135">Acima estamos chamando o método de ação de "Registrar" em controlador "RSVP" e passando o DinnerID como o parâmetro de "id" para ele.</span><span class="sxs-lookup"><span data-stu-id="e4573-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="e4573-136">O parâmetro AjaxOptions final, estamos passando indica que queremos levar o conteúdo retornado do método de ação e atualize o HTML &lt;div&gt; elemento na página cuja id é "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="e4573-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="e4573-137">E agora, quando um usuário navega até um jantar eles não estão registrados para ainda eles verão um link para o RSVP para ele:</span><span class="sxs-lookup"><span data-stu-id="e4573-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="e4573-138">Se eles clicarem no link "RSVP para este evento" fará uma chamada AJAX para o método de ação Register no controlador de RSVP, e quando ele for concluído, eles verão uma mensagem atualizada como abaixo:</span><span class="sxs-lookup"><span data-stu-id="e4573-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="e4573-139">A largura de banda de rede e o tráfego envolvidos ao fazer essa chamada AJAX é realmente simples.</span><span class="sxs-lookup"><span data-stu-id="e4573-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="e4573-140">Quando o usuário clica no link "RSVP para este evento", é feita uma solicitação de rede pequena do HTTP POST para o */Dinners/Register/1* URL se parece com abaixo durante a transmissão:</span><span class="sxs-lookup"><span data-stu-id="e4573-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="e4573-141">E a resposta do nosso método de ação Register é simplesmente:</span><span class="sxs-lookup"><span data-stu-id="e4573-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="e4573-142">Essa chamada leve é rápida e funcionarão mesmo em uma rede lenta.</span><span class="sxs-lookup"><span data-stu-id="e4573-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="e4573-143">Adicionar uma animação de jQuery</span><span class="sxs-lookup"><span data-stu-id="e4573-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="e4573-144">A funcionalidade AJAX que implementamos funciona bem e rápida.</span><span class="sxs-lookup"><span data-stu-id="e4573-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="e4573-145">Às vezes, isso pode acontecer muito rapidamente, no entanto, se um usuário não poderá notar que o link RSVP foi substituído pelo novo texto.</span><span class="sxs-lookup"><span data-stu-id="e4573-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="e4573-146">Para tornar o resultado um pouco mais óbvio que podemos adicionar uma animação simples para chamar atenção para a mensagem de atualização.</span><span class="sxs-lookup"><span data-stu-id="e4573-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="e4573-147">O modelo de projeto do ASP.NET MVC padrão inclui uma biblioteca de JavaScript excelentes (e muito popular) de software livre que também é suportada pela Microsoft – jQuery.</span><span class="sxs-lookup"><span data-stu-id="e4573-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="e4573-148">jQuery fornece inúmeros recursos, incluindo uma boa biblioteca de efeitos e seleção HTML DOM.</span><span class="sxs-lookup"><span data-stu-id="e4573-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="e4573-149">Para usar o jQuery primeiro vamos adicionar uma referência de script para ele.</span><span class="sxs-lookup"><span data-stu-id="e4573-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="e4573-150">Já que vamos usar jQuery dentro de uma variedade de locais dentro do nosso site, vamos adicionar a referência de script dentro do nosso arquivo de página mestra do site para que todas as páginas podem usá-lo.</span><span class="sxs-lookup"><span data-stu-id="e4573-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="e4573-151">*Dica: Verifique se que você instalou o hotfix do intellisense do JavaScript para o VS 2008 SP1 que habilita o suporte mais amplo de intellisense para arquivos JavaScript (incluindo jQuery). Você pode baixá-lo de: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="e4573-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="e4573-152">Código escrito usando o JQuery geralmente usa um global "$ ()" método de JavaScript que recupera um ou mais elementos HTML usando um seletor de CSS.</span><span class="sxs-lookup"><span data-stu-id="e4573-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="e4573-153">Por exemplo, <em>$("#rsvpmsg")</em> seleciona qualquer elemento HTML com a id de rsvpmsg, enquanto <em>$(".something")</em> selecionará todos os elementos com o "algo" CSS nome de classe.</span><span class="sxs-lookup"><span data-stu-id="e4573-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="e4573-154">Você também pode escrever consultas mais avançadas, como "retornar todos os botões de opção selecionado" usando uma consulta de seletor como: <em>$("entrada [@type= rádio] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="e4573-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="e4573-155">Depois de selecionar elementos, você pode chamar métodos neles para tomar uma ação, como ocultá-las: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="e4573-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="e4573-156">Para nosso cenário RSVP, vamos definir uma função de JavaScript simples chamada "AnimateRSVPMessage" que seleciona "rsvpmsg" &lt;div&gt; e anima o tamanho do seu conteúdo de texto.</span><span class="sxs-lookup"><span data-stu-id="e4573-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="e4573-157">O código abaixo inicia o texto pequeno e, em seguida, faz com que ele aumentará ao longo de um período de tempo de 400 milissegundos:</span><span class="sxs-lookup"><span data-stu-id="e4573-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="e4573-158">Podemos pode, em seguida, wire-up essa função JavaScript a ser chamado depois que nossa chamada AJAX é concluída com êxito, passando seu nome para nosso método auxiliar de Ajax.ActionLink() (via o AjaxOptions "OnSuccess" propriedade de evento):</span><span class="sxs-lookup"><span data-stu-id="e4573-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="e4573-159">E agora quando se clica no link "RSVP para este evento" e nossa chamada AJAX é concluída com êxito, a mensagem conteúda enviada back será animar e ficar grande:</span><span class="sxs-lookup"><span data-stu-id="e4573-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="e4573-160">Além de fornecer um evento de "OnSuccess", o objeto de AjaxOptions expõe eventos OnBegin, OnFailure e OnComplete que você pode manipular (juntamente com uma variedade de outras propriedades e opções úteis).</span><span class="sxs-lookup"><span data-stu-id="e4573-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="e4573-161">Limpeza - refatorar-out de uma exibição parcial RSVP</span><span class="sxs-lookup"><span data-stu-id="e4573-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="e4573-162">Nosso modelo de exibição de detalhes está começando a ficar um pouco longos, ao longo do tempo que irá torná-lo um pouco mais difícil de entender.</span><span class="sxs-lookup"><span data-stu-id="e4573-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="e4573-163">Para ajudar a melhorar a legibilidade do código, vamos terminam com a criação de uma exibição parcial – RSVPStatus.ascx – que encapsulam a todo o código de modo de exibição RSVP para nossa página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="e4573-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="e4573-164">Podemos fazer isso clicando duas vezes na pasta \Views\Dinners e, em seguida, escolher Add -&gt;exibir comando de menu.</span><span class="sxs-lookup"><span data-stu-id="e4573-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="e4573-165">Teremos que ele utilizam um objeto de jantar como seu ViewModel fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="e4573-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="e4573-166">Podemos pode, em seguida, copie/cole o conteúdo RSVP da nossa exibição details. aspx para ele.</span><span class="sxs-lookup"><span data-stu-id="e4573-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="e4573-167">Depois que nós já fizemos isso, vamos também criar outra exibição parcial – EditAndDeleteLinks.ascx - que encapsula o nosso código de exibição de link de edição e exclusão.</span><span class="sxs-lookup"><span data-stu-id="e4573-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="e4573-168">Também teremos ele utilizam um objeto de jantar como seu ViewModel fortemente tipados e copiar/colar a lógica de edição e exclusão de nossa exibição details. aspx para ele.</span><span class="sxs-lookup"><span data-stu-id="e4573-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="e4573-169">Nossos detalhes exibir modelo poderá então apenas incluem duas chamadas de método Html.RenderPartial() na parte inferior:</span><span class="sxs-lookup"><span data-stu-id="e4573-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="e4573-170">Isso torna o código mais limpo para ler e manter.</span><span class="sxs-lookup"><span data-stu-id="e4573-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="e4573-171">Próxima etapa</span><span class="sxs-lookup"><span data-stu-id="e4573-171">Next Step</span></span>

<span data-ttu-id="e4573-172">Agora vejamos como podemos usar o AJAX ainda mais e adicionar suporte a mapeamento interativa ao nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e4573-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4573-173">[Anterior](secure-applications-using-authentication-and-authorization.md)
> [Próximo](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="e4573-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
