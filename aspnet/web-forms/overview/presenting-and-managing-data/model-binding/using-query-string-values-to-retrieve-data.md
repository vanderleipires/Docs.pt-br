---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelos e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: b4615d004a32d00b91635bc321a2d4ea792fddbe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837257"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Usando valores de cadeia de caracteres de consulta para filtrar os dados com a associação de modelos e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.
> 
> Este tutorial mostra como passar um valor na cadeia de caracteres de consulta e use esse valor para recuperar dados por meio de associação de modelos.
> 
> Este tutorial se baseia no projeto criado a [anteriores](retrieving-data.md) partes da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.


## <a name="what-youll-build"></a>O que você vai criar

Neste tutorial, você vai:

1. Adicione uma nova página para mostrar os cursos registrados de um aluno
2. Recuperar os cursos registrados do aluno selecionado com base em um valor de cadeia de caracteres de consulta
3. Adicionar um hiperlink com um valor de cadeia de caracteres de consulta da exibição de grade para a nova página

As etapas neste tutorial são bastante semelhantes ao que você fez no anterior [tutorial](sorting-paging-and-filtering-data.md) para filtrar os alunos exibidos com base na seleção do usuário em uma lista suspensa. Neste tutorial, você usou o **controle** atributo do método select para especificar que o valor do parâmetro vem de um controle. Neste tutorial, você usará o **QueryString** atributo do método select para especificar que o valor do parâmetro vem da cadeia de consulta.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Adicionar nova página para exibir os cursos de student

Adicione um novo formulário da web que usa a página mestra do site e nomeie a página **cursos**.

No **Courses.aspx** de arquivo, adicione uma exibição de grade para exibir os cursos do aluno selecionado.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definir o método select

Na **Courses.aspx.cs**, você adicionará o método select com o nome especificado na exibição de grade **SelectMethod** propriedade. Nesse método, você define a consulta para recuperar os cursos de um aluno e especificar que o parâmetro vem de um valor de cadeia de caracteres de consulta com o mesmo nome que o parâmetro.

Primeiro, você deve adicionar o seguinte **usando** instruções.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Em seguida, adicione o seguinte código ao Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

O atributo de QueryString significa que um valor de cadeia de caracteres de consulta chamado StudentID é automaticamente atribuído ao parâmetro nesse método.

## <a name="add-hyperlink-with-query-string-value"></a>Adicionar um hiperlink com valor de cadeia de caracteres de consulta

Na exibição de grade Students.aspx, você adicionará um campo de hiperlink que vincula-se à sua nova página de cursos. O hiperlink incluirá um valor de cadeia de caracteres de consulta com a id do aluno.

No Students.aspx, adicione o seguinte campo para as colunas de exibição de grade logo abaixo do campo de Total de créditos.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Execute o aplicativo e observe que a exibição de grade agora inclui o link de cursos.

![Adicionar hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

Quando você clica em um dos links, você verá os cursos do aluno registrados.

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você adicionou um link com um valor de cadeia de caracteres de consulta. Você usou esse valor de cadeia de caracteres de consulta para o valor do parâmetro do método select.

Nos próximos [tutorial](adding-business-logic-layer.md), você moverá o código de arquivos code-behind para uma camada de lógica de negócios e uma camada de acesso a dados.

> [!div class="step-by-step"]
> [Anterior](integrating-jquery-ui.md)
> [Próximo](adding-business-logic-layer.md)
