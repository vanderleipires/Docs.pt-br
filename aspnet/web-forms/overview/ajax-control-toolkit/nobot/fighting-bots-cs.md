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
<a name="fighting-bots-c"></a>Combatendo Bots (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Os bots automatizados Estuque blogs e outros sites com spams, Enviar comentário formulários sem qualquer interação do usuário. O controle NoBot do ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.


## <a name="overview"></a>Visão geral

Os bots automatizados Estuque blogs e outros sites com spams, Enviar comentário formulários sem qualquer interação do usuário. O controle NoBot do ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.

## <a name="steps"></a>Etapas

Uma abordagem comum para derrotar bots é usar CAPTCHAs completamente automatizada pública teste de Turing para informar ao separar os seres humanos e computadores. Um teste de Turing foi originalmente um teste onde alguém necessários para decidir se um parceiro de comunicação é um ser humano ou um computador. Na web, um CAPTCHA geralmente consiste em uma imagem com algumas letras distorcidas nele. A ideia é que apenas uma pessoa pode ler as letras na imagem, ao passo que algoritmos de OCR falhará.

Há várias vantagens e desvantagens nessa abordagem, mas uma discussão sobre isso está além do escopo deste tutorial. No entanto há um controle no ASP.NET AJAX Control Toolkit, que fornece uma abordagem semelhante: `NoBot`. Ele é fácil superação que um CAPTCHA, mas é muito fácil de usar e são muito bem em sites como blogs, onde ela é considerada um sucesso se a maioria de spam tentativas é derrotada, que o `NoBot` controle pode fazer.

`NoBot` intercepta o postback do formulário da web ASP.NET atual se pelo menos uma das seguintes condições for atendida:

- O navegador não consegue resolver um quebra-cabeça JavaScript (por exemplo quando JavaScript está desativado)
- O usuário enviou o formulário para rápida
- O endereço IP do cliente enviado o formulário, muitas vezes em um determinado período de tempo.

Fim de verificar essas condições, o `NoBot` controle requer esses atributos (todos eles opcional):

- `ResponseMinimumDelaySeconds` valor mínimo de segundos entre postbacks
- `CutoffWindowSeconds` duração do intervalo de tempo em que os postbacks de um IP são medidas
- `CutoffMaximumInstances` quantidade máxima de segundos por intervalo de tempo

As demandas de marcação a seguir que pelo menos dois segundos decorrerem entre postbacks, e há postbacks apenas cinco ou menos dentro de um intervalo de 30 segundos:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Em seguida, como de costume Certifique-se de incluir o `ScriptManager` na página para que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Como a maior parte das verificações `NoBot` está fazendo ocorrem no lado do servidor, você precisa verificar o resultado dessas validações. Isso pode ser feito por meio da chamada `NoBot`do `IsValid()` método. Ele tem um argumento (como uma `out` parâmetro /`ByRef` parâmetro) que é do tipo `NoBotState`. Sua representação de cadeia de caracteres contém a razão quando a verificação falhar e `Valid` caso contrário. O código a seguir gera uma mensagem de acordo com a `NoBot`do resultado:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Por fim, você precisa de um formulário para enviar e um elemento de rótulo para a mensagem de saída e pronto!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Quando você executa esse script e desativar JavaScript ou enviar o formulário durante os primeiros dois segundos ou enviar o formulário de sete vezes dentro de trinta segundos, você receberá uma mensagem de erro. No entanto, usar esse controle com sabedoria, como apenas cerca de 90-95% dos usuários têm ativados por JavaScript, portanto 5 a 10% dos usuários falharão `NoBot`do teste.


[![Essa mensagem de erro pode ter sido causada por um bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Essa mensagem de erro pode ter sido causada por um bot ([clique para exibir a imagem em tamanho normal](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](fighting-bots-vb.md)
