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
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="05dc1-102">O que há de novo no ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="05dc1-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="05dc1-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="05dc1-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="05dc1-104">Este tópico descreve o que há de novo para ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="05dc1-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="05dc1-105">Baixar</span><span class="sxs-lookup"><span data-stu-id="05dc1-105">Download</span></span>](#download)
- [<span data-ttu-id="05dc1-106">Documentação</span><span class="sxs-lookup"><span data-stu-id="05dc1-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="05dc1-107">Novos recursos no ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="05dc1-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="05dc1-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="05dc1-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="05dc1-109">Aprimoramentos de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="05dc1-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="05dc1-110">Suporte do cliente da API da Web para Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="05dc1-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="05dc1-111">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="05dc1-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="05dc1-112">Correções de bugs</span><span class="sxs-lookup"><span data-stu-id="05dc1-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="05dc1-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="05dc1-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="05dc1-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="05dc1-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="05dc1-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="05dc1-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="05dc1-116">Baixar</span><span class="sxs-lookup"><span data-stu-id="05dc1-116">Download</span></span>

<span data-ttu-id="05dc1-117">Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet.</span><span class="sxs-lookup"><span data-stu-id="05dc1-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="05dc1-118">Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação.</span><span class="sxs-lookup"><span data-stu-id="05dc1-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="05dc1-119">O pacote mais recente do ASP.NET Web API 2.2 tem a seguinte versão: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="05dc1-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="05dc1-120">Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="05dc1-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="05dc1-121">A versão também inclui pacotes localizados correspondentes no NuGet.</span><span class="sxs-lookup"><span data-stu-id="05dc1-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="05dc1-122">Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:</span><span class="sxs-lookup"><span data-stu-id="05dc1-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="05dc1-123">Documentação</span><span class="sxs-lookup"><span data-stu-id="05dc1-123">Documentation</span></span>

<span data-ttu-id="05dc1-124">Tutoriais e outras informações sobre o ASP.NET Web API 2.2 estão disponíveis no site da web do ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="05dc1-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="05dc1-125">Novos recursos no ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="05dc1-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="05dc1-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="05dc1-126">OData v4</span></span>

<span data-ttu-id="05dc1-127">Essa versão adiciona suporte para o protocolo OData v4.</span><span class="sxs-lookup"><span data-stu-id="05dc1-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="05dc1-128">Para obter mais informações, consulte o [Web API OData v4 documentação.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="05dc1-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="05dc1-129">Aqui estão alguns dos principais recursos e alterações para OData v4:</span><span class="sxs-lookup"><span data-stu-id="05dc1-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="05dc1-130">Suporte para as propriedades de alias no modelo de OData</span><span class="sxs-lookup"><span data-stu-id="05dc1-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="05dc1-131">Suporte para ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute em ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="05dc1-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="05dc1-132">Fornecer a capacidade de fornecer um título amigável para ações</span><span class="sxs-lookup"><span data-stu-id="05dc1-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="05dc1-133">Integrar com UriParser ODL</span><span class="sxs-lookup"><span data-stu-id="05dc1-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="05dc1-134">Suporte para [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [confinamento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="05dc1-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="05dc1-135">Suporte à conversão para tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="05dc1-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="05dc1-136">Adicionado suporte de função do OData</span><span class="sxs-lookup"><span data-stu-id="05dc1-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="05dc1-137">Dá suporte a aliases de parâmetro para chamadas de função</span><span class="sxs-lookup"><span data-stu-id="05dc1-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="05dc1-138">Oferecer suporte a convenção de nomenclatura camel case no modelo</span><span class="sxs-lookup"><span data-stu-id="05dc1-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="05dc1-139">Suporte para cast () em $filter</span><span class="sxs-lookup"><span data-stu-id="05dc1-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="05dc1-140">Suporte para tipo complexo aberto</span><span class="sxs-lookup"><span data-stu-id="05dc1-140">Support for open complex type</span></span>
- <span data-ttu-id="05dc1-141">Removido do EntitySetController e AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="05dc1-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="05dc1-142">$Link alterados para $ref</span><span class="sxs-lookup"><span data-stu-id="05dc1-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="05dc1-143">Adicionado suporte de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="05dc1-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="05dc1-144">Usa as bibliotecas principais do OData do 6.4.0</span><span class="sxs-lookup"><span data-stu-id="05dc1-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="05dc1-145">Aprimoramentos de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="05dc1-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="05dc1-146">Agora, o roteamento de atributo fornece um ponto de extensibilidade chamado IDirectRouteProvider, que permite que o controle total sobre como as rotas de atributo são descobertas e configuradas.</span><span class="sxs-lookup"><span data-stu-id="05dc1-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="05dc1-147">Um IDirectRouteProvider é responsável por fornecer uma lista de ações e controladores, juntamente com informações de rotas associado para especificar exatamente qual configuração de roteamento for desejada para essas ações.</span><span class="sxs-lookup"><span data-stu-id="05dc1-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="05dc1-148">Uma implementação de IDirectRouteProvider pode ser especificada ao chamar MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="05dc1-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="05dc1-149">Personalizar IDirectRouteProvider será mais fácil, estendendo a nossa implementação padrão, DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="05dc1-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="05dc1-150">Essa classe fornece métodos virtuais substituíveis separados para alterar a lógica para descobrir atributos, criando entradas de rota e descoberta de prefixo de rota e o prefixo da área.</span><span class="sxs-lookup"><span data-stu-id="05dc1-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="05dc1-151">Estes são alguns exemplos em que você poderia fazer com esse novo ponto de extensibilidade:</span><span class="sxs-lookup"><span data-stu-id="05dc1-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="05dc1-152">Dá suporte à herança de atributos de rota</span><span class="sxs-lookup"><span data-stu-id="05dc1-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="05dc1-153">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="05dc1-153">Example:</span></span>

    <span data-ttu-id="05dc1-154">Aqui uma solicitação como "/ api/valores/10" com êxito retornaria "Sucesso: 10"</span><span class="sxs-lookup"><span data-stu-id="05dc1-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="05dc1-155">Forneça um nome de rota padrão para as rotas de atributo seguindo alguma convenção que você deseja.</span><span class="sxs-lookup"><span data-stu-id="05dc1-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="05dc1-156">Por padrão, o roteamento de atributo não cria automaticamente nomes para rotas de atributo.</span><span class="sxs-lookup"><span data-stu-id="05dc1-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="05dc1-157">Modificar o modelo de rota de rotas de atributo em um local central antes que eles acabam na tabela de rotas.</span><span class="sxs-lookup"><span data-stu-id="05dc1-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="05dc1-158">Suporte do cliente da API da Web para Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="05dc1-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="05dc1-159">Agora você pode usar o pacote NuGet de cliente de API da Web para implementar sua lógica de cliente de API da Web ao direcionar o Windows Phone 8.1 ou de dentro de um aplicativo Universal.</span><span class="sxs-lookup"><span data-stu-id="05dc1-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="05dc1-160">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="05dc1-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="05dc1-161">Esta seção descreve problemas conhecidos e alterações recentes do que o ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="05dc1-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="05dc1-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="05dc1-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="05dc1-163">Construtor de modelo</span><span class="sxs-lookup"><span data-stu-id="05dc1-163">Model builder</span></span>

<span data-ttu-id="05dc1-164">Problema: Funções sobrecarregadas não pode ser expostas como FunctionImport</span><span class="sxs-lookup"><span data-stu-id="05dc1-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="05dc1-165">Se não houver funções sobrecarregadas a 2 e também são FunctionImport conforme mostrado abaixo, em seguida, solicitar os resultados de ~/GetAllConventionCustomers(CustomerName={customerName}) em System. InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="05dc1-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="05dc1-166">Solução alternativa: A solução alternativa para esse problema é adicionar sobrecargas de função como FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="05dc1-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="05dc1-167">Roteamento de OData</span><span class="sxs-lookup"><span data-stu-id="05dc1-167">OData Routing</span></span>

<span data-ttu-id="05dc1-168">Literais de cadeia de caracteres que incluem a URL codificada em barra "/" (% 2F) e backslash(%5C) provocar um erro 404 quando eles são usados nos caminhos de recurso do OData.</span><span class="sxs-lookup"><span data-stu-id="05dc1-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="05dc1-169">Por exemplo, literais de cadeia de caracteres podem ser usados nos caminhos de recurso do OData como parâmetros de funções ou valores de chave de conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="05dc1-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="05dc1-170">/Employees/total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="05dc1-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="05dc1-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="05dc1-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="05dc1-172">Quando os serviços recebem tais solicitações de hosts serão un-escape as sequências de escape antes de passá-los para o tempo de execução de API da Web.</span><span class="sxs-lookup"><span data-stu-id="05dc1-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="05dc1-173">Isso protege contra ataques, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="05dc1-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="05dc1-174">Isso faz com que a pilha de OData da API Web retornar um erro 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="05dc1-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="05dc1-175">Para evitar esse erro, o cliente deve usar as sequências de escape duplo para barra "/" (% 252F) e uma barra invertida (% C 255).</span><span class="sxs-lookup"><span data-stu-id="05dc1-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="05dc1-176">Isso não acontece para cadeias de caracteres de consulta, como /Employees? $filter = Name eq 'Nome % 2F'</span><span class="sxs-lookup"><span data-stu-id="05dc1-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="05dc1-177">**Observe as barras sem escape ('/') e barras invertidas (") não são válidas em literais de cadeia de caracteres de caminho de recurso de OData. Barras "/" deve aparecer apenas como separadores de caminho e barras invertidas devem aparecer no caminho de recurso do OData. (Ambos são úteis em algumas partes de uma cadeia de caracteres de consulta OData.)**</span><span class="sxs-lookup"><span data-stu-id="05dc1-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="05dc1-178">Solução alternativa: Você pode substituir o método de análise de DefaultODataPathHandler para a barra "/" e uma barra invertida em literais de cadeia de caracteres de escape antes de realmente analisá-los.</span><span class="sxs-lookup"><span data-stu-id="05dc1-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="05dc1-179">Você pode encontrar um exemplo dessa abordagem aqui.</span><span class="sxs-lookup"><span data-stu-id="05dc1-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="05dc1-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="05dc1-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="05dc1-181">[Queryable]</span><span class="sxs-lookup"><span data-stu-id="05dc1-181">[Queryable]</span></span>

<span data-ttu-id="05dc1-182">O atributo [Queryable] é preterido.</span><span class="sxs-lookup"><span data-stu-id="05dc1-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="05dc1-183">Novos aplicativos do OData v3 devem usar **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="05dc1-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="05dc1-184">O **ODataHttpConfigurationExtensions.EnableQuerySupport** método de extensão agora adiciona uma **EnableQueryAttribute** à coleção de filtros globais.</span><span class="sxs-lookup"><span data-stu-id="05dc1-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="05dc1-185">Se tiver qualquer controlador a **[Queryable]** do atributo, chamando `config.EnableQuerySupport()` fará com que o **[Queryable]** atributo falha</span><span class="sxs-lookup"><span data-stu-id="05dc1-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="05dc1-186">A maneira recomendada para resolver esse problema é substituir todas as instâncias do **QueryableAttribute** com **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="05dc1-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="05dc1-187">Uma solução alternativa é usar o código a seguir em sua configuração de API da Web:</span><span class="sxs-lookup"><span data-stu-id="05dc1-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="05dc1-188">Roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="05dc1-188">Attribute Routing</span></span>

<span data-ttu-id="05dc1-189">Problema: Associação de modelos de tipo complexo que está decorada com o atributo FromUri tem um comportamento diferente ao usar o roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="05dc1-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="05dc1-190">Link a seguir para acompanhar o problema e também apresenta detalhes sobre uma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="05dc1-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="05dc1-191">Problema: Scaffolding MVC Web/API em um projeto com 5.2.0 resultados de pacotes no 5.1.2 pacotes para aqueles que já não existem no projeto</span><span class="sxs-lookup"><span data-stu-id="05dc1-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="05dc1-192">Atualizando pacotes do NuGet para ASP.NET MVC 5.2 não atualiza as ferramentas do Visual Studio, como o scaffolding do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="05dc1-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="05dc1-193">Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (por exemplo, 5.1.2 na atualização 2).</span><span class="sxs-lookup"><span data-stu-id="05dc1-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="05dc1-194">Como resultado, o scaffolding do ASP.NET instalará a versão anterior (por exemplo, 5.1.2 na atualização 2) dos pacotes necessários, se eles já não estão disponíveis em seus projetos.</span><span class="sxs-lookup"><span data-stu-id="05dc1-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="05dc1-195">No entanto, o scaffolding do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos.</span><span class="sxs-lookup"><span data-stu-id="05dc1-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="05dc1-196">Se você usar o scaffolding do ASP.NET depois de atualizar os pacotes de seus projetos Web API 2.2 ou ASP.NET MVC 5.2, verifique se que as versões de API da Web e ASP.NET MVC são consistentes.</span><span class="sxs-lookup"><span data-stu-id="05dc1-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="05dc1-197">Correções de bugs e atualizações de recursos secundários</span><span class="sxs-lookup"><span data-stu-id="05dc1-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="05dc1-198">Esta versão também inclui várias correções de bugs e recursos secundários atualizações.</span><span class="sxs-lookup"><span data-stu-id="05dc1-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="05dc1-199">Você pode encontrar a lista completa aqui:</span><span class="sxs-lookup"><span data-stu-id="05dc1-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="05dc1-200">pacote 5.2</span><span class="sxs-lookup"><span data-stu-id="05dc1-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="05dc1-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="05dc1-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="05dc1-202">O pacote de Microsoft.AspNet.OData 5.2.1 contém as atualizações de dependência do NuGet, mas não há correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="05dc1-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="05dc1-203">Com essa atualização, não há uma dependência estrita Microsoft.OData.Core 6.4.0, mas um pode atualizar a qualquer versão entre 6.4.0 e 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="05dc1-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="05dc1-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="05dc1-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="05dc1-205">Nesta versão, fizemos uma dependência de alteração para `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="05dc1-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="05dc1-206">Para obter mais informações sobre o que há de novo nesta versão do `Json.NET`, consulte [Json.NET 6.0 Release 4 - JSON Merge, injeção de dependência](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05dc1-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="05dc1-207">Esta versão não tem outros recursos novos ou correções de bugs no API da Web.</span><span class="sxs-lookup"><span data-stu-id="05dc1-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="05dc1-208">Nós atualizamos todos os outros pacotes que sabemos depender essa nova versão da API da Web.</span><span class="sxs-lookup"><span data-stu-id="05dc1-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="05dc1-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="05dc1-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="05dc1-210">Você pode ler sobre a versão [aqui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="05dc1-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="05dc1-211">Esta versão contém apenas as correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="05dc1-211">This release contains only bug fixes.</span></span> <span data-ttu-id="05dc1-212">Você pode usar [essa consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver a lista dos problemas corrigidos nesta versão.</span><span class="sxs-lookup"><span data-stu-id="05dc1-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
