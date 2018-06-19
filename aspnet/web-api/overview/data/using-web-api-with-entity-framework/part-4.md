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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872683"
---
<a name="handling-entity-relations"></a>Tratamento de relações de entidade
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Esta seção descreve alguns detalhes de como o EF carrega entidades relacionadas e como lidar com propriedades de navegação circular em suas classes de modelo. (Esta seção fornece os dados de conhecimento do plano de fundo e não é necessário para concluir o tutorial. Se preferir, vá para [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Carregamento em vez de carregamento lento rápido

Ao usar o EF com um banco de dados relacional, é importante entender como o EF carrega os dados relacionados.

Também é útil ver as consultas SQL que gera EF. Para rastrear o SQL, adicione a seguinte linha de código para o `BookServiceContext` construtor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Se você enviar uma solicitação GET para /api/books, ele retorna JSON com o seguinte:

[!code-console[Main](part-4/samples/sample2.cmd)]

Você pode ver que a propriedade Author é nula, mesmo que o catálogo contém uma AuthorId válido. Isso ocorre porque o EF não está carregando as entidades relacionadas do autor. O log de rastreamento da consulta SQL confirma isso:

[!code-console[Main](part-4/samples/sample3.sql)]

A instrução SELECT usa da tabela de livros e não faz referência à tabela de autor.

Para referência, aqui está o método de `BooksController` classe que retorna a lista de livros.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Vamos ver como podemos voltar autor como parte dos dados JSON. Há três maneiras de carregar os dados relacionados no Entity Framework: carregamento rápido, carregamento lento e carregamento explícito. Há vantagens e desvantagens de cada técnica, portanto, é importante entender como eles funcionam.

### <a name="eager-loading"></a>Carregamento adiantado

Com *carregamento adiantado*, EF carrega entidades relacionadas como parte da consulta de banco de dados inicial. Para executar o carregamento rápido, use o **System.Data.Entity.Include** método de extensão.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Isso informa ao EF para incluir os dados do autor da consulta. Se você fizer essa alteração e executar o aplicativo, agora os dados JSON tem esta aparência:

[!code-console[Main](part-4/samples/sample6.cmd)]

O log de rastreamento mostra que o EF executada uma junção nas tabelas de catálogo e um autor.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Carregamento preguiçoso

Com o carregamento lento, EF carrega uma entidade relacionada automaticamente quando a propriedade de navegação para essa entidade é cancelada. Para habilitar o carregamento lento, verifique a propriedade de navegação virtual. Por exemplo, na classe livro:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Agora, considere o seguinte código:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Quando o carregamento lento está habilitado, acessando o `Author` propriedade `books[0]` faz com que o EF consultar o banco de dados para o autor.

Carregamento preguiçoso requer várias viagens de banco de dados, como EF envia uma consulta de cada vez, ele recupera uma entidade relacionada. Em geral, convém carregamento lento desabilitado para objetos que você serializar. O serializador precisa ler todas as propriedades no modelo, o que dispara a carregar as entidades relacionadas. Por exemplo, aqui estão as consultas SQL ao EF serializa a lista de livros com carregamento lento habilitado. Você pode ver que o EF faz três consultas separadas para os autores de três.

[!code-console[Main](part-4/samples/sample10.sql)]

Ainda há ocasiões em que você talvez queira usar o carregamento lento. Carregamento adiantado pode fazer com que o EF gerar uma junção muito complexa. Ou você pode precisar entidades relacionadas para um pequeno subconjunto dos dados e carregamento preguiçoso seria mais eficiente.

É uma maneira de evitar problemas de serialização serializar objetos de transferência de dados (DTOs) em vez de objetos de entidade. Vou mostrar essa abordagem posteriormente neste artigo.

### <a name="explicit-loading"></a>Carregamento explícito

Carregamento explícito é semelhante ao carregamento lento, exceto que você explicitamente obter os dados relacionados no código; Isso não acontece automaticamente quando você acessa uma propriedade de navegação. Carregamento explícito oferece mais controle sobre quando carregar dados relacionados, mas requer código extra. Para obter mais informações sobre carregamento explícito, consulte [entidades relacionadas ao carregar](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propriedades de navegação e referências circulares

Ao defini os modelos de catálogo e autor, defini uma propriedade de navegação no `Book` classe para a relação do autor do catálogo, mas eu não definiu uma propriedade de navegação na outra direção.

O que acontece se você adicionar a propriedade de navegação correspondente para o `Author` classe?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Infelizmente, isso cria um problema ao serializar os modelos. Se você carregar os dados relacionados, ele cria um gráfico de objeto circular.

![](part-4/_static/image1.png)

Quando tenta o formatador JSON ou XML serializar o gráfico, ela irá gerar uma exceção. Os dois formatadores geram mensagens de exceção diferente. Aqui está um exemplo para o formatador JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Aqui está o formatador XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Uma solução é usar DTOs, que será descrito na próxima seção. Como alternativa, você pode configurar os formatadores JSON e XML para manipular os ciclos de gráfico. Para obter mais informações, consulte [tratamento referências circulares do objeto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Para este tutorial, você não precisa de `Author.Book` propriedade de navegação, portanto você pode omiti-lo.

> [!div class="step-by-step"]
> [Anterior](part-3.md)
> [Próximo](part-5.md)
