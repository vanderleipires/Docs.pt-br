---
uid: ajax/cdn/overview
title: "Rede de fornecimento de conteúdo do Microsoft Ajax | Microsoft Docs"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="9772e-102">Rede de fornecimento de conteúdo do Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="9772e-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="9772e-103">Observação: A Microsoft Ajax CDN não tem nenhum SLA além de usar um CDN do Azure.</span><span class="sxs-lookup"><span data-stu-id="9772e-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="9772e-104">Sumário</span><span class="sxs-lookup"><span data-stu-id="9772e-104">Table of Contents</span></span>

<span data-ttu-id="9772e-105">**[AJAX.microsoft.com renomeado para ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="9772e-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="9772e-106">**[Suporte de .vsdoc do Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="9772e-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="9772e-107">**[Usando o ASP.NET Ajax da CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="9772e-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="9772e-108">**[Usando jQuery da CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="9772e-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="9772e-109">**[Usando a interface do usuário da CDN do jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="9772e-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="9772e-110">**[Arquivos de terceiros em CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="9772e-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="9772e-111">Versões do jQuery em CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="9772e-112">jQuery migrar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="9772e-113">Versões de interface do usuário na CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="9772e-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="9772e-114">jQuery lançamentos de validação no CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="9772e-115">jQuery Mobile versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="9772e-116">jQuery versões de modelos na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="9772e-117">jQuery ciclo versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="9772e-118">jQuery DataTables versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="9772e-119">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="9772e-120">Versões de JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="9772e-121">Versões de separação na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="9772e-122">Globalizar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="9772e-123">Responder versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="9772e-124">Versões de inicialização na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="9772e-125">Versões de inicialização TouchCarousel na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="9772e-126">Versões de hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="9772e-127">Web Forms do ASP.NET e Ajax versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="9772e-128">O ASP.NET MVC lança na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="9772e-129">Versões do ASP.NET SignalR em CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="9772e-130">A Microsoft Ajax Content Delivery Network (CDN) hospeda bibliotecas JavaScript de terceiros populares, como jQuery e permite que você adicioná-los facilmente para seus aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="9772e-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="9772e-131">Por exemplo, começar a usar o jQuery que está hospedado neste CDN simplesmente adicionando um &lt;script&gt; marca para a página que aponta para ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="9772e-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="9772e-132">Aproveitando a CDN, você pode melhorar significativamente o desempenho de seus aplicativos Ajax.</span><span class="sxs-lookup"><span data-stu-id="9772e-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="9772e-133">O conteúdo da CDN do é armazenados em cache nos servidores localizados em todo o mundo.</span><span class="sxs-lookup"><span data-stu-id="9772e-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="9772e-134">Além disso, o CDN permite que navegadores para reutilizar arquivos JavaScript de terceiros em cache para sites da web que estão localizados em domínios diferentes.</span><span class="sxs-lookup"><span data-stu-id="9772e-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="9772e-135">A CDN oferece suporte a SSL (HTTPS), caso seja necessário atender a uma página da web usando o protocolo SSL.</span><span class="sxs-lookup"><span data-stu-id="9772e-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="9772e-136">A CDN hospeda as seguintes bibliotecas de scripts de terceiros que foram carregadas e estão licenciadas para você, pelos proprietários dessas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="9772e-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="9772e-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="9772e-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="9772e-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="9772e-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="9772e-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="9772e-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="9772e-140">jQuery validação (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="9772e-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="9772e-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="9772e-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="9772e-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="9772e-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="9772e-143">A Microsoft Ajax CDN também inclui as seguintes bibliotecas que foram carregadas pela Microsoft:</span><span class="sxs-lookup"><span data-stu-id="9772e-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="9772e-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="9772e-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="9772e-145">Arquivos de JavaScript do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9772e-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="9772e-146">Arquivos do ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="9772e-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="9772e-147">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="9772e-148">Os proprietários de direitos autorais das bibliotecas são licenciamento dessas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="9772e-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="9772e-149">Quaisquer direitos que você precisará baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="9772e-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="9772e-150">Porque eles não são bibliotecas Microsoft, a Microsoft não fornece nenhuma garantia ou licenças de direitos de propriedade intelectual (incluindo sem direitos de patente implícitos) para as bibliotecas de terceiros hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="9772e-151">Se você deseja enviar sua biblioteca de JavaScript e a biblioteca é uma das principais bibliotecas JavaScript (conforme listado em http://trends.builtwith.com) ou extensões/plug-ins para essas bibliotecas que são (a) populares; ou (b) útil para uso em ASP.NET e em seguida, entre em contato com AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="9772e-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="9772e-152">AJAX.microsoft.com renomeado para ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="9772e-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="9772e-153">A CDN usado para usar o nome de domínio microsoft.com e foi alterada para usar o nome de domínio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="9772e-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="9772e-154">Essa alteração foi feita para aumentar o desempenho porque quando um navegador referenciada domínio microsoft.com poderia enviar os cookies do domínio pela conexão com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="9772e-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="9772e-155">Renomeando com um nome de domínio diferente microsoft.com desempenho pode ser aumentado pelo máximo de 25%.</span><span class="sxs-lookup"><span data-stu-id="9772e-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="9772e-156">Observação ajax.microsoft.com continuarão a funcionar, mas ajax.aspnetcdn.com é recomendado.</span><span class="sxs-lookup"><span data-stu-id="9772e-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="9772e-157">Formato antigo: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="9772e-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="9772e-158">Novo formato de: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="9772e-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="9772e-159">Suporte de .vsdoc do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9772e-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="9772e-160">Para usar os arquivos de .vsdoc corretamente com o Visual Studio 2008, você precisará certificar-se de que você tenha VS 2008 SP1 e o hotfix para vsdoc arquivos instalados.</span><span class="sxs-lookup"><span data-stu-id="9772e-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="9772e-161">Você pode obter essas aqui:</span><span class="sxs-lookup"><span data-stu-id="9772e-161">You can get these from here:</span></span>

- [<span data-ttu-id="9772e-162">Baixe o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="9772e-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "baixar o Visual Studio 2008 SP1")
- [<span data-ttu-id="9772e-163">Baixar o hotfix .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="9772e-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "baixar o hotfix .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="9772e-164">Visual Studio 2010 dá suporte a arquivos .vsdoc sem qualquer patches adicionais.</span><span class="sxs-lookup"><span data-stu-id="9772e-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="9772e-165">Usando o ASP.NET Ajax da CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="9772e-166">Ao usar o ASP.NET 4, você pode redirecionar todas as solicitações de scripts de estrutura do ASP.NET para a CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="9772e-167">Recuperar scripts da CDN, em vez de seu servidor web local substancialmente pode melhorar o desempenho de sites da Web ASP.NET pública.</span><span class="sxs-lookup"><span data-stu-id="9772e-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="9772e-168">Use a propriedade ScriptManager EnableCDN para redirecionar todas as solicitações de script do ASP.NET framework para a Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="9772e-169">Usando jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="9772e-170">Você pode usar scripts de jQuery hospedados na CDN no seu aplicativo Web, adicionando o seguinte elemento de script para uma página:</span><span class="sxs-lookup"><span data-stu-id="9772e-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="9772e-171">A CDN também inclui a versão minimizada do script jQuery, que você pode obter usando o seguinte elemento:</span><span class="sxs-lookup"><span data-stu-id="9772e-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="9772e-172">Para permitir que a página de fallback para carregar jQuery de um caminho local no seu próprio site caso o CDN não esteja disponível, adicione o seguinte elemento imediatamente depois do elemento referenciando o CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="9772e-173">A página de exemplo a seguir usa a versão CDN da biblioteca do jQuery (com um fallback para uma cópia local) para exibir o conteúdo de um elemento div quando um botão é clicado.</span><span class="sxs-lookup"><span data-stu-id="9772e-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="9772e-174">Você pode saber mais sobre jQuery e baixar uma cópia local do jQuery visitando o [jQuery](http://jquery.com/) site da Web.</span><span class="sxs-lookup"><span data-stu-id="9772e-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="9772e-175">Usando a interface do usuário da CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="9772e-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="9772e-176">A CDN também hospeda a biblioteca de interface do usuário da jQuery.</span><span class="sxs-lookup"><span data-stu-id="9772e-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="9772e-177">A biblioteca de interface do usuário da jQuery inclui um conjunto avançado de widgets e efeitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9772e-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="9772e-178">Por exemplo, a página a seguir ilustra como você pode usar o jQuery UI Datepicker no contexto de um aplicativo de Web Forms do ASP.NET para exibir um calendário pop-up:</span><span class="sxs-lookup"><span data-stu-id="9772e-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="9772e-179">Quando você move o foco para a caixa de texto usando o teclado, será exibido um calendário:</span><span class="sxs-lookup"><span data-stu-id="9772e-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendário pop-up criado com Datepicker](overview/_static/image1.png)

<span data-ttu-id="9772e-181">Observe que você deve incluir três arquivos da CDN no código acima:</span><span class="sxs-lookup"><span data-stu-id="9772e-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="9772e-182">A biblioteca jQuery &mdash; a biblioteca de interface do usuário da jQuery depende da biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="9772e-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="9772e-183">Você deve adicionar a biblioteca jQuery para sua página antes de adicionar a biblioteca de interface do usuário da jQuery.</span><span class="sxs-lookup"><span data-stu-id="9772e-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="9772e-184">A biblioteca de interface do usuário da jQuery &mdash; a biblioteca de interface do usuário do jQuery contém todos os efeitos de interface do usuário da jQuery e widgets, como o widget Datepicker usado na página acima.</span><span class="sxs-lookup"><span data-stu-id="9772e-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="9772e-185">Um tema de interface do usuário da jQuery &mdash; o jQuery UI dá suporte a temas diferentes.</span><span class="sxs-lookup"><span data-stu-id="9772e-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="9772e-186">A página acima inclui um link para um arquivo CSS para importar o tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="9772e-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="9772e-187">Todos os temas de interface do usuário da jQuery padrão são hospedados na CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="9772e-188">[Visite essa página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 no Microsoft Ajax CDN") para visualizar miniaturas de cada tema.</span><span class="sxs-lookup"><span data-stu-id="9772e-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="9772e-189">Para saber mais sobre a biblioteca de interface do usuário do jQuery, visite o oficial [site de interface do usuário da jQuery](http://jQueryUI.com "site de interface do usuário da jQuery").</span><span class="sxs-lookup"><span data-stu-id="9772e-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="9772e-190">Arquivos de terceiros em CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="9772e-191">A CDN hospeda algumas das bibliotecas de JavaScript de terceiros mais populares.</span><span class="sxs-lookup"><span data-stu-id="9772e-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="9772e-192">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="9772e-193">Os proprietários de direitos autorais das bibliotecas são licenciamento dessas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="9772e-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="9772e-194">Quaisquer direitos que você precisará baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="9772e-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="9772e-195">Porque eles não são bibliotecas Microsoft, a Microsoft não fornece nenhuma garantia ou licenças de direitos de propriedade intelectual (incluindo sem direitos de patente implícitos) para as bibliotecas de terceiros hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="9772e-196">Versões do jQuery em CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="9772e-197">As seguintes versões do jQuery estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="9772e-198">versão do jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="9772e-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="9772e-199">jQuery versão 3.2.1</span><span class="sxs-lookup"><span data-stu-id="9772e-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="9772e-200">versão do jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="9772e-201">versão do jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="9772e-202">jQuery versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="9772e-203">versão do jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="9772e-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="9772e-204">versão do jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="9772e-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="9772e-205">versão do jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="9772e-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="9772e-206">versão 2.2.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="9772e-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="9772e-207">jQuery versão 2.2.1</span><span class="sxs-lookup"><span data-stu-id="9772e-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="9772e-208">versão do jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="9772e-209">versão do jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="9772e-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="9772e-210">versão do jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="9772e-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="9772e-211">versão do jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="9772e-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="9772e-212">versão do jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="9772e-213">versão do jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="9772e-214">versão do jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="9772e-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="9772e-215">versão do jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="9772e-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="9772e-216">jQuery versão 2.0.1</span><span class="sxs-lookup"><span data-stu-id="9772e-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="9772e-217">jQuery versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9772e-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="9772e-218">versão do jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="9772e-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="9772e-219">versão do jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="9772e-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="9772e-220">versão do jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="9772e-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="9772e-221">versão do jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="9772e-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="9772e-222">versão do jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="9772e-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="9772e-223">versão do jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="9772e-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="9772e-224">versão do jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="9772e-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="9772e-225">versão do jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="9772e-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="9772e-226">versão do jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="9772e-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="9772e-227">versão do jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="9772e-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="9772e-228">versão do jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="9772e-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="9772e-229">versão do jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="9772e-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="9772e-230">versão do jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="9772e-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="9772e-231">versão do jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="9772e-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="9772e-232">versão do jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="9772e-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="9772e-233">versão do jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="9772e-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="9772e-234">jQuery versão 1.8.1</span><span class="sxs-lookup"><span data-stu-id="9772e-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="9772e-235">versão do jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="9772e-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="9772e-236">versão do jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="9772e-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="9772e-237">versão do jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="9772e-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="9772e-238">jQuery versão 1.7</span><span class="sxs-lookup"><span data-stu-id="9772e-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="9772e-239">versão do jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="9772e-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="9772e-240">versão do jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="9772e-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="9772e-241">versão do jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="9772e-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="9772e-242">versão do jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="9772e-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="9772e-243">jQuery versão 1.6</span><span class="sxs-lookup"><span data-stu-id="9772e-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="9772e-244">versão do jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="9772e-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="9772e-245">versão do jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="9772e-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="9772e-246">versão do jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="9772e-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="9772e-247">versão do jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="9772e-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="9772e-248">versão do jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="9772e-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="9772e-249">versão do jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="9772e-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="9772e-250">versão do jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="9772e-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="9772e-251">jQuery versão 1.4</span><span class="sxs-lookup"><span data-stu-id="9772e-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="9772e-252">versão do jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="9772e-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="9772e-253">jQuery migrar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="9772e-254">As seguintes versões do jQuery migrar estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="9772e-255">jQuery migrar versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="9772e-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="9772e-256">jQuery migrar versão 1.2.1</span><span class="sxs-lookup"><span data-stu-id="9772e-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="9772e-257">jQuery migrar versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="9772e-258">jQuery migrar versão 1.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="9772e-259">jQuery migrar versão 1.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="9772e-260">Migrar a versão 1.0.0 do jQuery</span><span class="sxs-lookup"><span data-stu-id="9772e-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="9772e-261">Versões de interface do usuário na CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="9772e-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="9772e-262">As seguintes versões da biblioteca de interface do usuário do jQuery são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="9772e-263">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="9772e-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="9772e-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="9772e-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="9772e-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="9772e-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="9772e-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="9772e-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="9772e-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="9772e-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="9772e-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interface do usuário no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="9772e-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="9772e-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="9772e-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="9772e-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-278">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="9772e-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="9772e-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="9772e-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="9772e-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="9772e-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="9772e-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="9772e-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="9772e-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="9772e-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="9772e-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="9772e-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="9772e-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="9772e-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="9772e-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="9772e-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="9772e-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="9772e-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="9772e-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="9772e-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="9772e-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="9772e-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="9772e-299">jQuery lançamentos de validação no CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="9772e-300">As seguintes versões da biblioteca de validação jQuery são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="9772e-301">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-302">jQuery validar 1.17.0</span><span class="sxs-lookup"><span data-stu-id="9772e-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery validação 1.17.0")
- [<span data-ttu-id="9772e-303">jQuery validar 1.16.0</span><span class="sxs-lookup"><span data-stu-id="9772e-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery validação 1.16.0")
- [<span data-ttu-id="9772e-304">jQuery validar 1.15.1</span><span class="sxs-lookup"><span data-stu-id="9772e-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery validação 1.15.1")
- [<span data-ttu-id="9772e-305">jQuery validar 1.15.0</span><span class="sxs-lookup"><span data-stu-id="9772e-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery validação 1.15.0")
- [<span data-ttu-id="9772e-306">jQuery validar 1.14.0</span><span class="sxs-lookup"><span data-stu-id="9772e-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery validação 1.14.0")
- [<span data-ttu-id="9772e-307">jQuery validar 1.13.1</span><span class="sxs-lookup"><span data-stu-id="9772e-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery validação 1.13.1")
- [<span data-ttu-id="9772e-308">jQuery validar 1.13.0</span><span class="sxs-lookup"><span data-stu-id="9772e-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery validação 1.13.0")
- [<span data-ttu-id="9772e-309">jQuery validar 1.12.0</span><span class="sxs-lookup"><span data-stu-id="9772e-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery validação 1.12.0")
- [<span data-ttu-id="9772e-310">jQuery validar 1.11.1</span><span class="sxs-lookup"><span data-stu-id="9772e-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery validação 1.11.1")
- [<span data-ttu-id="9772e-311">jQuery validar 1.11.0</span><span class="sxs-lookup"><span data-stu-id="9772e-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery validação 1.11.0")
- [<span data-ttu-id="9772e-312">jQuery validar 1.10.0</span><span class="sxs-lookup"><span data-stu-id="9772e-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery validação 1.10.0")
- [<span data-ttu-id="9772e-313">jQuery validar 1.9</span><span class="sxs-lookup"><span data-stu-id="9772e-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versão 1.9")
- [<span data-ttu-id="9772e-314">jQuery validar 1.8.1</span><span class="sxs-lookup"><span data-stu-id="9772e-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versão 1.8.1")
- [<span data-ttu-id="9772e-315">jQuery validar 1.8</span><span class="sxs-lookup"><span data-stu-id="9772e-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versão 1.8")
- [<span data-ttu-id="9772e-316">jQuery validar 1.7</span><span class="sxs-lookup"><span data-stu-id="9772e-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versão 1.7")
- [<span data-ttu-id="9772e-317">jQuery validar 1.6</span><span class="sxs-lookup"><span data-stu-id="9772e-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validar 1.6")
- [<span data-ttu-id="9772e-318">jQuery validar 1.5.5</span><span class="sxs-lookup"><span data-stu-id="9772e-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validar 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="9772e-319">jQuery Mobile versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="9772e-320">As seguintes versões da biblioteca jQuery Mobile são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="9772e-321">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-322">o jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="9772e-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-323">o jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="9772e-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-324">o jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="9772e-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-325">o jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="9772e-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-326">o jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="9772e-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-327">o jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="9772e-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-328">o jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="9772e-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-329">o jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-330">o jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="9772e-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-331">o jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-332">o jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-333">o jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="9772e-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-334">o jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="9772e-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-335">o jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-336">o jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="9772e-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-337">o jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="9772e-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 no Microsoft Ajax CDN")
- [<span data-ttu-id="9772e-338">versão beta do jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="9772e-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 no Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="9772e-339">jQuery versões de modelos na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="9772e-340">As seguintes versões do plug-in do jQuery modelos são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="9772e-341">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-342">jQuery modelos Beta 1</span><span class="sxs-lookup"><span data-stu-id="9772e-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modelos Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="9772e-343">jQuery ciclo versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="9772e-344">As seguintes versões do plug-in do jQuery ciclo são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="9772e-345">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-346">jQuery ciclo 2.99</span><span class="sxs-lookup"><span data-stu-id="9772e-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 ciclo")
- [<span data-ttu-id="9772e-347">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="9772e-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [<span data-ttu-id="9772e-348">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="9772e-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="9772e-349">jQuery DataTables versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="9772e-350">As seguintes versões do plug-in jQuery DataTables são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="9772e-351">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-352">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="9772e-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="9772e-353">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="9772e-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="9772e-354">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="9772e-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="9772e-355">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="9772e-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="9772e-356">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="9772e-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="9772e-357">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="9772e-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="9772e-358">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="9772e-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="9772e-359">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="9772e-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="9772e-360">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="9772e-361">Os seguintes versões do [Modernizr](http://www.modernizr.com "Modernizr") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="9772e-362">Versões de JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="9772e-363">Os seguintes versões do [JSHint](http://www.jshint.com "JSHint") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="9772e-364">Versões de separação na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="9772e-365">Os seguintes versões do [Knockout](http://www.knockoutjs.com "Knockout") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="9772e-366">Globalizar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="9772e-367">Os seguintes versões do [Globalize](https://github.com/jquery/globalize "Globalize") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="9772e-368">A versão 1.0.0 de globalizar</span><span class="sxs-lookup"><span data-stu-id="9772e-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="9772e-369">Globalizar versão 0.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="9772e-370">todas as culturas</span><span class="sxs-lookup"><span data-stu-id="9772e-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="9772e-371">Substitua "{-código de cultura}" com o código de cultura desejado, por exemplo, a Microsoft globalize.culture.en GB.js== arquivos em CDN = = essas bibliotecas foram carregadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9772e-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="9772e-372">Responder versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="9772e-373">Os seguintes versões do [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") responder estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="9772e-374">Responder a versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="9772e-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="9772e-375">Responder a versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="9772e-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="9772e-376">Responder a versão 1.4.0</span><span class="sxs-lookup"><span data-stu-id="9772e-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="9772e-377">Responder a versão 1.3.0</span><span class="sxs-lookup"><span data-stu-id="9772e-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="9772e-378">Responder a versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="9772e-379">Versões de inicialização na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="9772e-380">Os seguintes versões do [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="9772e-381">Versão bootstrap 4.0.0</span><span class="sxs-lookup"><span data-stu-id="9772e-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="9772e-382">Versão bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="9772e-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="9772e-383">Versão bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="9772e-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="9772e-384">Inicialização versão 3.3.5</span><span class="sxs-lookup"><span data-stu-id="9772e-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="9772e-385">Versão bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="9772e-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="9772e-386">Versão bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="9772e-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="9772e-387">Versão bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="9772e-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="9772e-388">Versão bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="9772e-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="9772e-389">Versão bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="9772e-390">Versão bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="9772e-391">Inicialização versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="9772e-392">Versão bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="9772e-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="9772e-393">Versão bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="9772e-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="9772e-394">Versão bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="9772e-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="9772e-395">Versão bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="9772e-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="9772e-396">Inicialização versão 2.3.2</span><span class="sxs-lookup"><span data-stu-id="9772e-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="9772e-397">Versão bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="9772e-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="9772e-398">Versões de inicialização TouchCarousel na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="9772e-399">Os seguintes versões do [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") TouchCarousel Bootstrap versões são hospedados em CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="9772e-400">Inicialização TouchCarousel versão 0.8.0</span><span class="sxs-lookup"><span data-stu-id="9772e-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="9772e-401">Versões de hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="9772e-402">Os seguintes versões do [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js versões estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="9772e-403">Hammer.js versão 2.0.4</span><span class="sxs-lookup"><span data-stu-id="9772e-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="9772e-404">Web Forms do ASP.NET e Ajax versões na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="9772e-405">As seguintes versões do ASP.NET Ajax Library são hospedadas na CDN.</span><span class="sxs-lookup"><span data-stu-id="9772e-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="9772e-406">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="9772e-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="9772e-407">Web Forms do ASP.NET e Ajax versão 4.5.2</span><span class="sxs-lookup"><span data-stu-id="9772e-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms do ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="9772e-408">Web Forms do ASP.NET e Ajax versão 4</span><span class="sxs-lookup"><span data-stu-id="9772e-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms do ASP.NET e Ajax 4")
- [<span data-ttu-id="9772e-409">Versão 3.5 do ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="9772e-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="9772e-410">O ASP.NET MVC lança na CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="9772e-411">Os seguintes arquivos JavaScript do ASP.NET MVC são hospedados neste CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="9772e-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="9772e-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="9772e-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="9772e-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="9772e-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="9772e-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="9772e-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="9772e-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="9772e-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="9772e-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="9772e-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="9772e-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="9772e-419">Versões do ASP.NET SignalR em CDN</span><span class="sxs-lookup"><span data-stu-id="9772e-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="9772e-420">Os seguintes arquivos ASP.NET SignalR JavaScript são hospedados neste CDN:</span><span class="sxs-lookup"><span data-stu-id="9772e-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="9772e-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="9772e-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="9772e-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="9772e-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="9772e-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="9772e-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="9772e-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="9772e-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="9772e-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="9772e-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="9772e-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="9772e-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="9772e-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="9772e-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9772e-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="9772e-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="9772e-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="9772e-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="9772e-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="9772e-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="9772e-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="9772e-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="9772e-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="9772e-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="9772e-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="9772e-434">Para obter informações sobre os termos de uso para a CDN, consulte [Microsoft Ajax CDN termos de uso](https://www.asp.net/terms-of-use "Microsoft Ajax CDN termos de uso").</span><span class="sxs-lookup"><span data-stu-id="9772e-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
