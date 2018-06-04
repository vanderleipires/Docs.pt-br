---
title: Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.0 e posterior
author: Rick-Anderson
description: O metapacote Microsoft.AspNetCore.All inclui todos os pacotes do ASP.NET Core e Entity Framework Core compatíveis, juntamente com suas dependências.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: fbb76f41f3178ddc4e51faa14edece1869a30cd0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729071"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.0

> [!NOTE]
> Recomendamos que os aplicativos voltados para o ASP.NET Core 2.1 e posterior usem o [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) em vez deste pacote. Veja [Migração do Microsoft.AspNetCore.All para Microsoft.AspNetCore.App](#migrate) neste artigo.

Este recurso exige o ASP.NET Core 2.x direcionado ao .NET Core 2.x.

O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:

* Todos os pacotes com suporte da equipe do ASP.NET Core.
* Todos os pacotes com suporte pelo Entity Framework Core. 
* Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core. 

Todos os recursos do ASP.NET Core 2.x e do Entity Framework Core 2.x são incluídos no pacote `Microsoft.AspNetCore.All`. Os modelos de projeto padrão direcionados para ASP.NET Core 2.0 usam este pacote.

O número de versão do metapacote `Microsoft.AspNetCore.All` representa a versão do ASP.NET Core e a versão do Entity Framework Core.

Aplicativos que usam o metapacote `Microsoft.AspNetCore.All` aproveitam automaticamente o novo [Repositório de Tempo de Execução do .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). O Repositório de Tempo de Execução contém todos os ativos de tempo de execução necessários para executar aplicativos ASP.NET Core 2.x. Quando você usa o metapacote `Microsoft.AspNetCore.All`, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo &mdash;, porque o Repositório de Tempo de Execução do .NET Core contém esse ativos. Os ativos no Repositório de Tempo de Execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.

Use o processo de filtragem de pacote para remover os pacotes não utilizados. Pacotes filtrados são excluídos na saída do aplicativo publicado.

O seguinte arquivo *.csproj* referencia os metapacotes `Microsoft.AspNetCore.All` para o ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migração do Microsoft.AspNetCore.All para Microsoft.AspNetCore.App

Os pacotes a seguir estão incluídos no `Microsoft.AspNetCore.All`, mas não no pacote `Microsoft.AspNetCore.App`. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Para mover de `Microsoft.AspNetCore.All` para `Microsoft.AspNetCore.App`, se seu aplicativo usa APIs dos pacotes acima ou de pacotes trazidos por eles, adicione referências a eles em seu projeto.

Todas as dependências dos pacotes anteriores que, de outra forma, não são dependências de `Microsoft.AspNetCore.App` não são incluídas implicitamente. Por exemplo:

* `StackExchange.Redis` como uma dependência de `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` como uma dependência de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
