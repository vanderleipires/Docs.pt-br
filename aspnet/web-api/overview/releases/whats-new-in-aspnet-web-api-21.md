---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: O que há de novo no ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838397"
---
<a name="whats-new-in-aspnet-web-api-21"></a>O que há de novo no ASP.NET Web API 2.1
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web API 2.1.

- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos na API Web ASP.NET 2.1](#new-features)

    - [Tratamento de erro global](#global-error)
    - [Aprimoramentos de roteamento de atributo](#attribute-routing)
    - [Melhorias na página de ajuda](#help-page)
    - [Suporte de IgnoreRoute](#ignoreroute)
    - [Formatador de tipo de mídia BSON](#bson)
    - [Melhor suporte para filtros assíncronos](#async-filters)
    - [Consulta de análise para o cliente de biblioteca de formatação](#query-parsing)
- [Problemas conhecidos e alterações recentes](#known-issues)
- [Correções de bugs](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente do RTM do ASP.NET Web API 2.1 tem a seguinte versão: "5.1.2". Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o RTM do ASP.NET Web API 2.1 estão disponíveis no site da web do ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Novos recursos na API Web ASP.NET 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Tratamento de erro global

Todas as exceções sem tratamento agora podem ser registradas em log por meio de um mecanismo central e o comportamento para exceções não tratadas pode ser personalizado.

O framework oferece suporte a vários agentes de exceção, que consulte a exceção sem tratamento e informações sobre o contexto no qual ele ocorreu, como a solicitação sendo processada no momento.

Por exemplo, o código a seguir usa System.Diagnostics.TraceSource para registrar todas as exceções sem tratamento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Você também pode substituir o manipulador de exceção padrão, para que você pode personalizar totalmente a mensagem de resposta HTTP é enviada quando uma exceção sem tratamento ocorre.

Nós fornecemos uma [amostra](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) que registra em log exceções não tratadas por meio da estrutura do ELMAH popular.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamento de atributo

Roteamento de atributo agora dá suporte a restrições, habilitar o controle de versão e a seleção de rota baseada em cabeçalho. Além disso, muitos aspectos de rotas de atributo agora são personalizáveis por meio de **IDirectRouteFactory** interface e **RouteFactoryAttribute** classe. O prefixo da rota agora é extensível por meio de **IRoutePrefix** interface e **RoutePrefixAttribute** classe.

Nós fornecemos uma [amostra](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) que usa as restrições para filtrar dinamicamente controladores por um cabeçalho HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Melhorias na página de ajuda

API Web 2.1 inclui os seguintes aprimoramentos à [páginas de Ajuda da API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentação das propriedades individuais de parâmetros ou tipos de retorno de ações.
- Documentação do modelo de anotações de dados.

O design de interface do usuário das páginas de ajuda também foi atualizado, para acomodar essas alterações.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Suporte de IgnoreRoute

Web fornece suporte de API 2.1 a ignorando padrões de URL no roteamento de API da Web, por meio de um conjunto de **IgnoreRoute** métodos de extensão nas **HttpRouteCollection**. Esses métodos fazem com que a API da Web ignorar todas as URLs que correspondam a um modelo especificado e permitir que o host aplicar o processamento adicional, se apropriado.

O exemplo a seguir ignora os URIs que começam com um &quot;conteúdo&quot; segmento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formatador de tipo de mídia BSON

Web API agora é compatível com o [BSON](http://bsonspec.org/) formato de conexão no cliente e no servidor.

Para habilitar o BSON no lado do servidor, adicione a **BsonMediaTypeFormatter** à coleção de formatadores:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Aqui está como um cliente .NET pode consumir formato BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Nós fornecemos uma [amostra](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) que mostra o lado do cliente e no servidor.

Para obter mais informações, consulte [suporte a BSON na Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Melhor suporte para filtros assíncronos

API da Web agora dá suporte a uma maneira fácil de criar filtros que são executadas de forma assíncrona. Esse recurso é útil é o seu filtro precisa executar uma ação assíncrona, como o acesso um banco de dados. Anteriormente, para criar um filtro assíncrono, você precisava implementar a interface de filtro por conta própria, porque as classes base do filtro exposto somente métodos síncronos. Agora você pode substituir o virtual `On*Async` métodos do filtro de classe de base.

Por exemplo:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

O **AuthorizationFilterAttribute**, **ActionFilterAttribute**, e **ExceptionFilterAttribute** todas classes dão suporte a async no Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Consulta de análise para o cliente de biblioteca de formatação

Anteriormente, **Formatting** de análise e consultas URI para o código do lado do servidor de atualização com suporte, mas a biblioteca portátil equivalente não tinha esse recurso. 2.1 de API da Web, um aplicativo cliente pode agora facilmente analisar e atualizar uma cadeia de caracteres de consulta.

Os exemplos a seguir mostram como analisar, modificar e gerar consultas URI. (Os exemplos mostram um aplicativo de console para simplificar.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

Esta seção descreve problemas conhecidos e alterações significativas no RTM do 2.1 a ASP.NET Web API.

### <a name="attribute-routing"></a>Roteamento de atributo

Ambiguidades em correspondências de roteamento de atributo relatam um erro em vez de escolher a primeira correspondência.

Rotas de atributo estão proibidas de usar o *{controller}* parâmetro e usar o *{action}* colocado de parâmetro nas rotas em ações. Esses parâmetros muito provavelmente faria com que as ambiguidades.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>O scaffolding de API da Web do MVC em um projeto com resultados de pacotes 5.1 em 5.0 pacotes para aqueles que já não existem no projeto

Atualizando pacotes do NuGet para o RTM do ASP.NET Web API 2.1 não atualiza as ferramentas do Visual Studio, como o scaffolding do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (Version=5.0.0.0). Como resultado, o scaffolding do ASP.NET instalará a versão anterior (Version=5.0.0.0) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, o scaffolding do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos.

Se você usar o scaffolding do ASP.NET depois de atualizar os pacotes para a Web API 2.1 ou ASP.NET MVC 5.1, verifique se que as versões de API da Web e o MVC estão consistentes.

### <a name="type-renames"></a>Renomeações de tipo

Alguns dos tipos usados para extensibilidade de roteamento de atributo foram renomeadas do RC para RTM 2.1.

| Nome do tipo antigo (2.1 RC) | Novo tipo de nome (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtros de exceção não desencapsular agregar exceções geradas em ações assíncronas

Anteriormente, se uma ação de async lançou um **AggregateException**, um filtro de exceção seria desencapsular a exceção, e **OnException** obteria a exceção de base. 2.1, o filtro de exceção não desencapsular, e **OnException** obtém original **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correções de Bug

Esta versão também inclui várias correções de bugs. Você pode encontrar a lista completa aqui:

- [5.1.0 pacote](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacote de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

O 5.1.2 pacote contém as atualizações do IntelliSense, mas não há correções de bugs.
