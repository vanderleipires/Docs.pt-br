---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: O que há de novo no ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385694"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="457f2-102">O que há de novo no ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="457f2-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="457f2-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="457f2-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="457f2-104">Este tópico descreve o que há de novo para ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="457f2-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="457f2-105">Baixar</span><span class="sxs-lookup"><span data-stu-id="457f2-105">Download</span></span>](#download)
- [<span data-ttu-id="457f2-106">Documentação</span><span class="sxs-lookup"><span data-stu-id="457f2-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="457f2-107">Novos recursos na API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="457f2-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="457f2-108">Tratamento de erro global</span><span class="sxs-lookup"><span data-stu-id="457f2-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="457f2-109">Aprimoramentos de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="457f2-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="457f2-110">Melhorias na página de ajuda</span><span class="sxs-lookup"><span data-stu-id="457f2-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="457f2-111">Suporte de IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="457f2-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="457f2-112">Formatador de tipo de mídia BSON</span><span class="sxs-lookup"><span data-stu-id="457f2-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="457f2-113">Melhor suporte para filtros assíncronos</span><span class="sxs-lookup"><span data-stu-id="457f2-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="457f2-114">Consulta de análise para o cliente de biblioteca de formatação</span><span class="sxs-lookup"><span data-stu-id="457f2-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="457f2-115">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="457f2-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="457f2-116">Correções de bugs</span><span class="sxs-lookup"><span data-stu-id="457f2-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="457f2-117">Baixar</span><span class="sxs-lookup"><span data-stu-id="457f2-117">Download</span></span>

<span data-ttu-id="457f2-118">Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet.</span><span class="sxs-lookup"><span data-stu-id="457f2-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="457f2-119">Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação.</span><span class="sxs-lookup"><span data-stu-id="457f2-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="457f2-120">O pacote mais recente do RTM do ASP.NET Web API 2.1 tem a seguinte versão: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="457f2-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="457f2-121">Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="457f2-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="457f2-122">A versão também inclui pacotes localizados correspondentes no NuGet.</span><span class="sxs-lookup"><span data-stu-id="457f2-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="457f2-123">Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:</span><span class="sxs-lookup"><span data-stu-id="457f2-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="457f2-124">Documentação</span><span class="sxs-lookup"><span data-stu-id="457f2-124">Documentation</span></span>

<span data-ttu-id="457f2-125">Tutoriais e outras informações sobre o RTM do ASP.NET Web API 2.1 estão disponíveis no site da web do ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="457f2-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="457f2-126">Novos recursos na API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="457f2-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="457f2-127">Tratamento de erro global</span><span class="sxs-lookup"><span data-stu-id="457f2-127">Global Error Handling</span></span>

<span data-ttu-id="457f2-128">Todas as exceções sem tratamento agora podem ser registradas em log por meio de um mecanismo central e o comportamento para exceções não tratadas pode ser personalizado.</span><span class="sxs-lookup"><span data-stu-id="457f2-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="457f2-129">O framework oferece suporte a vários agentes de exceção, que consulte a exceção sem tratamento e informações sobre o contexto no qual ele ocorreu, como a solicitação sendo processada no momento.</span><span class="sxs-lookup"><span data-stu-id="457f2-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="457f2-130">Por exemplo, o código a seguir usa System.Diagnostics.TraceSource para registrar todas as exceções sem tratamento:</span><span class="sxs-lookup"><span data-stu-id="457f2-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="457f2-131">Você também pode substituir o manipulador de exceção padrão, para que você pode personalizar totalmente a mensagem de resposta HTTP é enviada quando uma exceção sem tratamento ocorre.</span><span class="sxs-lookup"><span data-stu-id="457f2-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="457f2-132">Nós fornecemos uma [amostra](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) que registra em log exceções não tratadas por meio da estrutura do ELMAH popular.</span><span class="sxs-lookup"><span data-stu-id="457f2-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="457f2-133">Aprimoramentos de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="457f2-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="457f2-134">Roteamento de atributo agora dá suporte a restrições, habilitar o controle de versão e a seleção de rota baseada em cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="457f2-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="457f2-135">Além disso, muitos aspectos de rotas de atributo agora são personalizáveis por meio de **IDirectRouteFactory** interface e **RouteFactoryAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="457f2-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="457f2-136">O prefixo da rota agora é extensível por meio de **IRoutePrefix** interface e **RoutePrefixAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="457f2-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="457f2-137">Nós fornecemos uma [amostra](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) que usa as restrições para filtrar dinamicamente controladores por um cabeçalho HTTP 'api-version'.</span><span class="sxs-lookup"><span data-stu-id="457f2-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="457f2-138">Melhorias na página de ajuda</span><span class="sxs-lookup"><span data-stu-id="457f2-138">Help Page Improvements</span></span>

<span data-ttu-id="457f2-139">API Web 2.1 inclui os seguintes aprimoramentos à [páginas de Ajuda da API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="457f2-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="457f2-140">Documentação das propriedades individuais de parâmetros ou tipos de retorno de ações.</span><span class="sxs-lookup"><span data-stu-id="457f2-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="457f2-141">Documentação do modelo de anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="457f2-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="457f2-142">O design de interface do usuário das páginas de ajuda também foi atualizado, para acomodar essas alterações.</span><span class="sxs-lookup"><span data-stu-id="457f2-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="457f2-143">Suporte de IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="457f2-143">IgnoreRoute Support</span></span>

<span data-ttu-id="457f2-144">Web fornece suporte de API 2.1 a ignorando padrões de URL no roteamento de API da Web, por meio de um conjunto de **IgnoreRoute** métodos de extensão nas **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="457f2-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="457f2-145">Esses métodos fazem com que a API da Web ignorar todas as URLs que correspondam a um modelo especificado e permitir que o host aplicar o processamento adicional, se apropriado.</span><span class="sxs-lookup"><span data-stu-id="457f2-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="457f2-146">O exemplo a seguir ignora os URIs que começam com um &quot;conteúdo&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="457f2-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="457f2-147">Formatador de tipo de mídia BSON</span><span class="sxs-lookup"><span data-stu-id="457f2-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="457f2-148">Web API agora é compatível com o [BSON](http://bsonspec.org/) formato de conexão no cliente e no servidor.</span><span class="sxs-lookup"><span data-stu-id="457f2-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="457f2-149">Para habilitar o BSON no lado do servidor, adicione a **BsonMediaTypeFormatter** à coleção de formatadores:</span><span class="sxs-lookup"><span data-stu-id="457f2-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="457f2-150">Aqui está como um cliente .NET pode consumir formato BSON:</span><span class="sxs-lookup"><span data-stu-id="457f2-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="457f2-151">Nós fornecemos uma [amostra](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) que mostra o lado do cliente e no servidor.</span><span class="sxs-lookup"><span data-stu-id="457f2-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="457f2-152">Para obter mais informações, consulte [suporte a BSON na Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="457f2-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="457f2-153">Melhor suporte para filtros assíncronos</span><span class="sxs-lookup"><span data-stu-id="457f2-153">Better Support for Async Filters</span></span>

<span data-ttu-id="457f2-154">API da Web agora dá suporte a uma maneira fácil de criar filtros que são executadas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="457f2-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="457f2-155">Esse recurso é útil é o seu filtro precisa executar uma ação assíncrona, como o acesso um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="457f2-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="457f2-156">Anteriormente, para criar um filtro assíncrono, você precisava implementar a interface de filtro por conta própria, porque as classes base do filtro exposto somente métodos síncronos.</span><span class="sxs-lookup"><span data-stu-id="457f2-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="457f2-157">Agora você pode substituir o virtual `On*Async` métodos do filtro de classe de base.</span><span class="sxs-lookup"><span data-stu-id="457f2-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="457f2-158">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="457f2-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="457f2-159">O **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** todas classes dão suporte a async no Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="457f2-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="457f2-160">Consulta de análise para o cliente de biblioteca de formatação</span><span class="sxs-lookup"><span data-stu-id="457f2-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="457f2-161">Anteriormente, **Formatting** de análise e consultas URI para o código do lado do servidor de atualização com suporte, mas a biblioteca portátil equivalente não tinha esse recurso.</span><span class="sxs-lookup"><span data-stu-id="457f2-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="457f2-162">2.1 de API da Web, um aplicativo cliente pode agora facilmente analisar e atualizar uma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="457f2-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="457f2-163">Os exemplos a seguir mostram como analisar, modificar e gerar consultas URI.</span><span class="sxs-lookup"><span data-stu-id="457f2-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="457f2-164">(Os exemplos mostram um aplicativo de console para simplificar.)</span><span class="sxs-lookup"><span data-stu-id="457f2-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="457f2-165">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="457f2-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="457f2-166">Esta seção descreve problemas conhecidos e alterações significativas no RTM do 2.1 a ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="457f2-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="457f2-167">Roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="457f2-167">Attribute Routing</span></span>

<span data-ttu-id="457f2-168">Ambiguidades em correspondências de roteamento de atributo relatam um erro em vez de escolher a primeira correspondência.</span><span class="sxs-lookup"><span data-stu-id="457f2-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="457f2-169">Rotas de atributo estão proibidas de usar o *{controller}* parâmetro e usar o *{action}* colocado de parâmetro nas rotas em ações.</span><span class="sxs-lookup"><span data-stu-id="457f2-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="457f2-170">Esses parâmetros muito provavelmente faria com que as ambiguidades.</span><span class="sxs-lookup"><span data-stu-id="457f2-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="457f2-171">O scaffolding de API da Web do MVC em um projeto com resultados de pacotes 5.1 em 5.0 pacotes para aqueles que já não existem no projeto</span><span class="sxs-lookup"><span data-stu-id="457f2-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="457f2-172">Atualizando pacotes do NuGet para o RTM do ASP.NET Web API 2.1 não atualiza as ferramentas do Visual Studio, como o scaffolding do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="457f2-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="457f2-173">Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (Version=5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="457f2-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="457f2-174">Como resultado, o scaffolding do ASP.NET instalará a versão anterior (Version=5.0.0.0) dos pacotes necessários, se eles já não estão disponíveis em seus projetos.</span><span class="sxs-lookup"><span data-stu-id="457f2-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="457f2-175">No entanto, o scaffolding do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos.</span><span class="sxs-lookup"><span data-stu-id="457f2-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="457f2-176">Se você usar o scaffolding do ASP.NET depois de atualizar os pacotes para a Web API 2.1 ou ASP.NET MVC 5.1, verifique se que as versões de API da Web e o MVC estão consistentes.</span><span class="sxs-lookup"><span data-stu-id="457f2-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="457f2-177">Renomeações de tipo</span><span class="sxs-lookup"><span data-stu-id="457f2-177">Type Renames</span></span>

<span data-ttu-id="457f2-178">Alguns dos tipos usados para extensibilidade de roteamento de atributo foram renomeadas do RC para RTM 2.1.</span><span class="sxs-lookup"><span data-stu-id="457f2-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="457f2-179">Nome do tipo antigo (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="457f2-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="457f2-180">Novo tipo de nome (RTM 2.1)</span><span class="sxs-lookup"><span data-stu-id="457f2-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="457f2-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="457f2-181">IDirectRouteProvider</span></span> | <span data-ttu-id="457f2-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="457f2-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="457f2-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="457f2-183">RouteProviderAttribute</span></span> | <span data-ttu-id="457f2-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="457f2-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="457f2-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="457f2-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="457f2-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="457f2-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="457f2-187">Filtros de exceção não desencapsular agregar exceções geradas em ações assíncronas</span><span class="sxs-lookup"><span data-stu-id="457f2-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="457f2-188">Anteriormente, se uma ação de async lançou um **AggregateException**, um filtro de exceção seria desencapsular a exceção, e **OnException** obteria a exceção de base.</span><span class="sxs-lookup"><span data-stu-id="457f2-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="457f2-189">2.1, o filtro de exceção não desencapsular, e **OnException** obtém original **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="457f2-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="457f2-190">Correções de Bug</span><span class="sxs-lookup"><span data-stu-id="457f2-190">Bug Fixes</span></span>

<span data-ttu-id="457f2-191">Esta versão também inclui várias correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="457f2-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="457f2-192">Você pode encontrar a lista completa aqui:</span><span class="sxs-lookup"><span data-stu-id="457f2-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="457f2-193">5.1.0 pacote</span><span class="sxs-lookup"><span data-stu-id="457f2-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="457f2-194">5.1.1 pacote de</span><span class="sxs-lookup"><span data-stu-id="457f2-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="457f2-195">O 5.1.2 pacote contém as atualizações do IntelliSense, mas não há correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="457f2-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
