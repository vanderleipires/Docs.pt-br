---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Site usando um CAPTCHA para evitar que os Bots usando o Razor da Web do ASP.NET) | Microsoft Docs
author: microsoft
description: Este artigo explica como usar ReCaptcha (uma medida de segurança) para impedir que programas automatizados (bots) executando tarefas em uma páginas da Web ASP.NET (Razor),...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: dc014f42490327743764787d58c613b7caa89f1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825208"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Site usando um CAPTCHA para evitar que os Bots usando o Razor da Web do ASP.NET)
====================
por [Microsoft](https://github.com/microsoft)

> Este artigo explica como usar ReCaptcha (uma medida de segurança) para impedir que programas automatizados (bots) executando tarefas em um site de páginas da Web do ASP.NET (Razor).
> 
> **O que você aprenderá:** 
> 
> - Como adicionar um teste CAPTCHA em seu site.
> 
> Estes são os recursos do ASP.NET introduzidos no artigo:
> 
> - O `ReCaptcha` auxiliar.
> 
> > [!NOTE]
> > As informações neste artigo se aplica a 1.0 de páginas da Web do ASP.NET e Web Pages 2.


## <a name="about-captchas"></a>Sobre CAPTCHAs

Sempre que você permitir que as pessoas se registrar no seu site ou até mesmo apenas Insira um nome e uma URL (como um comentário de blog), poderá receber uma inundação de nomes falsos. Eles costumam ser mantidos por programas automatizados (bots) que tentam deixar as URLs em cada site que eles possam encontrar. (Uma motivação comum é postar as URLs de produtos para venda.)

Você pode ajudar a garantir que um usuário é a pessoa real e não um programa de computador usando um *CAPTCHA* para validar os usuários quando eles se registrar ou caso contrário, insira seu nome e o site. CAPTCHA significa teste completamente automatizada pública Turing informar ao separar os seres humanos e computadores. Um CAPTCHA é um *desafio / resposta* teste no qual o usuário será solicitado a fazer algo que é fácil de uma pessoa fazer, mas difícil para um programa automatizado fazer. O tipo mais comum de CAPTCHA é um onde você pode ver algumas letras distorcidas e será solicitado a digitá-los. (A distorção deve torná-lo mais difícil para os bots decifrar as letras.)

## <a name="adding-a-recaptcha-test"></a>Adicionando um teste ReCaptcha

Em páginas ASP.NET, você pode usar o `ReCaptcha` auxiliar para renderizar um teste CAPTCHA baseia-se no serviço ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). O `ReCaptcha` auxiliar exibe uma imagem de duas palavras distorcidas que os usuários precisarão inserir corretamente antes da página é validada. A resposta do usuário é validada pelo serviço Recaptcha.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrar seu site em ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Quando você concluir o registro, você obterá uma chave pública e uma chave privada.
2. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não fez isso.
3. Se você ainda não tiver um  *\_AppStart.cshtml* de arquivo, na pasta raiz de um site, crie um arquivo chamado  *\_AppStart.cshtml*.
4. Adicione o seguinte `Recaptcha` configurações de auxiliar na  *\_AppStart.cshtml* arquivo: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Defina as `PublicKey` e `PrivateKey` propriedades usando suas próprias chaves públicas e privadas.
6. Salvar a  *\_AppStart.cshtml* de arquivo e feche-o.
7. Na pasta raiz de um site, crie a nova página chamada *Recaptcha.cshtml*.
8. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Execute o *Recaptcha.cshtml* página em um navegador. Se o `PrivateKey` valor é válido, a página exibe o controle ReCaptcha e um botão. Se você não tiver definido as chaves globalmente no  *\_AppStart.html*, a página exibirá um erro. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Insira as palavras para o teste. Se você passar o teste ReCaptcha, você verá uma mensagem para esse efeito. Caso contrário, você verá uma mensagem de erro e o controle ReCaptcha é exibida novamente.

> [!NOTE]
> Se o computador estiver em um domínio que usa o servidor proxy, você talvez precise configurar o `defaultproxy` elemento do *Web. config* arquivo. A exemplo a seguir mostra uma *Web. config* do arquivo com o `defaultproxy` elemento configurado para habilitar o serviço ReCaptcha funcione.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Personalizando o comportamento de todo o Site para Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Site ReCaptcha](https://www.google.com/recaptcha)
