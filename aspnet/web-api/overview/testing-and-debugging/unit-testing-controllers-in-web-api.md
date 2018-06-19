---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Testes de unidade controladores em ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tópico descreve algumas técnicas específicas para testes de unidade controladores em API Web 2. Antes de ler este tópico, você talvez queira ler o tutorial unidade...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039538"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Testes de unidade controladores em ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tópico descreve algumas técnicas específicas para testes de unidade controladores em API Web 2. Antes de ler este tópico, você talvez queira ler o tutorial [unidade de teste ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), que mostra como adicionar um projeto de teste de unidade para sua solução.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Usei Moq, mas a mesma ideia se aplica a qualquer estrutura fictícia. Moq 4.5.30 (e posterior) oferece suporte ao Visual Studio de 2017, Roslyn e .NET 4.5 e versões posteriores.

Um padrão comum em testes de unidade é &quot;organizar act-declaração&quot;:

- Organizar: Configure todos os pré-requisitos para executar o teste.
- ACT: Execute o teste.
- Assert: Verifique se o teste foi bem-sucedida.

Na etapa organizar, serão geralmente usados simulação ou objetos de stub. Que minimiza o número de dependências, para que o teste se concentra em algo de teste.

Aqui estão algumas coisas que você deve teste de unidade em seus controladores de API da Web:

- A ação retorna o tipo correto da resposta.
- Parâmetros inválidos retornam a resposta de corrigir o erro.
- A ação chama o método correto na camada de serviço ou de repositório.
- Se a resposta inclui um modelo de domínio, verifique se o tipo de modelo.

Estas são algumas das coisas gerais para testar, mas as especificações dependem de sua implementação do controlador. Em particular, ele faz uma grande diferença se suas ações do controlador retornam **HttpResponseMessage** ou **IHttpActionResult**. Para obter mais informações sobre esses tipos de resultados, consulte [resultados da ação na Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Ações que retornam HttpResponseMessage de testes

Aqui está um exemplo de um controlador de retorno cujo ações **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Observe que o controlador usa injeção de dependência para injetar um `IProductRepository`. Isso torna o controlador de maior capacidade de teste, porque você pode injetar um repositório de simulação. O teste de unidade a seguir verifica se o `Get` método gravações um `Product` ao corpo da resposta. Suponha que `repository` é uma simulação `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

É importante definir **solicitação** e **configuração** no controlador. Caso contrário, o teste falhará com um **ArgumentNullException** ou **InvalidOperationException**.

## <a name="testing-link-generation"></a>Geração de teste de vinculação

O `Post` chamadas de método **UrlHelper.Link** para criar links na resposta. Isso requer um pouco mais de configuração no teste de unidade:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

O **UrlHelper** classe precisa os dados de rota e a URL de solicitação, então o teste deve definir valores para essas. Outra opção é simulação ou stub **UrlHelper**. Com essa abordagem, você substitui o valor padrão de [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) com uma versão de simulação ou stub que retorna um valor fixo.

Vamos reescrever o teste usando o [Moq](https://github.com/Moq) framework. Instalar o `Moq` pacote do NuGet no projeto de teste.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Nesta versão, não é necessário configurar os dados de rota, pois a simulação **UrlHelper** retorna uma cadeia de caracteres constante.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Ações que retornam IHttpActionResult de testes

Em 2 de API da Web, uma ação do controlador pode retornar **IHttpActionResult**, que é análoga a **ActionResult** no ASP.NET MVC. O **IHttpActionResult** interface define um padrão de comando para criar respostas HTTP. Em vez de criar a resposta diretamente, o controlador retorna um **IHttpActionResult**. Posteriormente, o pipeline invoca o **IHttpActionResult** para criar a resposta. Essa abordagem facilita escrever testes de unidade, porque você pode ignorar muito a configuração necessária para **HttpResponseMessage**.

Aqui está um controlador de exemplo cujo retorno ações **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Este exemplo mostra alguns padrões comuns usando **IHttpActionResult**. Vamos ver como a unidade de testá-las.

### <a name="action-returns-200-ok-with-a-response-body"></a>Ação retorna 200 (Okey) com um corpo de resposta

O `Get` chamadas de método `Ok(product)` se o produto é encontrado. No teste de unidade, verifique se o tipo de retorno é **OkNegotiatedContentResult** e o produto retornado tem a ID ' direito.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Observe que o teste de unidade não executa o resultado da ação. Você pode presumir que o resultado da ação cria a resposta HTTP corretamente. (É por isso que o framework de API da Web tem seus próprios testes de unidade!)

### <a name="action-returns-404-not-found"></a>Ação retorna 404 (não encontrado)

O `Get` chamadas de método `NotFound()` se o produto não foi encontrado. Nesse caso, o teste de unidade verifica apenas se o tipo de retorno é **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Ação retorna 200 (Okey) sem corpo de resposta

O `Delete` chamadas de método `Ok()` para retornar uma resposta HTTP 200 vazia. Como no exemplo anterior, o teste de unidade verifica o tipo de retorno, nesse caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Ação retorna 201 (criado) com um cabeçalho de local

O `Post` chamadas de método `CreatedAtRoute` para retornar uma resposta HTTP 201 com um URI no cabeçalho de localização. No teste de unidade, verifique se a ação define os valores corretos de roteamentos.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Ação retorna outro 2xx com um corpo de resposta

O `Put` chamadas de método `Content` para retornar uma resposta HTTP 202 (aceito) com um corpo de resposta. Nesse caso é semelhante ao retorno de 200 (Okey), mas o teste de unidade também deve verificar o código de status.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionais

- [Simulação do Entity Framework quando ASP.NET Web API 2 de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Escrevendo testes de um serviço de API da Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (postagem de blog por Youssef Moussaoui).
- [Depuração da API da Web do ASP.NET com depurador de rota](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
