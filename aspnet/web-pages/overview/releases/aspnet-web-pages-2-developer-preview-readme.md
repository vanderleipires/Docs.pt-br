---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "Leiame de visualização da Web do ASP.NET 2 páginas desenvolvedor | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Leiame de visualização do desenvolvedor
====================
por [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Leiame de visualização do desenvolvedor

14 de setembro de 2011

### <a name="contents"></a>Conteúdo

#### <a id="_Toc303701284"></a>Notas de instalação

Para instalar a visualização do desenvolvedor 2 páginas da Web, você terá estas opções:

- Instalar o WebMatrix 2 Beta usando o [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883). O WebMatrix é um conjunto de ferramentas de desenvolvimento da web gratuita que inclui páginas da Web ASP.NET. Para obter mais informações, consulte a seção instalação [a recursos principais na visualização de desenvolvedores de 2 de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkID=227824).

- Instalar a visualização do desenvolvedor 2 páginas da Web diretamente usando o [link de download](https://go.microsoft.com/fwlink/?LinkID=226335). Use essa abordagem se você quiser criar páginas da Web de aplicativos usando um texto de editor, como o bloco de notas. Para executar aplicativos de 2 páginas da Web, você deve ter o IIS 7.5 Express. (Isso é incluído automaticamente com o WebMatrix.) Para obter dicas sobre como testar uma página de páginas da Web usando o IIS Express, consulte a barra lateral "Criando e testando ASP.NET páginas usando seu próprio Editor de texto" no [guia de Introdução ao WebMatrix e páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).

Páginas da Web do ASP.NET 2 Developer Preview pode ser instalado e pode executar lado a lado com 1 de páginas da Web do ASP.NET. <a id="a"></a>Para obter detalhes, consulte a seção "Executar páginas da Web aplicativos-lado a lado" em [a recursos principais em páginas da Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>Documentação

Tutoriais e outras informações sobre páginas da Web do ASP.NET estão disponíveis na página de páginas da Web do site da Web do ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Para obter informações sobre novos recursos e aprimoramentos em 2 de páginas da Web, consulte [a recursos principais em páginas da Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>Suporte

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>Isso é uma versão de visualização e não é oficialmente suportado. Se você tiver dúvidas sobre como trabalhar com esta versão, poste-as para o fórum de páginas da Web ASP.NET ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) ), onde os membros da comunidade do ASP.NET são frequentemente pode oferecer suporte informal.

#### <a id="_Toc303701287"></a>Requisitos de software

Páginas da Web ASP.NET 2 requer o .NET Framework 4. Também funciona com a versão de visualização do desenvolvedor do .NET Framework 4.5.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Correções de problemas conhecidos e as alterações recentes

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **É\* métodos (por exemplo, IsDateTime) agora retorna valores corretos para todas as culturas.** Alguns métodos como *IsDateTime* retornados anteriormente *false* quando deve ter retornado *true* porque eles foram anteriormente executando verificações de específicos de cultura. Esses métodos foram corrigidos para agora consideram cultura. Isso é uma alteração significativa; Se seu aplicativo depende do comportamento antigo, ele será interrompido.
- **O comportamento do método Href foi alterado.** Anteriormente, chamada Href("~/SomeFile") retornaria um URL relativo ao arquivo de atualmente em execução. Agora Href("~/SomeFile") sempre retorna um caminho absoluto da raiz do aplicativo. Na maioria dos casos, esse comportamento não fazer a diferença no valor de retorno. Essa alteração foi feita para corrigir determinados cenários Ajax. Por exemplo, considere o exemplo de código a seguir: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Esse código anteriormente deve resolver para Images/Logo.jpg, que seria incorreto para uma solicitação Ajax para a página. Agora, ela será resolvida para a raiz das (/ MySite/Images/Logo.jpg).
- **O método HttpContext.RedirectLocal alterado**. Agora, esse método aceita apenas as URLs são relativas ao aplicativo atual. URLs totalmente qualificadas são rejeitadas.
- **O método ModelState agora requer que você chame validar primeiro**. Se você estiver convertendo seu aplicativo para usar os novos métodos de validação de entrada e está chamando o *ModelState* método, você deve chamar agora *Validation.Validate* com antecedência. Por exemplo, agora você deve seguir esse padrão: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 No entanto, é recomendável que se você usar os novos métodos de validação de entrada, não use *ModelState*. Estrutura em vez disso, seu código como este: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **No Internet Explorer 7 e no Internet Explorer 8, a validação do lado do cliente não funciona**. Validação do lado do cliente não funciona devido a incompatibilidades com o jQuery 1.6.2, que é incluído com o modelo de projeto padrão. (A validação do lado do servidor funciona.).

#### <a id="_Toc303701289"></a>Isenção de responsabilidade

© Microsoft Corporation. de 2011. Todos os direitos reservados. Este documento é fornecido "como-é." Informações e opiniões expressadas neste documento, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio. Você assume o risco de utilizá-las.
