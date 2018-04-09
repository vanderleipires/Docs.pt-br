---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelo e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Usando valores de cadeia de caracteres de consulta para filtrar dados com o modelo de associação e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.
> 
> Este tutorial mostra como passar um valor de cadeia de caracteres de consulta e use esse valor para recuperar os dados por meio de associação de modelo.
> 
> Este tutorial baseia-se no projeto criado no [anteriores](retrieving-data.md) partes da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>Será possível compilar

Neste tutorial, você vai:

1. Adicione uma nova página para mostrar os cursos registrados para um aluno
2. Recuperar os cursos registrados do aluno selecionado com base em um valor de cadeia de caracteres de consulta
3. Adicionar um hiperlink com um valor de cadeia de caracteres de consulta do modo de exibição de grade para a nova página

As etapas neste tutorial são bastante semelhantes ao que você fez no anteriores [tutorial](sorting-paging-and-filtering-data.md) para filtrar os alunos exibidos com base na seleção do usuário em uma lista suspensa. Neste tutorial, você usou o **controle** atributo do método select para especificar que o valor do parâmetro vêm de um controle. Neste tutorial, você usará o **QueryString** atributo do método select para especificar que o valor do parâmetro é proveniente de cadeia de caracteres de consulta.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Adicionar nova página para exibir os cursos de student

Adicionar um novo formulário da web que usa a página mestra Site.master e nomeie a página **cursos**.

No **Courses.aspx** de arquivo, adicione uma exibição de grade para exibir os cursos do aluno selecionado.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definir o método select

Em **Courses.aspx.cs**, você irá adicionar método select com o nome especificado na exibição de grade **SelectMethod** propriedade. Nesse método, você vai definir a consulta para recuperar os cursos student e especificar que o parâmetro vem de um valor de cadeia de caracteres de consulta com o mesmo nome que o parâmetro.

Primeiro, você deve adicionar o seguinte **usando** instruções.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Em seguida, adicione o seguinte código para Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

O atributo de QueryString significa que um valor de cadeia de caracteres de consulta chamado StudentID é automaticamente atribuído ao parâmetro nesse método.

## <a name="add-hyperlink-with-query-string-value"></a>Adicionar hiperlink com valor de cadeia de caracteres de consulta

O modo de exibição de grade em Students.aspx, você adicionará um campo de hiperlink que contém links para a nova página de cursos. O hiperlink incluirá um valor de cadeia de caracteres de consulta com a identificação do aluno.

No Students.aspx, adicione o campo a seguir para as colunas de exibição de grade logo abaixo do campo de Total de créditos.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Execute o aplicativo e observe que a exibição de grade agora inclui o link de cursos.

![Adicionar hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

Quando você clica em um dos links, você verá os cursos registrados que student.

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você adicionou um link com um valor de cadeia de caracteres de consulta. Você usou esse valor de cadeia de caracteres de consulta para o valor do parâmetro do método select.

Na próxima [tutorial](adding-business-logic-layer.md), você moverá o código dos arquivos code-behind em uma camada de lógica de negócios e uma camada de acesso a dados.

> [!div class="step-by-step"]
> [Anterior](integrating-jquery-ui.md)
> [Próximo](adding-business-logic-layer.md)
