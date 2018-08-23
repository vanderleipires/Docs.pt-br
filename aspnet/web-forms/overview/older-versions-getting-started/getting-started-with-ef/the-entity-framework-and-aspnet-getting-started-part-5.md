---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Introdução ao banco de dados do Entity Framework 4.0 First e 4 do ASP.NET Web Forms - parte 5 | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 3209ab3bca58e0dde90cf279732d177418b034e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825019"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introdução ao banco de dados do Entity Framework 4.0 First e 4 do Web Forms do ASP.NET – parte 5
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial na série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Trabalhando com dados relacionados, continuação

No tutorial anterior, você começou a usar o `EntityDataSource` controle para trabalhar com dados relacionados. Exibidos vários níveis de hierarquia e editar dados nas propriedades de navegação. Neste tutorial, você continuará a trabalhar com dados relacionados, adicionando e excluindo relações e adicionando uma nova entidade que tem uma relação com uma entidade existente.

Você criará uma página que adiciona cursos são atribuídos aos departamentos. Os departamentos já existirem, e quando você cria um novo curso, ao mesmo tempo, você vai estabelecer uma relação entre ele e um departamento existente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Você também criará uma página que funciona com uma relação muitos-para-muitos, atribuindo um instrutor a um curso (adicionando uma relação entre duas entidades que você selecionar) ou removendo um instrutor de um curso (para remover uma relação entre duas entidades que você Selecione). No banco de dados, adicionando uma relação entre um instrutor e um curso resulta em uma nova linha que está sendo adicionada para o `CourseInstructor` tabela de associação; removendo envolve a exclusão de uma linha de uma relação a `CourseInstructor` tabela de associação. No entanto, você fazer isso no Entity Framework, definindo as propriedades de navegação, sem fazer referência à `CourseInstructor` explicitamente de tabela.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Adicionando uma entidade com uma relação a uma entidade existente

Criar uma nova página da web denominado *CoursesAdd.aspx* que usa o *Master* página mestra e adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Essa marcação cria um `EntityDataSource` controle que seleciona cursos, que permite a inserção, e que especifica um manipulador para o `Inserting` eventos. Você usará o manipulador para atualizar o `Department` propriedade de navegação quando um novo `Course` entidade é criada.

A marcação também cria uma `DetailsView` controle a ser usado para adicionar novos `Course` entidades. A marcação usa campos associados para `Course` propriedades da entidade. Você precisará inserir o `CourseID` valor porque isso não é um campo de ID gerada pelo sistema. Em vez disso, ele é um número de curso que deve ser especificado manualmente, quando o curso é criado.

Você usa um campo de modelo para o `Department` propriedade de navegação porque as propriedades de navegação não podem ser usadas com `BoundField` controles. O campo de modelo fornece uma lista suspensa para selecionar o departamento. A lista suspensa é associada à `Departments` definida por meio de entidade `Eval` em vez de `Bind`, novamente porque você não pode diretamente associar propriedades de navegação para atualizá-los. Especifique um manipulador para o `DropDownList` do controle `Init` evento para que você possa armazenar uma referência para o controle para uso pelo código que atualiza o `DepartmentID` chave estrangeira.

Na *CoursesAdd.aspx.cs* logo após a declaração de classe parcial, adicione um campo de classe para conter uma referência para o `DepartmentsDropDownList` controle:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Adicionar um manipulador para o `DepartmentsDropDownList` do controle `Init` eventos para que você possa armazenar uma referência ao controle. Isso permite que você obtenha o valor que o usuário inseriu e usá-lo para atualizar o `DepartmentID` valor da `Course` entidade.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Adicionar um manipulador para o `DetailsView` do controle `Inserting` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Quando o usuário clica `Insert`, o `Inserting` evento é gerado antes que o novo registro é inserido. Obtém o código no manipulador do `DepartmentID` do `DropDownList` controlar e usa-o para definir o valor que será usado para o `DepartmentID` propriedade do `Course` entidade.

O Entity Framework cuidará de adicionar este curso para o `Courses` propriedade de navegação de associado `Department` entidade. Ele também adiciona o departamento para o `Department` propriedade de navegação do `Course` entidade.

Execute a página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Insira uma ID, um título, um número de créditos e selecione um departamento, clique **inserir**.

Execute o *Courses.aspx* página e, em seguida, selecione o mesmo departamento para ver o novo curso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Trabalhando com relações muitos-para-muitos

A relação entre o `Courses` conjunto de entidades e o `People` conjunto de entidades é uma relação muitos-para-muitos. Um `Course` entidade tem uma propriedade de navegação nomeada `People` que pode conter zero, um ou mais relacionadas `Person` entidades (representando instrutores atribuídos para ensinar curso). E um `Person` entidade tem uma propriedade de navegação nomeada `Courses` que pode conter zero, um ou mais relacionadas `Course` entidades (representando cursos instrutor é atribuído a ensinar). Um instrutor pode ministrar vários cursos e um curso pode ser ministrado por vários instrutores. Nesta seção do passo a passo, você irá adicionar e remover relações entre `Person` e `Course` entidades pela atualização das propriedades de navegação de entidades relacionadas.

Criar uma nova página da web denominado *InstructorsCourses.aspx* que usa o *Master* página mestra e adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Essa marcação cria um `EntityDataSource` que recupera o nome do controle e `PersonID` de `Person` entidades para os instrutores. Um `DropDrownList` controle está vinculado a `EntityDataSource` controle. O `DropDownList` controle especifica um manipulador para o `DataBound` eventos. Você usará esse manipulador para listas suspensas databind os dois que exibem os cursos.

A marcação também cria o seguinte grupo de controles a serem usados para atribuir um curso para o instrutor selecionado:

- Um `DropDownList` controle para selecionar um curso para atribuir. Esse controle será preenchido com os cursos que atualmente não estão atribuídos ao instrutor selecionado.
- Um `Button` controle para iniciar a atribuição.
- Um `Label` controle para exibir uma mensagem de erro se a atribuição falhará.

Por fim, a marcação também cria um grupo de controles a ser usado para remover um curso do instrutor selecionado.

Na *InstructorsCourses.aspx.cs*, adicionar uma instrução:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Adicione um método para preencher as duas listas suspensos que exibem cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Esse código obtém todos os cursos do `Courses` entidade definida e obtém os cursos do `Courses` propriedade de navegação do `Person` entidade para o instrutor selecionado. Em seguida, ele determina quais cursos são atribuídos ao instrutor e preenche as listas suspensas de acordo.

Adicionar um manipulador para o `Assign` do botão `Click` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Esse código obtém os `Person` obtém de entidade para o instrutor selecionado, o `Course` entidade para o curso selecionado e adiciona o curso selecionado para o `Courses` propriedade de navegação do instrutor `Person` entidade. Em seguida, ele salva as alterações no banco de dados e preenche as listas suspensas para que os resultados podem ser vistos imediatamente.

Adicionar um manipulador para o `Remove` do botão `Click` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Esse código obtém os `Person` obtém de entidade para o instrutor selecionado, o `Course` entidade para o curso selecionado e remove o curso selecionado do `Person` da entidade `Courses` propriedade de navegação. Em seguida, ele salva as alterações no banco de dados e preenche as listas suspensas para que os resultados podem ser vistos imediatamente.

Adicione código para o `Page_Load` método que garante que as mensagens de erro não são visíveis quando não houver nenhum erro de relatório e adicionar manipuladores para o `DataBound` e `SelectedIndexChanged` eventos da lista suspensa de instrutores para preencher as listas suspensas de cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Execute a página.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Selecione um instrutor. O <strong>atribuir um curso</strong> lista suspensa exibe os cursos que o instrutor não ensina, e o <strong>remover um curso</strong> lista suspensa exibe os cursos que o instrutor já está atribuído a. No <strong>atribuir um curso</strong> seção, selecione um curso e, em seguida, clique em <strong>atribuir</strong>. O curso se move para o <strong>remover um curso</strong> lista suspensa. Selecione um curso na <strong>remover um curso</strong> seção e clique em <strong>remover</strong><em>.</em> O curso se move para o <strong>atribuir um curso</strong> lista suspensa.

Agora você já viu alguns mais maneiras de trabalhar com dados relacionados. O tutorial a seguir, você aprenderá como usar a herança no modelo de dados para melhorar a facilidade de manutenção do seu aplicativo.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-6.md)
