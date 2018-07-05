---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Tratamento de relações de entidade | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 7759b828068d99f9975d56671e427ccf6e94aef6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400946"
---
<a name="handling-entity-relations"></a>Tratamento de relações de entidade
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Esta seção descreve alguns detalhes de como o EF carrega entidades relacionadas e como lidar com propriedades de navegação circular em suas classes de modelo. (Esta seção fornece o conhecimento em segundo plano e não é necessário para concluir o tutorial. Se você preferir, vá para [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Carregamento rápido em comparação com o carregamento lento

Ao usar o EF com um banco de dados relacional, é importante entender como o EF carrega dados relacionados.

Também é útil ver as consultas SQL que o EF gera. Para rastrear o SQL, adicione a seguinte linha de código para o `BookServiceContext` construtor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Se você enviar uma solicitação GET para /api/books, ele retorna JSON semelhante ao seguinte:

[!code-console[Main](part-4/samples/sample2.cmd)]

Você pode ver que a propriedade Author é nula, mesmo que o livro contém um AuthorId válido. Isso ocorre porque o EF não está carregando entidades relacionadas do autor. O log de rastreamento da consulta SQL confirma isso:

[!code-console[Main](part-4/samples/sample3.sql)]

A instrução SELECT usa da tabela de livros e não faz referência à tabela de autor.

Para referência, aqui está o método no `BooksController` classe que retorna a lista de livros.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Vamos ver como podemos pode retornar o autor como parte dos dados JSON. Há três maneiras de carregar dados relacionados no Entity Framework: o carregamento explícito, carregamento lento e o carregamento adiantado. Há vantagens e desvantagens, com cada técnica, portanto, é importante entender como eles funcionam.

### <a name="eager-loading"></a>Carregamento adiantado

Com o *adiantado*, EF carrega entidades relacionadas como parte da consulta de banco de dados inicial. Para executar o carregamento adiantado, use o **System.Data.Entity.Include** método de extensão.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Isso informa ao EF para incluir os dados do autor da consulta. Se você fizer essa alteração e executar o aplicativo, agora os dados JSON parecem com este:

[!code-console[Main](part-4/samples/sample6.cmd)]

O log de rastreamento mostra que o EF executada uma junção nas tabelas de catálogo e autor.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Carregamento lento

Com o carregamento lento, o EF carrega automaticamente uma entidade relacionada quando a propriedade de navegação para essa entidade é cancelada. Para habilitar o carregamento lento, verifique a propriedade de navegação virtual. Por exemplo, na classe livro:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Agora, considere o seguinte código:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Quando o carregamento lento estiver habilitado, acessando o `Author` propriedade `books[0]` faz com que o EF consultar o banco de dados para o autor.

Carregamento lento requer várias viagens de banco de dados, pois o EF envia uma consulta cada vez que ele recupera uma entidade relacionada. Em geral, convém carregamento lento desativado para os objetos que você serializar. O serializador precisa ler todas as propriedades no modelo, o que dispara a carregar as entidades relacionadas. Por exemplo, aqui estão as consultas SQL ao EF serializa a lista de livros com carregamento lento ativado. Você pode ver que o EF faz três consultas separadas para os autores de três.

[!code-console[Main](part-4/samples/sample10.sql)]

Ainda há vezes quando você talvez queira usar o carregamento lento. O carregamento adiantado pode fazer com que o EF gerar uma junção muito complexa. Ou talvez seja necessário entidades relacionadas para um pequeno subconjunto dos dados e carregamento lento seria mais eficiente.

É uma maneira de evitar problemas de serialização serializar objetos de transferência de dados (DTOs) em vez de objetos de entidade. Vou mostrar essa abordagem posteriormente neste artigo.

### <a name="explicit-loading"></a>Carregamento explícito

Carregamento explícito é semelhante ao carregamento lento, exceto que você obter explicitamente os dados relacionados no código; Isso não acontece automaticamente quando você acessa uma propriedade de navegação. Carregamento explícito oferece mais controle em relação a quando carregar dados relacionados, mas exige código extra. Para obter mais informações sobre o carregamento explícito, consulte [Carregando entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propriedades de navegação e referências circulares

Quando eu defini os modelos de catálogo e autor, eu defini uma propriedade de navegação no `Book` classe para a relação de autor do livro, mas eu não defini uma propriedade de navegação na outra direção.

O que acontece se você adicionar a propriedade de navegação correspondente para o `Author` classe?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Infelizmente, isso cria um problema quando você serializa os modelos. Se você carregar os dados relacionados, ele cria um gráfico de objeto circular.

![](part-4/_static/image1.png)

Quando o formatador JSON ou XML tenta serializar o grafo, ele lançará uma exceção. Os dois formatadores geram mensagens de exceção diferente. Aqui está um exemplo para o formatador JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Aqui está o formatador XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Uma solução é usar DTOs, que será descrito na próxima seção. Como alternativa, você pode configurar os formatadores JSON e XML para lidar com os ciclos de gráfico. Para obter mais informações, consulte [tratamento referências circulares do objeto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Para este tutorial, você não precisa de `Author.Book` propriedade de navegação, portanto, você pode deixá-lo.

> [!div class="step-by-step"]
> [Anterior](part-3.md)
> [Próximo](part-5.md)
