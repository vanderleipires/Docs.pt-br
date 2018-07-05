---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368399"
---
<a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como enviar e receber cookies HTTP na API da Web.

## <a name="background-on-http-cookies"></a>Informações básicas sobre Cookies HTTP

Esta seção fornece uma visão geral de como os cookies são implementados no nível HTTP. Para obter detalhes, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).

Um cookie é uma parte dos dados enviados de um servidor na resposta HTTP. (Opcionalmente) o cliente armazena o cookie e o retorna em subsequet solicitações. Isso permite que o cliente e servidor compartilhar o estado. Para definir um cookie, o servidor inclui um cabeçalho Set-Cookie na resposta. O formato de um cookie é um par nome-valor, com atributos opcionais. Por exemplo:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Aqui está um exemplo com atributos:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Para retornar um cookie para o servidor, o cliente inclui um cabeçalho de Cookie em solicitações posteriores.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Uma resposta HTTP pode incluir vários cabeçalhos Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

O cliente retorna vários cookies usando um único cabeçalho de Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

O escopo e a duração de um cookie são controladas por atributos a seguir no cabeçalho Set-Cookie:

- **Domínio**: informa ao cliente qual domínio deve receber o cookie. Por exemplo, se o domínio for "example.com", o cliente retorna o cookie para cada subdomínio de example.com. Se não for especificado, o domínio é o servidor de origem.
- **Caminho**: restringe o cookie para o caminho especificado dentro do domínio. Se não for especificado, o caminho do URI da solicitação é usado.
- **Expira**: define uma data de expiração do cookie. O cliente exclui o cookie quando ela expirar.
- **Max-Age**: define a idade máxima para o cookie. O cliente exclui o cookie quando ele atinge a idade máxima.

Se os dois `Expires` e `Max-Age` estiverem definidas, `Max-Age` terá precedência. Se nenhuma for configurada, o cliente exclui o cookie quando termina a sessão atual. (O significado exato de "sessão" é determinado pelo agente do usuário).

No entanto, lembre-se de que os clientes podem ignorar cookies. Por exemplo, um usuário pode desabilitar cookies por motivos de privacidade. Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados. Por motivos de privacidade, os clientes geralmente rejeitam cookies "third party", em que o domínio não coincide com o servidor de origem. Em resumo, o servidor não deve depender voltando os cookies que ela define.

## <a name="cookies-in-web-api"></a>Cookies no API da Web

Para adicionar um cookie para uma resposta HTTP, crie uma **CookieHeaderValue** instância que representa o cookie. Em seguida, chame o **AddCookies** método de extensão, que é definido no **System.NET. HTTP. HttpResponseHeadersExtensions** classe, para adicionar o cookie.

Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Observe que **AddCookies** pega uma matriz de **CookieHeaderValue** instâncias.

Para extrair os cookies de solicitação do cliente, chame o **GetCookies** método:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Um **CookieHeaderValue** contém uma coleção de **CookieState** instâncias. Cada **CookieState** representa um cookie. Use o método de indexador para obter um **CookieState** por nome, conforme mostrado.

## <a name="structured-cookie-data"></a>Dados estruturados de Cookie

Muitos navegadores limitam quantos cookies eles armazenará&#8212;o número total e o número por domínio. Portanto, ele pode ser útil colocar dados estruturados em um cookie único, em vez de definir vários cookies.

> [!NOTE]
> RFC 6265 não define a estrutura dos dados de cookie.


Usando o **CookieHeaderValue** classe, você pode passar uma lista de pares nome-valor para os dados do cookie. Esses pares nome-valor são codificados como dados de formato codificado de URL no cabeçalho Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

O código anterior produz o seguinte cabeçalho Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

O **CookieState** classe fornece um método de indexador para ler os valores inferiores de um cookie na mensagem de solicitação:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Exemplo: Definir e recuperar Cookies em um manipulador de mensagens

Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web. Outra opção é usar [manipuladores de mensagem](http-message-handlers.md). Manipuladores de mensagens são invocados antes no pipeline que controladores. Um manipulador de mensagens pode ler cookies da solicitação antes que a solicitação atinja o controlador ou adicionar cookies para a resposta depois que o controlador de gera a resposta.

![](http-cookies/_static/image2.png)

O código a seguir mostra um manipulador de mensagens para a criação de IDs de sessão. A ID de sessão é armazenada em um cookie. O manipulador verifica a solicitação para o cookie de sessão. Se a solicitação não inclui o cookie, o manipulador gera uma ID de sessão nova. Em ambos os casos, o manipulador armazena a ID de sessão na **HttpRequestMessage.Properties** recipiente de propriedades. Ele também adiciona o cookie de sessão para a resposta HTTP.

Essa implementação não valida que a ID de sessão do cliente realmente foi emitida pelo servidor. Não usá-lo como uma forma de autenticação! O ponto desse exemplo é mostrar o gerenciamento de cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Um controlador pode obter a ID de sessão do **HttpRequestMessage.Properties** recipiente de propriedades.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
