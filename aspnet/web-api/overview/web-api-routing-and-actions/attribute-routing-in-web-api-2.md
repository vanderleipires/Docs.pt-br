---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atributo de roteamento de ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 67ab1536b4a72abf8c0d3ed5aa0c48bc79a8fb5f
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Roteamento de atributo em ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

*Roteamento* é como a API da Web corresponde um URI para uma ação. Web API 2 dá suporte a um novo tipo de roteamento, chamado *roteamento de atributo*. Como o nome implica, roteamento de atributo usa atributos para definir rotas. Roteamento de atributo fornece mais controle sobre os URIs em sua API da web. Por exemplo, você pode criar facilmente URIs que descrevem as hierarquias de recursos.

O estilo anterior de roteamento baseado em convenção de chamada roteamento é total suporte. Na verdade, você pode combinar as duas técnicas no mesmo projeto.

Este tópico mostra como habilitar o roteamento de atributo e descreve as várias opções para roteamento de atributo. Para um tutorial de ponta a ponta que usa o roteamento de atributo, consulte [criar uma API REST com atributo roteamento na API Web 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Pré-requisitos

[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition

Como alternativa, use o NuGet Package Manager para instalar os pacotes necessários. Do **ferramentas** menu do Visual Studio, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Digite o seguinte comando na janela do Console do Gerenciador de pacotes:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Por que atributo roteamento?

A primeira versão da API da Web usado *baseado em convenção* roteamento. Esse tipo de roteamento, você define um ou mais modelos de rota, que são basicamente parametrizadas cadeias de caracteres. Quando a estrutura recebe uma solicitação, ele corresponde o URI em relação ao modelo de rota. (Para obter mais informações sobre roteamento baseado em convenção, consulte [roteamento na API da Web ASP.NET](routing-in-aspnet-web-api.md).

Uma vantagem de roteamento baseado em convenção é que os modelos são definidos em um único local, e as regras de roteamento são aplicadas de forma consistente em todos os controladores. Infelizmente, roteamento baseado em convenção dificulta dar suporte a determinados padrões URI que são comuns em APIs RESTful. Por exemplo, recursos geralmente contêm recursos filho: os clientes tiverem pedidos, filmes tem atores, livros têm autores e assim por diante. É natural para criar URIs que reflitam essas relações:

`/customers/1/orders`

Esse tipo de URI é difícil criar usando roteamento baseado em convenção. Embora isso pode ser feito, os resultados não escala bem se você tiver muitos controladores ou tipos de recurso.

Com o roteamento de atributo, é comum para definir uma rota para esse URI. Você simplesmente adicionar um atributo de ação do controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Aqui estão alguns outros padrões esse atributo roteamento torna fácil.

**Controle de versão de API**

Neste exemplo, "/ v1/api/produtos" deve ser roteada para um controlador diferente de "/ v2/api/produtos".

`/api/v1/products`  
`/api/v2/products`

**Segmentos URI sobrecarregados**

Neste exemplo, "1" é um número de ordem, mas "pendente" é mapeado para uma coleção.

`/orders/1`  
`/orders/pending`

**Vários tipos de parâmetro**

Neste exemplo, "1" é um número de ordem, mas "2013/06/16" Especifica uma data.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Habilitar o roteamento de atributo

Para habilitar o roteamento de atributo, chame **MapHttpAttributeRoutes** durante a configuração. Esse método de extensão é definido no **System.Web.Http.HttpConfigurationExtensions** classe.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Roteamento de atributo pode ser combinado com [baseado em convenção](routing-in-aspnet-web-api.md) roteamento. Para definir rotas baseado em convenção, chame o **MapHttpRoute** método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Para obter mais informações sobre como configurar a API da Web, consulte [Configurando o ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Observação: Migrando do API Web 1

Antes da API Web 2, os modelos de projeto de API da Web gerado código como este:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Se o roteamento de atributo estiver habilitado, esse código gera uma exceção. Se você atualizar um projeto de API da Web existente para usar o roteamento de atributo, certifique-se atualizar esse código de configuração para o seguinte:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Para obter mais informações, consulte [Configurando Web API com hospedagem ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Adicionando atributos de rota

Aqui está um exemplo de uma rota definida usando um atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

A cadeia de caracteres &quot;clientes / {customerId} / ordena&quot; é o modelo URI para a rota. API da Web tenta corresponder o URI de solicitação para o modelo. Neste exemplo, "clientes" e "orders" são literais segmentos e "{customerId}" é um parâmetro de variável. Os URIs a seguir corresponderia a este modelo:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Você pode restringir a correspondência usando [restrições](#constraints), descrito mais adiante neste tópico.

Observe que o &quot;{customerId}&quot; parâmetro no modelo de rota corresponde ao nome do *customerId* parâmetro no método. Quando a API da Web chama a ação de controlador, ele tenta associar os parâmetros de rota. Por exemplo, se o URI é `http://example.com/customers/1/orders`, API da Web tenta associar o valor "1" para o *customerId* parâmetro na ação.

Um modelo de URI pode ter vários parâmetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Qualquer método de controlador que não tem um atributo da rota usa roteamento baseado em convenção. Dessa forma, você pode combinar os dois tipos de roteamento no mesmo projeto.

## <a name="http-methods"></a>Métodos HTTP

API da Web também seleciona ações com base no método de HTTP da solicitação (GET, POST, etc.). Por padrão, a API da Web procura uma correspondência de maiusculas e minúsculas com o início do nome do método do controlador. Por exemplo, um método de controlador chamado `PutCustomers` corresponde a uma solicitação HTTP PUT.

Você pode substituir essa convenção, decorar o método com qualquer os seguintes atributos:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

O exemplo a seguir mapeia o método CreateBook para solicitações HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Para todos os outros métodos HTTP, incluindo métodos não padrão, usem o **AcceptVerbs** atributo, que usa uma lista de métodos HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefixos de rota

Geralmente, as rotas em um controlador iniciados com o mesmo prefixo. Por exemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Você pode definir um prefixo comum para um controlador de inteiro usando o **[RoutePrefix]** atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Use um til (~) no atributo de método para substituir o prefixo da rota:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

O prefixo da rota pode incluir parâmetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Restrições da rota

Restrições da rota permitem que você restrinja como os parâmetros do modelo de rota são correspondidos. A sintaxe geral é &quot;{: restrição de parâmetro}&quot;. Por exemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Aqui, a primeira rota somente será selecionada se o &quot;id&quot; segmento do URI é um inteiro. Caso contrário, a segunda rota será escolhida.

A tabela a seguir lista as restrições que são suportadas.

| Restrição | Descrição | Exemplo |
| --- | --- | --- |
| Alpha | Correspondências maiusculas ou minúsculas caracteres do alfabeto latino (a-z, A-Z) | {x:alpha} |
| bool | Corresponde a um valor booliano. | {x:bool} |
| datetime | Correspondências um **DateTime** valor. | {x:datetime} |
| decimal | Corresponde a um valor decimal. | {x:decimal} |
| double | Corresponde a um valor de ponto flutuante de 64 bits. | {x:double} |
| float | Corresponde a um valor de ponto flutuante de 32 bits. | {x:float} |
| GUID | Corresponde a um valor GUID. | {x:guid} |
| int | Corresponde a um valor inteiro de 32 bits. | {x:int} |
| length | Corresponde a uma cadeia de caracteres com o comprimento especificado ou dentro de um intervalo de comprimentos especificado. | {x:length(6)} {x:length(1,20)} |
| long | Corresponde a um valor inteiro de 64 bits. | {x:long} |
| max | Corresponde a um número inteiro com um valor máximo. | {x:max(10)} |
| MaxLength | Corresponde a uma cadeia de caracteres com um comprimento máximo. | {x:maxlength(10)} |
| min | Corresponde a um número inteiro com um valor mínimo. | {x:min(10)} |
| minLength | Corresponde a uma cadeia de caracteres com um comprimento mínimo. | {x:minlength(10)} |
| range | Corresponde a um número inteiro dentro do intervalo de valores. | {x:range(10,50)} |
| Regex | Corresponde a uma expressão regular. | {x:regex(^\d{3}-\d{3}-\d{4}$)} |

Observe que algumas das restrições, como &quot;min&quot;, obtém argumentos entre parênteses. Você pode aplicar várias restrições a um parâmetro, separado por dois-pontos.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Restrições da rota personalizada

Você pode criar restrições de rota personalizados ao implementar o **IHttpRouteConstraint** interface. Por exemplo, a seguinte restrição restringe um parâmetro para um valor inteiro diferente de zero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

O código a seguir mostra como registrar a restrição:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Agora você pode aplicar a restrição em suas rotas:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Você também pode substituir toda a **DefaultInlineConstraintResolver** classe implementando o **IInlineConstraintResolver** interface. Isso substituirá todas as restrições internas, a menos que a implementação do **IInlineConstraintResolver** adiciona especificamente.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Valores padrão e parâmetros URI opcional

Você pode fazer um parâmetro URI opcional com a adição de um ponto de interrogação para o parâmetro de rota. Se um parâmetro de rota é opcional, você deve definir um valor padrão para o parâmetro do método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

Neste exemplo, `/api/books/locale/1033` e `/api/books/locale` retornam o mesmo recurso.

Como alternativa, você pode especificar um valor padrão dentro do modelo de rota, da seguinte maneira:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Isso é quase igual ao exemplo anterior, mas há uma pequena diferença de comportamento quando o valor padrão é aplicado.

- No primeiro exemplo ("{lcid?}"), o valor padrão de 1033 é atribuído diretamente para o parâmetro do método, portanto, o parâmetro terá esse valor exato.
- No segundo exemplo ("{lcid = 1033}"), o valor padrão de "1033" atravessa o processo de associação de modelo. O associador de modelo padrão será converter "1033" para o valor numérico 1033. No entanto, você pode conectar um associador de modelo personalizado, que pode fazer algo diferente.

(Na maioria dos casos, a menos que você tenha associadores de modelo personalizado em seu pipeline, as duas formas será equivalentes.)

<a id="route-names"></a>
## <a name="route-names"></a>Nomes de rota

Na API da Web, cada rota tem um nome. Os nomes de rota são úteis para gerar links, para que você pode incluir um link em uma resposta HTTP.

Para especificar o nome da rota, defina o **nome** propriedade no atributo. O exemplo a seguir mostra como definir o nome da rota e também como usar o nome da rota ao gerar um link.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordem de rota

Quando a estrutura tenta corresponder a um URI com uma rota, ela avalia as rotas em uma ordem específica. Para especificar a ordem, defina o **RouteOrder** propriedade no atributo da rota. Valores mais baixos são avaliados primeiro. O valor de ordem padrão é zero.

Aqui está como a ordenação total é determinada:

1. Comparar o **RouteOrder** propriedade do atributo da rota.
2. Examine cada segmento URI no modelo de rota. Para cada segmento, ordem da seguinte maneira: 

    1. Segmentos de literal.
    2. Parâmetros de rota com restrições.
    3. Parâmetros de rota sem restrições.
    4. Segmentos de parâmetro de curinga com restrições.
    5. Curinga segmentos de parâmetro sem restrições.
3. No caso de uma associação, as rotas são ordenadas por uma comparação de cadeia de caracteres maiusculas e minúsculas ordinal ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) do modelo de rota.

Vejamos um exemplo. Suponha que você definir o controlador a seguir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Essas rotas são ordenadas da seguinte maneira.

1. detalhes dos pedidos /
2. orders/{id}
3. orders/{customerName}
4. pedidos / {\*data}
5. pedidos / pendente

Observe que "Detalhes" é um segmento literal e aparece antes de "{id}", mas "pendente" aparece última porque o **RouteOrder** propriedade é 1. (Este exemplo assume que há é nenhum cliente chamada "Detalhes" ou "pendente". Em geral, tente evitar rotas ambíguas. Neste exemplo, um modelo de rota melhor para `GetByCustomer` é "clientes / {customerName}")
