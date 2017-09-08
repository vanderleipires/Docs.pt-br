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
ms.openlocfilehash: 7d216bc07d0a8d739f0cecbc5b571b6144c13e61
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="4f018-104">Criar um modelo de dados complexos - Core EF com o tutorial do MVC do ASP.NET Core (5 de 10)</span><span class="sxs-lookup"><span data-stu-id="4f018-104">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="4f018-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f018-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f018-106">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f018-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="4f018-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="4f018-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="4f018-108">Os tutoriais anteriores, você trabalhou com um modelo de dados simples que foi composto de três entidades.</span><span class="sxs-lookup"><span data-stu-id="4f018-108">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="4f018-109">Neste tutorial, você adicionará mais entidades e relações e você personalizará o modelo de dados especificando a formatação, validação e regras de mapeamento de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-109">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="4f018-110">Quando você terminar, as classes de entidade se tornará o modelo de dados completo é mostrado na ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-110">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagrama de entidade](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="4f018-112">Personalizar o modelo de dados por meio de atributos</span><span class="sxs-lookup"><span data-stu-id="4f018-112">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="4f018-113">Nesta seção, você verá como personalizar o modelo de dados por meio de atributos que especificam a formatação, validação e regras de mapeamento de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-113">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="4f018-114">Em seguida, em várias seções a seguir, você criará o modelo de dados de escola completo com a adição de atributos para as classes você já criou e criar novas classes para os demais tipos de entidade no modelo.</span><span class="sxs-lookup"><span data-stu-id="4f018-114">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="4f018-115">O atributo de tipo de dados</span><span class="sxs-lookup"><span data-stu-id="4f018-115">The DataType attribute</span></span>

<span data-ttu-id="4f018-116">Para datas de registro do aluno, todas as páginas da web atualmente exibem o tempo junto com a data, embora tudo o que você gosta para esse campo é a data.</span><span class="sxs-lookup"><span data-stu-id="4f018-116">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="4f018-117">Usando atributos de anotação de dados, você pode fazer uma alteração que corrige o formato de exibição em cada modo de exibição que mostra os dados de código.</span><span class="sxs-lookup"><span data-stu-id="4f018-117">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="4f018-118">Para ver um exemplo de como fazer o que, você adicionará um atributo para o `EnrollmentDate` propriedade o `Student` classe.</span><span class="sxs-lookup"><span data-stu-id="4f018-118">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="4f018-119">Em *Models/Student.cs*, adicione um `using` instrução para o `System.ComponentModel.DataAnnotations` namespace e adicionar `DataType` e `DisplayFormat` atributos para o `EnrollmentDate` propriedade, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-119">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

<span data-ttu-id="4f018-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span><span class="sxs-lookup"><span data-stu-id="4f018-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span></span>

<span data-ttu-id="4f018-121">O `DataType` atributo é usado para especificar um tipo de dados que é mais específico que o tipo de banco de dados intrínseco.</span><span class="sxs-lookup"><span data-stu-id="4f018-121">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="4f018-122">Nesse caso, apenas desejamos controlar a data, não a data e hora.</span><span class="sxs-lookup"><span data-stu-id="4f018-122">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="4f018-123">O `DataType` enumeração fornece para muitos tipos de dados, como data, hora, PhoneNumber, moeda, endereço de email e muito mais.</span><span class="sxs-lookup"><span data-stu-id="4f018-123">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="4f018-124">O `DataType` atributo também pode habilitar o aplicativo fornecer automaticamente os recursos de um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="4f018-124">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="4f018-125">Por exemplo, um `mailto:` link pode ser criado para `DataType.EmailAddress`, e um seletor de data pode ser fornecido para `DataType.Date` em navegadores que oferecem suporte a HTML5.</span><span class="sxs-lookup"><span data-stu-id="4f018-125">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="4f018-126">O `DataType` atributo emite HTML 5 `data-` atributos (pronunciado dados dash) que podem compreender navegadores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="4f018-126">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="4f018-127">O `DataType` atributos não fornecem nenhuma validação.</span><span class="sxs-lookup"><span data-stu-id="4f018-127">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="4f018-128">`DataType.Date`não especifique o formato da data que é exibida.</span><span class="sxs-lookup"><span data-stu-id="4f018-128">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="4f018-129">Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base em CultureInfo do servidor.</span><span class="sxs-lookup"><span data-stu-id="4f018-129">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="4f018-130">O `DisplayFormat` atributo é usado para especificar explicitamente o formato de data:</span><span class="sxs-lookup"><span data-stu-id="4f018-130">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="4f018-131">O `ApplyFormatInEditMode` configuração especifica que a formatação também deve ser aplicada quando o valor é exibido na caixa de texto para edição.</span><span class="sxs-lookup"><span data-stu-id="4f018-131">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="4f018-132">(Você talvez não queira que alguns campos — por exemplo, para valores de moeda, não convém o símbolo de moeda na caixa de texto para edição.)</span><span class="sxs-lookup"><span data-stu-id="4f018-132">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="4f018-133">Você pode usar o `DisplayFormat` atributo por si mesmo, mas geralmente é uma boa ideia usar a `DataType` atributo também.</span><span class="sxs-lookup"><span data-stu-id="4f018-133">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="4f018-134">O `DataType` atributo transmite a semântica dos dados em vez de como renderizá-lo em uma tela e fornece os seguintes benefícios que você não obtém com `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="4f018-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="4f018-135">O navegador pode habilitar os recursos do HTML5 (por exemplo mostrar um controle de calendário, o símbolo de moeda local apropriado, links de email, alguns cliente entrada validação, etc.).</span><span class="sxs-lookup"><span data-stu-id="4f018-135">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="4f018-136">Por padrão, o navegador processará os dados usando o formato correto com base na localidade.</span><span class="sxs-lookup"><span data-stu-id="4f018-136">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="4f018-137">Para obter mais informações, consulte o [ \<entrada > documentação do auxiliar de marca](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="4f018-137">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="4f018-138">Execute novamente a página de índice de alunos e observe que vezes não são exibidos para as datas de registro.</span><span class="sxs-lookup"><span data-stu-id="4f018-138">Run the Students Index page again and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="4f018-139">O mesmo será verdadeiro para qualquer modo de exibição que usa o modelo de Student.</span><span class="sxs-lookup"><span data-stu-id="4f018-139">The same will be true for any view that uses the Student model.</span></span>

![Página de índice de alunos mostrando datas sem vezes](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="4f018-141">O atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="4f018-141">The StringLength attribute</span></span>

<span data-ttu-id="4f018-142">Você também pode especificar regras de validação de dados e mensagens de erro de validação usando atributos.</span><span class="sxs-lookup"><span data-stu-id="4f018-142">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="4f018-143">O `StringLength` atributo define o comprimento máximo do banco de dados e fornece do lado do cliente e do lado do servidor validação para o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f018-143">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="4f018-144">Você também pode especificar o comprimento mínimo da cadeia de caracteres neste atributo, mas o valor mínimo não tem nenhum impacto sobre o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-144">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="4f018-145">Suponha que você deseja garantir que os usuários não insiram mais de 50 caracteres para um nome.</span><span class="sxs-lookup"><span data-stu-id="4f018-145">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="4f018-146">Para adicionar essa limitação, adicione `StringLength` atributos para o `LastName` e `FirstMidName` propriedades, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-146">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

<span data-ttu-id="4f018-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span><span class="sxs-lookup"><span data-stu-id="4f018-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span></span>

<span data-ttu-id="4f018-148">O `StringLength` atributo não impedem que um usuário inserir espaços em branco para um nome.</span><span class="sxs-lookup"><span data-stu-id="4f018-148">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="4f018-149">Você pode usar o `RegularExpression` atributo aplicar restrições para a entrada.</span><span class="sxs-lookup"><span data-stu-id="4f018-149">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="4f018-150">Por exemplo, o código a seguir exige que o primeiro caractere a ser maiuscula e os caracteres restantes para estar em ordem alfabética:</span><span class="sxs-lookup"><span data-stu-id="4f018-150">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="4f018-151">O `MaxLength` atributo fornece funcionalidade semelhante para o `StringLength` de atributo, mas não fornece do lado do cliente a validação.</span><span class="sxs-lookup"><span data-stu-id="4f018-151">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="4f018-152">Agora, o modelo de banco de dados foi alterado de forma que requer uma alteração no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-152">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="4f018-153">Você usará as migrações para atualizar o esquema sem perda dos dados que são adicionados ao banco de dados usando o interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4f018-153">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="4f018-154">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="4f018-154">Save your changes and build the project.</span></span> <span data-ttu-id="4f018-155">Em seguida, abra a janela de comando na pasta do projeto e digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="4f018-155">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="4f018-156">O `migrations add` comando avisa que pode ocorrer perda de dados, como a alteração torna o mais curto para duas colunas de comprimento máximo.</span><span class="sxs-lookup"><span data-stu-id="4f018-156">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="4f018-157">Migrações cria um arquivo chamado  *\<timeStamp > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="4f018-157">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="4f018-158">Este arquivo contém o código de `Up` método que atualizará o banco de dados para coincidir com o modelo de dados atual.</span><span class="sxs-lookup"><span data-stu-id="4f018-158">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="4f018-159">O `database update` esse código de execução do comando.</span><span class="sxs-lookup"><span data-stu-id="4f018-159">The `database update` command ran that code.</span></span>

<span data-ttu-id="4f018-160">O prefixo ao nome do arquivo de migrações de carimbo de hora é usado pelo Entity Framework para ordenar as migrações.</span><span class="sxs-lookup"><span data-stu-id="4f018-160">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="4f018-161">Você pode criar várias migrações antes de executar o comando de atualização de banco de dados e, em seguida, todas as migrações são aplicadas na ordem em que eles foram criados.</span><span class="sxs-lookup"><span data-stu-id="4f018-161">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="4f018-162">Execute a página de criação e digitar um nome de mais de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f018-162">Run the Create page, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="4f018-163">Quando você clicar em criar, validação do lado do cliente exibe uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="4f018-163">When you click Create, client side validation shows an error message.</span></span>

![Os alunos mostrando erros de comprimento de cadeia de caracteres de página de índice](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="4f018-165">O atributo de coluna</span><span class="sxs-lookup"><span data-stu-id="4f018-165">The Column attribute</span></span>

<span data-ttu-id="4f018-166">Você também pode usar atributos para controlar como as classes e propriedades são mapeadas para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-166">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="4f018-167">Suponha que você tenha usado o nome `FirstMidName` para o nome do primeiro campo porque o campo também pode conter um nome do meio.</span><span class="sxs-lookup"><span data-stu-id="4f018-167">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="4f018-168">Mas a coluna do banco de dados seja nomeada como `FirstName`, pois os usuários que serão gravado consultas ad hoc no banco de dados estão acostumados a esse nome.</span><span class="sxs-lookup"><span data-stu-id="4f018-168">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="4f018-169">Para tornar esse mapeamento, você pode usar o `Column` atributo.</span><span class="sxs-lookup"><span data-stu-id="4f018-169">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="4f018-170">O `Column` atributo especifica que quando o banco de dados é criado, a coluna do `Student` tabela que mapeia para o `FirstMidName` propriedade será nomeada `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="4f018-170">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="4f018-171">Em outras palavras, quando seu código se refere a `Student.FirstMidName`, os dados virão ou atualizados no `FirstName` coluna do `Student` tabela.</span><span class="sxs-lookup"><span data-stu-id="4f018-171">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="4f018-172">Se você não especificar nomes de coluna, eles recebem o mesmo nome que o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="4f018-172">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="4f018-173">No *Student.cs* de arquivo, adicione uma `using` instrução para `System.ComponentModel.DataAnnotations.Schema` e adicione o atributo de nome de coluna para o `FirstMidName` propriedade, como mostra o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="4f018-173">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

<span data-ttu-id="4f018-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span><span class="sxs-lookup"><span data-stu-id="4f018-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span></span>

<span data-ttu-id="4f018-175">A adição do `Column` atributo altera o backup do modelo de `SchoolContext`, portanto, ele não coincidir com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-175">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="4f018-176">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="4f018-176">Save your changes and build the project.</span></span> <span data-ttu-id="4f018-177">Em seguida, abra a janela de comando na pasta do projeto e digite os seguintes comandos para criar outra migração:</span><span class="sxs-lookup"><span data-stu-id="4f018-177">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="4f018-178">Em **Pesquisador de objetos do SQL Server**, abra o designer de tabela do aluno clicando duas vezes o **aluno** tabela.</span><span class="sxs-lookup"><span data-stu-id="4f018-178">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabela de alunos em SSOX após a migração](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="4f018-180">Antes de você aplicou as duas primeiras migrações, as colunas de nome forem do tipo nvarchar (max).</span><span class="sxs-lookup"><span data-stu-id="4f018-180">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="4f018-181">Agora são nvarchar (50) e o nome da coluna foi alterado de FirstMidName para FirstName.</span><span class="sxs-lookup"><span data-stu-id="4f018-181">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="4f018-182">Se você tentar compilar antes de concluir a criação de todas as classes de entidade nas seções a seguir, você poderá receber erros de compilador.</span><span class="sxs-lookup"><span data-stu-id="4f018-182">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="4f018-183">Alterações finais para a entidade do aluno</span><span class="sxs-lookup"><span data-stu-id="4f018-183">Final changes to the Student entity</span></span>

![Entidade do aluno](complex-data-model/_static/student-entity.png)

<span data-ttu-id="4f018-185">Em *Models/Student.cs*, substitua o código que você adicionou anteriormente com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="4f018-185">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="4f018-186">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="4f018-186">The changes are highlighted.</span></span>

<span data-ttu-id="4f018-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span><span class="sxs-lookup"><span data-stu-id="4f018-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="4f018-188">O atributo necessário</span><span class="sxs-lookup"><span data-stu-id="4f018-188">The Required attribute</span></span>

<span data-ttu-id="4f018-189">O `Required` atributo faz com que os campos obrigatórios de propriedades de nome.</span><span class="sxs-lookup"><span data-stu-id="4f018-189">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="4f018-190">O `Required` atributo não é necessária para tipos não anuláveis, como tipos de valor (DateTime, int, clique duas vezes, float, etc.).</span><span class="sxs-lookup"><span data-stu-id="4f018-190">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="4f018-191">Tipos que não podem ser nulos automaticamente são tratados como campos obrigatórios.</span><span class="sxs-lookup"><span data-stu-id="4f018-191">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="4f018-192">Você pode remover o `Required` de atributos e substituí-lo com um parâmetro de comprimento mínimo para o `StringLength` atributo:</span><span class="sxs-lookup"><span data-stu-id="4f018-192">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="4f018-193">O atributo de exibição</span><span class="sxs-lookup"><span data-stu-id="4f018-193">The Display attribute</span></span>

<span data-ttu-id="4f018-194">O `Display` atributo especifica que a legenda para as caixas de texto deve ser "Nome", "Sobrenome", "Nome completo" e "Data de registro" em vez do nome de propriedade em cada instância (que não tem espaço dividindo as palavras).</span><span class="sxs-lookup"><span data-stu-id="4f018-194">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="4f018-195">A propriedade FullName calculada</span><span class="sxs-lookup"><span data-stu-id="4f018-195">The FullName calculated property</span></span>

<span data-ttu-id="4f018-196">`FullName`é uma propriedade calculada que retorna um valor que é criado pela concatenação de duas outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="4f018-196">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="4f018-197">Portanto, ele tem um acessador get e não `FullName` coluna será gerada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-197">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="4f018-198">Criar a entidade do instrutor</span><span class="sxs-lookup"><span data-stu-id="4f018-198">Create the Instructor Entity</span></span>

![Entidade instrutor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="4f018-200">Criar *Models/Instructor.cs*, substituindo o código de modelo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-200">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

<span data-ttu-id="4f018-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span><span class="sxs-lookup"><span data-stu-id="4f018-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span></span>

<span data-ttu-id="4f018-202">Observe que várias propriedades são os mesmos nas entidades Student e instrutor.</span><span class="sxs-lookup"><span data-stu-id="4f018-202">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="4f018-203">No [implementando herança](inheritance.md) tutorial posteriormente nesta série, você vai refatorar esse código para eliminar a redundância.</span><span class="sxs-lookup"><span data-stu-id="4f018-203">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="4f018-204">Você pode colocar vários atributos em uma linha, portanto, você também pode escrever o `HireDate` atributos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4f018-204">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="4f018-205">As propriedades de navegação CourseAssignments e OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="4f018-205">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="4f018-206">O `CourseAssignments` e `OfficeAssignment` propriedades são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="4f018-206">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="4f018-207">Um instrutor pode ensinar a qualquer número de cursos, portanto `CourseAssignments` é definido como uma coleção.</span><span class="sxs-lookup"><span data-stu-id="4f018-207">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="4f018-208">Se uma propriedade de navegação pode conter várias entidades, seu tipo deve ser uma lista na qual as entradas podem ser adicionadas, excluídas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="4f018-208">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="4f018-209">Você pode especificar `ICollection<T>` ou um tipo, como `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="4f018-209">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="4f018-210">Se você especificar `ICollection<T>`, EF cria um `HashSet<T>` coleção por padrão.</span><span class="sxs-lookup"><span data-stu-id="4f018-210">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="4f018-211">O motivo por que esses são `CourseAssignment` entidades são explicados abaixo na seção sobre relações muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="4f018-211">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="4f018-212">As regras de negócio Contoso Universidade de estado que instrutor só pode ter no máximo um escritório, portanto, o `OfficeAssignment` propriedade contém uma única entidade OfficeAssignment (que pode ser null se o office não está atribuído).</span><span class="sxs-lookup"><span data-stu-id="4f018-212">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="4f018-213">Criar a entidade OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="4f018-213">Create the OfficeAssignment entity</span></span>

![Entidade OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="4f018-215">Criar *Models/OfficeAssignment.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-215">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

<span data-ttu-id="4f018-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="4f018-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span></span>

### <a name="the-key-attribute"></a><span data-ttu-id="4f018-217">O atributo de chave</span><span class="sxs-lookup"><span data-stu-id="4f018-217">The Key attribute</span></span>

<span data-ttu-id="4f018-218">Há uma relação um-para-zero-ou-um entre as entidades de OfficeAssignment e o instrutor.</span><span class="sxs-lookup"><span data-stu-id="4f018-218">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="4f018-219">Uma atribuição de office só existe em relação ao instrutor a que é atribuído e, portanto, sua chave primária também for sua chave estrangeira para a entidade do instrutor.</span><span class="sxs-lookup"><span data-stu-id="4f018-219">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="4f018-220">Mas o Entity Framework não pode reconhecer InstructorID como a chave primária dessa entidade porque seu nome não segue a convenção de nomenclatura de ID ou classnameID.</span><span class="sxs-lookup"><span data-stu-id="4f018-220">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="4f018-221">Portanto, o `Key` atributo é usado para identificá-lo como a chave:</span><span class="sxs-lookup"><span data-stu-id="4f018-221">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="4f018-222">Você também pode usar o `Key` atributo se a entidade tem sua própria chave primária, mas você deseja atribuir um nome de propriedade que não seja classnameID ou ID.</span><span class="sxs-lookup"><span data-stu-id="4f018-222">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="4f018-223">Por padrão, o EF trata a chave como não gerado pelo banco de dados porque a coluna é uma relação de identificação.</span><span class="sxs-lookup"><span data-stu-id="4f018-223">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="4f018-224">A propriedade de navegação do instrutor</span><span class="sxs-lookup"><span data-stu-id="4f018-224">The Instructor navigation property</span></span>

<span data-ttu-id="4f018-225">A entidade do instrutor tem um valor nulo `OfficeAssignment` propriedade de navegação (porque um instrutor pode não ter uma atribuição de office), e a entidade OfficeAssignment tem uma não-anulável `Instructor` propriedade de navegação (porque não é uma atribuição do office existir sem um instrutor – `InstructorID` é não anulável).</span><span class="sxs-lookup"><span data-stu-id="4f018-225">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="4f018-226">Quando uma entidade do instrutor tem uma entidade relacionada de OfficeAssignment, cada entidade tem uma referência a um em sua propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="4f018-226">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="4f018-227">Você pode colocar um `[Required]` atributo na propriedade de navegação instrutor para especificar que deve haver um instrutor relacionado, mas você não precisa fazer isso porque o `InstructorID` chave estrangeira (que também é a chave para esta tabela) é não anulável.</span><span class="sxs-lookup"><span data-stu-id="4f018-227">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="4f018-228">Modificar a entidade de curso</span><span class="sxs-lookup"><span data-stu-id="4f018-228">Modify the Course Entity</span></span>

![Entidade de curso](complex-data-model/_static/course-entity.png)

<span data-ttu-id="4f018-230">Em *Models/Course.cs*, substitua o código que você adicionou anteriormente com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="4f018-230">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="4f018-231">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="4f018-231">The changes are highlighted.</span></span>

<span data-ttu-id="4f018-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span><span class="sxs-lookup"><span data-stu-id="4f018-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span></span>

<span data-ttu-id="4f018-233">A entidade de curso tem uma propriedade de chave estrangeira `DepartmentID` que aponta para a entidade relacionada do departamento e ele tem um `Department` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="4f018-233">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="4f018-234">O Entity Framework não exigir que você adicionar uma propriedade de chave estrangeira para o modelo de dados quando você tem uma propriedade de navegação para uma entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="4f018-234">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="4f018-235">EF cria chaves estrangeiras no banco de dados sempre que eles são necessários e cria automaticamente [propriedades de sombra](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para eles.</span><span class="sxs-lookup"><span data-stu-id="4f018-235">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="4f018-236">Mas, com a chave estrangeira no modelo de dados pode fazer atualizações mais simples e mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="4f018-236">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="4f018-237">Por exemplo, quando você busca uma entidade de curso para editar, a entidade de departamento é null se não fazê-lo, então quando você atualiza a entidade de curso, você precisaria primeiro obter a entidade de departamento.</span><span class="sxs-lookup"><span data-stu-id="4f018-237">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="4f018-238">Quando a propriedade de chave estrangeira `DepartmentID` está incluído no modelo de dados, você não precisa buscar à entidade Department antes de atualizar.</span><span class="sxs-lookup"><span data-stu-id="4f018-238">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="4f018-239">O atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="4f018-239">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="4f018-240">O `DatabaseGenerated` atributo com o `None` parâmetro o `CourseID` propriedade especifica se valores de chave primária são fornecidos pelo usuário em vez de gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-240">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="4f018-241">Por padrão, o Entity Framework pressupõe que os valores de chave primária são gerados pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-241">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="4f018-242">É o que você quer na maioria dos cenários.</span><span class="sxs-lookup"><span data-stu-id="4f018-242">That's what you want in most scenarios.</span></span> <span data-ttu-id="4f018-243">No entanto, para entidades do curso, você usa um número especificado pelo usuário como uma série de 1000 de um departamento, uma série de 2000 para outro departamento e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4f018-243">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="4f018-244">O `DatabaseGenerated` atributo também pode ser usado para gerar valores padrão, como no caso de colunas de banco de dados usadas para registrar a data de uma linha foi criada ou atualizada.</span><span class="sxs-lookup"><span data-stu-id="4f018-244">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="4f018-245">Para obter mais informações, consulte [propriedades gerado](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="4f018-245">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4f018-246">Propriedades de navegação e de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="4f018-246">Foreign key and navigation properties</span></span>

<span data-ttu-id="4f018-247">As propriedades de chave estrangeira e propriedades de navegação na entidade curso refletem as seguintes relações:</span><span class="sxs-lookup"><span data-stu-id="4f018-247">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="4f018-248">Um curso é atribuído a um departamento, para que haja um `DepartmentID` chave estrangeira e um `Department` propriedade de navegação pelas razões mencionadas acima.</span><span class="sxs-lookup"><span data-stu-id="4f018-248">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="4f018-249">Um curso pode ter qualquer número de alunos inscritos, portanto, o `Enrollments` propriedade de navegação é uma coleção:</span><span class="sxs-lookup"><span data-stu-id="4f018-249">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="4f018-250">Um curso pode ser ensinado por vários instrutores, portanto, o `CourseAssignments` propriedade de navegação é uma coleção (o tipo `CourseAssignment` é explicado [posteriormente](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="4f018-250">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="4f018-251">Criar a entidade de departamento</span><span class="sxs-lookup"><span data-stu-id="4f018-251">Create the Department entity</span></span>

![Entidade Department](complex-data-model/_static/department-entity.png)


<span data-ttu-id="4f018-253">Criar *Models/Department.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-253">Create *Models/Department.cs* with the following code:</span></span>

<span data-ttu-id="4f018-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span><span class="sxs-lookup"><span data-stu-id="4f018-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="4f018-255">O atributo de coluna</span><span class="sxs-lookup"><span data-stu-id="4f018-255">The Column attribute</span></span>

<span data-ttu-id="4f018-256">Anteriormente, você usou o `Column` atributo para alterar o mapeamento de nome de coluna.</span><span class="sxs-lookup"><span data-stu-id="4f018-256">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="4f018-257">No código para a entidade de departamento, a `Column` atributo está sendo usado para alterar SQL dados mapeamento de tipo para que a coluna será definida usando o tipo de dinheiro do SQL Server no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="4f018-257">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="4f018-258">Mapeamento de coluna geralmente não é necessário, pois o Entity Framework escolhe o tipo de dados do SQL Server apropriado com base no tipo CLR que você definir para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="4f018-258">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="4f018-259">O CLR `decimal` tipo é mapeado para um SQL Server `decimal` tipo.</span><span class="sxs-lookup"><span data-stu-id="4f018-259">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="4f018-260">Mas, nesse caso, você sabe que a coluna será manter os valores de moeda e o tipo de dados money é mais apropriado para que.</span><span class="sxs-lookup"><span data-stu-id="4f018-260">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4f018-261">Propriedades de navegação e de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="4f018-261">Foreign key and navigation properties</span></span>

<span data-ttu-id="4f018-262">As propriedades de navegação e chave estrangeiras refletem as relações a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-262">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="4f018-263">Um departamento pode ou não ter um administrador, e um administrador é sempre um instrutor.</span><span class="sxs-lookup"><span data-stu-id="4f018-263">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="4f018-264">Portanto o `InstructorID` propriedade está incluída como a chave estrangeira à entidade instrutor e um ponto de interrogação é adicionado após o `int` designação para marcar a propriedade como um valor nulo de tipo.</span><span class="sxs-lookup"><span data-stu-id="4f018-264">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="4f018-265">A propriedade de navegação é denominada `Administrator` , mas contém uma entidade do instrutor:</span><span class="sxs-lookup"><span data-stu-id="4f018-265">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="4f018-266">Um departamento pode ter vários cursos, portanto, há uma propriedade de navegação de cursos:</span><span class="sxs-lookup"><span data-stu-id="4f018-266">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="4f018-267">Por convenção, o Entity Framework permite a exclusão em cascata para chaves estrangeiras não anuláveis em relações muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="4f018-267">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="4f018-268">Isso pode resultar em regras de exclusão em cascata circular, que fará com que uma exceção ao tentar adicionar uma migração.</span><span class="sxs-lookup"><span data-stu-id="4f018-268">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="4f018-269">Por exemplo, se você não definir a propriedade Department.InstructorID como anuláveis, EF poderia configurar uma regra de exclusão em cascata para excluir o instrutor quando você excluir o departamento, que é não o que você deseja que aconteça.</span><span class="sxs-lookup"><span data-stu-id="4f018-269">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="4f018-270">Se as regras de negócio é necessário o `InstructorID` propriedade a ser não anuláveis, você precisa usar a seguinte instrução API fluente para desabilitar a exclusão em cascata na relação:</span><span class="sxs-lookup"><span data-stu-id="4f018-270">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="4f018-271">Modificar a entidade de registro</span><span class="sxs-lookup"><span data-stu-id="4f018-271">Modify the Enrollment entity</span></span>

![Entidade de registro](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="4f018-273">Em *Models/Enrollment.cs*, substitua o código que você adicionou anteriormente com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-273">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

<span data-ttu-id="4f018-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span><span class="sxs-lookup"><span data-stu-id="4f018-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4f018-275">Propriedades de navegação e de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="4f018-275">Foreign key and navigation properties</span></span>

<span data-ttu-id="4f018-276">As propriedades de navegação e propriedades de chave estrangeira refletem as relações a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-276">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="4f018-277">Um registro é para um único curso, portanto, há um `CourseID` propriedade de chave estrangeira e um `Course` propriedade de navegação:</span><span class="sxs-lookup"><span data-stu-id="4f018-277">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="4f018-278">Um registro é para um aluno único, portanto, há um `StudentID` propriedade de chave estrangeira e um `Student` propriedade de navegação:</span><span class="sxs-lookup"><span data-stu-id="4f018-278">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="4f018-279">Relações muitos-para-muitos</span><span class="sxs-lookup"><span data-stu-id="4f018-279">Many-to-Many Relationships</span></span>

<span data-ttu-id="4f018-280">Há uma relação muitos-para-muitos entre as entidades Student e curso e a entidade de registro funciona como uma tabela de junção de muitos-para-muitos *com carga* no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-280">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="4f018-281">"Com carga" significa que a tabela de registro contém dados adicionais além de chaves estrangeiras de tabelas unidas (no caso, uma chave primária e uma propriedade de classificação).</span><span class="sxs-lookup"><span data-stu-id="4f018-281">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="4f018-282">A ilustração a seguir mostra a aparência dessas relações em um diagrama de entidade.</span><span class="sxs-lookup"><span data-stu-id="4f018-282">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="4f018-283">(Esse diagrama foi gerado usando o Entity Framework Power Tools para EF 6. x; criar o diagrama não faz parte do tutorial, ele está sendo usado apenas como uma ilustração.)</span><span class="sxs-lookup"><span data-stu-id="4f018-283">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Curso de aluno muitos para muitos](complex-data-model/_static/student-course.png)

<span data-ttu-id="4f018-285">Cada linha de relação tem um 1 em uma extremidade e um asterisco (*) em outro, indicando uma relação um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="4f018-285">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="4f018-286">Se a tabela de registro não incluir informações de classificação, ele só precisa conter as duas chaves estrangeiras CourseID e StudentID.</span><span class="sxs-lookup"><span data-stu-id="4f018-286">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="4f018-287">Nesse caso, seria uma tabela de junção de muitos-para-muitos sem carga (ou uma tabela de junção puro) no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-287">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="4f018-288">As entidades de curso e instrutor têm esse tipo de relação muitos-para-muitos, e a próxima etapa é criar uma classe de entidade para funcionar como uma tabela de junção sem carga.</span><span class="sxs-lookup"><span data-stu-id="4f018-288">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="4f018-289">(EF 6. x oferece suporte a tabelas de junção implícita para relações muitos-para-muitos, mas Core EF não.</span><span class="sxs-lookup"><span data-stu-id="4f018-289">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="4f018-290">Para obter mais informações, consulte o [discussão no repositório do GitHub de núcleo EF](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="4f018-290">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="4f018-291">A entidade CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="4f018-291">The CourseAssignment entity</span></span>

![Entidade CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="4f018-293">Criar *Models/CourseAssignment.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f018-293">Create *Models/CourseAssignment.cs* with the following code:</span></span>

<span data-ttu-id="4f018-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="4f018-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span></span>

### <a name="join-entity-names"></a><span data-ttu-id="4f018-295">Associar nomes de entidade</span><span class="sxs-lookup"><span data-stu-id="4f018-295">Join entity names</span></span>

<span data-ttu-id="4f018-296">Uma tabela de junção é necessária no banco de dados para a relação de muitos-para-muitos instrutor para cursos, e ele deve ser representado por um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="4f018-296">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="4f018-297">É comum para nomear uma entidade de junção `EntityName1EntityName2`, que nesse caso seria `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="4f018-297">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="4f018-298">No entanto, recomendamos que você escolha um nome que descreve a relação.</span><span class="sxs-lookup"><span data-stu-id="4f018-298">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="4f018-299">Modelos de dados começam simples e crescem, com junções de carga não obtendo frequentemente cargas mais tarde.</span><span class="sxs-lookup"><span data-stu-id="4f018-299">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="4f018-300">Se você iniciar com um nome descritivo de entidade, você não precisará alterar o nome posteriormente.</span><span class="sxs-lookup"><span data-stu-id="4f018-300">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="4f018-301">Idealmente, a entidade de junção terá seu próprio nome (possivelmente única palavra) natural no domínio da empresa.</span><span class="sxs-lookup"><span data-stu-id="4f018-301">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="4f018-302">Por exemplo, manuais e clientes podem ser vinculados por meio de classificações.</span><span class="sxs-lookup"><span data-stu-id="4f018-302">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="4f018-303">Para essa relação, `CourseAssignment` é uma escolha melhor do que `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="4f018-303">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="4f018-304">Chave composta</span><span class="sxs-lookup"><span data-stu-id="4f018-304">Composite key</span></span>

<span data-ttu-id="4f018-305">Como as chaves estrangeiras não são anuláveis e juntos exclusivamente identificar cada linha da tabela, não é necessário para uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="4f018-305">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="4f018-306">O *InstructorID* e *CourseID* propriedades devem funcionar como uma chave primária composta.</span><span class="sxs-lookup"><span data-stu-id="4f018-306">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="4f018-307">É a única maneira de identificar as chaves primárias compostas ao EF usando o *API fluente* (ele não pode ser feito por meio de atributos).</span><span class="sxs-lookup"><span data-stu-id="4f018-307">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="4f018-308">Você verá como configurar a chave primária composta na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="4f018-308">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="4f018-309">A chave composta garante que, embora você possa ter várias linhas para um curso e várias linhas para um instrutor, não pode ter várias linhas para o mesmo instrutor e um curso.</span><span class="sxs-lookup"><span data-stu-id="4f018-309">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="4f018-310">O `Enrollment` junção entidade define sua própria chave primária, portanto, duplicatas desse tipo são possíveis.</span><span class="sxs-lookup"><span data-stu-id="4f018-310">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="4f018-311">Para evitar essas duplicatas, você pode adicionar um índice exclusivo nos campos de chave estrangeiras ou configurar `Enrollment` com uma chave primária composta semelhante ao `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4f018-311">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="4f018-312">Para obter mais informações, consulte [índices](https://docs.efproject.net/en/latest/modeling/indexes.html).</span><span class="sxs-lookup"><span data-stu-id="4f018-312">For more information, see [Indexes](https://docs.efproject.net/en/latest/modeling/indexes.html).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="4f018-313">Atualizar o contexto do banco de dados</span><span class="sxs-lookup"><span data-stu-id="4f018-313">Update the database context</span></span>

<span data-ttu-id="4f018-314">Adicione o seguinte código realçado para o *Data/SchoolContext.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="4f018-314">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

<span data-ttu-id="4f018-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span><span class="sxs-lookup"><span data-stu-id="4f018-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span></span>

<span data-ttu-id="4f018-316">Esse código adiciona novas entidades e configura a chave primária composta de CourseAssignment da entidade.</span><span class="sxs-lookup"><span data-stu-id="4f018-316">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="4f018-317">Alternativa de API fluente para atributos</span><span class="sxs-lookup"><span data-stu-id="4f018-317">Fluent API alternative to attributes</span></span>

<span data-ttu-id="4f018-318">O código de `OnModelCreating` método do `DbContext` classe usa a *API fluente* para configurar o comportamento EF.</span><span class="sxs-lookup"><span data-stu-id="4f018-318">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="4f018-319">A API é chamada "fluente" porque ele geralmente é usado pelo encadeamento uma série de chamadas de método juntas em uma única instrução, como neste exemplo da [documentação principal do EF](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="4f018-319">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="4f018-320">Neste tutorial, você está usando a API fluente somente para mapeamento de banco de dados que você não pode fazer com os atributos.</span><span class="sxs-lookup"><span data-stu-id="4f018-320">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="4f018-321">No entanto, você também pode usar a API fluente para especificar a maioria da formatação, validação e regras de mapeamento que você pode fazer por meio de atributos.</span><span class="sxs-lookup"><span data-stu-id="4f018-321">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="4f018-322">Alguns atributos como `MinimumLength` não pode ser aplicado com a API fluente.</span><span class="sxs-lookup"><span data-stu-id="4f018-322">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="4f018-323">Conforme mencionado anteriormente, `MinimumLength` não altera o esquema, ela só se aplica a uma regra de validação do cliente e servidor.</span><span class="sxs-lookup"><span data-stu-id="4f018-323">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="4f018-324">Alguns desenvolvedores preferem usar a API fluente exclusivamente para que eles podem manter suas classes de entidade "normal".</span><span class="sxs-lookup"><span data-stu-id="4f018-324">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="4f018-325">Você pode misturar atributos e API fluente se desejar, e há algumas personalizações que só podem ser feitas usando a API fluente, mas em geral a prática recomendada é escolher uma destas duas abordagens e usar ou consistentemente tanto quanto possível.</span><span class="sxs-lookup"><span data-stu-id="4f018-325">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="4f018-326">Se você usar ambos, observe que sempre que houver um conflito, a API fluente substitui atributos.</span><span class="sxs-lookup"><span data-stu-id="4f018-326">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="4f018-327">Para obter mais informações sobre atributos versus API fluente, consulte [métodos de configuração](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="4f018-327">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="4f018-328">Relações de exibição de diagrama de entidade</span><span class="sxs-lookup"><span data-stu-id="4f018-328">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="4f018-329">A ilustração a seguir mostra o diagrama que cria o Entity Framework Power Tools para o modelo de escola concluído.</span><span class="sxs-lookup"><span data-stu-id="4f018-329">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagrama de entidade](complex-data-model/_static/diagram.png)

<span data-ttu-id="4f018-331">Além de linhas de relação um-para-muitos (1 a \*), você pode ver a linha de relação um-para-zero-ou-um (1 para entre 0 e 1) entre as entidades instrutor e OfficeAssignment e a linha de relação de zero-ou-um-para-muitos (entre 0 e 1 para *) entre o Entidades instrutor e departamento.</span><span class="sxs-lookup"><span data-stu-id="4f018-331">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="4f018-332">Preencha o banco de dados de teste</span><span class="sxs-lookup"><span data-stu-id="4f018-332">Seed the Database with Test Data</span></span>

<span data-ttu-id="4f018-333">Substitua o código no *Data/DbInitializer.cs* arquivo com o código a seguir para fornecer dados de propagação de novas entidades que você criou.</span><span class="sxs-lookup"><span data-stu-id="4f018-333">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

<span data-ttu-id="4f018-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span><span class="sxs-lookup"><span data-stu-id="4f018-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span></span>

<span data-ttu-id="4f018-335">Como você viu no primeiro tutorial, a maioria do código simplesmente cria novos objetos de entidade e carrega dados de exemplo em propriedades conforme necessário para teste.</span><span class="sxs-lookup"><span data-stu-id="4f018-335">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="4f018-336">Observe como as relações muitos-para-muitos são tratadas: o código cria relações Criando entidades a `Enrollments` e `CourseAssignment` ingressar em conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="4f018-336">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="4f018-337">Adicionar uma migração</span><span class="sxs-lookup"><span data-stu-id="4f018-337">Add a migration</span></span>

<span data-ttu-id="4f018-338">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="4f018-338">Save your changes and build the project.</span></span> <span data-ttu-id="4f018-339">Em seguida, abra a janela de comando na pasta do projeto e insira o `migrations add` comando (não fazer o comando de atualização de banco de dados ainda):</span><span class="sxs-lookup"><span data-stu-id="4f018-339">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="4f018-340">Você receber um aviso sobre a possível perda de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-340">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="4f018-341">Se você tentou executar o `database update` neste ponto de comando (não faça isso ainda), você deverá obter o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="4f018-341">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="4f018-342">A instrução ALTER TABLE em conflito com a restrição de chave estrangeira "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="4f018-342">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="4f018-343">O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Departamento", coluna 'DepartmentID'.</span><span class="sxs-lookup"><span data-stu-id="4f018-343">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="4f018-344">Às vezes, quando você executar migrações com dados existentes, você precisa inserir dados stub para o banco de dados para atender as restrições de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="4f018-344">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="4f018-345">O código gerado o `Up` método adiciona uma chave estrangeira de DepartmentID não anulável para a tabela de curso.</span><span class="sxs-lookup"><span data-stu-id="4f018-345">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="4f018-346">Se já houver linhas na tabela Course quando o código é executado, o `AddColumn` Falha na operação porque o SQL Server não sabe qual valor para colocar na coluna que não pode ser nula.</span><span class="sxs-lookup"><span data-stu-id="4f018-346">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="4f018-347">Para este tutorial, você executará a migração no novo banco de dados, mas em um aplicativo de produção você precisa fazer a migração de manipular os dados existentes, portanto as instruções a seguir mostram um exemplo de como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="4f018-347">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="4f018-348">Para fazer a migração trabalhar com dados existentes, que você precisa alterar o código para fornecer a nova coluna de um valor padrão e criar um departamento de stub chamado "Temp" para agir como o departamento de padrão.</span><span class="sxs-lookup"><span data-stu-id="4f018-348">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="4f018-349">Como resultado, existente linhas curso serão todas relacionadas ao departamento "Temp" após o `Up` execuções do método.</span><span class="sxs-lookup"><span data-stu-id="4f018-349">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="4f018-350">Abra o *{timestamp}_ComplexDataModel.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="4f018-350">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="4f018-351">Comentar a linha de código que adiciona a coluna de ID do departamento na tabela de curso.</span><span class="sxs-lookup"><span data-stu-id="4f018-351">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  <span data-ttu-id="4f018-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="4f018-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span></span>

* <span data-ttu-id="4f018-353">Adicione o seguinte código após o código que cria a tabela de departamento:</span><span class="sxs-lookup"><span data-stu-id="4f018-353">Add the following highlighted code after the code that creates the Department table:</span></span>

  <span data-ttu-id="4f018-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="4f018-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="4f018-355">Em um aplicativo de produção, você escreve código ou scripts para adicionar linhas de departamento e relacionar linhas curso para as novas linhas de departamento.</span><span class="sxs-lookup"><span data-stu-id="4f018-355">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="4f018-356">Você deve, em seguida, não precisar mais do departamento de "Temp" ou o valor padrão na coluna Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="4f018-356">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="4f018-357">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="4f018-357">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="4f018-358">Altere a cadeia de caracteres de conexão e atualize o banco de dados</span><span class="sxs-lookup"><span data-stu-id="4f018-358">Change the connection string and update the database</span></span>

<span data-ttu-id="4f018-359">Agora você tem novo código `DbInitializer` classe que adiciona dados de propagação de novas entidades para um banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="4f018-359">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="4f018-360">Para fazer com que o EF criar um novo banco de dados vazio, altere o nome do banco de dados na cadeia de conexão *appSettings. JSON* ContosoUniversity3 ou algum outro nome que você ainda não usado no computador que você está usando.</span><span class="sxs-lookup"><span data-stu-id="4f018-360">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="4f018-361">Salvar as alterações para *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4f018-361">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="4f018-362">Como alternativa para alterar o nome do banco de dados, você pode excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-362">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="4f018-363">Use **Pesquisador de objetos do SQL Server** (SSOX) ou o `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="4f018-363">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="4f018-364">Depois que você tiver alterado o nome do banco de dados ou excluir o banco de dados, execute o `database update` comando na janela de comando para executar as migrações.</span><span class="sxs-lookup"><span data-stu-id="4f018-364">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="4f018-365">Executar o aplicativo para fazer com que o `DbInitializer.Initialize` método para executar e popular o novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-365">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="4f018-366">Abra o banco de dados em SSOX como você fez anteriormente e expanda o **tabelas** nó para ver se todas as tabelas foram criadas.</span><span class="sxs-lookup"><span data-stu-id="4f018-366">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="4f018-367">(Se você ainda tiver SSOX abrir da hora anterior, clique no botão de atualização.)</span><span class="sxs-lookup"><span data-stu-id="4f018-367">(If you still have SSOX open from the earlier time, click the Refresh button.)</span></span>

![Tabelas no SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="4f018-369">Execute o aplicativo para disparar o código de inicializador que propaga o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f018-369">Run the application to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="4f018-370">Clique com botão direito do **CourseAssignment** de tabela e selecione **exibir dados** para verificar se ele tem dados nele.</span><span class="sxs-lookup"><span data-stu-id="4f018-370">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Dados CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="4f018-372">Resumo</span><span class="sxs-lookup"><span data-stu-id="4f018-372">Summary</span></span>

<span data-ttu-id="4f018-373">Agora você tem um modelo de dados mais complexo e o banco de dados correspondente.</span><span class="sxs-lookup"><span data-stu-id="4f018-373">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="4f018-374">O tutorial a seguir, você aprenderá mais sobre como acessar os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="4f018-374">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4f018-375">[Anterior](migrations.md)
[Próximo](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="4f018-375">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
