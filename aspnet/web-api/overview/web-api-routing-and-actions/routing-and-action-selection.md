---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Roteamento e seleção de ação na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043412"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Roteamento e seleção de ação na API da Web do ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve como o ASP.NET Web API encaminha uma solicitação HTTP para uma ação específica em um controlador de.

> [!NOTE]
> Para obter uma visão geral do roteamento, consulte [roteamento na API da Web ASP.NET](routing-in-aspnet-web-api.md).


Este artigo aborda os detalhes do processo de roteamento. Se você criar um projeto de API da Web e localizar algumas solicitações não obtém roteada da maneira esperada, esperamos que este artigo ajudará.

Roteamento tem três fases principais:

1. Combinar a URI para um modelo de rota.
2. Selecionar um controlador.
3. Selecionar uma ação.

Você pode substituir algumas partes do processo com seus próprios comportamentos personalizados. Neste artigo, eu descrevem o comportamento padrão. No final, observe os locais onde você pode personalizar o comportamento.

## <a name="route-templates"></a>Modelos de rota

Um modelo de rota é semelhante a um caminho de URI, mas ele pode ter valores de espaço reservado, indicados entre chaves:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Quando você cria uma rota, você pode fornecer valores padrão para alguns ou todos os espaços reservados:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Você também pode fornecer restrições, que restringem como um segmento URI pode corresponder a um espaço reservado:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

A estrutura tenta corresponder os segmentos no caminho de URI para o modelo. Literais no modelo devem corresponder exatamente. Um espaço reservado corresponde a qualquer valor, a menos que você especifica restrições. A estrutura não corresponde a outras partes do URI, como o nome do host ou os parâmetros de consulta. A estrutura seleciona a primeira rota na tabela de rotas que coincide com o URI.

Há dois espaços reservados de especiais: "{controlador}" e "{action}".

- "{controlador}" fornece o nome do controlador.
- "{action}" fornece o nome da ação. Na API da Web, a convenção comum é omitir "{action}".

### <a name="defaults"></a>Padrão

Se você fornecer padrões, a rota corresponderá a um URI que falta esses segmentos. Por exemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

O URI "`http://localhost/api/products`" corresponde a essa rota. O segmento '{categoria}' é atribuído o valor padrão "todos".

### <a name="route-dictionary"></a>Dicionário de rota

Se a estrutura de encontrar uma correspondência para um URI, ele cria um dicionário que contém o valor para cada espaço reservado. As chaves são os nomes de espaço reservado, não incluindo as chaves. Os valores são obtidos do caminho de URI ou dos padrões. O dicionário é armazenado no **IHttpRouteData** objeto.

Durante essa fase rota correspondente, o especial "{controlador}" e os espaços reservados de "{action}" são tratados como espaços reservados para outros. Eles são simplesmente armazenados no dicionário com os outros valores.

Um padrão pode ter o valor especial **RouteParameter.Optional**. Se um espaço reservado é atribuído a esse valor, o valor não é adicionado ao dicionário de rota. Por exemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Para o caminho URI "api/produtos", o dicionário de rota conterá:

- controlador: "produtos"
- categoria: "all"

Para "api/produtos/toys/123", no entanto, o dicionário de rota conterá:

- controlador: "produtos"
- categoria: "toys"
- id: "123"

Os padrões também podem incluir um valor que não aparecer em qualquer lugar no modelo de rota. Se a rota corresponde, esse valor é armazenado no dicionário. Por exemplo:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Se o caminho do URI é "raiz/api/8", o dicionário conterá dois valores:

- controlador: "clientes"
- id: "8"

## <a name="selecting-a-controller"></a>Selecionar um controlador

Seleção de controlador é tratada pelo **IHttpControllerSelector.SelectController** método. Esse método usa um **HttpRequestMessage** instância e retorna um **HttpControllerDescriptor**. A implementação padrão fornecida pelo **DefaultHttpControllerSelector** classe. Essa classe usa um algoritmo simples:

1. Examine o dicionário de rota para a chave "controller".
2. O valor para essa chave e acrescente a cadeia de caracteres "Controlador" para obter o nome do tipo de controlador.
3. Procure um controlador Web API com esse nome de tipo.

Por exemplo, se o dicionário de rota contém o par chave-valor "controlador" = "produtos", em seguida, o tipo de controlador é "ProductsController". Se não houver nenhum tipo de correspondência ou várias correspondências, o framework retornará um erro ao cliente.

Etapa 3, **DefaultHttpControllerSelector** usa o **IHttpControllerTypeResolver** interface para obter a lista de tipos de controlador de API da Web. A implementação padrão de **IHttpControllerTypeResolver** retorna todas as classes públicas que implementam (a) **IHttpController**, (b) são não abstrato e (c) tem um nome que termina em "Controller".

## <a name="action-selection"></a>Seleção de ação

Depois de selecionar o controlador, o framework seleciona a ação chamando o **IHttpActionSelector.SelectAction** método. Esse método usa um **HttpControllerContext** e retorna um **HttpActionDescriptor**.

A implementação padrão fornecida pelo **ApiControllerActionSelector** classe. Para selecionar uma ação, ele verifica o seguinte:

- O método HTTP da solicitação.
- O espaço reservado de "{action}" no modelo de rota, se presente.
- Os parâmetros de ações no controlador.

Antes de examinar o algoritmo de seleção, é preciso entender algumas coisas sobre ações do controlador.

**Os métodos do controlador são considerados "ações"?** Ao selecionar uma ação, a estrutura considera apenas em métodos de instância pública no controlador. Além disso, ele exclui ["nome especial"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) métodos (construtores, eventos, sobrecargas de operador e assim por diante) e métodos herdados do **ApiController** classe.

**Métodos HTTP.** A estrutura escolhe somente ações que correspondam o método HTTP da solicitação, determinado da seguinte maneira:

1. Você pode especificar o método HTTP com um atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, ou **HttpPut**.
2. Caso contrário, se o nome do método do controlador começa com "Get", "Post", "Put", "Delete", "Head", "Opções" ou "Patch", em seguida, por convenção, a ação dá suporte a esse método HTTP.
3. Se nenhuma das opções acima, o método dá suporte à POSTAGEM.

**Associações de parâmetro.** Uma associação de parâmetros é como a API da Web cria um valor para um parâmetro. Aqui está a regra padrão para a associação de parâmetros:

- Tipos simples são obtidos de URI.
- Tipos complexos são obtidos de corpo da solicitação.

Tipos simples incluem todas as [tipos primitivos do .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), além de **DateTime**, **Decimal**, **Guid**, **cadeia de caracteres** , e **TimeSpan**. Para cada ação, no máximo um parâmetro pode ler o corpo da solicitação.

> [!NOTE]
> É possível substituir as regras de associação padrão. Consulte [associação de parâmetro WebAPI nos bastidores](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Com esse plano de fundo, aqui está o algoritmo de seleção de ação.

1. Crie uma lista de todas as ações do controlador que corresponde o método de solicitação HTTP.
2. Se o dicionário de rota tem uma entrada de "ação", remova ações cujo nome não corresponde a esse valor.
3. Tenta corresponder a parâmetros de ação para o URI, da seguinte maneira: 

    1. Para cada ação, obtenha uma lista de parâmetros que são um tipo simples, onde a associação obtém o parâmetro do URI. Exclua parâmetros opcionais.
    2. Nesta lista, tente encontrar uma correspondência para cada nome de parâmetro no dicionário de rota ou na cadeia de caracteres de consulta URI. Correspondências diferenciam maiusculas de minúsculas e não dependem da ordem dos parâmetros.
    3. Selecione uma ação em que cada parâmetro na lista tem uma correspondência no URI.
    4. Se mais que uma ação atenda a esses critérios, selecione aquela com as maioria das correspondências de parâmetro.
4. Ignorar as ações com o **[NonAction]** atributo.

Etapa 3 # é provavelmente mais confusos. A ideia básica é que um parâmetro pode obter seu valor do URI, o corpo da solicitação ou de uma associação personalizada. Para parâmetros que vêm do URI, queremos garantir que o URI contém um valor para esse parâmetro, o caminho (por meio do dicionário de rota) ou na cadeia de caracteres de consulta.

Por exemplo, considere a seguinte ação:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

O *id* parâmetro associa-se para o URI. Portanto, esta ação só pode corresponder a um URI que contém um valor para "id", no dicionário de rota ou na cadeia de caracteres de consulta.

Parâmetros opcionais são uma exceção, porque eles são opcionais. Para um parâmetro opcional, é Okey se a associação não é possível obter o valor do URI.

Tipos complexos são uma exceção para um motivo diferente. Um tipo complexo só pode ser associado ao URI por meio de uma associação personalizada. Mas, nesse caso, a estrutura não é possível saber com antecedência se o parâmetro deve vincular um URI específico. Para descobrir, seria necessário invocar a associação. É o objetivo do algoritmo de seleção Selecionar uma ação da descrição de estático, antes de chamar qualquer associações. Portanto, os tipos complexos são excluídos do algoritmo de correspondência.

Depois que a ação for selecionada, todas as associações de parâmetro são invocadas.

Resumo:

- A ação deve corresponder o método HTTP da solicitação.
- O nome da ação deve corresponder à entrada de "ação" no dicionário de rota, se presente.
- Para cada parâmetro da ação, se o parâmetro é obtido de URI, em seguida, o nome do parâmetro deve ser encontrado no dicionário de rota ou na cadeia de caracteres de consulta URI. (Parâmetros com tipos complexos e parâmetros opcionais são excluídos.)
- Tenta corresponder o maior número de parâmetros. A melhor correspondência pode ser um método sem parâmetros.

## <a name="extended-example"></a>Exemplo estendido

Rotas:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controlador:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Solicitação HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Correspondência de rotas

O URI corresponde a rota chamada "DefaultApi". O dicionário de rota contém as seguintes entradas:

- controlador: "produtos"
- id: "1"

O dicionário de rota não contêm parâmetros de cadeia de caracteres de consulta, "versão" e "Detalhes", mas eles ainda serão considerados durante a seleção de ação.

### <a name="controller-selection"></a>Seleção de controlador

Da entrada "controller" no dicionário de rota, o tipo de controlador é `ProductsController`.

### <a name="action-selection"></a>Seleção de ação

A solicitação HTTP é uma solicitação GET. As ações do controlador que suportam GET são `GetAll`, `GetById`, e `FindProductsByName`. O dicionário de rota não contém uma entrada para "ação", não sendo necessário corresponder ao nome de ação.

Em seguida, tentamos corresponder nomes de parâmetro para as ações, apenas observando as ações de GET.

| Ação | Parâmetros para correspondência |
| --- | --- |
| `GetAll` | nenhum |
| `GetById` | "id" |
| `FindProductsByName` | "nome" |

Observe que o *versão* parâmetro `GetById` não é considerado, porque ele é um parâmetro opcional.

O `GetAll` método corresponde facilmente. O `GetById` método também corresponde, porque o dicionário de rota contém "id". O `FindProductsByName` método não corresponde.

O `GetById` método wins, porque ele corresponde a um parâmetro, em vez de nenhum parâmetro para `GetAll`. O método é invocado com os seguintes valores de parâmetro:

- *id* = 1
- *versão* = 1.5

Observe que, embora *versão* não foi usado o algoritmo de seleção, o valor do parâmetro vêm de cadeia de caracteres de consulta URI.

## <a name="extension-points"></a>Pontos de extensão

API da Web fornece pontos de extensão para algumas partes do processo de roteamento.

| Interface | Descrição |
| --- | --- |
| **IHttpControllerSelector** | Seleciona o controlador. |
| **IHttpControllerTypeResolver** | Obtém a lista de tipos de controlador. O **DefaultHttpControllerSelector** escolhe o tipo de controlador nesta lista. |
| **IAssembliesResolver** | Obtém a lista de assemblies de projeto. O **IHttpControllerTypeResolver** interface usa essa lista para localizar os tipos de controlador. |
| **IHttpControllerActivator** | Cria novas instâncias de controlador. |
| **IHttpActionSelector** | Seleciona a ação. |
| **IHttpActionInvoker** | Invoca a ação. |

Para fornecer sua própria implementação de qualquer uma dessas interfaces, use o **serviços** coleta sobre o **HttpConfiguration** objeto:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
