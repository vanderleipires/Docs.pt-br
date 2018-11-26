---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Auxiliar do Twitter com páginas da Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Este tópico e o aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto o WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar o auxiliar de...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299424"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="3706b-104">Auxiliar do Twitter com páginas da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3706b-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="3706b-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3706b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3706b-106">Twitter auxiliares estão obsoletos.</span><span class="sxs-lookup"><span data-stu-id="3706b-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="3706b-107">Para ferramentas de envolvimento de mais recente do Twitter, para sites, consulte [do Twitter para visão geral de sites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="3706b-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="3706b-108">Este tópico e o aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="3706b-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="3706b-109">Ele contém o código auxiliar do Twitter e mostra como chamar os métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="3706b-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="3706b-110">Esse código para o arquivo Twitter.cshtml foi desenvolvido pela **Tian panorâmica** da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3706b-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3706b-111">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="3706b-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3706b-112">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3706b-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3706b-113">Este tutorial também funciona com ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="3706b-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="3706b-114">Introdução</span><span class="sxs-lookup"><span data-stu-id="3706b-114">Introduction</span></span>

<span data-ttu-id="3706b-115">Este tópico demonstra como adicionar um auxiliar do Twitter ao seu aplicativo e usar a sintaxe do Razor para chamar os métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="3706b-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="3706b-116">O auxiliar do Twitter facilita a incorporar botões do Twitter e widgets em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3706b-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="3706b-117">Para usar um widget do Twitter, como o cronograma de um usuário ou os resultados da pesquisa para uma hashtag, você deve primeiro criar o [widget no Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="3706b-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="3706b-118">Depois de criar seu widget, você receberá uma id de widget. Você pode passar essa id de widget como um parâmetro ao chamar os métodos auxiliares que mostram o widget.</span><span class="sxs-lookup"><span data-stu-id="3706b-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="3706b-119">Este tópico foi escrito para a versão 1.1 da API do Twitter.</span><span class="sxs-lookup"><span data-stu-id="3706b-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="3706b-120">Ao adicionar o código auxiliar do Twitter diretamente ao seu projeto, você pode atualizar o código auxiliar se altera a API do Twitter.</span><span class="sxs-lookup"><span data-stu-id="3706b-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="3706b-121">Para obter informações sobre como instalar o WebMatrix, consulte [apresentando o ASP.NET Web Pages 2 - Introdução ao](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="3706b-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="3706b-122">Adicionar o auxiliar do Twitter ao seu projeto</span><span class="sxs-lookup"><span data-stu-id="3706b-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="3706b-123">Para adicionar o auxiliar do Twitter, em primeiro lugar, adicione uma pasta denominada **App\_código** ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="3706b-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="3706b-124">Em seguida, crie um arquivo chamado **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="3706b-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Pasta App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="3706b-126">Substitua o código padrão no Twitter.cshtml com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3706b-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="3706b-127">Chamar métodos do Twitter de suas páginas da web</span><span class="sxs-lookup"><span data-stu-id="3706b-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="3706b-128">O exemplo a seguir mostra como usar os métodos de auxiliar do Twitter de uma página em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="3706b-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="3706b-129">Em seu projeto, você desejará substituir os valores de parâmetros com valores que são relevantes para suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="3706b-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="3706b-130">Você pode usar as ids de widget fornecido para explorar como os métodos funcionam, mas você desejará gerar seus próprios widgets para seu projeto.</span><span class="sxs-lookup"><span data-stu-id="3706b-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="3706b-131">Nem todos os parâmetros mostrados abaixo são necessários.</span><span class="sxs-lookup"><span data-stu-id="3706b-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="3706b-132">Os parâmetros opcionais são usados para personalizar como o botão ou widget é exibida.</span><span class="sxs-lookup"><span data-stu-id="3706b-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="3706b-133">Por exemplo, o botão execute requer apenas o nome de usuário a seguir, mas o exemplo mostra como incluir o número de seguidores e como especificar o tamanho do botão e o idioma.</span><span class="sxs-lookup"><span data-stu-id="3706b-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="3706b-134">Ver os resultados</span><span class="sxs-lookup"><span data-stu-id="3706b-134">See the results</span></span>

<span data-ttu-id="3706b-135">O código acima produz os seguintes botões e widgets.</span><span class="sxs-lookup"><span data-stu-id="3706b-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="3706b-136">Esses botões e widgets são totalmente funcional, não capturas de tela.</span><span class="sxs-lookup"><span data-stu-id="3706b-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="3706b-137">Botão a seguir é exibido em espanhol, porque o parâmetro de idioma foi definido como **es**.</span><span class="sxs-lookup"><span data-stu-id="3706b-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="3706b-138">Siga o botão</span><span class="sxs-lookup"><span data-stu-id="3706b-138">Follow Button</span></span>

<span data-ttu-id="3706b-139">[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="3706b-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="3706b-140">Botão de Tweet</span><span class="sxs-lookup"><span data-stu-id="3706b-140">Tweet Button</span></span>

<span data-ttu-id="3706b-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="3706b-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="3706b-142">Linha do tempo do usuário (perfil)</span><span class="sxs-lookup"><span data-stu-id="3706b-142">User Timeline (Profile)</span></span>

<span data-ttu-id="3706b-143">[TWEETS por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3706b-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="3706b-144">Favoritos</span><span class="sxs-lookup"><span data-stu-id="3706b-144">Favorites</span></span>

<span data-ttu-id="3706b-145">[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3706b-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="3706b-146">Lista</span><span class="sxs-lookup"><span data-stu-id="3706b-146">List</span></span>

<span data-ttu-id="3706b-147">[TWEETS de @Microsoft/MS \_consumidor\_bandas](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3706b-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="3706b-148">Pesquisar</span><span class="sxs-lookup"><span data-stu-id="3706b-148">Search</span></span>

<span data-ttu-id="3706b-149">[Um Tweet sobre &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="3706b-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
