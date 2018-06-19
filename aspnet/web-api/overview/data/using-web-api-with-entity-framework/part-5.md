---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Criar objetos de transferência de dados (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878679"
---
<a name="create-data-transfer-objects-dtos"></a>Criar objetos de transferência de dados (DTOs)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Direita agora, nossa API da web expõe as entidades de banco de dados ao cliente. O cliente recebe os dados que mapeia diretamente para as tabelas de banco de dados. No entanto, que não é sempre uma boa ideia. Às vezes você deseja alterar a forma dos dados que você enviar ao cliente. Por exemplo, é possível:

- Remova as referências circulares (consulte a seção anterior).
- Oculte propriedades específicas que os clientes não devem para exibir.
- Omita algumas propriedades para reduzir o tamanho da carga.
- Mesclar os gráficos de objetos que contêm objetos aninhados, para torná-las mais conveniente para os clientes.
- Evite "excesso de lançamento" vulnerabilidades. (Consulte [validação de modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obter uma discussão de lançamento excedente.)
- Desacopla a camada de serviço de sua camada de banco de dados.

Para fazer isso, você pode definir um *o objeto de transferência de dados* (DTO). Um DTO é um objeto que define como os dados serão enviados pela rede. Vamos ver como isso funciona com a entidade de catálogo. Na pasta de modelos, adicione duas classes DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

O `BookDetailDTO` classe inclui todas as propriedades do modelo de catálogo, exceto que `AuthorName` é uma cadeia de caracteres que conterá o nome do autor. O `BookDTO` classe contém um subconjunto de propriedades de `BookDetailDTO`.

Em seguida, substitua os dois métodos GET no `BooksController` classe com versões que retornar DTOs. Vamos usar o LINQ **selecione** instrução converter de entidades de catálogo em DTOs.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Aqui está o SQL gerado pelo novo `GetBooks` método. Você pode ver que o EF converte o LINQ **selecione** em uma instrução SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Finalmente, modifique o `PostBook` método para retornar um DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> Neste tutorial, estamos convertendo em DTOs manualmente no código. Outra opção é usar uma biblioteca como [AutoMapper](http://automapper.org/) que manipula a conversão automaticamente.
> 
> [!div class="step-by-step"]
> [Anterior](part-4.md)
> [Próximo](part-6.md)
