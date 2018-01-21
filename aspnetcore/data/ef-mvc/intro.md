---
title: "Núcleo do ASP.NET MVC com o Entity Framework Core - Tutorial 1 de 10"
author: tdykstra
description: 
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: df13726689c430ab19786e104ea7404051107aa9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>Introdução ao ASP.NET MVC de núcleo e Entity Framework Core usando o Visual Studio (1 a 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Uma versão de páginas Razor deste tutorial está disponível [aqui](xref:data/ef-rp/intro). A versão das Páginas Razor é mais fácil de seguir e abrange mais recursos de EF. Recomendamos que você siga o [versão páginas Razor deste tutorial](xref:data/ef-rp/intro).

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos de web MVC do ASP.NET Core 2.0 usando o núcleo do Entity Framework (EF) 2.0 e o Visual Studio de 2017.

O aplicativo de exemplo é um site de uma universidade Contoso fictícia. Ele inclui a funcionalidade como admissão do aluno, criação de curso e atribuições do instrutor. Este é o primeiro em uma série de tutoriais que explicam como criar o aplicativo de exemplo Contoso University do zero.

[Baixar ou exibir o aplicativo concluído.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

É a versão mais recente do EF EF Core 2.0, mas ainda não tem todos os recursos do EF 6. x. Para obter informações sobre como escolher entre EF 6. x e EF Core, consulte [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/). Se você escolher EF 6. x, consulte [a versão anterior desta série de tutoriais](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> * Para obter a versão 1.1 do ASP.NET Core deste tutorial, consulte o [versão VS 2017 atualização 2 deste tutorial em formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).
> * Para obter a versão do Visual Studio 2015 deste tutorial, consulte a [Versão do VS 2015 da documentação do ASP.NET Core no formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>Solução de problemas

Se você tiver um problema que não é possível resolver, você pode encontrar a solução geralmente comparando seu código para o [projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção de solução de problemas do tutorial do último na série](advanced.md#common-errors). Se você não encontrar o que você precisa lá, você pode postar uma pergunta em StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP] 
> Essa é uma série de tutoriais de 10, cada um dos quais se baseia no qual é feito nos tutoriais anteriores. Considere a possibilidade de salvar uma cópia do projeto após cada tutorial foi bem-sucedida. Se você tiver problemas, você pode começar novamente do tutorial anterior em vez de voltar ao início da série inteira.

## <a name="the-contoso-university-web-application"></a>O aplicativo da Web da Contoso University

O aplicativo que você criará nos tutoriais é um site simples university.

Os usuários podem exibir e atualizar aluno, curso e informações do instrutor. Aqui estão algumas das telas, você criará.

![Página de índice de alunos](intro/_static/students-index.png)

![Página de edição de alunos](intro/_static/student-edit.png)

O estilo de interface do usuário desse site foi mantido perto o que é gerado pelos modelos internos, para que o tutorial pode se concentrar principalmente sobre como usar o Entity Framework.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Criar um aplicativo ASP.NET MVC de núcleo

Abra o Visual Studio e crie um novo ASP.NET Core c# web chamado "ContosoUniversity".

* Do **arquivo** menu, selecione **Novo > projeto**.

* No painel esquerdo, selecione **instalado > Visual C# > Web**.

* Selecione o modelo de projeto **Aplicativo Web ASP.NET Core**.

* Digite **ContosoUniversity** como o nome e clique em **Okey**.

  ![Caixa de diálogo Novo Projeto](intro/_static/new-project.png)

* Aguarde até que o **ASP.NET Core aplicações Web (.NET Core)** aparecer caixa de diálogo

* Selecione **ASP.NET Core 2.0** e **aplicativo Web (Model-View-Controller)** modelo.

  **Observação:** este tutorial requer o ASP.NET 2.0 de núcleo e Core EF 2.0 ou posterior — Certifique-se de que **1.1 do ASP.NET Core** não estiver selecionada.

* Certifique-se de **autenticação** é definido como **sem autenticação**.

* Clique em **OK**

  ![Caixa de diálogo Novo Projeto ASP.NET](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Definir o estilo de site

Algumas alterações simples configurará o menu de site, o layout e a página inicial.

Abra *Views/Shared/_Layout.cshtml* e faça as seguintes alterações:

* Altere cada ocorrência de "ContosoUniversity" para "Contoso University". Há três ocorrências.

* Adicionar entradas de menu de **alunos**, **cursos**, **instrutores**, e **departamentos**e exclua o **contato** entrada de menu.

As alterações são realçadas.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

Em *Views/Home/Index.cshtml*, substitua o conteúdo do arquivo com o código a seguir para substituir o texto sobre o ASP.NET e MVC com texto sobre este aplicativo:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Pressione CTRL + F5 para executar o projeto ou **Depurar > Start Without Debugging** no menu. Você consulte a home page com guias para as páginas que você criará nos tutoriais.

![Home page do Contoso University](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Pacotes do Entity Framework Core NuGet

Para adicionar suporte EF principal a um projeto, instale o provedor de banco de dados que você deseja como destino. Este tutorial usa o SQL Server e o pacote de provedor é [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Este pacote está incluído no [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, portanto não é necessário instalá-lo.

Este pacote e suas dependências (`Microsoft.EntityFrameworkCore` e `Microsoft.EntityFrameworkCore.Relational`) dão suporte de tempo de execução para o EF. Você adicionará um pacote de ferramentas posteriormente, o [migrações](migrations.md) tutorial. 

Para obter informações sobre outros provedores de banco de dados que estão disponíveis para o Entity Framework Core, consulte [provedores de banco de dados](https://docs.microsoft.com/ef/core/providers/).

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo da Contoso University. Comece com as seguintes três entidades.

![Diagrama de modelo de dados do aluno de registro de curso](intro/_static/data-model-diagram.png)

Há uma relação um-para-muitos entre `Student` e `Enrollment` entidades, e há uma relação um-para-muitos entre `Course` e `Enrollment` entidades. Em outras palavras, um aluno pode ser registrado em qualquer número de cursos e um curso pode ter qualquer número de alunos registrados nele.

As seções a seguir, você criará uma classe para cada uma dessas entidades.

### <a name="the-student-entity"></a>A entidade do aluno

![Diagrama de entidade do aluno](intro/_static/student-entity.png)

No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* e substitua o código de modelo com o código a seguir.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

O `ID` propriedade tornam-se a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o Entity Framework interpreta uma propriedade denominada `ID` ou `classnameID` como a chave primária.

O `Enrollments` propriedade é uma propriedade de navegação. Propriedades de navegação mantêm outras entidades que estão relacionadas a esta entidade. Nesse caso, o `Enrollments` propriedade de um `Student entity` conterá todos os `Enrollment` entidades relacionadas ao `Student` entidade. Em outras palavras, se uma determinada linha de Student no banco de dados tem dois registro linhas relacionadas (linhas que contêm o valor de chave primária que student na sua coluna de chave estrangeira StudentID), que `Student` da entidade `Enrollments` propriedade de navegação conterá os dois `Enrollment` entidades.

Se uma propriedade de navegação pode conter várias entidades (como relações muitos-para-muitos ou um-para-muitos), seu tipo deve ser uma lista na qual as entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection<T>`. Você pode especificar `ICollection<T>` ou um tipo, como `List<T>` ou `HashSet<T>`. Se você especificar `ICollection<T>`, EF cria um `HashSet<T>` coleção por padrão.

### <a name="the-enrollment-entity"></a>A entidade de registro

![Diagrama de entidade do registro](intro/_static/enrollment-entity.png)

No *modelos* pasta, criar *Enrollment.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

O `EnrollmentID` propriedade será a chave primária; esta entidade usa o `classnameID` padrão em vez de `ID` por si só, como você viu no `Student` entidade. Normalmente você deve escolher um padrão e usá-lo em todo o modelo de dados. Aqui, a variação ilustra que você pode usar o padrão. Em um [tutorial posterior](inheritance.md), você verá como usar ID sem classname torna mais fácil implementar a herança no modelo de dados.

O `Grade` propriedade é um `enum`. O ponto de interrogação após o `Grade` declaração de tipo indica que o `Grade` propriedade é anulável. Uma classificação que é null é diferente de uma classificação zero - nulo significa uma série não é conhecida ou ainda não foi atribuída.

O `StudentID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Um `Enrollment` entidade está associada um `Student` entidade, para a propriedade pode conter apenas um único `Student` entidade (ao contrário de `Student.Enrollments` propriedade de navegação que vimos anteriormente, que pode conter vários `Enrollment` entidades).

O `CourseID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Um `Enrollment` entidade está associada um `Course` entidade.

Entity Framework interpreta uma propriedade como uma propriedade de chave estrangeira se ele é nomeado `<navigation property name><primary key property name>` (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira também podem ser nomeadas simplesmente `<primary key property name>` (por exemplo, `CourseID` desde o `Course` chave primária da entidade é `CourseID`).

### <a name="the-course-entity"></a>A entidade de curso

![Diagrama de entidades de curso](intro/_static/course-entity.png)

No *modelos* pasta, criar *Course.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

O `Enrollments` propriedade é uma propriedade de navegação. Um `Course` entidade pode estar relacionada a qualquer número de `Enrollment` entidades.

Podemos dizer mais sobre o `DatabaseGenerated` atributo em uma [tutorial posterior](complex-data-model.md) na série. Basicamente, este atributo permite que você insira a chave primária para o curso, em vez de fazer com que o banco de dados gerá-lo.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena a funcionalidade do Entity Framework para um modelo de dados é a classe de contexto de banco de dados. Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`. No seu código, você especifica quais entidades são incluídas no modelo de dados. Você também pode personalizar o comportamento específico do Entity Framework. Neste projeto, a classe é nomeada `SchoolContext`.

Na pasta do projeto, crie uma pasta chamada *dados*.

No *dados* pasta criar um novo arquivo de classe chamado *SchoolContext.cs*e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Esse código cria um `DbSet` propriedade para cada conjunto de entidades. Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados, enquanto uma entidade corresponde a uma linha na tabela.

Você poderia omitir o `DbSet<Enrollment>` e `DbSet<Course>` instruções e ele seriam funcionam da mesma. O Entity Framework inclui-los implicitamente porque o `Student` referências de entidade de `Enrollment` entidade e o `Enrollment` referências de entidade o `Course` entidade.

Quando o banco de dados é criado, EF cria tabelas com nomes iguais a `DbSet` nomes de propriedade. Nomes de propriedade para coleções são normalmente plural (alunos em vez de estudante), mas os desenvolvedores Concordo sobre se os nomes de tabela devem ser pluralized ou não. Para esses tutoriais, você vai substituir o comportamento padrão especificando nomes de tabela única no DbContext. Para fazer isso, adicione o seguinte código após a última propriedade DbSet.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrar o contexto de injeção de dependência

ASP.NET Core implementa [injeção de dependência](../../fundamentals/dependency-injection.md) por padrão. Serviços (como o contexto de banco de dados do EF) são registrados com injeção de dependência durante a inicialização do aplicativo. Componentes que exigem que esses serviços (como controladores MVC) são fornecidas esses serviços por meio de parâmetros do construtor. Você verá o código de construtor do controlador que obtém uma instância de contexto posteriormente neste tutorial.

Para registrar `SchoolContext` como um serviço, abra *Startup.cs*e adicione as linhas destacadas para o `ConfigureServices` método.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

O nome da cadeia de caracteres de conexão é passado para o contexto chamando um método em um `DbContextOptionsBuilder` objeto. Para desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de caracteres de conexão a *appSettings. JSON* arquivo.

Adicionar `using` instruções para `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` namespaces e, em seguida, compilar o projeto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Abra o *appSettings. JSON* de arquivos e adicionar uma cadeia de caracteres de conexão, conforme mostrado no exemplo a seguir.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

A cadeia de caracteres de conexão Especifica um banco de dados LocalDB do SQL Server. LocalDB é uma versão leve do mecanismo de banco de dados do SQL Server Express e destina-se ao desenvolvimento de aplicativos, não o uso de produção. O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa. Por padrão, o LocalDB cria *. mdf* arquivos de banco de dados de `C:/Users/<user>` directory.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Adicione código para inicializar o banco de dados de teste

O Entity Framework criará um banco de dados vazio para você. Nesta seção, você escreve um método que é chamado depois que o banco de dados é criado para preenchê-la com dados de teste.

Aqui você vai usar o `EnsureCreated` método para criar automaticamente o banco de dados. Em um [tutorial posterior](migrations.md) você verá como lidar com as alterações do modelo usando migrações do Code First para alterar o esquema de banco de dados em vez de descartar e recriar o banco de dados.

No *dados* pasta, crie um novo arquivo de classe chamado *DbInitializer.cs* e substitua o código de modelo com o código a seguir, que faz com que um banco de dados a ser criado quando necessário e carrega dados para a nova de teste banco de dados.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

O código verifica se há algum dos alunos no banco de dados, e se não estiver, ele pressupõe que o banco de dados é novo e precisa ser propagada com dados de teste. Ele carrega os dados de teste em matrizes em vez de `List<T>` coleções para otimizar o desempenho.

Em *Program.cs*, modifique o `Main` método para fazer o seguinte na inicialização do aplicativo:

* Obtenha uma instância de contexto do banco de dados do contêiner de injeção de dependência.
* Chame o método de propagação, transmitindo a ele o contexto.
* Descarte o contexto quando o método de propagação é feito.

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Adicionar `using` instruções:

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

Tutoriais mais antiga, você poderá ver um código semelhante no `Configure` método *Startup.cs*. Recomendamos que você use o `Configure` método apenas para configurar o pipeline de solicitação. Código de inicialização do aplicativo pertence a `Main` método.

Agora a primeira vez que você executar o aplicativo, o banco de dados será criado e propagado com dados de teste. Sempre que você alterar o modelo de dados, excluir o banco de dados, atualize seu método de propagação e iniciar novamente com um novo banco de dados da mesma maneira. Em tutoriais subsequentes, você verá como modificar o banco de dados quando os dados de modelo alterações, sem excluir e recriá-lo.

## <a name="create-a-controller-and-views"></a>Criar um controlador e modos de exibição

Em seguida, você usará o mecanismo de scaffolding no Visual Studio para adicionar um controlador MVC e modos de exibição que usam EF para consultar e salvar dados.

A criação automática de métodos de ação CRUD e modos de exibição é conhecida como scaffolding. Scaffolding difere de geração de código em que o código de scaffolding é um ponto de partida você pode modificar para atender seus requisitos, enquanto que normalmente não modifique o código gerado. Quando você precisar personalizar o código gerado, use classes parciais ou gerar novamente o código quando as coisas mudam.

* Com o botão direito do **controladores** pasta **Solution Explorer** e selecione **Adicionar > Novo Item de Scaffold**.

Se a caixa de diálogo **Adicionar Dependências do MVC** for exibida:

* [Atualize o Visual Studio para a última versão](https://www.visualstudio.com/downloads/). Versões do Visual Studio anteriores a 15.5 mostram essa caixa de diálogo.
* Se não puder atualizar, selecione **ADICIONAR** e, em seguida, siga as etapas de adição do controlador novamente.

* No **adicionar Scaffold** caixa de diálogo:

  * Selecione **controlador MVC com modos de exibição usando o Entity Framework**.

  * Clique em **Adicionar**.

* No **Adicionar controlador** caixa de diálogo:

  * Em **classe modelo** selecione **aluno**.

  * Em **classe de contexto de dados** selecione **SchoolContext**.

  * Aceite o padrão **StudentsController** como o nome.

  * Clique em **Adicionar**.

  ![Scaffold aluno](intro/_static/scaffold-student.png)

  Quando você clica em **adicionar**, o mecanismo de scaffolding do Visual Studio cria um *StudentsController.cs* de arquivo e um conjunto de exibições (*. cshtml* arquivos) que funcionam com o controlador.

(O mecanismo de scaffolding também pode criar o contexto de banco de dados para você se você não criar manualmente primeiro como você fez anteriormente para este tutorial. Você pode especificar uma nova classe de contexto no **Adicionar controlador** clicando no sinal de mais à direita da caixa de **classe de contexto de dados**.  Visual Studio criará seu `DbContext` , bem como o controlador e os modos de exibição de classe.)

Você observará que o controlador leva um `SchoolContext` como um parâmetro de construtor.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

Injeção de dependência do ASP.NET cuidará de passando uma instância do `SchoolContext` no controlador. Você configurou que o *Startup.cs* arquivo anteriormente.

O controlador contém um `Index` método de ação, que exibe todos os alunos no banco de dados. O método obtém uma lista dos alunos da entidade alunos definida pela leitura de `Students` propriedade da instância de contexto do banco de dados:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Você aprenderá sobre os elementos de programação assíncronos nesse código no tutorial posteriormente.

O *Views/Students/Index.cshtml* exibe essa lista em uma tabela:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Pressione CTRL + F5 para executar o projeto ou **Depurar > Start Without Debugging** no menu.

Clique na guia alunos para ver os dados de teste que o `DbInitializer.Initialize` método inserido. Dependendo de como estreita a janela do navegador é, você verá o `Student` link guia na parte superior da página, ou você precisará clique no ícone no canto superior direito para ver o link de navegação.

![Página inicial da Contoso University estreita](intro/_static/home-page-narrow.png)

![Página de índice de alunos](intro/_static/students-index.png)

## <a name="view-the-database"></a>Exibir o banco de dados

Quando você iniciou o aplicativo, o `DbInitializer.Initialize` chamadas de método `EnsureCreated`. EF viu que não houve nenhum banco de dados e, portanto ele criado um, e o restante do `Initialize` código do método populado o banco de dados. Você pode usar **Pesquisador de objetos do SQL Server** (SSOX) para exibir o banco de dados no Visual Studio.

Feche o navegador.

Se a janela SSOX não estiver aberta, selecionar a partir de **exibição** menu do Visual Studio.

No SSOX, clique em **(localdb) \MSSQLLocalDB > bancos de dados**e, em seguida, clique na entrada para o nome do banco de dados que está na cadeia de conexão em seu *appSettings. JSON* arquivo.

Expanda o **tabelas** nó para ver as tabelas no banco de dados.

![Tabelas no SSOX](intro/_static/ssox-tables.png)

Com o botão direito do **aluno** de tabela e clique em **exibir dados** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

![Tabela de alunos em SSOX](intro/_static/ssox-student-table.png)

O *. mdf* e *. ldf* arquivos de banco de dados estão no *C:\Users\<seu nome de usuário >* pasta.

Porque você está chamando `EnsureCreated` no método inicializador que é executado no início do aplicativo, você pode fazer uma alteração de `Student` classe, excluir o banco de dados, executar novamente o aplicativo e o banco de dados será automaticamente ser recriado para corresponder a alteração. Por exemplo, se você adicionar um `EmailAddress` propriedade para o `Student` classe, você verá um novo `EmailAddress` coluna na tabela recriada.

## <a name="conventions"></a>Convenções

A quantidade de código, que você precisava criar para que o Entity Framework para poder criar um banco de dados completo para você é mínima devido ao uso de convenções ou suposições que faz com que o Entity Framework.

* Os nomes de `DbSet` propriedades são usadas como nomes de tabela. Para entidades que não é referenciadas por um `DbSet` propriedade, a classe da entidade nomes são usados como nomes de tabela.

* Nomes de propriedade de entidade são usados para nomes de coluna.

* Propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primárias.

* Uma propriedade é interpretada como uma propriedade de chave estrangeira, se ele é nomeado  *<navigation property name> <primary key property name>*  (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` é de chave primária da entidade `ID`). Propriedades de chave estrangeira também podem ser nomeadas simplesmente  *<primary key property name>*  (por exemplo, `EnrollmentID` desde o `Enrollment` chave primária da entidade é `EnrollmentID`).

Comportamento convencional pode ser substituído. Por exemplo, você pode especificar explicitamente os nomes de tabela, visto anteriormente neste tutorial. E você pode definir os nomes de coluna e definir qualquer propriedade de primary key ou foreign key como você verá em um [tutorial posterior](complex-data-model.md) na série.

## <a name="asynchronous-code"></a>Código assíncrono

Programação assíncrona é o modo padrão do ASP.NET Core e EF Core.

Um servidor web tem um número limitado de threads disponíveis e, em situações de alta carga de todos os threads disponíveis podem estar em uso. Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados. Com código síncrono, muitos threads podem ser vinculados ao enquanto eles não são realmente fazendo qualquer trabalho porque está aguardando para e/s concluir. Com um código assíncrono, quando um processo está aguardando e/s ser concluída, o thread é liberado para o servidor a ser usado para processar outras solicitações. Como resultado, o código assíncrono permite que os recursos de servidor a ser usado com mais eficiência, e o servidor está habilitado para manipular mais tráfego sem atrasos.

Código assíncrono apresente uma pequena quantidade de sobrecarga em tempo de execução, mas para situações de pouco tráfego o impacto no desempenho é insignificante, durante situações de alto tráfego, a melhoria de desempenho potencial é significativa.

No código a seguir, o `async` palavra-chave, `Task<T>` retornar valor, `await` palavra-chave, e `ToListAsync` método tornar o código executar de forma assíncrona.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* O `async` palavra-chave informa ao compilador para gerar retornos de chamada para partes do corpo do método e criar automaticamente o `Task<IActionResult>` objeto que é retornado.

* O tipo de retorno `Task<IActionResult>` representa um trabalho contínuo com um resultado do tipo `IActionResult`.

* O `await` palavra-chave faz com que o compilador dividir o método em duas partes. A primeira parte termina com a operação que é iniciada de forma assíncrona. A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação for concluída.

* `ToListAsync`é a versão assíncrona do `ToList` método de extensão.

Algumas coisas a serem consideradas quando você está escrevendo código assíncrono que usa o Entity Framework:

* Somente instruções que fazem com consultas ou comandos sejam enviados para o banco de dados que são executadas assincronamente. Por exemplo, que inclui `ToListAsync`, `SingleOrDefaultAsync`, e `SaveChangesAsync`. Ele não incluir, por exemplo, instruções que apenas alterar uma `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Um contexto EF não é thread-safe: não tenta realizar várias operações em paralelo. Quando você chamar qualquer método EF assíncrona, sempre use o `await` palavra-chave.

* Se você quiser tirar proveito dos benefícios de desempenho do código assíncrono, certifique-se de que qualquer biblioteca de pacotes que você está usando (por exemplo, para paginação), use também assíncrona se ele chamam qualquer método de Entity Framework que causam consultas sejam enviadas para o banco de dados.

Para obter mais informações sobre a programação assíncrona em .NET, consulte [visão geral de Async](https://docs.microsoft.com/dotnet/articles/standard/async).

## <a name="summary"></a>Resumo

Agora você criou um aplicativo simples que usa o Entity Framework Core e o SQL Server Express LocalDB para armazenar e exibir dados. O tutorial a seguir, você aprenderá executar básica CRUD (criar, ler, atualizar e excluir) operações.

>[!div class="step-by-step"]
[Avançar](crud.md)
