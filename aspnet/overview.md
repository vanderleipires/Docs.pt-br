---
uid: overview
title: "Visão geral do ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Introdução ao ASP.NET, uma estrutura gratuita para criar sites, aplicativos web e APIs da web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: ed11c882c801248ffaca95b82f16d23c87fb9be7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-overview"></a>Visão geral do ASP.NET

O ASP.NET é um framework web grátis para compilar ótimos sites e aplicativos da web usando HTML, CSS e JavaScript. Você também pode criar APIs da Web e usar tecnologias em tempo real como soquetes da Web.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) é uma alternativa para o ASP.NET.  Consulte o [orientação sobre como escolher entre ASP.NET e ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Introdução

[Baixe o Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=826064), um IDE é gratuito para ASP.NET no Windows.

## <a name="websites-and-web-applications"></a>Sites e aplicativos web

 O ASP.NET oferece três estruturas para a criação de aplicativos da web: formulários da Web, ASP.NET MVC e páginas da Web ASP.NET. Todas as estruturas de três são estáveis e maduro, e você pode criar aplicativos web grande com qualquer um deles. Independentemente do framework que você escolher, você obterá todos os benefícios e recursos do ASP.NET em qualquer lugar.

Cada estrutura tem como alvo um estilo diferente de desenvolvimento. Um que você escolher depende de uma combinação de seus ativos de programação (dados de Conhecimento, habilidades e experiência de desenvolvimento), o tipo de aplicativo que está criando e você estiver familiarizado com a abordagem de desenvolvimento.

Abaixo está uma visão geral de cada um das estruturas e algumas ideias sobre como escolher entre eles. Se você preferir um vídeo de Introdução, consulte [fazer sites com o ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) e [o que há de ferramentas da Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Se você tiver experiência em | Estilo de desenvolvimento | Experiência | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Windows Forms, WPF, .NET | Desenvolvimento rápido usando uma rica biblioteca de controles que encapsulam a marcação HTML | RAD de nível intermediário, Avançado |
| MVC       | Ruby on Rails .NET  | Controle total sobre a marcação HTML, código e marcação separada e fáceis de escrever os testes. A melhor escolha para aplicativos móveis e de página única (SPA). | Nível intermediário, Avançado |
| Páginas da Web  | Clássico ASP, PHP     | Marcação HTML e seu código junto no mesmo arquivo | Novo, de nível intermediário |

### <a name="web-forms"></a>Web Forms

Com o Web Forms do ASP.NET, você pode criar sites dinâmicos usando um modelo familiar de arrastar e soltar, controlada por evento. Uma superfície de design e centenas de controles e componentes lhe permitem criar rapidamente sofisticados, poderosos sites baseados em UI com acesso a dados. 

[Saiba mais sobre o Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC fornece uma maneira eficiente com base em padrões para criar sites dinâmicos que habilitam uma separação limpa de preocupações e que fornece a você controle total sobre a marcação de desenvolvimento ágil e divertido. O ASP.NET MVC inclui muitos recursos que permitem o desenvolvimento rápido e amigável a TDD para criar aplicativos sofisticados que usam os últimos padrões da web. 

[Saiba mais sobre o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Páginas da Web do ASP.NET

Páginas da Web ASP.NET e a sintaxe do Razor fornecem uma maneira rápida, acessível e leve de combinar código de servidor com HTML para criar conteúdo dinâmico da web. Conectar-se a bancos de dados, adicione vídeo, vincular a sites de rede social e incluem muitos mais recursos que ajudarão a criam lindos sites que estão em conformidade com os padrões da web mais recentes.

[Saiba mais sobre páginas da Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Observações sobre o Web Forms, MVC e páginas da Web

Todas as estruturas ASP.NET três são baseadas no .NET Framework e compartilham a funcionalidade de núcleo do .NET e do ASP.NET. Por exemplo, todas as estruturas de três oferecem um modelo de segurança de logon com base em torno de associação e todos os três compartilham os mesmo recursos para gerenciar solicitações, tratamento de sessões e assim por diante que fazem parte da funcionalidade do ASP.NET principal.

Além disso, as estruturas de três não são totalmente independentes e escolhendo uma não impede o uso de outro. Como as estruturas podem coexistir no mesmo aplicativo web, não é incomum ver componentes individuais de aplicativos escritos usando estruturas diferentes. Por exemplo, voltados para o cliente de partes de um aplicativo podem ser desenvolvidas MVC para otimizar a marcação, enquanto o acesso a dados e partes administrativas são desenvolvidas em Web Forms para tirar proveito dos controles de dados e acesso a dados simples.

## <a name="web-apis"></a>APIs da Web

API da Web do ASP.NET é uma estrutura que torna mais fácil criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. A API do ASP.NET Web é uma plataforma ideal para criar aplicativos com REST no .NET Framework.

[Saiba mais sobre a API da Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologias de tempo real

ASP.NET SignalR é uma nova biblioteca para desenvolvedores do ASP.NET que facilita a funcionalidade de desenvolvimento da web em tempo real. SignalR permite a comunicação bidirecional entre servidor e cliente. Servidores podem enviar conteúdos para clientes conectados imediatamente assim que possível. SignalR suporta soquetes da Web e reverterá para outras técnicas compatíveis para navegadores mais antigos. SignalR inclui APIs de gerenciamento de conexão (por exemplo, conexão e eventos de desconexão), agrupamento de conexões e autorização.

[Saiba mais sobre SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Sites e aplicativos móveis 

ASP.NET pode ligar nativo aplicativos móveis com um back-end de API da Web, bem como móveis sites usando estruturas de design responsivo como Bootstrap do Twitter. Se você estiver criando um aplicativo móvel nativo, é fácil criar uma API da Web com base em JSON para identificador de acesso a dados, autenticação e notificações por push para seu aplicativo. Se você estiver criando um site móvel responsivo, você pode usar qualquer sistema de grade aberta desejado ou selecione um sistema móvel avançado como o jQuery Mobile ou Sencha e ótimos aplicativos móveis com o PhoneGap ou estrutura CSS.

[Saiba mais sobre o desenvolvimento de aplicativo e do site móvel](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplicativos de única página 

Aplicativo de página ASP.NET única (SPA) ajuda a criar aplicativos que incluem significativas interações de cliente usando HTML 5, 3 de CSS e JavaScript. Visual Studio inclui um modelo para criar aplicativos de única página usando Knockout. js e a API da Web do ASP.NET. Além do modelo SPA interno, os modelos criados pela comunidade SPA também estão disponíveis para download.

[Saiba mais sobre o desenvolvimento de aplicativo de página única](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

WebHooks é um padrão HTTP leve, fornecendo um modelo de publicação/assinatura simples para conectar os serviços de APIs da Web e SaaS. Quando ocorre um evento em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados. A solicitação POST contém informações sobre o evento que torna possível para o receptor funcionar adequadamente.

WebHooks são expostos por um grande número de serviços, incluindo Dropbox, GitHub, Instagram, MailChimp, PayPal, margem de atraso, Trello e muito mais. Por exemplo, um WebHook pode indicar que um arquivo foi alterado no Dropbox, ou uma alteração de código foi confirmada no GitHub, ou um pagamento foi iniciado no PayPal ou um cartão foi criado no Trello.

[Saiba mais sobre WebHooks](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
