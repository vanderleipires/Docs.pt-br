---
title: "Tratamento de erro no núcleo do ASP.NET"
author: ardalis
description: Explica como manipular erros em aplicativos do ASP.NET Core
keywords: "ASP.NET Core, tratamento de erros, manipulação de exceção"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93f0724dbe98316e2b5a0af0ac1760c3aac2f1d0
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Introdução ao ASP.NET Core de tratamento de erros

Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)

Este artigo aborda appoaches comuns para tratamento de erros em aplicativos do ASP.NET Core.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a>A página de exceção do desenvolvedor

Para configurar um aplicativo para exibir uma página que mostra informações detalhadas sobre exceções, instale o `Microsoft.AspNetCore.Diagnostics` NuGet do pacote e adicione uma linha para o [configurar método na classe de inicialização](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Colocar `UseDeveloperExceptionPage` antes que qualquer middleware que você deseja capturar exceções, como `app.UseMvc`.

>[!WARNING]
> Habilitar a página de exceção de desenvolvedor **somente quando o aplicativo estiver em execução no ambiente de desenvolvimento**. Você não deseja compartilhar informações de exceção detalhadas publicamente quando o aplicativo for executado em produção. [Saiba mais sobre como configurar ambientes](environments.md).

Para ver a página de exceção de desenvolvedor, execute o aplicativo de exemplo com o ambiente definido como `Development`e adicione `?throw=true` para a URL base do aplicativo. A página inclui várias guias com informações sobre a exceção e a solicitação. A primeira guia inclui um rastreamento de pilha. 

![Rastreamento de pilha](error-handling/_static/developer-exception-page.png)

A próxima guia mostra a consulta parâmetros de cadeia de caracteres, se houver.

![Parâmetros de cadeia de caracteres de consulta](error-handling/_static/developer-exception-page-query.png)

Esta solicitação não tinha os cookies, mas se tivesse, apareceriam no **Cookies** guia. Você pode ver os cabeçalhos que foram passados a última guia.

![cabeçalhos](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configurando uma página de tratamento de exceções personalizadas

É uma boa ideia para configurar uma página de manipulador de exceção a ser usado quando o aplicativo não está executando o `Development` ambiente.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Em um aplicativo MVC, não explicitamente decorar o método de ação do manipulador de erro com os atributos de método HTTP, como `HttpGet`. Usando verbos explícitos pode impedir que algumas solicitações atinjam o método.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Configurar páginas de código de status

Por padrão, seu aplicativo não fornecerá uma página de código de status detalhadas para códigos de status HTTP como 500 (erro interno do servidor) ou 404 (não encontrado). Você pode configurar o `StatusCodePagesMiddleware` , adicionando uma linha para o `Configure` método:

```csharp
app.UseStatusCodePages();
```

Por padrão, este middleware adiciona manipuladores simples, somente de texto para os códigos de status comuns, como 404:

![página 404](error-handling/_static/default-404-status-code.png)

O middleware oferece suporte a vários métodos de extensão diferente. Um usa uma expressão lambda, outra usa uma cadeia de caracteres de formato e de tipo de conteúdo.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Também há métodos de extensão de redirecionamento. Um envia um código de 302 status para o cliente e um retorna o código de status original para o cliente, mas também executa o manipulador para a URL de redirecionamento.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Se você precisar desativar páginas de código de status para determinadas solicitações, você pode fazer isso:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Código de tratamento de exceção

Código em páginas de tratamento de exceção pode gerar exceções. Geralmente é uma boa ideia de páginas de erro de produção para consistem em conteúdo puramente estático.

Além disso, lembre-se que, depois que os cabeçalhos de resposta forem enviados, não é possível alterar o código de status da resposta, nem podem executar de nenhuma página de exceção ou manipuladores. A resposta deve ser concluída ou a conexão anulada.

## <a name="server-exception-handling"></a>Tratamento de exceção de servidor

Além da lógica em seu aplicativo de tratamento de exceção de [server](servers/index.md) hospedar seu aplicativo executa algumas tratamento de exceção. Se o servidor detecta uma exceção antes de serem enviados os cabeçalhos, o servidor envia uma resposta de erro de servidor interno 500 sem corpo. Se o servidor detecta uma exceção após terem sido enviados os cabeçalhos, o servidor fecha a conexão. Solicitações que não são manipuladas pelo seu aplicativo são manipuladas pelo servidor. Qualquer exceção que ocorre é tratada pela exceção do servidor tratamento. Algum configurado páginas de erro personalizadas ou middleware de tratamento de exceção ou filtros não afetam esse comportamento.

## <a name="startup-exception-handling"></a>Tratamento de exceções de inicialização

Apenas a camada de hospedagem pode lidar com exceções que ocorrem durante a inicialização do aplicativo. Você pode [configurar como o host se comporta em resposta a erros durante a inicialização](hosting.md#detailed-errors) usando `captureStartupErrors` e `detailedErrors` chave.

Hospedagem só pode mostrar uma página de erro para um erro de inicialização capturada se o erro ocorrer após o endereço de host/associação de porta. Se qualquer associação falhar por algum motivo, a camada de hospedagem registra uma exceção crítica, o travamento do processo dotnet, e nenhuma página de erro é exibida.

## <a name="aspnet-mvc-error-handling"></a>Tratamento de erros do ASP.NET MVC

[MVC](../mvc/index.md) aplicativos tem algumas opções adicionais de manipulação de erros, como configurar filtros de exceção e executar a validação de modelo.

### <a name="exception-filters"></a>Filtros de exceção

Filtros de exceção podem ser configurados globalmente ou em uma base por controlador ou por ação em um aplicativo MVC. Esses filtros lidar com qualquer exceção não tratada ocorre durante a execução de uma ação do controlador ou outro filtro e não são chamados caso contrário. Saiba mais sobre filtros de exceção do [filtros](../mvc/controllers/filters.md).

>[!TIP]
> Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas eles não são mais flexíveis middleware de tratamento de erros. Preferir middleware para o caso geral e usar filtros apenas em que você precisa fazer o tratamento de erros *diferentemente* com base em qual ação MVC foi escolhida.

### <a name="handling-model-state-errors"></a>Estado do modelo de tratamento de erros

[Validação de modelo](../mvc/models/validation.md) ocorre antes de cada ação de controlador que está sendo invocada, e é responsabilidade do método de ação para inspecionar `ModelState.IsValid` e reagir de forma adequada.

Alguns aplicativos escolherá a seguir uma convenção padrão para lidar com erros de validação de modelo, caso em que um [filtro](../mvc/controllers/filters.md) pode ser um local adequado para implementar essa política. Você deve testar o comportam de suas ações com os estados de modelo inválido. Saiba mais em [lógica do controlador de teste](../mvc/controllers/testing.md).



