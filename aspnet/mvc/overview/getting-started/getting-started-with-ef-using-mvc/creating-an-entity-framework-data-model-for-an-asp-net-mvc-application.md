---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Introdução ao Entity Framework 6 Code First usando MVC 5 | Microsoft Docs
author: tdykstra
description: 'Uma versão mais recente desta série de tutorial está disponível: Introdução ao ASP.NET Core e Entity Framework Core usando Visual Studio 2015. O Contoso Universi...'
ms.author: aspnetcontent
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f03ddcf7dcc8b5d20c5459a7fb0015ab20f340c5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837163"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introdução ao Entity Framework 6 Code First usando o MVC 5
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Uma versão mais recente desta série de tutorial está disponível: [Introdução ao ASP.NET Core e Entity Framework Core usando Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 e o Visual Studio 2013. Este tutorial usa o fluxo de trabalho do Code First. Para obter informações sobre como escolher entre o Code First, Model First e Database First, consulte [fluxos de trabalho de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> O aplicativo de exemplo é um site de uma Contoso University fictícia. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor. Esta série de tutoriais explica como compilar o aplicativo de exemplo Contoso University. Você pode [Baixe o aplicativo concluído](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Uma versão do Visual Basic traduzida por Mike Brind está disponível: [MVC 5 com o EF 6 no Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) no site Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (pacote do NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (opcional)
>   
> 
> O tutorial também deve funcionar com [Visual Studio 2013 Express para Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) ou o Visual Studio 2012. O [VS 2012 versão do SDK do Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323511) é necessário para a implantação do Windows Azure com o Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Versões de tutoriais
> 
> Para versões anteriores deste tutorial, consulte [o EF 4.1 / MVC 3 livro eletrônico](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) e [Introdução ao EF 5 usando o MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários sobre como você gostou neste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver perguntas que não estão diretamente relacionadas para o tutorial, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx), o [Entity Framework e LINQ para o fórum de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).
> 
> Se você tiver um problema que não é possível resolver, você geralmente pode encontrar a solução ao problema comparando o código para o projeto concluído que você pode baixar. Para alguns erros comuns e como resolvê-los, consulte [erros comuns e soluções ou soluções alternativas para eles.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>O aplicativo Web Contoso University

O aplicativo que você criará nestes tutoriais é um site simples de uma universidade.

Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores. Estas são algumas das telas que você criará.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

O estilo de interface do usuário desse site foi mantido perto do que é gerado pelos modelos internos, de modo que o tutorial possa se concentrar principalmente em como usar o Entity Framework.

## <a name="prerequisites"></a>Pré-requisitos

Ver **versões de Software** na parte superior da página. Entity Framework 6 não é um pré-requisito, porque você instalar o pacote NuGet do EF como parte do tutorial.

## <a name="create-an-mvc-web-application"></a>Criar um aplicativo Web MVC

Abra o Visual Studio e crie um novo projeto de Web em c# chamado "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

No **novo projeto ASP.NET** caixa de diálogo, selecione o **MVC** modelo.

Se o **hospedar na nuvem** caixa de seleção a **Microsoft Azure** seção estiver selecionada, desmarque-a.

Clique em **alterar autenticação**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

No **alterar autenticação** caixa de diálogo, selecione **sem autenticação**e, em seguida, clique em **Okey**. Para este tutorial você não ser exigir que os usuários façam logon ou restringindo o acesso com base em quem está conectado.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Na caixa de diálogo Novo projeto ASP.NET, clique em **Okey** para criar o projeto.

## <a name="set-up-the-site-style"></a>Configurar o estilo do Site

Algumas alterações simples configurarão o menu do site, o layout e a home page.

Abra *Views\Shared\\layout. cshtml*e faça as seguintes alterações:

- Altere cada ocorrência de "Meu aplicativo ASP.NET" e "Nome do aplicativo" para "Contoso University".
- Adicionar entradas de menu para os alunos, cursos, instrutores e departamentos e exclua a entrada de contato.

As alterações são realçadas.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

Na *Views\Home\Index.cshtml*, substitua o conteúdo do arquivo pelo código a seguir para substituir o texto sobre o ASP.NET e MVC pelo texto sobre este aplicativo:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Pressione CTRL + F5 para executar o site. Você ver a home page com o menu principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Instalar o Entity Framework 6

Dos **ferramentas** menu, clique em **Gerenciador de pacotes NuGet** e, em seguida, clique em **Package Manager Console**.

No **Package Manager Console** janela Digite o seguinte comando:

`Install-Package EntityFramework`

![EF instalado](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

A imagem mostra 6.0.0 sendo instalado, mas o NuGet instalará a versão mais recente do Entity Framework (excluindo as versões de pré-lançamento), que, a partir da atualização mais recente para o tutorial é 6.1.1.

Esta etapa é uma das poucas etapas que este tutorial tem a fazer manualmente, mas que poderia ter sido feito automaticamente pelo recurso de scaffolding do ASP.NET MVC. Você está fazendo-os manualmente para que você possa ver as etapas necessárias para usar o Entity Framework. Você vai usar scaffolding mais tarde para criar o controlador MVC e exibições. Uma alternativa é permitir que o scaffolding automaticamente instala o pacote NuGet do EF, criar a classe de contexto do banco de dados e criar a cadeia de caracteres de conexão. Quando você estiver pronto para fazer isso dessa maneira, tudo o que você precisa fazer é ignorar essas etapas e o scaffolding seu controlador MVC, depois de criar as classes de entidade.

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo Contoso University. Você começará com as três seguintes entidades:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`, e uma relação um-para-muitos entre as entidades `Course` e `Enrollment`. Em outras palavras, um aluno pode ser registrado em qualquer quantidade de cursos e um curso pode ter qualquer quantidade de alunos registrados.

Nas seções a seguir, você criará uma classe para cada uma dessas entidades.

> [!NOTE]
> Se você tentar compilar o projeto antes de concluir a criação de todas essas classes de entidade, você receberá erros de compilador.


### <a name="the-student-entity"></a>A entidade Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* e substitua o código de modelo pelo código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

A propriedade `ID` se tornará a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o Entity Framework interpreta uma propriedade chamada `ID` ou *classname* `ID` como a chave primária.

O `Enrollments` propriedade é um *propriedade de navegação*. As propriedades de navegação armazenam outras entidades que estão relacionadas a essa entidade. Nesse caso, o `Enrollments` propriedade de um `Student` entidade armazenará todas as `Enrollment` entidades relacionadas a essa `Student` entidade. Em outras palavras, se um determinado `Student` linha no banco de dados tem dois relacionadas `Enrollment` linhas (valor de linhas que contêm a chave primária do aluno em seus `StudentID` coluna de chave estrangeira), que `Student` da entidade `Enrollments` propriedade de navegação conterá as duas `Enrollment` entidades.

Propriedades de navegação geralmente são definidas como `virtual` para que eles podem tirar proveito de determinada funcionalidade do Entity Framework, como *carregamento lento*. (Carregamento lento será explicado mais adiante, o [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente nessa série.)

Se uma propriedade de navegação pode armazenar várias entidades (como em relações muitos para muitos ou um-para-muitos), o tipo precisa ser uma lista na qual entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection`.

### <a name="the-enrollment-entity"></a>A entidade Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Na pasta *Models*, crie *Enrollment.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

O `EnrollmentID` propriedade será a chave primária; essa entidade usa o *classname* `ID` padrão em vez de `ID` por si só, como você viu no `Student` entidade. Normalmente, você escolhe um padrão e usa-o em todo o modelo de dados. Aqui, a variação ilustra que você pode usar qualquer um dos padrões. Um tutorial posterior, você verá como o uso `ID` sem `classname` torna mais fácil de implementar a herança no modelo de dados.

O `Grade` propriedade é um [enum](https://msdn.microsoft.com/data/hh859576.aspx). O ponto de interrogação após a `Grade` declaração de tipo indica que o `Grade` é de propriedade [anuláveis](https://msdn.microsoft.com/library/2cf62fcy.aspx). Uma nota nula é diferente de uma nota zero – nulo significa que uma nota não é conhecida ou não tenha sido atribuída ainda.

A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Uma entidade `Enrollment` é associada a uma entidade `Student`, de modo que a propriedade possa armazenar apenas uma única entidade `Student` (ao contrário da propriedade de navegação `Student.Enrollments` que você viu anteriormente, que pode armazenar várias entidades `Enrollment`).

A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Uma entidade `Enrollment` está associada a uma entidade `Course`.

Entity Framework interpreta uma propriedade como uma propriedade de chave estrangeira se ela é nomeada *&lt;nome da propriedade de navegação&gt;&lt;nome da propriedade de chave primária&gt;* (por exemplo, `StudentID`para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira também podem ser nomeadas o mesmo simplesmente *&lt;nome da propriedade de chave primária&gt;* (por exemplo, `CourseID` uma vez que o `Course` chave primária da entidade é `CourseID`).

### <a name="the-course-entity"></a>A entidade de curso

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

No *modelos* pasta, crie *Course.cs*, substituindo o código de modelo pelo código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

A propriedade `Enrollments` é uma propriedade de navegação. Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.

Falaremos mais sobre o [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) atributo em um tutorial posterior nesta série. Basicamente, esse atributo permite que você insira a chave primária do curso, em vez de fazer com que ela seja gerada pelo banco de dados.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados é o *contexto de banco de dados* classe. Você cria essa classe derivando de [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. No código, especifique quais entidades são incluídas no modelo de dados. Também personalize o comportamento específico do Entity Framework. Neste projeto, a classe é chamada `SchoolContext`.

Para criar uma pasta no projeto ContosoUniversity, clique com botão direito no projeto no **Gerenciador de soluções** e clique em **Add**e, em seguida, clique em **nova pasta**. Nomeie a nova pasta *DAL* (para a camada de acesso a dados). Nessa pasta, crie um novo arquivo de classe chamado *SchoolContext.cs*e substitua o código de modelo pelo código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Especificando conjuntos de entidades

Esse código cria uma [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) propriedade para cada conjunto de entidades. Na terminologia do Entity Framework, um *conjunto de entidades* normalmente corresponde a uma tabela de banco de dados e um *entidade* corresponde a uma linha na tabela.

> [!NOTE] 
> 
> Você poderia ter omitido a `DbSet<Enrollment>` e `DbSet<Course>` instruções e ele funcionaria da mesma. O Entity Framework inclui-os de forma implícita porque a entidade `Student` referencia a entidade `Enrollment` e a entidade `Enrollment` referencia a entidade `Course`.


### <a name="specifying-the-connection-string"></a>Especificando a cadeia de caracteres de conexão

O nome da cadeia de conexão (que você irá adicionar posteriormente no arquivo Web. config) é passado para o construtor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Você também pode transmitir a cadeia de conexão em si em vez do nome de um que é armazenado no arquivo Web. config. Para obter mais informações sobre opções para especificar o banco de dados a serem usadas, consulte [Entity Framework - conexões e modelos](https://msdn.microsoft.com/data/jj592674).

Se você não especificar uma cadeia de caracteres de conexão ou o nome de um explicitamente, o Entity Framework pressupõe que o nome de cadeia de caracteres de conexão é o mesmo que o nome da classe. O nome de cadeia de caracteres de conexão padrão neste exemplo, em seguida, seria `SchoolContext`, o mesmo que o que você especificar explicitamente.

### <a name="specifying-singular-table-names"></a>Especificando nomes singulares de tabela

O `modelBuilder.Conventions.Remove` instrução em de [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impede que os nomes de tabela que está sendo pluralizados. Se você não fizer isso, as tabelas geradas no banco de dados seriam nomeadas `Students`, `Courses`, e `Enrollments`. Em vez disso, os nomes de tabela serão `Student`, `Course`, e `Enrollment`. Os desenvolvedores não concordam sobre se os nomes de tabela devem ser pluralizados ou não. Este tutorial usa a forma singular, mas o ponto importante é que você pode selecionar qualquer que seja o formulário que você preferir ao incluir ou omitir esta linha de código.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configurar o EF para inicializar o banco de dados com dados de teste

O Entity Framework pode automaticamente criar (ou descartar e recriar) um banco de dados para você quando o aplicativo é executado. Você pode especificar que isso deve ser feito sempre que seu aplicativo é executado ou apenas quando o modelo está fora de sincronia com o banco de dados existente. Você também pode escrever um `Seed` método que o Entity Framework chama automaticamente depois de criar o banco de dados para preenchê-lo com dados de teste.

O comportamento padrão é criar um banco de dados somente se ele não existe (e lançar uma exceção se o modelo foi alterado e o banco de dados já existe). Nesta seção, você especificará que o banco de dados deve ser descartado e recriado sempre que o modelo é alterado. Descartar o banco de dados faz com que a perda de todos os seus dados. Isso é geralmente Okey durante o desenvolvimento, porque o `Seed` método será executado quando o banco de dados é recriado e recriará seus dados de teste. Mas, na produção você geralmente não deseja perder todos os seus dados sempre que você precisar alterar o esquema de banco de dados. Posteriormente, você verá como lidar com alterações no modelo por meio de migrações do Code First para alterar o esquema de banco de dados, em vez de descartar e recriar o banco de dados.

Na pasta DAL, crie um novo arquivo de classe chamado *SchoolInitializer.cs* e substitua o código de modelo com o  
seguindo o código, que faz com que um banco de dados a ser criado quando necessário e carrega dados de teste no novo banco de dados.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

O `Seed` método usa o objeto de contexto do banco de dados como um parâmetro de entrada e o código no método utiliza o objeto para adicionar novas entidades no banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades e os adiciona ao apropriado `DbSet` propriedade e, em seguida, salva as alterações no banco de dados. Não é necessário chamar o `SaveChanges` método depois de cada grupo de entidades, assim como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto o código é escrito para o banco de dados.

Para informar ao Entity Framework para usar seu inicializador de classe, adicione um elemento para o `entityFramework` elemento no aplicativo *Web. config* arquivo (aquele na pasta raiz do projeto), conforme mostrado no exemplo a seguir:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

O `context type` Especifica o nome de classe totalmente qualificado do contexto e o assembly é, e o `databaseinitializer type` Especifica o nome totalmente qualificado da classe inicializador e o assembly está em. (Quando não desejar que o EF para usar o inicializador, você pode definir um atributo na `context` elemento: `disableDatabaseInitialization="true"`.) Para obter mais informações, consulte [Entity Framework – configurações do arquivo de configuração](https://msdn.microsoft.com/data/jj556606).

Como uma alternativa para configurar o inicializador *Web. config* arquivo é fazê-lo no código, adicionando um `Database.SetInitializer` instrução para o `Application_Start` método na *Global.asax.cs* arquivo. Para obter mais informações, consulte [Noções básicas sobre inicializadores de banco de dados no Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

O aplicativo agora está definido para cima para que quando você acessa o banco de dados pela primeira vez em uma determinada execução das  
o aplicativo, o Entity Framework compara o banco de dados para o modelo (seu `SchoolContext` e classes de entidade). Se houver uma diferença, o aplicativo descarta e recria o banco de dados.

> [!NOTE]
> Quando você implanta um aplicativo em um servidor web de produção, você deve remover ou desabilitar o código que descarta e recria o banco de dados. Você terá de fazer isso em um tutorial posterior nesta série.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configurar o EF para usar um banco de dados do SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) é uma versão leve do mecanismo de banco de dados do SQL Server Express. Ele é fácil de instalar e configurar, é iniciado sob demanda e é executado no modo de usuário. LocalDB é executado em um modo de execução especial do SQL Server Express que permite que você trabalhe com bancos de dados como *mdf* arquivos. Você pode colocar arquivos de banco de dados LocalDB na *App\_dados* pasta de um projeto web se você quiser ser capaz de copiar o banco de dados com o projeto. O recurso de instância de usuário do SQL Server Express também permite que você trabalhe com *mdf* arquivos, mas o recurso de instância de usuário é preterido; portanto, o LocalDB é recomendado para trabalhar com *mdf* arquivos. No Visual Studio 2012 e versões posteriores, o LocalDB é instalado por padrão com o Visual Studio.

Normalmente o SQL Server Express não é usado para aplicativos da web de produção. LocalDB em particular não é recomendado para uso em produção com um aplicativo web porque ela não foi projetada para trabalhar com o IIS.

Neste tutorial, você trabalhará com o LocalDB. Abra o aplicativo *Web. config* arquivo e adicione um `connectionStrings` elemento anterior a `appSettings` elemento, conforme mostrado no exemplo a seguir. (Certifique-se de atualizar o *Web. config* arquivo na pasta raiz do projeto. Há também uma *Web. config* arquivo está sendo a *modos de exibição* subpasta que você não precisa atualizar.)

Se você estiver usando o Visual Studio 2015, substitua "v11.0" na cadeia de conexão por "MSSQLLocalDB", pois o nome da instância padrão do SQL Server foi alterado.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

A cadeia de caracteres de conexão que você adicionou Especifica que o Entity Framework usará o banco de dados LocalDB denominado *ContosoUniversity1.mdf*. (O banco de dados ainda não exista; O EF irá criá-lo.) Se você quisesse que o banco de dados a ser criado em seu *App\_dados* pasta, você poderia adicionar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` na cadeia de caracteres de conexão. Para obter mais informações sobre cadeias de caracteres de conexão, consulte [cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Você não precisa ter uma cadeia de conexão, na verdade, o *Web. config* arquivo. Se você não fornecer uma cadeia de caracteres de conexão, o Entity Framework usará um padrão com base em sua classe de contexto. Para obter mais informações, consulte [Code First para um novo banco de dados](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Criando um controlador de aluno e exibições

Agora você criará uma página da web para exibir dados e o processo de solicitação de dados irá disparar automaticamente  
a criação do banco de dados. Você começará criando um novo controlador. Mas, antes de fazer isso, crie o projeto para disponibilizar as classes de modelo e o contexto para scaffolding do controlador MVC.

1. Com o botão direito do **controladores** pasta no **Gerenciador de soluções**, selecione **Add**e, em seguida, clique em **New Scaffolded Item**.
2. No **adicionar Scaffold** caixa de diálogo, selecione **controlador MVC 5 com modos de exibição usando o Entity Framework**.

     ![Adicionar Scaffold](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. Na caixa de diálogo Adicionar controlador, faça as seguintes seleções e, em seguida, clique em **adicionar**:

   - Classe de modelo: **aluno (ContosoUniversity.Models)**. (Se você não vir essa opção na lista suspensa, compile o projeto e tente novamente.)
   - Classe de contexto de dados: **SchoolContext (ContosoUniversity.DAL)**.
   - Nome do controlador: **StudentController** (não StudentsController).
   - Deixe os valores padrão para os outros campos.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Quando você clica em **adicionar**, o scaffolder cria um arquivo de StudentController.cs e um conjunto de exibições (arquivos. cshtml) que funcionam com o controlador. No futuro, quando você cria projetos que usam o Entity Framework, você também pode tirar proveito de algumas funcionalidades adicionais do scaffolder: basta criar sua primeira classe de modelo, não crie uma cadeia de caracteres de conexão e, em seguida, no **Adicionar controlador** caixa de especificar a nova classe de contexto. O scaffolder criará seu `DbContext` classe e sua conexão de cadeia de caracteres, bem como o controlador e exibições.
4. O Visual Studio abre o *Controllers\StudentController.cs* arquivo. Você verá que foi criada uma variável de classe que instancia um objeto de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     O `Index` método de ação obtém uma lista de alunos do *alunos* entidade definida lendo o `Students` propriedade da instância de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     O *Student\Index.cshtml* exibição exibe esta lista em uma tabela:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Pressione CTRL+F5 para executar o projeto. (Se você receber um erro "Não é possível criar a cópia de sombra", feche o navegador e tente novamente.)

     Clique o **alunos** guia para ver os dados de teste que o `Seed` método inserido. Dependendo de como restringir a janela do navegador é, você verá o link do guia do aluno na barra de endereços principais ou você terá de clicar o canto superior direito para ver o link.

     ![Botão de menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Página de índice de alunos](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Exibir o banco de dados

Quando você executou a página de alunos e o aplicativo tentou acessar o banco de dados, o EF observou que não havia nenhum banco de dados e e criou um, em seguida, ele executou o método de semente para preencher o banco de dados.

Você pode usar tanto **Gerenciador de servidores** ou **SQL Server Object Explorer** (SSOX) para exibir o banco de dados no Visual Studio. Para este tutorial você usará **Gerenciador de servidores**. (No Visual Studio Express editions anteriores a 2013, **Gerenciador de servidores** é chamado **Database Explorer**.)

1. Feche o navegador.
2. Na **Gerenciador de servidores**, expanda **conexões de dados**, expanda **School contexto (ContosoUniversity)** e, em seguida, expanda **tabelas** para ver as tabelas no seu novo banco de dados.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Clique com botão direito do **aluno** tabela e clique em **Mostrar dados da tabela** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

    ![Tabela aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Fechar o **Gerenciador de servidores** conexão.

O *ContosoUniversity1.mdf* e *. ldf* arquivos de banco de dados estão no `C:\Users\<yourusername>` pasta.

Porque você está usando o `DropCreateDatabaseIfModelChanges` inicializador, agora você pode fazer uma alteração para o `Student` classe, executar novamente o aplicativo e o banco de dados seria automaticamente recriado para corresponder a sua alteração. Por exemplo, se você adicionar um `EmailAddress` propriedade para o `Student` classe, execute novamente a página de alunos e, em seguida, examinar a tabela novamente, você verá um novo `EmailAddress` coluna.

## <a name="conventions"></a>Convenções

A quantidade de código, você precisava escrever para que o Entity Framework poder criar um banco de dados completo para você é mínima, devido ao uso de *convenções*, ou suposições que faz com que o Entity Framework. Alguns deles já tem sido observados ou eram usados sem seu conhecimento deles sendo:

- Os formulários pluralizados dos nomes de classe de entidade são usados como nomes de tabela.
- Os nomes de propriedade de entidade são usados para nomes de coluna.
- Propriedades da entidade que são nomeadas `ID` ou *classname* `ID` são reconhecidas como propriedades de chave primária.
- Uma propriedade é interpretada como uma propriedade de chave estrangeira se ela é nomeada *&lt;nome da propriedade de navegação&gt;&lt;nome da propriedade de chave primária&gt;* (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira também podem ser nomeadas o mesmo simplesmente &lt;nome de propriedade de chave primária&gt; (por exemplo, `EnrollmentID` uma vez que o `Enrollment` chave primária da entidade é `EnrollmentID`).

Você viu que as convenções podem ser substituídas. Por exemplo, você especificou que não devem ser pluralizados nomes de tabela, e você verá posteriormente como marcar explicitamente uma propriedade como uma propriedade de chave estrangeira. Você aprenderá mais sobre as convenções e como substituí-las na [criando um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial posteriormente nessa série. Para obter mais informações sobre as convenções, consulte [convenções de Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Resumo

Agora você criou um aplicativo simples que usa o Entity Framework e o SQL Server Express LocalDB para armazenar e exibir dados. O tutorial a seguir, você aprenderá como executar básica CRUD (criar, ler, atualizar e excluir) as operações.

Deixe comentários sobre como você gostou neste tutorial e o que poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework pode ser encontrado na [acesso a dados ASP.NET – recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Avançar](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
