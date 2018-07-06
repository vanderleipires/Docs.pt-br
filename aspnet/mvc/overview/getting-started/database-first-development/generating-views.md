---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Banco de dados do EF primeiro com o ASP.NET MVC: gerando exibições | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 9c85b330ac415d8c73d58b31a5108197b754669f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835322"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Banco de dados do EF primeiro com o ASP.NET MVC: gerando exibições
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra no uso de Scaffolding do ASP.NET para gerar os controladores e modos de exibição.


## <a name="add-scaffold"></a>Adicionar scaffold

Você está pronto para gerar o código que irá fornecer operações de dados padrão para as classes de modelo. Adicione o código adicionando um item de scaffold. Há muitas opções para o tipo de scaffolding que você pode adicionar; Neste tutorial, o scaffold incluirá um controlador e exibições que correspondem aos modelos de aluno e registro que você criou na seção anterior.

Para manter a consistência em seu projeto, você adicionará o novo controlador ao existente **controladores** pasta. Clique com botão direito do **controladores** pasta e selecione **Add** – **New Scaffolded Item**.

![Adicionar scaffold](generating-views/_static/image1.png)

Selecione o **controlador MVC 5 com modos de exibição usando o Entity Framework** opção. Esta opção irá gerar o controlador e exibições para atualizar, excluir, criando e exibindo os dados em seu modelo.

![Adicionar controlador mvc](generating-views/_static/image2.png)

Selecione **aluno** para a classe de modelo e selecione o **ContosoUniversityEntities** para a classe de contexto. Mantenha o nome do controlador como **StudentsController**,

![Especifique o controlador](generating-views/_static/image3.png)

Clique em **Adicionar**.

Se você receber um erro, pode ser porque você não tiver criado o projeto na seção anterior. Nesse caso, tente compilar o projeto e, em seguida, adicione o item com Scaffold novamente.

Após a conclusão do processo de geração de código, você verá um novo controlador e exibições em seu projeto.

![Mostrar modos de exibição](generating-views/_static/image4.png)

Execute as mesmas etapas novamente, mas adicionar um scaffold para a classe de registro. Quando terminar, você deve ter uma **EnrollmentsController.cs** arquivo e uma pasta sob **exibições** denominada **registros** com Create, Delete, detalhes, editar e índice Modos de exibição.

![Mostrar modos de exibição](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Adicionar links às novas exibições

Para tornar mais fácil de navegar para seus novos modos de exibição, você pode adicionar alguns dos hiperlinks para as exibições de índice para estudantes e registros. Abra o arquivo no **Views/Home/Index.cshtml**, que é a home page do seu site. Adicione o código a seguir o jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Para o método ActionLink, o primeiro parâmetro é o texto a ser exibido no link. O segundo parâmetro é a ação e o terceiro parâmetro é o nome do controlador. Por exemplo, o primeiro link aponta para a ação de índice em StudentsController. O hiperlink real é construído com esses valores. O primeiro link, por fim, leva os usuários para o **index. cshtml** dentro do arquivo a **modos de exibição/alunos** pasta.

## <a name="display-student-views"></a>Mostrar exibições de aluno

Você verificará que o código adicionado ao seu projeto corretamente exibe uma lista dos alunos e permite aos usuários editar, criar ou excluir os registros de alunos no banco de dados.

Clique com botão direito do **Views/Home/Index.cshtml** do arquivo e selecione **exibir no navegador**. Nessa página, clique no link para obter a lista de alunos.

![](generating-views/_static/image6.png)

Nessa página, observe a lista de alunos e links para modificar esses dados.

![lista de alunos](generating-views/_static/image7.png)

Clique o **criar novo** vincular e fornecer alguns valores para um novo aluno.

![Criar novo aluno](generating-views/_static/image8.png)

Clique em **criar**e observe o novo aluno é adicionado à sua lista.

![lista com o novo aluno](generating-views/_static/image9.png)

Selecione o **editar** link e alterar alguns dos valores de um aluno.

![Editar aluno](generating-views/_static/image10.png)

Clique em **salvar**e observe o registro de aluno foi alterado.

Por fim, selecione o **exclua** vincular e confirme que você deseja excluir o registro clicando o **excluir** botão.

![Excluir aluno](generating-views/_static/image11.png)

Sem escrever nenhum código, você adicionou as exibições que executam operações comuns nos dados na tabela aluno.

Você deve ter notado que o rótulo de texto para um campo se baseia na propriedade de banco de dados (como **LastName**) que não é necessariamente o que você deseja exibir na página da web. Por exemplo, você pode preferir o rótulo seja **Sobrenome**. Você corrigirá esse problema de exibição posteriormente no tutorial.

## <a name="display-enrollment-views"></a>Mostrar exibições de registro

Seu banco de dados inclui uma relação um-para-muitos entre as tabelas Student e registro e uma relação um-para-muitos entre as tabelas de registro e curso. Os modos de exibição para o registro corretamente lidar com essas relações. Navegue até a home page do seu site e selecione o **lista de registros** link e, em seguida, o **criar novo** link. O modo de exibição exibe um formulário para criar um novo registro de registro. Em particular, observe que o formulário contém duas listas suspensas que são preenchidas com valores das tabelas relacionadas.

![Criar registro](generating-views/_static/image12.png)

Além disso, a validação dos valores fornecidos é aplicada automaticamente com base no tipo de dados do campo. Nível empresarial requer um número, portanto, uma mensagem de erro será exibida se você tentar fornecer um valor incompatível.

![mensagem de validação](generating-views/_static/image13.png)

Você verificou que os modos de exibição gerados automaticamente permitem que os usuários trabalhar com os dados no banco de dados. No próximo tutorial desta série, você irá atualizar o banco de dados e faça as alterações correspondentes no aplicativo web.

> [!div class="step-by-step"]
> [Anterior](creating-the-web-application.md)
> [Próximo](changing-the-database.md)
