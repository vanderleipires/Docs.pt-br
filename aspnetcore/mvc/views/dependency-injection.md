---
title: "Injeção de dependência em exibições"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: 690fdd0fd841341d17de48c0a8c9af121da220de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-views"></a>Injeção de dependência em exibições

Por [Steve Smith](https://ardalis.com/)

O ASP.NET Core dá suporte à [injeção de dependência](xref:fundamentals/dependency-injection) em exibições. Isso pode ser útil para serviços específicos a uma exibição, como localização ou dados necessários apenas para o preenchimento de elementos de exibição. Você deve tentar manter a [separação de interesses](http://deviq.com/separation-of-concerns/) entre os controladores e as exibições. A maioria dos dados exibida pelas exibições deve ser passada pelo controlador.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Um exemplo simples

Injete um serviço em uma exibição usando a diretiva `@inject`. Considere `@inject` como a adição de uma propriedade à exibição e o preenchimento da propriedade usando a DI.

A sintaxe de `@inject`: `@inject <type> <name>`

Um exemplo de `@inject` em ação:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Essa exibição exibe uma lista de instâncias `ToDoItem`, junto com um resumo mostrando estatísticas gerais. O resumo é populado com base no `StatisticsService` injetado. Esse serviço é registrado para injeção de dependência em `ConfigureServices` em *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

O `StatisticsService` faz alguns cálculos no conjunto de instâncias `ToDoItem`, que ele acessa por meio de um repositório:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

O repositório de exemplo usa uma coleção em memória. A implementação mostrada acima (que opera em todos os dados na memória) não é recomendada para conjuntos de dados grandes e acessados remotamente.

A amostra exibe dados do modelo associado à exibição e o serviço injetado na exibição:

![Exibição To Do listando o total de itens, os itens concluídos, a prioridade média e uma lista de tarefas com seus níveis de prioridade e valores boolianos que indica a conclusão.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Populando os dados de pesquisa

A injeção de exibição pode ser útil para popular opções em elementos de interface do usuário, como listas suspensas. Considere um formulário de perfil do usuário que inclui opções para especificar o gênero, estado e outras preferências. A renderização desse formulário usando uma abordagem MVC padrão exigirá que o controlador solicite serviços de acesso a dados para cada um desses conjuntos de opções e, em seguida, popule um modelo ou `ViewBag` com cada conjunto de opções a ser associado.

Uma abordagem alternativa injeta os serviços diretamente na exibição para obter as opções. Isso minimiza a quantidade de código necessária para o controlador, movendo essa lógica de construção do elemento de exibição para a própria exibição. A ação do controlador para exibir um formulário de edição de perfil precisa apenas passar a instância de perfil para o formulário:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

O formulário HTML usado para atualizar essas preferências inclui listas suspensas para três das propriedades:

![Atualize a exibição de Perfil com um formulário que permite a entrada de nome, gênero, estado e Cor favorita.](dependency-injection/_static/updateprofile.png)

Essas listas são populadas por um serviço que foi injetado na exibição:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

O `ProfileOptionsService` é um serviço no nível da interface do usuário criado para fornecer apenas os dados necessários para esse formulário:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Não se esqueça de registrar tipos que você solicitará por meio da injeção de dependência no método `ConfigureServices` em *Startup.cs*.

## <a name="overriding-services"></a>Substituindo serviços

Além de injetar novos serviços, essa técnica também pode ser usada para substituir serviços injetados anteriormente em uma página. A figura abaixo mostra todos os campos disponíveis na página usada no primeiro exemplo:

![Menu contextual do IntelliSense em um símbolo @ tipado listando os campos Html, Component, StatsService e Url](dependency-injection/_static/razor-fields.png)

Como você pode ver, os campos padrão incluem `Html`, `Component` e `Url` (bem como o `StatsService` que injetamos). Se, para a instância, você desejava substituir os Auxiliares HTML padrão por seus próprios, faça isso com facilidade usando `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Se deseja estender os serviços existentes, basta usar essa técnica herdando da implementação existente ou encapsulando-a com sua própria implementação.

## <a name="see-also"></a>Consulte também

* Blog de Simon Timms: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/) (Inserindo dados de pesquisa na exibição)
