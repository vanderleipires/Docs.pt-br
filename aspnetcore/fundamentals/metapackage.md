---
title: Metapackage Microsoft.AspNetCore.All para ASP.NET Core 2. x e posterior
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage inclui todos os pacotes.
keywords: ASP.NET Core, o NuGet, pacote, Microsoft.AspNetCore.All, metapackage
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapackage Microsoft.AspNetCore.All para ASP.NET Core 2. x

Este recurso requer o ASP.NET Core 2. x.

O [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage para ASP.NET Core inclui:

* Todos os pacotes com suporte pela equipe do ASP.NET Core.
* Todos os pacotes, o Entity Framework Core, com suporte. 
* Dependências internas e de terceiros 3º usadas por ASP.NET Core e o Entity Framework Core. 

Todos os recursos do ASP.NET Core 2. x e o Entity Framework Core 2. x são incluídos no `Microsoft.AspNetCore.All` pacote. Os modelos de projeto padrão usam este pacote.

O número de versão de `Microsoft.AspNetCore.All` metapackage representa a versão do ASP.NET Core e a versão do Entity Framework Core (alinhado com a versão do .NET Core).

Aplicativos que usam o `Microsoft.AspNetCore.All` metapackage automaticamente tirar proveito do repositório de tempo de execução do .NET Core. O repositório de tempo de execução contém todos os ativos de tempo de execução necessários para executar o ASP.NET Core aplicativos 2. x. Quando você usa o `Microsoft.AspNetCore.All` metapackage, **sem** ativos dos pacotes do NuGet do ASP.NET Core referenciados são implantados com o aplicativo &mdash; o repositório de tempo de execução do .NET Core contém esses ativos. <!-- todo add link to Runtime store -->Os ativos no repositório de tempo de execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.

Você pode usar o processo de remoção de pacote para remover os pacotes que não são usadas. Pacotes cortados são excluídos na saída do aplicativo publicado.

O seguinte *. csproj* referências de arquivo a `Microsoft.AspNetCore.All` metapackage para ASP.NET Core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
