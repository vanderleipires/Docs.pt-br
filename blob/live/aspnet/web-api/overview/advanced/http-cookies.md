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
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="70b71-102">Cookies HTTP na API da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="70b71-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="70b71-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="70b71-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="70b71-104">Este tópico descreve como enviar e receber cookies HTTP na API da Web.</span><span class="sxs-lookup"><span data-stu-id="70b71-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="70b71-105">Plano de fundo em Cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="70b71-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="70b71-106">Esta seção fornece uma visão geral de como os cookies são implementados no nível de HTTP.</span><span class="sxs-lookup"><span data-stu-id="70b71-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="70b71-107">Para obter detalhes, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="70b71-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="70b71-108">Um cookie é uma parte dos dados enviados de um servidor na resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="70b71-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="70b71-109">(Opcionalmente) o cliente armazena o cookie e o retorna em solicitações de subsequet.</span><span class="sxs-lookup"><span data-stu-id="70b71-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="70b71-110">Isso permite que o cliente e o servidor compartilhar o estado.</span><span class="sxs-lookup"><span data-stu-id="70b71-110">This allows the client and server to share state.</span></span> <span data-ttu-id="70b71-111">Para definir um cookie, o servidor inclui um cabeçalho Set-Cookie na resposta.</span><span class="sxs-lookup"><span data-stu-id="70b71-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="70b71-112">O formato de um cookie é um par nome-valor, com atributos opcionais.</span><span class="sxs-lookup"><span data-stu-id="70b71-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="70b71-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70b71-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="70b71-114">Aqui está um exemplo com os atributos:</span><span class="sxs-lookup"><span data-stu-id="70b71-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="70b71-115">Para retornar um cookie para o servidor, o cliente inclui um cabeçalho de Cookie em solicitações posteriores.</span><span class="sxs-lookup"><span data-stu-id="70b71-115">To return a cookie to the server, the client inclues a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="70b71-116">Uma resposta HTTP pode incluir vários cabeçalhos Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="70b71-117">O cliente retorna vários cookies usando um único cabeçalho de Cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="70b71-118">O escopo e a duração de um cookie são controladas por atributos a seguir no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="70b71-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="70b71-119">**Domínio**: informa ao cliente qual domínio deve receber o cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="70b71-120">Por exemplo, se o domínio for "c o m", o cliente retorna o cookie para cada subdomínio de example.com. Se não for especificado, o domínio é o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="70b71-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="70b71-121">**Caminho**: restringe o cookie para o caminho especificado dentro do domínio.</span><span class="sxs-lookup"><span data-stu-id="70b71-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="70b71-122">Se não for especificado, o caminho do URI de solicitação é usado.</span><span class="sxs-lookup"><span data-stu-id="70b71-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="70b71-123">**Expirar**: define uma data de validade para o cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="70b71-124">O cliente exclui o cookie quando ela expirar.</span><span class="sxs-lookup"><span data-stu-id="70b71-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="70b71-125">**Max-Age**: define a duração máxima para o cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="70b71-126">O cliente exclui o cookie ao atingir a duração máxima.</span><span class="sxs-lookup"><span data-stu-id="70b71-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="70b71-127">Se ambos os `Expires` e `Max-Age` são definidos, `Max-Age` terá precedência.</span><span class="sxs-lookup"><span data-stu-id="70b71-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="70b71-128">Se não for definido, o cliente exclui o cookie quando termina a sessão atual.</span><span class="sxs-lookup"><span data-stu-id="70b71-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="70b71-129">(O significado exato de "sessão" é determinado pelo agente do usuário).</span><span class="sxs-lookup"><span data-stu-id="70b71-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="70b71-130">No entanto, lembre-se de que os clientes podem ignorar cookies.</span><span class="sxs-lookup"><span data-stu-id="70b71-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="70b71-131">Por exemplo, um usuário pode desabilitar os cookies por motivos de privacidade.</span><span class="sxs-lookup"><span data-stu-id="70b71-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="70b71-132">Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados.</span><span class="sxs-lookup"><span data-stu-id="70b71-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="70b71-133">Por motivos de privacidade, os clientes frequentemente rejeitam cookies "third party", onde o domínio não coincide com o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="70b71-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="70b71-134">Em resumo, o servidor não deve depender voltando os cookies que ele define.</span><span class="sxs-lookup"><span data-stu-id="70b71-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="70b71-135">Cookies na API da Web</span><span class="sxs-lookup"><span data-stu-id="70b71-135">Cookies in Web API</span></span>

<span data-ttu-id="70b71-136">Para adicionar um cookie para uma resposta HTTP, crie um **CookieHeaderValue** instância que representa o cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="70b71-137">Em seguida, chame o **AddCookies** método de extensão, que é definido no **System. HttpResponseHeadersExtensions** class, para adicionar o cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="70b71-138">Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="70b71-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="70b71-139">Observe que **AddCookies** pega uma matriz de **CookieHeaderValue** instâncias.</span><span class="sxs-lookup"><span data-stu-id="70b71-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="70b71-140">Para extrair os cookies de uma solicitação de cliente, chame o **GetCookies** método:</span><span class="sxs-lookup"><span data-stu-id="70b71-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="70b71-141">Um **CookieHeaderValue** contém uma coleção de **CookieState** instâncias.</span><span class="sxs-lookup"><span data-stu-id="70b71-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="70b71-142">Cada **CookieState** representa um cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="70b71-143">Use o método de indexador para obter um **CookieState** por nome, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="70b71-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="70b71-144">Dados do Cookie estruturado</span><span class="sxs-lookup"><span data-stu-id="70b71-144">Structured Cookie Data</span></span>

<span data-ttu-id="70b71-145">Muitos navegadores limitam quantos cookies que armazenarão &#8212; o número total, tanto o número por domínio.</span><span class="sxs-lookup"><span data-stu-id="70b71-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="70b71-146">Portanto, ele pode ser útil colocar dados estruturados em um único cookie, em vez de configurar vários cookies.</span><span class="sxs-lookup"><span data-stu-id="70b71-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="70b71-147">RFC 6265 não define a estrutura de dados do cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="70b71-148">Usando o **CookieHeaderValue** classe, você pode passar uma lista de pares nome-valor para os dados do cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="70b71-149">Esses pares de nome-valor são codificadas como dados de formato codificado de URL no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="70b71-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="70b71-150">O código anterior produz o seguinte cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="70b71-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="70b71-151">O **CookieState** classe fornece um método de indexador para ler os valores de subgrupos de um cookie na mensagem de solicitação:</span><span class="sxs-lookup"><span data-stu-id="70b71-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="70b71-152">Exemplo: Definir e recuperar Cookies em um manipulador de mensagens</span><span class="sxs-lookup"><span data-stu-id="70b71-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="70b71-153">Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="70b71-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="70b71-154">Outra opção é usar [manipuladores de mensagens](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="70b71-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="70b71-155">Manipuladores de mensagens são invocados anteriormente no pipeline de controladores.</span><span class="sxs-lookup"><span data-stu-id="70b71-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="70b71-156">Um manipulador de mensagens pode ler cookies a solicitação antes que a solicitação atinja o controlador ou adicionar cookies para a resposta depois que o controlador de gera a resposta.</span><span class="sxs-lookup"><span data-stu-id="70b71-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="70b71-157">O código a seguir mostra um manipulador de mensagens para a criação de IDs da sessão.</span><span class="sxs-lookup"><span data-stu-id="70b71-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="70b71-158">A ID de sessão é armazenada em um cookie.</span><span class="sxs-lookup"><span data-stu-id="70b71-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="70b71-159">O manipulador verifica a solicitação para o cookie de sessão.</span><span class="sxs-lookup"><span data-stu-id="70b71-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="70b71-160">Se a solicitação não inclui o cookie, o manipulador gera uma ID da nova sessão.</span><span class="sxs-lookup"><span data-stu-id="70b71-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="70b71-161">Em ambos os casos, o manipulador armazena a ID de sessão de **HttpRequestMessage.Properties** recipiente de propriedades.</span><span class="sxs-lookup"><span data-stu-id="70b71-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="70b71-162">Ele também adiciona o cookie de sessão para a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="70b71-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="70b71-163">Esta implementação não valida que a ID de sessão do cliente, na verdade, foi emitida pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="70b71-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="70b71-164">Não usá-lo como uma forma de autenticação!</span><span class="sxs-lookup"><span data-stu-id="70b71-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="70b71-165">O ponto do exemplo é mostrar o gerenciamento de cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="70b71-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="70b71-166">Um controlador pode obter a ID de sessão do **HttpRequestMessage.Properties** recipiente de propriedades.</span><span class="sxs-lookup"><span data-stu-id="70b71-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
