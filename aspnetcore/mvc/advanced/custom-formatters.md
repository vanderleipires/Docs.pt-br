---
title: "APIs da web personalizados formatadores no ASP.NET MVC de núcleo"
author: tdykstra
description: Saiba como criar e usar formatadores personalizados para APIs da web no ASP.NET Core.
keywords: Api, formatadores personalizados da web do ASP.NET Core
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>APIs da web personalizados formatadores no ASP.NET MVC de núcleo

Por [Tom Dykstra](https://github.com/tdykstra)

Núcleo do ASP.NET MVC tem suporte interno para troca de dados em APIs da web usando os formatos de texto sem formatação, XML ou JSON. Este artigo mostra como adicionar suporte para formatos adicionais criando formatadores personalizados.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).

## <a name="when-to-use-custom-formatters"></a>Quando usar formatadores personalizados

Use um formatador personalizado quando quiser o [negociação de conteúdo](xref:mvc/models/formatting) processo para dar suporte a um tipo de conteúdo que não é compatível com os formatadores internos (JSON, XML e texto sem formatação).

Por exemplo, se alguns dos clientes para sua API da web puderem manipular o [Protobuf](https://github.com/google/protobuf) formato, você talvez queira usar Protobuf com esses clientes porque é mais eficiente.  Ou talvez você queira sua API da web para enviar entre em contato com os nomes e endereços em [vCard](https://wikipedia.org/wiki/VCard) formato, um formato normalmente usado para a troca de dados de contato. O aplicativo de exemplo fornecido com este artigo implementa um formatador vCard simples.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Visão geral de como usar um formatador personalizado

Aqui estão as etapas para criar e usar um formatador personalizado:

* Crie uma classe de formatador de saída para serializar os dados a serem enviados ao cliente.
* Crie uma classe de formatador de entrada para desserializar dados recebidos do cliente. 
* Adicionar instâncias de formatadores a serem o `InputFormatters` e `OutputFormatters` coleções no [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

As seções a seguir fornecem diretrizes e exemplos de código para cada uma dessas etapas.

## <a name="how-to-create-a-custom-formatter-class"></a>Como criar uma classe de formatador personalizado

Para criar um formatador:

* A classe derivam da classe base apropriada.
* Especifica as codificações e tipos de mídia válido no construtor.
* Substituir `CanReadType` / `CanWriteType` métodos
* Substituir `ReadRequestBodyAsync` / `WriteResponseBodyAsync` métodos
  
### <a name="derive-from-the-appropriate-base-class"></a>Derivar da classe base apropriada

Para tipos de mídia de texto (por exemplo, vCard), derivam de [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) ou [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) classe base.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Para tipos binários, derivam de [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) ou [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) classe base.

### <a name="specify-valid-media-types-and-encodings"></a>Especificar as codificações e tipos de mídia válido

No construtor, especificar codificações e tipos de mídia válido adicionando-se para o `SupportedMediaTypes` e `SupportedEncodings` coleções.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> Você não pode fazer a injeção de dependência de construtor em uma classe de formatador. Por exemplo, você não pode obter um agente de log, adicionando um parâmetro de agente de log para o construtor. Para acessar serviços, você precisa usar o objeto de contexto que é passado para seus métodos. Um exemplo de código [abaixo](#read-write) mostra como fazer isso.

### <a name="override-canreadtypecanwritetype"></a>Substituir CanReadType/CanWriteType 

Especifique o tipo é possível desserializar em ou serializar de substituindo o `CanReadType` ou `CanWriteType` métodos. Por exemplo, você só poderá criar texto vCard de um `Contact` tipo e vice-versa.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>O método CanWriteResult

Em alguns cenários, você tem que substituir `CanWriteResult` em vez de `CanWriteType`. Use `CanWriteResult` se as seguintes condições forem verdadeiras:

  * O método de ação retorna uma classe de modelo.
  * Existem classes derivadas que podem ser retornadas em tempo de execução.
  * Você precisa saber em tempo de execução que derivado classe foi retornada pela ação.  

Por exemplo, suponha que sua assinatura do método de ação retorna um `Person` tipo, mas ele pode retornar um `Student` ou `Instructor` tipo que deriva de `Person`. Se você quiser que o formatador para lidar com apenas `Student` objetos, verifique o tipo de [objeto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) no objeto de contexto fornecido para o `CanWriteResult` método. Observe que não é necessário usar `CanWriteResult` quando o método de ação retorna `IActionResult`; nesse caso, o `CanWriteType` método recebe o tipo de tempo de execução.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Substituir ReadRequestBodyAsync/WriteResponseBodyAsync 

Fazer o trabalho real de desserialização ou serialização em `ReadRequestBodyAsync` ou `WriteResponseBodyAsync`.  As linhas destacadas no exemplo a seguir mostram como obter os serviços do contêiner de injeção de dependência (você não é possível obtê-los do construtor parâmetros).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Como configurar o MVC para usar um formatador personalizado
 
Para usar um formatador personalizado, adicione uma instância da classe de formatador para a `InputFormatters` ou `OutputFormatters` coleção.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formatadores são avaliadas na ordem em que você inseri-los. O primeiro deles terá precedência. 

## <a name="next-steps"></a>Próximas etapas

Consulte o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), que implementa vCard simples de entrada e saída formatadores.  O aplicativo lê e grava vCard se parecer com o exemplo a seguir:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Para ver vCard de saída, execute o aplicativo e envia uma solicitação Get com aceitar cabeçalho "texto/vcard" `http://localhost:63313/api/contacts/` (durante a execução do Visual Studio) ou `http://localhost:5000/api/contacts/` (quando executado na linha de comando).

Para adicionar um vCard à coleção de contatos na memória, envie uma solicitação Post para a mesma URL, com cabeçalho Content-Type "texto/vcard" e vCard texto no corpo, formatado como o exemplo acima.
