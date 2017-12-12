---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "Banco de dados EF primeiro com o ASP.NET MVC: gerar exibições | Microsoft Docs"
author: tfitzmac
description: "Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Banco de dados EF primeiro com o ASP.NET MVC: gerando modos de exibição
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra no uso de ASP.NET Scaffolding para gerar os controladores e exibições.


## <a name="add-scaffold"></a>Adicionar scaffold

Você está pronto para gerar o código que fornecerá as operações de dados padrão para as classes de modelo. Adicione o código adicionando um item de scaffold. Há muitas opções para o tipo de estrutura, que você pode adicionar; Neste tutorial, o scaffold incluirá um controlador e modos de exibição que correspondem aos modelos de Student e registro que você criou na seção anterior.

Para manter a consistência em seu projeto, você adicionará o novo controlador para existente **controladores** pasta. Clique com botão direito do **controladores** pasta e selecione **adicionar** – **Novo Item de Scaffold**.

![Adicionar scaffold](generating-views/_static/image1.png)

Selecione o **controlador MVC 5 com modos de exibição usando o Entity Framework** opção. Esta opção irá gerar o controlador e os modos de exibição para atualizar, excluir, criar e exibir os dados em seu modelo.

![Adicionar controlador mvc](generating-views/_static/image2.png)

Selecione **aluno** para a classe de modelo e selecione o **ContosoUniversityEntities** para a classe de contexto. Manter o nome do controlador como **StudentsController**,

![Especifique o controlador](generating-views/_static/image3.png)

Clique em **Adicionar**.

Se você receber um erro, pode ser porque você não compilar o projeto na seção anterior. Nesse caso, tente compilar o projeto e, em seguida, adicione o item de scaffolding novamente.

Após a conclusão do processo de geração de código, você verá um novo controlador e modos de exibição em seu projeto.

![Mostrar exibições](generating-views/_static/image4.png)

Execute as mesmas etapas novamente, mas adicionar um scaffold para a classe de registro. Quando terminar, você deve ter uma **EnrollmentsController.cs** arquivo e uma pasta sob **exibições** chamado **registros** com Create, Delete, detalhes, editar e índice Modos de exibição.

![Mostrar exibições](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Adicionar links às novas exibições

Para tornar mais fácil para que você navegue até seu novos modos de exibição, você pode adicionar alguns hiperlinks para as exibições de índice para estudantes e registros. Abra o arquivo em **Views/Home/Index.cshtml**, que é a home page do seu site. Adicione o seguinte código abaixo o jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Para o método ActionLink, o primeiro parâmetro é o texto a ser exibido no link. O segundo parâmetro é a ação e o terceiro parâmetro é o nome do controlador. Por exemplo, o primeiro link aponta para a ação de índice em StudentsController. O hiperlink real é construído com esses valores. O primeiro link, por fim, leva os usuários para o **cshtml** dentro do arquivo a **modos de exibição/alunos** pasta.

## <a name="display-student-views"></a>Exibições do aluno

Você verificará que o código adicionado ao seu projeto corretamente exibe uma lista dos alunos e permite aos usuários editar, criar ou excluir os registros do aluno no banco de dados.

Clique com botão direito do **Views/Home/Index.cshtml** de arquivo e selecione **exibir no navegador**. Nessa página, clique no link para a lista de alunos.

![](generating-views/_static/image6.png)

Nessa página, observe a lista dos alunos e links para modificar esses dados.

![lista de estudantes](generating-views/_static/image7.png)

Clique o **criar novo** vincular e fornecer alguns valores para um novo aluno.

![Criar novo aluno](generating-views/_static/image8.png)

Clique em **criar**e observe o aluno novo é adicionado à lista.

![lista com novo aluno](generating-views/_static/image9.png)

Selecione o **editar** vincular e alterar alguns dos valores de um aluno.

![Editar aluno](generating-views/_static/image10.png)

Clique em **salvar**e observe o registro do aluno foi alterado.

Por fim, selecione o **excluir** vincular e confirme que deseja excluir o registro clicando o **excluir** botão.

![Excluir aluno](generating-views/_static/image11.png)

Sem gravar nenhum código, você adicionou exibições que executam operações comuns nos dados na tabela de alunos.

Você pode ter observado que o rótulo de texto para um campo baseia-se na propriedade de banco de dados (como **LastName**) que não é necessariamente o que você deseja exibir na página da web. Por exemplo, você pode preferir que o rótulo a ser **Sobrenome**. Você corrigirá esse problema de exibição no tutorial posteriormente.

## <a name="display-enrollment-views"></a>Exibições de registro

O banco de dados inclui uma relação um-para-muitos entre as tabelas Student e registro e uma relação um-para-muitos entre as tabelas de curso e registro. Os modos de exibição para o registro corretamente lidar com essas relações. Navegue até a home page do seu site e selecione o **lista de registros** link e, em seguida, o **criar novo** link. O modo de exibição exibe um formulário para criar um novo registro. Em particular, observe que o formulário contém duas listas suspensas que são preenchidas com valores de tabelas relacionadas.

![Criar registro](generating-views/_static/image12.png)

Além disso, os valores fornecidos a validação é aplicada automaticamente com base no tipo de dados do campo. Classificação exige um número para que uma mensagem de erro será exibida se você tentar fornecer um valor incompatível.

![mensagem de validação](generating-views/_static/image13.png)

Você verificou que os modos de exibição gerado automaticamente habilitam usuários a trabalhar com os dados no banco de dados. No próximo tutorial nesta série, você atualizará o banco de dados e fazer as alterações correspondentes no aplicativo web.

>[!div class="step-by-step"]
[Anterior](creating-the-web-application.md)
[Próximo](changing-the-database.md)
