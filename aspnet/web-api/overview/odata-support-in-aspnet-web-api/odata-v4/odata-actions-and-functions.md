---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Ações e funções no OData v4 usando a API Web ASP.NET 2.2 | Microsoft Docs
author: MikeWasson
description: Em OData, ações e funções são uma maneira para adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD nas entidades. Este tutorial mostra como...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: a34361a2b298a6cb1efaa386d0a60011f4a578fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834968"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Ações e funções no OData v4 usando a API Web ASP.NET 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Em OData, ações e funções são uma maneira para adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD nas entidades. Este tutorial mostra como adicionar ações e funções a um ponto de extremidade de v4 do OData, usando o Web API 2.2. O tutorial se baseia no tutorial [criar um OData v4 ponto de extremidade usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 2.2
> - OData v4
> - [Visual Studio 2013 Atualização 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versões de tutoriais
> 
> Para OData versão 3, consulte [ações de OData na API Web ASP.NET 2](../odata-v3/odata-actions.md).


A diferença entre *ações* e *funções* é que as ações podem ter efeitos colaterais e não funções. Ações e funções podem retornar dados. Alguns usos para ações incluem:

- Transações complexas.
- Manipulando várias entidades de uma só vez.
- Permitindo atualizações apenas em determinadas propriedades de uma entidade.
- Envio de dados que não é uma entidade.

Funções são úteis para retornar informações que não correspondem diretamente a uma entidade ou a coleção.

Uma ação (ou função) pode direcionar uma única entidade ou uma coleção. Na terminologia do OData, isso é o *associação*. Você também pode ter &quot;desassociado&quot; ações/funções, que são chamadas como estáticas operações no serviço.

## <a name="example-adding-an-action"></a>Exemplo: Adicionar uma ação

Vamos definir uma ação para classificar um produto.

> [!NOTE]
> Este tutorial se baseia no tutorial [criar um OData v4 ponto de extremidade usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md)


Primeiro, adicione um `ProductRating` modelo para representar as classificações.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Adicione também uma **DbSet** para o `ProductsContext` de classe, para que o EF criará uma tabela de classificações no banco de dados.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Adicionar a ação para o EDM

Em WebApiConfig.cs, adicione o seguinte código:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

O **EntityTypeConfiguration.Action** método adiciona uma ação para o modelo de dados de entidade (EDM). O **parâmetro** método Especifica um parâmetro de tipo para a ação.

Esse código também define o namespace para o EDM. O namespace é importante porque o URI para a ação inclui o nome da ação totalmente qualificado:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Em uma configuração típica do IIS, o ponto nessa URL fará com que o IIS retornar o erro 404. Você pode resolver esse problema adicionando a seguinte seção ao seu arquivo Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Adicione um método de controlador para a ação

Para habilitar o &quot;taxa&quot; ação, adicione o seguinte método à `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Observe que o nome do método corresponde ao nome de ação. O **[HttpPost]** atributo especifica o método é um método HTTP POST.

Para invocar a ação, o cliente envia uma solicitação HTTP POST semelhante ao seguinte:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

O &quot;taxa&quot; ação está associada às instâncias de Product, portanto, o URI para a ação é o nome da ação totalmente qualificado acrescentado ao URI de entidade. (Lembre-se de que definimos o namespace EDM como &quot;ProductService&quot;, portanto, é o nome da ação totalmente qualificado &quot;ProductService.Rate&quot;.)

O corpo da solicitação contém os parâmetros de ação como um conteúdo JSON. API da Web converte automaticamente o conteúdo JSON para um **ODataActionParameters** objeto, que é apenas um dicionário de valores de parâmetro. Use este dicionário para acessar os parâmetros em seu método de controlador.

Se o cliente envia os parâmetros de ação em incorretos formato, o valor de **ModelState** é false. Marque esse sinalizador em seu método de controlador e retornar um erro se **IsValid** é false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Exemplo: Adicionar uma função

Agora vamos adicionar uma função do OData que retorna o produto mais caro. Como antes, a primeira etapa é adicionar a função ao EDM. Em WebApiConfig.cs, adicione o código a seguir.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Nesse caso, a função está associada para a coleção de produtos, em vez de instâncias individuais do produto. Os clientes invocar a função enviando uma solicitação GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Aqui está o método do controlador para esta função:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Observe que o nome do método corresponde ao nome de função. O **[HttpGet]** atributo especifica o método é um método HTTP GET.

Aqui está a resposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Exemplo: Adicionar uma função não associada

O exemplo anterior era uma função associada a uma coleção. Neste exemplo, vamos criar uma *desassociado* função. Funções não associadas são chamadas como estáticas operações no serviço. A função neste exemplo retornará o imposto sobre vendas para um determinado código postal.

No arquivo WebApiConfig, adicione a função ao EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Observe que estamos chamando **função** diretamente sobre o **ODataModelBuilder**, em vez do tipo de entidade ou a coleção. Isso informa ao construtor de modelo que a função é não associada.

Aqui está o método do controlador que implementa a função:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Não importa qual controlador de API da Web é colocar esse método no. Você poderá colocá-lo `ProductsController`, ou definir um controlador separado. O **[ODataRoute]** atributo define o modelo de URI para a função.

Aqui está um exemplo de solicitação de cliente:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

A resposta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
