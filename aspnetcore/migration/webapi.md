---
title: Migrar da API Web ASP.NET para ASP.NET Core
author: ardalis
description: Saiba como migrar uma implementação de API da Web da API Web ASP.NET para ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894186"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrar da API Web ASP.NET para ASP.NET Core

Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

As APIs da Web são serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. ASP.NET Core MVC inclui suporte para a criação de APIs da Web fornecendo uma maneira única e consistente de criação de aplicativos web. Neste artigo, demonstraremos as etapas necessárias para migrar de uma implementação de API da Web da API Web ASP.NET para ASP.NET Core MVC.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Projeto de API Web ASP.NET de revisão

Este artigo usa o projeto de exemplo *ProductsApp*, criado no artigo [Introdução à API Web ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como ponto de partida. Nesse projeto, um projeto de API Web ASP.NET simples está configurado da seguinte maneira.

Na *Global.asax.cs*, é feita uma chamada para `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` é definido em *App_Start*, e tem apenas um estático `Register` método:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Essa classe configura [roteamento de atributo](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), embora na verdade não está sendo usado no projeto. Ele também configura a tabela de roteamento, que é usada pela API Web ASP.NET. Nesse caso, a API Web ASP.NET esperarão URLs para corresponder ao formato */api/ {controller} / {id}*, com *{id}* opcionais.

O *ProductsApp* projeto inclui apenas um controlador simple, que herda de `ApiController` e expõe dois métodos:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Por fim, o modelo *produto*, usado pelas *ProductsApp*, é uma classe simples:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Agora que temos um projeto simples do qual iniciar, podemos demonstrar como migrar este projeto de API da Web para ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Criar o projeto de destino

Usando o Visual Studio, crie uma solução nova e vazia e nomeie- *WebAPIMigration*. Adicionar existente *ProductsApp* de projeto para ele, em seguida, adicione um novo projeto ASP.NET Core Web Application à solução. Nomeie o novo projeto *ProductsCore*.

![Abrir o diálogo Novo projeto para modelos da Web](webapi/_static/add-web-project.png)

Em seguida, escolha o modelo de projeto de API da Web. Nós migraremos a *ProductsApp* o conteúdo para esse novo projeto.

![Caixa de diálogo nova aplicativo Web com o modelo de projeto de API da Web selecionado na lista de modelos do ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Excluir o `Project_Readme.html` arquivo do novo projeto. Agora, sua solução deve ser assim:

![Solução de aplicativo aberta no Gerenciador de soluções mostrando arquivos e pastas dos projetos ProductsApp e ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrar configuração

O ASP.NET Core não usa *global. asax*, *Web. config*, ou *App_Start* pastas. Em vez disso, todas as tarefas de inicialização são feitas *Startup.cs* na raiz do projeto (consulte [inicialização do aplicativo](../fundamentals/startup.md)). No ASP.NET Core MVC, roteamento com base em atributo agora está incluído por padrão quando `UseMvc()` é chamado; e isso é a abordagem recomendada para configurar rotas de API da Web (e é como o projeto de início da API Web manipula o roteamento).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Supondo que você deseja usar o roteamento de atributo em seu projeto no futuro, nenhuma configuração adicional é necessária. Simplesmente aplicar os atributos conforme o necessário para os controladores e ações, como é feito no exemplo `ValuesController` classe que está incluído no projeto de início da API Web:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Observe a presença *[controller]* na linha 8. Roteamento com base em atributo agora oferece suporte a determinados tokens, como *[controller]* e *[action]*. Esses tokens são substituídos em tempo de execução com o nome do controlador ou ação, respectivamente, para o qual o atributo foi aplicado. Isso serve para reduzir o número de cadeias de caracteres mágicas no projeto e garante que as rotas serão mantidas sincronizadas com seus respectivos controladores e ações quando é aplicadas a refatoração de renomeação automática.

Para migrar o controlador da API de produtos, é necessário primeiro copiar *ProductsController* para o novo projeto. Em seguida, basta inclua o atributo de rota no controlador:

```csharp
[Route("api/[controller]")]
```

Você também precisará adicionar o `[HttpGet]` de atributo para os dois métodos, já que ambos devem ser chamados por meio de HTTP Get. Incluir a expectativa de um parâmetro de "id" no atributo para `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

Neste ponto, o roteamento é configurado corretamente; No entanto, nós ainda não é possível testá-lo. Alterações adicionais devem ser feitas antes *ProductsController* será compilado.

## <a name="migrate-models-and-controllers"></a>Migrar modelos e controladores

A última etapa no processo de migração para este projeto de API Web simple é copiar sobre os controladores e os modelos usam. Nesse caso, basta copiar *Controllers/ProductsController.cs* do projeto original para o novo. Em seguida, copie toda a pasta de modelos do projeto original para o novo. Ajustar os namespaces para coincidir com o novo nome do projeto (*ProductsCore*).  Neste ponto, você pode compilar o aplicativo, e você encontrará um número de erros de compilação. Eles devem geralmente se encaixam nas seguintes categorias:

* *ApiController* não existe

* *System* namespace não existe

* *IHttpActionResult* não existe

Felizmente, elas são muito fácil corrigir:

* Alteração *ApiController* à *controlador* (talvez você precise adicionar *usando Microsoft.AspNetCore.Mvc*)

* Excluir qualquer usando a instrução referindo-se a *System*

* Alterar qualquer método retornando *IHttpActionResult* para retornar um *IActionResult*

Depois que essas alterações foram feitas e não utilizados usando instruções removidas, migradas *ProductsController* classe tem esta aparência:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Agora você deve ser capaz de executar o projeto migrado e navegue até */api/produtos*; e, você deverá ver a lista completa de produtos de 3. Navegue até */api/products/1* e você deverá ver o primeiro produto.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Shim de compatibilidade do ASP.NET 4.x API Web 2

Uma ferramenta útil ao migrar do ASP.NET Web API projetos ASP.NET Core é o [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca. O shim de compatibilidade se estende do ASP.NET Core para permitir um número de diferentes convenções de Web API 2 a ser usada. O exemplo portado anteriormente neste documento é bastante básico para a correção de compatibilidade não era necessária. Para projetos maiores, usem o shim de compatibilidade pode ser útil para temporariamente preenchendo a lacuna de API entre o ASP.NET Core e ASP.NET Web API 2.

O shim de compatibilidade de API da Web destina-se a ser usado como uma medida temporária para facilitar a migração de grande projetos de API da Web para o ASP.NET Core. Ao longo do tempo, os projetos devem ser atualizados para usar os padrões do ASP.NET Core em vez de usar o shim de compatibilidade.

Recursos de compatibilidade incluídos no `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluem:

* Adiciona um `ApiController` tipo de forma que os tipos de base dos controladores não precisam ser atualizados.
* Habilita a associação de modelo de estilo de API da Web. Funções de associação do ASP.NET Core MVC modelo da mesma forma que o MVC 5, por padrão. As alterações de correção de compatibilidade associação de modelo para ser mais semelhante a convenções de associação de modelo Web API 2. Por exemplo, tipos complexos são vinculados automaticamente do corpo da solicitação.
* Estende a associação de modelo para que as ações do controlador podem usar parâmetros de tipo `HttpRequestMessage`.
* Adiciona os formatadores de mensagem, permitindo que as ações para retornar os resultados do tipo `HttpResponseMessage`.
* Adiciona métodos de resposta adicionais que ações de API Web 2 podem ter usado para servir as respostas:
  * Geradores de HttpResponseMessage:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Métodos de ação de resultados:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Adiciona uma instância do `IContentNegotiator` para o contêiner de injeção de dependência do aplicativo e torna o tipos relacionados a negociação de conteúdo [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponíveis. Isso inclui tipos, como `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.

Para usar o shim de compatibilidade, você precisa:

* Instalar o [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pacote do NuGet.
* Registrar os serviços do shim de compatibilidade com o contêiner de injeção de dependência do aplicativo chamando `services.AddMvc().AddWebApiConventions()` no aplicativo do `Startup.ConfigureServices` método.
* Definir rotas específicas à API Web usando `MapWebApiRoute` sobre o `IRouteBuilder` no aplicativo do `IApplicationBuilder.UseMvc` chamar.

## <a name="summary"></a>Resumo

Migrando um projeto de API Web ASP.NET simples para ASP.NET Core MVC é bastante simples, graças ao suporte interno para APIs da Web em ASP.NET Core MVC. As principais partes, que serão necessário migrar todos os projetos API Web do ASP.NET são as rotas, controladores e modelos, junto com as atualizações para os tipos usados por controladores e ações.
