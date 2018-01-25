---
title: "Injeção de dependência em exibições"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: a1258dbe2e659f6c5149d15b37451810ec7d6601
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="dependency-injection-into-views"></a>Injeção de dependência em exibições

Por [Steve Smith](https://ardalis.com/)

Dá suporte ao ASP.NET Core [injeção de dependência](xref:fundamentals/dependency-injection) em modos de exibição. Isso pode ser útil para serviços de exibição específicos, como localização ou os dados necessários apenas para o preenchimento de elementos de exibição. Você deve tentar manter [separação de preocupações](http://deviq.com/separation-of-concerns/) entre seus controladores e exibições. A maioria dos dados que exibem seus modos de exibição deve ser passada do controlador.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Um exemplo simples

Você pode injetar um serviço em uma exibição usando o `@inject` diretiva. Você pode pensar `@inject` como adicionar uma propriedade ao modo de exibição e preenchendo a propriedade usando a injeção de dependência.

A sintaxe para `@inject`:`@inject <type> <name>`

Um exemplo de `@inject` em ação:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Este modo de exibição exibe uma lista de `ToDoItem` instâncias, junto com um resumo mostrando estatísticas gerais. O resumo é preenchido do injetado `StatisticsService`. Esse serviço é registrado para injeção de dependência em `ConfigureServices` na *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

O `StatisticsService` realiza alguns cálculos no conjunto de `ToDoItem` instâncias que ele acessa por meio de um repositório:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

O repositório de exemplo usa uma coleção de memória. A implementação mostrada acima (que opera em todos os dados na memória) não é recomendado para conjuntos de dados grandes e acessados remotamente.

O exemplo exibe dados de modelo associado à exibição e o serviço injetados no modo de exibição:

![Para exibir a listagem de itens total, concluída itens, prioridade média e uma lista de tarefas com níveis de prioridade e valores boolean que indica a conclusão.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Preenchimento de dados de pesquisa

Injeção de exibição pode ser útil para preencher as opções em elementos de interface do usuário, como listas suspensas. Considere a possibilidade de um formulário de perfil de usuário que inclui opções para especificar o sexo, estado e outras preferências. Renderização desse formulário usando uma abordagem MVC padrão exigiria o controlador para solicitar serviços de acesso de dados para cada um desses conjuntos de opções e, em seguida, preencher um modelo ou `ViewBag` com cada conjunto de opções para ser associado.

Uma abordagem alternativa injeta services diretamente no modo de exibição para obter as opções. Isso minimiza a quantidade de código necessário para o controlador, movendo essa lógica de construção do elemento de exibição para o modo de exibição em si. A ação de controlador para exibir um formulário de edição de perfil só precisa passar o formulário a instância de perfil:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

O formulário HTML usado para atualizar essas preferências inclui listas suspensas para três propriedades:

![Atualize o modo de perfil com um formulário que permite que a entrada de nome, sexo, estado e cor favorita.](dependency-injection/_static/updateprofile.png)

Essas listas são preenchidas por um serviço que tenha sido injetado no modo de exibição:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

O `ProfileOptionsService` é um serviço de nível de interface do usuário criado para fornecer apenas os dados necessários para este formulário:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Não se esqueça de registrar tipos que você vai solicitar por meio de injeção de dependência no `ConfigureServices` método *Startup.cs*.

## <a name="overriding-services"></a>Serviços de substituição

Além de injeção de novos serviços, essa técnica pode ser usada para substituir os serviços anteriormente injetados em uma página. A figura a seguir mostra todos os campos disponíveis na página usada no primeiro exemplo:

![Menu contextual do IntelliSense em uma lista de campos de Html, componente, StatsService e Url do símbolo @](dependency-injection/_static/razor-fields.png)

Como você pode ver, os campos padrão incluem `Html`, `Component`, e `Url` (bem como o `StatsService` que é injetado). Se para a instância que você deseja substituir os auxiliares de HTML padrão com seu próprio, você pode facilmente fazer isso usando `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Se você quiser estender os serviços existentes, você pode simplesmente usar essa técnica ao herdar de ou encapsulamento a implementação existente com seus próprios.

## <a name="see-also"></a>Consulte também

* Blog do Simon Timms: [Obtendo dados de pesquisa em modo de exibição](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
