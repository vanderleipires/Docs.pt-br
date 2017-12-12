---
uid: whitepapers/request-validation
title: "Validação - impedindo ataques de Script de solicitação | Microsoft Docs"
author: rick-anderson
description: "Este documento descreve o recurso de validação de solicitação do ASP.NET, onde, por padrão, o aplicativo será impedido de processamento submitt de conteúdo HTML sem codificação..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 61a96b75fdc29bdd1510ed689ee0356ef30e03fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="request-validation---preventing-script-attacks"></a>Validação - impedindo ataques de Script de solicitação
====================
> Este documento descreve o recurso de validação de solicitação do ASP.NET, onde, por padrão, o aplicativo será impedido de processamento de conteúdo HTML não codificado enviado ao servidor. Esse recurso de validação de solicitação pode ser desabilitado quando o aplicativo foi projetado para processar com segurança dados HTML.
> 
> Aplica-se a ASP.NET 1.1 e o ASP.NET 2.0.


Validação de solicitação, um recurso do ASP.NET desde a versão 1.1, impede que o servidor aceite conteúdo HTML não codificado recipiente. Esse recurso é criado para ajudar a impedir que alguns ataques de injeção de script no qual o código de script do cliente ou HTML pode ser inadvertidamente enviada para um servidor, armazenado e apresentado a outros usuários. Ainda recomendamos que você validar a entrada de todos os dados e codificação HTML, quando apropriado.

Por exemplo, você pode criar uma página da Web que solicita o endereço de email do usuário e, em seguida, armazena esse endereço de email em um banco de dados. Se o usuário insere &lt;SCRIPT&gt;alerta ("Olá do script")&lt;/SCRIPT&gt; em vez de um endereço de email válido quando dados são apresentados, esse script pode ser executado se o conteúdo não foi devidamente codificado. O recurso de validação de solicitação do ASP.NET impede que isso aconteça.

## <a name="why-this-feature-is-useful"></a>Por que esse recurso é útil

Muitos sites não estão cientes de que eles estão abertos a ataques de injeção de script simples. Se a finalidade desses ataques é distorcer o site exibindo HTML ou potencialmente executar script de cliente para redirecionar o usuário ao site do hacker, ataques de injeção de script são um problema que os desenvolvedores da Web devem ser seguidas.

Ataques de injeção de script são uma preocupação de todos os desenvolvedores da web, se estiverem usando ASP.NET, ASP ou outras tecnologias de desenvolvimento na web.

O recurso de validação de solicitação ASP.NET proativamente impede que esses ataques não permitindo que o conteúdo HTML não codificado a ser processado pelo servidor, a menos que o desenvolvedor opta por permitir que o conteúdo.

## <a name="what-to-expect-error-page"></a>O que esperar: página de erro

Captura de tela abaixo mostra um exemplo de código ASP.NET:

![](request-validation/_static/image1.png)

Executando a resultados de código em uma página simple que permite que você digite algum texto na caixa de texto, clique no botão e exibir o texto no controle de rótulo:

![](request-validation/_static/image2.png)

No entanto, foram JavaScript, como `<script>alert("hello!")</script>` seja inserida e enviada se uma exceção:

![](request-validation/_static/image3.png)

A mensagem de erro indica que um 'potencialmente perigosos Request valor foi detectado' e fornece mais detalhes na descrição e exatamente o que aconteceu e como alterar o comportamento. Por exemplo:

Validação de solicitação detectou um valor de entrada de cliente possivelmente perigoso e o processamento da solicitação foi anulado. Esse valor pode indicar uma tentativa de comprometer a segurança de seu aplicativo, como um ataque de script entre sites. Você pode desabilitar a validação de solicitação definindo `validateRequest=false` na diretiva de página ou na seção de configuração. No entanto, é altamente recomendável que o aplicativo verifique explicitamente todas as entradas nesse caso.

## <a name="disabling-request-validation-on-a-page"></a>Desabilitar a validação de solicitação em uma página

Para desabilitar a validação de solicitação em uma página, você deve definir o `validateRequest` atributo da diretiva de página para `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Quando a validação de solicitação está desabilitada, conteúdo pode ser enviado a uma página. é responsabilidade do desenvolvedor de página para garantir que o conteúdo está codificada ou processada corretamente.

## <a name="disabling-request-validation-for-your-application"></a>Desabilitar a validação de solicitação para o seu aplicativo

Para desabilitar a validação de solicitação para o seu aplicativo, você deve modificar ou criar um arquivo Web. config para seu aplicativo e defina o atributo validateRequest do `<pages />` seção `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Se você quiser desabilitar a validação de solicitação para todos os aplicativos no seu servidor, você pode fazer a modificação do seu arquivo Machine. config.

> [!CAUTION]
> Quando a validação de solicitação está desabilitada, conteúdo pode ser enviado ao seu aplicativo. é responsabilidade do desenvolvedor do aplicativo para garantir que o conteúdo é codificada ou processada corretamente.

O código a seguir é modificado para desativar a validação de solicitação:

![](request-validation/_static/image4.png)

Agora, se o JavaScript a seguir foi inserido na caixa de texto `<script>alert("hello!")</script>` o resultado seria:

![](request-validation/_static/image5.png)

Para evitar que isso aconteça, com a validação de solicitação desativada, é necessário para HTML codificar o conteúdo.

## <a name="how-to-html-encode-content"></a>Como HTML codificar o conteúdo

Se você tiver desabilitado a validação de solicitação, é uma boa prática conteúdo codificação HTML que será armazenado para uso futuro. Codificação HTML automaticamente substituirá qualquer '&lt;'ou'&gt;' (juntamente com vários outros símbolos) com HTML correspondente a representação codificada. Por exemplo, '&lt;'é substituído por'&amp;lt;' e '&gt;'é substituído por'&amp;gt;'. Os navegadores usam esses códigos especiais para exibir a '&lt;'ou'&gt;' no navegador.

O conteúdo pode ser facilmente codificada em HTML no servidor usando o `Server.HtmlEncode(string)` API. Conteúdo também pode ser facilmente decodificado para o HTML, ou seja, revertido para HTML padrão usando o `Server.HtmlDecode(string)` método.

![](request-validation/_static/image6.png)

Resultando em:

![](request-validation/_static/image7.png)
