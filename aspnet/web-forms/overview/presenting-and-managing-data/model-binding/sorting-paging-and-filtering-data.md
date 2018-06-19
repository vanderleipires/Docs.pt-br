---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Classificação, paginação e filtragem de dados com o modelo de associação e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885400"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Classificação, paginação e filtragem de dados com o modelo de associação e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.
> 
> Este tutorial mostra como adicionar classificação, paginação e filtragem dos dados por meio de associação de modelo.
> 
> Este tutorial baseia-se no projeto criado no primeiro [parte](retrieving-data.md) da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>Será possível compilar

Neste tutorial, você vai:

1. Habilitar classificação e paginação dos dados
2. Habilitar a filtragem de dados com base em uma seleção pelo usuário

## <a name="add-sorting"></a>Adicionar uma classificação

Habilitar a classificação em GridView é muito fácil. No arquivo Student.aspx, basta definir **AllowSorting** para **true** em GridView. Você não precisa definir um **SortExpression** como DataField automaticamente é usado o valor para cada coluna. GridView modifica a consulta para incluir ordenando os dados pelo valor selecionado. O código realçado a seguir mostra a adição, que você precisa fazer para habilitar a classificação.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Executar o aplicativo web e testar classificação registros do aluno pelos valores em colunas diferentes.

![alunos de classificação](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Adicionar paginação

Habilitar paginação também é muito fácil. Em GridView, defina o **AllowPaging** propriedade **true** e defina o **PageSize** propriedade para o número de registros que você deseja exibir em cada página. Neste tutorial, você pode configurá-lo a 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Execute o aplicativo web e observe que agora os registros são divididos em várias páginas com não mais de 4 registros exibidos em uma única página.

![Adicionar paginação](sorting-paging-and-filtering-data/_static/image4.png)

Execução de consulta deferida melhora a eficiência do aplicativo. Em vez de recuperar todo o conjunto de dados, o GridView modifica a consulta para recuperar somente os registros para a página atual.

## <a name="filter-records-by-user-selection"></a>Filtrar registros por seleção de usuário

Associação de modelo adiciona vários atributos que permitem que você designar como definir o valor para um parâmetro em um método de associação de modelo. Esses atributos estão no **System.Web.ModelBinding** namespace. Entre elas estão:

- Controle
- Cookie
- Formulário
- Perfil
- QueryString
- RouteData
- Session
- Perfil de usuário
- ViewState

Neste tutorial, você usará o valor de um controle para filtrar quais registros são exibidos em GridView. Você adicionará o **controle** de atributo para o método de consulta que você tenha criado anteriormente. Em um [posteriormente](using-query-string-values-to-retrieve-data.md) tutorial, você aplicará a **QueryString** de atributo para um parâmetro para especificar que o valor do parâmetro vêm de um valor de cadeia de caracteres de consulta.

Primeiro, acima ValidationSummary, adicione uma lista suspensa de filtragem que alunos são mostrados.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

No arquivo code-behind, modifique o método de seleção para receber um valor do controle e defina o nome do parâmetro com o nome do controle que fornece o valor.

Você deve adicionar um **usando** instrução para o **System.Web.ModelBinding** namespace para resolver o atributo de controle.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

O código a seguir mostra o método select funcionou novamente para filtrar os dados retornados com base no valor da lista suspensa. Adicionar um atributo de controle antes de um parâmetro especifica que o valor para esse parâmetro vêm de um controle com o mesmo nome.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Execute o aplicativo web e selecione valores diferentes na lista suspensa lista para filtrar a lista de alunos.

![alunos de filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você habilitou a classificação e paginação dos dados. Você também habilitado filtrando os dados pelo valor de um controle.

Na próxima [tutorial](integrating-jquery-ui.md) aprimorará a interface do usuário, integrando um widget JQuery UI para o modelo de dados dinâmicos.

> [!div class="step-by-step"]
> [Anterior](updating-deleting-and-creating-data.md)
> [Próximo](integrating-jquery-ui.md)
