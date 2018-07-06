---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Roteamento na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/11/2012
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ed9c575c448563307a0657e734076962fe067164
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825402"
---
<a name="routing-in-aspnet-web-api"></a>Roteamento na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve como o API Web ASP.NET encaminha solicitações HTTP para controladores.

> [!NOTE]
> Se você estiver familiarizado com o ASP.NET MVC, roteamento API Web é muito semelhante ao roteamento MVC. A principal diferença é que a API Web usa o método HTTP, não o caminho URI, para selecionar a ação. Você também pode usar o roteamento na API Web no estilo MVC. Este artigo não assume nenhum conhecimento do ASP.NET MVC.


## <a name="routing-tables"></a>Tabelas de roteamento

Na API Web ASP.NET, uma *controlador* é uma classe que manipula as solicitações HTTP. Os métodos públicos do controlador são chamados *métodos de ação* ou simplesmente *ações*. Quando a estrutura da API Web recebe uma solicitação, ele encaminha a solicitação para uma ação.

Para determinar qual ação a ser invocada, a estrutura usa um *tabela de roteamento*. O modelo de projeto do Visual Studio para API da Web cria uma rota padrão:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Essa rota é definida no arquivo WebApiConfig.cs, que é colocado no aplicativo\_diretório inicial:

![](routing-in-aspnet-web-api/_static/image1.png)

Para obter mais informações sobre o **WebApiConfig** classe, consulte [Configurando o ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Se você hospedar internamente o API da Web, você deve definir a tabela de roteamento diretamente na **HttpSelfHostConfiguration** objeto. Para obter mais informações, consulte [hospedar internamente uma API Web](../older-versions/self-host-a-web-api.md).

Cada entrada na tabela de roteamento contém um *modelo de rota*. É o modelo de rota padrão para a API da Web &quot;api / {controller} / {id}&quot;. Neste modelo, &quot;api&quot; é um segmento de caminho literal e {controlador} e {id} são variáveis de espaço reservado.

Quando a estrutura da API Web recebe uma solicitação HTTP, ele tenta corresponder o URI em relação a um dos modelos de rota na tabela de roteamento. Se nenhuma rota corresponde, o cliente recebe um erro 404. Por exemplo, os URIs a seguir corresponde a rota padrão:

- / api/contatos
- /API/Contacts/1
- /API/Products/gizmo1

No entanto, o seguinte URI não corresponde, porque ele não tem o &quot;api&quot; segmento:

- / Contatos/1

> [!NOTE]
> O motivo para usar "api" na rota é evitar colisões com o roteamento do ASP.NET MVC. Dessa forma, você pode ter &quot;/entra em contato com&quot; vá para um controlador MVC, e &quot;/api/contatos&quot; vá para um controlador de API da Web. É claro que, se você não gostar essa convenção, você pode alterar a tabela de rotas padrão.

Depois que uma rota correspondente for encontrada, a API da Web seleciona o controlador e a ação:

- Para localizar o controlador, a API da Web adiciona &quot;controlador&quot; para o valor da *{controlador}* variável.
- Para localizar a ação, a API da Web examina o método HTTP e, em seguida, procura por uma ação cujo nome começa com esse nome de método HTTP. Por exemplo, API da Web com uma solicitação GET, procura por uma ação que inicia com &quot;obter... &quot;, como &quot;GetContact&quot; ou &quot;GetAllContacts&quot;. Essa convenção aplica-se somente para GET, POST, PUT e DELETE métodos. Você pode habilitar outros métodos HTTP por meio de atributos em seu controlador. Veremos um exemplo disso mais tarde.
- Outras variáveis de espaço reservado no modelo de rota, como *{id},* são mapeados para parâmetros de ação.

Vamos examinar um exemplo. Suponha que você defina o controlador a seguir:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Aqui estão algumas solicitações HTTP possíveis, junto com a ação que é invocado para cada:

| Método HTTP | Caminho de URI | Ação | Parâmetro |
| --- | --- | --- | --- |
| OBTER | API/produtos | GetAllProducts | *(nenhum)* |
| OBTER | produtos/API/4 | GetProductById | 4 |
| DELETE | produtos/API/4 | DeleteProduct | 4 |
| POSTAR | API/produtos | *(nenhuma correspondência)* |  |

Observe que o *{id}* segmento do URI, se presente, é mapeado para o *id* parâmetro da ação. Neste exemplo, o controlador define dois métodos GET com um *id* parâmetro e um sem parâmetros.

Além disso, observe que a solicitação POST falhará, pois o controlador não define uma &quot;Post... &quot; método.

## <a name="routing-variations"></a>Variações de roteamentos

A seção anterior descreveu o mecanismo de roteamento básico de API Web do ASP.NET. Esta seção descreve algumas variações.

### <a name="http-methods"></a>Métodos HTTP

Em vez de usar a convenção de nomenclatura para métodos HTTP, você pode especificar explicitamente o método HTTP para uma ação decorando o método de ação com o **HttpGet**, **HttpPut**, **HttpPost** , ou **HttpDelete** atributo.

No exemplo a seguir, o método FindProduct é mapeado para solicitações GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Para permitir que vários métodos HTTP para uma ação ou para permitir que os métodos HTTP diferente de GET, PUT, POST e DELETE, use o **AcceptVerbs** atributo, que utiliza uma lista de métodos HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Roteamento pelo nome da ação

Com o modelo de roteamento padrão, a API da Web usa o método HTTP para selecionar a ação. No entanto, você também pode criar uma rota em que o nome da ação está incluído no URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Neste modelo de rota, o *{action}* nomes de parâmetro de método de ação no controlador. Com esse estilo de roteamento, use atributos para especificar os métodos HTTP permitidos. Por exemplo, suponha que seu controlador tem o seguinte método:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Nesse caso, uma solicitação GET para "api/produtos/detalhes/1" seria mapeados para o método de detalhes. Esse estilo de roteamento é semelhante ao ASP.NET MVC e pode ser apropriado para uma API de estilo RPC.

Você pode substituir o nome da ação usando o **ActionName** atributo. No exemplo a seguir, há duas ações que mapeiam &quot;produtos/api/miniatura/*id*. Um oferece suporte a GET e o outro oferece suporte a POSTAGEM:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Não ações

Para impedir que um método sendo invocada como uma ação, use o **NonAction** atributo. Isso sinaliza para o framework que o método não é uma ação, mesmo que ele tenha correspondência as regras de roteamento.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Leitura adicional

Este tópico forneceu uma visão geral do roteamento. Para obter mais detalhes, consulte [roteamento e seleção de ação](routing-and-action-selection.md), que descreve exatamente como o framework corresponde a um URI para uma rota, seleciona um controlador e, em seguida, seleciona a ação a ser invocada.
