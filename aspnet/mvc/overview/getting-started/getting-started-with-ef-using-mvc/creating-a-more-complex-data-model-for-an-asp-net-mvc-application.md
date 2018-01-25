---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Criando um modelo de dados mais complexo para um aplicativo ASP.NET MVC | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9a89aa8e7dd3b2f6ac18e0b1a9c2a9d64d27189c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>Criando um modelo de dados mais complexo para um aplicativo ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Os tutoriais anteriores você trabalhou com um modelo de dados simples que foi composto de três entidades. Neste tutorial você adicionará mais entidades e relações, e você personalizará o modelo de dados especificando a formatação, validação e regras de mapeamento de banco de dados. Você verá duas maneiras de personalizar o modelo de dados: adicionando atributos a classes de entidade e adicionando o código para a classe de contexto de banco de dados.

Quando você terminar, as classes de entidade se tornará o modelo de dados completo é mostrado na ilustração a seguir:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizar o modelo de dados por meio de atributos

Nesta seção, você verá como personalizar o modelo de dados por meio de atributos que especificam a formatação, validação e regras de mapeamento de banco de dados. Em seguida, em várias seções a seguir, você criará completo `School` modelo de dados adicionando atributos para as classes já criados e criar novas classes para os tipos de entidade restantes no modelo.

### <a name="the-datatype-attribute"></a>O atributo de tipo de dados

Para datas de registro do aluno, todas as páginas da web atualmente exibem o tempo junto com a data, embora tudo o que você gosta para esse campo é a data. Usando atributos de anotação de dados, você pode fazer uma alteração que corrige o formato de exibição em cada modo de exibição que mostra os dados de código. Para ver um exemplo de como fazer o que, você adicionará um atributo para o `EnrollmentDate` propriedade o `Student` classe.

Em *Models\Student.cs*, adicione um `using` instrução para o `System.ComponentModel.DataAnnotations` namespace e adicionar `DataType` e `DisplayFormat` atributos para o `EnrollmentDate` propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

O [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo é usado para especificar um tipo de dados que é mais específico que o tipo de banco de dados intrínseco. Nesse caso, apenas desejamos controlar a data, não a data e hora. O [enumeração DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) oferece para muitos tipos de dados, tais como *data, hora, PhoneNumber, moeda, EmailAddress* e muito mais. O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo. Por exemplo, um `mailto:` link pode ser criado para [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), e um seletor de data pode ser fornecido para [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) em navegadores que oferecem suporte a [HTML5](http://html5.org/). O [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos emite HTML 5 [dados -](http://ejohn.org/blog/html-5-data-attributes/) (pronunciado *dash dados*) atributos HTML 5 navegadores podem entender. O [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos não fornecem nenhuma validação.

`DataType.Date` não especifica o formato da data exibida. Por padrão, o campo de dados será exibido conforme os formatos padrão com base no servidor de [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


O `ApplyFormatInEditMode` configuração especifica que a formatação especificada também deve ser aplicada quando o valor é exibido na caixa de texto para edição. (Talvez você não queira que alguns campos — por exemplo, para valores de moeda, não convém o símbolo de moeda na caixa de texto para edição.)

Você pode usar o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo por si mesmo, mas geralmente é uma boa ideia usar a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo também. O `DataType` atributo transmite o *semântica* dos dados como oposição para renderizá-lo em uma tela de instruções e fornece os seguintes benefícios que você não obtém com `DisplayFormat`:

- O navegador pode habilitar os recursos do HTML5 (por exemplo mostrar um controle de calendário, o símbolo de moeda local apropriado, links de email, alguns cliente entrada validação, etc.).
- Por padrão, o navegador processará os dados usando o formato correto com base em sua [localidade](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- O [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo pode habilitar MVC escolher o modelo de campo à direita para processar os dados (o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) usa o modelo de cadeia de caracteres). Para obter mais informações, consulte Brad Wilson [modelos do ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Embora escrito para MVC 2, este artigo ainda se aplica à versão atual do ASP.NET MVC.)

Se você usar o `DataType` atributo com um campo de data, você precisa especificar o `DisplayFormat` atributo também para garantir que o campo seja renderizado corretamente em navegadores Chrome. Para obter mais informações, consulte [esse thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Para obter mais informações sobre como lidar com outros formatos de data no MVC, vá para [MVC 5 Introdução: examinando os métodos de editar e editar exibição](../introduction/examining-the-edit-methods-and-edit-view.md) e pesquisa na página de &quot;internacionalização&quot;.

Execute a página de índice de estudante novamente e observe que vezes não são exibidos para as datas de registro. O mesmo será verdadeiro para qualquer modo de exibição que usa o `Student` modelo.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>O StringLengthAttribute

Você também pode especificar regras de validação de dados e mensagens de erro de validação usando atributos. O [StringLength atributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) define o comprimento máximo do banco de dados e fornece do lado do cliente e do lado do servidor validação para o ASP.NET MVC. Você também pode especificar o comprimento mínimo da cadeia de caracteres neste atributo, mas o valor mínimo não tem nenhum impacto sobre o esquema de banco de dados.

Suponha que você deseja garantir que os usuários não insiram mais de 50 caracteres para um nome. Para adicionar essa limitação, adicione [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributos para o `LastName` e `FirstMidName` propriedades, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

O [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo não impedem que um usuário inserir espaços em branco para um nome. Você pode usar o [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo aplicar restrições para a entrada. Por exemplo, o código a seguir exige o primeiro caractere a ser maiuscula e os caracteres restantes para estar em ordem alfabética:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

O [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atributo fornece funcionalidade semelhante para o [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) de atributo, mas não fornece do lado do cliente a validação.

Execute o aplicativo e clique no **alunos** guia. Você obterá o seguinte erro:

*O modelo fazendo o contexto de 'SchoolContext' foi alterado desde que o banco de dados foi criado. Considere o uso de migrações do Code First para atualizar o banco de dados ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

O modelo de banco de dados foi alterado de forma que requer uma alteração no esquema de banco de dados e do Entity Framework detectou que. Você usará as migrações para atualizar o esquema sem perda dos dados que você adicionou ao banco de dados usando a interface do usuário. Se você tiver alterado os dados que foi criados com o `Seed` método, que será alterado para seu estado original porque a [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) método que você está usando no `Seed` método. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) é equivalente a uma operação "upsert" da terminologia de banco de dados.)

No pacote Manager Console (PMC), digite os seguintes comandos:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

O `add-migration` comando cria um arquivo chamado  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Este arquivo contém o código de `Up` método que atualizará o banco de dados para coincidir com o modelo de dados atual. O `update-database` esse código de execução do comando.

O carimbo de hora acrescentado ao nome do arquivo migrações é usado pelo Entity Framework para ordenar as migrações. Você pode criar várias migrações antes de executar o `update-database` comando e, em seguida, todas as migrações são aplicadas na ordem em que eles foram criados.

Execute o **criar** página e, em seguida, digite um nome mais de 50 caracteres. Quando você clica em **criar**, validação do lado do cliente mostra uma mensagem de erro.

![Erro de val do lado do cliente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>O atributo de coluna

Você também pode usar atributos para controlar como as classes e propriedades são mapeadas para o banco de dados. Suponha que você tenha usado o nome `FirstMidName` para o nome do primeiro campo porque o campo também pode conter um nome do meio. Mas a coluna do banco de dados seja nomeada como `FirstName`, pois os usuários que serão gravado consultas ad hoc no banco de dados estão acostumados a esse nome. Para tornar esse mapeamento, você pode usar o `Column` atributo.

O `Column` atributo especifica que quando o banco de dados é criado, a coluna do `Student` tabela que mapeia para o `FirstMidName` propriedade será nomeada `FirstName`. Em outras palavras, quando seu código se refere a `Student.FirstMidName`, os dados virão ou atualizados no `FirstName` coluna do `Student` tabela. Se você não especificar nomes de coluna, eles recebem o mesmo nome que o nome da propriedade.

No *Student.cs* de arquivo, adicione uma `using` instrução para [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e adicione o atributo de nome de coluna para o `FirstMidName` propriedade, conforme mostrado no seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

A adição do [atributo de coluna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) altera o modelo fazendo SchoolContext, portanto ele não coincidir com o banco de dados. Digite os seguintes comandos em PMC para criar outra migração:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Em **Server Explorer**, abra o *aluno* designer de tabela clicando duas vezes o *aluno* tabela.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

A imagem a seguir mostra o nome da coluna original como estava antes de você aplicou as duas primeiras migrações. Além do nome de coluna de `FirstMidName` para `FirstName`, alterou as duas colunas de nome de `MAX` comprimento a 50 caracteres.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Você pode também alterar banco de dados de mapeamento usando o [API fluente](https://msdn.microsoft.com/data/jj591617), como você verá posteriormente neste tutorial.

> [!NOTE]
> Se você tentar compilar antes de concluir a criação de todas as classes de entidade nas seções a seguir, você poderá receber erros de compilador.


## <a name="complete-changes-to-the-student-entity"></a>Concluir as alterações para a entidade do aluno

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Em *Models\Student.cs*, substitua o código que você adicionou anteriormente com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>O atributo necessário

O [atributo necessário](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) faz com que os campos obrigatórios de propriedades de nome. O `Required attribute` não é necessária para tipos de valor como DateTime, int, double e float. Tipos de valor não podem ser atribuídos um valor nulo, para que sejam tratados inerentemente como campos obrigatórios. Você pode remover o [atributo necessário](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) e substituí-lo com um parâmetro de comprimento mínimo para o `StringLength` atributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>O atributo de exibição

O `Display` atributo especifica que a legenda para as caixas de texto deve ser "Nome", "Sobrenome", "Nome completo" e "Data de registro" em vez do nome de propriedade em cada instância (que não tem espaço dividindo as palavras).

### <a name="the-fullname-calculated-property"></a>O FullName calculada propriedade

`FullName`é uma propriedade calculada que retorna um valor que é criado pela concatenação de duas outras propriedades. Portanto, tem apenas um `get` acessador e nenhum `FullName` coluna será gerada no banco de dados.

## <a name="create-the-instructor-entity"></a>Criar a entidade do instrutor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Criar *Models\Instructor.cs*, substituindo o código de modelo com o código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Observe que várias propriedades são os mesmos no `Student` e `Instructor` entidades. No [implementando herança](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente nesta série, você vai refatorar esse código para eliminar a redundância.

Você pode colocar vários atributos em uma linha, portanto, você também pode escrever a classe instrutor da seguinte maneira:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Os cursos e as propriedades de navegação de OfficeAssignment

O `Courses` e `OfficeAssignment` propriedades são propriedades de navegação. Conforme explicado anteriormente, elas geralmente são definidas como [virtual](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) para que eles podem tirar proveito de um recurso do Entity Framework chamado [carregamento preguiçoso](https://msdn.microsoft.com/magazine/hh205756.aspx). Além disso, se uma propriedade de navegação pode conter várias entidades, seu tipo deve implementar o [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) Interface. Por exemplo [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifica mas não [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) porque `IEnumerable<T>` não implementa [adicionar ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Um instrutor pode ensinar a qualquer número de cursos, portanto `Courses` é definido como uma coleção de `Course` entidades.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Nosso regras de negócio estado instrutor só pode ter no máximo um escritório, portanto `OfficeAssignment` é definido como um único `OfficeAssignment` entidade (que pode ser `null` se office não estiver atribuído).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Criar a entidade OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Criar *Models\OfficeAssignment.cs* com o código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compilar o projeto, que salva as alterações e se que você não tiver feito qualquer cópia e cole erros que o compilador pode detectar.

### <a name="the-key-attribute"></a>O atributo de chave

Há uma relação um-para-zero-ou-um entre o `Instructor` e `OfficeAssignment` entidades. Uma atribuição de office só existe em relação ao instrutor é atribuído a e, portanto, sua chave primária também for sua chave estrangeira para a `Instructor` entidade. Mas o Entity Framework não reconhece automaticamente `InstructorID` como primário chave desta entidade porque seu nome não segue o `ID` ou *classname* `ID` convenção de nomenclatura. Portanto, o `Key` atributo é usado para identificá-lo como a chave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Você também pode usar o `Key` atributo se a entidade tem sua própria chave primária, mas você deseja nomear a propriedade algo diferente de `classnameID` ou `ID`. Por padrão o EF trata a chave como não gerado pelo banco de dados porque a coluna é uma relação de identificação.

### <a name="the-foreignkey-attribute"></a>O atributo ForeignKey

Quando há uma relação um-para-zero-ou-um ou um-para-um relacionamento entre duas entidades (tais como entre `OfficeAssignment` e `Instructor`), EF não é possível descobrir qual ponta da relação é a entidade de segurança e quais final é dependente. Relações um para um tem uma propriedade de navegação de referência em cada classe para a outra classe. O [ForeignKey atributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) pode ser aplicado à classe dependente para estabelecer a relação. Se você omitir o [ForeignKey atributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), você obterá o seguinte erro ao tentar criar a migração:

*Não é possível determinar a extremidade principal de uma associação entre os tipos 'ContosoUniversity.Models.OfficeAssignment' e 'ContosoUniversity.Models.Instructor'. A extremidade principal dessa associação deve ser configurada explicitamente usando a API fluente de relação ou as anotações de dados.*

No tutorial posteriormente, você verá como configurar esse relação com a API fluente.

### <a name="the-instructor-navigation-property"></a>A propriedade de navegação do instrutor

O `Instructor` entidade tem um valor nulo `OfficeAssignment` propriedade de navegação (porque um instrutor pode não ter uma atribuição de office) e o `OfficeAssignment` entidade tem uma não-anulável `Instructor` propriedade de navegação (porque não é uma atribuição do office existir sem um instrutor – `InstructorID` é não anulável). Quando um `Instructor` entidade tem um relacionados `OfficeAssignment` entidade, cada entidade terá uma referência a um em sua propriedade de navegação.

Você pode colocar um `[Required]` atributo na propriedade de navegação instrutor para especificar que deve haver um instrutor relacionado, mas você não precisa fazer isso porque a chave estrangeira de InstructorID (que também é a chave para esta tabela) é não anulável.

## <a name="modify-the-course-entity"></a>Modificar a entidade de curso

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Em *Models\Course.cs*, substitua o código que você adicionou anteriormente com o código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

A entidade de curso tem uma propriedade de chave estrangeira `DepartmentID` que aponta para o relacionados `Department` entidade e é necessário um `Department` propriedade de navegação. O Entity Framework não exigir que você adicionar uma propriedade de chave estrangeira para o modelo de dados quando você tem uma propriedade de navegação para uma entidade relacionada. EF cria automaticamente chaves estrangeiras no banco de dados sempre que forem necessários. Mas, com a chave estrangeira no modelo de dados pode fazer atualizações mais simples e mais eficiente. Por exemplo, ao buscar uma entidade de curso para editar o `Department` entidade é null se não fazê-lo, então quando você atualiza a entidade de curso, você precisaria primeiro obter o `Department` entidade. Quando a propriedade de chave estrangeira `DepartmentID` está incluído no modelo de dados, você não precisa buscar o `Department` entidade antes de atualizar.

### <a name="the-databasegenerated-attribute"></a>O atributo DatabaseGenerated

O [DatabaseGenerated atributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) com o [nenhum](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parâmetro o `CourseID` propriedade especifica se valores de chave primária são fornecidos pelo usuário em vez de gerado pelo banco de dados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Por padrão, o Entity Framework pressupõe que os valores de chave primária são gerados pelo banco de dados. É o que você quer na maioria dos cenários. No entanto, para `Course` entidades, você usa um número especificado pelo usuário como uma série de 1000 de um departamento, uma série de 2000 para outro departamento e assim por diante.

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de chave estrangeira e propriedades de navegação no `Course` entidade refletem as relações a seguir:

- Um curso é atribuído a um departamento, para que haja um `DepartmentID` chave estrangeira e um `Department` propriedade de navegação pelas razões mencionadas acima. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Um curso pode ter qualquer número de alunos inscritos, portanto, o `Enrollments` propriedade de navegação é uma coleção: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Um curso pode ser ensinado por vários instrutores, portanto, o `Instructors` propriedade de navegação é uma coleção: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Criar a entidade de departamento

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Criar *Models\Department.cs* com o código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>O atributo de coluna

Anteriormente, você usou o [atributo de coluna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) para alterar o mapeamento de nome de coluna. No código para o `Department` entidade, o `Column` atributo está sendo usado para alterar SQL dados mapeamento de tipo para que a coluna será definida usando o SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx) tipo no banco de dados:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapeamento de coluna geralmente não é necessário, pois o Entity Framework geralmente escolhe o tipo de dados do SQL Server apropriado com base no tipo CLR que você definir para a propriedade. O CLR `decimal` tipo é mapeado para um SQL Server `decimal` tipo. Mas nesse caso, você sabe que a coluna será mantendo valores de moeda e o [money](https://msdn.microsoft.com/library/ms179882.aspx) tipo de dados é mais apropriado para fazer isso. Para obter mais informações sobre tipos de dados CLR e como eles correspondem aos tipos de dados do SQL Server, consulte [SqlClient para Entity FrameworkTypes](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de navegação e chave estrangeiras refletem as relações a seguir:

- Um departamento pode ou não ter um administrador, e um administrador é sempre um instrutor. Portanto o `InstructorID` propriedade está incluída como a chave estrangeira para a `Instructor` entidade e um ponto de interrogação é adicionado após o `int` designação para marcar a propriedade como um valor nulo de tipo. A propriedade de navegação é denominada `Administrator` , mas contém um `Instructor` entidade: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Um departamento pode ter vários cursos, portanto, há um `Courses` propriedade de navegação: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > Por convenção, o Entity Framework permite a exclusão em cascata para chaves estrangeiras não anuláveis em relações muitos-para-muitos. Isso pode resultar em regras de exclusão em cascata circular, que fará com que uma exceção ao tentar adicionar uma migração. Por exemplo, se você não definiu o `Department.InstructorID` propriedade como anuláveis, você obteria a seguinte mensagem de exceção: "o relacionamento referencial resultará em uma referência cíclica não é permitida." Se as regras de negócio necessário `InstructorID` propriedade a ser não anuláveis, você precisa usar a seguinte instrução API fluente para desabilitar a exclusão em cascata na relação: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>Modificar a entidade de registro

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 Em *Models\Enrollment.cs*, substitua o código que você adicionou anteriormente com o código a seguir

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de navegação e propriedades de chave estrangeira refletem as relações a seguir:

- Um registro é para um único curso, portanto, há um `CourseID` propriedade de chave estrangeira e um `Course` propriedade de navegação: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Um registro é para um aluno único, portanto, há um `StudentID` propriedade de chave estrangeira e um `Student` propriedade de navegação: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relações muitos-para-muitos

Há uma relação muitos-para-muitos entre o `Student` e `Course` entidades e o `Enrollment` entidade funciona como uma tabela de junção de muitos-para-muitos *com carga* no banco de dados. Isso significa que o `Enrollment` tabela contém dados adicionais além de chaves estrangeiras de tabelas unidas (neste caso, uma chave primária e uma `Grade` propriedade).

A ilustração a seguir mostra a aparência dessas relações em um diagrama de entidade. (Este diagrama foi gerado usando o [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); criar o diagrama não faz parte do tutorial, ele está sendo usado apenas como uma ilustração.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Cada linha de relação tem um 1 em uma extremidade e um asterisco (\*) em outro, indicando uma relação um-para-muitos.

Se o `Enrollment` tabela não incluir informações de classificação, ele só precisa conter as duas chaves estrangeiras `CourseID` e `StudentID`. Nesse caso, ela corresponde a uma tabela de junção de muitos-para-muitos *sem carga* (ou um *tabela de junção puro*) no banco de dados, e você não precisa criar uma classe de modelo para ele em todos os. O `Instructor` e `Course` as entidades têm esse tipo de relação muitos-para-muitos, e como você pode ver, não há nenhuma classe de entidade entre eles:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Uma tabela de junção é necessária no banco de dados, no entanto, conforme mostrado no diagrama de banco de dados a seguir:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

O Entity Framework cria automaticamente o `CourseInstructor` tabela e ler e atualizá-lo indiretamente por ler e atualizar o `Instructor.Courses` e `Course.Instructors` propriedades de navegação.

## <a name="entity-diagram-showing-relationships"></a>Relações de exibição de diagrama de entidade

A ilustração a seguir mostra o diagrama que cria o Entity Framework Power Tools para o modelo de escola concluído.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

Além de linhas de relação muitos-para-muitos (\* para \*) e as linhas de relação um-para-muitos (1 a \*), você pode ver a linha de relação um-para-zero-ou-um (1 para entre 0 e 1) entre o `Instructor` e `OfficeAssignment` entidades e a linha de relação de zero-ou-um-para-muitos (entre 0 e 1 para \*) entre as entidades instrutor e departamento.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizar o modelo de dados, adicionando o código para o contexto do banco de dados

Em seguida, você adicionará novas entidades para o `SchoolContext` classe e personalizar alguns do mapeamento usando [API fluente](https://msdn.microsoft.com/data/jj591617) chamadas. A API é "fluente" porque ele geralmente é usado pelo encadeamento uma série de chamadas de método em uma única instrução, como no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

Neste tutorial, você usará a API fluente somente para mapeamento de banco de dados que você não pode fazer com os atributos. No entanto, você também pode usar a API fluente para especificar a maioria da formatação, validação e regras de mapeamento que você pode fazer por meio de atributos. Alguns atributos como `MinimumLength` não pode ser aplicado com a API fluente. Conforme mencionado anteriormente, `MinimumLength` não altera o esquema, ela só se aplica a uma regra de validação do cliente e servidor

Alguns desenvolvedores preferem usar a API fluente exclusivamente para que eles podem manter suas classes de entidade "normal". Você pode misturar atributos e API fluente se desejar, e há algumas personalizações que só podem ser feitas usando a API fluente, mas em geral a prática recomendada é escolher uma destas duas abordagens e usar ou consistentemente tanto quanto possível.

Para adicionar novas entidades para os dados de modelo e realize o mapeamento de banco de dados que você não fez por meio de atributos, substitua o código em *DAL\SchoolContext.cs* com o código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

A nova instrução no [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método configura a tabela de junção de muitos-para-muitos:

- Para a relação muitos-para-muitos entre o `Instructor` e `Course` entidades, o código especifica os nomes de tabela e coluna para a tabela de junção. Código primeiro pode configurar a relação muitos-para-muitos para você sem esse código, mas se você não chamá-lo, você obterá nomes padrão, como `InstructorInstructorID` para o `InstructorID` coluna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

O código a seguir fornece um exemplo de como você poderia ter usado a API fluente em vez de atributos para especificar a relação entre o `Instructor` e `OfficeAssignment` entidades:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Para obter informações sobre quais instruções "API fluente" estão fazendo em segundo plano, consulte o [API fluente](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) postagem de blog.

## <a name="seed-the-database-with-test-data"></a>Preencha o banco de dados de teste

Substitua o código no *Migrations\Configuration.cs* arquivo com o código a seguir para fornecer dados de propagação de novas entidades que você criou.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Como você viu no primeiro tutorial, a maioria do código simplesmente atualiza ou cria novos objetos de entidade e carrega dados de exemplo em propriedades conforme necessário para teste. No entanto, observe como o `Course` entidade, que tem uma relação muitos-para-muitos com a `Instructor` entidade, é tratado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Quando você cria um `Course` do objeto, você inicializar a `Instructors` propriedade de navegação como uma coleção vazia usando o código `Instructors = new List<Instructor>()`. Isso torna possível adicionar `Instructor` entidades que estão relacionadas a este `Course` usando o `Instructors.Add` método. Se você não criar uma lista vazia, você não poderá adicionar essas relações, porque o `Instructors` propriedade deve ser nula e não teria um `Add` método. Você também pode adicionar a inicialização de lista para o construtor.

## <a name="add-a-migration-and-update-the-database"></a>Adicionar uma migração e atualizar o banco de dados

De PMC, insira o `add-migration` comando (não faça a `update-database` comando ainda):

`add-Migration ComplexDataModel`

Se você tentou executar o `update-database` neste ponto de comando (não faça isso ainda), você deverá obter o seguinte erro:

*A instrução ALTER TABLE em conflito com a restrição de chave estrangeira "FK\_dbo. Curso\_dbo. Departamento\_DepartmentID ". O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Departamento", coluna 'DepartmentID'.*

Às vezes, quando você executar migrações com dados existentes, você precisa inserir dados stub para o banco de dados para atender as restrições de chave estrangeira, e que é o que você precisa fazer agora. O código gerado a ComplexDataModel `Up` método adiciona uma não-anulável `DepartmentID` chave estrangeira para a `Course` tabela. Porque já existem linhas no `Course` tabela quando o código é executado, o `AddColumn` haverá falha na operação porque o SQL Server não sabe qual valor para colocar na coluna que não pode ser nula. Portanto precisa alterar o código para fornecer a nova coluna de um valor padrão e criar um departamento de stub chamado "Temp" para agir como o departamento de padrão. Como resultado, existente `Course` linhas serão estar relacionadas ao departamento "Temp" após o `Up` execuções do método. Pode relacioná-los para os departamentos corretos do `Seed` método.

Editar o &lt; *timestamp&gt;\_ComplexDataModel.cs* , comente a linha de código que adiciona a coluna de ID do departamento na tabela de curso e adicione o seguinte código (o comentados linha também é realçada):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Quando o `Seed` execuções do método, ele irá inserir linhas no `Department` tabela e ela se relacionará existente `Course` linhas para essas novas `Department` linhas. Se você não adicionou nenhum curso na interface de usuário, você deve, em seguida, não precisar mais do departamento de "Temp" ou o valor padrão no `Course.DepartmentID` coluna. Para permitir a possibilidade de que alguém pode ter adicionado cursos, usando o aplicativo, você também deseja atualizar o `Seed` código do método para garantir que todos os `Course` linhas (não apenas aqueles inseridas por execuções anteriores do `Seed` método) tem válido `DepartmentID` valores antes de remover o padrão da coluna de valor e exclua o departamento de "Temp".

Depois de terminar de editar o &lt; *timestamp&gt;\_ComplexDataModel.cs* de arquivo, insira o `update-database` do PMC para executar a migração.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> É possível obter outros erros durante a migração de dados e fazer alterações de esquema. Se você obtiver erros de migração que não é possível resolver, você pode alterar o nome do banco de dados na cadeia de conexão ou excluir o banco de dados. A abordagem mais simples é renomear o banco de dados em *Web. config* arquivo. O exemplo a seguir mostra o nome alterado a CU\_teste:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> Com um novo banco de dados, não há nenhum dado para migrar e o `update-database` comando é muito mais probabilidade de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como descartar um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
> 
> Se isso falhar, outra coisa que você pode tentar é reinicialize o banco de dados digitando o seguinte comando no PMC:
> 
> `update-database -TargetMigration:0`


Abra o banco de dados em **Server Explorer** como você fez anteriormente e expanda o **tabelas** nó para ver se todas as tabelas foram criadas. (Se você ainda tiver **Server Explorer** abrir da hora anterior, clique no **atualização** botão.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Você não criar uma classe de modelo para o `CourseInstructor` tabela. Conforme explicado anteriormente, essa é uma tabela de junção para a relação muitos-para-muitos entre o `Instructor` e `Course` entidades.

Com o botão direito do `CourseInstructor` de tabela e selecione **Mostrar dados da tabela** para verificar se ele tem dados nele como resultado da `Instructor` entidades que você adicionou ao `Course.Instructors` propriedade de navegação.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>Resumo

Agora você tem um modelo de dados mais complexo e o banco de dados correspondente. O seguinte tutorial você aprenderá mais sobre as diferentes maneiras de acessar os dados relacionados.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Próximo](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
