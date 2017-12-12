---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como enviar e receber cookies HTTP na API da Web.

## <a name="background-on-http-cookies"></a>Plano de fundo em Cookies HTTP

Esta seção fornece uma visão geral de como os cookies são implementados no nível de HTTP. Para obter detalhes, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).

Um cookie é uma parte dos dados enviados de um servidor na resposta HTTP. (Opcionalmente) o cliente armazena o cookie e o retorna em solicitações de subsequet. Isso permite que o cliente e o servidor compartilhar o estado. Para definir um cookie, o servidor inclui um cabeçalho Set-Cookie na resposta. O formato de um cookie é um par nome-valor, com atributos opcionais. Por exemplo:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Aqui está um exemplo com os atributos:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Para retornar um cookie para o servidor, o cliente inclui um cabeçalho de Cookie em solicitações posteriores.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Uma resposta HTTP pode incluir vários cabeçalhos Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

O cliente retorna vários cookies usando um único cabeçalho de Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

O escopo e a duração de um cookie são controladas por atributos a seguir no cabeçalho Set-Cookie:

- **Domínio**: informa ao cliente qual domínio deve receber o cookie. Por exemplo, se o domínio for "c o m", o cliente retorna o cookie para cada subdomínio de example.com. Se não for especificado, o domínio é o servidor de origem.
- **Caminho**: restringe o cookie para o caminho especificado dentro do domínio. Se não for especificado, o caminho do URI de solicitação é usado.
- **Expirar**: define uma data de validade para o cookie. O cliente exclui o cookie quando ela expirar.
- **Max-Age**: define a duração máxima para o cookie. O cliente exclui o cookie ao atingir a duração máxima.

Se ambos os `Expires` e `Max-Age` são definidos, `Max-Age` terá precedência. Se não for definido, o cliente exclui o cookie quando termina a sessão atual. (O significado exato de "sessão" é determinado pelo agente do usuário).

No entanto, lembre-se de que os clientes podem ignorar cookies. Por exemplo, um usuário pode desabilitar os cookies por motivos de privacidade. Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados. Por motivos de privacidade, os clientes frequentemente rejeitam cookies "third party", onde o domínio não coincide com o servidor de origem. Em resumo, o servidor não deve depender voltando os cookies que ele define.

## <a name="cookies-in-web-api"></a>Cookies na API da Web

Para adicionar um cookie para uma resposta HTTP, crie um **CookieHeaderValue** instância que representa o cookie. Em seguida, chame o **AddCookies** método de extensão, que é definido no **System. HttpResponseHeadersExtensions** class, para adicionar o cookie.

Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Observe que **AddCookies** pega uma matriz de **CookieHeaderValue** instâncias.

Para extrair os cookies de uma solicitação de cliente, chame o **GetCookies** método:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Um **CookieHeaderValue** contém uma coleção de **CookieState** instâncias. Cada **CookieState** representa um cookie. Use o método de indexador para obter um **CookieState** por nome, conforme mostrado.

## <a name="structured-cookie-data"></a>Dados do Cookie estruturado

Muitos navegadores limitam quantos cookies que armazenarão &#8212; o número total, tanto o número por domínio. Portanto, ele pode ser útil colocar dados estruturados em um único cookie, em vez de configurar vários cookies.

> [!NOTE]
> RFC 6265 não define a estrutura de dados do cookie.


Usando o **CookieHeaderValue** classe, você pode passar uma lista de pares nome-valor para os dados do cookie. Esses pares de nome-valor são codificadas como dados de formato codificado de URL no cabeçalho Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

O código anterior produz o seguinte cabeçalho Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

O **CookieState** classe fornece um método de indexador para ler os valores de subgrupos de um cookie na mensagem de solicitação:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Exemplo: Definir e recuperar Cookies em um manipulador de mensagens

Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web. Outra opção é usar [manipuladores de mensagens](http-message-handlers.md). Manipuladores de mensagens são invocados anteriormente no pipeline de controladores. Um manipulador de mensagens pode ler cookies a solicitação antes que a solicitação atinja o controlador ou adicionar cookies para a resposta depois que o controlador de gera a resposta.

![](http-cookies/_static/image2.png)

O código a seguir mostra um manipulador de mensagens para a criação de IDs da sessão. A ID de sessão é armazenada em um cookie. O manipulador verifica a solicitação para o cookie de sessão. Se a solicitação não inclui o cookie, o manipulador gera uma ID da nova sessão. Em ambos os casos, o manipulador armazena a ID de sessão de **HttpRequestMessage.Properties** recipiente de propriedades. Ele também adiciona o cookie de sessão para a resposta HTTP.

Esta implementação não valida que a ID de sessão do cliente, na verdade, foi emitida pelo servidor. Não usá-lo como uma forma de autenticação! O ponto do exemplo é mostrar o gerenciamento de cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Um controlador pode obter a ID de sessão do **HttpRequestMessage.Properties** recipiente de propriedades.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
