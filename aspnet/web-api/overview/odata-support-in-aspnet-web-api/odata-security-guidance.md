---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Orientação de segurança para o ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Orientação de segurança para o ASP.NET Web API 2 OData
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve alguns dos problemas de segurança que você deve considerar ao expor um conjunto de dados por meio do OData.

## <a name="edm-security"></a>Segurança EDM

A semântica de consulta é baseada no modelo de dados de entidade (EDM), não os tipos de modelo subjacente. Você pode excluir uma propriedade de EDM e não será visível para a consulta. Por exemplo, suponha que seu modelo incluir um tipo de funcionário com uma propriedade de salário. Deseja excluir essa propriedade do EDM para ocultá-la de clientes.

Há duas maneiras de excluir uma propriedade do EDM. Você pode definir o **[IgnoreDataMember]** atributo da propriedade na classe de modelo:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Você também pode remover a propriedade do EDM programaticamente:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Segurança de consulta

Um cliente mal-intencionado ou simples pode ser capaz de construir uma consulta que demora muito tempo para executar. No pior dos casos isso pode interromper o acesso ao seu serviço.

O **[Queryable]** atributo é um filtro de ação que analisa, valida e aplica a consulta. O filtro converte as opções de consulta em uma expressão LINQ. Quando o controlador OData retorna um **IQueryable** tipo, o **IQueryable** provedor LINQ converte a expressão LINQ em uma consulta. Portanto, desempenho depende do provedor LINQ que é usado e também as características específicas de seu esquema de banco de dados ou conjunto de dados.

Para obter mais informações sobre como usar opções de consulta OData na API da Web ASP.NET, consulte [dando suporte a opções de consulta OData](supporting-odata-query-options.md).

Se você souber que todos os clientes são confiáveis (por exemplo, em um ambiente corporativo), ou se o conjunto de dados é pequeno, o desempenho da consulta não pode ser um problema. Caso contrário, você deve considerar as recomendações a seguir.

- Testar o serviço com várias consultas e o banco de dados de perfil.
- Habilite paginação orientado para servidor evitar o retorno de um grande conjunto de dados em uma consulta. Para obter mais informações, consulte [Server-Driven paginação](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Você precisa $filter e $orderby? Alguns aplicativos podem permitir que o cliente de paginação, usando $top e $skip, mas desabilitar as outras opções de consulta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Considere a possibilidade de restringir $orderby a propriedades em um índice clusterizado. Classificação de dados grandes sem um índice clusterizado está lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Contagem máxima do nó: O **MaxNodeCount** propriedade **[Queryable]** define o número máximo de nós permitido na árvore de sintaxe $filter. O valor padrão é 100, mas você talvez queira definir um valor mais baixo, porque um grande número de nós pode ser lento para compilar. Isso é especialmente verdadeiro se você estiver usando o LINQ para objetos (ou seja, consultas LINQ em uma coleção na memória, sem o uso de um provedor LINQ intermediário). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Considere desabilitar as funções any () e All (), pois elas podem ser lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Se as propriedades de cadeia de caracteres contém grandes cadeias de caracteres #8212for exemplo, uma descrição do produto ou uma entrada de blog & #8212consider desabilitar as funções de cadeia de caracteres. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Considere a desabilitação de filtragem nas propriedades de navegação. Filtrando em Propriedades de navegação pode resultar em uma junção, que pode ser lenta, dependendo de seu esquema de banco de dados. O código a seguir mostra um validador de consultas que impede a filtragem nas propriedades de navegação. Para obter mais informações sobre os validadores de consulta, consulte [validação de consulta](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Considere a possibilidade de restringir $filter consultas escrevendo um validador personalizada para seu banco de dados. Por exemplo, considere estas duas consultas: 

  - Todos os filmes com atores cujo sobrenome começa com 'A'.
  - Todos os filmes lançados em 1994.

    A menos que filmes são indexados por atores, a primeira consulta pode exigir que o mecanismo de banco de dados para verificar a lista completa de filmes. Enquanto a segunda consulta pode ser aceitável, assumindo filmes são indexadas por ano de lançamento.

    O código a seguir mostra um validador que permite a filtragem de propriedades "ReleaseYear" e "Title", mas nenhuma outra propriedade.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Em geral, considere quais funções $filter é necessário. Se os clientes não precisarem a expressividade completa de $filter, você pode limitar as funções permitidas.
