---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Classificação, paginação e filtragem de dados com a associação de modelos e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 86ddedb68b8d18057cd2f7d68e795efda33734b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833765"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Classificação, paginação e filtragem de dados com a associação de modelos e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.
> 
> Este tutorial mostra como adicionar classificação, paginação e filtragem dos dados por meio de associação de modelos.
> 
> Este tutorial se baseia no projeto criado no primeiro [parte](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>O que você vai criar

Neste tutorial, você vai:

1. Habilitar a classificação e paginação dos dados
2. Habilitar a filtragem dos dados com base em uma seleção pelo usuário

## <a name="add-sorting"></a>Adicionar uma classificação

Habilitar a classificação em GridView é muito fácil. No arquivo Student.aspx, basta definir **AllowSorting** à **verdadeira** no GridView. Não é necessário definir uma **SortExpression** valor para cada coluna como DataField é usado automaticamente. O GridView modifica a consulta para incluir ordenando os dados pelo valor selecionado. O código realçado a seguir mostra a adição, que você precisa fazer para habilitar a classificação.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Executar o aplicativo web e teste de classificação de registros de alunos por valores em colunas diferentes.

![alunos de classificação](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Adicionar paginação

Habilitar paginação também é muito fácil. Em GridView, defina as **AllowPaging** propriedade a ser **verdadeiro** e defina o **PageSize** propriedade para o número de registros que você deseja exibir em cada página. Neste tutorial, você pode configurá-lo a 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Executar o aplicativo web e observe que agora os registros são divididos em várias páginas com não mais do que 4 registros exibidos em uma única página.

![Adicionar paginação](sorting-paging-and-filtering-data/_static/image4.png)

Execução de consulta adiada melhora a eficiência do aplicativo. Em vez de recuperar todo o conjunto de dados, o GridView modifica a consulta para recuperar apenas os registros para a página atual.

## <a name="filter-records-by-user-selection"></a>Filtrar registros por seleção do usuário

Associação de modelos adiciona vários atributos que permitem que você designar como definir o valor para um parâmetro em um método de associação de modelo. Esses atributos estão na **modelbinding** namespace. Entre elas estão:

- Controle
- Cookie
- Formulário
- Perfil
- QueryString
- RouteData
- Session
- Perfil de usuário
- ViewState

Neste tutorial, você usará o valor de um controle para filtrar quais registros são exibidos em GridView. Você adicionará os **controle** de atributo para o método de consulta que você criou anteriormente. Em um [mais tarde](using-query-string-values-to-retrieve-data.md) tutorial, você aplicará a **QueryString** de atributo para um parâmetro para especificar que o valor do parâmetro vem de um valor de cadeia de caracteres de consulta.

Em primeiro lugar, acima ValidationSummary, adicione uma lista suspensa de filtragem que os alunos são mostrados.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

No arquivo code-behind, modifique o método select para receber um valor do controle e defina o nome do parâmetro como o nome do controle que fornece o valor.

Você deve adicionar uma **usando** instrução para o **modelbinding** namespace a resolver o atributo de controle.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

O código a seguir mostra o método select novamente trabalhado para filtrar os dados retornados com base no valor da lista suspensa. Adicionando um atributo de controle antes de um parâmetro especifica que o valor para esse parâmetro vem de um controle com o mesmo nome.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Executar o aplicativo web e selecione diferentes valores na lista suspensa lista para filtrar a lista de alunos.

![alunos de filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você habilitou a classificação e paginação dos dados. Você também habilitado filtrando os dados pelo valor de um controle.

Nos próximos [tutorial](integrating-jquery-ui.md) você aprimorará a interface do usuário, integrando um widget JQuery UI para o modelo de dados dinâmicos.

> [!div class="step-by-step"]
> [Anterior](updating-deleting-and-creating-data.md)
> [Próximo](integrating-jquery-ui.md)
