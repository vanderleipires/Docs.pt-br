---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Habilitar solicitações entre origens em ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Mostra como dar suporte a compartilhamento de recursos entre origens (CORS) na API da Web do ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508375"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="78665-103">Habilitar solicitações entre origens em ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="78665-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="78665-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="78665-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="78665-105">Segurança do navegador impede que uma página da web fazer solicitações do AJAX para outro domínio.</span><span class="sxs-lookup"><span data-stu-id="78665-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="78665-106">Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site.</span><span class="sxs-lookup"><span data-stu-id="78665-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="78665-107">No entanto, às vezes, convém permitir que outros sites chamar a API da web.</span><span class="sxs-lookup"><span data-stu-id="78665-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="78665-108">[Entre o compartilhamento de recursos de origem](http://www.w3.org/TR/cors/) (CORS) é um padrão de W3C que permite que um servidor atenuar a política de mesma origem.</span><span class="sxs-lookup"><span data-stu-id="78665-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="78665-109">Usando CORS, um servidor pode permitir explicitamente algumas solicitações entre origens durante a rejeição de outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="78665-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="78665-110">CORS é mais segura e mais flexível que técnicas anteriores como [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="78665-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="78665-111">Este tutorial mostra como habilitar o CORS em seu aplicativo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="78665-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="78665-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="78665-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="78665-113">Visual Studio 2013 Atualização 2</span><span class="sxs-lookup"><span data-stu-id="78665-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="78665-114">2.2 da API da Web</span><span class="sxs-lookup"><span data-stu-id="78665-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="78665-115">Introdução</span><span class="sxs-lookup"><span data-stu-id="78665-115">Introduction</span></span>

<span data-ttu-id="78665-116">Este tutorial demonstra o que suporte de CORS na API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="78665-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="78665-117">Vamos começar criando dois projetos ASP.NET – uma chamada "serviço Web", que hospeda o controlador de API da Web, e outro chamado "WebClient", que chama o serviço Web.</span><span class="sxs-lookup"><span data-stu-id="78665-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="78665-118">Como os dois aplicativos são hospedados em domínios diferentes, uma solicitação AJAX do WebClient para serviço Web é uma solicitação de entre origens.</span><span class="sxs-lookup"><span data-stu-id="78665-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="78665-119">O que é "Origem mesmo"?</span><span class="sxs-lookup"><span data-stu-id="78665-119">What is "Same Origin"?</span></span>

<span data-ttu-id="78665-120">Duas URLs têm a mesma origem se eles tiverem portas, hosts e esquemas idênticas.</span><span class="sxs-lookup"><span data-stu-id="78665-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="78665-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="78665-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="78665-122">Essas duas URLs têm a mesma origem:</span><span class="sxs-lookup"><span data-stu-id="78665-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="78665-123">Essas URLs têm diferentes origens que anterior dois:</span><span class="sxs-lookup"><span data-stu-id="78665-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="78665-124">`http://example.net` -Domínio diferente</span><span class="sxs-lookup"><span data-stu-id="78665-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="78665-125">`http://example.com:9000/foo.html` -Porta diferente</span><span class="sxs-lookup"><span data-stu-id="78665-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="78665-126">`https://example.com/foo.html` -Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="78665-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="78665-127">`http://www.example.com/foo.html` -Subdomínio diferente</span><span class="sxs-lookup"><span data-stu-id="78665-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="78665-128">Internet Explorer não considera a porta ao comparar as origens.</span><span class="sxs-lookup"><span data-stu-id="78665-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="78665-129">Criar o projeto de serviço Web</span><span class="sxs-lookup"><span data-stu-id="78665-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="78665-130">Esta seção pressupõe que você já sabe como criar projetos Web API.</span><span class="sxs-lookup"><span data-stu-id="78665-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="78665-131">Se não, consulte [guia de Introdução ao ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="78665-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="78665-132">Inicie o Visual Studio e crie um novo **aplicativo Web ASP.NET** projeto.</span><span class="sxs-lookup"><span data-stu-id="78665-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="78665-133">Selecione o **vazio** modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="78665-133">Select the **Empty** project template.</span></span> <span data-ttu-id="78665-134">Em "Adicionar pastas e referências de núcleo", selecione o **API da Web** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="78665-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="78665-135">Opcionalmente, selecione a opção "Host na nuvem" para implantar o aplicativo para o Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="78665-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="78665-136">A Microsoft oferece gratuito da web de hospedagem para até 10 sites em uma [conta de avaliação do Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="78665-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="78665-137">Adicionar um controlador de API da Web chamado `TestController` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="78665-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="78665-138">Você pode executar o aplicativo localmente ou implantar no Azure.</span><span class="sxs-lookup"><span data-stu-id="78665-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="78665-139">(Para as capturas de tela neste tutorial, eu implantado em aplicativos de Web do serviço de aplicativo do Azure.) Para verificar se a API da web está funcionando, navegue até `http://hostname/api/test/`, onde *hostname* é o domínio onde você implantou o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78665-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="78665-140">Você deve ver o texto de resposta, &quot;obter: mensagem de teste&quot;.</span><span class="sxs-lookup"><span data-stu-id="78665-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="78665-141">Criar o projeto de cliente</span><span class="sxs-lookup"><span data-stu-id="78665-141">Create the WebClient Project</span></span>

<span data-ttu-id="78665-142">Crie outro projeto de aplicativo Web do ASP.NET e selecione o **MVC** modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="78665-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="78665-143">Opcionalmente, selecione **alterar autenticação** > **sem autenticação**.</span><span class="sxs-lookup"><span data-stu-id="78665-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="78665-144">Autenticação não é necessário para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="78665-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="78665-145">No Solution Explorer, abra o arquivo Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="78665-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="78665-146">Substitua o código neste arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="78665-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="78665-147">Para o *serviceUrl* variável, use o URI do aplicativo de serviço Web.</span><span class="sxs-lookup"><span data-stu-id="78665-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="78665-148">Agora execute o aplicativo WebClient localmente ou publicá-lo em outro site.</span><span class="sxs-lookup"><span data-stu-id="78665-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="78665-149">Clicar no botão "Experimente" envia uma solicitação AJAX para o aplicativo de serviço Web, usando o método HTTP listado na caixa suspensa (GET, POST ou PUT).</span><span class="sxs-lookup"><span data-stu-id="78665-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="78665-150">Isso nos permite examinar as solicitações entre origens diferentes.</span><span class="sxs-lookup"><span data-stu-id="78665-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="78665-151">Agora, o aplicativo de serviço Web não dá suporte para CORS, portanto, se você clicar no botão, você receberá um erro.</span><span class="sxs-lookup"><span data-stu-id="78665-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="78665-152">Se você observar o tráfego HTTP em uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), você verá que o navegador envia a solicitação de obtenção e a solicitação for bem-sucedida, mas a chamada AJAX retornará um erro.</span><span class="sxs-lookup"><span data-stu-id="78665-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="78665-153">É importante entender a política de mesma origem não impede que o navegador de *enviar* a solicitação.</span><span class="sxs-lookup"><span data-stu-id="78665-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="78665-154">Em vez disso, impede que o aplicativo vendo o *resposta*.</span><span class="sxs-lookup"><span data-stu-id="78665-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="78665-155">Habilitar o CORS</span><span class="sxs-lookup"><span data-stu-id="78665-155">Enable CORS</span></span>

<span data-ttu-id="78665-156">Agora vamos habilitar CORS no aplicativo do serviço Web.</span><span class="sxs-lookup"><span data-stu-id="78665-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="78665-157">Primeiro, adicione o pacote NuGet de CORS.</span><span class="sxs-lookup"><span data-stu-id="78665-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="78665-158">No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="78665-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="78665-159">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="78665-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="78665-160">Esse comando instala o pacote mais recente e atualiza todas as dependências, inclusive as bibliotecas de API da Web principal.</span><span class="sxs-lookup"><span data-stu-id="78665-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="78665-161">Usuário-sinalizador de versão para o destino de uma versão específica.</span><span class="sxs-lookup"><span data-stu-id="78665-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="78665-162">O pacote CORS requer Web API 2.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="78665-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="78665-163">Abra o arquivo de aplicativo\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="78665-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="78665-164">Adicione o seguinte código para o **WebApiConfig.Register** método.</span><span class="sxs-lookup"><span data-stu-id="78665-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="78665-165">Em seguida, adicione o **[EnableCors]** de atributo para o `TestController` classe:</span><span class="sxs-lookup"><span data-stu-id="78665-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="78665-166">Para o *origens* parâmetro, use o URI em que você implantou o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="78665-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="78665-167">Isso permite que solicitações entre origens de WebClient, ao mesmo tempo em que ainda não permitir todas as outras solicitações entre domínios.</span><span class="sxs-lookup"><span data-stu-id="78665-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="78665-168">Posteriormente, irá descrever os parâmetros para **[EnableCors]** mais detalhadamente.</span><span class="sxs-lookup"><span data-stu-id="78665-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="78665-169">Não inclua uma barra invertida no final de *origens* URL.</span><span class="sxs-lookup"><span data-stu-id="78665-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="78665-170">Reimplante o aplicativo de serviço Web atualizado.</span><span class="sxs-lookup"><span data-stu-id="78665-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="78665-171">Você não precisa atualizar WebClient.</span><span class="sxs-lookup"><span data-stu-id="78665-171">You don't need to update WebClient.</span></span> <span data-ttu-id="78665-172">Agora a solicitação AJAX do WebClient deve ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="78665-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="78665-173">Os métodos GET, PUT e POST todos são permitidos.</span><span class="sxs-lookup"><span data-stu-id="78665-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="78665-174">Como funciona o CORS</span><span class="sxs-lookup"><span data-stu-id="78665-174">How CORS Works</span></span>

<span data-ttu-id="78665-175">Esta seção descreve o que acontece em uma solicitação CORS, o nível das mensagens HTTP.</span><span class="sxs-lookup"><span data-stu-id="78665-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="78665-176">É importante compreender o funcionamento de CORS, para que você possa configurar o **[EnableCors]** atributo corretamente e solucionar problemas se as coisas não funcionam conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="78665-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="78665-177">A especificação CORS apresenta vários novos cabeçalhos HTTP que habilitam solicitações entre origens.</span><span class="sxs-lookup"><span data-stu-id="78665-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="78665-178">Se um navegador dá suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens; Você não precisa fazer nada especial em seu código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="78665-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="78665-179">Aqui está um exemplo de uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="78665-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="78665-180">O cabeçalho de "Origem" fornece o domínio do site que está fazendo a solicitação.</span><span class="sxs-lookup"><span data-stu-id="78665-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="78665-181">Se o servidor permite que a solicitação, ele define o cabeçalho Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="78665-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="78665-182">O valor desse cabeçalho corresponde o cabeçalho de origem, tanto é o valor de curinga "\*", que significa que qualquer origem é permitida.</span><span class="sxs-lookup"><span data-stu-id="78665-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="78665-183">Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falhará.</span><span class="sxs-lookup"><span data-stu-id="78665-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="78665-184">Especificamente, o navegador não permite a solicitação.</span><span class="sxs-lookup"><span data-stu-id="78665-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="78665-185">Mesmo se o servidor retornará uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="78665-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="78665-186">**Solicitações de simulação**</span><span class="sxs-lookup"><span data-stu-id="78665-186">**Preflight Requests**</span></span>

<span data-ttu-id="78665-187">Para algumas solicitações CORS, o navegador envia uma solicitação de adicional, chamada uma "solicitação de simulação," antes de enviar a solicitação real do recurso.</span><span class="sxs-lookup"><span data-stu-id="78665-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="78665-188">O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="78665-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="78665-189">O método de solicitação é GET, HEAD ou POST, *e*</span><span class="sxs-lookup"><span data-stu-id="78665-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="78665-190">O aplicativo não definir os cabeçalhos de solicitação diferente de idioma do conteúdo Accept, Accept-Language, Content-Type ou última--ID do evento, *e*</span><span class="sxs-lookup"><span data-stu-id="78665-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="78665-191">O cabeçalho Content-Type (se definido) é um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="78665-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="78665-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="78665-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="78665-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="78665-193">multipart/form-data</span></span>
    - <span data-ttu-id="78665-194">texto/sem formatação</span><span class="sxs-lookup"><span data-stu-id="78665-194">text/plain</span></span>

<span data-ttu-id="78665-195">A regra sobre cabeçalhos de solicitação se aplica aos cabeçalhos que o aplicativo define chamando **setRequestHeader** no **XMLHttpRequest** objeto.</span><span class="sxs-lookup"><span data-stu-id="78665-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="78665-196">(A especificação CORS chama esses cabeçalhos de solicitação"autor".) A regra não se aplica aos cabeçalhos de *navegador* pode definir como o agente do usuário, o Host ou o comprimento do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="78665-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="78665-197">Aqui está um exemplo de uma solicitação de simulação:</span><span class="sxs-lookup"><span data-stu-id="78665-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="78665-198">A solicitação de simulação usa o método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="78665-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="78665-199">Ele inclui dois cabeçalhos especiais:</span><span class="sxs-lookup"><span data-stu-id="78665-199">It includes two special headers:</span></span>

- <span data-ttu-id="78665-200">Access-Control-Request-Method: O método HTTP que será usado para a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="78665-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="78665-201">Access-Control-Request-Headers: Uma lista de cabeçalhos de solicitação que o *aplicativo* definido na solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="78665-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="78665-202">(Novamente, isso não inclui os cabeçalhos que define o navegador.)</span><span class="sxs-lookup"><span data-stu-id="78665-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="78665-203">Aqui está um exemplo de resposta, supondo que o servidor permite que a solicitação:</span><span class="sxs-lookup"><span data-stu-id="78665-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="78665-204">A resposta inclui um cabeçalho de acesso--permitir-métodos de controle que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-permitir-Headers, que lista os cabeçalhos permitidos.</span><span class="sxs-lookup"><span data-stu-id="78665-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="78665-205">Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real, conforme descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="78665-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="78665-206">Regras de escopo para [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="78665-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="78665-207">Você pode habilitar o CORS por ação, por controlador ou globalmente para todos os controladores de API da Web em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78665-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="78665-208">**Cada ação**</span><span class="sxs-lookup"><span data-stu-id="78665-208">**Per Action**</span></span>

<span data-ttu-id="78665-209">Para habilitar o CORS para uma única ação, defina o **[EnableCors]** atributo no método de ação.</span><span class="sxs-lookup"><span data-stu-id="78665-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="78665-210">O exemplo a seguir habilita o CORS para o `GetItem` método apenas.</span><span class="sxs-lookup"><span data-stu-id="78665-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="78665-211">**Por controlador**</span><span class="sxs-lookup"><span data-stu-id="78665-211">**Per Controller**</span></span>

<span data-ttu-id="78665-212">Se você definir **[EnableCors]** na classe de controlador, ela se aplica a todas as ações no controlador.</span><span class="sxs-lookup"><span data-stu-id="78665-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="78665-213">Para desabilitar CORS para uma ação, adicione o **[DisableCors]** de atributo para a ação.</span><span class="sxs-lookup"><span data-stu-id="78665-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="78665-214">O exemplo a seguir habilita o CORS para cada método, exceto `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="78665-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="78665-215">**Globalmente**</span><span class="sxs-lookup"><span data-stu-id="78665-215">**Globally**</span></span>

<span data-ttu-id="78665-216">Para habilitar o CORS para todos os controladores de API da Web em seu aplicativo, passe um **EnableCorsAttribute** de instância para o **EnableCors** método:</span><span class="sxs-lookup"><span data-stu-id="78665-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="78665-217">Se você definir o atributo em mais de um escopo, a ordem de precedência é:</span><span class="sxs-lookup"><span data-stu-id="78665-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="78665-218">Ação</span><span class="sxs-lookup"><span data-stu-id="78665-218">Action</span></span>
2. <span data-ttu-id="78665-219">Controlador</span><span class="sxs-lookup"><span data-stu-id="78665-219">Controller</span></span>
3. <span data-ttu-id="78665-220">Global</span><span class="sxs-lookup"><span data-stu-id="78665-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="78665-221">Definir as origens permitidas</span><span class="sxs-lookup"><span data-stu-id="78665-221">Set the Allowed Origins</span></span>

<span data-ttu-id="78665-222">O *origens* parâmetro o **[EnableCors]** atributo especifica quais origens são permitidas para acessar o recurso.</span><span class="sxs-lookup"><span data-stu-id="78665-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="78665-223">O valor é uma lista separada por vírgulas das origens permitidas.</span><span class="sxs-lookup"><span data-stu-id="78665-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="78665-224">Você também pode usar o valor de curinga "\*" para permitir solicitações de qualquer origem.</span><span class="sxs-lookup"><span data-stu-id="78665-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="78665-225">Considere cuidadosamente antes de permitir solicitações de qualquer origem.</span><span class="sxs-lookup"><span data-stu-id="78665-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="78665-226">Isso significa que literalmente qualquer site pode fazer chamadas AJAX para sua API da web.</span><span class="sxs-lookup"><span data-stu-id="78665-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="78665-227">Definir os métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="78665-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="78665-228">O *métodos* parâmetro o **[EnableCors]** atributo especifica quais métodos HTTP têm permissão para acessar o recurso.</span><span class="sxs-lookup"><span data-stu-id="78665-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="78665-229">Para permitir que todos os métodos, use o valor de curinga "\*".</span><span class="sxs-lookup"><span data-stu-id="78665-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="78665-230">O exemplo a seguir permite que somente solicitações GET e POST.</span><span class="sxs-lookup"><span data-stu-id="78665-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="78665-231">Definir os cabeçalhos de solicitação permitido</span><span class="sxs-lookup"><span data-stu-id="78665-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="78665-232">Anteriormente descrito como uma solicitação de simulação pode incluir um cabeçalho Access-Control-Request-Headers, listando os cabeçalhos HTTP definidos pelo aplicativo (os chamados "author cabeçalhos de solicitação").</span><span class="sxs-lookup"><span data-stu-id="78665-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="78665-233">O *cabeçalhos* parâmetro o **[EnableCors]** atributo especifica quais cabeçalhos de solicitação de autor são permitidos.</span><span class="sxs-lookup"><span data-stu-id="78665-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="78665-234">Para permitir que todos os cabeçalhos, defina *cabeçalhos* para "\*".</span><span class="sxs-lookup"><span data-stu-id="78665-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="78665-235">A lista branca de cabeçalhos específicos, defina *cabeçalhos* para uma lista separada por vírgulas dos cabeçalhos permitidos:</span><span class="sxs-lookup"><span data-stu-id="78665-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="78665-236">No entanto, os navegadores não são totalmente consistentes em como eles definidos Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="78665-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="78665-237">Por exemplo, o Chrome atualmente inclui "origem"; enquanto o FireFox não incluir cabeçalhos padrão como "Aceitar", mesmo quando o aplicativo define-los no script.</span><span class="sxs-lookup"><span data-stu-id="78665-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="78665-238">Se você definir *cabeçalhos* para algo diferente de "\*", você deve incluir pelo menos "aceitar", "content-type" e "origem", além de quaisquer cabeçalhos personalizados que você deseja dar suporte.</span><span class="sxs-lookup"><span data-stu-id="78665-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="78665-239">Definir os cabeçalhos de resposta permitidos</span><span class="sxs-lookup"><span data-stu-id="78665-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="78665-240">Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78665-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="78665-241">Os cabeçalhos de resposta que estão disponíveis por padrão são:</span><span class="sxs-lookup"><span data-stu-id="78665-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="78665-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="78665-242">Cache-Control</span></span>
- <span data-ttu-id="78665-243">Idioma do conteúdo</span><span class="sxs-lookup"><span data-stu-id="78665-243">Content-Language</span></span>
- <span data-ttu-id="78665-244">Tipo de conteúdo</span><span class="sxs-lookup"><span data-stu-id="78665-244">Content-Type</span></span>
- <span data-ttu-id="78665-245">Expirar</span><span class="sxs-lookup"><span data-stu-id="78665-245">Expires</span></span>
- <span data-ttu-id="78665-246">Última modificação</span><span class="sxs-lookup"><span data-stu-id="78665-246">Last-Modified</span></span>
- <span data-ttu-id="78665-247">Pragma</span><span class="sxs-lookup"><span data-stu-id="78665-247">Pragma</span></span>

<span data-ttu-id="78665-248">A especificação CORS chama esses [cabeçalhos de resposta simples](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="78665-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="78665-249">Para disponibilizar outros cabeçalhos para o aplicativo, defina o *exposedHeaders* parâmetro **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="78665-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="78665-250">No exemplo a seguir, o controlador `Get` método define um cabeçalho personalizado chamado 'X-personalizado-cabeçalho'.</span><span class="sxs-lookup"><span data-stu-id="78665-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="78665-251">Por padrão, o navegador não irá expor esse cabeçalho em uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="78665-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="78665-252">Para disponibilizar o cabeçalho, inclua 'X-personalizado-cabeçalho' no *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="78665-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="78665-253">Passagem de credenciais nas solicitações entre origens</span><span class="sxs-lookup"><span data-stu-id="78665-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="78665-254">Credenciais requerem tratamento especial em uma solicitação CORS.</span><span class="sxs-lookup"><span data-stu-id="78665-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="78665-255">Por padrão, o navegador não envia credenciais com uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="78665-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="78665-256">As credenciais incluem cookies, bem como os esquemas de autenticação HTTP.</span><span class="sxs-lookup"><span data-stu-id="78665-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="78665-257">Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir **XMLHttpRequest.withCredentials** como true.</span><span class="sxs-lookup"><span data-stu-id="78665-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="78665-258">Usando **XMLHttpRequest** diretamente:</span><span class="sxs-lookup"><span data-stu-id="78665-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="78665-259">Em jQuery:</span><span class="sxs-lookup"><span data-stu-id="78665-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="78665-260">Além disso, o servidor deve permitir que as credenciais.</span><span class="sxs-lookup"><span data-stu-id="78665-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="78665-261">Para permitir que as credenciais de entre origens na API da Web, defina o **SupportsCredentials** propriedade como true se o **[EnableCors]** atributo:</span><span class="sxs-lookup"><span data-stu-id="78665-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="78665-262">Se essa propriedade for true, a resposta HTTP incluirá um cabeçalho Access-controle-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="78665-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="78665-263">Esse cabeçalho informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="78665-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="78665-264">Se o navegador envia as credenciais, mas a resposta não incluir um cabeçalho Access-controle-Allow-Credentials válido, o navegador não irá expor a resposta para o aplicativo e haverá falha na solicitação AJAX.</span><span class="sxs-lookup"><span data-stu-id="78665-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="78665-265">Tenha muito cuidado sobre configuração **SupportsCredentials** como true, pois isso significa que um site da Web em outro domínio pode enviar credenciais do usuário conectado à API da Web em nome do usuário, sem o conhecimento do usuário.</span><span class="sxs-lookup"><span data-stu-id="78665-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="78665-266">A especificação CORS estados também essa configuração *origens* para &quot; \* &quot; não é válido se **SupportsCredentials** é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="78665-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="78665-267">Provedores de política CORS personalizado</span><span class="sxs-lookup"><span data-stu-id="78665-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="78665-268">O **[EnableCors]** atributo implementa o **ICorsPolicyProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="78665-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="78665-269">Você pode fornecer sua própria implementação, criando uma classe que deriva de **atributo** e implementa **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="78665-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="78665-270">Agora você pode aplicar o atributo de qualquer lugar que você colocaria **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="78665-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="78665-271">Por exemplo, um provedor personalizado de política CORS pode ler as configurações de um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="78665-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="78665-272">Como alternativa ao uso de atributos, você pode registrar um **ICorsPolicyProviderFactory** objeto cria **ICorsPolicyProvider** objetos.</span><span class="sxs-lookup"><span data-stu-id="78665-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="78665-273">Para definir o **ICorsPolicyProviderFactory**, chame o **SetCorsPolicyProviderFactory** método de extensão na inicialização, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="78665-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="78665-274">Suporte a navegador</span><span class="sxs-lookup"><span data-stu-id="78665-274">Browser Support</span></span>

<span data-ttu-id="78665-275">O pacote de Web API CORS é uma tecnologia de servidor.</span><span class="sxs-lookup"><span data-stu-id="78665-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="78665-276">O navegador do usuário também precisa dar suporte a CORS.</span><span class="sxs-lookup"><span data-stu-id="78665-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="78665-277">Felizmente, as versões atuais de todos os principais navegadores incluem [suporte para CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="78665-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="78665-278">Internet Explorer 8 e Internet Explorer 9 têm suporte parcial para CORS, usando o objeto herdado do XDomainRequest em vez de XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="78665-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="78665-279">Para obter mais informações, consulte [XDomainRequest - restrições, limitações e soluções alternativas](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="78665-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
