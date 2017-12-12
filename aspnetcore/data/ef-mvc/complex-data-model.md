---
title: "Núcleo do ASP.NET MVC com núcleo EF – modelo de dados - 5 de 10"
author: tdykstra
description: "Neste tutorial, você adiciona mais entidades e relações e personalizar o modelo de dados especificando a formatação, validação e regras de mapeamento de banco de dados."
keywords: "Anotações de dados do ASP.NET Core, Entity Framework Core,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: cacb23441e5f5ab06c6be27f3068276f21ff4ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>Criar um modelo de dados complexos - Core EF com o tutorial do MVC do ASP.NET Core (5 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

Os tutoriais anteriores, você trabalhou com um modelo de dados simples que foi composto de três entidades. Neste tutorial, você adicionará mais entidades e relações e você personalizará o modelo de dados especificando a formatação, validação e regras de mapeamento de banco de dados.

Quando você terminar, as classes de entidade se tornará o modelo de dados completo é mostrado na ilustração a seguir:

![Diagrama de entidade](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizar o modelo de dados por meio de atributos

Nesta seção, você verá como personalizar o modelo de dados por meio de atributos que especificam a formatação, validação e regras de mapeamento de banco de dados. Em seguida, em várias seções a seguir, você criará o modelo de dados de escola completo com a adição de atributos para as classes você já criou e criar novas classes para os demais tipos de entidade no modelo.

### <a name="the-datatype-attribute"></a>O atributo de tipo de dados

Para datas de registro do aluno, todas as páginas da web atualmente exibem o tempo junto com a data, embora tudo o que você gosta para esse campo é a data. Usando atributos de anotação de dados, você pode fazer uma alteração que corrige o formato de exibição em cada modo de exibição que mostra os dados de código. Para ver um exemplo de como fazer o que, você adicionará um atributo para o `EnrollmentDate` propriedade o `Student` classe.

Em *Models/Student.cs*, adicione um `using` instrução para o `System.ComponentModel.DataAnnotations` namespace e adicionar `DataType` e `DisplayFormat` atributos para o `EnrollmentDate` propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

O atributo `DataType` é usado para especificar um tipo de dados mais específico do que o tipo intrínseco de banco de dados. Nesse caso, apenas desejamos controlar a data, não a data e hora. O `DataType` enumeração fornece para muitos tipos de dados, como data, hora, PhoneNumber, moeda, endereço de email e muito mais. O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo. Por exemplo, um link `mailto:` pode ser criado para `DataType.EmailAddress` e um seletor de data pode ser fornecido para `DataType.Date` em navegadores que dão suporte a HTML5. O `DataType` atributo emite HTML 5 `data-` atributos (pronunciado dados dash) que podem compreender navegadores HTML 5. O `DataType` atributos não fornecem nenhuma validação.

`DataType.Date` não especifica o formato da data exibida. Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base em CultureInfo do servidor.

O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

A configuração `ApplyFormatInEditMode` especifica que a formatação também deve ser aplicada quando o valor é exibido em uma caixa de texto para edição. (Você talvez não queira que alguns campos — por exemplo, para valores de moeda, não convém o símbolo de moeda na caixa de texto para edição.)

Você pode usar o `DisplayFormat` atributo por si mesmo, mas geralmente é uma boa ideia usar a `DataType` atributo também. O `DataType` atributo transmite a semântica dos dados em vez de como renderizá-lo em uma tela e fornece os seguintes benefícios que você não obtém com `DisplayFormat`:

* O navegador pode habilitar os recursos do HTML5 (por exemplo mostrar um controle de calendário, o símbolo de moeda local apropriado, links de email, alguns cliente entrada validação, etc.).

* Por padrão, o navegador renderizará os dados usando o formato correto de acordo com a localidade.

Para obter mais informações, consulte o [ \<entrada > documentação do auxiliar de marca](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Executar o aplicativo, vá para a página de índice de alunos e observe que vezes não são exibidos para as datas de registro. O mesmo será verdadeiro para qualquer modo de exibição que usa o modelo de Student.

![Página de índice de alunos mostrando datas sem vezes](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>O atributo StringLength

Você também pode especificar regras de validação de dados e mensagens de erro de validação usando atributos. O `StringLength` atributo define o comprimento máximo do banco de dados e fornece do lado do cliente e do lado do servidor validação para o ASP.NET MVC. Você também pode especificar o comprimento mínimo da cadeia de caracteres neste atributo, mas o valor mínimo não tem nenhum impacto sobre o esquema de banco de dados.

Suponha que você deseja garantir que os usuários não insiram mais de 50 caracteres para um nome. Para adicionar essa limitação, adicione `StringLength` atributos para o `LastName` e `FirstMidName` propriedades, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

O `StringLength` atributo não impedem que um usuário inserir espaços em branco para um nome. Você pode usar o `RegularExpression` atributo aplicar restrições para a entrada. Por exemplo, o código a seguir exige que o primeiro caractere a ser maiuscula e os caracteres restantes para estar em ordem alfabética:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

O `MaxLength` atributo fornece funcionalidade semelhante para o `StringLength` de atributo, mas não fornece do lado do cliente a validação.

Agora, o modelo de banco de dados foi alterado de forma que requer uma alteração no esquema de banco de dados. Você usará as migrações para atualizar o esquema sem perda dos dados que são adicionados ao banco de dados usando o interface do usuário do aplicativo.

Salve suas alterações e compilar o projeto. Em seguida, abra a janela de comando na pasta do projeto e digite os seguintes comandos:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

O `migrations add` comando avisa que pode ocorrer perda de dados, como a alteração torna o mais curto para duas colunas de comprimento máximo.  Migrações cria um arquivo chamado  *\<timeStamp > _MaxLengthOnNames.cs*. Este arquivo contém o código de `Up` método que atualizará o banco de dados para coincidir com o modelo de dados atual. O `database update` esse código de execução do comando.

O prefixo ao nome do arquivo de migrações de carimbo de hora é usado pelo Entity Framework para ordenar as migrações. Você pode criar várias migrações antes de executar o comando de atualização de banco de dados e, em seguida, todas as migrações são aplicadas na ordem em que eles foram criados.

Executar o aplicativo, selecione o **alunos** , clique em **criar novo**e digite um nome com mais de 50 caracteres. Quando você clica em **criar**, validação do lado do cliente mostra uma mensagem de erro.

![Os alunos mostrando erros de comprimento de cadeia de caracteres de página de índice](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>O atributo de coluna

Você também pode usar atributos para controlar como as classes e propriedades são mapeadas para o banco de dados. Suponha que você tenha usado o nome `FirstMidName` para o nome do primeiro campo porque o campo também pode conter um nome do meio. Mas a coluna do banco de dados seja nomeada como `FirstName`, pois os usuários que serão gravado consultas ad hoc no banco de dados estão acostumados a esse nome. Para tornar esse mapeamento, você pode usar o `Column` atributo.

O `Column` atributo especifica que quando o banco de dados é criado, a coluna do `Student` tabela que mapeia para o `FirstMidName` propriedade será nomeada `FirstName`. Em outras palavras, quando seu código se refere a `Student.FirstMidName`, os dados virão ou atualizados no `FirstName` coluna do `Student` tabela. Se você não especificar nomes de coluna, eles recebem o mesmo nome que o nome da propriedade.

No *Student.cs* de arquivo, adicione uma `using` instrução para `System.ComponentModel.DataAnnotations.Schema` e adicione o atributo de nome de coluna para o `FirstMidName` propriedade, como mostra o seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

A adição do `Column` atributo altera o backup do modelo de `SchoolContext`, portanto, ele não coincidir com o banco de dados.

Salve suas alterações e compilar o projeto. Em seguida, abra a janela de comando na pasta do projeto e digite os seguintes comandos para criar outra migração:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

Em **Pesquisador de objetos do SQL Server**, abra o designer de tabela do aluno clicando duas vezes o **aluno** tabela.

![Tabela de alunos em SSOX após a migração](complex-data-model/_static/ssox-after-migration.png)

Antes de você aplicou as duas primeiras migrações, as colunas de nome forem do tipo nvarchar (max). Agora são nvarchar (50) e o nome da coluna foi alterado de FirstMidName para FirstName.

> [!Note]
> Se você tentar compilar antes de concluir a criação de todas as classes de entidade nas seções a seguir, você poderá receber erros de compilador.

## <a name="final-changes-to-the-student-entity"></a>Alterações finais para a entidade do aluno

![Entidade do aluno](complex-data-model/_static/student-entity.png)

Em *Models/Student.cs*, substitua o código que você adicionou anteriormente com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>O atributo necessário

O `Required` atributo faz com que os campos obrigatórios de propriedades de nome. O `Required` atributo não é necessária para tipos não anuláveis, como tipos de valor (DateTime, int, clique duas vezes, float, etc.). Tipos que não podem ser nulos automaticamente são tratados como campos obrigatórios.

Você pode remover o `Required` de atributos e substituí-lo com um parâmetro de comprimento mínimo para o `StringLength` atributo:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>O atributo de exibição

O `Display` atributo especifica que a legenda para as caixas de texto deve ser "Nome", "Sobrenome", "Nome completo" e "Data de registro" em vez do nome de propriedade em cada instância (que não tem espaço dividindo as palavras).

### <a name="the-fullname-calculated-property"></a>A propriedade FullName calculada

`FullName`é uma propriedade calculada que retorna um valor que é criado pela concatenação de duas outras propriedades. Portanto, ele tem um acessador get e não `FullName` coluna será gerada no banco de dados.

## <a name="create-the-instructor-entity"></a>Criar a entidade do instrutor

![Entidade instrutor](complex-data-model/_static/instructor-entity.png)

Criar *Models/Instructor.cs*, substituindo o código de modelo com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Observe que várias propriedades são os mesmos nas entidades Student e instrutor. No [implementando herança](inheritance.md) tutorial posteriormente nesta série, você vai refatorar esse código para eliminar a redundância.

Você pode colocar vários atributos em uma linha, portanto, você também pode escrever o `HireDate` atributos da seguinte maneira:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>As propriedades de navegação CourseAssignments e OfficeAssignment

O `CourseAssignments` e `OfficeAssignment` propriedades são propriedades de navegação.

Um instrutor pode ensinar a qualquer número de cursos, portanto `CourseAssignments` é definido como uma coleção.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Se uma propriedade de navegação pode conter várias entidades, seu tipo deve ser uma lista na qual as entradas podem ser adicionadas, excluídas e atualizadas.  Você pode especificar `ICollection<T>` ou um tipo, como `List<T>` ou `HashSet<T>`. Se você especificar `ICollection<T>`, EF cria um `HashSet<T>` coleção por padrão.

O motivo por que esses são `CourseAssignment` entidades são explicados abaixo na seção sobre relações muitos-para-muitos.

As regras de negócio Contoso Universidade de estado que instrutor só pode ter no máximo um escritório, portanto, o `OfficeAssignment` propriedade contém uma única entidade OfficeAssignment (que pode ser null se o office não está atribuído).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Criar a entidade OfficeAssignment

![Entidade OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Criar *Models/OfficeAssignment.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>O atributo de chave

Há uma relação um-para-zero-ou-um entre as entidades de OfficeAssignment e o instrutor. Uma atribuição de office só existe em relação ao instrutor a que é atribuído e, portanto, sua chave primária também for sua chave estrangeira para a entidade do instrutor. Mas o Entity Framework não pode reconhecer InstructorID como a chave primária dessa entidade porque seu nome não segue a convenção de nomenclatura de ID ou classnameID. Portanto, o `Key` atributo é usado para identificá-lo como a chave:

```csharp
[Key]
public int InstructorID { get; set; }
```

Você também pode usar o `Key` atributo se a entidade tem sua própria chave primária, mas você deseja atribuir um nome de propriedade que não seja classnameID ou ID.

Por padrão, o EF trata a chave como não gerado pelo banco de dados porque a coluna é uma relação de identificação.

### <a name="the-instructor-navigation-property"></a>A propriedade de navegação do instrutor

A entidade do instrutor tem um valor nulo `OfficeAssignment` propriedade de navegação (porque um instrutor pode não ter uma atribuição de office), e a entidade OfficeAssignment tem uma não-anulável `Instructor` propriedade de navegação (porque não é uma atribuição do office existir sem um instrutor – `InstructorID` é não anulável). Quando uma entidade do instrutor tem uma entidade relacionada de OfficeAssignment, cada entidade tem uma referência a um em sua propriedade de navegação.

Você pode colocar um `[Required]` atributo na propriedade de navegação instrutor para especificar que deve haver um instrutor relacionado, mas você não precisa fazer isso porque o `InstructorID` chave estrangeira (que também é a chave para esta tabela) é não anulável.

## <a name="modify-the-course-entity"></a>Modificar a entidade de curso

![Entidade de curso](complex-data-model/_static/course-entity.png)

Em *Models/Course.cs*, substitua o código que você adicionou anteriormente com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

A entidade de curso tem uma propriedade de chave estrangeira `DepartmentID` que aponta para a entidade relacionada do departamento e ele tem um `Department` propriedade de navegação.

O Entity Framework não exigir que você adicionar uma propriedade de chave estrangeira para o modelo de dados quando você tem uma propriedade de navegação para uma entidade relacionada.  EF cria chaves estrangeiras no banco de dados sempre que eles são necessários e cria automaticamente [propriedades de sombra](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para eles. Mas, com a chave estrangeira no modelo de dados pode fazer atualizações mais simples e mais eficiente. Por exemplo, quando você busca uma entidade de curso para editar, a entidade de departamento é null se não fazê-lo, então quando você atualiza a entidade de curso, você precisaria primeiro obter a entidade de departamento. Quando a propriedade de chave estrangeira `DepartmentID` está incluído no modelo de dados, você não precisa buscar à entidade Department antes de atualizar.

### <a name="the-databasegenerated-attribute"></a>O atributo DatabaseGenerated

O `DatabaseGenerated` atributo com o `None` parâmetro o `CourseID` propriedade especifica se valores de chave primária são fornecidos pelo usuário em vez de gerado pelo banco de dados.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Por padrão, o Entity Framework pressupõe que os valores de chave primária são gerados pelo banco de dados. É o que você quer na maioria dos cenários. No entanto, para entidades do curso, você usa um número especificado pelo usuário como uma série de 1000 de um departamento, uma série de 2000 para outro departamento e assim por diante.

O `DatabaseGenerated` atributo também pode ser usado para gerar valores padrão, como no caso de colunas de banco de dados usadas para registrar a data de uma linha foi criada ou atualizada.  Para obter mais informações, consulte [propriedades gerado](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Propriedades de navegação e de chave estrangeira

As propriedades de chave estrangeira e propriedades de navegação na entidade curso refletem as seguintes relações:

Um curso é atribuído a um departamento, para que haja um `DepartmentID` chave estrangeira e um `Department` propriedade de navegação pelas razões mencionadas acima.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Um curso pode ter qualquer número de alunos inscritos, portanto, o `Enrollments` propriedade de navegação é uma coleção:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Um curso pode ser ensinado por vários instrutores, portanto, o `CourseAssignments` propriedade de navegação é uma coleção (o tipo `CourseAssignment` é explicado [posteriormente](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Criar a entidade de departamento

![Entidade Department](complex-data-model/_static/department-entity.png)


Criar *Models/Department.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>O atributo de coluna

Anteriormente, você usou o `Column` atributo para alterar o mapeamento de nome de coluna. No código para a entidade de departamento, a `Column` atributo está sendo usado para alterar SQL dados mapeamento de tipo para que a coluna será definida usando o tipo de dinheiro do SQL Server no banco de dados:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapeamento de coluna geralmente não é necessário, pois o Entity Framework escolhe o tipo de dados do SQL Server apropriado com base no tipo CLR que você definir para a propriedade. O CLR `decimal` tipo é mapeado para um SQL Server `decimal` tipo. Mas, nesse caso, você sabe que a coluna será manter os valores de moeda e o tipo de dados money é mais apropriado para que.

### <a name="foreign-key-and-navigation-properties"></a>Propriedades de navegação e de chave estrangeira

As propriedades de navegação e chave estrangeiras refletem as relações a seguir:

Um departamento pode ou não ter um administrador, e um administrador é sempre um instrutor. Portanto o `InstructorID` propriedade está incluída como a chave estrangeira à entidade instrutor e um ponto de interrogação é adicionado após o `int` designação para marcar a propriedade como um valor nulo de tipo. A propriedade de navegação é denominada `Administrator` , mas contém uma entidade do instrutor:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Um departamento pode ter vários cursos, portanto, há uma propriedade de navegação de cursos:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Por convenção, o Entity Framework permite a exclusão em cascata para chaves estrangeiras não anuláveis em relações muitos-para-muitos. Isso pode resultar em regras de exclusão em cascata circular, que fará com que uma exceção ao tentar adicionar uma migração. Por exemplo, se você não definir a propriedade Department.InstructorID como anuláveis, EF poderia configurar uma regra de exclusão em cascata para excluir o instrutor quando você excluir o departamento, que é não o que você deseja que aconteça. Se as regras de negócio é necessário o `InstructorID` propriedade a ser não anuláveis, você precisa usar a seguinte instrução API fluente para desabilitar a exclusão em cascata na relação:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Modificar a entidade de registro

![Entidade de registro](complex-data-model/_static/enrollment-entity.png)

Em *Models/Enrollment.cs*, substitua o código que você adicionou anteriormente com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Propriedades de navegação e de chave estrangeira

As propriedades de navegação e propriedades de chave estrangeira refletem as relações a seguir:

Um registro é para um único curso, portanto, há um `CourseID` propriedade de chave estrangeira e um `Course` propriedade de navegação:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Um registro é para um aluno único, portanto, há um `StudentID` propriedade de chave estrangeira e um `Student` propriedade de navegação:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relações muitos-para-muitos

Há uma relação muitos-para-muitos entre as entidades Student e curso e a entidade de registro funciona como uma tabela de junção de muitos-para-muitos *com carga* no banco de dados. "Com carga" significa que a tabela de registro contém dados adicionais além de chaves estrangeiras de tabelas unidas (no caso, uma chave primária e uma propriedade de classificação).

A ilustração a seguir mostra a aparência dessas relações em um diagrama de entidade. (Esse diagrama foi gerado usando o Entity Framework Power Tools para EF 6. x; criar o diagrama não faz parte do tutorial, ele está sendo usado apenas como uma ilustração.)

![Curso de aluno muitos para muitos](complex-data-model/_static/student-course.png)

Cada linha de relação tem um 1 em uma extremidade e um asterisco (*) em outro, indicando uma relação um-para-muitos.

Se a tabela de registro não incluir informações de classificação, ele só precisa conter as duas chaves estrangeiras CourseID e StudentID. Nesse caso, seria uma tabela de junção de muitos-para-muitos sem carga (ou uma tabela de junção puro) no banco de dados. As entidades de curso e instrutor têm esse tipo de relação muitos-para-muitos, e a próxima etapa é criar uma classe de entidade para funcionar como uma tabela de junção sem carga.

(EF 6. x oferece suporte a tabelas de junção implícita para relações muitos-para-muitos, mas Core EF não. Para obter mais informações, consulte o [discussão no repositório do GitHub de núcleo EF](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>A entidade CourseAssignment

![Entidade CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Criar *Models/CourseAssignment.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Associar nomes de entidade

Uma tabela de junção é necessária no banco de dados para a relação de muitos-para-muitos instrutor para cursos, e ele deve ser representado por um conjunto de entidades. É comum para nomear uma entidade de junção `EntityName1EntityName2`, que nesse caso seria `CourseInstructor`. No entanto, recomendamos que você escolha um nome que descreve a relação. Modelos de dados começam simples e crescem, com junções de carga não obtendo frequentemente cargas mais tarde. Se você iniciar com um nome descritivo de entidade, você não precisará alterar o nome posteriormente. Idealmente, a entidade de junção terá seu próprio nome (possivelmente única palavra) natural no domínio da empresa. Por exemplo, manuais e clientes podem ser vinculados por meio de classificações. Para essa relação, `CourseAssignment` é uma escolha melhor do que `CourseInstructor`.

### <a name="composite-key"></a>Chave composta

Como as chaves estrangeiras não são anuláveis e juntos exclusivamente identificar cada linha da tabela, não é necessário para uma chave primária. O *InstructorID* e *CourseID* propriedades devem funcionar como uma chave primária composta. É a única maneira de identificar as chaves primárias compostas ao EF usando o *API fluente* (ele não pode ser feito por meio de atributos). Você verá como configurar a chave primária composta na próxima seção.

A chave composta garante que, embora você possa ter várias linhas para um curso e várias linhas para um instrutor, não pode ter várias linhas para o mesmo instrutor e um curso. O `Enrollment` junção entidade define sua própria chave primária, portanto, duplicatas desse tipo são possíveis. Para evitar essas duplicatas, você pode adicionar um índice exclusivo nos campos de chave estrangeiras ou configurar `Enrollment` com uma chave primária composta semelhante ao `CourseAssignment`. Para obter mais informações, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Atualizar o contexto do banco de dados

Adicione o seguinte código realçado para o *Data/SchoolContext.cs* arquivo:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Esse código adiciona novas entidades e configura a chave primária composta de CourseAssignment da entidade.

## <a name="fluent-api-alternative-to-attributes"></a>Alternativa de API fluente para atributos

O código de `OnModelCreating` método do `DbContext` classe usa a *API fluente* para configurar o comportamento EF. A API é chamada "fluente" porque ele geralmente é usado pelo encadeamento uma série de chamadas de método juntas em uma única instrução, como neste exemplo da [documentação principal do EF](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

Neste tutorial, você está usando a API fluente somente para mapeamento de banco de dados que você não pode fazer com os atributos. No entanto, você também pode usar a API fluente para especificar a maioria da formatação, validação e regras de mapeamento que você pode fazer por meio de atributos. Alguns atributos como `MinimumLength` não pode ser aplicado com a API fluente. Conforme mencionado anteriormente, `MinimumLength` não altera o esquema, ela só se aplica a uma regra de validação do cliente e servidor.

Alguns desenvolvedores preferem usar a API fluente exclusivamente para que eles podem manter suas classes de entidade "normal". Você pode misturar atributos e API fluente se desejar, e há algumas personalizações que só podem ser feitas usando a API fluente, mas em geral a prática recomendada é escolher uma destas duas abordagens e usar ou consistentemente tanto quanto possível. Se você usar ambos, observe que sempre que houver um conflito, a API fluente substitui atributos.

Para obter mais informações sobre atributos versus API fluente, consulte [métodos de configuração](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Relações de exibição de diagrama de entidade

A ilustração a seguir mostra o diagrama que cria o Entity Framework Power Tools para o modelo de escola concluído.

![Diagrama de entidade](complex-data-model/_static/diagram.png)

Além de linhas de relação um-para-muitos (1 a \*), você pode ver a linha de relação um-para-zero-ou-um (1 para entre 0 e 1) entre as entidades instrutor e OfficeAssignment e a linha de relação de zero-ou-um-para-muitos (entre 0 e 1 para *) entre o Entidades instrutor e departamento.

## <a name="seed-the-database-with-test-data"></a>Preencha o banco de dados de teste

Substitua o código no *Data/DbInitializer.cs* arquivo com o código a seguir para fornecer dados de propagação de novas entidades que você criou.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Como você viu no primeiro tutorial, a maioria do código simplesmente cria novos objetos de entidade e carrega dados de exemplo em propriedades conforme necessário para teste. Observe como as relações muitos-para-muitos são tratadas: o código cria relações Criando entidades a `Enrollments` e `CourseAssignment` ingressar em conjuntos de entidades.

## <a name="add-a-migration"></a>Adicionar uma migração

Salve suas alterações e compilar o projeto. Em seguida, abra a janela de comando na pasta do projeto e insira o `migrations add` comando (não fazer o comando de atualização de banco de dados ainda):

```console
dotnet ef migrations add ComplexDataModel
```

Você receber um aviso sobre a possível perda de dados.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Se você tentou executar o `database update` neste ponto de comando (não faça isso ainda), você deverá obter o seguinte erro:

> A instrução ALTER TABLE em conflito com a restrição de chave estrangeira "FK_dbo. Course_dbo. Department_DepartmentID". O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Departamento", coluna 'DepartmentID'.

Às vezes, quando você executar migrações com dados existentes, você precisa inserir dados stub para o banco de dados para atender as restrições de chave estrangeira. O código gerado o `Up` método adiciona uma chave estrangeira de DepartmentID não anulável para a tabela de curso. Se já houver linhas na tabela Course quando o código é executado, o `AddColumn` Falha na operação porque o SQL Server não sabe qual valor para colocar na coluna que não pode ser nula. Para este tutorial, você executará a migração no novo banco de dados, mas em um aplicativo de produção você precisa fazer a migração de manipular os dados existentes, portanto as instruções a seguir mostram um exemplo de como fazer isso.

Para fazer a migração trabalhar com dados existentes, que você precisa alterar o código para fornecer a nova coluna de um valor padrão e criar um departamento de stub chamado "Temp" para agir como o departamento de padrão. Como resultado, existente linhas curso serão todas relacionadas ao departamento "Temp" após o `Up` execuções do método.

* Abra o *{timestamp}_ComplexDataModel.cs* arquivo. 

* Comentar a linha de código que adiciona a coluna de ID do departamento na tabela de curso.

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Adicione o seguinte código após o código que cria a tabela de departamento:

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Em um aplicativo de produção, você escreve código ou scripts para adicionar linhas de departamento e relacionar linhas curso para as novas linhas de departamento. Você deve, em seguida, não precisar mais do departamento de "Temp" ou o valor padrão na coluna Course.DepartmentID.

Salve suas alterações e compilar o projeto.

## <a name="change-the-connection-string-and-update-the-database"></a>Altere a cadeia de caracteres de conexão e atualize o banco de dados

Agora você tem novo código `DbInitializer` classe que adiciona dados de propagação de novas entidades para um banco de dados vazio. Para fazer com que o EF criar um novo banco de dados vazio, altere o nome do banco de dados na cadeia de conexão *appSettings. JSON* ContosoUniversity3 ou algum outro nome que você ainda não usado no computador que você está usando.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Salvar as alterações para *appSettings. JSON*.

> [!NOTE]
> Como alternativa para alterar o nome do banco de dados, você pode excluir o banco de dados. Use **Pesquisador de objetos do SQL Server** (SSOX) ou o `database drop` comando CLI:
> ```console
> dotnet ef database drop
> ```

Depois que você tiver alterado o nome do banco de dados ou excluir o banco de dados, execute o `database update` comando na janela de comando para executar as migrações.

```console
dotnet ef database update
```

Executar o aplicativo para fazer com que o `DbInitializer.Initialize` método para executar e popular o novo banco de dados.

Abra o banco de dados em SSOX como você fez anteriormente e expanda o **tabelas** nó para ver se todas as tabelas foram criadas. (Se você ainda tiver SSOX abrir da hora anterior, clique o **atualização** botão.)

![Tabelas no SSOX](complex-data-model/_static/ssox-tables.png)

Execute o aplicativo para acionar o código de inicializador que propaga o banco de dados.

Clique com botão direito do **CourseAssignment** de tabela e selecione **exibir dados** para verificar se ele tem dados nele.

![Dados CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Resumo

Agora você tem um modelo de dados mais complexo e o banco de dados correspondente. O tutorial a seguir, você aprenderá mais sobre como acessar os dados relacionados.

>[!div class="step-by-step"]
[Anterior](migrations.md)
[Próximo](read-related-data.md)  
