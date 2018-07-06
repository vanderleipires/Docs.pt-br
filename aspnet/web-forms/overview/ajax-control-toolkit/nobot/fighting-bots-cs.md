---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Combatendo Bots (c#) | Microsoft Docs
author: wenz
description: Os bots automatizados Estuque blogs e outros sites com spams, Enviar comentário formulários sem qualquer interação do usuário. Controle NoBot do ASP.NET AJAX Con...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: ed03b46c8a4859a79322290106a5181867392d0a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824193"
---
<a name="fighting-bots-c"></a><span data-ttu-id="e55ec-104">Combatendo Bots (c#)</span><span class="sxs-lookup"><span data-stu-id="e55ec-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="e55ec-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e55ec-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e55ec-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e55ec-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="e55ec-107">Os bots automatizados Estuque blogs e outros sites com spams, Enviar comentário formulários sem qualquer interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="e55ec-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="e55ec-108">O controle NoBot do ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.</span><span class="sxs-lookup"><span data-stu-id="e55ec-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="e55ec-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="e55ec-109">Overview</span></span>

<span data-ttu-id="e55ec-110">Os bots automatizados Estuque blogs e outros sites com spams, Enviar comentário formulários sem qualquer interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="e55ec-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="e55ec-111">O controle NoBot do ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.</span><span class="sxs-lookup"><span data-stu-id="e55ec-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="e55ec-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="e55ec-112">Steps</span></span>

<span data-ttu-id="e55ec-113">Uma abordagem comum para derrotar bots é usar CAPTCHAs completamente automatizada pública teste de Turing para informar ao separar os seres humanos e computadores.</span><span class="sxs-lookup"><span data-stu-id="e55ec-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="e55ec-114">Um teste de Turing foi originalmente um teste onde alguém necessários para decidir se um parceiro de comunicação é um ser humano ou um computador.</span><span class="sxs-lookup"><span data-stu-id="e55ec-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="e55ec-115">Na web, um CAPTCHA geralmente consiste em uma imagem com algumas letras distorcidas nele.</span><span class="sxs-lookup"><span data-stu-id="e55ec-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="e55ec-116">A ideia é que apenas uma pessoa pode ler as letras na imagem, ao passo que algoritmos de OCR falhará.</span><span class="sxs-lookup"><span data-stu-id="e55ec-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="e55ec-117">Há várias vantagens e desvantagens nessa abordagem, mas uma discussão sobre isso está além do escopo deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e55ec-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="e55ec-118">No entanto há um controle no ASP.NET AJAX Control Toolkit, que fornece uma abordagem semelhante: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="e55ec-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="e55ec-119">Ele é fácil superação que um CAPTCHA, mas é muito fácil de usar e são muito bem em sites como blogs, onde ela é considerada um sucesso se a maioria de spam tentativas é derrotada, que o `NoBot` controle pode fazer.</span><span class="sxs-lookup"><span data-stu-id="e55ec-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="e55ec-120">`NoBot` intercepta o postback do formulário da web ASP.NET atual se pelo menos uma das seguintes condições for atendida:</span><span class="sxs-lookup"><span data-stu-id="e55ec-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="e55ec-121">O navegador não consegue resolver um quebra-cabeça JavaScript (por exemplo quando JavaScript está desativado)</span><span class="sxs-lookup"><span data-stu-id="e55ec-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="e55ec-122">O usuário enviou o formulário para rápida</span><span class="sxs-lookup"><span data-stu-id="e55ec-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="e55ec-123">O endereço IP do cliente enviado o formulário, muitas vezes em um determinado período de tempo.</span><span class="sxs-lookup"><span data-stu-id="e55ec-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="e55ec-124">Fim de verificar essas condições, o `NoBot` controle requer esses atributos (todos eles opcional):</span><span class="sxs-lookup"><span data-stu-id="e55ec-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="e55ec-125">`ResponseMinimumDelaySeconds` valor mínimo de segundos entre postbacks</span><span class="sxs-lookup"><span data-stu-id="e55ec-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="e55ec-126">`CutoffWindowSeconds` duração do intervalo de tempo em que os postbacks de um IP são medidas</span><span class="sxs-lookup"><span data-stu-id="e55ec-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="e55ec-127">`CutoffMaximumInstances` quantidade máxima de segundos por intervalo de tempo</span><span class="sxs-lookup"><span data-stu-id="e55ec-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="e55ec-128">As demandas de marcação a seguir que pelo menos dois segundos decorrerem entre postbacks, e há postbacks apenas cinco ou menos dentro de um intervalo de 30 segundos:</span><span class="sxs-lookup"><span data-stu-id="e55ec-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="e55ec-129">Em seguida, como de costume Certifique-se de incluir o `ScriptManager` na página para que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="e55ec-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="e55ec-130">Como a maior parte das verificações `NoBot` está fazendo ocorrem no lado do servidor, você precisa verificar o resultado dessas validações.</span><span class="sxs-lookup"><span data-stu-id="e55ec-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="e55ec-131">Isso pode ser feito por meio da chamada `NoBot`do `IsValid()` método.</span><span class="sxs-lookup"><span data-stu-id="e55ec-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="e55ec-132">Ele tem um argumento (como uma `out` parâmetro /`ByRef` parâmetro) que é do tipo `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="e55ec-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="e55ec-133">Sua representação de cadeia de caracteres contém a razão quando a verificação falhar e `Valid` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="e55ec-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="e55ec-134">O código a seguir gera uma mensagem de acordo com a `NoBot`do resultado:</span><span class="sxs-lookup"><span data-stu-id="e55ec-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="e55ec-135">Por fim, você precisa de um formulário para enviar e um elemento de rótulo para a mensagem de saída e pronto!</span><span class="sxs-lookup"><span data-stu-id="e55ec-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="e55ec-136">Quando você executa esse script e desativar JavaScript ou enviar o formulário durante os primeiros dois segundos ou enviar o formulário de sete vezes dentro de trinta segundos, você receberá uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="e55ec-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="e55ec-137">No entanto, usar esse controle com sabedoria, como apenas cerca de 90-95% dos usuários têm ativados por JavaScript, portanto 5 a 10% dos usuários falharão `NoBot`do teste.</span><span class="sxs-lookup"><span data-stu-id="e55ec-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="e55ec-138">[![Essa mensagem de erro pode ter sido causada por um bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e55ec-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="e55ec-139">Essa mensagem de erro pode ter sido causada por um bot ([clique para exibir a imagem em tamanho normal](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e55ec-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e55ec-140">Avançar</span><span class="sxs-lookup"><span data-stu-id="e55ec-140">Next</span></span>](fighting-bots-vb.md)
