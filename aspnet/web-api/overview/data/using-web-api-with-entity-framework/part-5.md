---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Criar objetos de transferência de dados (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: a1be1b72559aaffdf530402313f58d6c2e25b189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828157"
---
<a name="create-data-transfer-objects-dtos"></a>Criar objetos de transferência de dados (DTOs)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Direita agora, nossa API web expõe as entidades de banco de dados para o cliente. O cliente recebe os dados que mapeia diretamente para suas tabelas de banco de dados. No entanto, que nem sempre é uma boa ideia. Às vezes você deseja alterar a forma dos dados que você envia ao cliente. Por exemplo, é possível:

- Remova referências circulares (consulte a seção anterior).
- Oculte propriedades específicas que os clientes não devem para exibir.
- Omita algumas propriedades para reduzir o tamanho da carga.
- Mescle gráficos de objetos que contêm objetos aninhados, para torná-las mais conveniente para clientes.
- Evite "vulnerabilidades de overposting". (Consulte [validação de modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para uma discussão sobre excesso de postagem.)
- Desacoplar a camada de serviço de sua camada de banco de dados.

Para fazer isso, você pode definir um *objeto de transferência de dados* (DTO). Um DTO é um objeto que define como os dados serão enviados pela rede. Vamos ver como isso funciona com a entidade de livro. Na pasta modelos, adicione duas classes DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

O `BookDetailDTO` classe inclui todas as propriedades do modelo de livro, exceto que `AuthorName` é uma cadeia de caracteres que conterá o nome do autor. O `BookDTO` classe contém um subconjunto de propriedades de `BookDetailDTO`.

Em seguida, substitua os dois métodos GET no `BooksController` classe, com as versões que retornar DTOs. Vamos usar o LINQ **selecionar** instrução para converter de entidades do livro em DTOs.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Aqui está o SQL gerado pelo novo `GetBooks` método. Você pode ver que o EF converte o LINQ **selecionar** em uma instrução SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Por fim, modifique o `PostBook` método para retornar um DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> Neste tutorial, estamos convertendo em DTOs manualmente no código. Outra opção é usar uma biblioteca, como [AutoMapper](http://automapper.org/) que manipula a conversão automaticamente.
> 
> [!div class="step-by-step"]
> [Anterior](part-4.md)
> [Próximo](part-6.md)
