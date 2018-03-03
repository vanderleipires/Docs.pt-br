---
title: Migrando de API da Web do ASP.NET
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 9eb5f4dfec82ec1c60d33bff94d35857a4c0cfd6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-web-api"></a>Migrando de API da Web do ASP.NET

Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

APIs da Web são serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. Núcleo do ASP.NET MVC inclui suporte para a criação de APIs da Web fornecendo uma maneira simples e consistente de criação de aplicativos web. Neste artigo, vamos demonstrar as etapas necessárias para migrar uma implementação da API da Web do ASP.NET Web API para MVC do ASP.NET Core.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Revisão ASP.NET Web API do projeto

Este artigo usa o projeto de exemplo, *ProductsApp*, criado no artigo [guia de Introdução ao ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como ponto de partida. No projeto, um projeto de API da Web ASP.NET simple é configurado da seguinte maneira.

Em *Global.asax.cs*, é feita uma chamada para `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` é definido em *App_Start*, e tem apenas uma estática `Register` método:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Essa classe configura [roteamento de atributo](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), embora na verdade não está sendo usado no projeto. Ele também configura a tabela de roteamento que é usada pela API da Web do ASP.NET. Nesse caso, o ASP.NET Web API esperará URLs para corresponder ao formato */api/ {controller} / {id}*, com *{id}* opcionais.

O *ProductsApp* projeto inclui apenas um controlador simple, que herda de `ApiController` e expõe dois métodos:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Por fim, o modelo *produto*, usada pelo *ProductsApp*, é uma classe simple:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Agora que temos um projeto simple da qual iniciar, podemos demonstrar como migrar este projeto de API da Web para ASP.NET MVC de núcleo.

## <a name="create-the-destination-project"></a>Criar o projeto de destino

Usando o Visual Studio, crie uma solução nova e vazia e nomeie- *WebAPIMigration*. Adicionar existente *ProductsApp* projeto a ele, em seguida, adicionar um novo projeto de aplicativo de Web de núcleo ASP.NET à solução. Nomeie o novo projeto *ProductsCore*.

![Abrir uma caixa de diálogo Nova projeto modelos da Web](webapi/_static/add-web-project.png)

Em seguida, escolha o modelo de projeto de API da Web. Migraremos o *ProductsApp* conteúdo para esse novo projeto.

![Caixa de diálogo nova aplicativo Web com o modelo de projeto de API da Web selecionado na lista de modelos do ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Excluir o `Project_Readme.html` arquivo do novo projeto. Agora, sua solução deve ser assim:

![Solução de aplicativo abrir no Gerenciador de soluções mostrando arquivos e pastas dos projetos ProductsApp e ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrar a configuração

Não usa o ASP.NET Core *global. asax*, *Web. config*, ou *App_Start* pastas. Em vez disso, todas as tarefas de inicialização são realizadas em *Startup.cs* na raiz do projeto (consulte [inicialização do aplicativo](../fundamentals/startup.md)). No ASP.NET MVC de núcleo, roteamento baseado em atributo agora está incluído por padrão quando `UseMvc()` é chamado; e isso é a abordagem recomendada para configurar rotas de API da Web (e é como o projeto de starter API da Web trata de roteamento).

[!code-none[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

Supondo que você deseja usar o roteamento de atributo em seu projeto no futuro, nenhuma configuração adicional é necessária. Simplesmente aplicar os atributos conforme necessário para seus controladores e ações, como é feito no exemplo `ValuesController` classe que está incluído no projeto de starter API da Web:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Observe a presença de *[controller]* na linha 8. Roteamento baseado em atributo agora oferece suporte a determinados símbolos, como *[controller]* e *[ação]*. Esses tokens são substituídos em tempo de execução com o nome do controlador ou ação, respectivamente, para que o atributo foi aplicado. Isso serve para reduzir o número de cadeias de caracteres mágicas no projeto e garante que as rotas serão mantidas sincronizadas com os respectivos controladores e ações quando refatorações renomear automaticamente são aplicadas.

Para migrar o controlador de API de produtos, é necessário primeiro copiar *ProductsController* para o novo projeto. Basta inclua o atributo da rota no controlador:

```csharp
[Route("api/[controller]")]
```

Você também precisará adicionar o `[HttpGet]` atributo para os dois métodos, desde que ambos devem ser chamados por meio de HTTP Get. Incluir a expectativa de um parâmetro de "id" no atributo para `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

Neste ponto, o roteamento está configurado corretamente. No entanto, podemos ainda não é possível testá-lo. Outras alterações devem ser feitas antes de *ProductsController* serão compilados.

## <a name="migrate-models-and-controllers"></a>Migrar modelos e controladores

A última etapa no processo de migração para este projeto de API da Web simple é copiar os controladores e os modelos usarem. Nesse caso, basta copiar *Controllers/ProductsController.cs* do projeto original para o novo. Em seguida, copie toda a pasta de modelos de projeto original para o novo. Ajustar os namespaces para coincidir com o novo nome do projeto (*ProductsCore*).  Neste ponto, você pode criar o aplicativo e você encontrará um número de erros de compilação. Eles geralmente devem se encaixam nas seguintes categorias:

* *ApiController* não existe

* *System.Web.Http* namespace não existe

* *IHttpActionResult* não existe

Felizmente, elas são muito fácil corrigir:

* Alterar *ApiController* para *controlador* (talvez seja necessário adicionar *usando Microsoft.AspNetCore.Mvc*)

* Excluir qualquer uso instrução referindo-se a *System.Web.Http*

* Alterar qualquer método retornando *IHttpActionResult* para retornar um *IActionResult*

Depois que essas alterações foram feitas e não utilizados usando instruções removido, o migrados *ProductsController* classe tem esta aparência:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Agora você deve ser capaz de executar o projeto migrado e navegue até */api/produtos*; e, você deve ver a lista completa de 3 produtos. Navegue até */api/products/1* e você verá o primeiro produto.

## <a name="summary"></a>Resumo

Migrando um projeto ASP.NET Web API simple para o ASP.NET MVC de núcleo é relativamente simples, graças ao suporte interno para APIs da Web em ASP.NET MVC de núcleo. As principais partes que todos os projetos ASP.NET Web API precisará migrar são rotas, controladores e modelos, junto com as atualizações para os tipos usados por controladores e ações.
