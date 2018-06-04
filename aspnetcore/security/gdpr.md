---
title: Suporte a norma de proteção de dados geral (GDPR) no núcleo do ASP.NET
author: rick-anderson
description: Mostra como acessar os pontos de extensão GDPR no núcleo do ASP.NET de aplicativo web.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688621"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="42269-103">Suporte de regulamentação de proteção de dados geral (GDPR) da UE no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="42269-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="42269-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42269-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42269-105">ASP.NET Core fornece APIs e modelos para ajudar a atender alguns do [regulamentação de proteção de dados geral (GDPR) da UE](https://www.eugdpr.org/) requisitos:</span><span class="sxs-lookup"><span data-stu-id="42269-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="42269-106">Os modelos de projeto incluem pontos de extensão e marcação fragmentada, que você pode substituir por sua privacidade e a política de uso de cookies.</span><span class="sxs-lookup"><span data-stu-id="42269-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="42269-107">Um recurso de consentimento do cookie permite solicitar (e acompanhar) consentimento dos usuários para o armazenamento de informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="42269-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="42269-108">Se um usuário não aceitou a coleta de dados e o aplicativo é definido com [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) para `true`, cookies não-essenciais não serão enviados para o navegador.</span><span class="sxs-lookup"><span data-stu-id="42269-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="42269-109">Cookies podem ser marcados como essenciais.</span><span class="sxs-lookup"><span data-stu-id="42269-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="42269-110">Cookies essenciais são enviados para o navegador, mesmo quando o usuário não aceitou e rastreamento é desabilitado.</span><span class="sxs-lookup"><span data-stu-id="42269-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="42269-111">[Cookies de sessão e TempData](#tempdata) não são funcionais quando o controle está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="42269-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="42269-112">O [gerenciar identidade](#pd) página fornece um link para baixar e excluir dados de usuário.</span><span class="sxs-lookup"><span data-stu-id="42269-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="42269-113">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) permite que você teste a maioria dos pontos de extensão GDPR e APIs adicionado aos modelos ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="42269-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="42269-114">Consulte o [Leiame](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) arquivo para testar as instruções.</span><span class="sxs-lookup"><span data-stu-id="42269-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="42269-115">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="42269-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="42269-116">Suporte do ASP.NET Core GDPR no código de modelo gerado</span><span class="sxs-lookup"><span data-stu-id="42269-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="42269-117">Páginas Razor e MVC projetos criados com os modelos de projeto incluem o seguinte GDPR:</span><span class="sxs-lookup"><span data-stu-id="42269-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="42269-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) são definidos no `Startup`.</span><span class="sxs-lookup"><span data-stu-id="42269-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="42269-119">O *_CookieConsentPartial.cshtml* [exibição parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="42269-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="42269-120">O *Pages/Privacy.cshtml* ou *Home/Privacy.cshtml* exibição fornece uma página detalhada da política de privacidade do seu site.</span><span class="sxs-lookup"><span data-stu-id="42269-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="42269-121">O *_CookieConsentPartial.cshtml* arquivo gera um link para a página de privacidade.</span><span class="sxs-lookup"><span data-stu-id="42269-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="42269-122">Para aplicativos criados com contas de usuário individuais, a página de gerenciamento fornece links para baixar e excluir [dados pessoais do usuário](#pd).</span><span class="sxs-lookup"><span data-stu-id="42269-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="42269-123">CookiePolicyOptions e UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="42269-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="42269-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) são inicializadas no `Startup` classe `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="42269-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="42269-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) é chamado `Startup` classe `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="42269-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="42269-126">Exibição parcial _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="42269-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="42269-127">O *_CookieConsentPartial.cshtml* exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="42269-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="42269-128">Essa parcial:</span><span class="sxs-lookup"><span data-stu-id="42269-128">This partial:</span></span>

* <span data-ttu-id="42269-129">Obtém o estado do controle do usuário.</span><span class="sxs-lookup"><span data-stu-id="42269-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="42269-130">Se o aplicativo está configurado para exigir o consentimento do usuário deve consentir antes de cookies que podem ser controlados.</span><span class="sxs-lookup"><span data-stu-id="42269-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="42269-131">Se o consentimento for necessário, o chrome de consentimento do cookie é fixa na parte superior da barra de navegação criada no *Pages/Shared/_Layout.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="42269-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="42269-132">Fornece um HTML `<p>` elemento para resumir sua privacidade e cookies usar a política.</span><span class="sxs-lookup"><span data-stu-id="42269-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="42269-133">Fornece um link para *Pages/Privacy.cshtml* onde você pode detalhar a política de privacidade do seu site.</span><span class="sxs-lookup"><span data-stu-id="42269-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="42269-134">Cookies essenciais</span><span class="sxs-lookup"><span data-stu-id="42269-134">Essential cookies</span></span>

<span data-ttu-id="42269-135">Se o consentimento não foi fornecido, somente os cookies marcados essenciais são enviados para o navegador.</span><span class="sxs-lookup"><span data-stu-id="42269-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="42269-136">O código a seguir faz com que um cookie essenciais:</span><span class="sxs-lookup"><span data-stu-id="42269-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="42269-137">Cookies de estado de sessão e de provedor TempData não são essenciais</span><span class="sxs-lookup"><span data-stu-id="42269-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="42269-138">O [provedor Tempdata](xref:fundamentals/app-state#tempdata) cookie não é essencial.</span><span class="sxs-lookup"><span data-stu-id="42269-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="42269-139">Se o controle está desabilitado, o provedor de Tempdata não está funcionando.</span><span class="sxs-lookup"><span data-stu-id="42269-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="42269-140">Para habilitar o provedor Tempdata quando o controle está desabilitado, marcar o cookie de TempData como essenciais em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="42269-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="42269-141">[Estado de sessão](xref:fundamentals/app-state) cookies não são essenciais.</span><span class="sxs-lookup"><span data-stu-id="42269-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="42269-142">Estado da sessão não está funcionando, quando o controle está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="42269-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="42269-143">Dados pessoais</span><span class="sxs-lookup"><span data-stu-id="42269-143">Personal data</span></span>

<span data-ttu-id="42269-144">Aplicativos do ASP.NET Core criados com contas de usuário individuais incluem código para baixar e excluir dados pessoais.</span><span class="sxs-lookup"><span data-stu-id="42269-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="42269-145">Selecione o nome de usuário e, em seguida, selecione **dados pessoais**:</span><span class="sxs-lookup"><span data-stu-id="42269-145">Select the user name and then select **Personal data**:</span></span>

![Gerenciar a página de dados pessoais](gdpr/_static/pd.png)

<span data-ttu-id="42269-147">Notas:</span><span class="sxs-lookup"><span data-stu-id="42269-147">Notes:</span></span>

* <span data-ttu-id="42269-148">Para gerar o `Account/Manage` de código, consulte [Scaffold identidade](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="42269-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="42269-149">Excluir e baixar apenas impacto sobre os dados de identidade padrão.</span><span class="sxs-lookup"><span data-stu-id="42269-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="42269-150">Os dados de usuário personalizadas de criação de aplicativos devem ser estendidos para delete/baixar os dados de usuário personalizada.</span><span class="sxs-lookup"><span data-stu-id="42269-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="42269-151">O problema do GitHub [como adicionar/excluir dados de usuário personalizada para identidade](https://github.com/aspnet/Docs/issues/6226) acompanha um artigo proposto na criação personalizada/excluir/download de dados de usuário personalizada.</span><span class="sxs-lookup"><span data-stu-id="42269-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="42269-152">Se você quiser ver esse tópico priorizado, deixe um polegar para cima reação no problema.</span><span class="sxs-lookup"><span data-stu-id="42269-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="42269-153">Salvo tokens para o usuário que são armazenados na tabela de banco de dados de identidade `AspNetUserTokens` são excluídos quando o usuário é excluído por meio do comportamento de exclusão em cascata devido ao [chave estrangeira](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="42269-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="42269-154">Criptografia em repouso</span><span class="sxs-lookup"><span data-stu-id="42269-154">Encryption at rest</span></span>

<span data-ttu-id="42269-155">Alguns bancos de dados e mecanismos de armazenamento permitem criptografia em repouso.</span><span class="sxs-lookup"><span data-stu-id="42269-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="42269-156">Criptografia em repouso:</span><span class="sxs-lookup"><span data-stu-id="42269-156">Encryption at rest:</span></span>

* <span data-ttu-id="42269-157">Criptografa dados armazenados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="42269-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="42269-158">Criptografa sem configuração, programação ou outro trabalho para o software que acessa os dados.</span><span class="sxs-lookup"><span data-stu-id="42269-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="42269-159">É a opção mais fácil e segura.</span><span class="sxs-lookup"><span data-stu-id="42269-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="42269-160">Permite que o banco de dados gerenciar chaves e criptografia.</span><span class="sxs-lookup"><span data-stu-id="42269-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="42269-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="42269-161">For example:</span></span>

* <span data-ttu-id="42269-162">Microsoft SQL e SQL Azure fornecem [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="42269-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="42269-163">O SQL Azure criptografa o banco de dados por padrão</span><span class="sxs-lookup"><span data-stu-id="42269-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="42269-164">[Blobs do Azure, arquivos, tabela e fila de armazenamento são criptografadas por padrão](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="42269-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="42269-165">Você poderá usar a criptografia de disco para fornecer a mesma proteção para bancos de dados que não fornecem criptografia interna em repouso.</span><span class="sxs-lookup"><span data-stu-id="42269-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="42269-166">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="42269-166">For example:</span></span>

* [<span data-ttu-id="42269-167">BitLocker para windows server</span><span class="sxs-lookup"><span data-stu-id="42269-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="42269-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="42269-168">Linux:</span></span>
  * [<span data-ttu-id="42269-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="42269-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="42269-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="42269-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42269-171">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="42269-171">Additional Resources</span></span>

* [<span data-ttu-id="42269-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="42269-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
