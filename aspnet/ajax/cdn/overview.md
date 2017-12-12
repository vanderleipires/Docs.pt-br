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
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="ec1a0-102">Rede de fornecimento de conteúdo do Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="ec1a0-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="ec1a0-103">Observação: A Microsoft Ajax CDN não tem nenhum SLA além de usar um CDN do Azure.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="ec1a0-104">Sumário</span><span class="sxs-lookup"><span data-stu-id="ec1a0-104">Table of Contents</span></span>

<span data-ttu-id="ec1a0-105">**[AJAX.microsoft.com renomeado para ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="ec1a0-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="ec1a0-106">**[Suporte de .vsdoc do Visual Studio](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="ec1a0-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="ec1a0-107">**[Usando o ASP.NET Ajax da CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="ec1a0-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="ec1a0-108">**[Usando jQuery da CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="ec1a0-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="ec1a0-109">**[Usando a interface do usuário da CDN do jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="ec1a0-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="ec1a0-110">**[Arquivos de terceiros em CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="ec1a0-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="ec1a0-111">Versões do jQuery em CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="ec1a0-112">jQuery migrar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="ec1a0-113">Versões de interface do usuário na CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="ec1a0-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="ec1a0-114">jQuery lançamentos de validação no CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="ec1a0-115">jQuery Mobile versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="ec1a0-116">jQuery versões de modelos na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="ec1a0-117">jQuery ciclo versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="ec1a0-118">jQuery DataTables versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="ec1a0-119">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="ec1a0-120">Versões de JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="ec1a0-121">Versões de separação na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="ec1a0-122">Globalizar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="ec1a0-123">Responder versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="ec1a0-124">Versões de inicialização na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="ec1a0-125">Versões de inicialização TouchCarousel na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="ec1a0-126">Versões de hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="ec1a0-127">Web Forms do ASP.NET e Ajax versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="ec1a0-128">O ASP.NET MVC lança na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="ec1a0-129">Versões do ASP.NET SignalR em CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="ec1a0-130">A Microsoft Ajax Content Delivery Network (CDN) hospeda bibliotecas JavaScript de terceiros populares, como jQuery e permite que você adicioná-los facilmente para seus aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="ec1a0-131">Por exemplo, começar a usar o jQuery que está hospedado neste CDN simplesmente adicionando um &lt;script&gt; marca para a página que aponta para ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="ec1a0-132">Aproveitando a CDN, você pode melhorar significativamente o desempenho de seus aplicativos Ajax.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="ec1a0-133">O conteúdo da CDN do é armazenados em cache nos servidores localizados em todo o mundo.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="ec1a0-134">Além disso, o CDN permite que navegadores para reutilizar arquivos JavaScript de terceiros em cache para sites da web que estão localizados em domínios diferentes.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="ec1a0-135">A CDN oferece suporte a SSL (HTTPS), caso seja necessário atender a uma página da web usando o protocolo SSL.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="ec1a0-136">A CDN hospeda as seguintes bibliotecas de scripts de terceiros que foram carregadas e estão licenciadas para você, pelos proprietários dessas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="ec1a0-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="ec1a0-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="ec1a0-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="ec1a0-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="ec1a0-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="ec1a0-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="ec1a0-140">jQuery validação (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="ec1a0-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="ec1a0-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="ec1a0-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="ec1a0-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="ec1a0-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="ec1a0-143">A Microsoft Ajax CDN também inclui as seguintes bibliotecas que foram carregadas pela Microsoft:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="ec1a0-144">Ajax ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ec1a0-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="ec1a0-145">Arquivos de JavaScript do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ec1a0-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="ec1a0-146">Arquivos do ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="ec1a0-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="ec1a0-147">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-148">Os proprietários de direitos autorais das bibliotecas são licenciamento dessas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="ec1a0-149">Quaisquer direitos que você precisará baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="ec1a0-150">Porque eles não são bibliotecas Microsoft, a Microsoft não fornece nenhuma garantia ou licenças de direitos de propriedade intelectual (incluindo sem direitos de patente implícitos) para as bibliotecas de terceiros hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="ec1a0-151">Se você deseja enviar sua biblioteca de JavaScript e a biblioteca é uma das principais bibliotecas JavaScript (conforme listado em http://trends.builtwith.com) ou extensões/plug-ins para essas bibliotecas que são (a) populares; ou (b) úteis para usar em ASP.NET e em seguida, entre em contato com AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="ec1a0-152">AJAX.microsoft.com renomeado para ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="ec1a0-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="ec1a0-153">A CDN usado para usar o nome de domínio microsoft.com e foi alterada para usar o nome de domínio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="ec1a0-154">Essa alteração foi feita para aumentar o desempenho porque quando um navegador referenciada domínio microsoft.com poderia enviar os cookies do domínio pela conexão com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="ec1a0-155">Renomeando com um nome de domínio diferente microsoft.com desempenho pode ser aumentado pelo máximo de 25%.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="ec1a0-156">Observação ajax.microsoft.com continuarão a funcionar, mas ajax.aspnetcdn.com é recomendado.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="ec1a0-157">Formato antigo: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="ec1a0-158">Novo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="ec1a0-159">Suporte de .vsdoc do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec1a0-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="ec1a0-160">Para usar os arquivos de .vsdoc corretamente com o Visual Studio 2008, você precisará certificar-se de que você tenha VS 2008 SP1 e o hotfix para vsdoc arquivos instalados.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="ec1a0-161">Você pode obter essas aqui:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-161">You can get these from here:</span></span>

- [<span data-ttu-id="ec1a0-162">Baixe o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "baixar o Visual Studio 2008 SP1")
- [<span data-ttu-id="ec1a0-163">Baixar o hotfix .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "baixar o hotfix .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="ec1a0-164">Visual Studio 2010 dá suporte a arquivos .vsdoc sem qualquer patches adicionais.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="ec1a0-165">Usando o ASP.NET Ajax da CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="ec1a0-166">Ao usar o ASP.NET 4, você pode redirecionar todas as solicitações de scripts de estrutura do ASP.NET para a CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="ec1a0-167">Recuperar scripts da CDN, em vez de seu servidor web local substancialmente pode melhorar o desempenho de sites da Web ASP.NET pública.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="ec1a0-168">Use a propriedade ScriptManager EnableCDN para redirecionar todas as solicitações de script do ASP.NET framework para a Microsoft Ajax CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="ec1a0-169">Usando jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="ec1a0-170">Você pode usar scripts de jQuery hospedados na CDN no seu aplicativo Web, adicionando o seguinte elemento de script para uma página:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="ec1a0-171">A CDN também inclui a versão minimizada do script jQuery, que você pode obter usando o seguinte elemento:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="ec1a0-172">Para permitir que a página de fallback para carregar jQuery de um caminho local no seu próprio site caso o CDN não esteja disponível, adicione o seguinte elemento imediatamente depois do elemento referenciando o CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="ec1a0-173">A página de exemplo a seguir usa a versão CDN da biblioteca do jQuery (com um fallback para uma cópia local) para exibir o conteúdo de um elemento div quando um botão é clicado.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="ec1a0-174">Você pode saber mais sobre jQuery e baixar uma cópia local do jQuery visitando o [jQuery](http://jquery.com/) site da Web.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="ec1a0-175">Usando a interface do usuário da CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="ec1a0-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="ec1a0-176">A CDN também hospeda a biblioteca de interface do usuário da jQuery.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="ec1a0-177">A biblioteca de interface do usuário da jQuery inclui um conjunto avançado de widgets e efeitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="ec1a0-178">Por exemplo, a página a seguir ilustra como você pode usar o jQuery UI Datepicker no contexto de um aplicativo de Web Forms do ASP.NET para exibir um calendário pop-up:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="ec1a0-179">Quando você move o foco para a caixa de texto usando o teclado, será exibido um calendário:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendário pop-up criado com Datepicker](overview/_static/image1.png)

<span data-ttu-id="ec1a0-181">Observe que você deve incluir três arquivos da CDN no código acima:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="ec1a0-182">A biblioteca jQuery &mdash; a biblioteca de interface do usuário da jQuery depende da biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="ec1a0-183">Você deve adicionar a biblioteca jQuery para sua página antes de adicionar a biblioteca de interface do usuário da jQuery.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="ec1a0-184">A biblioteca de interface do usuário da jQuery &mdash; a biblioteca de interface do usuário do jQuery contém todos os efeitos de interface do usuário da jQuery e widgets, como o widget Datepicker usado na página acima.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="ec1a0-185">Um tema de interface do usuário da jQuery &mdash; o jQuery UI dá suporte a temas diferentes.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="ec1a0-186">A página acima inclui um link para um arquivo CSS para importar o tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="ec1a0-187">Todos os temas de interface do usuário da jQuery padrão são hospedados na CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="ec1a0-188">[Visite essa página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 no Microsoft Ajax CDN") para visualizar miniaturas de cada tema.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="ec1a0-189">Para saber mais sobre a biblioteca de interface do usuário do jQuery, visite o oficial [site de interface do usuário da jQuery](http://jQueryUI.com "site de interface do usuário da jQuery").</span><span class="sxs-lookup"><span data-stu-id="ec1a0-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="ec1a0-190">Arquivos de terceiros em CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="ec1a0-191">A CDN hospeda algumas das bibliotecas de JavaScript de terceiros mais populares.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="ec1a0-192">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-193">Os proprietários de direitos autorais das bibliotecas são licenciamento dessas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="ec1a0-194">Quaisquer direitos que você precisará baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="ec1a0-195">Porque eles não são bibliotecas Microsoft, a Microsoft não fornece nenhuma garantia ou licenças de direitos de propriedade intelectual (incluindo sem direitos de patente implícitos) para as bibliotecas de terceiros hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-196">Versões do jQuery em CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-197">As seguintes versões do jQuery estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="ec1a0-198">jQuery versão 3.2.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="ec1a0-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="ec1a0-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="ec1a0-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="ec1a0-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="ec1a0-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="ec1a0-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="ec1a0-205">versão do jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="ec1a0-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="ec1a0-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="ec1a0-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="ec1a0-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="ec1a0-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="ec1a0-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="ec1a0-212">versão do jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="ec1a0-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="ec1a0-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="ec1a0-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="ec1a0-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="ec1a0-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="ec1a0-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="ec1a0-219">jQuery versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="ec1a0-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="ec1a0-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="ec1a0-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="ec1a0-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="ec1a0-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="ec1a0-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="ec1a0-226">versão do jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="ec1a0-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="ec1a0-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="ec1a0-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="ec1a0-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="ec1a0-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="ec1a0-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="ec1a0-233">versão do jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="ec1a0-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="ec1a0-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="ec1a0-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="ec1a0-237">versão do jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="ec1a0-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="ec1a0-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="ec1a0-240">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="ec1a0-241">versão 2.2.2 jQuery</span><span class="sxs-lookup"><span data-stu-id="ec1a0-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="ec1a0-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="ec1a0-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="ec1a0-244">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="ec1a0-245">jQuery versão 2.2.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="ec1a0-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="ec1a0-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="ec1a0-248">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="ec1a0-249">versão do jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="ec1a0-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="ec1a0-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="ec1a0-252">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="ec1a0-253">versão do jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="ec1a0-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="ec1a0-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="ec1a0-256">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="ec1a0-257">versão do jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="ec1a0-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="ec1a0-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="ec1a0-260">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="ec1a0-261">versão do jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="ec1a0-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="ec1a0-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="ec1a0-264">versão do jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="ec1a0-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="ec1a0-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="ec1a0-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="ec1a0-268">versão do jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="ec1a0-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="ec1a0-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="ec1a0-271">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="ec1a0-273">versão do jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="ec1a0-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="ec1a0-275">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="ec1a0-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="ec1a0-278">versão do jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="ec1a0-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="ec1a0-280">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="ec1a0-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="ec1a0-283">jQuery versão 2.0.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="ec1a0-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="ec1a0-285">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="ec1a0-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="ec1a0-288">jQuery versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="ec1a0-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="ec1a0-290">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="ec1a0-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="ec1a0-293">versão do jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="ec1a0-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="ec1a0-295">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="ec1a0-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="ec1a0-297">versão do jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="ec1a0-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="ec1a0-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="ec1a0-300">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="ec1a0-301">versão do jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="ec1a0-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="ec1a0-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="ec1a0-304">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="ec1a0-305">versão do jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="ec1a0-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="ec1a0-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="ec1a0-308">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="ec1a0-309">versão do jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="ec1a0-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="ec1a0-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="ec1a0-312">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="ec1a0-313">versão do jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="ec1a0-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="ec1a0-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="ec1a0-316">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="ec1a0-317">versão do jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="ec1a0-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="ec1a0-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="ec1a0-320">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="ec1a0-321">versão do jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="ec1a0-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="ec1a0-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="ec1a0-324">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="ec1a0-325">versão do jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="ec1a0-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="ec1a0-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="ec1a0-328">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="ec1a0-330">versão do jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="ec1a0-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="ec1a0-332">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="ec1a0-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="ec1a0-335">versão do jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="ec1a0-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="ec1a0-337">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="ec1a0-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="ec1a0-340">versão do jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="ec1a0-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="ec1a0-342">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="ec1a0-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="ec1a0-345">versão do jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="ec1a0-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="ec1a0-347">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="ec1a0-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="ec1a0-350">versão do jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="ec1a0-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="ec1a0-352">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="ec1a0-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="ec1a0-355">versão do jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="ec1a0-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="ec1a0-357">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="ec1a0-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="ec1a0-359">versão do jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="ec1a0-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="ec1a0-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="ec1a0-362">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="ec1a0-363">jQuery versão 1.8.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="ec1a0-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="ec1a0-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="ec1a0-366">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="ec1a0-367">versão do jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="ec1a0-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="ec1a0-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="ec1a0-370">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="ec1a0-371">versão do jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="ec1a0-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="ec1a0-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="ec1a0-374">versão do jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="ec1a0-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="ec1a0-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="ec1a0-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="ec1a0-378">jQuery versão 1.7</span><span class="sxs-lookup"><span data-stu-id="ec1a0-378">jQuery version 1.7</span></span>

- <span data-ttu-id="ec1a0-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="ec1a0-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="ec1a0-381">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="ec1a0-382">versão do jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="ec1a0-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="ec1a0-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="ec1a0-385">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="ec1a0-386">versão do jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="ec1a0-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="ec1a0-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="ec1a0-389">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="ec1a0-390">versão do jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="ec1a0-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="ec1a0-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="ec1a0-393">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="ec1a0-394">versão do jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="ec1a0-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="ec1a0-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="ec1a0-397">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="ec1a0-398">jQuery versão 1.6</span><span class="sxs-lookup"><span data-stu-id="ec1a0-398">jQuery version 1.6</span></span>

- <span data-ttu-id="ec1a0-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="ec1a0-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="ec1a0-401">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="ec1a0-402">versão do jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="ec1a0-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="ec1a0-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="ec1a0-405">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="ec1a0-406">versão do jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="ec1a0-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="ec1a0-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="ec1a0-409">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="ec1a0-410">versão do jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="ec1a0-410">jQuery version 1.5</span></span>

- <span data-ttu-id="ec1a0-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="ec1a0-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="ec1a0-413">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="ec1a0-414">versão do jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="ec1a0-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="ec1a0-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="ec1a0-417">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="ec1a0-418">versão do jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="ec1a0-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="ec1a0-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="ec1a0-421">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="ec1a0-422">versão do jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="ec1a0-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="ec1a0-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="ec1a0-425">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="ec1a0-426">versão do jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="ec1a0-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="ec1a0-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="ec1a0-429">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="ec1a0-430">jQuery versão 1.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-430">jQuery version 1.4</span></span>

- <span data-ttu-id="ec1a0-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="ec1a0-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="ec1a0-433">versão do jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="ec1a0-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="ec1a0-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="ec1a0-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="ec1a0-437">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.Min-VSDoc.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-438">jQuery migrar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-439">As seguintes versões do jQuery migrar estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="ec1a0-440">jQuery migrar versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="ec1a0-441">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="ec1a0-442">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="ec1a0-443">jQuery migrar versão 1.2.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="ec1a0-444">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="ec1a0-445">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="ec1a0-446">jQuery migrar versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="ec1a0-447">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="ec1a0-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="ec1a0-449">jQuery migrar versão 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="ec1a0-450">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="ec1a0-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="ec1a0-452">jQuery migrar versão 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="ec1a0-453">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="ec1a0-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="ec1a0-455">Migrar a versão 1.0.0 do jQuery</span><span class="sxs-lookup"><span data-stu-id="ec1a0-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="ec1a0-456">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="ec1a0-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-458">Versões de interface do usuário na CDN do jQuery</span><span class="sxs-lookup"><span data-stu-id="ec1a0-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-459">As seguintes versões da biblioteca de interface do usuário do jQuery são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-460">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 interface do usuário no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-475">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="ec1a0-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="ec1a0-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="ec1a0-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="ec1a0-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="ec1a0-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="ec1a0-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="ec1a0-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="ec1a0-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="ec1a0-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="ec1a0-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="ec1a0-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="ec1a0-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="ec1a0-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="ec1a0-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="ec1a0-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="ec1a0-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="ec1a0-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="ec1a0-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="ec1a0-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="ec1a0-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-496">jQuery lançamentos de validação no CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-497">As seguintes versões da biblioteca de validação jQuery são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-498">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-499">jQuery validar 1.17.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery validação 1.17.0")
- [<span data-ttu-id="ec1a0-500">jQuery validar 1.16.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery validação 1.16.0")
- [<span data-ttu-id="ec1a0-501">jQuery validar 1.15.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery validação 1.15.1")
- [<span data-ttu-id="ec1a0-502">jQuery validar 1.15.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery validação 1.15.0")
- [<span data-ttu-id="ec1a0-503">jQuery validar 1.14.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery validação 1.14.0")
- [<span data-ttu-id="ec1a0-504">jQuery validar 1.13.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery validação 1.13.1")
- [<span data-ttu-id="ec1a0-505">jQuery validar 1.13.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery validação 1.13.0")
- [<span data-ttu-id="ec1a0-506">jQuery validar 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery validação 1.12.0")
- [<span data-ttu-id="ec1a0-507">jQuery validar 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery validação 1.11.1")
- [<span data-ttu-id="ec1a0-508">jQuery validar 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery validação 1.11.0")
- [<span data-ttu-id="ec1a0-509">jQuery validar 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery validação 1.10.0")
- [<span data-ttu-id="ec1a0-510">jQuery validar 1.9</span><span class="sxs-lookup"><span data-stu-id="ec1a0-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versão 1.9")
- [<span data-ttu-id="ec1a0-511">jQuery validar 1.8.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versão 1.8.1")
- [<span data-ttu-id="ec1a0-512">jQuery validar 1.8</span><span class="sxs-lookup"><span data-stu-id="ec1a0-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versão 1.8")
- [<span data-ttu-id="ec1a0-513">jQuery validar 1.7</span><span class="sxs-lookup"><span data-stu-id="ec1a0-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versão 1.7")
- [<span data-ttu-id="ec1a0-514">jQuery validar 1.6</span><span class="sxs-lookup"><span data-stu-id="ec1a0-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validar 1.6")
- [<span data-ttu-id="ec1a0-515">jQuery validar 1.5.5</span><span class="sxs-lookup"><span data-stu-id="ec1a0-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validar 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-516">jQuery Mobile versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-517">As seguintes versões da biblioteca jQuery Mobile são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-518">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-519">o jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="ec1a0-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-520">o jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-521">o jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-522">o jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-523">o jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-524">o jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-525">o jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-526">o jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-527">o jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-528">o jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-529">o jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-530">o jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-531">o jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-532">o jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-533">o jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-534">o jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 no Microsoft Ajax CDN")
- [<span data-ttu-id="ec1a0-535">versão beta do jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 no Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-536">jQuery versões de modelos na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-537">As seguintes versões do plug-in do jQuery modelos são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-538">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-539">jQuery modelos Beta 1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modelos Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-540">jQuery ciclo versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-541">As seguintes versões do plug-in do jQuery ciclo são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-542">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-543">jQuery ciclo 2.99</span><span class="sxs-lookup"><span data-stu-id="ec1a0-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2.99 ciclo")
- [<span data-ttu-id="ec1a0-544">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="ec1a0-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [<span data-ttu-id="ec1a0-545">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="ec1a0-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-546">jQuery DataTables versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-547">As seguintes versões do plug-in jQuery DataTables são hospedadas neste CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="ec1a0-548">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="ec1a0-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="ec1a0-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="ec1a0-551">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="ec1a0-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="ec1a0-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="ec1a0-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="ec1a0-555">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="ec1a0-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-557">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-558">Os seguintes versões do [Modernizr](http://www.modernizr.com "Modernizr") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="ec1a0-559">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="ec1a0-560">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="ec1a0-561">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="ec1a0-562">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="ec1a0-563">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="ec1a0-564">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-565">Versões de JSHint na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-566">Os seguintes versões do [JSHint](http://www.jshint.com "JSHint") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="ec1a0-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-568">Versões de separação na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-569">Os seguintes versões do [Knockout](http://www.knockoutjs.com "Knockout") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="ec1a0-570">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="ec1a0-571">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="ec1a0-572">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="ec1a0-573">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-574">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="ec1a0-575">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-576">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="ec1a0-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="ec1a0-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="ec1a0-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="ec1a0-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="ec1a0-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="ec1a0-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="ec1a0-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="ec1a0-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="ec1a0-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-590">Globalizar versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-591">Os seguintes versões do [Globalize](https://github.com/jquery/globalize "Globalize") estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="ec1a0-592">A versão 1.0.0 de globalizar</span><span class="sxs-lookup"><span data-stu-id="ec1a0-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="ec1a0-593">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="ec1a0-594">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="ec1a0-595">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="ec1a0-596">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="ec1a0-597">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="ec1a0-598">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="ec1a0-599">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="ec1a0-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-time.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="ec1a0-601">Globalizar versão 0.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="ec1a0-602">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="ec1a0-603">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="ec1a0-604">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="ec1a0-605">todas as culturas</span><span class="sxs-lookup"><span data-stu-id="ec1a0-605">all cultures</span></span>
- <span data-ttu-id="ec1a0-606">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.Culture. . js de {código de cultura}</span><span class="sxs-lookup"><span data-stu-id="ec1a0-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="ec1a0-607">Substitua "{-código de cultura}" com o código de cultura desejado, por exemplo, a Microsoft globalize.culture.en GB.js== arquivos em CDN = = essas bibliotecas foram carregadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-608">Responder versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-609">Os seguintes versões do [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") responder estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="ec1a0-610">Responder a versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="ec1a0-611">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="ec1a0-612">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="ec1a0-613">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="ec1a0-614">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="ec1a0-615">Responder a versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="ec1a0-616">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="ec1a0-617">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="ec1a0-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="ec1a0-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="ec1a0-620">Responder a versão 1.4.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="ec1a0-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="ec1a0-622">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="ec1a0-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="ec1a0-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="ec1a0-625">Responder a versão 1.3.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="ec1a0-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="ec1a0-627">Responder a versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="ec1a0-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-629">Versões de inicialização na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-630">Os seguintes versões do [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="ec1a0-631">Versão bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="ec1a0-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="ec1a0-632">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-633">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-634">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-635">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-636">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-637">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-638">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ec1a0-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="ec1a0-645">Versão bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="ec1a0-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="ec1a0-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-652">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ec1a0-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="ec1a0-659">Inicialização versão 3.3.5</span><span class="sxs-lookup"><span data-stu-id="ec1a0-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="ec1a0-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-666">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ec1a0-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="ec1a0-673">Versão bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="ec1a0-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-680">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ec1a0-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="ec1a0-687">Versão bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="ec1a0-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-694">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ec1a0-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="ec1a0-701">Versão bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="ec1a0-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-708">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="ec1a0-714">Versão bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="ec1a0-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-721">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="ec1a0-727">Versão bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="ec1a0-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-734">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="ec1a0-740">Versão bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="ec1a0-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-747">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="ec1a0-753">Inicialização versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="ec1a0-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ec1a0-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-760">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ec1a0-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="ec1a0-766">Versão bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="ec1a0-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-773">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="ec1a0-777">Versão bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="ec1a0-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-784">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="ec1a0-788">Versão bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="ec1a0-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-795">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="ec1a0-799">Versão bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="ec1a0-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ec1a0-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ec1a0-806">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="ec1a0-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ec1a0-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ec1a0-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ec1a0-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ec1a0-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="ec1a0-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="ec1a0-810">Inicialização versão 2.3.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="ec1a0-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="ec1a0-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="ec1a0-817">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="ec1a0-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="ec1a0-819">Versão bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="ec1a0-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="ec1a0-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ec1a0-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ec1a0-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ec1a0-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="ec1a0-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="ec1a0-826">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="ec1a0-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="ec1a0-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-828">Versões de inicialização TouchCarousel na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-829">Os seguintes versões do [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel Bootstrap versões são hospedados em CDN :</span><span class="sxs-lookup"><span data-stu-id="ec1a0-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="ec1a0-830">Inicialização TouchCarousel versão 0.8.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="ec1a0-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="ec1a0-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="ec1a0-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-833">Versões de hammer.js na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-834">Os seguintes versões do [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versões estão hospedados na CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="ec1a0-835">Hammer.js versão 2.0.4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="ec1a0-836">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="ec1a0-837">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="ec1a0-838">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="ec1a0-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-839">Web Forms do ASP.NET e Ajax versões na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-840">As seguintes versões do ASP.NET Ajax Library são hospedadas na CDN.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="ec1a0-841">Clique em cada link para ver a lista atual de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ec1a0-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ec1a0-842">Web Forms do ASP.NET e Ajax versão 4.5.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms do ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="ec1a0-843">Web Forms do ASP.NET e Ajax versão 4</span><span class="sxs-lookup"><span data-stu-id="ec1a0-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms do ASP.NET e Ajax 4")
- [<span data-ttu-id="ec1a0-844">Versão 3.5 do ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="ec1a0-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-845">O ASP.NET MVC lança na CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-846">Os seguintes arquivos JavaScript do ASP.NET MVC são hospedados neste CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="ec1a0-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="ec1a0-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ec1a0-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="ec1a0-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="ec1a0-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ec1a0-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="ec1a0-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="ec1a0-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ec1a0-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="ec1a0-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="ec1a0-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ec1a0-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="ec1a0-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="ec1a0-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="ec1a0-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="ec1a0-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ec1a0-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="ec1a0-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="ec1a0-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="ec1a0-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="ec1a0-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="ec1a0-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="ec1a0-869">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="ec1a0-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="ec1a0-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="ec1a0-872">Versões do ASP.NET SignalR em CDN</span><span class="sxs-lookup"><span data-stu-id="ec1a0-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="ec1a0-873">Os seguintes arquivos ASP.NET SignalR JavaScript são hospedados neste CDN:</span><span class="sxs-lookup"><span data-stu-id="ec1a0-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="ec1a0-874">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="ec1a0-875">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="ec1a0-876">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="ec1a0-877">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="ec1a0-878">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="ec1a0-879">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="ec1a0-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="ec1a0-881">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="ec1a0-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="ec1a0-883">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="ec1a0-884">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="ec1a0-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="ec1a0-886">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="ec1a0-887">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="ec1a0-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="ec1a0-889">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="ec1a0-890">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="ec1a0-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="ec1a0-892">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="ec1a0-893">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="ec1a0-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="ec1a0-895">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="ec1a0-896">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="ec1a0-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="ec1a0-898">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="ec1a0-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="ec1a0-899">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="ec1a0-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="ec1a0-901">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="ec1a0-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="ec1a0-902">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="ec1a0-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="ec1a0-904">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="ec1a0-905">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="ec1a0-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="ec1a0-907">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ec1a0-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="ec1a0-908">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="ec1a0-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="ec1a0-910">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="ec1a0-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="ec1a0-911">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="ec1a0-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="ec1a0-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="ec1a0-913">Para obter informações sobre os termos de uso para a CDN, consulte [Microsoft Ajax CDN termos de uso](https://www.asp.net/terms-of-use "Microsoft Ajax CDN termos de uso").</span><span class="sxs-lookup"><span data-stu-id="ec1a0-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
