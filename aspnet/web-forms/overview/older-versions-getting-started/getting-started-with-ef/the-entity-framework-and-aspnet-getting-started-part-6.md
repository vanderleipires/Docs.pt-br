---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 6 | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888793"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 6
====================
por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementando a herança de tabela por hierarquia

O tutorial anterior você trabalhou com dados relacionados, adicionando e excluindo relações e adicionando uma nova entidade que tenha uma relação com uma entidade existente. Este tutorial mostrará como implementar a herança no modelo de dados.

Em programação orientada a objeto, você pode usar a herança para tornar mais fácil trabalhar com classes relacionadas. Por exemplo, você pode criar `Instructor` e `Student` classes que derivam de um `Person` classe base. Você pode criar os mesmos tipos de estruturas de herança entre as entidades no Entity Framework.

Nesta parte do tutorial, você não crie novas páginas da web. Em vez disso, você adicionará derivado entidades para o modelo de dados e modificar páginas existentes para usar as novas entidades.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela por hierarquia e herança de tabela por tipo

Um banco de dados pode armazenar informações sobre objetos relacionados em uma tabela ou em várias tabelas. Por exemplo, o `School` banco de dados, o `Person` tabela inclui informações sobre os alunos e instrutores em uma única tabela. Algumas das colunas se aplicam somente a instrutores (`HireDate`), alguns somente para os alunos (`EnrollmentDate`) e outros (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Você pode configurar o Entity Framework para criar `Instructor` e `Student` entidades que herdam o `Person` entidade. Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados único é chamado *tabela por hierarquia* herança (TPH).

Para cursos, o `School` banco de dados usa um padrão diferente. Cursos online e cursos no local são armazenados em tabelas separadas, cada qual com uma chave estrangeira que aponta para o `Course` tabela. Informações comuns a ambos os tipos de curso são armazenadas apenas no `Course` tabela.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Você pode configurar o modelo de dados do Entity Framework para que `OnlineCourse` e `OnsiteCourse` entidades herdam o `Course` entidade. Esse padrão de geração de uma estrutura de herança de entidade de tabelas separadas para cada tipo, com cada tabela separada, referência de volta a uma tabela que armazena dados comuns a todos os tipos, é chamado *tabela por tipo* herança (TPT).

Padrões de herança TPH geralmente fornecem melhor desempenho no Entity Framework de padrões de herança TPT, como padrões TPT podem resultar em consultas de junção complexas. Este passo a passo demonstra como implementar a herança TPH. Você fará isso executando as seguintes etapas:

- Criar `Instructor` e `Student` tipos de entidade que derivam de `Person`.
- Propriedades de movimentação que pertencem às entidades derivadas do `Person` entidade para a entidade derivada.
- Definir restrições em propriedades em tipos derivados.
- Verifique o `Person` entidade uma entidade abstrata.
- Derivado de mapa de cada entidade para a `Person` tabela com uma condição que especifica como determinar se um `Person` linha representa o tipo derivado.

## <a name="adding-instructor-and-student-entities"></a>Adicionando instrutor e entidades de Student

Abra o <em>SchoolModel.edmx</em> de arquivos, clique em designer, selecione uma área desocupada <strong>adicionar</strong>, em seguida, selecione <strong>entidade</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

No **Adicionar entidade** caixa de diálogo, o nome da entidade `Instructor` e defina seu **tipo Base** opção para `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Clique em **OK**. O designer cria um `Instructor` entidade da qual deriva a `Person` entidade. A nova entidade ainda não tem nenhuma propriedade.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Repita o procedimento para criar um `Student` entidade também deriva de `Person`.

Somente instrutores têm datas de contratação, portanto você precisa mover essa propriedade do `Person` entidade para o `Instructor` entidade. No `Person` entidade, com o botão direito do `HireDate` propriedade e clique em **Recortar**. Em seguida, clique com botão direito **propriedades** no `Instructor` entidade e clique em **colar**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

A data de admissão um `Instructor` entidade não pode ser nula. Com o botão direito do `HireDate` propriedade, clique em **propriedades**e, em seguida, no **propriedades** alteração de janela `Nullable` para `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Repita o procedimento para mover o `EnrollmentDate` propriedade o `Person` entidade para o `Student` entidade. Certifique-se de que você também definir `Nullable` para `False` para o `EnrollmentDate` propriedade.

Agora que um `Person` entidade tem apenas as propriedades que são comuns a `Instructor` e `Student` entidades (além de propriedades de navegação, que você está movendo não), a entidade só pode ser usada como uma entidade base na estrutura de herança. Portanto, você precisa garantir que ele nunca será tratado como uma entidade independente. Com o botão direito a `Person` entidade, selecione **propriedades**e, em seguida, no **propriedades** janela alterar o valor da **abstrata** propriedade  **True**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapeando instrutor e entidades de estudante para a tabela Person

Agora você precisa informar ao Entity Framework como diferenciar `Instructor` e `Student` entidades no banco de dados.

Clique com botão direito do `Instructor` entidade e selecione **mapeamento de tabela**. No **detalhes de mapeamento** janela, clique em **adicionar uma tabela ou exibição** e selecione **pessoa**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Clique em **adicionar uma condição**e, em seguida, selecione **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Alterar **operador** para **é** e **valor / propriedade** para **não Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Repita o procedimento para o `Students` entidade, especificando que essa entidade é mapeada para o `Person` tabela quando o `EnrollmentDate` coluna não é nulo. Em seguida, salve e feche o modelo de dados.

Crie o projeto para criar novas entidades como classes e disponibilizá-los no designer.

## <a name="using-the-instructor-and-student-entities"></a>Usando o instrutor e entidades de Student

Quando você criou as páginas da web que funcionam com dados de student e instrutor, você ligação de dados para o `Person` do conjunto de entidades e filtrados no `HireDate` ou `EnrollmentDate` propriedade para restringir os dados retornados ao alunos ou professores. No entanto, agora quando você associa cada controle de fonte de dados para o `Person` conjunto de entidades, você pode especificar que apenas `Student` ou `Instructor` tipos de entidade devem ser selecionados. Como o Entity Framework sabe como diferenciar os alunos e instrutores no `Person` conjunto de entidades, você pode remover o `Where` as configurações de propriedade que você inseriu manualmente para fazer isso.

No Designer do Visual Studio, você pode especificar tipo de entidade um `EntityDataSource` selecione no controle o **EntityTypeFilter** caixa de lista suspensa do `Configure Data Source` assistente, conforme mostrado no exemplo a seguir.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

E, no **propriedades** janela você pode remover `Where` valores da cláusula que não são mais necessários, conforme mostrado no exemplo a seguir.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

No entanto, porque você alterou a marcação para `EntityDataSource` controles para utilizar o `ContextTypeName` atributo, não é possível executar o **configurar fonte de dados** assistente em `EntityDataSource` controles que você criou. Portanto, você fará as alterações necessárias, alterando a marcação em vez disso.

Abra o *Students.aspx* página. No `StudentsEntityDataSource` controlar, remova o `Where` de atributos e adicionar um `EntityTypeFilter="Student"` atributo. A marcação agora será parecida com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Definindo o `EntityTypeFilter` atributo garante que o `EntityDataSource` controle irá selecionar apenas o tipo de entidade especificado. Se você quiser recuperar ambos `Student` e `Instructor` tipos de entidade, não defina este atributo. (Você tem a opção de recuperação de vários tipos de entidade com um `EntityDataSource` controlar somente se você estiver usando o controle de acesso a dados somente leitura. Se você estiver usando um `EntityDataSource` controle para inserir, atualizar ou excluir entidades e se o conjunto de entidades que ele está vinculado ao pode conter vários tipos, você só pode trabalhar com o tipo de uma entidade, e você deve definir esse atributo.)

Repita o procedimento para o `SearchEntityDataSource` controlar, exceto remover somente a parte do `Where` atributo seleciona `Student` entidades em vez de remover a propriedade completamente. A marca de abertura do controle agora será parecida com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Execute a página para verificar se ele ainda funciona como antes.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Atualize as seguintes páginas que você criou nos tutoriais anteriores para que eles usam o novo `Student` e `Instructor` entidades em vez de `Person` entidades, em seguida, executá-los para verificar se eles funcionam como funcionavam antes:

- Em *StudentsAdd.aspx*, adicionar `EntityTypeFilter="Student"` para o `StudentsEntityDataSource` controle. A marcação agora será parecida com o exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Em *About*, adicionar `EntityTypeFilter="Student"` para o `StudentStatisticsEntityDataSource` controlar e remover `Where="it.EnrollmentDate is not null"`. A marcação agora será parecida com o exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Em *Instructors.aspx* e *InstructorsCourses.aspx*, adicionar `EntityTypeFilter="Instructor"` para o `InstructorsEntityDataSource` controlar e remover `Where="it.HireDate is not null"`. A marcação em *Instructors.aspx* agora se parece com o exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    A marcação em *InstructorsCourses.aspx* agora será parecida com o exemplo a seguir:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Como resultado dessas alterações, você já aprimorado facilidade de manutenção do aplicativo Contoso Universidade de várias maneiras. Você moveu a lógica de validação e seleção de fora da camada de interface do usuário (*. aspx* marcação) e uma parte integrante da camada de acesso a dados. Isso ajuda a isolar o código do aplicativo de alterações que você pode fazer no futuro para o esquema de banco de dados ou o modelo de dados. Por exemplo, você pode decidir que os alunos podem ser contratados como auxílios dos professores e, portanto, obterá uma data de contratação. Em seguida, você pode adicionar uma nova propriedade para diferenciar os alunos dos professores e atualizar o modelo de dados. Nenhum código no aplicativo web precisaria alterar exceto onde você deseja mostrar uma data de contratação para estudantes. Outro benefício de adição de `Instructor` e `Student` entidades é que seu código é mais prontamente entendido que quando ele chamado `Person` objetos que foram realmente alunos ou professores.

Você viu uma maneira de implementar um padrão de herança no Entity Framework. O tutorial a seguir, você aprenderá como usar procedimentos armazenados para ter mais controle sobre como o Entity Framework acessa o banco de dados.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-7.md)
