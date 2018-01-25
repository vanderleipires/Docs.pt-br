---
title: "Introdução ao ASP.NET Core"
author: rick-anderson
description: Apresenta o ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 7d5afd5871316509a385d14bbfe886bfe55d1ff4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
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

O ASP.NET Core é fornecido inteiramente como pacotes [NuGet](https://www.nuget.org/). Isso permite otimizar o aplicativo para incluir apenas os pacotes NuGet necessários. Na verdade, aplicativos do ASP.NET Core 2.x direcionando o .NET Core só exigem um [único pacote de NuGet](xref:fundamentals/metapackage). Os benefícios de uma área de superfície menor do aplicativo incluem maior segurança, manutenção reduzida e desempenho aprimorado.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilar APIs Web e uma interface do usuário da Web usando o ASP.NET Core MVC

O ASP.NET Core MVC fornece recursos que ajudam você a compilar [APIs Web](xref:tutorials/index#build-web-apis) e [aplicativos Web](xref:tutorials/index#build-web-apps):

* O [padrão MVC (Model-View-Controller)](xref:mvc/overview) ajuda a tornar as APIs Web e os aplicativos Web [testáveis](testing/index.md).
* As [Páginas Razor](xref:mvc/razor-pages/index) (novidade no ASP.NET Core 2.0) é um modelo de programação baseado em página que torna a criação da interface do usuário da Web mais fácil e produtiva.
* A [marcação Razor](xref:mvc/views/razor) fornece uma sintaxe produtiva para [Páginas Razor](xref:mvc/razor-pages/index) e as [Exibições do MVC](xref:mvc/views/overview).
* Os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro) permitem que o código do servidor participe da criação e renderização de elementos HTML em arquivos do Razor.
* O suporte interno para [vários formatos de dados e negociação de conteúdo](mvc/models/formatting.md) permite que as APIs Web alcancem uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.
* A [Associação de Modelos](xref:mvc/models/model-binding) mapeia automaticamente os dados de solicitações HTTP para os parâmetros de método de ação.
* A [Validação de Modelos](xref:mvc/models/validation) executa automaticamente a validação do cliente e do servidor.

## <a name="client-side-development"></a>Desenvolvimento do lado do cliente

ASP.NET Core integra-se perfeitamente com estruturas conhecidas do lado do cliente e bibliotecas, incluindo [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](xref:client-side/bootstrap). Consulte [Desenvolvimento do cliente](xref:client-side/index) para obter mais detalhes.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Tutoriais do ASP.NET Core](xref:tutorials/index)
* [Conceitos básicos do ASP.NET Core](xref:fundamentals/index)
* O [Community Standup semanal do ASP.NET](https://live.asp.net/) aborda o progresso e os planos da equipe. Ele apresenta o novo software de terceiros e blogs.
