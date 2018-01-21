---
title: "Migrando configuração"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 90d9f730d31c2c70aec3d47610b9031a7d8e621b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-configuration"></a><span data-ttu-id="e8a7c-102">Migrando configuração</span><span class="sxs-lookup"><span data-stu-id="e8a7c-102">Migrating Configuration</span></span>

<span data-ttu-id="e8a7c-103">Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="e8a7c-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="e8a7c-104">O artigo anterior, começamos [migrando um projeto ASP.NET MVC para MVC do ASP.NET Core](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="e8a7c-104">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="e8a7c-105">Neste artigo, vamos migrar a configuração.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-105">In this article, we migrate configuration.</span></span>

<span data-ttu-id="e8a7c-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e8a7c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="e8a7c-107">Configuração da instalação</span><span class="sxs-lookup"><span data-stu-id="e8a7c-107">Setup Configuration</span></span>

<span data-ttu-id="e8a7c-108">ASP.NET Core não usa mais o *global. asax* e *Web. config* arquivos utilizadas de versões anteriores do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-108">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="e8a7c-109">Em versões anteriores do ASP.NET, a lógica de inicialização do aplicativo foi colocada em uma `Application_StartUp` método *global. asax*.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-109">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="e8a7c-110">Posteriormente, no ASP.NET MVC, um *Startup.cs* arquivo foi incluído na raiz do projeto; e ele foi chamado quando o aplicativo foi iniciado.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-110">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="e8a7c-111">ASP.NET Core adotou essa abordagem completamente colocando toda lógica de inicialização no *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-111">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="e8a7c-112">O *Web. config* arquivo também foi substituído no núcleo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-112">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="e8a7c-113">Configuração propriamente dita agora pode ser configurada como parte do procedimento de inicialização de aplicativo descrito em *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-113">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="e8a7c-114">Configuração ainda pode utilizar para arquivos XML, mas normalmente projetos do ASP.NET Core colocará os valores de configuração em um arquivo no formato JSON, como *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-114">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="e8a7c-115">Sistema de configuração do ASP.NET Core pode acessar facilmente as variáveis de ambiente, que podem fornecer um local mais seguro e robusto para valores específicos do ambiente.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-115">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="e8a7c-116">Isso é especialmente verdadeiro para segredos, como cadeias de caracteres de conexão e chaves de API que não devem ser verificadas no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-116">This is especially true for secrets like connection strings and API keys that should not be checked into source control.</span></span> <span data-ttu-id="e8a7c-117">Consulte [configuração](xref:fundamentals/configuration/index) para saber mais sobre a configuração no núcleo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-117">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="e8a7c-118">Neste artigo, estamos começando com o projeto do ASP.NET Core parcialmente migrados de [artigo anterior](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="e8a7c-118">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="e8a7c-119">Para configurar a configuração, adicione o seguinte construtor e propriedade para o *Startup.cs* arquivo localizado na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="e8a7c-119">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="e8a7c-120">Observe que neste ponto, o *Startup.cs* arquivo não será compilado, pois precisamos adicionar o seguinte `using` instrução:</span><span class="sxs-lookup"><span data-stu-id="e8a7c-120">Note that at this point, the *Startup.cs* file will not compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="e8a7c-121">Adicionar uma *appSettings. JSON* arquivo para a raiz do projeto usando o modelo de item apropriado:</span><span class="sxs-lookup"><span data-stu-id="e8a7c-121">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Adicionar AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="e8a7c-123">Migrar configurações de Web. config</span><span class="sxs-lookup"><span data-stu-id="e8a7c-123">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="e8a7c-124">Nosso projeto ASP.NET MVC incluído a cadeia de caracteres de conexão de banco de dados necessários no *Web. config*, além de `<connectionStrings>` elemento.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-124">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="e8a7c-125">Em nosso projeto do ASP.NET Core, vamos armazenar essas informações no *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-125">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="e8a7c-126">Abra *appSettings. JSON*e observe que ele já inclui o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e8a7c-126">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="e8a7c-127">Na linha realçada descrita acima, altere o nome do banco de dados **_CHANGE_ME** para o nome do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-127">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="e8a7c-128">Resumo</span><span class="sxs-lookup"><span data-stu-id="e8a7c-128">Summary</span></span>

<span data-ttu-id="e8a7c-129">ASP.NET Core coloca toda a lógica de inicialização para o aplicativo em um único arquivo, no qual os serviços necessários e dependências podem ser definidas e configuradas.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-129">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="e8a7c-130">Ele substitui o *Web. config* arquivo com um recurso de configuração flexíveis que pode aproveitar uma variedade de formatos de arquivo, como JSON, bem como variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="e8a7c-130">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
