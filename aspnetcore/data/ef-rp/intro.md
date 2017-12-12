---
title: "Páginas Razor com o Entity Framework Core - 1 Tutorial de 8"
author: rick-anderson
description: "Mostra como criar um aplicativo de páginas Razor usando o Entity Framework Core"
keywords: Tutorial do ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: d3bcf9aaf7fa809825a0ba8631ee52d3860b090d
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Guia de Introdução com páginas Razor e o Entity Framework Core usando o Visual Studio (1 a 8)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo de web de exemplo Contoso University demonstra como criar aplicativos de web MVC do ASP.NET Core 2.0 usando o núcleo do Entity Framework (EF) 2.0 e o Visual Studio de 2017.

O aplicativo de exemplo é um site de uma universidade Contoso fictícia. Ele inclui a funcionalidade como admissão do aluno, criação de curso e atribuições do instrutor. Esta página é o primeiro em uma série de tutoriais que explicam como criar o aplicativo de exemplo Contoso University.

[Baixar ou exibir o aplicativo concluído.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instruções de download](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>Solução de problemas

Se você tiver um problema que não é possível resolver, você pode encontrar a solução geralmente comparando seu código para o [concluída estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) ou [projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final). Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção de solução de problemas do tutorial do último na série](xref:data/ef-mvc/advanced#common-errors). Se você não encontrar o que você precisa lá, você pode postar uma pergunta em StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Esta série de tutoriais amplia o que é feito nos tutoriais anteriores. Considere a possibilidade de salvar uma cópia do projeto após cada tutorial foi bem-sucedida. Se você tiver problemas, você poderá começar desde o tutorial anterior em vez de voltar ao início. Como alternativa, você pode baixar um [concluída estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e inicie novamente usando o estágio concluído.

## <a name="the-contoso-university-web-app"></a>O aplicativo web Contoso University

O aplicativo compilado nos tutoriais é um site university básico.

Os usuários podem exibir e atualizar aluno, curso e informações do instrutor. Aqui estão algumas das telas do criado no tutorial.

![Página de índice de alunos](intro/_static/students-index.png)

![Página de edição de alunos](intro/_static/student-edit.png)

O estilo de interface do usuário deste site é quase o que é gerado pelos modelos internos. É o foco de tutorial no EF Core com páginas Razor, não a interface do usuário.

## <a name="create-a-razor-pages-web-app"></a>Criar um aplicativo da web de páginas Razor

* No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.
* Crie um novo Aplicativo Web ASP.NET Core. Nomeie o projeto **ContosoUniversity**. É importante para o nome do projeto *ContosoUniversity* para os namespaces corresponda ao código é copiar/colar.
 ![novo aplicativo Web ASP.NET Core](intro/_static/np.png)
* Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.
 ![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)

Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador

## <a name="set-up-the-site-style"></a>Definir o estilo de site

Algumas alterações de configurar o menu de site, o layout e a página inicial.

Abra *Pages/_Layout.cshtml* e faça as seguintes alterações:

* Alterar cada ocorrência de "ContosoUniversity" para "Contoso University." Há três ocorrências.

* Adicionar entradas de menu de **alunos**, **cursos**, **instrutores**, e **departamentos**e exclua o **contato** entrada de menu.

As alterações são realçadas.

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-39,47&range=1-50)]

Em *Views/Home/Index.cshtml*, substitua o conteúdo do arquivo com o código a seguir para substituir o texto sobre o ASP.NET e MVC com o texto sobre este aplicativo:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Pressione CTRL+F5 para executar o projeto. A home page é exibida com as guias criadas nos tutoriais a seguir:

![Home page do Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Criar o modelo de dados

Crie classes de entidade para o aplicativo da Contoso University. Comece com as três seguintes entidades:

![Diagrama de modelo de dados do aluno de registro de curso](intro/_static/data-model-diagram.png)

Há uma relação um-para-muitos entre `Student` e `Enrollment` entidades. Há uma relação um-para-muitos entre `Course` e `Enrollment` entidades. Um aluno pode registrar qualquer número de cursos. Um curso pode ter qualquer número de alunos registrados nele.

As seções a seguir, uma classe para cada uma dessas entidades é criada.

### <a name="the-student-entity"></a>A entidade do aluno

![Diagrama de entidade do aluno](intro/_static/student-entity.png)

Criar um *modelos* pasta. No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

O `ID` propriedade torna-se a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o núcleo de EF interpreta uma propriedade denominada `ID` ou `classnameID` como a chave primária.

O `Enrollments` propriedade é uma propriedade de navegação. Link de propriedades de navegação para outras entidades que estão relacionados a esta entidade. Nesse caso, o `Enrollments` propriedade de um `Student entity` mantém todos o `Enrollment` entidades relacionadas ao `Student`. Por exemplo, se uma linha de Student no banco de dados tem dois relacionados linhas de registro, o `Enrollments` propriedade de navegação contém dois `Enrollment` entidades. Um relacionados `Enrollment` linha é uma linha que contém o valor de chave primária que student no `StudentID` coluna. Por exemplo, suponha que o aluno com ID = 1 tem duas linhas `Enrollment` tabela. O `Enrollment` tabela tem duas linhas com `StudentID` = 1. `StudentID`é uma chave estrangeira no `Enrollment` tabela que especifica o aluno no `Student` tabela.

Se uma propriedade de navegação pode conter várias entidades, a propriedade de navegação deve ser um tipo de lista, como `ICollection<T>`. `ICollection<T>`pode ser especificado, ou um tipo, como `List<T>` ou `HashSet<T>`. Quando `ICollection<T>` é usada, o núcleo do EF cria um `HashSet<T>` coleção por padrão. Propriedades de navegação que contêm várias entidades provenientes de relações muitos-para-muitos e um-para-muitos.

### <a name="the-enrollment-entity"></a>A entidade de registro

![Diagrama de entidade do registro](intro/_static/enrollment-entity.png)

No *modelos* pasta, criar *Enrollment.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

O `EnrollmentID` propriedade é a chave primária. Esta entidade usa o `classnameID` padrão em vez de `ID` como o `Student` entidade. Normalmente, os desenvolvedores escolher um padrão e usá-lo em todo o modelo de dados. Um tutorial posterior, usando uma ID sem classname é mostrado para tornar mais fácil de implementar a herança no modelo de dados.

O `Grade` propriedade é um `enum`. O ponto de interrogação após o `Grade` declaração de tipo indica que o `Grade` propriedade é anulável. Uma classificação que é null é diferente de uma classificação zero - nulo significa uma série não é conhecida ou ainda não foi atribuída.

O `StudentID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Um `Enrollment` entidade está associada um `Student` entidade, portanto, a propriedade contém um único `Student` entidade. O `Student` entidade difere do `Student.Enrollments` propriedade de navegação, que contém várias `Enrollment` entidades.

O `CourseID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Um `Enrollment` entidade está associada um `Course` entidade.

EF Core interpreta uma propriedade como uma chave estrangeira, se ele é nomeado `<navigation property name><primary key property name>`. Por exemplo,`StudentID` para o `Student` propriedade de navegação, como o `Student` chave primária da entidade é `ID`. Propriedades de chave estrangeira também podem ser nomeadas `<primary key property name>`. Por exemplo, `CourseID` desde o `Course` chave primária da entidade é `CourseID`.

### <a name="the-course-entity"></a>A entidade de curso

![Diagrama de entidades de curso](intro/_static/course-entity.png)

No *modelos* pasta, criar *Course.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

O `Enrollments` propriedade é uma propriedade de navegação. Um `Course` entidade pode estar relacionada a qualquer número de `Enrollment` entidades.

O `DatabaseGenerated` atributo permite que o aplicativo especificar a chave primária em vez de ter o banco de dados gerá-lo.

## <a name="create-the-schoolcontext-db-context"></a>Criar o contexto de banco de dados SchoolContext

A classe principal que coordena a funcionalidade principal do EF de um modelo de dados é a classe de contexto de banco de dados. O contexto de dados é derivado de `Microsoft.EntityFrameworkCore.DbContext`. O contexto de dados especifica quais entidades são incluídas no modelo de dados. Neste projeto, a classe é nomeada `SchoolContext`.

Na pasta do projeto, crie uma pasta chamada *dados*.

No *dados* criar pasta *SchoolContext.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Esse código cria um `DbSet` propriedade para cada conjunto de entidades. Na terminologia de EF principais:

* Uma entidade definida normalmente corresponde a uma tabela de banco de dados.
* Uma entidade corresponde a uma linha na tabela.

`DbSet<Enrollment>`e `DbSet<Course>` pode ser omitido. EF Core inclui-los ou implicitamente porque o `Student` referências de entidade de `Enrollment` entidade e o `Enrollment` referências de entidade o `Course` entidade. Para este tutorial, mantenha `DbSet<Enrollment>` e `DbSet<Course>` no `SchoolContext`.

Quando o banco de dados é criado, Core EF cria tabelas com nomes iguais a `DbSet` nomes de propriedade. Nomes de propriedade para coleções são normalmente plurais (alunos em vez de aluno). Os desenvolvedores não concordo sobre se os nomes de tabela devem ser plural. Para esses tutoriais, o comportamento padrão é substituído pela especificação de nomes de tabela única no DbContext. Para especificar nomes de tabela única, adicione o seguinte código:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrar o contexto de injeção de dependência

Inclui o ASP.NET Core [injeção de dependência](xref:fundamentals/dependency-injection). Serviços (como o contexto de DB de núcleo EF) são registrados com injeção de dependência durante a inicialização do aplicativo. Componentes que exigem que esses serviços (como páginas Razor) são fornecidas esses serviços por meio de parâmetros do construtor. O código de construtor que obtém uma instância de contexto do banco de dados é mostrado posteriormente no tutorial.

Para registrar `SchoolContext` como um serviço, abra *Startup.cs*e adicione as linhas destacadas para o `ConfigureServices` método.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

O nome da cadeia de caracteres de conexão é passado para o contexto chamando um método em um `DbContextOptionsBuilder` objeto. Para desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de caracteres de conexão a *appSettings. JSON* arquivo.

Adicionar `using` instruções para `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` namespaces. Compile o projeto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Abra o *appSettings. JSON* de arquivos e adicionar uma cadeia de caracteres de conexão, conforme mostrado no código a seguir:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

A cadeia de caracteres de conexão anterior usa `ConnectRetryCount=0` para evitar [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) de deslocamento.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

A cadeia de caracteres de conexão Especifica um banco de dados do SQL Server LocalDB. LocalDB é uma versão leve do mecanismo de banco de dados do SQL Server Express e destina-se ao desenvolvimento de aplicativos, não o uso de produção. O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa. Por padrão, o LocalDB cria *. mdf* arquivos de banco de dados no `C:/Users/<user>` directory.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Adicione código para inicializar o banco de dados com dados de teste

EF principal cria um banco de dados vazio. Nesta seção, uma *semente* método é gravada para preenchê-la com dados de teste.

No *dados* pasta, crie um novo arquivo de classe chamado *DbInitializer.cs* e adicione o seguinte código:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

O código verifica se há algum dos alunos no banco de dados. Se não houver nenhum alunos no banco de dados, o banco de dados é propagado com dados de teste. Ele carrega os dados de teste em matrizes em vez de `List<T>` coleções para otimizar o desempenho.

O `EnsureCreated` método cria automaticamente o banco de dados para o contexto do banco de dados. Se o banco de dados existir, `EnsureCreated` retorna sem modificar o banco de dados.

Em *Program.cs*, modifique o `Main` método para fazer o seguinte:

* Obtenha uma instância de contexto do banco de dados do contêiner de injeção de dependência.
* Chame o método de propagação, transmitindo a ele o contexto.
* Descarte o contexto quando o método de propagação é concluído.

O código a seguir mostra a atualização *Program.cs* arquivo.

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

A primeira vez que o aplicativo é executado, o banco de dados é criado e propagado com dados de teste. Quando o modelo de dados é atualizado:
* Exclua o banco de dados.
* Atualize o método de propagação.
* Executar o aplicativo e um novo banco de dados de propagação é criado. 

Em tutoriais subsequentes, o banco de dados é atualizado quando os dados de modelo alterações, sem excluir e recriar o banco de dados.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Adicionar ferramentas de scaffold

Nesta seção, o Console de Gerenciador de pacote (PMC) é usado para adicionar o pacote de geração de código do Visual Studio web. Esse pacote é necessário para executar o mecanismo de scaffolding.

No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.

No pacote Manager Console (PMC), digite os seguintes comandos:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

O comando anterior adiciona os pacotes do NuGet para o arquivo *. csproj:

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>O modelo de Scaffold

* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute os seguintes comandos:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
Se o seguinte erro é gerado:

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

Execute o comando novamente e deixar um comentário na parte inferior da página.

Compile o projeto. A compilação gera erros, como o seguinte:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Globalmente altere `_context.Student` para `_context.Students` (ou seja, adicionar um "s" para `Student`). 7 ocorrências forem encontradas e atualizadas. Esperamos que corrigir [esse bug](https://github.com/aspnet/Scaffolding/issues/633) na próxima versão.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Testar o aplicativo

Execute o aplicativo e selecione o **alunos** link. Dependendo da largura do navegador, o **alunos** link aparece na parte superior da página. Se o **alunos** link não estiver visível, clique no ícone de navegação no canto superior direito.

![Página inicial da Contoso University estreita](intro/_static/home-page-narrow.png)

Teste o **criar**, **editar**, e **detalhes** links.

## <a name="view-the-db"></a>Exibir o banco de dados

Quando o aplicativo é iniciado, `DbInitializer.Initialize` chamadas `EnsureCreated`. `EnsureCreated`detecta se o banco de dados existe e cria um, se necessário. Se não houver nenhum alunos no banco de dados, o `Initialize` método adiciona os alunos.

Abra **Pesquisador de objetos do SQL Server** (SSOX) da **exibição** menu do Visual Studio.
No SSOX, clique em **(localdb) \MSSQLLocalDB > bancos de dados > ContosoUniversity1**.

Expanda o **tabelas** nó.

Com o botão direito do **aluno** de tabela e clique em **exibir dados** para ver as colunas criadas e as linhas inseridas na tabela.

O *. mdf* e *. ldf* arquivos de banco de dados estão no *C:\Users\<seu nome de usuário >* pasta.

`EnsureCreated`é chamado no início do aplicativo, que permite que o fluxo de trabalho a seguir:

* Exclua o banco de dados.
* Alterar o esquema de banco de dados (por exemplo, adicionar um `EmailAddress` campo).
* Execute o aplicativo.

`EnsureCreated`cria um banco de dados com o`EmailAddress` coluna.

## <a name="conventions"></a>Convenções

A quantidade de código escrito em ordem para EF principal criar um banco de dados completo é mínima devido ao uso de convenções ou suposições que torna o EF Core.

* Os nomes de `DbSet` propriedades são usadas como nomes de tabela. Para entidades que não é referenciadas por um `DbSet` propriedade, a classe da entidade nomes são usados como nomes de tabela.

* Nomes de propriedade de entidade são usados para nomes de coluna.

* Propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primárias.

* Uma propriedade é interpretada como uma propriedade de chave estrangeira, se ele é nomeado  *<navigation property name> <primary key property name>*  (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` é de chave primária da entidade `ID`). Propriedades de chave estrangeira que podem ser nomeadas  *<primary key property name>*  (por exemplo, `EnrollmentID` desde o `Enrollment` chave primária da entidade é `EnrollmentID`).

Comportamento convencional pode ser substituído. Por exemplo, os nomes de tabela podem ser especificados explicitamente, como mostrado anteriormente neste tutorial. Os nomes de coluna podem ser definidos explicitamente. As chaves primária e estrangeira pode ser definida explicitamente.

## <a name="asynchronous-code"></a>Código assíncrono

Programação assíncrona é o modo padrão do ASP.NET Core e EF Core.

Um servidor web tem um número limitado de threads disponíveis e, em situações de alta carga de todos os threads disponíveis podem estar em uso. Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados. Com código síncrono, muitos threads podem ser vinculados ao enquanto eles não são realmente fazendo qualquer trabalho porque está aguardando para e/s concluir. Com um código assíncrono, quando um processo está aguardando e/s ser concluída, o thread é liberado para o servidor a ser usado para processar outras solicitações. Como resultado, o código assíncrono permite que os recursos de servidor a ser usado com mais eficiência, e o servidor está habilitado para manipular mais tráfego sem atrasos.

Código assíncrono apresentam uma pequena quantidade de sobrecarga em tempo de execução. Para situações de tráfego baixo, o impacto no desempenho é insignificante, durante situações de alto tráfego, a melhoria de desempenho potencial é significativa.

No código a seguir, o `async` palavra-chave, `Task<T>` retornar valor, `await` palavra-chave, e `ToListAsync` método tornar o código executar de forma assíncrona.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* O `async` palavra-chave informa ao compilador para:

  * Gere retornos de chamada para partes do corpo do método.
  * Criar automaticamente o [tarefa](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objeto que é retornado. Para obter mais informações, consulte [o tipo de retorno de tarefa](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Tipo de retorno de implícita `Task` representa um trabalho em andamento.

* O `await` palavra-chave faz com que o compilador dividir o método em duas partes. A primeira parte termina com a operação que é iniciada de forma assíncrona. A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação for concluída.

* `ToListAsync`é a versão assíncrona do `ToList` método de extensão.

Algumas coisas a serem consideradas ao escrever código assíncrono que usa EF Core:

* Somente instruções que fazem com consultas ou comandos sejam enviados para o banco de dados que são executadas assincronamente. Isso inclui, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, e `SaveChangesAsync`. Ele não inclui instruções que apenas alterar uma `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Um contexto de EF Core não é thread safe: não tenta realizar várias operações em paralelo. 

* Para tirar proveito dos benefícios de desempenho do código assíncrono, verifique se pacotes de biblioteca (como paginação) usam async se chamam métodos básicos de EF que enviam consultas ao banco de dados.

Para obter mais informações sobre a programação assíncrona em .NET, consulte [visão geral de Async](https://docs.microsoft.com/dotnet/articles/standard/async).

No tutorial de Avançar, CRUD básica (criar, ler, atualizar e excluir) operações são examinadas.

>[!div class="step-by-step"]
[Avançar](xref:data/ef-rp/crud)
