---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: "Adicionar redes sociais Web ASP.NET de páginas de Sites (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este capítulo explica como integrar o seu site com os serviços de rede sociais. Neste capítulo, você aprenderá como permitir que pessoas/link de indicador seu site..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2c43fa7d286e43f3a4581662ce421c7435e1871f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="ba4f8-104">Adicionando Social Sites de rede para páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="ba4f8-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="ba4f8-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ba4f8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ba4f8-106">Este artigo explica como adicionar links de rede social para Facebook, Twitter, Reddit e Digg a páginas em um site de páginas da Web do ASP.NET (Razor) e como incluir feeds do Twitter, cartões jogador Xbox e Gravatar imagens.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="ba4f8-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="ba4f8-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ba4f8-108">Como permitir que as pessoas/link de indicador seu site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="ba4f8-109">Como adicionar um feed do Twitter.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="ba4f8-110">Como adicionar um Facebook **como** botão às páginas.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="ba4f8-111">Como renderizar imagens Gravatar.com.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="ba4f8-112">Como exibir um cartão de jogador Xbox em seu site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ba4f8-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="ba4f8-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ba4f8-114">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ba4f8-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ba4f8-115">Biblioteca auxiliar da Web ASP.NET (pacote do NuGet)</span><span class="sxs-lookup"><span data-stu-id="ba4f8-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="ba4f8-116">Este tutorial também funciona com o ASP.NET Web Pages 3, exceto as partes que usam a biblioteca auxiliar da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="ba4f8-117">Vincular seu site em Sites de rede Social</span><span class="sxs-lookup"><span data-stu-id="ba4f8-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="ba4f8-118">Se as pessoas que algo em seu site, eles querem geralmente compartilhá-lo com seus amigos.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="ba4f8-119">Você pode fazer isso fácil exibindo glifos (ícones) que pode ser clicado para compartilhar uma página no Digg, Reddit, Facebook, Twitter ou sites semelhantes.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="ba4f8-120">Para exibir esses glifos, adicione o `LinkSharecode` auxiliar a uma página.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="ba4f8-121">As pessoas que visite a página podem clicar em um glifo individuais.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="ba4f8-122">Se eles tiverem uma conta com esse site de rede social, eles, em seguida, podem lançar um link para a página no site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Figura 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="ba4f8-124">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado proprietário. - criar uma página chamada *ListLinkShare.cshtml* e adicione a marcação a seguir:</span><span class="sxs-lookup"><span data-stu-id="ba4f8-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="ba4f8-125">Neste exemplo, quando o `LinkShare` auxiliar execuções, o título da página é passado como um parâmetro, que por sua vez, passa o título da página para o site de rede social.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="ba4f8-126">No entanto, você pode transmitir qualquer cadeia de caracteres que você deseja.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="ba4f8-127">Este exemplo também especifica quais sites de rede social para incluir na lista.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="ba4f8-128">Você pode especificar os sites de rede social que são relevantes para seu site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-128">You can specify the social networking sites that are relevant to your site.</span></span>
- <span data-ttu-id="ba4f8-129">Execute o *ListLinkShare.cshtml* página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="ba4f8-130">(Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.)</span><span class="sxs-lookup"><span data-stu-id="ba4f8-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
- <span data-ttu-id="ba4f8-131">Clique em um glifo para um dos sites que você está conectado para.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="ba4f8-132">O link leva você à página no site de rede social selecionado onde você pode compartilhar um link.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="ba4f8-133">Por exemplo, se você clicar no link de Reddit, você é levado para a `submit to reddit` no site da Reddit.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

    ![Figura 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="ba4f8-135">Adicionar um Twitter Feed</span><span class="sxs-lookup"><span data-stu-id="ba4f8-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="ba4f8-136">Para obter informações sobre como usar um auxiliar do Twitter que é compatível com a versão atual da API do Twitter, consulte [auxiliar do Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="ba4f8-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="ba4f8-137">Este exemplo mostra como gravar seu próprio auxiliar para que possa reutilizar facilmente o código de várias páginas.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="ba4f8-138">Exibindo um Facebook &quot;como&quot; botão</span><span class="sxs-lookup"><span data-stu-id="ba4f8-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="ba4f8-139">Em alguns casos, a melhor opção é obter o código diretamente do provedor de rede social em vez de um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="ba4f8-140">Isso é especialmente verdadeiro se o provedor de rede social atualiza suas opções mais rapidamente do que o auxiliar é atualizado.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="ba4f8-141">Para adicionar recursos do Facebook (como o botão Like) em seu site, você pode recuperar os trechos de código do [developers.facebook.com](https://developers.facebook.com/) site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="ba4f8-142">No site do Facebook, você pode usar suas ferramentas para gerar um trecho de código que são relevante para seu site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="ba4f8-143">O seguinte código é o código que foi recuperado para a ferramenta como o botão no site developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="ba4f8-144">Você deve fornecer sua própria ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="ba4f8-145">Processar uma imagem de Gravatar</span><span class="sxs-lookup"><span data-stu-id="ba4f8-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="ba4f8-146">Um *Gravatar* (uma &quot;avatar globalmente reconhecido&quot;) é uma imagem que pode ser usado em vários sites como seu avatar &#8212; ou seja, uma imagem que representa a você.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="ba4f8-147">Por exemplo, um Gravatar pode identificar uma pessoa em uma postagem no fórum, um comentário de blog e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="ba4f8-148">(Você pode registrar seu próprio Gravatar no site Gravatar no [http://www.gravatar.com/](http://www.gravatar.com/).) Se você quiser exibir imagens ao lado de nomes ou endereços de email de pessoas no seu site, você pode usar o auxiliar Gravatar.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="ba4f8-149">Neste exemplo, você está usando um único Gravatar que representa a mesmo.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="ba4f8-150">Outra maneira de usar um Gravatar é permitir que as pessoas especificar seu endereço de Gravatar quando eles se registram no seu site.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="ba4f8-151">(Você pode aprender a permitir que as pessoas registrar no [adicionando segurança e associação a um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Em seguida, sempre que você exibe informações para o usuário, basta adicionar o Gravatar para onde você pode exibir o nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="ba4f8-152">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="ba4f8-153">Criar uma nova página da web denominado *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="ba4f8-154">Adicione a seguinte marcação para o arquivo:</span><span class="sxs-lookup"><span data-stu-id="ba4f8-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="ba4f8-155">O `Gravatar.GetHtml` método exibe a imagem Gravatar na página.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="ba4f8-156">Para alterar o tamanho da imagem, você pode incluir um número como um segundo parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="ba4f8-157">O tamanho padrão é 80.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-157">The default size is 80.</span></span> <span data-ttu-id="ba4f8-158">Números de tornar menor que 80 a imagem menores.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="ba4f8-159">Números maiores do que 80 aumente a imagem.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="ba4f8-160">No `Gravatar.GetHtml` métodos, substitua `<Your Gravatar account here>` com o endereço de email que você usa para sua conta Gravatar.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="ba4f8-161">(Se você não tiver uma conta de Gravatar, você pode usar o endereço de email de alguém que o faça.)</span><span class="sxs-lookup"><span data-stu-id="ba4f8-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="ba4f8-162">Execute a página em seu navegador.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-162">Run the page in your browser.</span></span> <span data-ttu-id="ba4f8-163">A página exibe duas imagens Gravatar para o endereço de email especificado.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="ba4f8-164">A segunda imagem é menor do que o primeiro.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-164">The second image is smaller than the first.</span></span> 

    ![Figura 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="ba4f8-166">Exibindo um cartão de jogador do Xbox</span><span class="sxs-lookup"><span data-stu-id="ba4f8-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="ba4f8-167">Quando as pessoas jogar Xbox Microsoft online, cada usuário tem uma ID exclusiva.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="ba4f8-168">As estatísticas são mantidas para cada jogador na forma de um cartão de jogador, que mostra suas reputação, a pontuação do jogador e executados recentemente jogos.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="ba4f8-169">Se você for um Xbox jogador, você pode mostrar seu cartão de jogador nas páginas em seu site usando o `GamerCard` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="ba4f8-170">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="ba4f8-171">Criar uma nova página chamada *XboxGamer.cshtml* e adicione a seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="ba4f8-172">Você usa o `GamerCard.GetHtml` propriedade para especificar o alias para o cartão jogador a ser exibido.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="ba4f8-173">Execute a página em seu navegador.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-173">Run the page in your browser.</span></span> <span data-ttu-id="ba4f8-174">A página exibe o cartão de jogador Xbox que você especificou.</span><span class="sxs-lookup"><span data-stu-id="ba4f8-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Figura 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
