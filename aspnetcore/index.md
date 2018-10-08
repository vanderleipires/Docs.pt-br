---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Obtenha uma introdução ao ASP.NET Core, uma estrutura de software livre, plataforma cruzada e alto desempenho para a criação de aplicativos modernos conectados à Internet e baseados em nuvem.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 85f2aa04e4b6692dd03a63f14bcebdec97ee270e
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454772"
---
# <a name="introduction-to-aspnet-core"></a>Introdução ao ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Shaun Luttin](https://twitter.com/dicshaunary)

O ASP.NET Core é uma estrutura de [software livre](https://github.com/aspnet/home), de multiplaforma e alto desempenho para a criação de aplicativos modernos conectados à Internet e baseados em nuvem. Com o ASP.NET Core, você pode:

* Compilar aplicativos e serviços Web, aplicativos [IoT](https://www.microsoft.com/internet-of-things/) e back-ends móveis.
* Usar suas ferramentas de desenvolvimento favoritas no Windows, macOS e Linux.
* Implantar na nuvem ou local.
* Executar no [.NET Core ou no .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Por que usar o ASP.NET Core?

Milhões de desenvolvedores usaram (e continuam usando) o [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) para criar aplicativos Web. O ASP.NET Core é uma reformulação do ASP.NET 4.x, com alterações de arquitetura que resultam em uma estrutura mais enxuta e modular.

O ASP.NET Core oferece os seguintes benefícios:

* Uma história unificada para a criação da interface do usuário da Web e das APIs Web.
* Integração de [estruturas modernas do lado do cliente](xref:client-side/index) e fluxos de trabalho de desenvolvimento.
* Um [sistema de configuração](xref:fundamentals/configuration/index) pronto para a nuvem, baseado no ambiente.
* [Injeção de dependência](xref:fundamentals/dependency-injection) interna.
* Um pipeline de solicitação HTTP leve, modular e de [alto desempenho](https://github.com/aspnet/benchmarks).
* A capacidade de hospedar no [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index) ou hospedar em seu próprio processo.
* Controle de versão do aplicativo do lado a lado ao direcionar [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
* Ferramentas que simplificam o moderno desenvolvimento para a Web.
* A capacidade de criar e executar no Windows, macOS e Linux.
* De software livre e [voltado para a comunidade](https://live.asp.net/).

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilar APIs Web e uma interface do usuário da Web usando o ASP.NET Core MVC

O ASP.NET Core MVC fornece recursos que ajudam você a compilar [APIs Web](xref:tutorials/index#build-web-apis) e [aplicativos Web](xref:tutorials/index#build-web-apps):

* O [padrão MVC (Model-View-Controller)](xref:mvc/overview) ajuda a tornar as APIs Web e os aplicativos Web [testáveis](xref:test/index).
* As [Páginas Razor](xref:razor-pages/index) (novidade no ASP.NET Core 2.0) é um modelo de programação baseado em página que torna a criação da interface do usuário da Web mais fácil e produtiva.
* A [marcação Razor](xref:mvc/views/razor) fornece uma sintaxe produtiva para [Páginas Razor](xref:razor-pages/index) e as [Exibições do MVC](xref:mvc/views/overview).
* Os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro) permitem que o código do servidor participe da criação e renderização de elementos HTML em arquivos do Razor.
* O suporte interno para [vários formatos de dados e negociação de conteúdo](xref:web-api/advanced/formatting) permite que as APIs Web alcancem uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.
* O [model binding](xref:mvc/models/model-binding) mapeia automaticamente os dados de solicitações HTTP para os parâmetros de método de ação.
* A [Validação de Modelos](xref:mvc/models/validation) executa automaticamente a validação no lado do cliente e do servidor.

## <a name="client-side-development"></a>Desenvolvimento do lado do cliente

ASP.NET Core integra-se perfeitamente com estruturas conhecidas do lado do cliente e bibliotecas, incluindo [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](xref:client-side/bootstrap). Para saber mais, consulte [Desenvolvimento do lado do cliente](xref:client-side/index).

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core direcionado para o .NET Framework

O ASP.NET Core pode ser direcionado para o .NET Core ou ao .NET Framework. Os aplicativos do ASP.NET Core direcionados ao .NET Framework não são multiplataforma,&mdash; são executados somente no Windows. Não existem planos para interromper o suporte ao direcionamento para .NET Framework no ASP.NET Core. Em geral, o ASP.NET Core é composto de bibliotecas do [.NET Standard](/dotnet/standard/net-standard). Aplicativos criados com o .NET Standard 2.0 são executados em qualquer lugar com suporte para ele.

Há várias vantagens em direcionar para o .NET Core, e essas vantagens aumentam com cada versão. Algumas vantagens do .NET Core em relação ao .NET Framework incluem:

* Multiplataforma. É executado no Windows, no Linux e no macOS.
* Desempenho aprimorado
* Controle de versão lado a lado
* Novas APIs
* Código Aberto

Estamos trabalhando duro para diminuir a diferença de API entre o .NET Framework e o .NET Core. O [Pacote de Compatibilidade do Windows](/dotnet/core/porting/windows-compat-pack) disponibilizou milhares de APIs exclusivas do Windows no .NET Core. Essas APIs não estavam disponíveis no .NET Core 1.x.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Tutoriais do ASP.NET Core](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Conceitos básicos do ASP.NET Core](xref:fundamentals/index)
* O [Community Standup semanal do ASP.NET](https://live.asp.net/) aborda o progresso e os planos da equipe. Ele apresenta o novo software de terceiros e blogs.
