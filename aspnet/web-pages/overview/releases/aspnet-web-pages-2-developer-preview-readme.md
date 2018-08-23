---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: Leiame do ASP.NET Web Pages 2 Developer Preview | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/14/2011
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 93e3f9c9d7c90f1ebfd9f482166aeb833cae73e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833073"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Leiame do ASP.NET Web Pages 2 Developer Preview
====================
por [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Leiame do ASP.NET Web Pages 2 Developer Preview

14 de setembro de 2011

### <a name="contents"></a>Conteúdo

#### <a id="_Toc303701284"></a>  Notas de instalação

Para instalar as páginas da Web 2 Developer Preview, você tem estas opções:

- Instale o WebMatrix 2 Beta usando o [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883). O WebMatrix é um conjunto de ferramentas de desenvolvimento da web gratuito que inclui páginas da Web ASP.NET. Para obter mais informações, consulte a seção de instalação [os principais recursos em páginas da Web de ASP.NET 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Instalar a visualização do desenvolvedor 2 páginas da Web diretamente usando o [link de download](https://go.microsoft.com/fwlink/?LinkID=226335). Use essa abordagem se você quiser criar aplicativos de páginas da Web usando um texto de editor como o bloco de notas. Para executar aplicativos Web Pages 2, você deve ter o IIS Express 7.5. (Isso é incluído automaticamente com o WebMatrix). Para obter dicas sobre como testar uma página de páginas da Web usando o IIS Express, consulte a barra lateral "Criando e testando ASP.NET páginas usando seu próprio Editor de texto" em [Introdução ao WebMatrix e páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).

Páginas da Web do ASP.NET 2 Developer Preview pode ser instalado e pode ser executado lado a lado com o ASP.NET Web Pages 1. <a id="a"></a>Para obter detalhes, consulte a seção "Executar páginas da Web aplicativos lado a lado" na [os principais recursos em páginas da Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Documentação

Tutoriais e outras informações sobre páginas da Web do ASP.NET estão disponíveis na página de páginas da Web do site do ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Para obter informações sobre novos recursos e aprimoramentos do Web Pages 2, consulte [os principais recursos em páginas da Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Suporte

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Essa é uma versão de visualização e não é oficialmente suportada. Se você tiver dúvidas sobre como trabalhar com esta versão, poste-os para o fórum de páginas da Web ASP.NET ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), onde os membros da comunidade do ASP.NET são com frequência pode oferecer suporte informal.

#### <a id="_Toc303701287"></a>  Requisitos de software

Páginas da Web ASP.NET 2 requer o .NET Framework 4. Ele também funciona com a versão de visualização do desenvolvedor do .NET Framework 4.5.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Correções, problemas conhecidos e alterações significativas

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **É\* métodos (por exemplo, IsDateTime) agora retorna valores corretos para todas as culturas.** Alguns métodos como *IsDateTime* retornado anteriormente *falso* quando eles deveriam ter retornado *verdadeiro* porque eles foram anteriormente realizando verificações específicas da cultura. Esses métodos foram corrigidos agora considerar cultura. Essa é uma alteração significativa; Se seu aplicativo se basear no comportamento antigo, ele será interrompido.
- **O comportamento do método Href foi alterado.** Anteriormente, chamar Href("~/SomeFile") retornaria um URL relativo ao arquivo em execução no momento. Agora Href("~/SomeFile") sempre retorna um caminho absoluto da raiz do aplicativo. Na maioria dos casos, esse comportamento não fará uma diferença no valor de retorno. Essa alteração foi feita para corrigir determinados cenários Ajax. Por exemplo, considere o exemplo de código a seguir: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Esse código anteriormente resolveria para Images/Logo.jpg, que seria incorreto para uma solicitação Ajax para a página. Agora, ele será interpretado para a raiz das (/ MySite/Images/Logo.jpg).
- **O método HttpContext.RedirectLocal mudou**. Agora, esse método aceita apenas as URLs são relativas ao aplicativo atual. URLs totalmente qualificadas serão rejeitadas.
- **O método ModelState IsValid agora exige que você chame validar primeiro**. Se você estiver convertendo seu aplicativo para usar os novos métodos de validação de entrada e se está chamando o *ModelState* método, você deve chamar *Validation.Validate* com antecedência. Por exemplo, agora você deve seguir esse padrão: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  No entanto, recomendamos que se você usar os novos métodos de validação de entrada, não use *ModelState*. Em vez disso, a estruturar seu código como este: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **No Internet Explorer 7 e no Internet Explorer 8, a validação do lado do cliente não funciona**. Validação do lado do cliente não funciona devido a incompatibilidades com o jQuery 1.6.2, que é incluído com o modelo de projeto padrão. (A validação do lado do servidor funciona.).

#### <a id="_Toc303701289"></a>  Isenção de responsabilidade

© 2011 Microsoft Corporation. Todos os direitos reservados. Este documento é fornecido "como-está." As informações e opiniões expressadas neste documento, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio. Você assume o risco de utilizá-las.
