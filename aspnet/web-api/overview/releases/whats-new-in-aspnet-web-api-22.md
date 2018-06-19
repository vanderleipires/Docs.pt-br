---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: O que há de novo no ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508395"
---
<a name="whats-new-in-aspnet-web-api-22"></a>O que há de novo no ASP.NET Web API 2.2
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web API 2.2.

- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos no ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Aprimoramentos de roteamentos de atributo](#ARI)
    - [Suporte a cliente de API da Web para Windows Phone 8.1](#phone)
- [Problemas conhecidos e as alterações recentes](#known-issues)
- [Correções de bugs](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes do NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente do ASP.NET Web API 2.2 tem a seguinte versão: "5.2.0". Você pode instalar ou atualizar esses pacotes por meio de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados usando o NuGet Package Manager Console:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET Web API 2.2 estão disponíveis no site da web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Novos recursos no ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Essa versão adiciona suporte para o protocolo v4 do OData. Para obter mais informações, consulte o [Web API OData v4 documentação.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Aqui estão alguns dos principais recursos e alterações de OData v4:

- [Suporte para propriedades de alias no modelo de OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Suporte para ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute em ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Oferecer a capacidade de fornecer título amigável para ações](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrar UriParser ODL
- Suporte para [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contenção](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Suporte a conversão para tipos primitivos
- [Adicionado suporte de função do OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Suporte a aliases de parâmetro para chamadas de função](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Oferecer suporte a concatenação com maiusculas convenção de nomenclatura no modelo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Suporte para cast () em $filter
- Suporte para tipo complexo aberto
- Removido EntitySetController e AsyncEntitySetController
- [$Link alterado para $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Adicionado suporte de roteamento de atributo](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Usar as bibliotecas do núcleo do OData 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamentos de atributo

Roteamento de atributo agora fornece um ponto de extensibilidade chamado IDirectRouteProvider, que permite o controle completo sobre como as rotas de atributo são descobertas e configuradas. Um IDirectRouteProvider é responsável por fornecer uma lista de ações e controladores juntamente com informações de rota associado para especificar exatamente qual configuração de roteamento for desejada para essas ações. Uma implementação de IDirectRouteProvider pode ser especificada ao chamar MapAttributes/MapHttpAttributeRoutes.

Personalizar IDirectRouteProvider será mais fácil estendendo nossa implementação do padrão, DefaultDirectRouteProvider. Essa classe fornece métodos substituíveis virtuais separados para alterar a lógica de detecção de atributos, criando entradas de rota e descoberta de prefixo da rota e o prefixo da área.

Estes são alguns exemplos em que você pode fazer com esse novo ponto de extensibilidade:

1. Suporte a herança de atributos de rota

    Exemplo:

    Aqui uma solicitação como "/ 10/de valores de api" retornaria com êxito "Sucesso: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Forneça um nome de rota padrão para as rotas de atributo seguindo alguma convenção que você deseja. Por padrão, o roteamento de atributo não cria automaticamente nomes para rotas de atributo.
3. Modificar o modelo de rota de rotas de atributo em um local central antes de eles terminam na tabela de rotas.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Suporte a cliente de API da Web para Windows Phone 8.1

Agora você pode usar o pacote NuGet do cliente de API da Web para implementar a lógica de cliente de API da Web durante o direcionamento do Windows Phone 8.1 ou de dentro de um aplicativo Universal.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

Esta seção descreve problemas conhecidos e as alterações recentes de 2.2 do ASP.NET Web API.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Construtor de modelo

Problema: Funções sobrecarregadas não podem ser expostas como FunctionImport

Se não houver funções sobrecarregadas a 2 e também são FunctionImport conforme mostrado abaixo, em seguida, solicitar ~/GetAllConventionCustomers(CustomerName={customerName}) resultados em System. InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Solução alternativa: A solução alternativa para esse problema é adicionar as sobrecargas de função como FunctionImports.

#### <a name="odata-routing"></a>Roteamento de OData

Literais de cadeia de caracteres que incluem a URL codificada barra (% 2F) e backslash(%5C) causar um erro 404 quando eles são usados nos caminhos de recurso do OData.

Por exemplo, literais de cadeia de caracteres podem ser usados nos caminhos OData recursos como parâmetros de funções ou valores de chave de conjuntos de entidades.

/Employees/total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Quando os serviços recebem tais solicitações a hosts será un-escape as sequências de escape antes de passá-los para o tempo de execução de API da Web. Isso protege contra ataques semelhante ao seguinte:  
  
 http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:

Isso faz com que a pilha de OData da API Web retornar um erro 404 (não encontrado). Para evitar esse erro, o cliente deve usar sequências de escape duplo para barra (% 252F) e barra invertida (% C de 255). Isso não acontecer por cadeias de caracteres de consulta como /Employees? $filter = Name eq 'Nome % 2F'

**Observe as barras sem escape ('/') e barras invertidas (") não são válidas em literais de cadeia de caracteres de caminho de recurso de OData. Barras devem aparecer somente como separadores de caminho e barras invertidas devem aparecer no caminho de recurso do OData. (Ambos são úteis em algumas partes de uma cadeia de caracteres de consulta OData.)**

Solução alternativa: Você pode substituir o método de análise de DefaultODataPathHandler para a barra e a barra invertida em literais de cadeia de caracteres de escape antes de realmente análise-los. Você pode encontrar um exemplo dessa abordagem aqui.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Consulta]

O atributo [Queryable] foi preterido. Novos aplicativos do OData v3 devem usar **System.Web.Http.OData.EnableQueryAttribute**.

O **ODataHttpConfigurationExtensions.EnableQuerySupport** método de extensão agora adiciona um **EnableQueryAttribute** à coleção de filtros globais. Se tem quaisquer controladores o **[Queryable]** de atributo, chamando `config.EnableQuerySupport()` fará com que o **[Queryable]** atributo falha

É a maneira recomendada para resolver esse problema substituir todas as instâncias de **QueryableAttribute** com **System.Web.Http.OData.EnableQueryAttribute**.

Uma solução alternativa é usar o código a seguir em sua configuração de API da Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Roteamento de atributo

Problema: Associação de modelo de tipo complexo que está decorado com atributos FromUri tem um comportamento diferente ao usar o roteamento de atributo.

Link a seguir para acompanhar o problema e também exibe detalhes sobre uma solução alternativa.  
[http://aspnetwebstack.codeplex.com/WorkItem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problema: Scaffolding MVC/Web API em um projeto com 5.2.0 resultados de pacotes em 5.1.2 pacotes para aqueles que já não existe no projeto

Atualizando pacotes do NuGet para ASP.NET MVC 5.2 não atualizar as ferramentas do Visual Studio, como a estrutura do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (por exemplo, 5.1.2 na atualização 2). Como resultado, o ASP.NET scaffolding instalará a versão anterior (por exemplo, 5.1.2 na atualização 2) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, a estrutura do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos. Se você usar ASP.NET scaffolding depois de atualizar os pacotes de seus projetos Web API 2.2 ou 5.2 do ASP.NET MVC, assegure-se de que as versões de API da Web e ASP.NET MVC são consistentes.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correções de bugs e atualizações de recursos secundários

Esta versão também inclui várias correções de bugs e recursos secundários atualizações. Você pode encontrar a lista completa aqui:

- [pacote 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

O pacote de Microsoft.AspNet.OData 5.2.1 contém atualizações de dependência NuGet, mas nenhuma correção de bug. Com essa atualização, não há uma dependência estrita Microsoft.OData.Core 6.4.0, mas um pode atualizar para qualquer versão entre 6.4.0 e 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

Nesta versão fizemos uma dependência alterar para `Json.Net 6.0.4`. Para obter mais informações sobre o que há de novo nesta versão do `Json.NET`, consulte [Json.NET 6.0 versão 4 - JSON mesclar, injeção de dependência](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Esta versão não tem outros novos recursos ou correções de bug na API da Web. Atualizamos subsequentemente todos os outros pacotes dependentes que temos para depender essa nova versão da API da Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Você pode ler sobre a versão [aqui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Esta versão contém somente as correções de bugs. Você pode usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver a lista de problemas corrigidos nesta versão.
