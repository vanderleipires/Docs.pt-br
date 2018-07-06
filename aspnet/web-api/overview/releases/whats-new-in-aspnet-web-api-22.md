---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: O que há de novo no ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 0f0794988da712897092ab808a08fca5eeebb6d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813272"
---
<a name="whats-new-in-aspnet-web-api-22"></a>O que há de novo no ASP.NET Web API 2.2
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web API 2.2.

- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos no ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Aprimoramentos de roteamento de atributo](#ARI)
    - [Suporte do cliente da API da Web para Windows Phone 8.1](#phone)
- [Problemas conhecidos e alterações recentes](#known-issues)
- [Correções de bugs](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente do ASP.NET Web API 2.2 tem a seguinte versão: "5.2.0". Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET Web API 2.2 estão disponíveis no site da web do ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Novos recursos no ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Essa versão adiciona suporte para o protocolo OData v4. Para obter mais informações, consulte o [Web API OData v4 documentação.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Aqui estão alguns dos principais recursos e alterações para OData v4:

- [Suporte para as propriedades de alias no modelo de OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Suporte para ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute em ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Fornecer a capacidade de fornecer um título amigável para ações](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrar com UriParser ODL
- Suporte para [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [confinamento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Suporte à conversão para tipos primitivos
- [Adicionado suporte de função do OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Dá suporte a aliases de parâmetro para chamadas de função](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Oferecer suporte a convenção de nomenclatura camel case no modelo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Suporte para cast () em $filter
- Suporte para tipo complexo aberto
- Removido do EntitySetController e AsyncEntitySetController
- [$Link alterados para $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Adicionado suporte de roteamento de atributo](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Usa as bibliotecas principais do OData do 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamento de atributo

Agora, o roteamento de atributo fornece um ponto de extensibilidade chamado IDirectRouteProvider, que permite que o controle total sobre como as rotas de atributo são descobertas e configuradas. Um IDirectRouteProvider é responsável por fornecer uma lista de ações e controladores, juntamente com informações de rotas associado para especificar exatamente qual configuração de roteamento for desejada para essas ações. Uma implementação de IDirectRouteProvider pode ser especificada ao chamar MapAttributes/MapHttpAttributeRoutes.

Personalizar IDirectRouteProvider será mais fácil, estendendo a nossa implementação padrão, DefaultDirectRouteProvider. Essa classe fornece métodos virtuais substituíveis separados para alterar a lógica para descobrir atributos, criando entradas de rota e descoberta de prefixo de rota e o prefixo da área.

Estes são alguns exemplos em que você poderia fazer com esse novo ponto de extensibilidade:

1. Dá suporte à herança de atributos de rota

    Exemplo:

    Aqui uma solicitação como "/ api/valores/10" com êxito retornaria "Sucesso: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Forneça um nome de rota padrão para as rotas de atributo seguindo alguma convenção que você deseja. Por padrão, o roteamento de atributo não cria automaticamente nomes para rotas de atributo.
3. Modificar o modelo de rota de rotas de atributo em um local central antes que eles acabam na tabela de rotas.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Suporte do cliente da API da Web para Windows Phone 8.1

Agora você pode usar o pacote NuGet de cliente de API da Web para implementar sua lógica de cliente de API da Web ao direcionar o Windows Phone 8.1 ou de dentro de um aplicativo Universal.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

Esta seção descreve problemas conhecidos e alterações recentes do que o ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Construtor de modelo

Problema: Funções sobrecarregadas não pode ser expostas como FunctionImport

Se não houver funções sobrecarregadas a 2 e também são FunctionImport conforme mostrado abaixo, em seguida, solicitar os resultados de ~/GetAllConventionCustomers(CustomerName={customerName}) em System. InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Solução alternativa: A solução alternativa para esse problema é adicionar sobrecargas de função como FunctionImports.

#### <a name="odata-routing"></a>Roteamento de OData

Literais de cadeia de caracteres que incluem a URL codificada em barra "/" (% 2F) e backslash(%5C) provocar um erro 404 quando eles são usados nos caminhos de recurso do OData.

Por exemplo, literais de cadeia de caracteres podem ser usados nos caminhos de recurso do OData como parâmetros de funções ou valores de chave de conjuntos de entidades.

/Employees/total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Quando os serviços recebem tais solicitações de hosts serão un-escape as sequências de escape antes de passá-los para o tempo de execução de API da Web. Isso protege contra ataques, como o seguinte:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Isso faz com que a pilha de OData da API Web retornar um erro 404 (não encontrado). Para evitar esse erro, o cliente deve usar as sequências de escape duplo para barra "/" (% 252F) e uma barra invertida (% C 255). Isso não acontece para cadeias de caracteres de consulta, como /Employees? $filter = Name eq 'Nome % 2F'

**Observe as barras sem escape ('/') e barras invertidas (") não são válidas em literais de cadeia de caracteres de caminho de recurso de OData. Barras "/" deve aparecer apenas como separadores de caminho e barras invertidas devem aparecer no caminho de recurso do OData. (Ambos são úteis em algumas partes de uma cadeia de caracteres de consulta OData.)**

Solução alternativa: Você pode substituir o método de análise de DefaultODataPathHandler para a barra "/" e uma barra invertida em literais de cadeia de caracteres de escape antes de realmente analisá-los. Você pode encontrar um exemplo dessa abordagem aqui.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Queryable]

O atributo [Queryable] é preterido. Novos aplicativos do OData v3 devem usar **System.Web.Http.OData.EnableQueryAttribute**.

O **ODataHttpConfigurationExtensions.EnableQuerySupport** método de extensão agora adiciona uma **EnableQueryAttribute** à coleção de filtros globais. Se tiver qualquer controlador a **[Queryable]** do atributo, chamando `config.EnableQuerySupport()` fará com que o **[Queryable]** atributo falha

A maneira recomendada para resolver esse problema é substituir todas as instâncias do **QueryableAttribute** com **System.Web.Http.OData.EnableQueryAttribute**.

Uma solução alternativa é usar o código a seguir em sua configuração de API da Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Roteamento de atributo

Problema: Associação de modelos de tipo complexo que está decorada com o atributo FromUri tem um comportamento diferente ao usar o roteamento de atributo.

Link a seguir para acompanhar o problema e também apresenta detalhes sobre uma solução alternativa.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problema: Scaffolding MVC Web/API em um projeto com 5.2.0 resultados de pacotes no 5.1.2 pacotes para aqueles que já não existem no projeto

Atualizando pacotes do NuGet para ASP.NET MVC 5.2 não atualiza as ferramentas do Visual Studio, como o scaffolding do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (por exemplo, 5.1.2 na atualização 2). Como resultado, o scaffolding do ASP.NET instalará a versão anterior (por exemplo, 5.1.2 na atualização 2) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, o scaffolding do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos. Se você usar o scaffolding do ASP.NET depois de atualizar os pacotes de seus projetos Web API 2.2 ou ASP.NET MVC 5.2, verifique se que as versões de API da Web e ASP.NET MVC são consistentes.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correções de bugs e atualizações de recursos secundários

Esta versão também inclui várias correções de bugs e recursos secundários atualizações. Você pode encontrar a lista completa aqui:

- [pacote 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

O pacote de Microsoft.AspNet.OData 5.2.1 contém as atualizações de dependência do NuGet, mas não há correções de bugs. Com essa atualização, não há uma dependência estrita Microsoft.OData.Core 6.4.0, mas um pode atualizar a qualquer versão entre 6.4.0 e 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

Nesta versão, fizemos uma dependência de alteração para `Json.Net 6.0.4`. Para obter mais informações sobre o que há de novo nesta versão do `Json.NET`, consulte [Json.NET 6.0 Release 4 - JSON Merge, injeção de dependência](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Esta versão não tem outros recursos novos ou correções de bugs no API da Web. Nós atualizamos todos os outros pacotes que sabemos depender essa nova versão da API da Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Você pode ler sobre a versão [aqui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Esta versão contém apenas as correções de bugs. Você pode usar [essa consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver a lista dos problemas corrigidos nesta versão.
