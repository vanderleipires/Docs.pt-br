---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Site usando um CAPTCHA para impedir que Bots usando o Razor da Web do ASP.NET) | Microsoft Docs
author: microsoft
description: Este artigo explica como usar ReCaptcha (uma medida de segurança) para impedir a execução de tarefas em um páginas da Web ASP.NET (Razor) programas automatizados (bots),...
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
ms.locfileid: "26529915"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Site usando um CAPTCHA para impedir que Bots usando o Razor da Web do ASP.NET)
====================
por [Microsoft](https://github.com/microsoft)

> Este artigo explica como usar ReCaptcha (uma medida de segurança) para impedir a execução de tarefas em um site de páginas da Web do ASP.NET (Razor) programas automatizados (bots).
> 
> **O que você aprenderá:** 
> 
> - Como adicionar um teste CAPTCHA em seu site.
> 
> Estes são os recursos ASP.NET introduzidos no artigo:
> 
> - O `ReCaptcha` auxiliar.
> 
> > [!NOTE]
> > As informações neste artigo se aplica a páginas da Web de ASP.NET 1.0 e 2 páginas da Web.


## <a name="about-captchas"></a>Sobre CAPTCHAs

Sempre que você permitir que pessoas registrar no seu site, ou até mesmo Insira um nome e uma URL (como um comentário de blog), você pode obter uma inundação de falsos nomes. Eles costumam ser mantidos por programas automatizados (bots) que tentam deixar de URLs em cada site, que ele pode encontrar. (Uma motivação comum é postar as URLs de produtos para venda.)

Você pode ajudar a garantir que um usuário é a pessoa real e não um programa de computador usando um *CAPTCHA* para validar os usuários quando eles registrar ou caso contrário, insira seu nome e o site. CAPTCHA significa teste completamente automatizada pública Turing dizer a separar os usuários e computadores. É um CAPTCHA um *desafio / resposta* teste no qual o usuário é solicitado a fazer algo que é fácil de uma pessoa fazer mas difícil para um programa automatizado fazer. O tipo mais comum de CAPTCHA é um onde você pode ver algumas letras distorcidas e será solicitado a digitá-los. (A distorção deve para dificultar bots decifrar as letras.)

## <a name="adding-a-recaptcha-test"></a>Adicionando um teste ReCaptcha

Nas páginas do ASP.NET, você pode usar o `ReCaptcha` auxiliar para renderizar um teste CAPTCHA baseia-se no serviço ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). O `ReCaptcha` auxiliar exibe uma imagem de duas palavras distorcidas que os usuários precisarão inserir corretamente antes da página é validada. A resposta do usuário é validada pelo serviço ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrar seu site em ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Ao concluir o registro, você receberá uma chave pública e uma chave privada.
2. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.
3. Se você ainda não tiver um  *\_AppStart.cshtml* de arquivo, na pasta raiz de um site, crie um arquivo chamado  *\_AppStart.cshtml*.
4. Adicione o seguinte `Recaptcha` configurações auxiliar a  *\_AppStart.cshtml* arquivo: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Definir o `PublicKey` e `PrivateKey` propriedades usando suas próprias chaves públicas e privadas.
6. Salve o  *\_AppStart.cshtml* de arquivo e fechá-la.
7. Na pasta raiz de um site, crie a nova página chamada *Recaptcha.cshtml*.
8. Substitua o conteúdo existente com o seguinte: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Execute o *Recaptcha.cshtml* página em um navegador. Se o `PrivateKey` valor é válido, a página exibe o controle de ReCaptcha e um botão. Se você não tiver definido as chaves globalmente no  *\_AppStart.html*, a página exibirá um erro. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Digite as palavras para o teste. Se você passar no teste de ReCaptcha, você verá uma mensagem para esse efeito. Caso contrário, você verá uma mensagem de erro e o controle ReCaptcha é exibida novamente.

> [!NOTE]
> Se o computador estiver em um domínio que usa um servidor proxy, você talvez precise configurar o `defaultproxy` elemento o *Web. config* arquivo. A exemplo a seguir mostra um *Web. config* arquivo com o `defaultproxy` elemento configurado para permitir que o serviço ReCaptcha.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Personalizar o comportamento de todo o Site para Sites de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Site de ReCaptcha](https://www.google.com/recaptcha)
