---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Ações e funções no OData v4 usando ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Em OData, ações e funções são uma maneira de adicionar os comportamentos do servidor que não são facilmente definidos como operações CRUD nas entidades. Este tutorial mostra como...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508225"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Ações e funções no OData v4 usando ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Em OData, ações e funções são uma maneira de adicionar os comportamentos do servidor que não são facilmente definidos como operações CRUD nas entidades. Este tutorial mostra como adicionar funções e ações para um ponto de extremidade de v4 do OData, usando 2.2 de API da Web. O tutorial se baseia o tutorial [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - 2.2 da API da Web
> - OData v4
> - [Visual Studio 2013 Atualização 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versões do tutoriais
> 
> Para OData versão 3, consulte [ações de OData no ASP.NET Web API 2](../odata-v3/odata-actions.md).


A diferença entre *ações* e *funções* que ações podem ter efeitos colaterais e não funções. Ações e funções podem retornar dados. Alguns usos para ações incluem:

- Transações complexas.
- Manipulando várias entidades de uma vez.
- Permitir atualizações apenas para determinadas propriedades de uma entidade.
- Envio de dados que não é uma entidade.

Funções são úteis para retornar informações que não correspondem diretamente a uma entidade ou coleção.

Uma ação (ou função) pode direcionar uma única entidade ou uma coleção. Na terminologia do OData, este é o *associação*. Você também pode ter &quot;desassociado&quot; ações/funções, que são chamadas como estáticas operações no serviço.

## <a name="example-adding-an-action"></a>Exemplo: Adicionar uma ação

Vamos definir uma ação para avaliar um produto.

> [!NOTE]
> Este tutorial baseia-se no tutorial [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


Primeiro, adicione um `ProductRating` modelo para representar as classificações.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Também adicionar uma **DbSet** para o `ProductsContext` de classe, para que o EF criará uma tabela de classificação no banco de dados.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Adicione a ação de EDM

Em WebApiConfig.cs, adicione o seguinte código:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

O **EntityTypeConfiguration.Action** método adiciona uma ação para o modelo de dados de entidade (EDM). O **parâmetro** método Especifica um parâmetro de tipo para a ação.

Esse código também define o namespace do EDM. O namespace é importante porque o URI para a ação inclui o nome totalmente qualificado de ação:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Em uma configuração típica do IIS, o ponto nessa URL fará com que o IIS retornar o erro 404. Você pode resolver esse problema adicionando a seguinte seção ao seu arquivo Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Adicionar um método de controlador para a ação

Para habilitar o &quot;taxa&quot; ação, adicione o seguinte método para `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Observe que o nome do método corresponde ao nome de ação. O **[HttpPost]** atributo especifica o método é um método HTTP POST.

Para chamar a ação, o cliente envia uma solicitação HTTP POST com o seguinte:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

O &quot;taxa&quot; ação está associada às instâncias de produto, portanto, o URI para a ação é o nome da ação totalmente qualificado acrescentado à entidade URI. (Lembre-se de que definimos o namespace EDM como &quot;ProductService&quot;, portanto, é o nome totalmente qualificado ação &quot;ProductService.Rate&quot;.)

O corpo da solicitação contém os parâmetros de ação como uma carga JSON. API da Web converte automaticamente a carga de JSON para um **ODataActionParameters** objeto, que é apenas um dicionário de valores de parâmetro. Use este dicionário para acessar os parâmetros em seu método de controlador.

Se o cliente envia os parâmetros de ação em errado Formatar, o valor de **ModelState** é false. Verificar esse sinalizador em seu método de controlador e retornará um erro se **IsValid** é false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Exemplo: Adicionar uma função

Agora vamos adicionar uma função OData que retorna o produto mais caro. Como antes, a primeira etapa é adicionar a função de EDM. No WebApiConfig.cs, adicione o código a seguir.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Nesse caso, a função está associada a coleção de produtos, em vez de instâncias individuais do produto. Os clientes invocam a função enviando uma solicitação GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Aqui está o método do controlador para esta função:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Observe que o nome do método corresponde ao nome de função. O **[HttpGet]** atributo especifica o método é um método HTTP GET.

Aqui está a resposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Exemplo: Adicionar uma função não associada

O exemplo anterior era uma função associada a uma coleção. No exemplo a seguir, vamos criar um *desassociado* função. Funções não associadas são chamadas como estáticas operações no serviço. A função neste exemplo retorna o imposto sobre vendas para um determinado código postal.

No arquivo WebApiConfig, adicione a função de EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Observe que estamos ligando **função** diretamente no **ODataModelBuilder**, em vez do tipo de entidade ou coleção. Isso informa ao construtor de modelo que a função é desassociada.

Aqui está o método do controlador que implementa a função:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Não importa qual controlador de API da Web que você colocar este método em. Você pode colocá-lo `ProductsController`, ou definir um controlador separado. O **[ODataRoute]** atributo define o modelo de URI para a função.

Aqui está um exemplo de solicitação de cliente:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

A resposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
