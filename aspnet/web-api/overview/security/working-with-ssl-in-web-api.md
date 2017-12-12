---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Trabalhando com SSL na API da Web | Microsoft Docs
author: MikeWasson
description: Mostra como usar o SSL com a API da Web do ASP.NET, incluindo o uso de certificados de cliente SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8c631900c8c5ab6097e0cb9fd4a71abbcba1c88b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="8cf1f-103">Trabalhando com SSL na API da Web</span><span class="sxs-lookup"><span data-stu-id="8cf1f-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="8cf1f-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8cf1f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8cf1f-105">Vários esquemas de autenticação comuns não são seguras por HTTP sem formatação.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="8cf1f-106">Em particular, a autenticação básica e autenticação de formulários enviam credenciais sem criptografia.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="8cf1f-107">Para ser seguro, esses esquemas de autenticação *deve* usar SSL.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="8cf1f-108">Além disso, os certificados de cliente SSL podem ser usados para autenticar clientes.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="8cf1f-109">Habilitar o SSL no servidor</span><span class="sxs-lookup"><span data-stu-id="8cf1f-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="8cf1f-110">Para configurar o SSL no IIS 7 ou posterior:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="8cf1f-111">Criar ou obter um certificado.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-111">Create or get a certificate.</span></span> <span data-ttu-id="8cf1f-112">Para testar, você pode criar um certificado autoassinado.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="8cf1f-113">Adicione uma associação HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="8cf1f-114">Para obter detalhes, consulte [como configurar o SSL no IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="8cf1f-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="8cf1f-115">Para testes locais, você pode habilitar o SSL no IIS Express do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="8cf1f-116">Na janela Propriedades, defina **SSL habilitado** para **True**.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="8cf1f-117">Observe o valor de **SSL URL**; usar essa URL para testar conexões HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="8cf1f-118">Imposição de SSL em um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="8cf1f-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="8cf1f-119">Se você tiver um HTTPS e uma associação HTTP, os clientes ainda podem usar HTTP para acessar o site.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="8cf1f-120">Você pode permitir que alguns recursos estejam disponíveis por meio de HTTP, enquanto outros recursos exigem SSL.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="8cf1f-121">Nesse caso, use um filtro de ação para exigir SSL para os recursos protegidos.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="8cf1f-122">O código a seguir mostra um filtro de autenticação de API da Web que verifica o SSL:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="8cf1f-123">Adicione esse filtro para as ações de API da Web que exigem SSL:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="8cf1f-124">Certificados de cliente SSL</span><span class="sxs-lookup"><span data-stu-id="8cf1f-124">SSL Client Certificates</span></span>

<span data-ttu-id="8cf1f-125">O SSL fornece autenticação usando certificados de infraestrutura de chave pública.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="8cf1f-126">O servidor deve fornecer um certificado que autentica o servidor para o cliente.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="8cf1f-127">É menos comum para o cliente fornecer um certificado para o servidor, mas essa é uma opção para autenticação de clientes.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="8cf1f-128">Para usar certificados de cliente com SSL, você precisa de uma maneira de distribuir certificados autoassinados para seus usuários.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="8cf1f-129">Para muitos tipos de aplicativo, isso não será uma boa experiência do usuário, mas em alguns ambientes (por exemplo, enterprise) pode ser viável.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="8cf1f-130">Vantagens</span><span class="sxs-lookup"><span data-stu-id="8cf1f-130">Advantages</span></span> | <span data-ttu-id="8cf1f-131">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="8cf1f-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="8cf1f-132">-As credenciais de certificado são mais seguras do que o nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="8cf1f-133">-SSL fornece um canal seguro concluído, com autenticação, integridade e criptografia de mensagens.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="8cf1f-134">-Você deve obter e gerenciar certificados PKI.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="8cf1f-135">-A plataforma do cliente deve oferecer suporte a certificados de cliente SSL.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="8cf1f-136">Para configurar o IIS para aceitar certificados de cliente, abra o Gerenciador do IIS e execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="8cf1f-137">Clique no nó do site no modo de exibição de árvore.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="8cf1f-138">Clique duas vezes o **configurações de SSL** recurso no painel central.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="8cf1f-139">Em **certificados de cliente**, selecione uma destas opções:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="8cf1f-140">**Aceitar**: IIS aceitará um certificado do cliente, mas não requer um.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="8cf1f-141">**Exigir**: exigem um certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="8cf1f-142">(Para habilitar essa opção, você também deve selecionar "Exigir SSL")</span><span class="sxs-lookup"><span data-stu-id="8cf1f-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="8cf1f-143">Você também pode definir essas opções no arquivo applicationHost. config:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="8cf1f-144">O **SslNegotiateCert** sinalizador significa IIS aceitará um certificado do cliente, mas não requer um (equivalente à opção "Aceitar" no Gerenciador do IIS).</span><span class="sxs-lookup"><span data-stu-id="8cf1f-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="8cf1f-145">Para solicitar um certificado, defina o **SslRequireCert** sinalizador.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="8cf1f-146">Para testar, você também pode definir essas opções no IIS Express, em que o host de aplicativos local. Arquivo de configuração, localizado em "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="8cf1f-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="8cf1f-147">Criar um certificado de cliente para teste</span><span class="sxs-lookup"><span data-stu-id="8cf1f-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="8cf1f-148">Para fins de teste, você pode usar [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) para criar um certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="8cf1f-149">Primeiro, crie uma autoridade raiz de teste:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="8cf1f-150">Makecert solicitará que você insira uma senha para a chave privada.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="8cf1f-151">Em seguida, adicione o certificado para o teste "Autoridades de certificação confiáveis" do servidor armazenar, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="8cf1f-152">Abra o MMC.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-152">Open MMC.</span></span>
2. <span data-ttu-id="8cf1f-153">Em **arquivo**, selecione **Adicionar/Remover Snap-In**.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="8cf1f-154">Selecione **conta de computador**.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="8cf1f-155">Selecione **computador Local** e conclua o assistente.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="8cf1f-156">No painel de navegação, expanda o nó de "Autoridades de certificação confiáveis".</span><span class="sxs-lookup"><span data-stu-id="8cf1f-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="8cf1f-157">Sobre o **ação** , aponte para **todas as tarefas**e, em seguida, clique em **importação** para iniciar o Assistente de importação de certificado.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="8cf1f-158">Navegue até o arquivo de certificado, TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="8cf1f-159">Clique em **abrir**, em seguida, clique em **próximo** e conclua o assistente.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="8cf1f-160">(Você será solicitado a inserir novamente a senha.)</span><span class="sxs-lookup"><span data-stu-id="8cf1f-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="8cf1f-161">Agora, crie um certificado de cliente que está assinado pelo certificado primeiro:</span><span class="sxs-lookup"><span data-stu-id="8cf1f-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="8cf1f-162">Usando certificados de cliente na API da Web</span><span class="sxs-lookup"><span data-stu-id="8cf1f-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="8cf1f-163">No lado do servidor, você pode obter o certificado de cliente chamando [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) na mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="8cf1f-164">O método retornará nulo se não houver nenhum certificado do cliente.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="8cf1f-165">Caso contrário, ele retorna um **X509Certificate2** instância.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="8cf1f-166">Use esse objeto para obter informações do certificado, como o assunto e emissor.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="8cf1f-167">Em seguida, você pode usar essas informações para autenticação ou autorização.</span><span class="sxs-lookup"><span data-stu-id="8cf1f-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
