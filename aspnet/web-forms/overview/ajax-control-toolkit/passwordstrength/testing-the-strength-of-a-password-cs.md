---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Testando a força da senha (c#) | Microsoft Docs
author: wenz
description: Senhas são necessárias em praticamente qualquer lugar, para que usuários lentos tendem a escolher senhas simples, que são fáceis de quebra. O controle PasswordStrength no ASP. N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f4a7128f2edbef4fbe95faf9de19bdae5f436e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="testing-the-strength-of-a-password-c"></a>Testando a força da senha (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Senhas são necessárias em praticamente qualquer lugar, para que usuários lentos tendem a escolher senhas simples, que são fáceis de quebra. O controle PasswordStrength no Kit de ferramentas de controle AJAX ASP.NET pode verificar o quão bom é de uma senha.


## <a name="overview"></a>Visão geral

Senhas são necessárias em praticamente qualquer lugar, para que usuários lentos tendem a escolher senhas simples, que são fáceis de quebra. O `PasswordStrength` controle no ASP.NET AJAX Control Toolkit pode verificar o quão bom é de uma senha.

## <a name="steps"></a>Etapas

O `PasswordStrength` controle estende uma caixa de texto e verifica se a senha nele é bom o bastante. Ele oferece uma variedade de opções por meio de atributos; Aqui estão apenas alguns deles:

- `MinimumNumericCharacters` número mínimo de caracteres numéricos necessários na senha
- `MinimumSymbolCharacters` número mínimo de caracteres de símbolo (não letras e dígitos) necessário na senha
- `PreferredPasswordLength` comprimento mínimo da senha
- `RequiresUpperAndLowerCaseCharacters` Se a senha deve usar caracteres maiusculos e minúsculos

O `StrengthIndicatorType` fornece as informações como apresentar a força da senha, como texto (valor `"Text"`) ou como um tipo de barra de progresso (valor `"BarIndicator"`). No `DisplayPosition` atributo, configure onde as informações são exibidas. Aqui está um exemplo completo, incluindo o ASP.NET AJAX `ScriptManager` controle, o `PasswordStrength` controle e logicamente uma caixa de texto onde o usuário pode digitar uma senha. Para fins de demonstração, o campo de formulário de segundo é um campo de texto normal e não é um campo de senha para que você pode ver durante o desenvolvimento, o que está digitando.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Execute a página e digite ausente: somente depois que você inseriu letras minúsculas, letras maiusculas, dígitos e símbolos, a senha é considerada como inviolável.


[![Agora, a senha é válida (muito)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Agora, a senha é válida (muito) ([clique para exibir a imagem em tamanho normal](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](testing-the-strength-of-a-password-vb.md)
