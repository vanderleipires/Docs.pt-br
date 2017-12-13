---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: "Guia de solução de problemas (Razor) do ASP.NET Web Pages | Microsoft Docs"
author: tfitzmac
description: "Este artigo descreve problemas que você possa ter ao trabalhar com páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas. Versões de software da página de Web do ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: e507d97903583c7233456e9139e1a869f8bf75bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="5040e-104">Guia de solução de problemas (Razor) de páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5040e-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="5040e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5040e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5040e-106">Este artigo descreve problemas que você possa ter ao trabalhar com páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas.</span><span class="sxs-lookup"><span data-stu-id="5040e-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="5040e-107">Versões de software</span><span class="sxs-lookup"><span data-stu-id="5040e-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="5040e-108">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5040e-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5040e-109">Este tutorial também funciona com 2 de páginas da Web do ASP.NET e páginas da Web de ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="5040e-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="5040e-110">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="5040e-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5040e-111">Problemas com a execução de páginas</span><span class="sxs-lookup"><span data-stu-id="5040e-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="5040e-112">Problemas com o código do Razor</span><span class="sxs-lookup"><span data-stu-id="5040e-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="5040e-113">Problemas de segurança e associação</span><span class="sxs-lookup"><span data-stu-id="5040e-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="5040e-114">Problemas com o envio de Email</span><span class="sxs-lookup"><span data-stu-id="5040e-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="5040e-115">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5040e-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="5040e-116">Para perguntas gerais, consulte [páginas da Web do ASP.NET (Razor) perguntas frequentes sobre](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="5040e-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="5040e-117">Problemas com a execução de páginas</span><span class="sxs-lookup"><span data-stu-id="5040e-117">Issues with Running Pages</span></span>

<span data-ttu-id="5040e-118">Uma variedade de problemas que pode impedir que *. cshtml* e *. vbhtml* páginas sejam executados corretamente.</span><span class="sxs-lookup"><span data-stu-id="5040e-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="5040e-119">Esta seção lista mensagens de erro comuns e causas prováveis.</span><span class="sxs-lookup"><span data-stu-id="5040e-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="5040e-120">Erro de HTTP 403 - Proibido: Acesso negado</span><span class="sxs-lookup"><span data-stu-id="5040e-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="5040e-121">*Você não tem permissão para exibir este diretório ou página usando as credenciais que você forneceu.*</span><span class="sxs-lookup"><span data-stu-id="5040e-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="5040e-122">Esse erro pode ocorrer se o servidor não está executando a versão correta do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5040e-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="5040e-123">Certifique-se de que o computador que está executando o servidor (local ou remotamente) tem pelo menos o .NET Framework 4 instalado.</span><span class="sxs-lookup"><span data-stu-id="5040e-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="5040e-124">Além disso, certifique-se de que o aplicativo em si é configurado para executar a versão correta.</span><span class="sxs-lookup"><span data-stu-id="5040e-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="5040e-125">Se você encontrar esse problema localmente enquanto estiver trabalhando no WebMatrix, clique o **Site** espaço de trabalho e, em que o modo de exibição de árvore, clique em **configurações**.</span><span class="sxs-lookup"><span data-stu-id="5040e-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="5040e-126">No **selecione a versão do .NET Framework** , selecione **(integrado) do .NET 4**.</span><span class="sxs-lookup"><span data-stu-id="5040e-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="5040e-127">Se esta versão já estiver definida, tente executar o WebMatrix como um administrador.</span><span class="sxs-lookup"><span data-stu-id="5040e-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="5040e-128">Certifique-se de que a raiz do seu site possui pelo menos um *. cshtml* arquivo nele.</span><span class="sxs-lookup"><span data-stu-id="5040e-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="5040e-129">Se você vir esse erro quando o servidor web está em um servidor remoto, contate o administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="5040e-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="5040e-130">Verifique se o servidor tem o .NET Framework 4 ou posterior instalado.</span><span class="sxs-lookup"><span data-stu-id="5040e-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="5040e-131">Além disso, certifique-se de que o aplicativo é executado em um pool de aplicativos está configurado para usar essa versão do.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5040e-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="5040e-132">Se você tem controle sobre o servidor, verifique se que ele está executando a versão correta do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5040e-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="5040e-133">Você também pode tentar reparar a instalação executando o `aspnet_regiis -iru` comando.</span><span class="sxs-lookup"><span data-stu-id="5040e-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="5040e-134">(Por exemplo, se você instalar o IIS depois de instalar o .NET Framework, o IIS não será ser corretamente configurado para executar as páginas ASP.NET.) Para obter mais informações, consulte [ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="5040e-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="5040e-135">Erro de HTTP 403.14 - proibido</span><span class="sxs-lookup"><span data-stu-id="5040e-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="5040e-136">*O servidor Web está configurado para não listar o conteúdo desse diretório.*</span><span class="sxs-lookup"><span data-stu-id="5040e-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="5040e-137">Esse erro pode ocorrer se você solicitar um recurso protegido (como o *Web. config* arquivo) ou em uma pasta que está protegida (como *aplicativo\_dados* ou *aplicativo\_Código*).</span><span class="sxs-lookup"><span data-stu-id="5040e-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="5040e-138">Erro de HTTP 404.17 - não encontrado</span><span class="sxs-lookup"><span data-stu-id="5040e-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="5040e-139">*O conteúdo solicitado parece ser script e não será servido pelo manipulador de arquivo estático.*</span><span class="sxs-lookup"><span data-stu-id="5040e-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="5040e-140">Esse erro pode ocorrer se o servidor não está configurado corretamente para usar o .NET Framework 4 ou posterior e, portanto, não reconhece o código em `@{ }` blocos.</span><span class="sxs-lookup"><span data-stu-id="5040e-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="5040e-141">Consulte a descrição anteriormente *HTTP Erro 403 - Proibido: acesso negado*.</span><span class="sxs-lookup"><span data-stu-id="5040e-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="5040e-142">Erro de HTTP 404.7 - não encontrado</span><span class="sxs-lookup"><span data-stu-id="5040e-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="5040e-143">*A módulo de filtragem de solicitação está configurado para negar a extensão de arquivo*</span><span class="sxs-lookup"><span data-stu-id="5040e-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="5040e-144">Esse erro pode ocorrer se *. cshtml* ou *. vbhtml* extensões foram bloqueadas explicitamente no servidor.</span><span class="sxs-lookup"><span data-stu-id="5040e-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="5040e-145">Um sintoma desse problema é que o trabalho URLs quando eles não têm a extensão, mas as URLs que incluem *. cshtml* ou *. vbhtml* não funcionam.</span><span class="sxs-lookup"><span data-stu-id="5040e-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="5040e-146">Uma solução possível é habilitar novamente as extensões do site *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5040e-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="5040e-147">O exemplo a seguir mostra como habilitar o *. cshtml* extensão.</span><span class="sxs-lookup"><span data-stu-id="5040e-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="5040e-148">Erro de HTTP 404.8 - não encontrado</span><span class="sxs-lookup"><span data-stu-id="5040e-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="5040e-149">*O módulo de filtragem de solicitação está configurado para negar um caminho na URL que contém uma seção hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="5040e-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="5040e-150">Esse erro pode ocorrer se você solicitar um recurso protegido (como o *Web. config* arquivo) ou em uma pasta que está protegida (como *aplicativo\_dados* ou *aplicativo\_Código*).</span><span class="sxs-lookup"><span data-stu-id="5040e-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="5040e-151">Esse tipo de página não é atendido (erro de servidor no aplicativo '/')</span><span class="sxs-lookup"><span data-stu-id="5040e-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="5040e-152">Consulte a descrição anteriormente 404.17 de erro de HTTP.</span><span class="sxs-lookup"><span data-stu-id="5040e-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="5040e-153">Problemas com o código do Razor</span><span class="sxs-lookup"><span data-stu-id="5040e-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="5040e-154">O nome '*classe*' não existe no contexto atual</span><span class="sxs-lookup"><span data-stu-id="5040e-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="5040e-155">Geralmente, um motivo que você vir esse erro é que `class` referências um auxiliar, mas o auxiliar não está instalado.</span><span class="sxs-lookup"><span data-stu-id="5040e-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="5040e-156">Por exemplo, se você tentar usar um auxiliar, mas se você ainda não instalou o pacote do NuGet, você verá esse erro.</span><span class="sxs-lookup"><span data-stu-id="5040e-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="5040e-157">Use a galeria no WebMatrix para localizar e instalar o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="5040e-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="5040e-158">Se o auxiliar estiver instalado, mas a página ainda não o reconhecerá, tente adicionar adicionar um `using` instrução no código.</span><span class="sxs-lookup"><span data-stu-id="5040e-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="5040e-159">No `using` instrução, o namespace que inclui o auxiliar de referência.</span><span class="sxs-lookup"><span data-stu-id="5040e-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="5040e-160">Por exemplo, os auxiliares básico que estão no pacote auxiliares de Web do ASP.NET estão no `System.Web.Helpers` namespace.</span><span class="sxs-lookup"><span data-stu-id="5040e-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="5040e-161">Na parte superior da página onde você deseja usar o auxiliar, adicione esta linha:</span><span class="sxs-lookup"><span data-stu-id="5040e-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="5040e-162">Problemas de segurança e associação</span><span class="sxs-lookup"><span data-stu-id="5040e-162">Issues with Security and Membership</span></span>

<span data-ttu-id="5040e-163">Se você estiver usando o sistema de segurança internas (associação) em páginas da Web do ASP.NET (Razor), você pode encontrar os problemas a seguir.</span><span class="sxs-lookup"><span data-stu-id="5040e-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="5040e-164">Para chamar esse método, a propriedade "Membership.Provider" deve ser uma instância de "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="5040e-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="5040e-165">Esse erro pode indicar que nenhuma `AspNetSqlMembershipProvider` classe está configurada.</span><span class="sxs-lookup"><span data-stu-id="5040e-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="5040e-166">(Um sintoma é que o site funciona bem localmente, mas gera este erro quando você publicá-lo no servidor de um provedor de hospedagem). Uma correção para esse problema é habilitar associação simples explicitamente adicionando o seguinte para o site *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5040e-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="5040e-167">Problemas com o envio de Email</span><span class="sxs-lookup"><span data-stu-id="5040e-167">Issues with Sending Email</span></span>

<span data-ttu-id="5040e-168">Problemas com o envio de email podem ser um desafio para depurar.</span><span class="sxs-lookup"><span data-stu-id="5040e-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="5040e-169">Um problema inicial pode ser que você não pode se conectar ao servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="5040e-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="5040e-170">Se a conexão for bem-sucedida, ASP.NET transmite a mensagem para o servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="5040e-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="5040e-171">No entanto, pode haver problemas com a mensagem que impede que o servidor SMTP enviá-la.</span><span class="sxs-lookup"><span data-stu-id="5040e-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="5040e-172">Se seu aplicativo não enviar email com êxito, tente o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5040e-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="5040e-173">O nome do servidor SMTP costuma ser algo como `smtp.provider.com` ou `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="5040e-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="5040e-174">No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse momento pode ser `localhost`.</span><span class="sxs-lookup"><span data-stu-id="5040e-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="5040e-175">Isso acontece porque, após ter publicado seu site estiver sendo executado no servidor do provedor, o servidor SMTP pode ser local da perspectiva do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5040e-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="5040e-176">Essa alteração nos nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte do processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="5040e-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="5040e-177">O número da porta normalmente é 25.</span><span class="sxs-lookup"><span data-stu-id="5040e-177">The port number is usually 25.</span></span> <span data-ttu-id="5040e-178">No entanto, alguns provedores exigem que você usar a porta 587 ou alguma outra porta.</span><span class="sxs-lookup"><span data-stu-id="5040e-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="5040e-179">Verifique com o proprietário do servidor SMTP que número de porta esperam que você use.</span><span class="sxs-lookup"><span data-stu-id="5040e-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="5040e-180">Certifique-se de que você use as credenciais corretas.</span><span class="sxs-lookup"><span data-stu-id="5040e-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="5040e-181">Se você tiver publicado seu site para um provedor de hospedagem, use as credenciais que o provedor indicou especificamente são para email.</span><span class="sxs-lookup"><span data-stu-id="5040e-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="5040e-182">Essas credenciais podem ser diferentes das credenciais que você usa para publicar.</span><span class="sxs-lookup"><span data-stu-id="5040e-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="5040e-183">Às vezes, você não precisará de credenciais em todos os.</span><span class="sxs-lookup"><span data-stu-id="5040e-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="5040e-184">Se você estiver enviando email por meio de seu ISP pessoal, seu provedor de email já saberá suas credenciais.</span><span class="sxs-lookup"><span data-stu-id="5040e-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="5040e-185">Depois de publicar, você precisará usar credenciais diferentes quando você testar no computador local.</span><span class="sxs-lookup"><span data-stu-id="5040e-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="5040e-186">Se seu provedor de email usa criptografia, defina `WebMail.EnableSsl` para `true`.</span><span class="sxs-lookup"><span data-stu-id="5040e-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="5040e-187">Se houver um erro ao enviar o email, você verá uma mensagem de erro padrão do ASP.NET, que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="5040e-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Mensagem de erro do ASP.NET quando há um problema com email](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="5040e-189">Você também pode depurar problemas com o envio de email usando um `try-catch` bloco, como no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="5040e-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="5040e-190">Quando você usa um `try-catch` bloco, o ASP.NET não exibe suas mensagens de erro padrão.</span><span class="sxs-lookup"><span data-stu-id="5040e-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="5040e-191">Em vez disso, você pode capturar o erro no `catch` parte do bloco.</span><span class="sxs-lookup"><span data-stu-id="5040e-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="5040e-192">Substitua os valores apropriados para `your-SMTP-server-name`, e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="5040e-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="5040e-193">Algumas das mensagens de erro, que talvez você veja dessa maneira incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5040e-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="5040e-194">*Falha ao enviar email.*</span><span class="sxs-lookup"><span data-stu-id="5040e-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="5040e-195">-ou-</span><span class="sxs-lookup"><span data-stu-id="5040e-195">-or-</span></span>

    <span data-ttu-id="5040e-196">*Uma tentativa de conexão falhou porque a parte conectada não respondeu corretamente após um período de tempo ou a conexão estabelecida falhou porque o host conectado não respondeu*</span><span class="sxs-lookup"><span data-stu-id="5040e-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="5040e-197">Esse erro geralmente significa que o aplicativo não pôde se conectar ao servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="5040e-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="5040e-198">Verifique o nome do servidor e o número da porta.</span><span class="sxs-lookup"><span data-stu-id="5040e-198">Check the server name and port number.</span></span>
- <span data-ttu-id="5040e-199">*Caixa de correio indisponível. A resposta do servidor foi: 5.1.0 &lt; someuser@invaliddomain &gt; remetente rejeitada: domínio remetente inválido*</span><span class="sxs-lookup"><span data-stu-id="5040e-199">*Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain*</span></span>

    <span data-ttu-id="5040e-200">Essa mensagem pode indicar que o `From` endereço não está correto ou está ausente.</span><span class="sxs-lookup"><span data-stu-id="5040e-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="5040e-201">*A cadeia de caracteres especificada não está no formato necessário para um endereço de email.*</span><span class="sxs-lookup"><span data-stu-id="5040e-201">*The specified string is not in the form required for an e-mail address.*</span></span>

    <span data-ttu-id="5040e-202">Esse erro pode indicar que o valor de `To` ou `From` propriedades não são reconhecidas como endereços de email.</span><span class="sxs-lookup"><span data-stu-id="5040e-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="5040e-203">(ASP.NET não é possível verificar se o endereço de email é válido, só que ele 's no formato correto, como  *name@domain.com* .)</span><span class="sxs-lookup"><span data-stu-id="5040e-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="5040e-204">Remover a marcação que exibe o erro (`@errorMessage`) antes de publicar a página em um site ativo.</span><span class="sxs-lookup"><span data-stu-id="5040e-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="5040e-205">Não é uma boa ideia para permitir aos usuários ver mensagens de erro que você pode obter de um servidor.</span><span class="sxs-lookup"><span data-stu-id="5040e-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5040e-206">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5040e-206">Additional Resources</span></span>

[<span data-ttu-id="5040e-207">ASP.NET Web Pages FAQ (Razor)</span><span class="sxs-lookup"><span data-stu-id="5040e-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="5040e-208">[O WebMatrix e páginas da Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) fórum no site do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5040e-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
