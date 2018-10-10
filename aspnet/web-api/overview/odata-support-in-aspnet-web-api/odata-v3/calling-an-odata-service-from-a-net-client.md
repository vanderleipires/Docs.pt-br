---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Chamar um serviço OData de um cliente .NET (c#) | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como chamar um serviço OData de um aplicativo cliente c#. Versões de software usadas no tutorial do Visual Studio 2013 (funciona com Visual s....
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 75f8e3eab7bd5667bbdcccbb5ae8a8e5b1f5fdba
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912046"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Chamar um serviço OData de um cliente .NET (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Este tutorial mostra como chamar um serviço OData de um aplicativo cliente c#.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funciona com o Visual Studio 2012)
> - [WCF Data Services Client Library](https://msdn.microsoft.com/library/cc668772.aspx) (Biblioteca de clientes do WCF Data Services)
> - API Web 2. (O serviço OData de exemplo é criado usando a API Web 2, mas o aplicativo cliente não depende do API da Web).


Neste tutorial, eu explicarei como criar um aplicativo cliente que chama um serviço OData. O serviço OData expõe as seguintes entidades:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Os artigos a seguir descrevem como implementar o serviço OData na API da Web. (Você não precisará lê-los para entender neste tutorial, no entanto.)

- [Criando um ponto de extremidade OData na API Web 2](creating-an-odata-endpoint.md)
- [Relações de entidade do OData na API Web 2](working-with-entity-relations.md)
- [Ações de OData na API Web 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Gerar o Proxy de serviço

A primeira etapa é gerar um proxy de serviço. O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData. O proxy converte as chamadas de método em solicitações HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Comece abrindo o projeto do serviço OData no Visual Studio. Pressione CTRL + F5 para executar o serviço localmente no IIS Express. Anote o endereço local, incluindo o número da porta que o Visual Studio atribui. Quando você cria o proxy, você precisará desse endereço.

Em seguida, abra outra instância do Visual Studio e crie um projeto de aplicativo de console. O aplicativo de console será nosso aplicativo de cliente do OData. (Você também pode adicionar o projeto para a mesma solução que o serviço.)

> [!NOTE]
> As etapas restantes consulte o projeto de console.


No Gerenciador de soluções, clique com botão direito **referências** e selecione **Add Service Reference**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

No **adicionar referência de serviço** caixa de diálogo, digite o endereço do serviço OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

em que *porta* é o número da porta.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Para **Namespace**, digite "ProductService". Essa opção define o namespace da classe de proxy.

Clique em **acesse**. Visual Studio lê o documento de metadados para descobrir as entidades no serviço do OData.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Clique em **Okey** para adicionar a classe de proxy ao seu projeto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Criar uma instância da classe de Proxy de serviço

Dentro de seu `Main` método, crie uma nova instância da classe de proxy, da seguinte maneira:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Novamente, use o número de porta real em que o seu serviço está em execução. Quando você implanta seu serviço, você usará o URI do serviço ativo. Você não precisa atualizar o proxy.

O código a seguir adiciona um manipulador de eventos que imprime os URIs de solicitação para a janela do console. Essa etapa não é necessária, mas é interessante ver os URIs para cada consulta.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>O serviço de consulta

O código a seguir obtém a lista de produtos do serviço OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Observe que você não precisa escrever nenhum código para enviar a solicitação HTTP ou analisar a resposta. A classe de proxy faz isso automaticamente quando você enumerar os `Container.Products` coleta na **foreach** loop.

Quando você executa o aplicativo, a saída deve ser semelhante ao seguinte:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Para obter uma entidade por ID, use um `where` cláusula.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Para o restante deste tópico, não vou mostrar todo o `Main` funcionar, apenas o código necessário para chamar o serviço.

## <a name="apply-query-options"></a>Aplicar as opções de consulta

OData define [opções de consulta](../supporting-odata-query-options.md) que pode ser usado para filtrar, classificar, dados da página e assim por diante. No proxy de serviço, você pode aplicar essas opções usando várias expressões de LINQ.

Nesta seção, vou mostrar breves exemplos. Para obter mais detalhes, consulte o tópico [considerações sobre o LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) no MSDN.

### <a name="filtering-filter"></a>Filtragem ($filter)

Para filtrar, use um `where` cláusula. A exemplo a seguir filtra por categoria de produto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Esse código corresponde à consulta de OData a seguir.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Observe que o proxy converte o `where` cláusula em um OData `$filter` expressão.

### <a name="sorting-orderby"></a>Classificação ($orderby)

Para classificar, use um `orderby` cláusula. O exemplo a seguir classifica por preço, em ordem decrescente.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Aqui está a solicitação correspondente do OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paginação do cliente ($skip Skip e $top)

Para grandes conjuntos de entidade, o cliente talvez queira limitar o número de resultados. Por exemplo, um cliente pode mostrar 10 entradas por vez. Isso é chamado *paginação do lado do cliente*. (Também há [paginação de servidor](../supporting-odata-query-options.md#server-paging), em que o servidor limita o número de resultados.) Para realizar a paginação do lado do cliente, use o LINQ **Skip** e **levar** métodos. O exemplo a seguir ignora os primeiros 40 resultados e usa os 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Aqui está a solicitação correspondente do OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Selecione ($select) e Expand ($expand)

Para incluir as entidades relacionadas, use o `DataServiceQuery<t>.Expand` método. Por exemplo, para incluir a `Supplier` para cada `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Aqui está a solicitação correspondente do OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Para alterar a forma da resposta, use o LINQ **selecionar** cláusula. O exemplo a seguir obtém apenas o nome de cada produto, com nenhuma outra propriedade.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Aqui está a solicitação correspondente do OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Uma cláusula select pode incluir entidades relacionadas. Nesse caso, não chame **expandir**; o proxy automaticamente inclui a expansão neste caso. O exemplo a seguir obtém o nome e o fornecedor de cada produto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Aqui está a solicitação correspondente do OData. Observe que ele inclui o **$expand** opção.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Para obter mais informações sobre $select e $expandir, consulte [usando $select, $expand e $value na API Web 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Adicionar uma nova entidade

Para adicionar uma nova entidade para um conjunto de entidades, chame `AddToEntitySet`, onde *EntitySet* é o nome do conjunto de entidades. Por exemplo, `AddToProducts` adiciona um novo `Product` para o `Products` conjunto de entidades. Quando você gerar o proxy, WCF Data Services automaticamente cria esses fortemente tipadas **AdicionarPara** métodos.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Para adicionar um link entre duas entidades, use o **AddLink** e **SetLink** métodos. O código a seguir adiciona um novo fornecedor e um novo produto e, em seguida, cria links entre eles.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Use **AddLink** quando a propriedade de navegação é uma coleção. Neste exemplo, estamos adicionando um produto para o `Products` coleta sobre o fornecedor.

Use **SetLink** quando a propriedade de navegação é uma única entidade. Neste exemplo, definimos o `Supplier` propriedade no produto.

## <a name="update--patch"></a>Atualizar / Aplicar Patch

Para atualizar uma entidade, chame o **UpdateObject** método.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

A atualização é executada quando você chama **SaveChanges**. Por padrão, o WCF envia uma solicitação HTTP MERGE. O **PatchOnUpdate** opção informa ao WCF para enviar uma HTTP PATCH.

> [!NOTE]
> Por que o PATCH versus mesclagem? A especificação HTTP 1.1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) não definiu qualquer método HTTP com a semântica de "atualização parcial". Para dar suporte a atualizações parciais, a especificação de OData definido o método de mesclagem. Em 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definido o método de PATCH para atualizações parciais. Você pode ler algumas do histórico desta [postagem de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) no Blog do WCF Data Services. Hoje, PATCH é preferível direta. O controlador de OData criado pelo scaffolding de API da Web dá suporte a ambos os métodos.


Se você deseja substituir a entidade inteira (semântica PUT), especifique o **ReplaceOnUpdate** opção. Isso faz com que o WCF enviar uma solicitação HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Excluir uma entidade

Para excluir uma entidade, chame **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Invocar uma ação do OData

Em OData, [ações](odata-actions.md) são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD nas entidades.

Embora o documento de metadados OData descreve as ações, a classe de proxy não cria todos os métodos fortemente tipados para eles. Você ainda pode invocar uma ação OData usando o genérico **Execute** método. No entanto, você precisará saber os tipos de dados dos parâmetros e o valor de retorno.

Por exemplo, o `RateProduct` ação leva um parâmetro denominado "Classificação" do tipo `Int32` e retorna um `double`. O código a seguir mostra como invocar essa ação.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Para obter mais informações, consulte[chamar operações de serviço e ações](https://msdn.microsoft.com/library/hh230677.aspx).

Uma opção é estender a **contêiner** classe para fornecer um método com rigidez de tipos que invoca a ação:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
