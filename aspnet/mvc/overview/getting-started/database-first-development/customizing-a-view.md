---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Banco de dados do EF primeiro com o ASP.NET MVC: como personalizar um modo de exibição | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: bfbcfd39dd1cf0abe89a00d2958ca010f0e5e109
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376492"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Banco de dados do EF primeiro com o ASP.NET MVC: como personalizar um modo de exibição
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra sobre como alterar os modos de exibição gerados automaticamente para aprimorar a apresentação.


## <a name="add-enrolled-courses-to-student-details"></a>Adicionar cursos registrados para detalhes do aluno

O código gerado fornece um bom ponto de partida para seu aplicativo, mas não necessariamente fornece toda a funcionalidade que você precisa em seu aplicativo. Você pode personalizar o código para atender a determinados requisitos de seu aplicativo. No momento, seu aplicativo não exibe os cursos registrados aluno selecionado. Nesta seção, você adicionará os cursos registrados para cada aluno para o **detalhes** exibição aluno.

Abra **Students/Details.cshtml**e abaixo do último &lt;/dl&gt; guia, mas antes do fechamento &lt;/div&gt; de marca, adicione o código a seguir.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Esse código cria uma tabela que exibe uma linha para cada registro na tabela Enrollment aluno selecionado. O **exibição** método renderiza o HTML para o objeto (modelItem) que representa a expressão. Use o método de exibição (em vez de simplesmente inserindo o valor da propriedade no código) para garantir que o valor é formatado corretamente com base no seu tipo e o modelo para esse tipo. Neste exemplo, cada expressão retorna uma única propriedade de registro atual no loop, e os valores são tipos primitivos que são renderizados como texto.

Navegue até o modo de exibição/índice de alunos novamente e selecione **detalhes** para um dos alunos. Você verá que os cursos registrados foram incluídos no modo de exibição.

![aluno com registro](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](changing-the-database.md)
> [Próximo](enhancing-data-validation.md)
