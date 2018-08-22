---
uid: whitepapers/request-validation
title: Solicitação de validação – impedindo ataques de Script | Microsoft Docs
author: rick-anderson
description: Este documento descreve o recurso de validação de solicitação do ASP.NET, onde, por padrão, o aplicativo será impedido de processamento submitt conteúdo de HTML não codificado...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 087f30428602137e01f574825f3ebcd4db9285ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824160"
---
<a name="request-validation---preventing-script-attacks"></a>Solicitação de validação – impedindo ataques de Script
====================
> Este documento descreve o recurso de validação de solicitação do ASP.NET, onde, por padrão, o aplicativo será impedido de processamento de conteúdo HTML não codificado enviado ao servidor. Esse recurso de validação de solicitação pode ser desabilitado quando o aplicativo foi projetado para processar com segurança dados HTML.
> 
> Aplica-se para o ASP.NET 1.1 e ASP.NET 2.0.


Validação de solicitação, um recurso do ASP.NET desde a versão 1.1, impede que o servidor aceite conteúdo sem HTML codificado. Esse recurso é criado para ajudar a evitar alguns ataques de injeção de script no qual código de script de cliente ou HTML pode ser enviado inadvertidamente para um servidor, armazenado e, em seguida, apresentado a outros usuários. Ainda recomendamos que você validar a entrada de todos os dados e a codificação HTML quando for apropriado.

Por exemplo, você pode criar uma página da Web que solicita o endereço de email do usuário e, em seguida, armazenamentos de endereços de email em um banco de dados. Se o usuário insere &lt;SCRIPT&gt;("Olá do script") do alerta&lt;/SCRIPT&gt; em vez de um endereço de email válido, quando esses dados são apresentados, esse script pode ser executado se o conteúdo não foi devidamente codificado. O recurso de validação de solicitação do ASP.NET impede que isso ocorra.

## <a name="why-this-feature-is-useful"></a>Por que esse recurso é útil

Muitos sites não estão cientes de que eles estão abertos a ataques de injeção de script simples. Se a finalidade desses ataques é distorcer o site exibindo HTML ou para executar o script de cliente para redirecionar o usuário ao site do hacker potencialmente, ataques de injeção de script são um problema que os desenvolvedores da Web devem ser seguidas.

Ataques de injeção de script são uma preocupação de todos os desenvolvedores da web, seja usando ASP.NET, ASP ou outras tecnologias de desenvolvimento da web.

O recurso de validação de solicitação do ASP.NET proativamente evita esses ataques, não permitindo que o conteúdo HTML não codificado a ser processada pelo servidor, a menos que o desenvolvedor decide permitir que o conteúdo.

## <a name="what-to-expect-error-page"></a>O que esperar: página de erro

Captura de tela abaixo mostra alguns exemplos de código do ASP.NET:

![](request-validation/_static/image1.png)

Em execução desse código resulta em uma página simple que permite que você insira algum texto na caixa de texto, clique no botão e exibir o texto no controle de rótulo:

![](request-validation/_static/image2.png)

No entanto, eram JavaScript, tal como `<script>alert("hello!")</script>` seja inserido e enviado, teríamos uma exceção:

![](request-validation/_static/image3.png)

A mensagem de erro informa que um 'potencialmente perigosos Request. Form valor foi detectado' e fornece mais detalhes na descrição de como exatamente o que ocorreu e como alterar o comportamento. Por exemplo:

Validação de solicitação detectou um valor de entrada potencialmente perigosas de cliente e o processamento da solicitação foi anulado. Esse valor pode indicar uma tentativa de comprometer a segurança de seu aplicativo, como um ataque de script entre sites. Você pode desabilitar a validação de solicitação definindo `validateRequest=false` na diretiva de página ou na seção de configuração. No entanto, é altamente recomendável que o aplicativo verifique explicitamente todas as entradas nesse caso.

## <a name="disabling-request-validation-on-a-page"></a>Desabilitar a validação de solicitação em uma página

Para desabilitar a validação de solicitação em uma página, você deve definir a `validateRequest` atributo da diretiva Page para `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Quando a validação de solicitação está desabilitada, o conteúdo pode ser enviado para uma página; é responsabilidade do desenvolvedor da página para garantir que o conteúdo é codificada ou processada corretamente.

## <a name="disabling-request-validation-for-your-application"></a>Desabilitar a validação de solicitação para o seu aplicativo

Para desabilitar a validação de solicitação para seu aplicativo, você deve modificar ou criar um arquivo Web. config para seu aplicativo e defina o atributo validateRequest do `<pages />` seção para `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Se você quiser desabilitar a validação de solicitação para todos os aplicativos no seu servidor, você pode fazer essa modificação ao arquivo Machine. config.

> [!CAUTION]
> Quando a validação de solicitação está desabilitada, conteúdo possa ser enviado ao seu aplicativo. é responsabilidade do desenvolvedor do aplicativo para garantir que o conteúdo é codificada ou processada corretamente.

O código a seguir é modificado para desativar a validação de solicitação:

![](request-validation/_static/image4.png)

Agora, se o seguinte JavaScript foi inserido na caixa de texto `<script>alert("hello!")</script>` o resultado seria:

![](request-validation/_static/image5.png)

Para evitar que isso aconteça, com a validação de solicitação seja desativado, é necessário para HTML codificar o conteúdo.

## <a name="how-to-html-encode-content"></a>Como a HTML codificar conteúdo

Se você tiver desabilitado a validação de solicitação, ele é uma boa prática para o conteúdo a codificação HTML que será armazenada para uso futuro. A codificação HTML automaticamente substituirá qualquer '&lt;'ou'&gt;' (junto com vários outros símbolos) com o HTML correspondente a representação codificada. Por exemplo, '&lt;'é substituída por'&amp;lt;' e '&gt;'é substituída por'&amp;gt;'. Os navegadores usam esses códigos especiais para exibir a '&lt;'ou'&gt;' no navegador.

O conteúdo pode ser facilmente codificada em HTML no servidor usando o `Server.HtmlEncode(string)` API. Conteúdo também pode ser facilmente decodificado para o HTML, ou seja, revertida ao padrão HTML usando o `Server.HtmlDecode(string)` método.

![](request-validation/_static/image6.png)

Resultando em:

![](request-validation/_static/image7.png)
