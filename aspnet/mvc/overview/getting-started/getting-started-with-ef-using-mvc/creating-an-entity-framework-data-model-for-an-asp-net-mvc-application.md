---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Introdução ao Entity Framework 6 Code First usando MVC 5 | Microsoft Docs
author: tdykstra
ms.author: riande
ms.date: 12/04/2018
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c7ab9458f83e05af84f72d9a2519a8c1c39b84b5
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861427"
---
# <a name="get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introdução ao Entity Framework 6 Code First usando MVC 5

por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> [!NOTE]
> Para novos desenvolvimentos, recomendamos [páginas do Razor do ASP.NET Core](/aspnet/core/razor-pages) sobre controladores e exibições MVC do ASP.NET. Uma série de tutoriais semelhante a este está disponível para páginas do Razor, o [tutorial das páginas do Razor](/aspnet/core/tutorials/razor-pages/razor-pages-start):
>
> * É mais fácil de acompanhar.
> * Fornece mais práticas recomendadas do EF Core.
> * Usa consultas mais eficientes.
> * É mais atual com a API mais recente.
> * Aborda mais recursos.
> * É a abordagem preferencial para o desenvolvimento de novos aplicativos.

> Este artigo demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 e o Visual Studio. Este tutorial usa o fluxo de trabalho do Code First. Para obter informações sobre como escolher entre o Code First, Model First e Database First, consulte [criar um modelo](/ef/ef6/modeling/).
>
> O aplicativo de exemplo é um site da web para uma universidade fictícia chamada Contoso University. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor. Esta série de tutoriais explica como compilar o aplicativo de exemplo Contoso University. Você pode [Baixe o aplicativo concluído](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
>
> Uma versão do Visual Basic traduzida por Mike Brind está disponível: [MVC 5 com o EF 6 no Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) no site Mikesdotnetting.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - [Entity Framework 6](https://www.nuget.org/packages/EntityFramework)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (opcional)
>
> ## <a name="tutorial-versions"></a>Versões de tutoriais
>
> Para versões anteriores deste tutorial, consulte [o EF 4.1 / MVC 3 livro eletrônico](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) e [Introdução ao EF 5 usando o MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou neste tutorial e o que poderíamos melhorar usando os comentários na parte inferior da página. Se você tiver perguntas que não estão diretamente relacionadas para o tutorial, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx) ou [StackOverflow.com](http://stackoverflow.com/).
>
> Se você tiver um problema que você não conseguir resolver, você geralmente pode encontrar a solução ao problema comparando o código para o projeto concluído que você pode baixar. Para alguns erros comuns e como resolvê-los, consulte [erros comuns e soluções ou soluções alternativas](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors).

## <a name="the-contoso-university-web-app"></a>O aplicativo Web Contoso University

O aplicativo que você criará nestes tutoriais é um site simples Universidade. Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores. Aqui estão algumas das telas, você criará:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Para que o tutorial possa se concentrar principalmente em como usar o Entity Framework, a interface do usuário do site da web não mudou muito do que é gerado pelos modelos internos.

## <a name="prerequisites"></a>Pré-requisitos

Ver **versões de Software** na parte superior da página. Entity Framework 6 não é um pré-requisito, porque você instalar o pacote NuGet do EF como parte do tutorial.

## <a name="create-an-mvc-web-app"></a>Criar um aplicativo web MVC

1. Abra o Visual Studio e crie um novo c# web projeto usando o **aplicativo Web ASP.NET (.NET Framework)** modelo. Nomeie o projeto "ContosoUniversity".

   ![Caixa de diálogo Novo projeto no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

2. Na caixa de diálogo Novo projeto ASP.NET, selecione o **MVC** modelo.

   ![Nova caixa de diálogo de aplicativo web no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

3. Se **autenticação** não está definido como **sem autenticação**, altere-o clicando em **alterar autenticação**.

   No **alterar autenticação** caixa de diálogo, selecione **sem autenticação**e, em seguida, escolha **Okey**. Para este tutorial, o aplicativo web não requer que os usuários entrem nem restringe o acesso com base em quem estiver conectado.

   ![Caixa de diálogo Alterar autenticação no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/change-authentication.png)

4. Na caixa de diálogo Novo projeto ASP.NET, clique em **Okey** para criar o projeto.

## <a name="set-up-the-site-style"></a>Configurar o estilo do site

Algumas alterações simples configurarão o menu do site, o layout e a home page.

1. Abra *Views\Shared\\layout. cshtml*e faça as seguintes alterações:

   - Altere cada ocorrência de "Meu aplicativo ASP.NET" e "Nome do aplicativo" para "Contoso University".
   - Adicionar entradas de menu para os alunos, cursos, instrutores e departamentos e exclua a entrada de contato.

   As alterações são realçadas no trecho de código a seguir:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. Na *Views\Home\Index.cshtml*, substitua o conteúdo do arquivo pelo código a seguir para substituir o texto sobre o ASP.NET e MVC pelo texto sobre este aplicativo:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Pressione **Ctrl**+**F5** para executar o site da web. Você ver a home page com o menu principal.

   ![Home page da Contoso University](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Instalar o Entity Framework 6

1. Dos **ferramentas** menu, escolha **Gerenciador de pacotes NuGet**e, em seguida, escolha **Package Manager Console**.

2. No **Package Manager Console** janela, digite o seguinte comando:

   ```text
   Install-Package EntityFramework
   ```

   ![EF instalado](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

   A imagem mostra 6.0.0 sendo instalado, mas o NuGet instalará a versão mais recente do Entity Framework (excluindo as versões de pré-lançamento), que, a partir da atualização mais recente para o tutorial é 6.2.0.

Esta etapa é uma das poucas etapas que este tutorial tem a fazer manualmente, mas que poderia ter sido feito automaticamente pelo recurso de scaffolding do ASP.NET MVC. Você está fazendo-os manualmente para que você possa ver as etapas necessárias para usar o Entity Framework (EF). Você vai usar scaffolding mais tarde para criar o controlador MVC e exibições. Uma alternativa é permitir que o scaffolding automaticamente instala o pacote NuGet do EF, criar a classe de contexto do banco de dados e criar a cadeia de caracteres de conexão. Quando você estiver pronto para fazer isso dessa maneira, tudo o que você precisa fazer é ignorar essas etapas e o scaffolding seu controlador MVC, depois de criar as classes de entidade.

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo Contoso University. Você começará com as três seguintes entidades:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`, e uma relação um-para-muitos entre as entidades `Course` e `Enrollment`. Em outras palavras, um aluno pode ser registrado em qualquer quantidade de cursos e um curso pode ter qualquer quantidade de alunos registrados.

As seções a seguir, você criará uma classe para cada uma dessas entidades.

> [!NOTE]
> Se você tentar compilar o projeto antes de concluir a criação de todas essas classes de entidade, você receberá erros de compilador.

### <a name="the-student-entity"></a>A entidade Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

- No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* clicando na pasta do **Gerenciador de soluções** e escolhendo **Add**  >  **Classe**. Substitua o código de modelo pelo código a seguir:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

A propriedade `ID` se tornará a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o Entity Framework interpreta uma propriedade chamada `ID` ou *classname* `ID` como a chave primária.

A propriedade `Enrollments` é uma *propriedade de navegação*. As propriedades de navegação armazenam outras entidades que estão relacionadas a essa entidade. Nesse caso, o `Enrollments` propriedade de um `Student` entidade armazenará todas as `Enrollment` entidades relacionadas a essa `Student` entidade. Em outras palavras, se um determinado `Student` linha no banco de dados tem dois relacionadas `Enrollment` linhas (valor de linhas que contêm a chave primária do aluno em seus `StudentID` coluna de chave estrangeira), que `Student` da entidade `Enrollments` propriedade de navegação conterá as duas `Enrollment` entidades.

Propriedades de navegação geralmente são definidas como `virtual` para que eles podem tirar proveito de determinada funcionalidade do Entity Framework, como *carregamento lento*. (Carregamento lento será explicado mais adiante, o [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente nessa série.)

Se uma propriedade de navegação pode armazenar várias entidades (como em relações muitos para muitos ou um-para-muitos), o tipo precisa ser uma lista na qual entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection`.

### <a name="the-enrollment-entity"></a>A entidade Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

- Na pasta *Models*, crie *Enrollment.cs* e substitua o código existente pelo seguinte código:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

O `EnrollmentID` propriedade será a chave primária; essa entidade usa o *classname* `ID` padrão em vez de `ID` por si só, como você viu no `Student` entidade. Normalmente, você escolhe um padrão e usa-o em todo o modelo de dados. Aqui, a variação ilustra que você pode usar qualquer um dos padrões. Um tutorial posterior, você verá como o uso `ID` sem `classname` torna mais fácil de implementar a herança no modelo de dados.

O `Grade` propriedade é um [enum](/ef/ef6/modeling/code-first/data-types/enums). O ponto de interrogação após a `Grade` declaração de tipo indica que o `Grade` é de propriedade [anuláveis](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Uma nota nula é diferente de uma nota zero – nulo significa que uma nota não é conhecida ou não tenha sido atribuída ainda.

A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Uma entidade `Enrollment` é associada a uma entidade `Student`, de modo que a propriedade possa armazenar apenas uma única entidade `Student` (ao contrário da propriedade de navegação `Student.Enrollments` que você viu anteriormente, que pode armazenar várias entidades `Enrollment`).

A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Uma entidade `Enrollment` está associada a uma entidade `Course`.

Entity Framework interpreta uma propriedade como uma propriedade de chave estrangeira se ela é nomeada *&lt;nome da propriedade de navegação&gt;&lt;nome da propriedade de chave primária&gt;* (por exemplo, `StudentID`para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira também podem ser nomeadas o mesmo simplesmente *&lt;nome da propriedade de chave primária&gt;* (por exemplo, `CourseID` uma vez que o `Course` chave primária da entidade é `CourseID`).

### <a name="the-course-entity"></a>A entidade Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

- No *modelos* pasta, crie *Course.cs*, substituindo o código de modelo pelo código a seguir:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

A propriedade `Enrollments` é uma propriedade de navegação. Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.

Falaremos mais sobre o <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> atributo em um tutorial posterior nesta série. Basicamente, esse atributo permite que você insira a chave primária do curso, em vez de fazer com que ela seja gerada pelo banco de dados.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados é o *contexto de banco de dados* classe. Você cria essa classe derivando de [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) classe. No seu código, você especifica quais entidades são incluídas no modelo de dados. Também personalize o comportamento específico do Entity Framework. Neste projeto, a classe é chamada `SchoolContext`.

- Para criar uma pasta no projeto ContosoUniversity, clique com botão direito no projeto no **Gerenciador de soluções** e clique em **Add**e, em seguida, clique em **nova pasta**. Nomeie a nova pasta *DAL* (para a camada de acesso a dados). Nessa pasta, crie um novo arquivo de classe chamado *SchoolContext.cs*e substitua o código de modelo pelo código a seguir:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Especificar conjuntos de entidades

Esse código cria uma [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) propriedade para cada conjunto de entidades. Na terminologia do Entity Framework, um *conjunto de entidades* normalmente corresponde a uma tabela de banco de dados e um *entidade* corresponde a uma linha na tabela.

> [!NOTE]
>
> Você pode omitir as `DbSet<Enrollment>` e `DbSet<Course>` instruções e ele funcionaria da mesma. Entity Framework inclui-los implicitamente porque o `Student` referências de entidade a `Enrollment` entidade e o `Enrollment` referências de entidade a `Course` entidade.

### <a name="specify-the-connection-string"></a>Especifique a cadeia de conexão

O nome da cadeia de conexão (que você irá adicionar posteriormente no arquivo Web. config) é passado para o construtor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Você também pode transmitir a cadeia de conexão em si em vez do nome de um que é armazenado no arquivo Web. config. Para obter mais informações sobre opções para especificar o banco de dados a serem usadas, consulte [cadeias de caracteres de Conexão e modelos](/ef/ef6/fundamentals/configuring/connection-strings).

Se você não especificar uma cadeia de caracteres de conexão ou o nome de um explicitamente, o Entity Framework pressupõe que o nome de cadeia de caracteres de conexão é o mesmo que o nome da classe. O nome de cadeia de caracteres de conexão padrão neste exemplo, em seguida, seria `SchoolContext`, o mesmo que o que você especificar explicitamente.

### <a name="specify-singular-table-names"></a>Especificar nomes singulares de tabela

O `modelBuilder.Conventions.Remove` instrução em de [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impede que os nomes de tabela que está sendo pluralizados. Se você não fizer isso, as tabelas geradas no banco de dados seriam nomeadas `Students`, `Courses`, e `Enrollments`. Em vez disso, os nomes de tabela serão `Student`, `Course`, e `Enrollment`. Os desenvolvedores não concordam sobre se os nomes de tabela devem ser pluralizados ou não. Este tutorial usa a forma singular, mas o ponto importante é que você pode selecionar qualquer que seja o formulário que você preferir ao incluir ou omitir esta linha de código.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configurar o EF para inicializar o banco de dados com dados de teste

Entity Framework pode automaticamente criar (ou descartar e recriar) um banco de dados para você quando o aplicativo é executado. Você pode especificar que isso deve ser feito sempre que seu aplicativo é executado ou apenas quando o modelo está fora de sincronia com o banco de dados existente. Você também pode escrever um `Seed` método que o Entity Framework chama automaticamente depois de criar o banco de dados para preenchê-lo com dados de teste.

O comportamento padrão é criar um banco de dados somente se ele não existe (e lançar uma exceção se o modelo foi alterado e o banco de dados já existe). Nesta seção, você especificará que o banco de dados deve ser descartado e recriado sempre que o modelo é alterado. Descartar o banco de dados faz com que a perda de todos os seus dados. Isso é geralmente okey durante o desenvolvimento, porque o `Seed` método será executado quando o banco de dados é recriado e recriará seus dados de teste. Mas, na produção você geralmente não deseja perder todos os seus dados sempre que você precisar alterar o esquema de banco de dados. Posteriormente, você verá como lidar com alterações no modelo por meio de migrações do Code First para alterar o esquema de banco de dados, em vez de descartar e recriar o banco de dados.

1. Na pasta DAL, crie um novo arquivo de classe chamado *SchoolInitializer.cs* e substitua o código de modelo pelo código a seguir, que faz com que um banco de dados a ser criado quando necessário e carrega dados de teste no novo banco de dados.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   O `Seed` método usa o objeto de contexto do banco de dados como um parâmetro de entrada e o código no método utiliza o objeto para adicionar novas entidades no banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades e os adiciona ao apropriado `DbSet` propriedade e, em seguida, salva as alterações no banco de dados. Não é necessário chamar o `SaveChanges` método depois de cada grupo de entidades, assim como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto o código é escrito para o banco de dados.

2. Para informar ao Entity Framework para usar seu inicializador de classe, adicione um elemento para o `entityFramework` elemento no aplicativo *Web. config* arquivo (aquele na pasta raiz do projeto), conforme mostrado no exemplo a seguir:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   O `context type` Especifica o nome de classe totalmente qualificado do contexto e o assembly é, e o `databaseinitializer type` Especifica o nome totalmente qualificado da classe inicializador e o assembly está em. (Quando não desejar que o EF para usar o inicializador, você pode definir um atributo na `context` elemento: `disableDatabaseInitialization="true"`.) Para obter mais informações, consulte [arquivo de configuração](/ef/ef6/fundamentals/configuring/config-file).

   Uma alternativa para configurar o inicializador *Web. config* arquivo é fazê-lo no código, adicionando um `Database.SetInitializer` instrução para o `Application_Start` método na *Global.asax.cs* arquivo. Para obter mais informações, consulte [Noções básicas sobre inicializadores de banco de dados no Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

O aplicativo agora está configurado para que quando você acessa o banco de dados pela primeira vez em uma determinada execução do aplicativo, o Entity Framework compara o banco de dados para o modelo (seu `SchoolContext` e classes de entidade). Se houver uma diferença, o aplicativo descarta e recria o banco de dados.

> [!NOTE]
> Quando você implanta um aplicativo em um servidor web de produção, você deve remover ou desabilitar o código que descarta e recria o banco de dados. Você terá de fazer isso em um tutorial posterior nesta série.

## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configurar o EF para usar um banco de dados do SQL Server Express LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) é uma versão leve do mecanismo de banco de dados do SQL Server Express. Ele é fácil de instalar e configurar, é iniciado sob demanda e é executado no modo de usuário. LocalDB é executado em um modo de execução especial do SQL Server Express que permite que você trabalhe com bancos de dados como *mdf* arquivos. Você pode colocar arquivos de banco de dados LocalDB na *App\_dados* pasta de um projeto web se você quiser ser capaz de copiar o banco de dados com o projeto. O recurso de instância de usuário do SQL Server Express também permite que você trabalhe com *mdf* arquivos, mas o recurso de instância de usuário é preterido; portanto, o LocalDB é recomendado para trabalhar com *mdf* arquivos. LocalDB é instalado por padrão com o Visual Studio.

Normalmente, o SQL Server Express não é usado para aplicativos da web de produção. LocalDB em particular não é recomendado para uso em produção com um aplicativo web porque ele não foi projetado para trabalhar com o IIS.

- Neste tutorial, você trabalhará com o LocalDB. Abra o aplicativo *Web. config* arquivo e adicione um `connectionStrings` elemento anterior a `appSettings` elemento, conforme mostrado no exemplo a seguir. (Certifique-se de atualizar o *Web. config* arquivo na pasta raiz do projeto. Há também uma *Web. config* arquivo na *modos de exibição* subpasta que você não precisa atualizar.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

A cadeia de caracteres de conexão que você adicionou Especifica que o Entity Framework usará o banco de dados LocalDB denominado *ContosoUniversity1.mdf*. (O banco de dados ainda não exista, mas o EF vai criá-la). Se você deseja criar o banco de dados no seu *App\_dados* pasta, você poderia adicionar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` na cadeia de caracteres de conexão. Para obter mais informações sobre cadeias de caracteres de conexão, consulte [cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

Você não precisa realmente uma cadeia de caracteres de conexão na *Web. config* arquivo. Se você não fornecer uma cadeia de caracteres de conexão, o Entity Framework usa uma cadeia de conexão padrão com base em sua classe de contexto. Para obter mais informações, consulte [Code First para um novo banco de dados](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-a-student-controller-and-views"></a>Criar um controlador de aluno e modos de exibição

Agora, você criará uma página da web para exibir dados. O processo de solicitação de dados automaticamente dispara a criação do banco de dados. Você começará criando um novo controlador. Mas, antes de fazer isso, crie o projeto para disponibilizar as classes de modelo e o contexto para scaffolding do controlador MVC.

1. Com o botão direito do **controladores** pasta no **Gerenciador de soluções**, selecione **Add**e, em seguida, clique em **New Scaffolded Item**.
2. No **adicionar Scaffold** caixa de diálogo, selecione **controlador MVC 5 com modos de exibição usando o Entity Framework**e, em seguida, escolha **adicionar**.

     ![Adicionar caixa de diálogo Scaffold no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. No **Adicionar controlador** caixa de diálogo, faça as seguintes seleções e, em seguida, escolha **Add**:

   - Classe de modelo: **aluno (ContosoUniversity.Models)**. (Se você não vir essa opção na lista suspensa, compile o projeto e tente novamente.)
   - Classe de contexto de dados: **SchoolContext (ContosoUniversity.DAL)**.
   - Nome do controlador: **StudentController** (não StudentsController).
   - Deixe os valores padrão para os outros campos.

     ![Adicionar caixa de diálogo do controlador no Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-controller.png)

     Quando você clica em **Add**, o scaffolder cria um *StudentController.cs* arquivo e um conjunto de exibições (*. cshtml* arquivos) que funcionam com o controlador. No futuro quando você cria projetos que usam o Entity Framework, você pode também aproveitar algumas funcionalidades adicionais do scaffolder: criar sua primeira classe de modelo, não crie uma cadeia de caracteres de conexão e, em seguida, no **Adicionar controlador** especificar **novo contexto de dados** selecionando o **+** lado **classe de contexto de dados**. O scaffolder criará seu `DbContext` classe e sua conexão de cadeia de caracteres, bem como o controlador e exibições.
4. O Visual Studio abre o *Controllers\StudentController.cs* arquivo. Você verá que uma variável de classe foi criada que instancia um objeto de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     O `Index` método de ação obtém uma lista de alunos do *alunos* entidade definida lendo o `Students` propriedade da instância de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     O *Student\Index.cshtml* exibição exibe esta lista em uma tabela:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Pressione **Ctrl**+**F5** para executar o projeto. (Se você receber um erro "Não é possível criar a cópia de sombra", feche o navegador e tente novamente.)

     Clique o **alunos** guia para ver os dados de teste que o `Seed` método inserido. Dependendo de como restringir a janela do navegador é, você verá o link do guia do aluno na barra de endereços principais ou você terá de clicar o canto superior direito para ver o link.

     ![Botão de menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Página de índice de alunos](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Exibir o banco de dados

Quando você executou a página de alunos e o aplicativo tentou acessar o banco de dados, o EF descobertos que há não foi nenhum banco de dados e criado uma. O EF, em seguida, executou o método de semente para preencher o banco de dados.

Você pode usar tanto **Gerenciador de servidores** ou **SQL Server Object Explorer** (SSOX) para exibir o banco de dados no Visual Studio. Para este tutorial, você usará **Gerenciador de servidores**.

1. Feche o navegador.
2. Na **Gerenciador de servidores**, expanda **conexões de dados** (talvez seja necessário selecionar o botão atualizar primeiro), expanda **School contexto (ContosoUniversity)** e, em seguida, expanda  **Tabelas** para ver as tabelas no seu novo banco de dados.

    ![Tabelas de banco de dados no Gerenciador de servidores](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)

3. Clique com botão direito do **aluno** tabela e clique em **Mostrar dados da tabela** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

    ![Tabela aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/table-data.png)
4. Fechar o **Gerenciador de servidores** conexão.

O *ContosoUniversity1.mdf* e *. ldf* arquivos de banco de dados estão no *% USERPROFILE %* pasta.

Porque você está usando o `DropCreateDatabaseIfModelChanges` inicializador, agora você pode fazer uma alteração para o `Student` classe, executar novamente o aplicativo e o banco de dados seria automaticamente recriado para corresponder a sua alteração. Por exemplo, se você adicionar um `EmailAddress` propriedade para o `Student` classe, execute novamente a página de alunos e, em seguida, examinar a tabela novamente, você verá uma nova `EmailAddress` coluna.

## <a name="conventions"></a>Convenções

A quantidade de código, você precisava escrever em ordem para o Entity Framework poder criar um banco de dados completo para você é mínima devido *convenções*, ou suposições que torna a Entity Framework. Alguns deles já tem sido observados ou eram usados sem seu conhecimento deles sendo:

- Os formulários pluralizados dos nomes de classe de entidade são usados como nomes de tabela.
- Os nomes de propriedade de entidade são usados para nomes de coluna.
- Propriedades da entidade que são nomeadas `ID` ou *classname* `ID` são reconhecidas como propriedades de chave primária.
- Uma propriedade é interpretada como uma propriedade de chave estrangeira se ela é nomeada *&lt;nome da propriedade de navegação&gt;&lt;nome da propriedade de chave primária&gt;* (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira também podem ser nomeadas o mesmo simplesmente &lt;nome de propriedade de chave primária&gt; (por exemplo, `EnrollmentID` uma vez que o `Enrollment` chave primária da entidade é `EnrollmentID`).

Você viu que as convenções podem ser substituídas. Por exemplo, você especificou que não devem ser pluralizados nomes de tabela, e você verá posteriormente como marcar explicitamente uma propriedade como uma propriedade de chave estrangeira. Você aprenderá mais sobre as convenções e como substituí-las na [criando um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial posteriormente nessa série. Para obter mais informações sobre as convenções, consulte [convenções de Code First](/ef/ef6/modeling/code-first/conventions/built-in).

## <a name="summary"></a>Resumo

Você criou um aplicativo simples que usa o Entity Framework e o SQL Server Express LocalDB para armazenar e exibir dados. No próximo tutorial, você aprenderá a executar básica criar, ler, atualizar e excluir (CRUD) de operações.

Deixe comentários sobre como você gostou neste tutorial e o que poderíamos melhorar.

Links para outros recursos do Entity Framework pode ser encontrado na [acesso a dados ASP.NET – recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Avançar](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
