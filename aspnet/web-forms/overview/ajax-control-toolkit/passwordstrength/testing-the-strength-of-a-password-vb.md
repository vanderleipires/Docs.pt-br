---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testando a força da senha (VB) | Microsoft Docs
author: wenz
description: Senhas são necessárias em praticamente qualquer lugar, para que usuários lentos tendem a escolher senhas simples, que são fáceis de quebra. O controle PasswordStrength no ASP. N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="91f61-104">Testando a força da senha (VB)</span><span class="sxs-lookup"><span data-stu-id="91f61-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="91f61-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="91f61-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="91f61-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="91f61-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="91f61-107">Senhas são necessárias em praticamente qualquer lugar, para que usuários lentos tendem a escolher senhas simples, que são fáceis de quebra.</span><span class="sxs-lookup"><span data-stu-id="91f61-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="91f61-108">O controle PasswordStrength no Kit de ferramentas de controle AJAX ASP.NET pode verificar o quão bom é de uma senha.</span><span class="sxs-lookup"><span data-stu-id="91f61-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="91f61-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="91f61-109">Overview</span></span>

<span data-ttu-id="91f61-110">Senhas são necessárias em praticamente qualquer lugar, para que usuários lentos tendem a escolher senhas simples, que são fáceis de quebra.</span><span class="sxs-lookup"><span data-stu-id="91f61-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="91f61-111">O `PasswordStrength` controle no ASP.NET AJAX Control Toolkit pode verificar o quão bom é de uma senha.</span><span class="sxs-lookup"><span data-stu-id="91f61-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="91f61-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="91f61-112">Steps</span></span>

<span data-ttu-id="91f61-113">O `PasswordStrength` controle estende uma caixa de texto e verifica se a senha nele é bom o bastante.</span><span class="sxs-lookup"><span data-stu-id="91f61-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="91f61-114">Ele oferece uma variedade de opções por meio de atributos; Aqui estão apenas alguns deles:</span><span class="sxs-lookup"><span data-stu-id="91f61-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="91f61-115">`MinimumNumericCharacters` número mínimo de caracteres numéricos necessários na senha</span><span class="sxs-lookup"><span data-stu-id="91f61-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="91f61-116">`MinimumSymbolCharacters` número mínimo de caracteres de símbolo (não letras e dígitos) necessário na senha</span><span class="sxs-lookup"><span data-stu-id="91f61-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="91f61-117">`PreferredPasswordLength` comprimento mínimo da senha</span><span class="sxs-lookup"><span data-stu-id="91f61-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="91f61-118">`RequiresUpperAndLowerCaseCharacters` Se a senha deve usar caracteres maiusculos e minúsculos</span><span class="sxs-lookup"><span data-stu-id="91f61-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="91f61-119">O `StrengthIndicatorType` fornece as informações como apresentar a força da senha, como texto (valor `"Text"`) ou como um tipo de barra de progresso (valor `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="91f61-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="91f61-120">No `DisplayPosition` atributo, configure onde as informações são exibidas.</span><span class="sxs-lookup"><span data-stu-id="91f61-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="91f61-121">Aqui está um exemplo completo, incluindo o ASP.NET AJAX `ScriptManager` controle, o `PasswordStrength` controle e logicamente uma caixa de texto onde o usuário pode digitar uma senha.</span><span class="sxs-lookup"><span data-stu-id="91f61-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="91f61-122">Para fins de demonstração, o campo de formulário de segundo é um campo de texto normal e não é um campo de senha para que você pode ver durante o desenvolvimento, o que está digitando.</span><span class="sxs-lookup"><span data-stu-id="91f61-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="91f61-123">Execute a página e digite ausente: somente depois que você inseriu letras minúsculas, letras maiusculas, dígitos e símbolos, a senha é considerada como inviolável.</span><span class="sxs-lookup"><span data-stu-id="91f61-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="91f61-124">[![Agora, a senha é válida (muito)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="91f61-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="91f61-125">Agora, a senha é válida (muito) ([clique para exibir a imagem em tamanho normal](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="91f61-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="91f61-126">Anterior</span><span class="sxs-lookup"><span data-stu-id="91f61-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
