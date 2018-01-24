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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="f29d2-102">Rede de fornecimento de conteúdo do Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="f29d2-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="f29d2-103">Observação: A Microsoft Ajax CDN não tem nenhum SLA além de usar um CDN do Azure.</span><span class="sxs-lookup"><span data-stu-id="f29d2-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="f29d2-104">Sumário</span><span class="sxs-lookup"><span data-stu-id="f29d2-104">Table of Contents</span></span>

<span data-ttu-id="f29d2-105">**[AJAX.microsoft.com renomeado para ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="f29d2-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="f29d2-106">**[Suporte de .vsdoc do Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="f29d2-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="f29d2-107">**[Usando o ASP.NET Ajax da CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="f29d2-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="f29d2-108">**[Usando jQuery da CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="f29d2-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="f29d2-109">**[Usando a interface do usuário da CDN do jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="f29d2-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="f29d2-110">**[Arquivos de terceiros em CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="f29d2-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="f29d2-111">Versões do jQuery em CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="f29d2-112">jQuery migrar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="f29d2-113">Versões de interface do usuário na CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="f29d2-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="f29d2-114">jQuery lançamentos de validação no CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="f29d2-115">jQuery Mobile versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="f29d2-116">jQuery versões de modelos na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="f29d2-117">jQuery ciclo versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="f29d2-118">jQuery DataTables versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="f29d2-119">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="f29d2-120">Versões de JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="f29d2-121">Versões de separação na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="f29d2-122">Globalizar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="f29d2-123">Responder versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="f29d2-124">Versões de inicialização na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="f29d2-125">Versões de inicialização TouchCarousel na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="f29d2-126">Versões de hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="f29d2-127">Web Forms do ASP.NET e Ajax versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="f29d2-128">O ASP.NET MVC lança na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="f29d2-129">Versões do ASP.NET SignalR em CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="f29d2-130">A Microsoft Ajax Content Delivery Network (CDN) hospeda bibliotecas JavaScript de terceiros populares, como jQuery e permite que você adicioná-los facilmente para seus aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="f29d2-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="f29d2-131">Por exemplo, começar a usar o jQuery que está hospedado neste CDN simplesmente adicionando um &lt;script&gt; marca para a página que aponta para ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="f29d2-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="f29d2-132">Aproveitando a CDN, você pode melhorar significativamente o desempenho de seus aplicativos Ajax.</span><span class="sxs-lookup"><span data-stu-id="f29d2-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="f29d2-133">O conteúdo da CDN do é armazenados em cache nos servidores localizados em todo o mundo.</span><span class="sxs-lookup"><span data-stu-id="f29d2-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="f29d2-134">Além disso, o CDN permite que navegadores para reutilizar arquivos JavaScript de terceiros em cache para sites da web que estão localizados em domínios diferentes.</span><span class="sxs-lookup"><span data-stu-id="f29d2-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="f29d2-135">A CDN oferece suporte a SSL (HTTPS), caso seja necessário atender a uma página da web usando o protocolo SSL.</span><span class="sxs-lookup"><span data-stu-id="f29d2-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="f29d2-136">A CDN hospeda as seguintes bibliotecas de scripts de terceiros que foram carregadas e estão licenciadas para você, pelos proprietários dessas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="f29d2-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="f29d2-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="f29d2-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="f29d2-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="f29d2-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="f29d2-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="f29d2-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="f29d2-140">jQuery validação (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="f29d2-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="f29d2-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="f29d2-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="f29d2-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="f29d2-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="f29d2-143">A Microsoft Ajax CDN também inclui as seguintes bibliotecas que foram carregadas pela Microsoft:</span><span class="sxs-lookup"><span data-stu-id="f29d2-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="f29d2-144">Ajax ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f29d2-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="f29d2-145">Arquivos de JavaScript do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f29d2-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="f29d2-146">Arquivos do ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="f29d2-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="f29d2-147">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="f29d2-148">Os proprietários de direitos autorais das bibliotecas são licenciamento dessas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="f29d2-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="f29d2-149">Quaisquer direitos que você precisará baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="f29d2-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="f29d2-150">Porque eles não são bibliotecas Microsoft, a Microsoft não fornece nenhuma garantia ou licenças de direitos de propriedade intelectual (incluindo sem direitos de patente implícitos) para as bibliotecas de terceiros hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="f29d2-151">Se você deseja enviar sua biblioteca de JavaScript e a biblioteca é uma das principais bibliotecas JavaScript (conforme listado em http://trends.builtwith.com) ou extensões/plug-ins para essas bibliotecas que são (a) populares; ou (b) úteis para usar em ASP.NET e em seguida, entre em contato com AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="f29d2-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="f29d2-152">AJAX.microsoft.com renomeado para ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="f29d2-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="f29d2-153">A CDN usado para usar o nome de domínio microsoft.com e foi alterada para usar o nome de domínio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="f29d2-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="f29d2-154">Essa alteração foi feita para aumentar o desempenho porque quando um navegador referenciada domínio microsoft.com poderia enviar os cookies do domínio pela conexão com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="f29d2-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="f29d2-155">Renomeando com um nome de domínio diferente microsoft.com desempenho pode ser aumentado pelo máximo de 25%.</span><span class="sxs-lookup"><span data-stu-id="f29d2-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="f29d2-156">Observação ajax.microsoft.com continuarão a funcionar, mas ajax.aspnetcdn.com é recomendado.</span><span class="sxs-lookup"><span data-stu-id="f29d2-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="f29d2-157">Formato antigo: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="f29d2-158">Novo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="f29d2-159">Suporte de .vsdoc do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f29d2-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="f29d2-160">Para usar os arquivos de .vsdoc corretamente com o Visual Studio 2008, você precisará certificar-se de que você tenha VS 2008 SP1 e o hotfix para vsdoc arquivos instalados.</span><span class="sxs-lookup"><span data-stu-id="f29d2-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="f29d2-161">Você pode obter essas aqui:</span><span class="sxs-lookup"><span data-stu-id="f29d2-161">You can get these from here:</span></span>

- [<span data-ttu-id="f29d2-162">Baixe o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="f29d2-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "baixar o Visual Studio 2008 SP1")
- [<span data-ttu-id="f29d2-163">Baixar o hotfix .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="f29d2-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "baixar o hotfix .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="f29d2-164">Visual Studio 2010 dá suporte a arquivos .vsdoc sem qualquer patches adicionais.</span><span class="sxs-lookup"><span data-stu-id="f29d2-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="f29d2-165">Usando o ASP.NET Ajax da CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="f29d2-166">Ao usar o ASP.NET 4, você pode redirecionar todas as solicitações de scripts de estrutura do ASP.NET para a CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="f29d2-167">Recuperar scripts da CDN, em vez de seu servidor web local substancialmente pode melhorar o desempenho de sites da Web ASP.NET pública.</span><span class="sxs-lookup"><span data-stu-id="f29d2-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="f29d2-168">Use a propriedade ScriptManager EnableCDN para redirecionar todas as solicitações de script do ASP.NET framework para a Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="f29d2-169">Usando jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="f29d2-170">Você pode usar scripts de jQuery hospedados na CDN no seu aplicativo Web, adicionando o seguinte elemento de script para uma página:</span><span class="sxs-lookup"><span data-stu-id="f29d2-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="f29d2-171">A CDN também inclui a versão minimizada do script jQuery, que você pode obter usando o seguinte elemento:</span><span class="sxs-lookup"><span data-stu-id="f29d2-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="f29d2-172">Para permitir que a página de fallback para carregar jQuery de um caminho local no seu próprio site caso o CDN não esteja disponível, adicione o seguinte elemento imediatamente depois do elemento referenciando o CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="f29d2-173">A página de exemplo a seguir usa a versão CDN da biblioteca do jQuery (com um fallback para uma cópia local) para exibir o conteúdo de um elemento div quando um botão é clicado.</span><span class="sxs-lookup"><span data-stu-id="f29d2-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="f29d2-174">Você pode saber mais sobre jQuery e baixar uma cópia local do jQuery visitando o [jQuery](http://jquery.com/) site da Web.</span><span class="sxs-lookup"><span data-stu-id="f29d2-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="f29d2-175">Usando a interface do usuário da CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="f29d2-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="f29d2-176">A CDN também hospeda a biblioteca de interface do usuário da jQuery.</span><span class="sxs-lookup"><span data-stu-id="f29d2-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="f29d2-177">A biblioteca de interface do usuário da jQuery inclui um conjunto avançado de widgets e efeitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f29d2-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="f29d2-178">Por exemplo, a página a seguir ilustra como você pode usar o jQuery UI Datepicker no contexto de um aplicativo de Web Forms do ASP.NET para exibir um calendário pop-up:</span><span class="sxs-lookup"><span data-stu-id="f29d2-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="f29d2-179">Quando você move o foco para a caixa de texto usando o teclado, será exibido um calendário:</span><span class="sxs-lookup"><span data-stu-id="f29d2-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendário pop-up criado com Datepicker](overview/_static/image1.png)

<span data-ttu-id="f29d2-181">Observe que você deve incluir três arquivos da CDN no código acima:</span><span class="sxs-lookup"><span data-stu-id="f29d2-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="f29d2-182">A biblioteca jQuery &mdash; a biblioteca de interface do usuário da jQuery depende da biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="f29d2-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="f29d2-183">Você deve adicionar a biblioteca jQuery para sua página antes de adicionar a biblioteca de interface do usuário da jQuery.</span><span class="sxs-lookup"><span data-stu-id="f29d2-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="f29d2-184">A biblioteca de interface do usuário da jQuery &mdash; a biblioteca de interface do usuário do jQuery contém todos os efeitos de interface do usuário da jQuery e widgets, como o widget Datepicker usado na página acima.</span><span class="sxs-lookup"><span data-stu-id="f29d2-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="f29d2-185">Um tema de interface do usuário da jQuery &mdash; o jQuery UI dá suporte a temas diferentes.</span><span class="sxs-lookup"><span data-stu-id="f29d2-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="f29d2-186">A página acima inclui um link para um arquivo CSS para importar o tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="f29d2-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="f29d2-187">Todos os temas de interface do usuário da jQuery padrão são hospedados na CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="f29d2-188">[Visite essa página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 no Microsoft Ajax CDN") para visualizar miniaturas de cada tema.</span><span class="sxs-lookup"><span data-stu-id="f29d2-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="f29d2-189">Para saber mais sobre a biblioteca de interface do usuário do jQuery, visite o oficial [site de interface do usuário da jQuery](http://jQueryUI.com "site de interface do usuário da jQuery").</span><span class="sxs-lookup"><span data-stu-id="f29d2-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="f29d2-190">Arquivos de terceiros em CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="f29d2-191">A CDN hospeda algumas das bibliotecas de JavaScript de terceiros mais populares.</span><span class="sxs-lookup"><span data-stu-id="f29d2-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="f29d2-192">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="f29d2-193">Os proprietários de direitos autorais das bibliotecas são licenciamento dessas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="f29d2-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="f29d2-194">Quaisquer direitos que você precisará baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="f29d2-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="f29d2-195">Porque eles não são bibliotecas Microsoft, a Microsoft não fornece nenhuma garantia ou licenças de direitos de propriedade intelectual (incluindo sem direitos de patente implícitos) para as bibliotecas de terceiros hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="f29d2-196">Versões do jQuery em CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="f29d2-197">As seguintes versões do jQuery estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="f29d2-198">versão do jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="f29d2-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="f29d2-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="f29d2-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="f29d2-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="f29d2-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="f29d2-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="f29d2-205">jQuery versão 3.2.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="f29d2-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="f29d2-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="f29d2-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="f29d2-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="f29d2-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="f29d2-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="f29d2-212">versão do jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="f29d2-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="f29d2-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="f29d2-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="f29d2-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="f29d2-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="f29d2-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="f29d2-219">versão do jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="f29d2-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="f29d2-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="f29d2-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="f29d2-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="f29d2-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="f29d2-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="f29d2-226">jQuery versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="f29d2-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="f29d2-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="f29d2-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="f29d2-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="f29d2-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="f29d2-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="f29d2-233">versão do jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="f29d2-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="f29d2-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="f29d2-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="f29d2-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="f29d2-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="f29d2-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="f29d2-240">versão do jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="f29d2-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="f29d2-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="f29d2-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="f29d2-244">versão do jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="f29d2-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="f29d2-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="f29d2-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="f29d2-248">versão 2.2.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="f29d2-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="f29d2-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="f29d2-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="f29d2-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="f29d2-252">jQuery versão 2.2.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="f29d2-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="f29d2-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="f29d2-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="f29d2-256">versão do jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="f29d2-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="f29d2-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="f29d2-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="f29d2-260">versão do jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="f29d2-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="f29d2-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="f29d2-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="f29d2-264">versão do jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="f29d2-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="f29d2-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="f29d2-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="f29d2-268">versão do jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="f29d2-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="f29d2-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="f29d2-271">versão do jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="f29d2-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="f29d2-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="f29d2-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="f29d2-275">versão do jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="f29d2-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="f29d2-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="f29d2-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="f29d2-280">versão do jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="f29d2-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="f29d2-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="f29d2-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="f29d2-285">versão do jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="f29d2-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="f29d2-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="f29d2-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="f29d2-290">jQuery versão 2.0.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="f29d2-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="f29d2-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="f29d2-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="f29d2-295">jQuery versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="f29d2-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="f29d2-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="f29d2-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="f29d2-300">versão do jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="f29d2-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="f29d2-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="f29d2-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="f29d2-304">versão do jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="f29d2-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="f29d2-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="f29d2-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="f29d2-308">versão do jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="f29d2-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="f29d2-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="f29d2-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="f29d2-312">versão do jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="f29d2-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="f29d2-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="f29d2-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="f29d2-316">versão do jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="f29d2-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="f29d2-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="f29d2-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="f29d2-320">versão do jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="f29d2-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="f29d2-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="f29d2-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="f29d2-324">versão do jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="f29d2-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="f29d2-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="f29d2-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="f29d2-328">versão do jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="f29d2-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="f29d2-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="f29d2-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="f29d2-332">versão do jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="f29d2-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="f29d2-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="f29d2-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="f29d2-337">versão do jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="f29d2-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="f29d2-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="f29d2-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="f29d2-342">versão do jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="f29d2-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="f29d2-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="f29d2-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="f29d2-347">versão do jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="f29d2-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="f29d2-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="f29d2-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="f29d2-352">versão do jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="f29d2-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="f29d2-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="f29d2-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="f29d2-357">versão do jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="f29d2-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="f29d2-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="f29d2-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="f29d2-362">versão do jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="f29d2-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="f29d2-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="f29d2-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="f29d2-366">versão do jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="f29d2-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="f29d2-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="f29d2-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="f29d2-370">jQuery versão 1.8.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="f29d2-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="f29d2-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="f29d2-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="f29d2-374">versão do jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="f29d2-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="f29d2-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="f29d2-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="f29d2-378">versão do jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="f29d2-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="f29d2-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="f29d2-381">versão do jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="f29d2-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="f29d2-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="f29d2-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="f29d2-385">jQuery versão 1.7</span><span class="sxs-lookup"><span data-stu-id="f29d2-385">jQuery version 1.7</span></span>

- <span data-ttu-id="f29d2-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="f29d2-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="f29d2-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="f29d2-389">versão do jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="f29d2-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="f29d2-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="f29d2-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="f29d2-393">versão do jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="f29d2-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="f29d2-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="f29d2-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="f29d2-397">versão do jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="f29d2-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="f29d2-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="f29d2-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="f29d2-401">versão do jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="f29d2-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="f29d2-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="f29d2-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="f29d2-405">jQuery versão 1.6</span><span class="sxs-lookup"><span data-stu-id="f29d2-405">jQuery version 1.6</span></span>

- <span data-ttu-id="f29d2-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="f29d2-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="f29d2-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="f29d2-409">versão do jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="f29d2-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="f29d2-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="f29d2-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="f29d2-413">versão do jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="f29d2-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="f29d2-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="f29d2-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="f29d2-417">versão do jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="f29d2-417">jQuery version 1.5</span></span>

- <span data-ttu-id="f29d2-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="f29d2-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="f29d2-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="f29d2-421">versão do jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="f29d2-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="f29d2-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="f29d2-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="f29d2-425">versão do jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="f29d2-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="f29d2-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="f29d2-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="f29d2-429">versão do jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="f29d2-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="f29d2-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="f29d2-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="f29d2-433">versão do jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="f29d2-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="f29d2-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="f29d2-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="f29d2-437">jQuery versão 1.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-437">jQuery version 1.4</span></span>

- <span data-ttu-id="f29d2-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="f29d2-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="f29d2-440">versão do jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="f29d2-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="f29d2-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="f29d2-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="f29d2-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.Min-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="f29d2-445">jQuery migrar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="f29d2-446">As seguintes versões do jQuery migrar estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="f29d2-447">jQuery migrar versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="f29d2-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="f29d2-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="f29d2-450">jQuery migrar versão 1.2.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="f29d2-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="f29d2-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="f29d2-453">jQuery migrar versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="f29d2-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="f29d2-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="f29d2-456">jQuery migrar versão 1.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="f29d2-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="f29d2-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="f29d2-459">jQuery migrar versão 1.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="f29d2-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="f29d2-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="f29d2-462">Migrar a versão 1.0.0 do jQuery</span><span class="sxs-lookup"><span data-stu-id="f29d2-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="f29d2-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="f29d2-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="f29d2-465">Versões de interface do usuário na CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="f29d2-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="f29d2-466">As seguintes versões da biblioteca de interface do usuário do jQuery são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="f29d2-467">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interface do usuário no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="f29d2-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="f29d2-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="f29d2-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="f29d2-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="f29d2-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="f29d2-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="f29d2-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="f29d2-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="f29d2-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="f29d2-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="f29d2-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="f29d2-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="f29d2-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="f29d2-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="f29d2-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="f29d2-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="f29d2-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="f29d2-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="f29d2-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="f29d2-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="f29d2-503">jQuery lançamentos de validação no CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="f29d2-504">As seguintes versões da biblioteca de validação jQuery são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="f29d2-505">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-506">jQuery validar 1.17.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery validação 1.17.0")
- [<span data-ttu-id="f29d2-507">jQuery validar 1.16.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery validação 1.16.0")
- [<span data-ttu-id="f29d2-508">jQuery validar 1.15.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery validação 1.15.1")
- [<span data-ttu-id="f29d2-509">jQuery validar 1.15.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery validação 1.15.0")
- [<span data-ttu-id="f29d2-510">jQuery validar 1.14.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery validação 1.14.0")
- [<span data-ttu-id="f29d2-511">jQuery validar 1.13.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery validação 1.13.1")
- [<span data-ttu-id="f29d2-512">jQuery validar 1.13.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery validação 1.13.0")
- [<span data-ttu-id="f29d2-513">jQuery validar 1.12.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery validação 1.12.0")
- [<span data-ttu-id="f29d2-514">jQuery validar 1.11.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery validação 1.11.1")
- [<span data-ttu-id="f29d2-515">jQuery validar 1.11.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery validação 1.11.0")
- [<span data-ttu-id="f29d2-516">jQuery validar 1.10.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery validação 1.10.0")
- [<span data-ttu-id="f29d2-517">jQuery validar 1.9</span><span class="sxs-lookup"><span data-stu-id="f29d2-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versão 1.9")
- [<span data-ttu-id="f29d2-518">jQuery validar 1.8.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versão 1.8.1")
- [<span data-ttu-id="f29d2-519">jQuery validar 1.8</span><span class="sxs-lookup"><span data-stu-id="f29d2-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versão 1.8")
- [<span data-ttu-id="f29d2-520">jQuery validar 1.7</span><span class="sxs-lookup"><span data-stu-id="f29d2-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versão 1.7")
- [<span data-ttu-id="f29d2-521">jQuery validar 1.6</span><span class="sxs-lookup"><span data-stu-id="f29d2-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validar 1.6")
- [<span data-ttu-id="f29d2-522">jQuery validar 1.5.5</span><span class="sxs-lookup"><span data-stu-id="f29d2-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validar 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="f29d2-523">jQuery Mobile versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="f29d2-524">As seguintes versões da biblioteca jQuery Mobile são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="f29d2-525">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-526">o jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="f29d2-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-527">o jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-528">o jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-529">o jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-530">o jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-531">o jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-532">o jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-533">o jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-534">o jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-535">o jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-536">o jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-537">o jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="f29d2-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-538">o jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-539">o jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-540">o jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="f29d2-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-541">o jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="f29d2-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 no Microsoft Ajax CDN")
- [<span data-ttu-id="f29d2-542">versão beta do jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="f29d2-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 no Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="f29d2-543">jQuery versões de modelos na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="f29d2-544">As seguintes versões do plug-in do jQuery modelos são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="f29d2-545">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-546">jQuery modelos Beta 1</span><span class="sxs-lookup"><span data-stu-id="f29d2-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modelos Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="f29d2-547">jQuery ciclo versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="f29d2-548">As seguintes versões do plug-in do jQuery ciclo são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="f29d2-549">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-550">jQuery ciclo 2.99</span><span class="sxs-lookup"><span data-stu-id="f29d2-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 ciclo")
- [<span data-ttu-id="f29d2-551">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="f29d2-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [<span data-ttu-id="f29d2-552">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="f29d2-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="f29d2-553">jQuery DataTables versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="f29d2-554">As seguintes versões do plug-in jQuery DataTables são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="f29d2-555">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="f29d2-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="f29d2-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="f29d2-558">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="f29d2-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="f29d2-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="f29d2-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="f29d2-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="f29d2-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="f29d2-564">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="f29d2-565">Os seguintes versões do [Modernizr](http://www.modernizr.com "Modernizr") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="f29d2-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="f29d2-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="f29d2-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="f29d2-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="f29d2-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="f29d2-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="f29d2-572">Versões de JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="f29d2-573">Os seguintes versões do [JSHint](http://www.jshint.com "JSHint") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="f29d2-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="f29d2-575">Versões de separação na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="f29d2-576">Os seguintes versões do [Knockout](http://www.knockoutjs.com "Knockout") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="f29d2-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="f29d2-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="f29d2-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="f29d2-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="f29d2-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="f29d2-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="f29d2-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="f29d2-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="f29d2-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="f29d2-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="f29d2-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="f29d2-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="f29d2-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="f29d2-590">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="f29d2-591">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="f29d2-592">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="f29d2-593">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="f29d2-594">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="f29d2-595">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="f29d2-596">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="f29d2-597">Globalizar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="f29d2-598">Os seguintes versões do [Globalize](https://github.com/jquery/globalize "Globalize") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="f29d2-599">A versão 1.0.0 de globalizar</span><span class="sxs-lookup"><span data-stu-id="f29d2-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="f29d2-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="f29d2-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="f29d2-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="f29d2-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="f29d2-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="f29d2-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="f29d2-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="f29d2-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-time.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="f29d2-608">Globalizar versão 0.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="f29d2-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="f29d2-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="f29d2-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="f29d2-612">todas as culturas</span><span class="sxs-lookup"><span data-stu-id="f29d2-612">all cultures</span></span>
- <span data-ttu-id="f29d2-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.Culture. . js de {código de cultura}</span><span class="sxs-lookup"><span data-stu-id="f29d2-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="f29d2-614">Substitua "{-código de cultura}" com o código de cultura desejado, por exemplo, a Microsoft globalize.culture.en GB.js== arquivos em CDN = = essas bibliotecas foram carregadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f29d2-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="f29d2-615">Responder versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="f29d2-616">Os seguintes versões do [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") responder estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="f29d2-617">Responder a versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="f29d2-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="f29d2-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="f29d2-620">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="f29d2-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="f29d2-622">Responder a versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="f29d2-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="f29d2-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="f29d2-625">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="f29d2-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="f29d2-627">Responder a versão 1.4.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="f29d2-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="f29d2-629">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="f29d2-630">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="f29d2-631">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="f29d2-632">Responder a versão 1.3.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="f29d2-633">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="f29d2-634">Responder a versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="f29d2-635">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="f29d2-636">Versões de inicialização na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="f29d2-637">Os seguintes versões do [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="f29d2-638">Versão bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="f29d2-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="f29d2-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="f29d2-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="f29d2-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="f29d2-652">Versão bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="f29d2-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="f29d2-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="f29d2-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="f29d2-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="f29d2-666">Inicialização versão 3.3.5</span><span class="sxs-lookup"><span data-stu-id="f29d2-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="f29d2-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="f29d2-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="f29d2-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="f29d2-680">Versão bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="f29d2-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="f29d2-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="f29d2-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="f29d2-694">Versão bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="f29d2-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="f29d2-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="f29d2-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="f29d2-708">Versão bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="f29d2-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="f29d2-721">Versão bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="f29d2-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="f29d2-734">Versão bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="f29d2-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="f29d2-747">Versão bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="f29d2-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="f29d2-760">Inicialização versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="f29d2-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="f29d2-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="f29d2-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="f29d2-773">Versão bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="f29d2-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="f29d2-784">Versão bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="f29d2-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="f29d2-795">Versão bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="f29d2-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="f29d2-806">Versão bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="f29d2-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="f29d2-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="f29d2-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="f29d2-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="f29d2-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="f29d2-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="f29d2-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="f29d2-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="f29d2-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="f29d2-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="f29d2-817">Inicialização versão 2.3.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="f29d2-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="f29d2-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="f29d2-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="f29d2-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="f29d2-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="f29d2-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="f29d2-826">Versão bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="f29d2-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="f29d2-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="f29d2-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="f29d2-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="f29d2-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="f29d2-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="f29d2-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="f29d2-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="f29d2-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="f29d2-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="f29d2-835">Versões de inicialização TouchCarousel na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="f29d2-836">Os seguintes versões do [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel Bootstrap versões são hospedados em CDN :</span><span class="sxs-lookup"><span data-stu-id="f29d2-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="f29d2-837">Inicialização TouchCarousel versão 0.8.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="f29d2-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="f29d2-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="f29d2-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="f29d2-840">Versões de hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="f29d2-841">Os seguintes versões do [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versões estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="f29d2-842">Hammer.js versão 2.0.4</span><span class="sxs-lookup"><span data-stu-id="f29d2-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="f29d2-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="f29d2-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="f29d2-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="f29d2-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="f29d2-846">Web Forms do ASP.NET e Ajax versões na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="f29d2-847">As seguintes versões do ASP.NET Ajax Library são hospedadas na CDN.</span><span class="sxs-lookup"><span data-stu-id="f29d2-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="f29d2-848">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="f29d2-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="f29d2-849">Web Forms do ASP.NET e Ajax versão 4.5.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms do ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="f29d2-850">Web Forms do ASP.NET e Ajax versão 4</span><span class="sxs-lookup"><span data-stu-id="f29d2-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms do ASP.NET e Ajax 4")
- [<span data-ttu-id="f29d2-851">Versão 3.5 do ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="f29d2-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="f29d2-852">O ASP.NET MVC lança na CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="f29d2-853">Os seguintes arquivos JavaScript do ASP.NET MVC são hospedados neste CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="f29d2-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="f29d2-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="f29d2-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="f29d2-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="f29d2-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="f29d2-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="f29d2-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="f29d2-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="f29d2-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="f29d2-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="f29d2-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="f29d2-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="f29d2-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="f29d2-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="f29d2-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="f29d2-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="f29d2-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="f29d2-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="f29d2-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="f29d2-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="f29d2-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="f29d2-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="f29d2-876">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="f29d2-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="f29d2-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="f29d2-879">Versões do ASP.NET SignalR em CDN</span><span class="sxs-lookup"><span data-stu-id="f29d2-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="f29d2-880">Os seguintes arquivos ASP.NET SignalR JavaScript são hospedados neste CDN:</span><span class="sxs-lookup"><span data-stu-id="f29d2-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="f29d2-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="f29d2-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="f29d2-883">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="f29d2-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="f29d2-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="f29d2-886">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="f29d2-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="f29d2-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="f29d2-889">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="f29d2-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="f29d2-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="f29d2-892">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="f29d2-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="f29d2-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="f29d2-895">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="f29d2-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="f29d2-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="f29d2-898">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="f29d2-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="f29d2-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="f29d2-901">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="f29d2-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="f29d2-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="f29d2-904">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="f29d2-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="f29d2-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="f29d2-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="f29d2-907">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="f29d2-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="f29d2-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="f29d2-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="f29d2-910">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="f29d2-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="f29d2-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="f29d2-913">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="f29d2-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="f29d2-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="f29d2-915">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="f29d2-916">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="f29d2-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="f29d2-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="f29d2-918">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="f29d2-919">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="f29d2-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="f29d2-920">Para obter informações sobre os termos de uso para a CDN, consulte [Microsoft Ajax CDN termos de uso](https://www.asp.net/terms-of-use "Microsoft Ajax CDN termos de uso").</span><span class="sxs-lookup"><span data-stu-id="f29d2-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
