---
title: Páginas Razor com o Entity Framework Core no ASP.NET Core – Tutorial 1 de 8
author: rick-anderson
description: Mostra como criar um aplicativo das Páginas do Razor usando o Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 99a8d158c896566c2f6e6c22e4b37b1956e21cbf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Páginas Razor com o Entity Framework Core no ASP.NET Core – Tutorial 1 de 8

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo Web de exemplo Contoso University demonstra como criar aplicativos Web ASP.NET Core 2.0 MVC usando o EF (Entity Framework) Core 2.0 e o Visual Studio 2017.

O aplicativo de exemplo é um site de uma Contoso University fictícia. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor. Esta página é a primeira de uma série de tutoriais que explica como criar o aplicativo de exemplo Contoso University.

[Baixe ou exiba o aplicativo concluído.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instruções de download](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [](~/includes/net-core-prereqs.md)]

Familiaridade com as [Páginas do Razor](xref:mvc/razor-pages/index). Os novos programadores devem concluir a [Introdução às Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start) antes de começar esta série.

## <a name="troubleshooting"></a>Solução de problemas

Se você tem algum problema que não consegue resolver, geralmente é possível encontrar a solução comparando seu código com o [estágio concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots). Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção Solução de problemas do último tutorial da série](xref:data/ef-mvc/advanced#common-errors). Caso não encontre o que precisa na seção, poste uma pergunta no [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) sobre o [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Esta série de tutoriais baseia-se no que foi feito nos tutoriais anteriores. Considere a possibilidade de salvar uma cópia do projeto após a conclusão bem-sucedida de cada tutorial. Caso tenha problemas, comece novamente no tutorial anterior em vez de voltar ao início. Como alternativa, você pode baixar um [estágio concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e começar novamente usando o estágio concluído.

## <a name="the-contoso-university-web-app"></a>O aplicativo Web Contoso University

O aplicativo criado nesses tutoriais é um site básico de universidade.

Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores. Veja a seguir algumas das telas criadas no tutorial.

![Página Índice de Alunos](intro/_static/students-index.png)

![Página Editar Alunos](intro/_static/student-edit.png)

O estilo de interface do usuário deste site é próximo ao que é gerado pelos modelos internos. O foco do tutorial é o EF Core com as Páginas do Razor, não a interface do usuário.

## <a name="create-a-razor-pages-web-app"></a>Criar um aplicativo Web das Páginas do Razor

* No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.
* Crie um novo Aplicativo Web ASP.NET Core. Nomeie o projeto **ContosoUniversity**. É importante nomear o projeto *ContosoUniversity* para que os namespaces sejam correspondentes quando o código for copiado/colado.
 ![novo aplicativo Web ASP.NET Core](intro/_static/np.png)
* Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.
 ![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)

Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador

## <a name="set-up-the-site-style"></a>Configurar o estilo do site

Algumas alterações configuram o menu do site, o layout e a home page.

Abra *Pages/_Layout.cshtml* e faça as seguintes alterações:

* Altere cada ocorrência de "ContosoUniversity" para "Contoso University". Há três ocorrências.

* Adicione entradas de menu para **Alunos**, **Cursos**, **Instrutores** e **Departamentos** e exclua a entrada de menu **Contato**.

As alterações são realçadas. (Toda a marcação *não* é exibida.)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

Em *Pages/Index.cshtml*, substitua o conteúdo do arquivo pelo seguinte código para substituir o texto sobre o ASP.NET e MVC pelo texto sobre este aplicativo:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Pressione CTRL+F5 para executar o projeto. A home page é exibida com as guias criadas nos seguintes tutoriais:

![Home page da Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Criar o modelo de dados

Crie classes de entidade para o aplicativo Contoso University. Comece com as três seguintes entidades:

![Diagrama de Modelo de Dados Course-Enrollment-Student](intro/_static/data-model-diagram.png)

Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`. Há uma relação um-para-muitos entre as entidades `Course` e `Enrollment`. Um aluno pode se registrar em qualquer quantidade de cursos. Um curso pode ter qualquer quantidade de alunos registrados.

Nas seções a seguir, é criada uma classe para cada uma dessas entidades.

### <a name="the-student-entity"></a>A entidade Student

![Diagrama da entidade Student](intro/_static/student-entity.png)

Crie uma pasta *Models*. Na pasta *Models*, crie um arquivo de classe chamado *Student.cs* com o seguinte código:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

A propriedade `ID` se torna a coluna de chave primária da tabela de BD (banco de dados) que corresponde a essa classe. Por padrão, o EF Core interpreta uma propriedade nomeada `ID` ou `classnameID` como a chave primária. Em `classnameID`, `classname` é o nome da classe, como `Student` no exemplo anterior.

A propriedade `Enrollments` é uma propriedade de navegação. As propriedades de navegação vinculam-se a outras entidades que estão relacionadas a essa entidade. Nesse caso, a propriedade `Enrollments` de uma `Student entity` armazena todas as entidades `Enrollment` relacionadas a essa `Student`. Por exemplo, se uma linha Aluno no BD tiver duas linhas Registro relacionadas, a propriedade de navegação `Enrollments` conterá duas entidades `Enrollment`. Uma linha `Enrollment` relacionada é uma linha que contém o valor de chave primária do aluno na coluna `StudentID`. Por exemplo, suponha que o aluno com ID=1 tenha duas linhas na tabela `Enrollment`. A tabela `Enrollment` tem duas linhas com `StudentID` = 1. `StudentID` é uma chave estrangeira na tabela `Enrollment` que especifica o aluno na tabela `Student`.

Se uma propriedade de navegação puder armazenar várias entidades, a propriedade de navegação deverá ser um tipo de lista, como `ICollection<T>`. `ICollection<T>` pode ser especificado ou um tipo como `List<T>` ou `HashSet<T>`. Quando `ICollection<T>` é usado, o EF Core cria uma coleção `HashSet<T>` por padrão. As propriedades de navegação que armazenam várias entidades são provenientes de relações muitos para muitos e um-para-muitos.

### <a name="the-enrollment-entity"></a>A entidade Enrollment

![Diagrama da entidade Enrollment](intro/_static/enrollment-entity.png)

Na pasta *Models*, crie *Enrollment.cs* com o seguinte código:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

A propriedade `EnrollmentID` é a chave primária. Essa entidade usa o padrão `classnameID` em vez de `ID` como a entidade `Student`. Normalmente, os desenvolvedores escolhem um padrão e o usam em todo o modelo de dados. Em um tutorial posterior, o uso de uma ID sem nome de classe é mostrado para facilitar a implementação da herança no modelo de dados.

A propriedade `Grade` é um `enum`. O ponto de interrogação após a declaração de tipo `Grade` indica que a propriedade `Grade` permite valor nulo. Uma nota nula é diferente de uma nota zero – nulo significa que uma nota não é conhecida ou que ainda não foi atribuída.

A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Uma entidade `Enrollment` está associada a uma entidade `Student` e, portanto, a propriedade contém uma única entidade `Student`. A entidade `Student` é distinta da propriedade de navegação `Student.Enrollments`, que contém várias entidades `Enrollment`.

A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Uma entidade `Enrollment` está associada a uma entidade `Course`.

O EF Core interpreta uma propriedade como uma chave estrangeira se ela é nomeada `<navigation property name><primary key property name>`. Por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`. Propriedades de chave estrangeira também podem ser nomeadas `<primary key property name>`. Por exemplo, `CourseID`, pois a chave primária da entidade `Course` é `CourseID`.

### <a name="the-course-entity"></a>A entidade Course

![Diagrama de entidade Curso](intro/_static/course-entity.png)

Na pasta *Models*, crie *Course.cs* com o seguinte código:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

A propriedade `Enrollments` é uma propriedade de navegação. Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.

O atributo `DatabaseGenerated` permite que o aplicativo especifique a chave primária em vez de fazer com que ela seja gerada pelo BD.

## <a name="create-the-schoolcontext-db-context"></a>Criar o contexto de BD SchoolContext

A classe principal que coordena a funcionalidade do EF Core de um modelo de dados é a classe de contexto de BD. O contexto de dados é derivado de `Microsoft.EntityFrameworkCore.DbContext`. O contexto de dados especifica quais entidades são incluídas no modelo de dados. Neste projeto, a classe é chamada `SchoolContext`.

Na pasta do projeto, crie uma pasta chamada *Dados*.

Na pasta *Dados*, crie *SchoolContext.cs* com o seguinte código:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Esse código cria uma propriedade `DbSet` para cada conjunto de entidades. Na terminologia do EF Core:

* Um conjunto de entidades normalmente corresponde a uma tabela de BD.
* Uma entidade corresponde a uma linha da tabela.

`DbSet<Enrollment>` e `DbSet<Course>` podem ser omitidos. O EF Core inclui-os de forma implícita porque a entidade `Student` referencia a entidade `Enrollment` e a entidade `Enrollment` referencia a entidade `Course`. Para este tutorial, mantenha `DbSet<Enrollment>` e `DbSet<Course>` no `SchoolContext`.

Quando o BD é criado, o EF Core cria tabelas que têm nomes iguais aos nomes de propriedade `DbSet`. Nomes de propriedade para coleções são normalmente plurais (Students em vez de Student). Os desenvolvedores não concordam sobre se os nomes de tabela devem estar no plural. Para esses tutoriais, o comportamento padrão é substituído pela especificação de nomes singulares de tabela no DbContext. Para especificar nomes singulares de tabela, adicione o seguinte código realçado:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrar o contexto com a injeção de dependência

O ASP.NET Core inclui a [injeção de dependência](xref:fundamentals/dependency-injection). Serviços (como o contexto de BD do EF Core) são registrados com injeção de dependência durante a inicialização do aplicativo. Os componentes que exigem esses serviços (como as Páginas do Razor) recebem esses serviços por meio de parâmetros do construtor. O código de construtor que obtém uma instância de contexto do BD é mostrado mais adiante no tutorial.

Para registrar `SchoolContext` como um serviço, abra *Startup.cs* e adicione as linhas realçadas ao método `ConfigureServices`.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

O nome da cadeia de conexão é passado para o contexto com a chamada de um método em um objeto `DbContextOptionsBuilder`. Para o desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de conexão do arquivo *appsettings.json*.

Adicione instruções `using` aos namespaces `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore`. Compile o projeto.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Abra o arquivo *appsettings.json* e adicione uma cadeia de conexão, conforme mostrado no seguinte código:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

A cadeia de conexão anterior usa `ConnectRetryCount=0` para impedir o travamento do [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

A cadeia de conexão especifica um BD LocalDB do SQL Server. LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express destinado ao desenvolvimento de aplicativos, e não ao uso em produção. O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa. Por padrão, o LocalDB cria arquivos *.mdf* de BD no diretório `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Adicionar um código para inicializar o BD com os dados de teste

O EF Core cria um BD vazio. Nesta seção, um método *Seed* é escrito para populá-lo com os dados de teste.

Na pasta *Dados*, crie um novo arquivo de classe chamado *DbInitializer.cs* e adicione o seguinte código:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

O código verifica se há alunos no BD. Se não houver nenhum aluno no BD, o BD será propagado com os dados de teste. Ele carrega os dados de teste em matrizes em vez de em coleções `List<T>` para otimizar o desempenho.

O método `EnsureCreated` cria o BD automaticamente para o contexto de BD. Se o BD existir, `EnsureCreated` retornará sem modificar o BD.

Em *Program.cs*, modifique o método `Main` para fazer o seguinte:

* Obtenha uma instância de contexto de BD do contêiner de injeção de dependência.
* Chame o método de semente passando a ele o contexto.
* Descarte o contexto quando o método de semente for concluído.

O código a seguir mostra o arquivo *Program.cs* atualizado.

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

A primeira vez que o aplicativo é executado, o BD é criado e propagado com os dados de teste. Quando o modelo de dados é atualizado:
* Exclua o BD.
* Atualize o método de semente.
* Execute o aplicativo e um novo BD propagado será criado. 

Nos próximos tutoriais, o BD é atualizado quando o modelo de dados é alterado, sem excluir nem recriar o BD.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Adicionar ferramentas de scaffolding

Nesta seção, o PMC (Console do Gerenciador de Pacotes) é usado para adicionar o pacote de geração de código da Web do Visual Studio. Esse pacote é necessário para executar o mecanismo de scaffolding.

No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.

No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

O comando anterior adiciona os pacotes NuGet ao arquivo *.csproj:

[!code-csharp[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Gerar o modelo por scaffolding

* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute os seguintes comandos:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

Se você obtiver o erro:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).


Compile o projeto. O build gera erros, como o seguinte:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Altere `_context.Student` globalmente para `_context.Students` (ou seja, adicione um "s" a `Student`). 7 ocorrências foram encontradas e atualizadas. Esperamos corrigir [este bug](https://github.com/aspnet/Scaffolding/issues/633) na próxima versão.

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Testar o aplicativo

Execute o aplicativo e selecione o link **Alunos**. Dependendo da largura do navegador, o link **Alunos** é exibido na parte superior da página. Se o link **Alunos** não estiver visível, clique no ícone de navegação no canto superior direito.

![Home page estreita da Contoso University](intro/_static/home-page-narrow.png)

Teste os links **Criar**, **Editar** e **Detalhes**.

## <a name="view-the-db"></a>Exibir o BD

Quando o aplicativo é iniciado, `DbInitializer.Initialize` chama `EnsureCreated`. `EnsureCreated` detecta se o BD existe e cria um, se necessário. Se não houver nenhum aluno no BD, o método `Initialize` adicionará alunos.

Abra o **SSOX** (Pesquisador de Objetos do SQL Server) no menu **Exibir** do Visual Studio.
No SSOX, clique em **(localdb)\MSSQLLocalDB > Bancos de Dados > ContosoUniversity1**.

Expanda o nó **Tabelas**.

Clique com o botão direito do mouse na tabela **Aluno** e clique em **Exibir Dados** para ver as colunas criadas e as linhas inseridas na tabela.

Os arquivos <em>.mdf</em> e <em>.ldf</em> do BD estão localizados na pasta <em>C:\Users\\<yourusername></em>.

`EnsureCreated` é chamado na inicialização do aplicativo, que permite que o seguinte fluxo de trabalho:

* Exclua o BD.
* Altere o esquema de BD (por exemplo, adicione um campo `EmailAddress`).
* Execute o aplicativo.

`EnsureCreated` cria um BD com a coluna `EmailAddress`.

## <a name="conventions"></a>Convenções

A quantidade de código feita para que o EF Core crie um BD completo é mínima, devido ao uso de convenções ou de suposições feitas pelo EF Core.

* Os nomes de propriedades `DbSet` são usadas como nomes de tabela. Para entidades não referenciadas por uma propriedade `DbSet`, os nomes de classe de entidade são usados como nomes de tabela.

* Os nomes de propriedade de entidade são usados para nomes de coluna.

* As propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primária.

* Uma propriedade é interpretada como uma propriedade de chave estrangeira se ela é nomeada *<navigation property name><primary key property name>* (por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`). As propriedades de chave estrangeira podem ser nomeadas *<primary key property name>* (por exemplo, `EnrollmentID`, pois a chave primária da entidade `Enrollment` é `EnrollmentID`).

O comportamento convencional pode ser substituído. Por exemplo, os nomes de tabela podem ser especificados de forma explícita, conforme mostrado anteriormente neste tutorial. Os nomes de coluna podem ser definidos de forma explícita. As chaves primária e estrangeira podem ser definidas de forma explícita.

## <a name="asynchronous-code"></a>Código assíncrono

A programação assíncrona é o modo padrão do ASP.NET Core e EF Core.

Um servidor Web tem um número limitado de threads disponíveis e, em situações de alta carga, todos os threads disponíveis podem estar em uso. Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados. Com um código síncrono, muitos threads podem ser vinculados enquanto realmente não são fazendo nenhum trabalho porque estão aguardando a conclusão da E/S. Com um código assíncrono, quando um processo está aguardando a conclusão da E/S, seu thread é liberado para o servidor para ser usado para processar outras solicitações. Como resultado, o código assíncrono permite que os recursos do servidor sejam usados com mais eficiência, e o servidor fica capacitado a manipular mais tráfego sem atrasos.

O código assíncrono introduz uma pequena quantidade de sobrecarga em tempo de execução. Para situações de baixo tráfego, o impacto no desempenho é insignificante, enquanto para situações de alto tráfego, a melhoria de desempenho potencial é significativa.

No código a seguir, a palavra-chave `async`, o valor retornado `Task<T>`, a palavra-chave `await` e o método `ToListAsync` fazem o código ser executado de forma assíncrona.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* A palavra-chave `async` instrui o compilador a:

  * Gerar retornos de chamada para partes do corpo do método.
  * Criar automaticamente o objeto [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) que é retornado. Para obter mais informações, consulte [Tipo de retorno de Tarefa](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* O tipo de retorno implícito `Task` representa um trabalho em andamento.

* A palavra-chave `await` faz com que o compilador divida o método em duas partes. A primeira parte termina com a operação que é iniciada de forma assíncrona. A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação é concluída.

* `ToListAsync` é a versão assíncrona do método de extensão `ToList`.

Algumas coisas a serem consideradas ao escrever um código assíncrono que usa o EF Core:

* Somente instruções que fazem com que consultas ou comandos sejam enviados ao BD são executadas de forma assíncrona. Isso inclui `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`. Isso não inclui instruções que apenas alteram um `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Um contexto do EF Core não é thread-safe: não tente realizar várias operações em paralelo. 

* Para aproveitar os benefícios de desempenho do código assíncrono, verifique se os pacotes de biblioteca (como para paginação) usam o código assíncrono se eles chamam métodos do EF Core que enviam consultas ao BD.

Para obter mais informações sobre a programação assíncrona no .NET, consulte [Visão geral da programação assíncrona](/dotnet/articles/standard/async).

No próximo tutorial, as operações CRUD (criar, ler, atualizar e excluir) básicas são examinadas.

> [!div class="step-by-step"]
> [Avançar](xref:data/ef-rp/crud)
