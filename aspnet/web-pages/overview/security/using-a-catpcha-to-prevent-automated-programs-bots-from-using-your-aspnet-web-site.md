---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Site usando um CAPTCHA para impedir que Bots usando o Razor da Web do ASP.NET) | Microsoft Docs
author: microsoft
description: "Este artigo explica como usar ReCaptcha (uma medida de segurança) para impedir a execução de tarefas em um páginas da Web ASP.NET (Razor) programas automatizados (bots),..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="74930-103">Site usando um CAPTCHA para impedir que Bots usando o Razor da Web do ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="74930-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="74930-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="74930-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="74930-105">Este artigo explica como usar ReCaptcha (uma medida de segurança) para impedir a execução de tarefas em um site de páginas da Web do ASP.NET (Razor) programas automatizados (bots).</span><span class="sxs-lookup"><span data-stu-id="74930-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="74930-106">**O que você aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="74930-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="74930-107">Como adicionar um teste CAPTCHA em seu site.</span><span class="sxs-lookup"><span data-stu-id="74930-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="74930-108">Estes são os recursos ASP.NET introduzidos no artigo:</span><span class="sxs-lookup"><span data-stu-id="74930-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="74930-109">O `ReCaptcha` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="74930-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="74930-110">As informações neste artigo se aplica a páginas da Web de ASP.NET 1.0 e 2 páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="74930-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="74930-111">Sobre CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="74930-111">About CAPTCHAs</span></span>

<span data-ttu-id="74930-112">Sempre que você permitir que pessoas registrar no seu site, ou até mesmo Insira um nome e uma URL (como um comentário de blog), você pode obter uma inundação de falsos nomes.</span><span class="sxs-lookup"><span data-stu-id="74930-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="74930-113">Eles costumam ser mantidos por programas automatizados (bots) que tentam deixar de URLs em cada site, que ele pode encontrar.</span><span class="sxs-lookup"><span data-stu-id="74930-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="74930-114">(Uma motivação comum é postar as URLs de produtos para venda.)</span><span class="sxs-lookup"><span data-stu-id="74930-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="74930-115">Você pode ajudar a garantir que um usuário é a pessoa real e não um programa de computador usando um *CAPTCHA* para validar os usuários quando eles registrar ou caso contrário, insira seu nome e o site.</span><span class="sxs-lookup"><span data-stu-id="74930-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="74930-116">CAPTCHA significa teste completamente automatizada pública Turing dizer a separar os usuários e computadores.</span><span class="sxs-lookup"><span data-stu-id="74930-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="74930-117">É um CAPTCHA um *desafio / resposta* teste no qual o usuário é solicitado a fazer algo que é fácil de uma pessoa fazer mas difícil para um programa automatizado fazer.</span><span class="sxs-lookup"><span data-stu-id="74930-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="74930-118">O tipo mais comum de CAPTCHA é um onde você pode ver algumas letras distorcidas e será solicitado a digitá-los.</span><span class="sxs-lookup"><span data-stu-id="74930-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="74930-119">(A distorção deve para dificultar bots decifrar as letras.)</span><span class="sxs-lookup"><span data-stu-id="74930-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="74930-120">Adicionando um teste ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="74930-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="74930-121">Nas páginas do ASP.NET, você pode usar o `ReCaptcha` auxiliar para renderizar um teste CAPTCHA baseia-se no serviço ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="74930-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="74930-122">O `ReCaptcha` auxiliar exibe uma imagem de duas palavras distorcidas que os usuários precisarão inserir corretamente antes da página é validada.</span><span class="sxs-lookup"><span data-stu-id="74930-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="74930-123">A resposta do usuário é validada pelo serviço ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="74930-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="74930-124">Registrar seu site em ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="74930-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="74930-125">Ao concluir o registro, você receberá uma chave pública e uma chave privada.</span><span class="sxs-lookup"><span data-stu-id="74930-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="74930-126">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.</span><span class="sxs-lookup"><span data-stu-id="74930-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="74930-127">Se você ainda não tiver um  *\_AppStart.cshtml* de arquivo, na pasta raiz de um site, crie um arquivo chamado  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="74930-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="74930-128">Adicione o seguinte `Recaptcha` configurações auxiliar a  *\_AppStart.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="74930-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="74930-129">Definir o `PublicKey` e `PrivateKey` propriedades usando suas próprias chaves públicas e privadas.</span><span class="sxs-lookup"><span data-stu-id="74930-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="74930-130">Salve o  *\_AppStart.cshtml* de arquivo e fechá-la.</span><span class="sxs-lookup"><span data-stu-id="74930-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="74930-131">Na pasta raiz de um site, crie a nova página chamada *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="74930-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="74930-132">Substitua o conteúdo existente com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="74930-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="74930-133">Execute o *Recaptcha.cshtml* página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="74930-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="74930-134">Se o `PrivateKey` valor é válido, a página exibe o controle de ReCaptcha e um botão.</span><span class="sxs-lookup"><span data-stu-id="74930-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="74930-135">Se você não tiver definido as chaves globalmente no  *\_AppStart.html*, a página exibirá um erro.</span><span class="sxs-lookup"><span data-stu-id="74930-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="74930-136">Digite as palavras para o teste.</span><span class="sxs-lookup"><span data-stu-id="74930-136">Enter the words for the test.</span></span> <span data-ttu-id="74930-137">Se você passar no teste de ReCaptcha, você verá uma mensagem para esse efeito.</span><span class="sxs-lookup"><span data-stu-id="74930-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="74930-138">Caso contrário, você verá uma mensagem de erro e o controle ReCaptcha é exibida novamente.</span><span class="sxs-lookup"><span data-stu-id="74930-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="74930-139">Se o computador estiver em um domínio que usa um servidor proxy, você talvez precise configurar o `defaultproxy` elemento o *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="74930-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="74930-140">A exemplo a seguir mostra um *Web. config* arquivo com o `defaultproxy` elemento configurado para permitir que o serviço ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="74930-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="74930-141">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="74930-141">Additional Resources</span></span>


- [<span data-ttu-id="74930-142">Personalizar o comportamento de todo o Site para Sites de páginas da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="74930-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="74930-143">Site de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="74930-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
