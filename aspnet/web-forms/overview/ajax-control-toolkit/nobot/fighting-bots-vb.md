---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Combate Bots (VB) | Microsoft Docs
author: wenz
description: "Bots automatizados Estuque weblogs e outros sites com spam, envio de formulários de comentário sem qualquer interação do usuário. O controle de NoBot no ASP.NET AJAX Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b786fd8605c7521a4aae8e49ca236363a71b572
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-vb"></a>Combate Bots (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Bots automatizados Estuque weblogs e outros sites com spam, envio de formulários de comentário sem qualquer interação do usuário. O controle NoBot no ASP.NET AJAX Control Toolkit pode ajudar a combater os bots.


## <a name="overview"></a>Visão Geral

Bots automatizados Estuque weblogs e outros sites com spam, envio de formulários de comentário sem qualquer interação do usuário. O controle NoBot no ASP.NET AJAX Control Toolkit pode ajudar a combater os bots.

## <a name="steps"></a>Etapas

Uma abordagem comum para anular bots é usar o teste CAPTCHAs completamente automatizada pública Turing dizer a separar os usuários e computadores. Um teste Turing foi originalmente um teste em que alguém precisa decidir se um parceiro de comunicação é um ser humano ou um computador. Na web, um CAPTCHA normalmente consiste em uma imagem com algumas letras distorcidas nele. A ideia é que apenas uma pessoa pode ler as letras na imagem, enquanto que os algoritmos de OCR falhará.

Há várias vantagens e desvantagens dessa abordagem, mas uma abordagem sobre isso está além do escopo deste tutorial. No entanto há um controle no ASP.NET AJAX Control Toolkit, que fornece uma abordagem semelhante: `NoBot`. Ele é mais fácil superar que um CAPTCHA, mas é muito fácil de usar e é muito bem em sites como blogs, onde ela é considerada um sucesso se mais tentativas de spam é desfeito, que o `NoBot` pode fazer.

`NoBot`intercepta a postagem de formulário da web ASP.NET atual se pelo menos uma das seguintes condições for atendida:

- O navegador não consegue resolver um problema de JavaScript (por exemplo quando JavaScript está desativado)
- O usuário enviou o formulário rápido
- O endereço IP do cliente enviado o formulário, muitas vezes em um determinado período de tempo.

Para verificar essas condições, o `NoBot` controle requer esses atributos (todos eles opcional):

- `ResponseMinimumDelaySeconds`valor mínimo de segundos entre postbacks
- `CutoffWindowSeconds`duração do intervalo de tempo no qual postagens de um IP são medidas
- `CutoffMaximumInstances`máximo de segundos por intervalo de tempo

As seguintes exigências de marcação que pelo menos dois segundos decorrerem entre postbacks e há postbacks apenas cinco ou menos dentro de um intervalo de 30 segundos:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Em seguida, como de costume Certifique-se de incluir o `ScriptManager` na página de forma que a biblioteca ASP.NET AJAX foi carregada e o Kit de ferramentas de controle pode ser usado:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Como a maior parte das verificações `NoBot` está fazendo ocorrer no lado do servidor, você precisa verificar o resultado dessas validações. Isso pode ser feito chamando `NoBot`do `IsValid()` método. Ele tem um argumento (como um `out` parâmetro /`ByRef` parâmetro) que é do tipo `NoBotState`. Sua representação de cadeia de caracteres contém a razão quando a verificação falhar e `Valid` caso contrário. O código a seguir gera uma mensagem de acordo com a `NoBot`do resultado:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Por fim, você precisa de um formulário para enviar e um elemento de rótulo para a mensagem de saída e pronto!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Quando você executa esse script e desativar JavaScript ou enviar o formulário durante os primeiros dois segundos ou enviar o formulário de sete vezes dentro de 30 segundos, você receberá uma mensagem de erro. No entanto, use este controle criteriosamente, como somente cerca de 90 95% dos usuários têm JavaScript ativado, portanto de 5 a 10% dos usuários falhará `NoBot`do teste.


[![Essa mensagem de erro pode ter sido causada por um bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Essa mensagem de erro pode ter sido causada por um bot ([clique para exibir a imagem em tamanho normal](fighting-bots-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](fighting-bots-cs.md)
