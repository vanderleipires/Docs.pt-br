---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Teste de unidade controladores no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tópico descreve algumas técnicas específicas para controladores de teste na Web API 2 de unidade. Antes de ler este tópico, você talvez queira ler o tutorial de unidade...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7b0d5266757219a05b25fc3d1d4cba8514a4dff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824616"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Teste de unidade controladores no ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tópico descreve algumas técnicas específicas para controladores de teste na Web API 2 de unidade. Antes de ler este tópico, você talvez queira ler o tutorial [unidade de teste de API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), que mostra como adicionar um projeto de teste de unidade à sua solução.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - API Web 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Eu usei o Moq, mas a mesma ideia se aplica a qualquer estrutura de simulação. Moq 4.5.30 (e posterior) dá suporte ao Visual Studio 2017, Roslyn e .NET 4.5 e versões posteriores.

É um padrão comum em testes de unidade &quot;organizar act-declaração&quot;:

- Organizar: Configure todos os pré-requisitos para o teste seja executado.
- O ACT: Execute o teste.
- Assert: Verifique se o teste foi bem-sucedido.

Na etapa Arranjar, você geralmente usar simulação ou objetos stub. Que minimiza o número de dependências, portanto, o teste é voltada para o teste uma coisa.

Aqui estão algumas coisas que você deve teste de unidade em seus controladores de API da Web:

- A ação retorna o tipo correto da resposta.
- Parâmetros inválidos retornam a resposta de corrigir o erro.
- A ação chama o método correto na camada de repositório ou serviço.
- Se a resposta inclui um modelo de domínio, verifique se o tipo de modelo.

Estas são algumas das coisas gerais para testar, mas as especificações dependem de sua implementação do controlador. Em particular, ele faz uma grande diferença se ações do controlador retornam **HttpResponseMessage** ou **IHttpActionResult**. Para obter mais informações sobre esses tipos de resultado, consulte [resultados da ação na Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Ações que retornam HttpResponseMessage de testes

Aqui está um exemplo de um controlador cujo retorno de ações **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Observe que o controlador usa a injeção de dependência para injetar um `IProductRepository`. Isso torna o controlador mais testável, porque você pode injetar um repositório fictício. O teste de unidade a seguir verifica se o `Get` método gravações um `Product` no corpo da resposta. Suponha que `repository` é uma simulação `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

É importante definir **solicitar** e **configuração** no controlador. Caso contrário, o teste falhará com um **ArgumentNullException** ou **InvalidOperationException**.

## <a name="testing-link-generation"></a>Teste de geração de Link

O `Post` chamadas de método **UrlHelper.Link** criar links na resposta. Isso requer um pouco mais de configuração no teste de unidade:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

O **UrlHelper** classe precisa os dados de rota e de URL de solicitação, para que o teste precisa definir valores para eles. Outra opção é simulação ou stub **UrlHelper**. Com essa abordagem, você substitua o valor padrão de [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) com uma versão de simulação ou stub que retorna um valor fixo.

Vamos reescrever o teste usando o [Moq](https://github.com/Moq) framework. Instalar o `Moq` pacote do NuGet no projeto de teste.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Nesta versão, você não precisa configurar quaisquer dados de rota, porque a simulação **UrlHelper** retorna uma cadeia de caracteres constante.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Ações que retornam IHttpActionResult de testes

Na API Web 2, uma ação do controlador pode retornar **IHttpActionResult**, que é análogo à **ActionResult** no ASP.NET MVC. O **IHttpActionResult** interface define um padrão de comando para criar respostas HTTP. Em vez de criar a resposta diretamente, o controlador retorna um **IHttpActionResult**. Posteriormente, o pipeline chama o **IHttpActionResult** para criar a resposta. Essa abordagem torna mais fácil escrever testes de unidade, porque você pode ignorar muito o que é necessária para instalação **HttpResponseMessage**.

Aqui está um exemplo controller cujo retorno de ações **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Este exemplo mostra alguns padrões comuns de uso **IHttpActionResult**. Vamos ver como a unidade de testá-los.

### <a name="action-returns-200-ok-with-a-response-body"></a>Ação retorna 200 (Okey) com um corpo de resposta

O `Get` chamadas de método `Ok(product)` se o produto for encontrado. No teste de unidade, verifique se o tipo de retorno será **OkNegotiatedContentResult** e o produto retornado tem a ID da direita.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Observe que o teste de unidade não é o resultado da ação executada. Você pode supor que o resultado da ação cria a resposta HTTP corretamente. (É por isso que a estrutura da API Web tem seus próprios testes de unidade!)

### <a name="action-returns-404-not-found"></a>Ação retorna 404 (não encontrado)

O `Get` chamadas de método `NotFound()` se o produto não for encontrado. Nesse caso, o teste de unidade apenas verifica se o tipo de retorno será **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Ação retorna 200 (Okey) sem o corpo da resposta

O `Delete` chamadas de método `Ok()` para retornar uma resposta HTTP 200 vazia. Como no exemplo anterior, o teste de unidade verifica o tipo de retorno, neste caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Ação retorna 201 (criado) com um cabeçalho de localização

O `Post` chamadas de método `CreatedAtRoute` para retornar uma resposta HTTP 201 com um URI no cabeçalho Location. No teste de unidade, verifique se que a ação define os valores corretos de roteamentos.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Ação retorna outro 2xx com um corpo de resposta

O `Put` chamadas de método `Content` para retornar uma resposta de HTTP 202 (aceito) com um corpo de resposta. Nesse caso, é semelhante ao retornar 200 (Okey), mas o teste de unidade também deve verificar o código de status.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionais

- [Simulação do Entity Framework quando a API Web ASP.NET 2 de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Escrever testes para um serviço de API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (postagem de blog de Youssef Moussaoui).
- [API da Web do ASP.NET com depurador de rota de depuração](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
