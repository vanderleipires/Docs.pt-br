---
title: Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.x e posterior
author: Rick-Anderson
description: O metapacote Microsoft.AspNetCore.All inclui todos os pacotes do ASP.NET Core e Entity Framework Core compatíveis, juntamente com suas dependências.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.x

Este recurso exige o ASP.NET Core 2.x direcionado ao .NET Core 2.x.

O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:

* Todos os pacotes com suporte da equipe do ASP.NET Core.
* Todos os pacotes com suporte pelo Entity Framework Core. 
* Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core. 

Todos os recursos do ASP.NET Core 2.x e do Entity Framework Core 2.x são incluídos no pacote `Microsoft.AspNetCore.All`. Os modelos de projeto padrão direcionados para ASP.NET Core 2.0 usam este pacote.

O número de versão do metapacote `Microsoft.AspNetCore.All` representa a versão do ASP.NET Core e a versão do Entity Framework Core (alinhadas com a versão do .NET Core).

Aplicativos que usam o metapacote `Microsoft.AspNetCore.All` aproveitam automaticamente o novo [Repositório de Tempo de Execução do .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). O Repositório de Tempo de Execução contém todos os ativos de tempo de execução necessários para executar aplicativos ASP.NET Core 2.x. Quando você usa o metapacote `Microsoft.AspNetCore.All`, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo &mdash;, porque o Repositório de Tempo de Execução do .NET Core contém esse ativos. Os ativos no Repositório de Tempo de Execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.

Use o processo de filtragem de pacote para remover os pacotes não utilizados. Pacotes filtrados são excluídos na saída do aplicativo publicado.

O seguinte arquivo *.csproj* referencia os metapacotes `Microsoft.AspNetCore.All` para o ASP.NET Core:

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
