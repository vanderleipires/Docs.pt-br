---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 5 | Microsoft Docs"
author: tdykstra
description: "O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 5efc5ff367d5da5df060eba0028399af898a69fa
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 5
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Trabalhando com dados relacionados, continuação

No tutorial anterior, você começou a usar o `EntityDataSource` controle para trabalhar com dados relacionados. Exibidos vários níveis de hierarquia e editar dados nas propriedades de navegação. Neste tutorial, você continuará a trabalhar com dados relacionados, adicionando e excluindo relações e adicionando uma nova entidade que tem uma relação com uma entidade existente.

Você criará uma página que adiciona cursos que são atribuídos para departamentos. Os departamentos já existirem, e quando você cria um novo curso, ao mesmo tempo você vai estabelecer uma relação entre ele e um departamento existente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Você também criará uma página que funciona com uma relação muitos-para-muitos, atribuindo um instrutor a um curso (Adicionar uma relação entre duas entidades que você selecionar) ou removendo um instrutor de um curso (para remover uma relação entre duas entidades que você Selecione). No banco de dados, adicionar uma relação entre um instrutor e um curso resulta em uma nova linha que está sendo adicionada para o `CourseInstructor` tabela de associação; remover uma relação envolve a exclusão de uma linha do `CourseInstructor` tabela de associação. No entanto, você fazer isso no Entity Framework definindo propriedades de navegação, sem fazer referência ao `CourseInstructor` tabela explicitamente.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Adicionar uma entidade com uma relação a uma entidade existente

Criar uma nova página da web denominado *CoursesAdd.aspx* que usa o *Site.Master* página mestra e adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Essa marcação cria um `EntityDataSource` controle que seleciona cursos, que permite a inserção e que especifica um manipulador para o `Inserting` evento. Você usará o manipulador para atualizar o `Department` propriedade de navegação quando um novo `Course` entidade é criada.

A marcação também cria um `DetailsView` controle a ser usado para adicionar novos `Course` entidades. A marcação usa campos vinculados para `Course` propriedades da entidade. Insira o `CourseID` porque isso não é um campo de ID gerada pelo sistema. Em vez disso, é um número que deve ser especificado manualmente, quando o curso é criado.

Usar um campo de modelo para o `Department` propriedade de navegação como propriedades de navegação não podem ser usadas com `BoundField` controles. O campo de modelo fornece uma lista suspensa para selecionar o departamento. A lista suspensa é vinculada ao `Departments` entidade definida usando `Eval` em vez de `Bind`, novamente porque você não pode associar diretamente as propriedades de navegação para atualizá-los. Especifique um manipulador para o `DropDownList` do controle `Init` evento para que você pode armazenar uma referência para o controle para uso pelo código que atualiza o `DepartmentID` chave estrangeira.

Em *CoursesAdd.aspx.cs* logo após a declaração de classe parcial, adicione um campo de classe para manter uma referência para o `DepartmentsDropDownList` controle:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Adicionar um manipulador para o `DepartmentsDropDownList` do controle `Init` evento para que você pode armazenar uma referência para o controle. Isso permite que você obtenha o valor que o usuário tiver inserido e usá-lo para atualizar o `DepartmentID` valor o `Course` entidade.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Adicionar um manipulador para o `DetailsView` do controle `Inserting` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Quando o usuário clica `Insert`, o `Inserting` evento é gerado antes que o novo registro é inserido. Obtém o código no manipulador do `DepartmentID` do `DropDownList` controlar e usa-o para definir o valor que será usado para o `DepartmentID` propriedade do `Course` entidade.

O Entity Framework cuidará da adição deste curso para o `Courses` propriedade de navegação associada `Department` entidade. Ele também adiciona ao departamento a `Department` propriedade de navegação a `Course` entidade.

Execute a página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Insira uma ID, um título, um número de créditos e selecione um departamento, clique **inserir**.

Execute o *Courses.aspx* página e, em seguida, selecione o mesmo departamento para ver o novo curso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Trabalhando com relações muitos-para-muitos

A relação entre o `Courses` conjunto de entidades e a `People` conjunto de entidades é uma relação muitos-para-muitos. Um `Course` entidade tem uma propriedade de navegação nomeada `People` que pode conter zero, um ou mais relacionadas `Person` entidades (representando instrutores atribuídos ensinar curso). E um `Person` entidade tem uma propriedade de navegação nomeada `Courses` que pode conter zero, um ou mais relacionadas `Course` entidades (representando cursos é atribuído pelo instrutor que ensinar). Um instrutor pode ensinar vários cursos e um curso pode ser ensinado por vários instrutores. Nesta seção do passo a passo, você irá adicionar e remover relações entre `Person` e `Course` entidades Atualizando as propriedades de navegação das entidades relacionadas.

Criar uma nova página da web denominado *InstructorsCourses.aspx* que usa o *Site.Master* página mestra e adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Essa marcação cria um `EntityDataSource` controle que recupera o nome e `PersonID` de `Person` entidades para instrutores. Um `DropDrownList` controle está vinculado a `EntityDataSource` controle. O `DropDownList` controle especifica um manipulador para o `DataBound` evento. Você usará este manipulador listas suspensas databind dois que exibem cursos.

A marcação também cria o seguinte grupo de controles a ser usado para atribuir um curso para o instrutor selecionado:

- Um `DropDownList` controle para selecionar um curso para atribuir. Esse controle será preenchido com cursos que atualmente não estão atribuídos ao instrutor selecionado.
- Um `Button` controle para iniciar a atribuição.
- Um `Label` controle para exibir uma mensagem de erro se a atribuição falhará.

Por fim, a marcação também cria um grupo de controles a ser usado para remover um curso do instrutor selecionado.

Em *InstructorsCourses.aspx.cs*, adicionar um uso instrução:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Adicione um método para preencher as duas listas suspensas que exibem cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Esse código obtém todos os cursos do `Courses` entidade definida e obtém os cursos do `Courses` propriedade de navegação a `Person` entidade para o instrutor selecionado. Ele determina quais cursos são atribuídos a essa instrutor e preenche a lista suspensa adequadamente.

Adicionar um manipulador para o `Assign` do botão `Click` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Esse código obtém a `Person` entidade para o instrutor selecionado, obtém o `Course` entidade para o curso selecionado e adiciona o curso selecionado para o `Courses` propriedade de navegação do instrutor `Person` entidade. Em seguida, ele salva as alterações no banco de dados e preenche a lista suspensa para que os resultados podem ser vistos imediatamente.

Adicionar um manipulador para o `Remove` do botão `Click` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Esse código obtém a `Person` entidade para o instrutor selecionado, obtém o `Course` entidade para o curso selecionado e remove o curso selecionado do `Person` da entidade `Courses` propriedade de navegação. Em seguida, ele salva as alterações no banco de dados e preenche a lista suspensa para que os resultados podem ser vistos imediatamente.

Adicione código para o `Page_Load` método que garante que as mensagens de erro não são visíveis quando não há nenhum erro de relatório e adicionar manipuladores para o `DataBound` e `SelectedIndexChanged` eventos da lista suspensa instrutores para preencher a lista suspensa de cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Execute a página.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Selecione um instrutor. O **atribuir um curso** lista suspensa exibe os cursos instrutor não ensina, e o **remover um curso** lista suspensa exibe os cursos instrutor já está atribuído a. No **atribuir um curso** seção, selecione um curso e, em seguida, clique em **atribuir**. O curso move para o **remover um curso** lista suspensa. Selecione um curso no **remover um curso** seção e clique em **remover *.* O curso move para o **atribuir um curso** lista suspensa.

Agora você já viu alguns mais maneiras de trabalhar com dados relacionados. O tutorial a seguir, você aprenderá como usar herança no modelo de dados para melhorar a facilidade de manutenção de seu aplicativo.

>[!div class="step-by-step"]
[Anterior](the-entity-framework-and-aspnet-getting-started-part-4.md)
[Próximo](the-entity-framework-and-aspnet-getting-started-part-6.md)
