---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guia de solução de problemas (Razor) de páginas da Web ASP.NET | Microsoft Docs
author: tfitzmac
description: Este artigo descreve problemas que você possa ter ao trabalhar com páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas. Versões de software Pag de Web do ASP.NET...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: c27139a720decd34a4ab89e6f93e71c97d123b45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834877"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="37b3d-104">Guia de solução de problemas (Razor) de páginas da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="37b3d-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="37b3d-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="37b3d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="37b3d-106">Este artigo descreve problemas que você possa ter ao trabalhar com páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas.</span><span class="sxs-lookup"><span data-stu-id="37b3d-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="37b3d-107">Versões de software</span><span class="sxs-lookup"><span data-stu-id="37b3d-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="37b3d-108">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="37b3d-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="37b3d-109">Este tutorial também funciona com ASP.NET Web Pages 2 e páginas da Web de ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="37b3d-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="37b3d-110">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="37b3d-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="37b3d-111">Problemas com a execução de páginas</span><span class="sxs-lookup"><span data-stu-id="37b3d-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="37b3d-112">Problemas com o código do Razor</span><span class="sxs-lookup"><span data-stu-id="37b3d-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="37b3d-113">Problemas com a segurança e associação</span><span class="sxs-lookup"><span data-stu-id="37b3d-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="37b3d-114">Problemas com o envio de Email</span><span class="sxs-lookup"><span data-stu-id="37b3d-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="37b3d-115">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="37b3d-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="37b3d-116">Para perguntas gerais, consulte [páginas da Web do ASP.NET (Razor) perguntas frequentes sobre](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="37b3d-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="37b3d-117">Problemas com a execução de páginas</span><span class="sxs-lookup"><span data-stu-id="37b3d-117">Issues with Running Pages</span></span>

<span data-ttu-id="37b3d-118">Uma variedade de problemas pode impedir *. cshtml* e *. vbhtml* páginas sejam executados corretamente.</span><span class="sxs-lookup"><span data-stu-id="37b3d-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="37b3d-119">Esta seção lista mensagens de erro comuns e causas mais prováveis.</span><span class="sxs-lookup"><span data-stu-id="37b3d-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="37b3d-120">Erro HTTP 403 - Proibido: Acesso negado</span><span class="sxs-lookup"><span data-stu-id="37b3d-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="37b3d-121">*Você não tem permissão para exibir este diretório ou página usando as credenciais que você forneceu.*</span><span class="sxs-lookup"><span data-stu-id="37b3d-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="37b3d-122">Esse erro pode ocorrer se o servidor não está executando a versão correta do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="37b3d-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="37b3d-123">Certifique-se de que o computador que está executando o servidor (local ou remotamente) tenha pelo menos o .NET Framework 4 instalado.</span><span class="sxs-lookup"><span data-stu-id="37b3d-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="37b3d-124">Além disso, certifique-se de que o aplicativo em si é configurado para executar a versão correta.</span><span class="sxs-lookup"><span data-stu-id="37b3d-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="37b3d-125">Se você vir esse problema localmente enquanto estiver trabalhando no WebMatrix, clique o **Site** espaço de trabalho e, em que o modo de exibição de árvore, clique em **configurações**.</span><span class="sxs-lookup"><span data-stu-id="37b3d-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="37b3d-126">No **selecione a versão do .NET Framework** , selecione **(integrado) do .NET 4**.</span><span class="sxs-lookup"><span data-stu-id="37b3d-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="37b3d-127">Se essa versão já está definido, tente executar o WebMatrix como administrador.</span><span class="sxs-lookup"><span data-stu-id="37b3d-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="37b3d-128">Certifique-se de que a raiz do seu site tenha pelo menos um *. cshtml* arquivo nele.</span><span class="sxs-lookup"><span data-stu-id="37b3d-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="37b3d-129">Se você vir esse erro quando o servidor web está em um servidor remoto, entre em contato com o administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="37b3d-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="37b3d-130">Certifique-se de que o servidor tem o .NET Framework 4 ou posterior instalado.</span><span class="sxs-lookup"><span data-stu-id="37b3d-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="37b3d-131">Além disso, certifique-se de que o aplicativo é executado em um pool de aplicativos está configurado para usar essa versão do.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="37b3d-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="37b3d-132">Se você tiver controle sobre o servidor, verifique se que ele está executando a versão correta do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="37b3d-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="37b3d-133">Você também pode tentar reparar a instalação executando o `aspnet_regiis -iru` comando.</span><span class="sxs-lookup"><span data-stu-id="37b3d-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="37b3d-134">(Por exemplo, se você instalar o IIS depois de instalar o .NET Framework, o IIS não será ser corretamente configurado para executar as páginas ASP.NET.) Para obter mais informações, consulte [ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="37b3d-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="37b3d-135">Erro de HTTP 403.14 - proibido</span><span class="sxs-lookup"><span data-stu-id="37b3d-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="37b3d-136">*O servidor Web está configurado para não listar o conteúdo desse diretório.*</span><span class="sxs-lookup"><span data-stu-id="37b3d-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="37b3d-137">Esse erro pode ocorrer se você solicitar um recurso protegido (como o *Web. config* arquivo) ou em uma pasta protegida (como *App\_dados* ou *aplicativo\_Código*).</span><span class="sxs-lookup"><span data-stu-id="37b3d-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="37b3d-138">Erro de HTTP 404.17 – não encontrado</span><span class="sxs-lookup"><span data-stu-id="37b3d-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="37b3d-139">*O conteúdo solicitado parece ser o script e não será servido pelo manipulador de arquivo estático.*</span><span class="sxs-lookup"><span data-stu-id="37b3d-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="37b3d-140">Esse erro pode ocorrer se o servidor não está configurado corretamente para usar o .NET Framework 4 ou posterior e, portanto, não reconhece o código em `@{ }` blocos.</span><span class="sxs-lookup"><span data-stu-id="37b3d-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="37b3d-141">Consulte a descrição anteriormente por *HTTP Erro 403 - Proibido: acesso negado*.</span><span class="sxs-lookup"><span data-stu-id="37b3d-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="37b3d-142">Erro de HTTP 404.7 - não encontrado</span><span class="sxs-lookup"><span data-stu-id="37b3d-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="37b3d-143">*O módulo de filtragem de solicitações é configurada para negar a extensão de arquivo*</span><span class="sxs-lookup"><span data-stu-id="37b3d-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="37b3d-144">Esse erro pode ocorrer se *. cshtml* ou *. vbhtml* extensões foram bloqueadas explicitamente no servidor.</span><span class="sxs-lookup"><span data-stu-id="37b3d-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="37b3d-145">Um sintoma desse problema é que o trabalho URLs quando elas não incluem a extensão, mas as URLs que incluem *. cshtml* ou *. vbhtml* não funcionam.</span><span class="sxs-lookup"><span data-stu-id="37b3d-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="37b3d-146">Uma solução possível é habilitar novamente as extensões do site *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="37b3d-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="37b3d-147">O exemplo a seguir mostra como habilitar o *. cshtml* extensão.</span><span class="sxs-lookup"><span data-stu-id="37b3d-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="37b3d-148">Erro de HTTP 404.8 - não encontrado</span><span class="sxs-lookup"><span data-stu-id="37b3d-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="37b3d-149">*O módulo de filtragem de solicitações está configurada para negar a um caminho da URL que contém uma seção hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="37b3d-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="37b3d-150">Esse erro pode ocorrer se você solicitar um recurso protegido (como o *Web. config* arquivo) ou em uma pasta protegida (como *App\_dados* ou *aplicativo\_Código*).</span><span class="sxs-lookup"><span data-stu-id="37b3d-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="37b3d-151">Esse tipo de página não é atendido (erro de servidor no aplicativo '/')</span><span class="sxs-lookup"><span data-stu-id="37b3d-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="37b3d-152">Consulte a descrição anterior de HTTP 404.17 de erro.</span><span class="sxs-lookup"><span data-stu-id="37b3d-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="37b3d-153">Problemas com o código do Razor</span><span class="sxs-lookup"><span data-stu-id="37b3d-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="37b3d-154">O nome '*classe*' não existe no contexto atual</span><span class="sxs-lookup"><span data-stu-id="37b3d-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="37b3d-155">Geralmente, um motivo que você vir esse erro é que `class` referências de um auxiliar, mas o auxiliar não está instalado.</span><span class="sxs-lookup"><span data-stu-id="37b3d-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="37b3d-156">Por exemplo, se você tentar usar um auxiliar, mas se você ainda não instalou o pacote do NuGet, você verá esse erro.</span><span class="sxs-lookup"><span data-stu-id="37b3d-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="37b3d-157">Use a galeria no WebMatrix para encontrar e instalar o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="37b3d-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="37b3d-158">Se o auxiliar está instalado, mas a página ainda não reconhecê-lo, tente adicionar adicionar um `using` instrução no código.</span><span class="sxs-lookup"><span data-stu-id="37b3d-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="37b3d-159">No `using` instrução, o namespace que inclui o auxiliar de referência.</span><span class="sxs-lookup"><span data-stu-id="37b3d-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="37b3d-160">Por exemplo, os auxiliares básicos que estão no pacote auxiliares do ASP.NET Web estão no `System.Web.Helpers` namespace.</span><span class="sxs-lookup"><span data-stu-id="37b3d-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="37b3d-161">Na parte superior da página em que você deseja usar o auxiliar, adicione esta linha:</span><span class="sxs-lookup"><span data-stu-id="37b3d-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="37b3d-162">Problemas com a segurança e associação</span><span class="sxs-lookup"><span data-stu-id="37b3d-162">Issues with Security and Membership</span></span>

<span data-ttu-id="37b3d-163">Se você estiver usando o sistema de segurança internas (associação) em páginas da Web do ASP.NET (Razor), você pode encontrar os problemas a seguir.</span><span class="sxs-lookup"><span data-stu-id="37b3d-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="37b3d-164">Para chamar esse método, a propriedade "Membership.Provider" deve ser uma instância de "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="37b3d-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="37b3d-165">Esse erro pode indicar que nenhum `AspNetSqlMembershipProvider` classe está configurada.</span><span class="sxs-lookup"><span data-stu-id="37b3d-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="37b3d-166">(Um sintoma é que o site funciona bem localmente, mas gera este erro quando você publicá-lo no servidor de um provedor de hospedagem). Uma correção para esse problema é habilitar explicitamente a associação simples, adicionando o seguinte para o site *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="37b3d-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="37b3d-167">Problemas com o envio de Email</span><span class="sxs-lookup"><span data-stu-id="37b3d-167">Issues with Sending Email</span></span>

<span data-ttu-id="37b3d-168">Problemas com o envio de email podem ser um desafio de depurar.</span><span class="sxs-lookup"><span data-stu-id="37b3d-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="37b3d-169">Um problema inicial pode ser que você não pode se conectar ao servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="37b3d-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="37b3d-170">Se a conexão for bem-sucedida, ASP.NET entrega a mensagem para o servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="37b3d-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="37b3d-171">No entanto, pode haver problemas com a mensagem em si que impede que o servidor SMTP enviá-la.</span><span class="sxs-lookup"><span data-stu-id="37b3d-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="37b3d-172">Se seu aplicativo não enviar email com êxito, tente o seguinte:</span><span class="sxs-lookup"><span data-stu-id="37b3d-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="37b3d-173">O nome do servidor SMTP costuma ser algo como `smtp.provider.com` ou `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="37b3d-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="37b3d-174">No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse ponto pode ser `localhost`.</span><span class="sxs-lookup"><span data-stu-id="37b3d-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="37b3d-175">Essa situação ocorre porque depois que você publicou e seu site está em execução no servidor do provedor, o servidor SMTP pode ser local da perspectiva do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37b3d-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="37b3d-176">Essa alteração de nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte de seu processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="37b3d-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="37b3d-177">Geralmente, o número da porta é 25.</span><span class="sxs-lookup"><span data-stu-id="37b3d-177">The port number is usually 25.</span></span> <span data-ttu-id="37b3d-178">No entanto, alguns provedores exigem que você usar a porta 587 ou alguma outra porta.</span><span class="sxs-lookup"><span data-stu-id="37b3d-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="37b3d-179">Verifique com o proprietário do servidor SMTP de qual número de porta que eles esperam que você use.</span><span class="sxs-lookup"><span data-stu-id="37b3d-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="37b3d-180">Certifique-se de que você use as credenciais corretas.</span><span class="sxs-lookup"><span data-stu-id="37b3d-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="37b3d-181">Se você publicar seu site em um provedor de hospedagem, use as credenciais que o provedor indicou especificamente são para email.</span><span class="sxs-lookup"><span data-stu-id="37b3d-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="37b3d-182">Essas credenciais podem ser diferentes das credenciais que usar para publicar.</span><span class="sxs-lookup"><span data-stu-id="37b3d-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="37b3d-183">Às vezes, você não precisa credenciais em todos os.</span><span class="sxs-lookup"><span data-stu-id="37b3d-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="37b3d-184">Se você estiver enviando um email por meio de seu ISP pessoal, seu provedor de email talvez já saiba suas credenciais.</span><span class="sxs-lookup"><span data-stu-id="37b3d-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="37b3d-185">Depois de publicar, você talvez precise usar credenciais diferentes quando você testa em seu computador local.</span><span class="sxs-lookup"><span data-stu-id="37b3d-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="37b3d-186">Se seu provedor de email usa a criptografia, defina `WebMail.EnableSsl` para `true`.</span><span class="sxs-lookup"><span data-stu-id="37b3d-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="37b3d-187">Se houver um erro ao enviar email, você poderá ver uma mensagem de erro padrão do ASP.NET, que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="37b3d-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Mensagem de erro do ASP.NET quando há um problema com o email](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="37b3d-189">Você também pode depurar problemas com o envio de email usando um `try-catch` bloco, como no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="37b3d-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="37b3d-190">Quando você usa um `try-catch` bloco, o ASP.NET não exibe suas mensagens de erro padrão.</span><span class="sxs-lookup"><span data-stu-id="37b3d-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="37b3d-191">Em vez disso, você pode capturar o erro no `catch` parte do bloco.</span><span class="sxs-lookup"><span data-stu-id="37b3d-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="37b3d-192">Substitua os valores apropriados para `your-SMTP-server-name`e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="37b3d-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="37b3d-193">Algumas das mensagens de erro, que talvez você veja dessa forma incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="37b3d-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="37b3d-194">*Falha ao enviar email.*</span><span class="sxs-lookup"><span data-stu-id="37b3d-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="37b3d-195">-ou-</span><span class="sxs-lookup"><span data-stu-id="37b3d-195">-or-</span></span>

    <span data-ttu-id="37b3d-196">*Uma tentativa de conexão falhou porque a parte conectada não respondeu corretamente após um período de tempo ou a conexão estabelecida falhou porque o host conectado não respondeu*</span><span class="sxs-lookup"><span data-stu-id="37b3d-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="37b3d-197">Esse erro geralmente significa que o aplicativo não pôde se conectar ao servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="37b3d-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="37b3d-198">Verifique o nome do servidor e número da porta.</span><span class="sxs-lookup"><span data-stu-id="37b3d-198">Check the server name and port number.</span></span>
- <span data-ttu-id="37b3d-199"><em>Caixa de correio indisponível. A resposta do servidor foi: 5.1.0 &lt; someuser@invaliddomain &gt; remetente rejeitado: domínio do remetente inválido</em></span><span class="sxs-lookup"><span data-stu-id="37b3d-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="37b3d-200">Essa mensagem pode indicar que o `From` endereço não está correto ou está ausente.</span><span class="sxs-lookup"><span data-stu-id="37b3d-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="37b3d-201">*A cadeia de caracteres especificada não está no formato necessário para um endereço de email.*</span><span class="sxs-lookup"><span data-stu-id="37b3d-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="37b3d-202">Esse erro pode indicar que o valor de `To` ou `From` propriedades não são reconhecidas como endereços de email.</span><span class="sxs-lookup"><span data-stu-id="37b3d-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="37b3d-203">(ASP.NET não é possível verificar se o endereço de email é válido, apenas que ele do dentro o formato correto, como *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="37b3d-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="37b3d-204">Remova a marcação que exibe o erro (`@errorMessage`) antes de publicar a página em um site ativo.</span><span class="sxs-lookup"><span data-stu-id="37b3d-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="37b3d-205">Não é uma boa ideia permitir que os usuários a ver as mensagens de erro que você obtém de um servidor.</span><span class="sxs-lookup"><span data-stu-id="37b3d-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="37b3d-206">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="37b3d-206">Additional Resources</span></span>

[<span data-ttu-id="37b3d-207">Perguntas frequentes sobre Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="37b3d-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="37b3d-208">[O WebMatrix e páginas da Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) fórum no site do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="37b3d-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
