---
title: Formatadores personalizados em APIs Web do ASP.NET Core MVC
author: tdykstra
description: Saiba como criar e usar formatadores personalizados para APIs Web no ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/custom-formatters
ms.openlocfilehash: 8a42f2d885bd0a0c6d2bd05f9c589def2e15d50a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>Formatadores personalizados em APIs Web do ASP.NET Core MVC

Por [Tom Dykstra](https://github.com/tdykstra)

O ASP.NET Core MVC tem suporte interno para troca de dados em APIs Web usando formatos de texto sem formatação, XML ou JSON. Este artigo mostra como adicionar suporte para formatos adicionais criando formatadores personalizados.

[Exibir ou baixar amostra do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).

## <a name="when-to-use-custom-formatters"></a>Quando usar formatadores personalizados

Use um formatador personalizado quando quiser que o processo de [negociação de conteúdo](xref:mvc/models/formatting) dê suporte a um tipo de conteúdo que não tem suporte dos formatadores internos (JSON, XML e texto sem formatação).

Por exemplo, se alguns dos clientes de sua API Web puderem usar o formato [Protobuf](https://github.com/google/protobuf), talvez você queira usar Protobuf com esses clientes porque é mais eficiente.  Ou talvez você queira que sua API Web envie endereços e nomes de contato no formato [vCard](https://wikipedia.org/wiki/VCard), um formato usado normalmente para troca de dados de contato. O aplicativo de exemplo fornecido com este artigo implementa um formatador vCard simples.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Visão geral de como usar um formatador personalizado

Estas são as etapas para criar e usar um formatador personalizado:

* Criar uma classe de formatador de saída se quiser serializar os dados a serem enviados ao cliente.
* Criar uma classe de formatador de entrada se quiser desserializar os dados recebidos do cliente. 
* Adicionar instâncias de seus formatadores às coleções `InputFormatters` e `OutputFormatters` em [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

As seções a seguir fornecem diretrizes e exemplos de código para cada uma dessas etapas.

## <a name="how-to-create-a-custom-formatter-class"></a>Como criar uma classe de formatador personalizado

Para criar um formatador:

* Derive a classe da classe base apropriada.
* Especifique codificações e tipos de mídia válidos no construtor.
* Substitua os métodos `CanReadType`/`CanWriteType`
* Substitua os métodos `ReadRequestBodyAsync`/`WriteResponseBodyAsync`
  
### <a name="derive-from-the-appropriate-base-class"></a>Derivar da classe base apropriada

Para tipos de mídia de texto (por exemplo, vCard), derive da classe base [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) ou [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Para tipos binários, derive da classe base [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) ou [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter).

### <a name="specify-valid-media-types-and-encodings"></a>Especifique codificações e tipos de mídia válidos

No construtor, especifique codificações e tipos de mídia válidos adicionando-os às coleções `SupportedMediaTypes` e `SupportedEncodings`.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> Não é possível fazer a injeção de dependência de construtor em uma classe de formatador. Por exemplo, não é possível obter um agente adicionando um parâmetro de agente ao construtor. Para acessar serviços, você precisa usar o objeto de contexto que é passado para seus métodos. O exemplo de código [abaixo](#read-write) mostra como isso é feito.

### <a name="override-canreadtypecanwritetype"></a>Substituir CanReadType/CanWriteType 

Especifique o tipo no qual desserializar ou do qual serializar substituindo os métodos `CanReadType` ou `CanWriteType`. Por exemplo, talvez você só possa criar texto de vCard de um tipo `Contact` e vice-versa.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>O método CanWriteResult

Em alguns cenários, você precisa substituir `CanWriteResult` em vez de `CanWriteType`. Use `CanWriteResult` se as condições a seguir forem verdadeiras:

  * O método de ação retorna uma classe de modelo.
  * Há classes derivadas que podem ser retornadas em tempo de execução.
  * Você precisa saber em tempo de execução qual classe derivada foi retornada pela ação.  

Por exemplo, suponha que sua assinatura do método de ação retorne um tipo `Person`, mas ele pode retornar um tipo `Student` ou `Instructor` que deriva de `Person`. Se você quiser que o formatador trate apenas de objetos `Student`, verifique o tipo de [Objeto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) no objeto de contexto fornecido ao método `CanWriteResult`. Observe que não é necessário usar `CanWriteResult` quando o método de ação retorna `IActionResult`; nesse caso, o método `CanWriteType` recebe o tipo de tempo de execução.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Substituir ReadRequestBodyAsync/WriteResponseBodyAsync 

Você faz o trabalho real de desserialização ou serialização em `ReadRequestBodyAsync` ou `WriteResponseBodyAsync`.  As linhas destacadas no exemplo a seguir mostram como obter serviços do contêiner de injeção de dependência (não é possível obtê-los dos parâmetros do construtor).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Como configurar o MVC para usar um formatador personalizado
 
Para usar um formatador personalizado, adicione uma instância da classe de formatador à coleção `InputFormatters` ou `OutputFormatters`.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formatadores são avaliados na ordem em que você os insere. O primeiro deles tem precedência. 

## <a name="next-steps"></a>Próximas etapas

Veja o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), que implementa formatadores de entrada e saída simples de vCard.  O aplicativo lê e grava vCards parecidos com o exemplo a seguir:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Para ver a saída do vCard, execute o aplicativo e envie uma solicitação Get com o cabeçalho de aceitação "texto/vcard" para `http://localhost:63313/api/contacts/` (ao executar com Visual Studio) ou `http://localhost:5000/api/contacts/` (ao executar da linha de comando).

Para adicionar um vCard à coleção de contatos na memória, envie uma solicitação Post para a mesma URL, com cabeçalho Content-Type "texto/vcard" e com o texto do vCard no corpo, formatado como o exemplo acima.
