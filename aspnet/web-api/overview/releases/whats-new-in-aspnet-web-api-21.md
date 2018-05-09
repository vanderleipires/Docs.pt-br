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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>O que há de novo no ASP.NET Web API 2.1
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve quais são as novidades do ASP.NET Web API 2.1.

- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos no ASP.NET Web API 2.1](#new-features)

    - [Tratamento de erro global](#global-error)
    - [Aprimoramentos de roteamentos de atributo](#attribute-routing)
    - [Aprimoramentos da página de ajuda](#help-page)
    - [Suporte de IgnoreRoute](#ignoreroute)
    - [Formatador de tipo de mídia BSON](#bson)
    - [Melhor suporte para filtros de Async](#async-filters)
    - [Consulta de análise para o cliente de biblioteca de formatação](#query-parsing)
- [Problemas conhecidos e as alterações recentes](#known-issues)
- [Correções de bugs](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes do NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente do RTM do ASP.NET Web API 2.1 tem a seguinte versão: "5.1.2". Você pode instalar ou atualizar esses pacotes por meio de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados usando o NuGet Package Manager Console:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre RTM do ASP.NET Web API 2.1 estão disponíveis no site da web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Novos recursos no ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Tratamento de erro global

Todas as exceções sem tratamento agora podem ser registradas em log por meio de um mecanismo central e o comportamento para exceções não tratadas pode ser personalizado.

A estrutura oferece suporte a diversos agentes de exceção, que consulte a exceção sem tratamento e informações sobre o contexto no qual ele ocorreu, como a solicitação que está sendo processada no momento.

Por exemplo, o código a seguir usa System.Diagnostics.TraceSource para registrar todas as exceções sem tratamento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Você também pode substituir o manipulador de exceção padrão, para que você pode personalizar totalmente a mensagem de resposta HTTP é enviada quando uma exceção sem tratamento ocorre.

Fornecemos um [exemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) que registra todos os não manipulada por meio da estrutura ELMAH popular.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamentos de atributo

Roteamento de atributo agora dá suporte a restrições, habilitar o controle de versão e seleção de rota com base no cabeçalho. Além disso, muitos aspectos de rotas de atributo agora são personalizáveis por meio de **IDirectRouteFactory** interface e **RouteFactoryAttribute** classe. O prefixo da rota agora é extensível via o **IRoutePrefix** interface e **RoutePrefixAttribute** classe.

Fornecemos um [exemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) que usa restrições para filtrar dinamicamente controladores por um cabeçalho HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Aprimoramentos da página de ajuda

Web API 2.1 inclui os seguintes aprimoramentos para [páginas de Ajuda do API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentação de propriedades individuais de parâmetros ou tipos de retorno de ações.
- Documentação de anotações do modelo de dados.

O design de interface do usuário das páginas de ajuda também foi atualizado, para acomodar essas alterações.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Suporte de IgnoreRoute

Web API 2.1 oferece suporte a ignorando padrões de URL no roteamento de API da Web, por meio de um conjunto de **IgnoreRoute** métodos de extensão em **HttpRouteCollection**. Esses métodos fazer com que a API da Web ignorar todas as URLs que correspondem a um modelo especificado e permitir que o host aplicar o processamento adicional, se apropriado.

O exemplo a seguir ignora os URIs que começam com um &quot;conteúdo&quot; segmento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formatador de tipo de mídia BSON

Web API agora dá suporte ao [BSON](http://bsonspec.org/) formato de conexão no cliente e no servidor.

Para habilitar o BSON no lado do servidor, adicione o **BsonMediaTypeFormatter** à coleção de formatadores:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Aqui está como um cliente .NET pode consumir formato BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Fornecemos um [exemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) que mostra do lado do cliente e servidor.

Para obter mais informações, consulte [suporte BSON 2.1 de API da Web](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Melhor suporte para filtros de Async

API da Web agora dá suporte a uma maneira fácil de criar filtros que são executadas de forma assíncrona. Esse recurso é útil é o filtro precisa executar uma ação assíncrona, como o acesso um banco de dados. Anteriormente, para criar um filtro de assíncrono, era necessário implementar a interface de filtro por conta própria, porque as classes de base de filtro só expostas métodos síncronos. Agora você pode substituir virtual `On*Async` métodos do filtro de classe de base.

Por exemplo:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

O **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** todas as classes de suportam assíncrono 2.1 de API da Web.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Consulta de análise para o cliente de biblioteca de formatação

Anteriormente, **System.Net.Http.Formatting** análise e consultas URI para o código do lado do servidor de atualização com suporte, mas a biblioteca portátil equivalente não tinha esse recurso. 2.1 de API da Web, um aplicativo cliente pode facilmente analisar e atualizar uma cadeia de caracteres de consulta.

Os exemplos a seguir mostram como analisar, modificar e gerar consultas URI. (Os exemplos mostram um aplicativo de console para simplificar.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

Esta seção descreve problemas conhecidos e as alterações recentes no RTM do 2.1 a ASP.NET Web API.

### <a name="attribute-routing"></a>Roteamento de atributo

Ambiguidades em correspondências de roteamento de atributo agora relatam um erro em vez de escolher a primeira correspondência.

Rotas de atributo são proibidas de usar o *{controlador}* parâmetro e usa o *{action}* parâmetro nas rotas colocadas em ações. Esses parâmetros muito provavelmente causará ambiguidades.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Estrutura MVC/Web API em um projeto com 5.1 resultados de pacotes em 5.0 pacotes para aqueles que já não existe no projeto

Atualizar pacotes do NuGet para RTM do ASP.NET Web API 2.1 não atualizar as ferramentas do Visual Studio, como ASP.NET scaffolding ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (5.0.0.0). Como resultado, o ASP.NET scaffolding instalará a versão anterior (5.0.0.0) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, a estrutura do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos.

Se você usar o ASP.NET scaffolding depois de atualizar os pacotes para 2.1 de API da Web ou o ASP.NET MVC 5.1, verifique se que as versões de API da Web e MVC são consistentes.

### <a name="type-renames"></a>Renomeações de tipo

Alguns dos tipos usados para extensibilidade de roteamento de atributo foram renomeadas do RC para RTM 2.1.

| Nome do tipo antigo (2.1 RC) | Novo tipo de nome (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtros de exceção não desencapsular agregação exceções lançadas em ações assíncronas

Anteriormente, se uma ação assíncrona gerou uma **AggregateException**, um filtro de exceção seria decodificar a exceção, e **OnException** obteria a exceção de base. 2.1, o filtro de exceção não unwrap, e **OnException** obtém original **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correções de Bug

Esta versão também inclui várias correções de bug. Você pode encontrar a lista completa aqui:

- [5.1.0 pacote de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacote de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

O 5.1.2 pacote contém atualizações do IntelliSense, mas nenhuma correção de bug.
