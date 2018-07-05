---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introdução ao banco de dados do Entity Framework 4.0 First e 4 do ASP.NET Web Forms - parte 6 | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 99861197e00a3f2f6811ef13136fac63b993ef32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372100"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introdução ao banco de dados do Entity Framework 4.0 First e 4 do Web Forms do ASP.NET - parte 6
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial na série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementando a herança de tabela por hierarquia

No tutorial anterior você trabalhou com os dados relacionados, adicionando e excluindo relações e adicionando uma nova entidade que tinha uma relação com uma entidade existente. Este tutorial mostrará como implementar a herança no modelo de dados.

Na programação orientada a objeto, você pode usar a herança para torná-lo mais fácil trabalhar com classes relacionadas. Por exemplo, você pode criar `Instructor` e `Student` classes que derivam de um `Person` classe base. Você pode criar os mesmos tipos de estruturas de herança entre as entidades no Entity Framework.

Nesta parte do tutorial, você não crie novas páginas da web. Em vez disso, você adicionará derivado de entidades para o modelo de dados e modificar páginas existentes para usar as novas entidades.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela por hierarquia versus a herança de tabela por tipo

Um banco de dados pode armazenar informações sobre objetos relacionados em uma tabela ou em várias tabelas. Por exemplo, o `School` banco de dados, o `Person` tabela inclui informações sobre os alunos e instrutores em uma única tabela. Algumas das colunas se aplicam somente a instrutores (`HireDate`), algumas somente a alunos (`EnrollmentDate`) e alguns para ambos (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Você pode configurar o Entity Framework para criar `Instructor` e `Student` entidades que herdam o `Person` entidade. Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados individual é chamado *tabela por hierarquia* herança (TPH).

Para cursos, o `School` banco de dados usa um padrão diferente. Cursos online e cursos no local são armazenados em tabelas separadas, cada qual com uma chave estrangeira que aponta para o `Course` tabela. Informações comuns a ambos os tipos de curso são armazenadas somente no `Course` tabela.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Você pode configurar o modelo de dados do Entity Framework, de modo que `OnlineCourse` e `OnsiteCourse` entidades herdam o `Course` entidade. Esse padrão de geração de uma estrutura de herança de entidade de tabelas separadas para cada tipo, com cada tabela separada, referência de volta a uma tabela que armazena dados comuns a todos os tipos, é chamado *tabela por tipo* herança (TPT).

Padrões de herança TPH oferecem melhor desempenho geralmente no Entity Framework que os padrões de herança TPT, porque os padrões TPT podem resultar em consultas de junção complexas. Este passo a passo demonstra como implementar a herança TPH. Você terá de fazer isso executando as seguintes etapas:

- Crie `Instructor` e `Student` tipos de entidade que derivam de `Person`.
- Mover as propriedades pertinentes para as entidades derivadas do `Person` entidade para as entidades derivadas.
- Definir restrições em propriedades em tipos derivados.
- Verifique o `Person` entidade uma entidade abstrata.
- Derivado de mapa de cada entidade para o `Person` tabela com uma condição que especifica como determinar se um `Person` linha representa o tipo derivado.

## <a name="adding-instructor-and-student-entities"></a>Adicionando entidades de aluno e instrutor

Abra o <em>Schoolmodel</em> de arquivos, clique com botão direito em designer, selecione uma área desocupada <strong>Add</strong>, em seguida, selecione <strong>entidade</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

No **Adicionar entidade** caixa de diálogo, o nome da entidade `Instructor` e defina seu **tipo de Base** opção `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Clique em **OK**. O designer cria um `Instructor` entidade deriva o `Person` entidade. A nova entidade ainda não tem nenhuma propriedade.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Repita o procedimento para criar uma `Student` entidade também deriva de `Person`.

Somente os instrutores têm datas de contratação, portanto, você precisará mover essa propriedade do `Person` entidade para o `Instructor` entidade. No `Person` entidade, clique com botão direito do `HireDate` propriedade e clique em **Recortar**. Em seguida, clique com botão direito **propriedades** na `Instructor` entidade e clique em **colar**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

A data de admissão um `Instructor` entidade não pode ser nula. Com o botão direito a `HireDate` propriedade, clique em **propriedades**e, em seguida, no **as propriedades** alteração janela `Nullable` para `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Repita o procedimento para mover o `EnrollmentDate` propriedade do `Person` entidade para o `Student` entidade. Certifique-se de que você também defina `Nullable` à `False` para o `EnrollmentDate` propriedade.

Agora que um `Person` entidade tem apenas as propriedades que são comuns a `Instructor` e `Student` entidades (além das propriedades de navegação, que você não estiver em movimento), a entidade só pode ser usada como uma entidade base na estrutura de herança. Portanto, você precisa garantir que ele nunca será tratado como uma entidade independente. Com o botão direito o `Person` entidade, selecione **propriedades**e, em seguida, no **propriedades** janela alterar o valor da **abstrata** propriedade  **True**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapeamento de entidades de aluno e instrutor para a tabela Person

Agora, você precisa informar ao Entity Framework como diferenciar `Instructor` e `Student` entidades no banco de dados.

Clique com botão direito do `Instructor` entidade e selecione **mapeamento de tabela**. No **Mapping Details** janela, clique em **adicionar uma tabela ou exibição** e selecione **pessoa**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Clique em **adicionar uma condição**e, em seguida, selecione **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Alteração **operador** à **é** e **valor / propriedade** para **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Repita o procedimento para o `Students` entidade, especificando que essa entidade é mapeada para o `Person` tabela quando o `EnrollmentDate` coluna não é nula. Em seguida, salve e feche o modelo de dados.

Compile o projeto para criar novas entidades como classes e disponibilizá-las no designer.

## <a name="using-the-instructor-and-student-entities"></a>Usando a entidades Student e Instructor

Quando você criou as páginas da web que funcionam com dados de aluno e instrutor, ligação de dados você-los para o `Person` do conjunto de entidades e filtrado em de `HireDate` ou `EnrollmentDate` propriedade para restringir os dados retornados aos alunos ou instrutores. No entanto, agora quando você associa cada controle de fonte de dados para o `Person` do conjunto de entidades, você pode especificar que somente `Student` ou `Instructor` tipos de entidade devem ser selecionados. Como o Entity Framework sabe como diferenciar os alunos e instrutores na `Person` conjunto de entidades, você pode remover o `Where` configurações de propriedade que você inseriu manualmente para fazer isso.

No Designer do Visual Studio, você pode especificar tipo de entidade que um `EntityDataSource` controle deve selecionar na **EntityTypeFilter** caixa de lista suspensa do `Configure Data Source` assistente, conforme mostrado no exemplo a seguir.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

E, na **propriedades** janela você pode remover `Where` valores de cláusula que não são mais necessários, conforme mostrado no exemplo a seguir.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

No entanto, porque você alterou a marcação para `EntityDataSource` controles para usar o `ContextTypeName` atributo, não é possível executar o **configurar fonte de dados** assistente em `EntityDataSource` controles que você já criou. Portanto, você fará as alterações necessárias, alterando a marcação em vez disso.

Abra o *Students.aspx* página. No `StudentsEntityDataSource` controlar, remova o `Where` do atributo e adicione um `EntityTypeFilter="Student"` atributo. A marcação agora será semelhante ao exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Definindo o `EntityTypeFilter` atributo garante que o `EntityDataSource` controle selecionará apenas o tipo de entidade especificada. Se você quiser recuperar ambos `Student` e `Instructor` tipos de entidade, não defina esse atributo. (Você tem a opção de recuperação de vários tipos de entidade com um `EntityDataSource` controlam somente se você estiver usando o controle de acesso a dados somente leitura. Se você estiver usando um `EntityDataSource` controlar para inserir, atualizar ou excluir entidades e se o conjunto de entidades que ele está vinculado ao pode conter vários tipos, você só poderá trabalhar com o tipo de uma entidade, e você deve definir esse atributo.)

Repita o procedimento para o `SearchEntityDataSource` controlar, exceto remover somente a parte do `Where` atributo seleciona `Student` entidades em vez de remover a propriedade completamente. A marca de abertura do controle agora será semelhante ao exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Execute a página para verificar se ele ainda funciona como antes.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Atualizar as páginas a seguir que você criou nos tutoriais anteriores, para que eles usem a nova `Student` e `Instructor` entidades em vez de `Person` entidades, em seguida, execute-os para verificar se eles funcionam como antes:

- Na *StudentsAdd.aspx*, adicione `EntityTypeFilter="Student"` para o `StudentsEntityDataSource` controle. A marcação agora será semelhante ao exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Na *About*, adicione `EntityTypeFilter="Student"` para o `StudentStatisticsEntityDataSource` controlar e remover `Where="it.EnrollmentDate is not null"`. A marcação agora será semelhante ao exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Na *Instructors.aspx* e *InstructorsCourses.aspx*, adicione `EntityTypeFilter="Instructor"` para o `InstructorsEntityDataSource` controlar e remover `Where="it.HireDate is not null"`. A marcação na *Instructors.aspx* agora se parece com o exemplo a seguir: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    A marcação na *InstructorsCourses.aspx* agora será parecida com o exemplo a seguir:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Como resultado dessas alterações, aprimoramos o facilidade de manutenção do aplicativo Contoso University de várias maneiras. Você moveu a lógica de seleção e validação de fora a camada de interface do usuário (*. aspx* marcação) e fez parte integrante da camada de acesso a dados. Isso ajuda a isolar o código do aplicativo contra alterações que você pode fazer no futuro para o esquema de banco de dados ou o modelo de dados. Por exemplo, você poderia decidir que os alunos podem ser contratados como auxílios dos professores e, portanto, obteria uma data de contratação. Em seguida, você poderia adicionar uma nova propriedade para diferenciar os alunos de instrutores e atualizar o modelo de dados. Nenhum código no aplicativo web precisaria alterar, exceto onde você deseja mostrar uma data de contratação para alunos. Outro benefício da adição `Instructor` e `Student` entidades é que seu código é mais prontamente compreensível que quando ele chamado `Person` objetos que eram, na verdade, estudantes ou instrutores.

Agora você já viu uma maneira de implementar um padrão de herança no Entity Framework. O tutorial a seguir, você aprenderá como usar procedimentos armazenados para ter mais controle sobre como o Entity Framework acessa o banco de dados.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-7.md)
