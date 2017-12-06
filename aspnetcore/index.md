---
title: "Introdução ao ASP.NET Core"
author: rick-anderson
description: Apresenta o ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a>Introdução ao ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Shaun Luttin](https://twitter.com/dicshaunary)

O ASP.NET Core é uma estrutura de [software livre](https://github.com/aspnet/home), de multiplaforma e alto desempenho para a criação de aplicativos modernos conectados à Internet e baseados em nuvem. Com o ASP.NET Core, você pode:

* Compilar aplicativos e serviços Web, aplicativos [IoT](https://www.microsoft.com/en-us/internet-of-things/) e back-ends móveis.
* Usar suas ferramentas de desenvolvimento favoritas no Windows, macOS e Linux.
* Implantar na nuvem ou local
* Executar no [.NET Core ou no .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Por que usar o ASP.NET Core?

Milhões de desenvolvedores usaram (e continuam usando) o ASP.NET para criar aplicativos Web. O ASP.NET Core é uma reformulação do ASP.NET, com alterações de arquitetura que resultam em uma estrutura mais enxuta e modular.

O ASP.NET Core oferece os seguintes benefícios:

* Uma história unificada para a criação da interface do usuário da Web e das APIs Web.
* Integração de [modernas estruturas do lado do cliente](xref:client-side/index) e fluxos de trabalho de desenvolvimento.
* Um [sistema de configuração](xref:fundamentals/configuration/index) pronto para a nuvem, baseado no ambiente.
* [Injeção de dependência](xref:fundamentals/dependency-injection) interna.
* Um pipeline de solicitação HTTP leve, modular e de alto desempenho.
* A capacidade de hospedar no IIS ou hospedar automaticamente em seu próprio processo.
* Pode ser executado no [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), que dá suporte ao verdadeiro controle de versão do aplicativo lado a lado.
* Ferramentas que simplificam o moderno desenvolvimento para a Web.
* A capacidade de criar e executar no Windows, macOS e Linux.
* De software livre e voltado para a comunidade.

O ASP.NET Core é fornecido inteiramente como pacotes [NuGet](https://www.nuget.org/). Isso permite otimizar o aplicativo para incluir apenas os pacotes NuGet necessários. Os benefícios de uma área de superfície menor do aplicativo incluem maior segurança, manutenção reduzida e desempenho aprimorado.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilar APIs Web e uma interface do usuário da Web usando o ASP.NET Core MVC

O ASP.NET Core MVC fornece recursos que ajudam você a criar [APIs Web](xref:tutorials/index#building-web-apis) e [aplicativos Web](xref:tutorials/index#building-web-applications):

* O [padrão MVC (Model-View-Controller)](xref:mvc/overview) ajuda a tornar as APIs Web e os aplicativos Web [testáveis](testing/index.md).
* As [Páginas Razor](xref:mvc/razor-pages/index) (novidade no 2.0) é um modelo de programação baseado em página que torna a criação da interface do usuário da Web mais fácil e produtiva.
* A [sintaxe Razor](xref:mvc/views/razor) fornece uma linguagem produtiva para as [Páginas Razor](xref:mvc/razor-pages/index) e as [Exibições do MVC](xref:mvc/views/overview).
* Os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro) permitem que o código do servidor participe da criação e renderização de elementos HTML em arquivos do Razor.
* O suporte interno para [vários formatos de dados e negociação de conteúdo](mvc/models/formatting.md) permite que as APIs Web alcancem uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.
* A [Associação de Modelos](xref:mvc/models/model-binding) mapeia automaticamente os dados de solicitações HTTP para os parâmetros de método de ação.
* A [Validação de Modelos](xref:mvc/models/validation) executa automaticamente a validação do cliente e do servidor.

## <a name="client-side-development"></a>Desenvolvimento do lado do cliente

O ASP.NET Core foi projetado para ser integrado perfeitamente a uma variedade de estruturas do lado do cliente, incluindo [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) e [Bootstrap](xref:client-side/bootstrap). Consulte [Desenvolvimento do cliente](client-side/index.md) para obter mais detalhes.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Tutoriais do ASP.NET Core](xref:tutorials/index)
* [Conceitos básicos do ASP.NET Core](xref:fundamentals/index)
* O [Community Standup semanal do ASP.NET](https://live.asp.net/) aborda o progresso e os planos da equipe e apresenta novos blogs e software de terceiros.
