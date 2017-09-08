---
title: "Tratamento de solicitações com controladores no ASP.NET MVC de núcleo"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.assetid: 9da9eb52-8583-4069-af91-155ba3529d7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: b7d6341c0312b3f5f122acfb2ee01210151b33bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>Tratamento de solicitações com controladores no ASP.NET MVC de núcleo

Por [Steve Smith](http://ardalis.com) e [Scott Addie](https://github.com/scottaddie)

Resultados da ação, ações e controladores são uma parte fundamental de como os desenvolvedores criar aplicativos usando o ASP.NET MVC de núcleo.

## <a name="what-is-a-controller"></a>O que é um controlador?

Um controlador é usado para definir e agrupar um conjunto de ações. Uma ação (ou *método de ação*) é um método em um controlador que trata as solicitações. Controladores de agrupam logicamente ações semelhantes. Essa agregação de ações permite comuns conjuntos de regras, como roteamento, cache e autorização, seja aplicado coletivamente. Solicitações são mapeadas para ações por meio de [roteamento](xref:mvc/controllers/routing).

Por convenção, as classes do controlador:
* Residem no nível raiz do projeto *controladores* pasta
* Herdar de`Microsoft.AspNetCore.Mvc.Controller`

Um controlador é uma classe pode ser instanciada em que pelo menos uma das seguintes condições é verdadeira:
* O nome da classe é o sufixo "Controller"
* A classe herda de uma classe cujo nome é o sufixo "Controller"
* A classe está decorada com o `[Controller]` atributo

Uma classe de controlador não deve ter um tipo de `[NonController]` atributo.

Controladores devem seguir o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle). Há duas abordagens para implementar esse princípio. Se várias ações do controlador requerem o mesmo serviço, considere o uso de [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) para solicitar essas dependências. Se o serviço é necessário por apenas um método de ação, considere o uso de [injeção de ação](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) para solicitar a dependência.

Dentro de **M**odelo -**V**ibir -**C**ontroller padrão, um controlador é responsável pelo processamento da solicitação inicial e instanciação do modelo. Em geral, as decisões de negócios devem ser executadas dentro do modelo.

O controlador usa o resultado do modelo de processamento (se houver) e retorna o modo de exibição correta e seus dados associados ou o resultado da chamada de API. Saiba mais em [visão geral do ASP.NET Core MVC](xref:mvc/overview) e [guia de Introdução ao ASP.NET MVC de núcleos e do Visual Studio](xref:tutorials/first-mvc-app/start-mvc).

O controlador é um *nível de interface do usuário* abstração. Suas responsabilidades são para garantir que os dados de solicitação são válidos e escolha o modo de exibição (ou resultado de uma API) deve ser retornado. Em aplicativos bem fatorados, ele não incluir diretamente dados acesso ou a lógica comercial. Em vez disso, o controlador delega para tratar essas responsabilidades de serviços.

## <a name="defining-actions"></a>Definindo ações

Métodos públicos em um controlador, exceto aqueles decorados com o `[NonAction]` atributo, são ações. Parâmetros em ações são associados a dados de solicitação e são validados usando [associação de modelo](xref:mvc/models/model-binding). Ocorre a validação de modelo para tudo o que é ligada ao modelo. O `ModelState.IsValid` o valor da propriedade indica se a associação de modelo e a validação foi bem-sucedida.

Métodos de ação devem conter lógica para mapear uma solicitação para um problema de negócios. Questões de negócios normalmente devem ser representados como serviços que acessa o controlador por meio de [injeção de dependência](xref:mvc/controllers/dependency-injection). Ações, em seguida, mapeiam o resultado da ação de negócios para um estado de aplicativo.

Ações podem retornar qualquer coisa, mas frequentemente retornar uma instância de `IActionResult` (ou `Task<IActionResult>` para métodos assíncronos) que produz uma resposta. O método de ação é responsável por escolher *o tipo de resposta*. O resultado da ação *a responder*.

### <a name="controller-helper-methods"></a>Métodos auxiliares do controlador

Controladores geralmente herdam [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), embora isso não é necessário. Derivando de `Controller` fornece acesso a três categorias de métodos auxiliares:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Métodos, resultando em um corpo de resposta vazia

Não `Content-Type` cabeçalho de resposta HTTP é incluído, desde que o corpo da resposta não tem conteúdo para descrever.

Há dois tipos de resultado dessa categoria: redirecionamento e código de Status HTTP.

* **Código de Status HTTP**

    Este tipo retorna um código de status HTTP. Alguns métodos auxiliares desse tipo são `BadRequest`, `NotFound`, e `Ok`. Por exemplo, `return BadRequest();` produz um código de 400 status quando executado. Quando métodos como `BadRequest`, `NotFound`, e `Ok` estão sobrecarregados, eles não se qualifica como respondentes de código de Status HTTP, pois a negociação de conteúdo está em andamento.

* **Redirecionamento**

    Este tipo retorna um redirecionamento para uma ação ou um destino (usando `Redirect`, `LocalRedirect`, `RedirectToAction`, ou `RedirectToRoute`). Por exemplo, `return RedirectToAction("Complete", new {id = 123});` redireciona para `Complete`, passando um objeto anônimo.

    O tipo de resultado de redirecionamento é diferente do tipo de código de Status HTTP principalmente na adição de um `Location` cabeçalho de resposta HTTP.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Métodos, resultando em um corpo de resposta não vazia com um tipo de conteúdo predefinido

A maioria dos métodos auxiliares nesta categoria incluem um `ContentType` propriedade, permitindo que você defina o `Content-Type` cabeçalho de resposta para descrever o corpo da resposta.

Há dois tipos de resultado dessa categoria: [exibição](xref:mvc/views/overview) e [resposta formatada](xref:mvc/models/formatting).

* **Exibir**

    Esse tipo retorna uma exibição que usa um modelo para renderizar HTML. Por exemplo, `return View(customer);` passa a um modelo para o modo de exibição para associação de dados.

* **Resposta formatada**

    Esse tipo retorna um formato de troca de dados semelhantes ou JSON para representar um objeto de uma maneira específica. Por exemplo, `return Json(customer);` serializa o objeto fornecido no formato JSON.
    
    Outros métodos comuns desse tipo incluem `File`, `PhysicalFile`, e `VirtualFile`. Por exemplo, `return PhysicalFile(customerFilePath, "text/xml");` retorna um arquivo XML descrito por um `Content-Type` valor de cabeçalho de resposta de "text/xml".

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Métodos, resultando em um corpo de resposta não vazio formatado em um tipo de conteúdo negociado com o cliente

Esta categoria é conhecido como **negociação de conteúdo**. [Negociação de conteúdo](xref:mvc/models/formatting#content-negotiation) aplica-se sempre que uma ação retorna um [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) tipo ou algo diferente de um [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementação. Uma ação que retorna uma não -`IActionResult` implementação (por exemplo, `object`) também retorna uma resposta formatada.

Alguns métodos auxiliares desse tipo incluem `BadRequest`, `CreatedAtRoute`, e `Ok`. Exemplos desses métodos `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, e `return Ok(value);`, respectivamente. Observe que `BadRequest` e `Ok` realizar a negociação de conteúdo apenas quando um valor transmitido; sem serem passados a um valor, em vez disso, servem como tipos de resultado do código de Status HTTP. O `CreatedAtRoute` método, por outro lado, sempre executa negociação de conteúdo desde seus sobrecargas exigem que um valor passado.

### <a name="cross-cutting-concerns"></a>Resolvem preocupações

Aplicativos normalmente compartilhar partes de seu fluxo de trabalho. Exemplos incluem um aplicativo que requer autenticação para acessar o carrinho de compras, ou um aplicativo que armazena dados em algumas páginas em cache. Para executar lógica antes ou depois de um método de ação, use um *filtro*. Usando [filtros](xref:mvc/controllers/filters) em resolvem preocupações pode reduzir duplicação, possibilitando que siga a [princípio não repita por conta própria (teste)](http://deviq.com/don-t-repeat-yourself/).

Filtrar mais atributos, como `[Authorize]`, podem ser aplicadas no nível do controlador ou ação de acordo com o nível desejado de granularidade.

Tratamento de erros e o cache de resposta são geralmente resolvem preocupações:
   * [Tratamento de erros](xref:mvc/controllers/filters#exception-filters)
   * [O cache de resposta](xref:performance/caching/response)

Muitos resolvem preocupações podem ser feitas usando filtros ou personalizado [middleware](xref:fundamentals/middleware).