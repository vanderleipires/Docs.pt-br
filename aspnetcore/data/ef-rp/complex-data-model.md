---
title: "Páginas Razor com núcleo EF – modelo de dados - 5 de 8"
author: rick-anderson
description: "Neste tutorial, você adiciona mais entidades e relações e personalizar o modelo de dados especificando a formatação, validação e regras de mapeamento de banco de dados."
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c375fe6ea98c621012eb55589c8b174c2a95b697
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>Criar um modelo de dados complexos - Core EF com tutorial páginas Razor (5 de 8)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Os tutoriais anteriores trabalharam com um modelo de dados básico composto de três entidades. Neste tutorial:

* Mais de entidades e relações são adicionadas.
* O modelo de dados é personalizado com a especificação de formatação, validação e regras de mapeamento de banco de dados.

As classes de entidade para o modelo de dados completo é mostrado na ilustração a seguir:

![Diagrama de entidade](complex-data-model/_static/diagram.png)

Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Personalizar o modelo de dados com atributos

Nesta seção, o modelo de dados personalizado usando atributos.

### <a name="the-datatype-attribute"></a>O atributo de tipo de dados

As páginas do aluno atualmente exibe a hora da data de registro. Normalmente, os campos de data mostram apenas a data e não o tempo.

Atualização *Models/Student.cs* com o seguinte realçado código:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

O [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atributo especifica um tipo de dados que é mais específico que o tipo de banco de dados intrínseco. Neste caso, que apenas a data deve ser exibida, não a data e hora. O [enumeração DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fornece para muitos tipos de dados, como data, hora, PhoneNumber, moeda, endereço de email, etc. O `DataType` atributo também pode habilitar o aplicativo fornecer automaticamente os recursos de um tipo específico. Por exemplo:

* O `mailto:` link é criado automaticamente para `DataType.EmailAddress`.
* O seletor de data é fornecido para `DataType.Date` na maioria dos navegadores.

O `DataType` atributo emite HTML 5 `data-` atributos (pronunciado dados dash) que consomem os navegadores HTML 5. O `DataType` atributos não fornecem validação.

`DataType.Date` não especifica o formato da data exibida. Por padrão, o campo de data será exibido conforme os formatos padrão com base no servidor de [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

O `ApplyFormatInEditMode` configuração especifica que a formatação também deve ser aplicada para a edição da interface do usuário. Alguns campos não devem usar `ApplyFormatInEditMode`. Por exemplo, o símbolo de moeda deve geralmente não ser exibido em uma caixa de texto de edição.

O `DisplayFormat` atributo pode ser usado por si só. Geralmente, é recomendável usar o `DataType` atributo com o `DisplayFormat` atributo. O `DataType` atributo transmite a semântica dos dados em vez de como renderizá-lo em uma tela. O `DataType` atributo fornece os seguintes benefícios que não estão disponíveis em `DisplayFormat`:

* O navegador pode habilitar recursos do HTML5. Por exemplo, mostra um controle de calendário, o símbolo de moeda local apropriado, links de email, validação de entrada do lado do cliente, etc.
* Por padrão, o navegador renderiza dados usando o formato correto com base na localidade.

Para obter mais informações, consulte o [ \<entrada > documentação do auxiliar de marca](xref:mvc/views/working-with-forms#the-input-tag-helper).

Execute o aplicativo. Navegue até a página de índice de alunos. Vezes não são mais exibidos. Cada modo de exibição que usa o `Student` modelo exibe a data sem tempo.

![Página de índice de alunos mostrando datas sem vezes](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>O atributo StringLength

Regras de validação de dados e mensagens de erro de validação podem ser especificadas com atributos. O [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atributo especifica o comprimento mínimo e máximo de caracteres que são permitidos em um campo de dados. O `StringLength` atributo também fornece a validação do lado do cliente e do servidor. O valor mínimo não tem impacto sobre o esquema de banco de dados.

Atualização de `Student` modelo com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

O código anterior limita os nomes a não mais que 50 caracteres. O `StringLength` atributo não impede que um usuário inserir espaços em branco para um nome. O [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atributo é usado para aplicar restrições para a entrada. Por exemplo, o código a seguir exige que o primeiro caractere a ser maiuscula e os caracteres restantes para estar em ordem alfabética:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Execute o aplicativo:

* Navegue até a página de alunos.
* Selecione **criar novo**e digite um nome mais de 50 caracteres.
* Selecione **criar**, validação do lado do cliente mostra uma mensagem de erro.

![Os alunos mostrando erros de comprimento de cadeia de caracteres de página de índice](complex-data-model/_static/string-length-errors.png)

Em **Pesquisador de objetos do SQL Server** (SSOX), abra o designer de tabela do aluno clicando duas vezes o **aluno** tabela.

![Tabela de alunos em SSOX antes da migração](complex-data-model/_static/ssox-before-migration.png)

A imagem anterior mostra o esquema para o `Student` tabela. Os campos nome tem tipo `nvarchar(MAX)` porque migrações não tiver sido executada no banco de dados. Quando as migrações são executadas em breve neste tutorial, os campos de nome se tornam `nvarchar(50)`.

### <a name="the-column-attribute"></a>O atributo de coluna

Atributos podem controlar como as classes e propriedades são mapeadas para o banco de dados. Nesta seção, o `Column` atributo é usado para mapear o nome do `FirstMidName` propriedade como "FirstName" no banco de dados.

Quando o banco de dados é criado, os nomes de propriedade no modelo são usados para nomes de coluna (exceto quando o `Column` atributo é usado).

O `Student` modelo usa `FirstMidName` para o nome do primeiro campo porque o campo também pode conter um nome do meio.

Atualização de *Student.cs* arquivo com o seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Com a alteração anterior, `Student.FirstMidName` no aplicativo mapeia para o `FirstName` coluna o `Student` tabela.

A adição do `Column` atributo altera o backup do modelo de `SchoolContext`. O backup do modelo de `SchoolContext` não coincide mais com o banco de dados. Se o aplicativo é executado antes de aplicar as migrações, a seguinte exceção será gerada:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Para atualizar o banco de dados:

* Compile o projeto.
* Abra uma janela de comando na pasta do projeto. Digite os comandos a seguir para criar uma nova migração e atualizar o banco de dados:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

O `dotnet ef migrations add ColumnFirstName` comando gera a seguinte mensagem de aviso:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

O aviso é gerado porque os campos de nome agora estão limitado a 50 caracteres. Se um nome de banco de dados tiver mais de 50 caracteres, 51 último caractere seriam perdidos.

* Teste o aplicativo.

Abra a tabela de alunos em SSOX:

![Tabela de alunos em SSOX após a migração](complex-data-model/_static/ssox-after-migration.png)

Antes da migração foi aplicada, as colunas de nome forem do tipo [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). As colunas de nome agora são `nvarchar(50)`. O nome da coluna foi alterado de `FirstMidName` para `FirstName`.

> [!Note]
> Na seção a seguir, criar o aplicativo em alguns estágios gera erros de compilador. As instruções especifiquem quando compilar o aplicativo.

## <a name="student-entity-update"></a>Atualização da entidade aluno

![Entidade do aluno](complex-data-model/_static/student-entity.png)

Atualização *Models/Student.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>O atributo necessário

O `Required` atributo faz com que os campos obrigatórios de propriedades de nome. O `Required` atributo não é necessária para tipos não anuláveis, como tipos de valor (`DateTime`, `int`, `double`, etc.). Tipos que não podem ser nulos automaticamente são tratados como campos obrigatórios.

O `Required` atributo pode ser substituído com um parâmetro de comprimento mínimo de `StringLength` atributo:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>O atributo de exibição

O `Display` atributo especifica que a legenda para as caixas de texto deve ser "Nome", "Sobrenome", "Nome completo" e "Data de inscrição". As legendas padrão não tinham dividindo as palavras, por exemplo, "Sobrenome".

### <a name="the-fullname-calculated-property"></a>A propriedade FullName calculada

`FullName`é uma propriedade calculada que retorna um valor que é criado pela concatenação de duas outras propriedades. `FullName`não pode ser definida, que ela tem um acessador get. Não `FullName` coluna é criada no banco de dados.

## <a name="create-the-instructor-entity"></a>Criar a entidade do instrutor

![Entidade instrutor](complex-data-model/_static/instructor-entity.png)

Criar *Models/Instructor.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Observe que várias propriedades são os mesmos no `Student` e `Instructor` entidades. No tutorial implementando herança posteriormente na série, esse código é refatorado para eliminar a redundância.

Vários atributos podem estar em uma linha. O `HireDate` atributos podem ser escritos da seguinte maneira:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>As propriedades de navegação CourseAssignments e OfficeAssignment

O `CourseAssignments` e `OfficeAssignment` propriedades são propriedades de navegação.

Um instrutor pode ensinar a qualquer número de cursos, portanto `CourseAssignments` é definido como uma coleção.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Se uma propriedade de navegação contém várias entidades:

* Ele deve ser um tipo de lista onde as entradas podem ser adicionadas, excluídas e atualizadas.

Tipos de propriedade de navegação:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Se `ICollection<T>` for especificado, Core EF cria um `HashSet<T>` coleção por padrão.

O `CourseAssignment` entidade é explicada na seção em relações muitos-para-muitos.

Estado que instrutor pode ter no máximo um escritório de regras de negócio de Contoso University. O `OfficeAssignment` propriedade contém um único `OfficeAssignment` entidade. `OfficeAssignment`será null se o office não está atribuído.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Criar a entidade OfficeAssignment

![Entidade OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Criar *Models/OfficeAssignment.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>O atributo de chave

O `[Key]` atributo é usado para identificar uma propriedade como a chave primária (PK) quando o nome da propriedade é algo diferente de classnameID ou ID.

Há uma relação um-para-zero-ou-um entre o `Instructor` e `OfficeAssignment` entidades. Uma atribuição de office existe apenas em relação ao instrutor a que é atribuído. O `OfficeAssignment` PK também é a chave estrangeira (FK) para o `Instructor` entidade. EF principal não pode reconhecer `InstructorID` como PK de `OfficeAssignment` porque:

* `InstructorID`não siga a convenção de nomenclatura de ID ou classnameID.

Portanto, o `Key` atributo é usado para identificar `InstructorID` como o PK:

```csharp
[Key]
public int InstructorID { get; set; }
```

Por padrão, o núcleo de EF trata a chave como não gerado pelo banco de dados porque a coluna é uma relação de identificação.

### <a name="the-instructor-navigation-property"></a>A propriedade de navegação do instrutor

O `OfficeAssignment` propriedade de navegação para o `Instructor` entidade é anulável porque:

* Tipos de referência (como classes são nulas).
* Um instrutor pode não ter uma atribuição de escritório.


O `OfficeAssignment` entidade tem uma não-anulável `Instructor` propriedade de navegação porque:

* `InstructorID`é não anulável.
* Uma atribuição de office não pode existir sem um instrutor.

Quando um `Instructor` entidade tem um relacionados `OfficeAssignment` entidade, cada entidade tem uma referência para um em sua propriedade de navegação.

O `[Required]` atributo pode ser aplicado para o `Instructor` propriedade de navegação:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

O código anterior Especifica que deve ser um instrutor relacionado. O código anterior é desnecessário porque o `InstructorID` chave estrangeira (que é também o PK) é não anulável.

## <a name="modify-the-course-entity"></a>Modificar a entidade de curso

![Entidade de curso](complex-data-model/_static/course-entity.png)

Atualização *Models/Course.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

O `Course` entidade tem uma propriedade de chave estrangeira (FK) `DepartmentID`. `DepartmentID`aponta para relacionado `Department` entidade. O `Course` entidade tem um `Department` propriedade de navegação.

Núcleo EF não requer uma propriedade FK para um modelo de dados quando o modelo tem uma propriedade de navegação para uma entidade relacionada.

EF Core cria automaticamente FKs no banco de dados sempre que forem necessários. Cria Core EF [propriedades de sombra](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para FKs criados automaticamente. Ter FK no modelo de dados pode fazer atualizações mais simples e mais eficiente. Por exemplo, considere um modelo em que a propriedade FK `DepartmentID` é *não* incluídos. Quando uma entidade de curso é buscada para editar:

* O `Department` entidade será null se ele não foi explicitamente é carregado.
* Para atualizar a entidade de curso, o `Department` primeiro deve ser buscada de entidade.

Quando a propriedade FK `DepartmentID` está incluído no modelo de dados, não é necessário para buscar o `Department` entidade antes de uma atualização.

### <a name="the-databasegenerated-attribute"></a>O atributo DatabaseGenerated

O `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atributo especifica que o PK é fornecida pelo aplicativo em vez de gerado pelo banco de dados.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Por padrão, o EF Core assume que os valores de PK são gerados pelo banco de dados. Banco de dados gerado PK valores geralmente é a melhor abordagem. Para `Course` entidades, o usuário Especifica o PK. Por exemplo, um número de curso como uma série de 1000 para o departamento de matemática, uma série de 2000 para o departamento em inglês.

O `DatabaseGenerated` atributo também pode ser usado para gerar valores padrão. Por exemplo, o banco de dados pode gerar automaticamente um campo de data para registrar a data em que uma linha foi criada ou atualizada. Para obter mais informações, consulte [propriedades gerado](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Propriedades de navegação e de chave estrangeira

As propriedades de chave estrangeira (FK) e propriedades de navegação no `Course` entidade refletem as relações a seguir:

Um curso é atribuído a um departamento, para que haja um `DepartmentID` FK e um `Department` propriedade de navegação.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Um curso pode ter qualquer número de alunos inscritos, portanto, o `Enrollments` propriedade de navegação é uma coleção:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Um curso pode ser ensinado por vários instrutores, portanto, o `CourseAssignments` propriedade de navegação é uma coleção:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`é explicado [posteriormente](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Criar a entidade de departamento

![Entidade Department](complex-data-model/_static/department-entity.png)

Criar *Models/Department.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>O atributo de coluna

Anteriormente a `Column` atributo foi usado para alterar o mapeamento de nome de coluna. No código para o `Department` entidade, o `Column` atributo é usado para alterar o mapeamento de tipo de dados SQL. O `Budget` coluna é definida usando o tipo de dinheiro do SQL Server no banco de dados:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapeamento de coluna, geralmente não é necessário. Núcleo EF geralmente escolhe o tipo de dados do SQL Server apropriado com base no tipo CLR para a propriedade. O CLR `decimal` tipo é mapeado para um SQL Server `decimal` tipo. `Budget`para moeda, e o tipo de dados money é mais apropriado para moeda.

### <a name="foreign-key-and-navigation-properties"></a>Propriedades de navegação e de chave estrangeira

As propriedades de navegação e FK refletem as relações a seguir:

* Um departamento pode ou não ter um administrador.
* Um administrador é sempre um instrutor. Portanto, o `InstructorID` propriedade está incluída como FK para o `Instructor` entidade.

A propriedade de navegação é denominada `Administrator` , mas contém um `Instructor` entidade:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

O ponto de interrogação (?) no código anterior Especifica que a propriedade é anulável.

Um departamento pode ter vários cursos, portanto, há uma propriedade de navegação de cursos:

```csharp
public ICollection<Course> Courses { get; set; }
```

Observação: Por convenção, o núcleo do EF permite a exclusão em cascata para FKs não anuláveis em relações muitos-para-muitos. Exclusão em cascata pode resultar em regras de exclusão em cascata circular. Circular exclusão em cascata causas regras uma exceção quando uma migração é adicionada.

Por exemplo, se o `Department.InstructorID` propriedade não foi definida como anulável:

* Núcleo EF configura uma regra de exclusão em cascata para excluir o instrutor quando o departamento é excluído.
* Excluir o instrutor quando o departamento é excluído não é o comportamento desejado.

Se as regras de negócio é necessário o `InstructorID` propriedade ser não anuláveis, use a seguinte instrução API fluente:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

O código anterior desabilita a exclusão em cascata na relação instrutor departamento.

## <a name="update-the-enrollment-entity"></a>Atualizar a entidade de registro

Um registro é um curso uma tomada por um aluno.

![Entidade de registro](complex-data-model/_static/enrollment-entity.png)

Atualização *Models/Enrollment.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Propriedades de navegação e de chave estrangeira

As propriedades FK e propriedades de navegação refletem as relações a seguir:

Um registro é um curso, portanto, há um `CourseID` propriedade FK e um `Course` propriedade de navegação:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Um registro é para um aluno, portanto, há um `StudentID` propriedade FK e um `Student` propriedade de navegação:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relações muitos-para-muitos

Há uma relação muitos-para-muitos entre o `Student` e `Course` entidades. O `Enrollment` entidade funciona como uma tabela de junção de muitos-para-muitos *com carga* no banco de dados. "Com carga" significa que o `Enrollment` tabela contém dados adicionais além FKs para tabelas unidas (nesse caso, a CP e `Grade`).

A ilustração a seguir mostra a aparência dessas relações em um diagrama de entidade. (Esse diagrama foi gerado usando EF Power Tools para EF 6. x. Criar o diagrama não faz parte do tutorial.)

![Curso de aluno muitos para muitos](complex-data-model/_static/student-course.png)

Cada linha de relação tem um 1 em uma extremidade e um asterisco (*) em outro, indicando uma relação um-para-muitos.

Se o `Enrollment` tabela não incluir informações de classificação, ele apenas precisa conter os dois FKs (`CourseID` e `StudentID`). Uma tabela de junção de muitos-para-muitos sem carga às vezes é chamada de uma tabela de junção puro (PJT).

O `Instructor` e `Course` as entidades têm uma relação muitos-para-muitos usando uma tabela de junção puro.

Observação: EF 6. x oferece suporte a tabelas de junção implícita para relações muitos-para-muitos, mas Core EF não. Para obter mais informações, consulte [muitos-para-muitos relações no EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>A entidade CourseAssignment

![Entidade CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Criar *Models/CourseAssignment.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Instrutor para cursos

![M:M instrutor para cursos](complex-data-model/_static/courseassignment.png)

A relação de muitos-para-muitos instrutor para cursos:

* Requer uma tabela de junção que deve ser representada por um conjunto de entidades.
* É uma tabela de junção puro (tabela sem carga).

É comum para nomear uma entidade de junção `EntityName1EntityName2`. Por exemplo, a tabela de junção instrutor para cursos usando esse padrão é `CourseInstructor`. No entanto, é recomendável usar um nome que descreve a relação.

Modelos de dados começam simples e crescem. Nenhuma carga junções (PJTs) evoluem com frequência para incluir a carga. Iniciando com um nome descritivo de entidade, o nome não precisa alterar quando a tabela de junção é alterado. Idealmente, a entidade de junção terá seu próprio nome (possivelmente única palavra) natural no domínio da empresa. Por exemplo, manuais e clientes foi vinculados com uma entidade de associação chamada classificações. Para a relação de muitos-para-muitos instrutor para cursos, `CourseAssignment` é preferencial em `CourseInstructor`.

### <a name="composite-key"></a>Chave composta

FKs não são nulas. Os dois FKs em `CourseAssignment` (`InstructorID` e `CourseID`) juntas identificam exclusivamente cada linha do `CourseAssignment` tabela. `CourseAssignment`não requer um PK. dedicado O `InstructorID` e `CourseID` propriedades funcionam como um PK de composição. A única maneira de especificar PKs compostos Core EF é com o *API fluente*. A próxima seção mostra como configurar o PK de composição.

A chave composta garante:

* São permitidas várias linhas para um curso.
* São permitidas várias linhas para um instrutor.
* Várias linhas para o mesmo instrutor e curso não é permitido.

O `Enrollment` junção entidade define seu próprio PK, portanto, duplicatas desse tipo são possíveis. Para evitar tais duplicatas:

* Adicionar um índice exclusivo nos campos de FK, ou
* Configurar `Enrollment` com uma chave primária composta semelhante ao `CourseAssignment`. Para obter mais informações, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Atualizar o contexto do banco de dados

Adicione o seguinte código realçado para *Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

O código anterior adiciona novas entidades e configura o `CourseAssignment` PK. de composição da entidade

## <a name="fluent-api-alternative-to-attributes"></a>Alternativa de API fluente para atributos

O `OnModelCreating` método anterior de código usa o *API fluente* para configurar o comportamento de EF Core. A API é chamada "fluente" porque ele geralmente é usado pelo encadeamento uma série de chamadas de método em uma única instrução. O [código a seguir](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) é um exemplo de como a API fluente:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Neste tutorial, a API fluente é usada apenas para mapeamento de banco de dados que não pode ser feito com atributos. No entanto, a API fluente pode especificar mais de formatação, validação e regras de mapeamento que podem ser feitas com os atributos.

Alguns atributos como `MinimumLength` não pode ser aplicado com a API fluente. `MinimumLength`não altera o esquema, ela só se aplica a uma regra de validação de comprimento mínimo.

Alguns desenvolvedores preferem usar a API fluente exclusivamente para que eles podem manter suas classes de entidade "normal". Atributos e a API fluente podem ser mesclados. Há algumas configurações que só podem ser feitas com a API fluente (especificando uma CP composto). Há algumas configurações que só podem ser feitas com os atributos (`MinimumLength`). A prática recomendada para o uso de atributos ou API fluente:

* Escolha uma destas duas abordagens.
* Use a abordagem escolhida consistentemente tanto quanto possível.

Alguns dos atributos usados neste tutorial são usados para:

* Somente validação (por exemplo, `MinimumLength`).
* Apenas para a configuração de núcleo EF (por exemplo, `HasKey`).
* Configuração de validação e EF Core (por exemplo, `[StringLength(50)]`).

Para obter mais informações sobre atributos versus API fluente, consulte [métodos de configuração](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Relações de exibição de diagrama de entidade

A ilustração a seguir mostra o diagrama que criar EF Power Tools para o modelo de escola concluído.

![Diagrama de entidade](complex-data-model/_static/diagram.png)

O diagrama anterior mostra:

* Várias linhas de relação um-para-muitos (1 a \*).
* A linha de relação um-para-zero-ou-um (1 para entre 0 e 1) entre o `Instructor` e `OfficeAssignment` entidades.
* A linha de relação de zero-ou-um-para-muitos (entre 0 e 1 para *) entre o `Instructor` e `Department` entidades.

## <a name="seed-the-db-with-test-data"></a>O banco de dados com dados de teste de propagação

Atualize o código em *Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

O código anterior fornece dados de propagação de novas entidades. A maioria do código cria novos objetos de entidade e carrega dados de exemplo. Os dados de exemplo são usados para teste. O código anterior cria as seguintes relações muitos-para-muitos:

* `Enrollments`
* `CourseAssignment`

Observação: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) dará suporte [propagação de dados](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Adicionar uma migração

Compile o projeto. Abra uma janela de comando na pasta do projeto e digite o seguinte comando:

```console
dotnet ef migrations add ComplexDataModel
```

O comando anterior exibe um aviso sobre a possível perda de dados.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Se o `database update` comando é executado, o seguinte erro é gerado:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Quando as migrações são executadas com os dados existentes, pode haver restrições FK que não são atendidas com os dados existentes. Para este tutorial, um novo banco de dados é criado, portanto, não há nenhuma violação de restrição FK. Consulte [corrigir restrições de chave estrangeira com dados herdados](#fk) para obter instruções sobre como corrigir as violações de FK no banco de dados atual.

## <a name="change-the-connection-string-and-update-the-db"></a>Altere a cadeia de caracteres de conexão e atualize o banco de dados

O código atualizado `DbInitializer` adiciona dados de propagação de novas entidades. Para forçar o EF principal para criar um novo banco de dados vazio:

* Alterar o nome de cadeia de caracteres de conexão de banco de dados no *appSettings. JSON* para ContosoUniversity3. O novo nome deve ser um nome que ainda não foi usado no computador.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* Como alternativa, exclua o banco de dados usando:

    * **Pesquisador de objetos do SQL Server** (SSOX).
    * O `database drop` comando CLI:

   ```console
   dotnet ef database drop
   ```

Executar `database update` na janela de comando:

```console
dotnet ef database update
```

O comando anterior é executado todas as migrações.

Execute o aplicativo. Executando o aplicativo será executado o `DbInitializer.Initialize` método. O `DbInitializer.Initialize` preenche o novo banco de dados.

Abra o banco de dados SSOX:

* Expanda o **tabelas** nó. As tabelas criadas são exibidas.
* Se SSOX foi aberto anteriormente, clique o **atualização** botão.

![Tabelas no SSOX](complex-data-model/_static/ssox-tables.png)

Examine o **CourseAssignment** tabela:

* Clique com botão direito do **CourseAssignment** de tabela e selecione **exibir dados**.
* Verifique se o **CourseAssignment** tabela contiver dados.

![Dados CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Correção de restrições de chave estrangeira com dados herdados

Esta seção é opcional.

Quando as migrações são executadas com os dados existentes, pode haver restrições FK que não são atendidas com os dados existentes. Com dados de produção, as etapas devem ser executadas para migrar os dados existentes. Esta seção fornece um exemplo de correção de violações de restrição FK. Não fazer essas alterações de código sem um backup. Não fazer essas alterações de código se você concluir a seção anterior e atualizar o banco de dados.

O *{timestamp}_ComplexDataModel.cs* arquivo contém o código a seguir:

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

O código anterior adiciona uma não-anulável `DepartmentID` FK para o `Course` tabela. O banco de dados do tutorial anterior contém linhas de `Course`, portanto, essa tabela não pode ser atualizada por migrações.

Para fazer o `ComplexDataModel` trabalho de migração com dados existentes:

* Altere o código para dar a nova coluna (`DepartmentID`) um valor padrão.
* Crie um departamento falso chamado "Temp" para agir como o departamento de padrão.

### <a name="fix-the-foreign-key-constraints"></a>Corrija as restrições de chave estrangeiras

Atualização de `ComplexDataModel` classes `Up` método:

* Abra o *{timestamp}_ComplexDataModel.cs* arquivo.
* Comente a linha de código que adiciona o `DepartmentID` coluna para o `Course` tabela.

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Adicione o seguinte código realçado. O novo código é após o `.CreateTable( name: "Department"` bloco:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Com as alterações anteriores, existente `Course` linhas estão relacionadas ao departamento "Temp" após o `ComplexDataModel` `Up` execuções do método.

Um aplicativo de produção seria:

* Incluir código ou scripts para adicionar `Department` linhas e relacionadas `Course` linhas para o novo `Department` linhas.
* Não use o departamento de "Temp" ou o valor padrão para `Course.DepartmentID `.

O seguinte tutorial abrange os dados relacionados.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/migrations)
[Próximo](xref:data/ef-rp/read-related-data)
