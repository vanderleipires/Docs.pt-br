---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 21ba186c11f39bbeedd1c320b98476ba13af27e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819154"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="cf81e-102">Cookies HTTP na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cf81e-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="cf81e-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cf81e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cf81e-104">Este tópico descreve como enviar e receber cookies HTTP na API da Web.</span><span class="sxs-lookup"><span data-stu-id="cf81e-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="cf81e-105">Informações básicas sobre Cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="cf81e-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="cf81e-106">Esta seção fornece uma visão geral de como os cookies são implementados no nível HTTP.</span><span class="sxs-lookup"><span data-stu-id="cf81e-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="cf81e-107">Para obter detalhes, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="cf81e-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="cf81e-108">Um cookie é uma parte dos dados enviados de um servidor na resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cf81e-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="cf81e-109">(Opcionalmente) o cliente armazena o cookie e o retorna em subsequet solicitações.</span><span class="sxs-lookup"><span data-stu-id="cf81e-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="cf81e-110">Isso permite que o cliente e servidor compartilhar o estado.</span><span class="sxs-lookup"><span data-stu-id="cf81e-110">This allows the client and server to share state.</span></span> <span data-ttu-id="cf81e-111">Para definir um cookie, o servidor inclui um cabeçalho Set-Cookie na resposta.</span><span class="sxs-lookup"><span data-stu-id="cf81e-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="cf81e-112">O formato de um cookie é um par nome-valor, com atributos opcionais.</span><span class="sxs-lookup"><span data-stu-id="cf81e-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="cf81e-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cf81e-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="cf81e-114">Aqui está um exemplo com atributos:</span><span class="sxs-lookup"><span data-stu-id="cf81e-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="cf81e-115">Para retornar um cookie para o servidor, o cliente inclui um cabeçalho de Cookie em solicitações posteriores.</span><span class="sxs-lookup"><span data-stu-id="cf81e-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="cf81e-116">Uma resposta HTTP pode incluir vários cabeçalhos Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="cf81e-117">O cliente retorna vários cookies usando um único cabeçalho de Cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="cf81e-118">O escopo e a duração de um cookie são controladas por atributos a seguir no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="cf81e-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="cf81e-119">**Domínio**: informa ao cliente qual domínio deve receber o cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="cf81e-120">Por exemplo, se o domínio for "example.com", o cliente retorna o cookie para cada subdomínio de example.com.</span><span class="sxs-lookup"><span data-stu-id="cf81e-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="cf81e-121">Se não for especificado, o domínio é o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="cf81e-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="cf81e-122">**Caminho**: restringe o cookie para o caminho especificado dentro do domínio.</span><span class="sxs-lookup"><span data-stu-id="cf81e-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="cf81e-123">Se não for especificado, o caminho do URI da solicitação é usado.</span><span class="sxs-lookup"><span data-stu-id="cf81e-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="cf81e-124">**Expira**: define uma data de expiração do cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="cf81e-125">O cliente exclui o cookie quando ela expirar.</span><span class="sxs-lookup"><span data-stu-id="cf81e-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="cf81e-126">**Max-Age**: define a idade máxima para o cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="cf81e-127">O cliente exclui o cookie quando ele atinge a idade máxima.</span><span class="sxs-lookup"><span data-stu-id="cf81e-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="cf81e-128">Se os dois `Expires` e `Max-Age` estiverem definidas, `Max-Age` terá precedência.</span><span class="sxs-lookup"><span data-stu-id="cf81e-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="cf81e-129">Se nenhuma for configurada, o cliente exclui o cookie quando termina a sessão atual.</span><span class="sxs-lookup"><span data-stu-id="cf81e-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="cf81e-130">(O significado exato de "sessão" é determinado pelo agente do usuário).</span><span class="sxs-lookup"><span data-stu-id="cf81e-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="cf81e-131">No entanto, lembre-se de que os clientes podem ignorar cookies.</span><span class="sxs-lookup"><span data-stu-id="cf81e-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="cf81e-132">Por exemplo, um usuário pode desabilitar cookies por motivos de privacidade.</span><span class="sxs-lookup"><span data-stu-id="cf81e-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="cf81e-133">Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados.</span><span class="sxs-lookup"><span data-stu-id="cf81e-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="cf81e-134">Por motivos de privacidade, os clientes geralmente rejeitam cookies "third party", em que o domínio não coincide com o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="cf81e-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="cf81e-135">Em resumo, o servidor não deve depender voltando os cookies que ela define.</span><span class="sxs-lookup"><span data-stu-id="cf81e-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="cf81e-136">Cookies no API da Web</span><span class="sxs-lookup"><span data-stu-id="cf81e-136">Cookies in Web API</span></span>

<span data-ttu-id="cf81e-137">Para adicionar um cookie para uma resposta HTTP, crie uma **CookieHeaderValue** instância que representa o cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="cf81e-138">Em seguida, chame o **AddCookies** método de extensão, que é definido no **System.NET. HTTP. HttpResponseHeadersExtensions** classe, para adicionar o cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="cf81e-139">Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="cf81e-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="cf81e-140">Observe que **AddCookies** pega uma matriz de **CookieHeaderValue** instâncias.</span><span class="sxs-lookup"><span data-stu-id="cf81e-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="cf81e-141">Para extrair os cookies de solicitação do cliente, chame o **GetCookies** método:</span><span class="sxs-lookup"><span data-stu-id="cf81e-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="cf81e-142">Um **CookieHeaderValue** contém uma coleção de **CookieState** instâncias.</span><span class="sxs-lookup"><span data-stu-id="cf81e-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="cf81e-143">Cada **CookieState** representa um cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="cf81e-144">Use o método de indexador para obter um **CookieState** por nome, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="cf81e-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="cf81e-145">Dados estruturados de Cookie</span><span class="sxs-lookup"><span data-stu-id="cf81e-145">Structured Cookie Data</span></span>

<span data-ttu-id="cf81e-146">Muitos navegadores limitam quantos cookies eles armazenará&#8212;o número total e o número por domínio.</span><span class="sxs-lookup"><span data-stu-id="cf81e-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="cf81e-147">Portanto, ele pode ser útil colocar dados estruturados em um cookie único, em vez de definir vários cookies.</span><span class="sxs-lookup"><span data-stu-id="cf81e-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="cf81e-148">RFC 6265 não define a estrutura dos dados de cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="cf81e-149">Usando o **CookieHeaderValue** classe, você pode passar uma lista de pares nome-valor para os dados do cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="cf81e-150">Esses pares nome-valor são codificados como dados de formato codificado de URL no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="cf81e-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="cf81e-151">O código anterior produz o seguinte cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="cf81e-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="cf81e-152">O **CookieState** classe fornece um método de indexador para ler os valores inferiores de um cookie na mensagem de solicitação:</span><span class="sxs-lookup"><span data-stu-id="cf81e-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="cf81e-153">Exemplo: Definir e recuperar Cookies em um manipulador de mensagens</span><span class="sxs-lookup"><span data-stu-id="cf81e-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="cf81e-154">Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="cf81e-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="cf81e-155">Outra opção é usar [manipuladores de mensagem](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="cf81e-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="cf81e-156">Manipuladores de mensagens são invocados antes no pipeline que controladores.</span><span class="sxs-lookup"><span data-stu-id="cf81e-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="cf81e-157">Um manipulador de mensagens pode ler cookies da solicitação antes que a solicitação atinja o controlador ou adicionar cookies para a resposta depois que o controlador de gera a resposta.</span><span class="sxs-lookup"><span data-stu-id="cf81e-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="cf81e-158">O código a seguir mostra um manipulador de mensagens para a criação de IDs de sessão.</span><span class="sxs-lookup"><span data-stu-id="cf81e-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="cf81e-159">A ID de sessão é armazenada em um cookie.</span><span class="sxs-lookup"><span data-stu-id="cf81e-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="cf81e-160">O manipulador verifica a solicitação para o cookie de sessão.</span><span class="sxs-lookup"><span data-stu-id="cf81e-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="cf81e-161">Se a solicitação não inclui o cookie, o manipulador gera uma ID de sessão nova.</span><span class="sxs-lookup"><span data-stu-id="cf81e-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="cf81e-162">Em ambos os casos, o manipulador armazena a ID de sessão na **HttpRequestMessage.Properties** recipiente de propriedades.</span><span class="sxs-lookup"><span data-stu-id="cf81e-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="cf81e-163">Ele também adiciona o cookie de sessão para a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cf81e-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="cf81e-164">Essa implementação não valida que a ID de sessão do cliente realmente foi emitida pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="cf81e-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="cf81e-165">Não usá-lo como uma forma de autenticação!</span><span class="sxs-lookup"><span data-stu-id="cf81e-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="cf81e-166">O ponto desse exemplo é mostrar o gerenciamento de cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="cf81e-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="cf81e-167">Um controlador pode obter a ID de sessão do **HttpRequestMessage.Properties** recipiente de propriedades.</span><span class="sxs-lookup"><span data-stu-id="cf81e-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
