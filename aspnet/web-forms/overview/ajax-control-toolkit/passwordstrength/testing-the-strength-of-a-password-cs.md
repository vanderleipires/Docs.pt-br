---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Teste da força de uma senha (c#) | Microsoft Docs
author: wenz
description: As senhas são necessárias em praticamente qualquer lugar, para que os usuários lentos tendem a escolher senhas simples, que são fáceis de interromper. O controle PasswordStrength do ASP. N...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: 2eec810a0e37347059d3e82a37b25d5663eab987
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839798"
---
<a name="testing-the-strength-of-a-password-c"></a>Teste da força de uma senha (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> As senhas são necessárias em praticamente qualquer lugar, para que os usuários lentos tendem a escolher senhas simples, que são fáceis de interromper. O controle PasswordStrength no ASP.NET AJAX Control Toolkit pode verificar o quão bom é de uma senha.


## <a name="overview"></a>Visão geral

As senhas são necessárias em praticamente qualquer lugar, para que os usuários lentos tendem a escolher senhas simples, que são fáceis de interromper. O `PasswordStrength` controle no ASP.NET AJAX Control Toolkit pode verificar o quão bom é de uma senha.

## <a name="steps"></a>Etapas

O `PasswordStrength` controle estende uma caixa de texto e verifica se a senha nele é bom o suficiente. Ele oferece uma infinidade de opções por meio de atributos. Aqui estão apenas alguns deles:

- `MinimumNumericCharacters` número mínimo de caracteres necessários na senha
- `MinimumSymbolCharacters` número mínimo de caracteres de símbolo (não letras e dígitos) necessário na senha
- `PreferredPasswordLength` comprimento mínimo da senha
- `RequiresUpperAndLowerCaseCharacters` Se a senha precisa usar caracteres maiusculos e minúsculos

O `StrengthIndicatorType` fornece as informações de como apresentar a força da senha, como texto (valor `"Text"`) ou como um tipo de barra de progresso (valor `"BarIndicator"`). No `DisplayPosition` atributo, configure onde as informações são exibidas. Aqui está um exemplo completo, incluindo o ASP.NET AJAX `ScriptManager` controle, o `PasswordStrength` controle e certamente uma caixa de texto em que o usuário pode inserir uma senha. Para fins de demonstração, o campo de segunda forma é um campo de texto normal e não é um campo de senha para que você pode ver durante o desenvolvimento, o que está digitando.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Execute a página e digite distância: somente depois que você inseriu letras minúsculas, letras maiusculas, dígitos e símbolos, a senha é considerada como inviolável.


[![Agora, a senha é bom (muito)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Agora, a senha é bom (muito) ([clique para exibir a imagem em tamanho normal](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](testing-the-strength-of-a-password-vb.md)
