---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Permitindo solicitações entre origens na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Mostra como dar suporte a compartilhamento de recursos entre origens (CORS) na API Web ASP.NET.
ms.author: riande
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc95c39af0821c2f456f5a312de5532c5aeb3c10
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912196"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="3364e-103">Permitindo solicitações entre origens na API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="3364e-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3364e-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3364e-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3364e-105">Segurança do navegador impede que uma página da web faça solicitações AJAX para outro domínio.</span><span class="sxs-lookup"><span data-stu-id="3364e-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="3364e-106">Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site.</span><span class="sxs-lookup"><span data-stu-id="3364e-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="3364e-107">No entanto, às vezes, talvez queira permitir que outros sites chamar sua API da web.</span><span class="sxs-lookup"><span data-stu-id="3364e-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="3364e-108">[Entre o compartilhamento de recursos de origem](http://www.w3.org/TR/cors/) (CORS) é um padrão W3C que permite que um servidor relaxar a política de mesma origem.</span><span class="sxs-lookup"><span data-stu-id="3364e-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="3364e-109">Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens enquanto rejeita outras.</span><span class="sxs-lookup"><span data-stu-id="3364e-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="3364e-110">O CORS é mais seguro e flexível que técnicas anteriores, como [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="3364e-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="3364e-111">Este tutorial mostra como habilitar o CORS em seu aplicativo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="3364e-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3364e-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="3364e-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="3364e-113">Visual Studio 2013 Atualização 2</span><span class="sxs-lookup"><span data-stu-id="3364e-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3364e-114">API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="3364e-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="3364e-115">Introdução</span><span class="sxs-lookup"><span data-stu-id="3364e-115">Introduction</span></span>

<span data-ttu-id="3364e-116">Este tutorial demonstra o que suporte a CORS na API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3364e-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="3364e-117">Vamos começar criando dois projetos do ASP.NET – um chamado "serviço Web", que hospeda um controlador de API da Web, e outro chamado "WebClient", que chama o serviço Web.</span><span class="sxs-lookup"><span data-stu-id="3364e-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="3364e-118">Porque os dois aplicativos hospedados em diferentes domínios, uma solicitação de AJAX do WebClient para o serviço Web é uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="3364e-119">O que é "Mesma origem"?</span><span class="sxs-lookup"><span data-stu-id="3364e-119">What is "Same Origin"?</span></span>

<span data-ttu-id="3364e-120">Duas URLs têm a mesma origem se eles tiverem as portas, hosts e esquemas idênticas.</span><span class="sxs-lookup"><span data-stu-id="3364e-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="3364e-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="3364e-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="3364e-122">Essas duas URLs têm a mesma origem:</span><span class="sxs-lookup"><span data-stu-id="3364e-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="3364e-123">Essas URLs tem diferentes origens que o anterior dois:</span><span class="sxs-lookup"><span data-stu-id="3364e-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="3364e-124">`http://example.net` -Domínio diferente</span><span class="sxs-lookup"><span data-stu-id="3364e-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="3364e-125">`http://example.com:9000/foo.html` -Porta diferente</span><span class="sxs-lookup"><span data-stu-id="3364e-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="3364e-126">`https://example.com/foo.html` -Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="3364e-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="3364e-127">`http://www.example.com/foo.html` -Subdomínio diferente</span><span class="sxs-lookup"><span data-stu-id="3364e-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="3364e-128">Internet Explorer não considera a porta ao comparar as origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="3364e-129">Criar o projeto WebService</span><span class="sxs-lookup"><span data-stu-id="3364e-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="3364e-130">Esta seção pressupõe que você já sabe como criar projetos de API da Web.</span><span class="sxs-lookup"><span data-stu-id="3364e-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="3364e-131">Caso contrário, consulte [Introdução ao ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3364e-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="3364e-132">Inicie o Visual Studio e crie um novo **aplicativo Web ASP.NET** projeto.</span><span class="sxs-lookup"><span data-stu-id="3364e-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="3364e-133">Selecione o **vazio** modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="3364e-133">Select the **Empty** project template.</span></span> <span data-ttu-id="3364e-134">Em "Adicionar pastas e core references for", selecione a **API Web** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="3364e-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="3364e-135">Opcionalmente, selecione a opção de "Host na nuvem" para implantar o aplicativo para o Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3364e-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="3364e-136">A Microsoft oferece a hospedagem de web gratuita para até 10 sites em uma [conta gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="3364e-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="3364e-137">Adicionar um controlador de API da Web chamado `TestController` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3364e-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="3364e-138">Você pode executar o aplicativo localmente ou implantar no Azure.</span><span class="sxs-lookup"><span data-stu-id="3364e-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="3364e-139">(Para as capturas de tela neste tutorial, eu implantei aplicativos de Web do serviço de aplicativo do Azure.) Para verificar se a API da web está funcionando, navegue até `http://hostname/api/test/`, onde *hostname* é o domínio onde você implantou o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3364e-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="3364e-140">Você deve ver o texto de resposta &quot;obter: mensagem de teste&quot;.</span><span class="sxs-lookup"><span data-stu-id="3364e-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="3364e-141">Criar o projeto de WebClient</span><span class="sxs-lookup"><span data-stu-id="3364e-141">Create the WebClient Project</span></span>

<span data-ttu-id="3364e-142">Crie outro projeto de aplicativo Web ASP.NET e selecione o **MVC** modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="3364e-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="3364e-143">Opcionalmente, selecione **alterar autenticação** > **sem autenticação**.</span><span class="sxs-lookup"><span data-stu-id="3364e-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="3364e-144">Autenticação não é necessário para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3364e-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="3364e-145">No Gerenciador de soluções, abra o arquivo Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="3364e-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="3364e-146">Substitua o código nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="3364e-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="3364e-147">Para o *serviceUrl* variável, use o URI do aplicativo de serviço Web.</span><span class="sxs-lookup"><span data-stu-id="3364e-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="3364e-148">Agora, execute o aplicativo de WebClient localmente ou publicá-lo em outro site.</span><span class="sxs-lookup"><span data-stu-id="3364e-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="3364e-149">Clicar no botão "Experimente" envia uma solicitação AJAX para o aplicativo de serviço Web, usando o método HTTP listado na caixa suspensa (GET, POST ou PUT).</span><span class="sxs-lookup"><span data-stu-id="3364e-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="3364e-150">Isso nos permite examinar as solicitações entre origens diferentes.</span><span class="sxs-lookup"><span data-stu-id="3364e-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="3364e-151">Agora, o aplicativo de serviço Web não dá suporte a CORS, portanto, se você clicar no botão, você receberá um erro.</span><span class="sxs-lookup"><span data-stu-id="3364e-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="3364e-152">Se você observar o tráfego HTTP em uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), você verá que o navegador envie a solicitação GET e a solicitação for bem-sucedida, mas a chamada do AJAX retorna um erro.</span><span class="sxs-lookup"><span data-stu-id="3364e-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="3364e-153">É importante entender a política de mesma origem não impede que o navegador do *enviando* a solicitação.</span><span class="sxs-lookup"><span data-stu-id="3364e-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="3364e-154">Em vez disso, ele impede que o aplicativo vendo os *resposta*.</span><span class="sxs-lookup"><span data-stu-id="3364e-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="3364e-155">Habilitar o CORS</span><span class="sxs-lookup"><span data-stu-id="3364e-155">Enable CORS</span></span>

<span data-ttu-id="3364e-156">Agora vamos habilitar CORS no aplicativo do serviço Web.</span><span class="sxs-lookup"><span data-stu-id="3364e-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="3364e-157">Primeiro, adicione o pacote NuGet de CORS.</span><span class="sxs-lookup"><span data-stu-id="3364e-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="3364e-158">No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="3364e-158">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3364e-159">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="3364e-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="3364e-160">Este comando instala o pacote mais recente e atualiza todas as dependências, incluindo as bibliotecas de API da Web principal.</span><span class="sxs-lookup"><span data-stu-id="3364e-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="3364e-161">Usuário-sinalizador de versão como destino uma versão específica.</span><span class="sxs-lookup"><span data-stu-id="3364e-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="3364e-162">O pacote CORS requer Web API 2.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="3364e-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="3364e-163">Abra o arquivo de aplicativo\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="3364e-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="3364e-164">Adicione o seguinte código para o **Webapiconfig** método.</span><span class="sxs-lookup"><span data-stu-id="3364e-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="3364e-165">Em seguida, adicione a **[EnableCors]** atributo para o `TestController` classe:</span><span class="sxs-lookup"><span data-stu-id="3364e-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="3364e-166">Para o *origens* parâmetro, use o URI de onde você implantou o aplicativo de WebClient.</span><span class="sxs-lookup"><span data-stu-id="3364e-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="3364e-167">Isso permite que solicitações entre origens do WebClient, enquanto ainda desautorizando todas as outras solicitações entre domínios.</span><span class="sxs-lookup"><span data-stu-id="3364e-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="3364e-168">Posteriormente, descreverei os parâmetros para **[EnableCors]** em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="3364e-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="3364e-169">Não inclua uma barra invertida no final de *origens* URL.</span><span class="sxs-lookup"><span data-stu-id="3364e-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="3364e-170">Reimplante o aplicativo de serviço Web atualizado.</span><span class="sxs-lookup"><span data-stu-id="3364e-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="3364e-171">Você não precisa atualizar o WebClient.</span><span class="sxs-lookup"><span data-stu-id="3364e-171">You don't need to update WebClient.</span></span> <span data-ttu-id="3364e-172">Agora a solicitação AJAX do WebClient deve ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="3364e-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="3364e-173">Os métodos GET, PUT e POST todos são permitidos.</span><span class="sxs-lookup"><span data-stu-id="3364e-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="3364e-174">Como funciona o CORS</span><span class="sxs-lookup"><span data-stu-id="3364e-174">How CORS Works</span></span>

<span data-ttu-id="3364e-175">Esta seção descreve o que acontece em uma solicitação CORS no nível das mensagens HTTP.</span><span class="sxs-lookup"><span data-stu-id="3364e-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="3364e-176">É importante entender como funciona o CORS, para que você possa configurar o **[EnableCors]** atributo corretamente e solucionar problemas se as coisas não funcionam conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="3364e-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="3364e-177">A especificação CORS apresenta vários novos cabeçalhos HTTP que permitem que solicitações entre origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="3364e-178">Se um navegador oferece suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens; Você não precisa fazer nada especial em seu código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3364e-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="3364e-179">Aqui está um exemplo de uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="3364e-180">O cabeçalho de "Origem" fornece o domínio do site que está fazendo a solicitação.</span><span class="sxs-lookup"><span data-stu-id="3364e-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="3364e-181">Se o servidor permite que a solicitação, ele define o cabeçalho Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="3364e-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="3364e-182">O valor desse cabeçalho corresponde o cabeçalho de origem, ou é o valor de curinga "\*", o que significa que qualquer origem é permitida.</span><span class="sxs-lookup"><span data-stu-id="3364e-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="3364e-183">Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falha.</span><span class="sxs-lookup"><span data-stu-id="3364e-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="3364e-184">Especificamente, o navegador não permite a solicitação.</span><span class="sxs-lookup"><span data-stu-id="3364e-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="3364e-185">Mesmo se o servidor retorna uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="3364e-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="3364e-186">**Solicitações de simulação**</span><span class="sxs-lookup"><span data-stu-id="3364e-186">**Preflight Requests**</span></span>

<span data-ttu-id="3364e-187">Para algumas solicitações CORS, o navegador envia uma solicitação adicional, chamada de uma "solicitação de simulação," antes de enviar a solicitação real para o recurso.</span><span class="sxs-lookup"><span data-stu-id="3364e-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="3364e-188">O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="3364e-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="3364e-189">O método de solicitação é GET, HEAD ou POST, *e*</span><span class="sxs-lookup"><span data-stu-id="3364e-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="3364e-190">O aplicativo não definir quaisquer cabeçalhos de solicitação que não seja Accept, Accept-Language, Content-Language, Content-Type ou última--ID do evento, *e*</span><span class="sxs-lookup"><span data-stu-id="3364e-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="3364e-191">O cabeçalho Content-Type (se definido) é um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="3364e-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="3364e-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="3364e-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="3364e-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="3364e-193">multipart/form-data</span></span>
    - <span data-ttu-id="3364e-194">texto/simples</span><span class="sxs-lookup"><span data-stu-id="3364e-194">text/plain</span></span>

<span data-ttu-id="3364e-195">A regra sobre cabeçalhos de solicitação se aplica aos cabeçalhos que o aplicativo define chamando **setRequestHeader** sobre o **XMLHttpRequest** objeto.</span><span class="sxs-lookup"><span data-stu-id="3364e-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="3364e-196">(A especificação CORS chama esses cabeçalhos de solicitação"autor".) A regra não se aplica aos cabeçalhos de *navegador* pode definir, como o agente do usuário, o Host ou o comprimento do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="3364e-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="3364e-197">Aqui está um exemplo de uma solicitação de simulação:</span><span class="sxs-lookup"><span data-stu-id="3364e-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="3364e-198">A solicitação de simulação usa o método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="3364e-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="3364e-199">Ele inclui dois cabeçalhos especiais:</span><span class="sxs-lookup"><span data-stu-id="3364e-199">It includes two special headers:</span></span>

- <span data-ttu-id="3364e-200">Access-Control-Request-Method: O método HTTP que será usado para a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="3364e-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="3364e-201">Access-Control-Request-Headers: Uma lista de cabeçalhos de solicitação que o *aplicativo* configurado na solicitação real.</span><span class="sxs-lookup"><span data-stu-id="3364e-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="3364e-202">(Novamente, isso não inclui cabeçalhos que define o navegador.)</span><span class="sxs-lookup"><span data-stu-id="3364e-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="3364e-203">Aqui está um exemplo de resposta, supondo que o servidor permite que a solicitação:</span><span class="sxs-lookup"><span data-stu-id="3364e-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="3364e-204">A resposta inclui um cabeçalho Access-Control-Allow-Methods que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-Allow-Headers, que lista os cabeçalhos permitidos.</span><span class="sxs-lookup"><span data-stu-id="3364e-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="3364e-205">Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real, conforme descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3364e-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="3364e-206">Regras de escopo para [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="3364e-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="3364e-207">Você pode habilitar o CORS por ação, por controlador ou globalmente para todos os controladores de API da Web em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3364e-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="3364e-208">**Por ação**</span><span class="sxs-lookup"><span data-stu-id="3364e-208">**Per Action**</span></span>

<span data-ttu-id="3364e-209">Para habilitar o CORS para uma única ação, defina as **[EnableCors]** atributo no método de ação.</span><span class="sxs-lookup"><span data-stu-id="3364e-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="3364e-210">O exemplo a seguir habilita o CORS para o `GetItem` somente no método.</span><span class="sxs-lookup"><span data-stu-id="3364e-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="3364e-211">**Por controlador**</span><span class="sxs-lookup"><span data-stu-id="3364e-211">**Per Controller**</span></span>

<span data-ttu-id="3364e-212">Se você definir **[EnableCors]** na classe do controlador, ele se aplica a todas as ações no controlador.</span><span class="sxs-lookup"><span data-stu-id="3364e-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="3364e-213">Para desabilitar CORS para uma ação, adicione a **[DisableCors]** atributo à ação.</span><span class="sxs-lookup"><span data-stu-id="3364e-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="3364e-214">O exemplo a seguir habilita o CORS para cada método, exceto `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="3364e-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="3364e-215">**Globalmente**</span><span class="sxs-lookup"><span data-stu-id="3364e-215">**Globally**</span></span>

<span data-ttu-id="3364e-216">Para habilitar o CORS para todos os controladores de API da Web em seu aplicativo, passe uma **EnableCorsAttribute** da instância para o **EnableCors** método:</span><span class="sxs-lookup"><span data-stu-id="3364e-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="3364e-217">Se você definir o atributo em mais de um escopo, a ordem de precedência é:</span><span class="sxs-lookup"><span data-stu-id="3364e-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="3364e-218">Ação</span><span class="sxs-lookup"><span data-stu-id="3364e-218">Action</span></span>
2. <span data-ttu-id="3364e-219">Controlador</span><span class="sxs-lookup"><span data-stu-id="3364e-219">Controller</span></span>
3. <span data-ttu-id="3364e-220">Global</span><span class="sxs-lookup"><span data-stu-id="3364e-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="3364e-221">Defina as origens permitidas</span><span class="sxs-lookup"><span data-stu-id="3364e-221">Set the Allowed Origins</span></span>

<span data-ttu-id="3364e-222">O *origens* parâmetro do **[EnableCors]** atributo especifica quais origens têm permissão para acessar o recurso.</span><span class="sxs-lookup"><span data-stu-id="3364e-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="3364e-223">O valor é uma lista separada por vírgulas das origens permitidas.</span><span class="sxs-lookup"><span data-stu-id="3364e-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="3364e-224">Você também pode usar o valor de curinga "\*" para permitir solicitações de qualquer origem.</span><span class="sxs-lookup"><span data-stu-id="3364e-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="3364e-225">Considere cuidadosamente antes de permitir que solicitações de qualquer origem.</span><span class="sxs-lookup"><span data-stu-id="3364e-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="3364e-226">Isso significa que, literalmente, qualquer site possa fazer chamadas AJAX para sua API da web.</span><span class="sxs-lookup"><span data-stu-id="3364e-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="3364e-227">Defina os métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="3364e-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="3364e-228">O *métodos* parâmetro do **[EnableCors]** atributo especifica quais métodos HTTP têm permissão para acessar o recurso.</span><span class="sxs-lookup"><span data-stu-id="3364e-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="3364e-229">Para permitir que todos os métodos, use o valor de curinga "\*".</span><span class="sxs-lookup"><span data-stu-id="3364e-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="3364e-230">O exemplo a seguir permite que somente as solicitações GET e POST.</span><span class="sxs-lookup"><span data-stu-id="3364e-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="3364e-231">Definir os cabeçalhos de solicitação permitido</span><span class="sxs-lookup"><span data-stu-id="3364e-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="3364e-232">Anteriormente, descrevi como uma solicitação de simulação pode incluir um cabeçalho Access-Control-Request-Headers, listando os cabeçalhos HTTP definidos pelo aplicativo (os chamados "author cabeçalhos de solicitação").</span><span class="sxs-lookup"><span data-stu-id="3364e-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="3364e-233">O *cabeçalhos* parâmetro do **[EnableCors]** atributo especifica quais cabeçalhos de solicitação do autor são permitidos.</span><span class="sxs-lookup"><span data-stu-id="3364e-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="3364e-234">Para permitir que todos os cabeçalhos, defina *cabeçalhos* para "\*".</span><span class="sxs-lookup"><span data-stu-id="3364e-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="3364e-235">Para cabeçalhos específicos de lista de permissões, defina *cabeçalhos* a uma lista separada por vírgula de cabeçalhos permitidos:</span><span class="sxs-lookup"><span data-stu-id="3364e-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="3364e-236">No entanto, os navegadores não são totalmente consistentes em como eles definir Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="3364e-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="3364e-237">Por exemplo, o Chrome atualmente inclui "origem"; enquanto o FireFox não inclui cabeçalhos padrão, como "Aceitar", mesmo quando o aplicativo define-los no script.</span><span class="sxs-lookup"><span data-stu-id="3364e-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="3364e-238">Se você definir *cabeçalhos* para qualquer coisa diferente de "\*", você deve incluir pelo menos "accept", "content-type" e "origem", além de quaisquer cabeçalhos personalizados que você deseja dar suporte.</span><span class="sxs-lookup"><span data-stu-id="3364e-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="3364e-239">Definir os cabeçalhos de resposta permitidos</span><span class="sxs-lookup"><span data-stu-id="3364e-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="3364e-240">Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3364e-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="3364e-241">Os cabeçalhos de resposta que estão disponíveis por padrão são:</span><span class="sxs-lookup"><span data-stu-id="3364e-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="3364e-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="3364e-242">Cache-Control</span></span>
- <span data-ttu-id="3364e-243">Idioma do conteúdo</span><span class="sxs-lookup"><span data-stu-id="3364e-243">Content-Language</span></span>
- <span data-ttu-id="3364e-244">Tipo de conteúdo</span><span class="sxs-lookup"><span data-stu-id="3364e-244">Content-Type</span></span>
- <span data-ttu-id="3364e-245">Expira</span><span class="sxs-lookup"><span data-stu-id="3364e-245">Expires</span></span>
- <span data-ttu-id="3364e-246">Última modificação</span><span class="sxs-lookup"><span data-stu-id="3364e-246">Last-Modified</span></span>
- <span data-ttu-id="3364e-247">Pragma</span><span class="sxs-lookup"><span data-stu-id="3364e-247">Pragma</span></span>

<span data-ttu-id="3364e-248">A especificação CORS chama esses [cabeçalhos de resposta simples](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="3364e-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="3364e-249">Para disponibilizar outros cabeçalhos para o aplicativo, defina as *exposedHeaders* parâmetro do **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="3364e-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="3364e-250">No exemplo a seguir, o controlador `Get` método define um cabeçalho personalizado chamado 'X-Custom-Header'.</span><span class="sxs-lookup"><span data-stu-id="3364e-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="3364e-251">Por padrão, o navegador não irá expor esse cabeçalho em uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="3364e-252">Para disponibilizar o cabeçalho, incluir 'X-Custom-Header' em *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="3364e-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="3364e-253">Passagem de credenciais nas solicitações entre origens</span><span class="sxs-lookup"><span data-stu-id="3364e-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="3364e-254">Credenciais exigem tratamento especial em uma solicitação CORS.</span><span class="sxs-lookup"><span data-stu-id="3364e-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="3364e-255">Por padrão, o navegador não envia todas as credenciais com uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="3364e-256">As credenciais incluem cookies, bem como os esquemas de autenticação HTTP.</span><span class="sxs-lookup"><span data-stu-id="3364e-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="3364e-257">Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir **XMLHttpRequest.withCredentials** como true.</span><span class="sxs-lookup"><span data-stu-id="3364e-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="3364e-258">Usando o **XMLHttpRequest** diretamente:</span><span class="sxs-lookup"><span data-stu-id="3364e-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="3364e-259">No jQuery:</span><span class="sxs-lookup"><span data-stu-id="3364e-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="3364e-260">Além disso, o servidor deve permitir que as credenciais.</span><span class="sxs-lookup"><span data-stu-id="3364e-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="3364e-261">Para permitir credenciais entre origens na API da Web, defina as **SupportsCredentials** propriedade como true na **[EnableCors]** atributo:</span><span class="sxs-lookup"><span data-stu-id="3364e-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="3364e-262">Se essa propriedade for true, a resposta HTTP incluirá um cabeçalho Access-Control-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="3364e-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="3364e-263">Esse cabeçalho informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="3364e-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="3364e-264">Se o navegador envia as credenciais, mas a resposta não inclui um cabeçalho Access-Control-Allow-Credentials válido, o navegador não exporá a resposta para o aplicativo e a solicitação AJAX falhar.</span><span class="sxs-lookup"><span data-stu-id="3364e-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="3364e-265">Tenha muito cuidado sobre a configuração **SupportsCredentials** como true, porque significa que um site em outro domínio pode enviar credenciais do usuário conectado à sua API da Web em nome do usuário, sem que o usuário estar ciente.</span><span class="sxs-lookup"><span data-stu-id="3364e-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="3364e-266">A especificação CORS também declara que a configuração *origens* à &quot; \* &quot; não é válido se **SupportsCredentials** é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="3364e-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="3364e-267">Provedores de política CORS personalizado</span><span class="sxs-lookup"><span data-stu-id="3364e-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="3364e-268">O **[EnableCors]** atributo implementa o **ICorsPolicyProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="3364e-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="3364e-269">Você pode fornecer sua própria implementação, criando uma classe que deriva de **atributo** e implementa **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="3364e-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="3364e-270">Agora você pode aplicar o atributo de qualquer lugar que você colocaria **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="3364e-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="3364e-271">Por exemplo, um provedor de política CORS personalizado pode ler as configurações de um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="3364e-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="3364e-272">Como uma alternativa ao uso de atributos, você pode registrar um **ICorsPolicyProviderFactory** objeto cria **ICorsPolicyProvider** objetos.</span><span class="sxs-lookup"><span data-stu-id="3364e-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="3364e-273">Para definir a **ICorsPolicyProviderFactory**, chame o **SetCorsPolicyProviderFactory** método de extensão na inicialização, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3364e-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="3364e-274">Suporte ao navegador</span><span class="sxs-lookup"><span data-stu-id="3364e-274">Browser Support</span></span>

<span data-ttu-id="3364e-275">O pacote de CORS da API Web é uma tecnologia do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="3364e-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="3364e-276">O navegador do usuário também precisa oferecer suporte a CORS.</span><span class="sxs-lookup"><span data-stu-id="3364e-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="3364e-277">Felizmente, as versões atuais de todos os principais navegadores incluem [suporte para CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="3364e-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="3364e-278">Internet Explorer 8 e Internet Explorer 9 têm suporte parcial para CORS, usando o objeto XDomainRequest herdado em vez de XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="3364e-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="3364e-279">Para obter mais informações, consulte [XDomainRequest - restrições, limitações e soluções alternativas](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="3364e-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
