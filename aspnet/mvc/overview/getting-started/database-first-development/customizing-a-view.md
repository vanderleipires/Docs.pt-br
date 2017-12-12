---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "Banco de dados EF primeiro com o ASP.NET MVC: personalizar um modo de exibição | Microsoft Docs"
author: tfitzmac
description: "Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Banco de dados EF primeiro com o ASP.NET MVC: personalizar um modo de exibição
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra em como alterar os modos de exibição gerado automaticamente para aprimorar a apresentação.


## <a name="add-enrolled-courses-to-student-details"></a>Adicionar cursos registrados detalhes do aluno

O código gerado fornece um bom ponto de partida para o seu aplicativo, mas não necessariamente fornece toda a funcionalidade que você necessita em seu aplicativo. Você pode personalizar o código para atender os requisitos específicos de seu aplicativo. Atualmente, o aplicativo não exibir os cursos registrados do aluno selecionado. Nesta seção, você adicionará os cursos registrados para cada aluno para o **detalhes** exibição do aluno.

Abra **Students/Details.cshtml**e abaixo do último &lt;/dl&gt; guia, mas antes do fechamento &lt;/div&gt; marca, adicione o código a seguir.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Esse código cria uma tabela que exibe uma linha para cada registro na tabela de registro do aluno selecionado. O **exibição** método renderiza HTML para o objeto (modelItem) que representa a expressão. Use o método de exibição (em vez de simplesmente inserindo o valor da propriedade no código) para verificar se o valor é formatado corretamente com base no seu tipo e o modelo para esse tipo. Neste exemplo, cada expressão retorna uma única propriedade de registro atual no loop e os valores são tipos primitivos que são processados como texto.

Navegue até o modo de exibição de alunos/índice novamente e selecione **detalhes** para um dos alunos. Você verá que os cursos registrados foram incluídos na exibição.

![aluno com registro](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
[Anterior](changing-the-database.md)
[Próximo](enhancing-data-validation.md)
