---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Guia de Introdução ao Entity Framework 6 Code First usando MVC 5 | Microsoft Docs"
author: tdykstra
description: "Há uma versão mais recente desta série tutorial: Introdução ao ASP.NET Core e o Entity Framework Core usando o Visual Studio 2015. Universi a Contoso..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84ca4bbaebe401d14233131bcaa027debf7ea0f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introdução ao Entity Framework 6 Code First usando o MVC 5
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Uma versão mais recente desta série tutorial está disponível: [Introdução ao ASP.NET Core e o Entity Framework Core usando o Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 e o Visual Studio 2013. Este tutorial usa o fluxo de trabalho Code First. Para obter informações sobre como escolher entre Code First, Database First e Model First, consulte [fluxos de trabalho de desenvolvimento do Entity Framework](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> O aplicativo de exemplo é um site de uma universidade Contoso fictícia. Ele inclui a funcionalidade como admissão do aluno, criação de curso e atribuições do instrutor. Esta série de tutorial explica como construir o aplicativo de exemplo Contoso University. Você pode [Baixe o aplicativo concluído](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Está disponível uma versão do Visual Basic traduzida por Mike Brind: [MVC 5 com o EF 6 no Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) no site Mikesdotnetting.
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
> O tutorial também deve trabalhar com [Visual Studio 2013 Express para Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) ou o Visual Studio 2012. O [versão VS 2012 do SDK do Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323511) é necessário para a implantação do Windows Azure com o Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Versões do tutoriais
> 
> Para versões anteriores deste tutorial, consulte [EF 4.1 / livro eletrônico do MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) e [guia de Introdução ao EF 5 usando MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), o [do Entity Framework e LINQ to Fórum de entidades](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).
> 
> Se você tiver um problema que não é possível resolver, você geralmente pode encontrar a solução do problema comparando seu código para o projeto completo que você pode baixar. Para alguns erros comuns e como resolvê-los, consulte [os erros comuns e soluções ou soluções alternativas para eles.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>O aplicativo da Web da Contoso University

O aplicativo que você criará nos tutoriais é um site simples university.

Os usuários podem exibir e atualizar aluno, curso e informações do instrutor. Aqui estão algumas das telas, você criará.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

O estilo de interface do usuário desse site foi mantido perto o que é gerado pelos modelos internos, para que o tutorial pode se concentrar principalmente sobre como usar o Entity Framework.

## <a name="prerequisites"></a>Pré-requisitos

Consulte **versões de Software** na parte superior da página. Entity Framework 6 não é um pré-requisito porque você instalar o pacote NuGet EF como parte do tutorial.

## <a name="create-an-mvc-web-application"></a>Criar um aplicativo MVC

Abra o Visual Studio e crie um novo projeto c# Web chamado "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

No **novo projeto ASP.NET** select da caixa de diálogo de **MVC** modelo.

Se o **Host na nuvem** caixa de seleção de **Microsoft Azure** seção estiver selecionada, desmarque-a.

Clique em **alterar autenticação**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

No **alterar autenticação** caixa de diálogo, selecione **sem autenticação**e, em seguida, clique em **Okey**. Neste tutorial você não ser exigir que os usuários façam logon ou restringir o acesso com base em quem está conectado.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Novamente na caixa de diálogo Novo projeto ASP.NET, clique em **Okey** para criar o projeto.

## <a name="set-up-the-site-style"></a>Definir o estilo de Site

Algumas alterações simples configurará o menu de site, o layout e a página inicial.

Abra *exibições \ compartilhadas\\cshtml*e faça as seguintes alterações:

- Altere cada ocorrência de "Meu aplicativo ASP.NET" e "Nome do aplicativo" para "Contoso University".
- Adicionar entradas de menu para departamentos, cursos, professores e alunos e exclua a entrada de contato.

As alterações são realçadas.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

Em *Views\Home\Index.cshtml*, substitua o conteúdo do arquivo com o código a seguir para substituir o texto sobre o ASP.NET e MVC com texto sobre este aplicativo:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Pressione CTRL + F5 para executar o site. Você ver a página inicial com o menu principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Instale o Entity Framework 6

Do **ferramentas** menu clique **NuGet Package Manager** e, em seguida, clique em **Package Manager Console**.

No **Package Manager Console** janela Digite o seguinte comando:

`Install-Package EntityFramework`

![EF instalado](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

A imagem mostra 6.0.0 sendo instalado, mas NuGet irá instalar a versão mais recente do Entity Framework (excluindo as versões de pré-lançamento), que a partir da atualização mais recente para o tutorial é 6.1.1.

Esta etapa é uma das poucas etapas que este tutorial tem a fazer manualmente, mas que poderia ter sido feito automaticamente pelo recurso de estrutura do ASP.NET MVC. Você está fazendo-los manualmente para que você possa ver as etapas necessárias para usar o Entity Framework. Você usará scaffolding de mais tarde para criar o controlador MVC e modos de exibição. Uma alternativa é permitir que o scaffolding automaticamente instala o pacote NuGet EF, criar a classe de contexto de banco de dados e criar a cadeia de caracteres de conexão. Quando você estiver pronto para fazê-lo dessa forma, você precisa fazer é ignorar essas etapas e scaffold seu controlador MVC depois de criar as classes de entidade.

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo da Contoso University. Comece com as três seguintes entidades:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Há uma relação um-para-muitos entre `Student` e `Enrollment` entidades, e há uma relação um-para-muitos entre `Course` e `Enrollment` entidades. Em outras palavras, um aluno pode ser registrado em qualquer número de cursos e um curso pode ter qualquer número de alunos registrados nele.

As seções a seguir, você criará uma classe para cada uma dessas entidades.

> [!NOTE]
> Se você tentar compilar o projeto antes de terminar a criação de todas essas classes de entidade, você receberá erros do compilador.


### <a name="the-student-entity"></a>A entidade do aluno

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

O `ID` propriedade tornam-se a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o Entity Framework interpreta uma propriedade denominada `ID` ou *classname* `ID` como a chave primária.

O `Enrollments` propriedade é um *propriedade de navegação*. Propriedades de navegação mantêm outras entidades que estão relacionadas a esta entidade. Nesse caso, o `Enrollments` propriedade de um `Student` entidade conterá todos os `Enrollment` entidades relacionadas ao `Student` entidade. Em outras palavras, se um determinado `Student` linha no banco de dados tem duas relacionadas `Enrollment` linhas (valor de linhas que contêm a chave primária que student em seus `StudentID` coluna de chave estrangeira), que `Student` da entidade `Enrollments` propriedade de navegação conterá dois `Enrollment` entidades.

Propriedades de navegação costumam ser definidas como `virtual` para que eles podem tirar proveito de alguns recursos do Entity Framework, como *carregamento preguiçoso*. (Carregamento preguiçoso será explicado mais adiante, a [dados relacionados de leitura](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente na série.)

Se uma propriedade de navegação pode conter várias entidades (como relações muitos-para-muitos ou um-para-muitos), seu tipo deve ser uma lista na qual as entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection`.

### <a name="the-enrollment-entity"></a>A entidade de registro

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

No *modelos* pasta, criar *Enrollment.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

O `EnrollmentID` propriedade será a chave primária; esta entidade usa o *classname* `ID` padrão em vez de `ID` por si só, como você viu no `Student` entidade. Normalmente você deve escolher um padrão e usá-lo em todo o modelo de dados. Aqui, a variação ilustra que você pode usar o padrão. Um tutorial posterior, você verá como usar `ID` sem `classname` torna mais fácil de implementar a herança no modelo de dados.

O `Grade` propriedade é um [enum](https://msdn.microsoft.com/en-us/data/hh859576.aspx). O ponto de interrogação após o `Grade` declaração de tipo indica que o `Grade` é de propriedade [anulável](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx). Uma classificação que é null é diferente de uma classificação zero — null significa que um nível não é conhecida ou ainda não foi atribuído.

O `StudentID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Um `Enrollment` entidade está associada um `Student` entidade, para a propriedade pode conter apenas um único `Student` entidade (ao contrário de `Student.Enrollments` propriedade de navegação que vimos anteriormente, que pode conter vários `Enrollment` entidades).

O `CourseID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Um `Enrollment` entidade está associada um `Course` entidade.

Entity Framework interpreta uma propriedade como uma propriedade de chave estrangeira se ele é nomeado  *&lt;nome da propriedade de navegação&gt;&lt;nome de propriedade de chave primária&gt;*  (por exemplo, `StudentID`para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira podem também ser o mesmo nome simplesmente  *&lt;nome de propriedade de chave primária&gt;*  (por exemplo, `CourseID` desde o `Course` chave primária da entidade é `CourseID`).

### <a name="the-course-entity"></a>A entidade de curso

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

No *modelos* pasta, criar *Course.cs*, substituindo o código de modelo com o código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

O `Enrollments` propriedade é uma propriedade de navegação. Um `Course` entidade pode estar relacionada a qualquer número de `Enrollment` entidades.

Podemos dizer mais sobre o [DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) atributo em um tutorial posterior nesta série. Basicamente, este atributo permite que você insira a chave primária para o curso, em vez de fazer com que o banco de dados gerá-lo.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena a funcionalidade do Entity Framework para um modelo de dados é o *contexto de banco de dados* classe. Crie esta classe derivando de [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. No seu código, você especifica quais entidades são incluídas no modelo de dados. Você também pode personalizar o comportamento específico do Entity Framework. Neste projeto, a classe é nomeada `SchoolContext`.

Para criar uma pasta no projeto ContosoUniversity, com o botão direito no projeto no **Solution Explorer** e clique em **adicionar**e, em seguida, clique em **nova pasta**. Nomeie a nova pasta *DAL* (para a camada de acesso a dados). Nessa pasta, crie um novo arquivo de classe chamado *SchoolContext.cs*e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Especificando conjuntos de entidade

Esse código cria um [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=VS.103).aspx) propriedade para cada conjunto de entidades. Na terminologia do Entity Framework, um *conjunto de entidades* normalmente corresponde a uma tabela de banco de dados e um *entidade* corresponde a uma linha na tabela.

> [!NOTE] 
> 
> Você poderia omitir o `DbSet<Enrollment>` e `DbSet<Course>` instruções e ele seriam funcionam da mesma. O Entity Framework inclui-los implicitamente porque o `Student` referências de entidade de `Enrollment` entidade e o `Enrollment` referências de entidade o `Course` entidade.


### <a name="specifying-the-connection-string"></a>Especifica a cadeia de caracteres de conexão

O nome da cadeia de conexão (que você adicionará ao arquivo Web. config posteriormente) é passado para o construtor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Você também pode passar na cadeia de conexão em si em vez do nome de um que é armazenado no arquivo Web. config. Para obter mais informações sobre as opções para especificar o banco de dados para usar, consulte [Entity Framework - conexões e modelos](https://msdn.microsoft.com/en-us/data/jj592674).

Se você não especificar uma cadeia de caracteres de conexão ou o nome de um explicitamente, o Entity Framework pressupõe que o nome da cadeia de caracteres de conexão é o mesmo que o nome da classe. O nome de cadeia de caracteres de conexão padrão neste exemplo, em seguida, seria `SchoolContext`, o mesmo que o que você especificar explicitamente.

### <a name="specifying-singular-table-names"></a>Especificando nomes de tabela única

O `modelBuilder.Conventions.Remove` instrução o [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impede que os nomes de tabela que está sendo pluralized. Se você não tiver feito isso, as tabelas geradas no banco de dados serão chamadas `Students`, `Courses`, e `Enrollments`. Em vez disso, os nomes de tabela serão `Student`, `Course`, e `Enrollment`. Os desenvolvedores não concordam sobre se os nomes de tabela devem ser pluralizados ou não. Este tutorial usa a forma singular, mas o ponto importante é que você pode selecionar qualquer formato que preferir, incluindo ou omitir esta linha de código.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configurar o EF para inicializar o banco de dados de teste

O Entity Framework pode automaticamente (ou descartar e recriar) quando o aplicativo é executado em um banco de dados. Você pode especificar que isso deve ser feito sempre que seu aplicativo é executado ou apenas quando o modelo está fora de sincronia com o banco de dados existente. Você também pode escrever um `Seed` método que o Entity Framework chama automaticamente depois de criar o banco de dados para preenchê-la com dados de teste.

O comportamento padrão é criar um banco de dados somente se ela não existir (e lançar uma exceção se o modelo foi alterado e o banco de dados já existe). Nesta seção, você especificará que o banco de dados deve ser descartado e recriado sempre que o modelo é alterado. Descartar o banco de dados faz com que a perda de todos os seus dados. Isso é geralmente Okey durante o desenvolvimento, pois o `Seed` método será executado quando o banco de dados é recriado e recriará seus dados de teste. Mas em produção geralmente não deseja perder todos os seus dados sempre que você precisa alterar o esquema de banco de dados. Posteriormente, você verá como lidar com as alterações do modelo usando migrações do Code First para alterar o esquema de banco de dados em vez de descartar e recriar o banco de dados.

Na pasta DAL, crie um novo arquivo de classe chamado *SchoolInitializer.cs* e substitua o código de modelo com o  
seguir o código, o que faz com que um banco de dados a ser criado quando necessário e carrega dados de teste para o novo banco de dados.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

O `Seed` método usa o objeto de contexto do banco de dados como um parâmetro de entrada e o código no método usa  
Esse objeto para adicionar novas entidades no banco de dados. Para cada tipo de entidade, o código cria uma coleção de novos  
 entidades, adiciona-os ao apropriado `DbSet` propriedade e, em seguida, salva as alterações no banco de dados. Não é  
necessário para chamar o `SaveChanges` método depois de cada grupo de entidades, como é feito aqui, mas que ajuda a fazer  
você localizar a origem de um problema se ocorrer uma exceção enquanto estiver gravando o código para o banco de dados.

Para informar o Entity Framework para usar seu inicializador de classe, adicione um elemento para o `entityFramework` elemento no aplicativo *Web. config* arquivo (aquele na pasta raiz do projeto), conforme mostrado no exemplo a seguir:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

O `context type` Especifica o nome totalmente qualificado do contexto da classe e o assembly é, e o `databaseinitializer type` Especifica o nome totalmente qualificado da classe de inicializador e o assembly está em. (Quando você não quiser EF para usar o inicializador, você pode definir um atributo no `context` elemento: `disableDatabaseInitialization="true"`.) Para obter mais informações, consulte [Entity Framework - configurações do arquivo de configuração](https://msdn.microsoft.com/en-us/data/jj556606).

Como uma alternativa para definir o inicializador *Web. config* arquivo é fazer isso no código adicionando um `Database.SetInitializer` instrução para o `Application_Start` método no *Global.asax.cs* arquivo. Para obter mais informações, consulte [inicializadores de banco de dados de Conhecimento no Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

O aplicativo agora está definido para cima para que quando você acessa o banco de dados pela primeira vez em uma determinada execução do  
o aplicativo, o Entity Framework compara o banco de dados para o modelo (seu `SchoolContext` e classes de entidade). Se houver uma diferença, o aplicativo descarta e recria o banco de dados.

> [!NOTE]
> Quando você implanta um aplicativo em um servidor web de produção, você deve remover ou desabilitar o código que descarta e recria o banco de dados. Isso será feito que em um tutorial posterior nesta série.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configurar o EF para usar um banco de dados do SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) é uma versão leve do mecanismo de banco de dados do SQL Server Express. É fácil de instalar e configurar, iniciado sob demanda e é executado no modo de usuário. LocalDB é executado em um modo especial de execução do SQL Server Express que permite que você trabalhe com bancos de dados como *. mdf* arquivos. Você pode colocar arquivos de banco de dados LocalDB o *aplicativo\_dados* pasta de um projeto da web se você deseja ser capaz de copiar o banco de dados com o projeto. O recurso de instância de usuário do SQL Server Express também permite que você trabalhe com *. mdf* arquivos, mas o recurso de instância de usuário são preteridos; portanto, o LocalDB é recomendado para trabalhar com *. mdf* arquivos. No Visual Studio 2012 e versões posteriores, o LocalDB é instalado por padrão com o Visual Studio.

Normalmente o SQL Server Express não é usado para aplicativos da web de produção. LocalDB em particular não é recomendável para uso em produção com um aplicativo web porque ele não foi projetado para trabalhar com o IIS.

Neste tutorial, você trabalhará com LocalDB. Abra o aplicativo *Web. config* e adicione um `connectionStrings` elemento anterior a `appSettings` elemento, conforme mostrado no exemplo a seguir. (Certifique-se de atualizar o *Web. config* arquivo na pasta raiz do projeto. Também há um *Web. config* arquivo está no *exibições* subpasta que você não precisa atualizar.)

Se você estiver usando o Visual Studio 2015, substitua "v 11.0" na cadeia de conexão "MSSQLLocalDB", pois o nome da instância padrão do SQL Server foi alterado.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

A cadeia de caracteres de conexão que você adicionou Especifica que o Entity Framework usará um banco de dados LocalDB denominado *ContosoUniversity1.mdf*. (O banco de dados ainda não existe; EF vai criá-lo.) Se desejar que o banco de dados a serem criados no seu *aplicativo\_dados* pasta, você pode adicionar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` à cadeia de conexão. Para obter mais informações sobre cadeias de caracteres de conexão, consulte [cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Na verdade, não é necessário ter uma cadeia de caracteres de conexão no *Web. config* arquivo. Se você não fornecer uma cadeia de caracteres de conexão, o Entity Framework usará um padrão de um com base em sua classe de contexto. Para obter mais informações, consulte [Code First para um novo banco de dados](https://msdn.microsoft.com/en-us/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Criando um controlador de estudante e exibições

Agora você criará uma página da web para exibir dados e o processo de solicitação de dados irá disparar automaticamente  
a criação do banco de dados. Você começará com a criação de um novo controlador. Mas, antes de fazer isso, compile o projeto para disponibilizar as classes de modelo e o contexto de scaffolding do controlador MVC.

1. Com o botão direito do **controladores** pasta **Solution Explorer**, selecione **adicionar**e, em seguida, clique em **Novo Item de Scaffold**.
- No **adicionar Scaffold** caixa de diálogo, selecione **controlador MVC 5 com modos de exibição usando o Entity Framework**.

    ![Adicionar Scaffold](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- Na caixa de diálogo Adicionar controlador, faça as seguintes seleções e, em seguida, clique em **adicionar**:

    - Classe de modelo: **aluno (ContosoUniversity.Models)**. (Se você não vir essa opção na lista suspensa, compile o projeto e tente novamente.)
    - Classe de contexto de dados: **SchoolContext (ContosoUniversity.DAL)**.
    - Nome do controlador: **StudentController** (não StudentsController).
    - Deixe os valores padrão para os outros campos.

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Quando você clica em **adicionar**, o scaffolder cria um arquivo de StudentController.cs e um conjunto de exibições (arquivos. cshtml) que funcionam com o controlador. No futuro, quando você cria projetos que usam o Entity Framework você pode também aproveitar algumas funcionalidades adicionais do scaffolder: basta criar sua primeira classe do modelo, não crie uma cadeia de caracteres de conexão e, em seguida, no **Adicionar controlador** caixa Especifique a nova classe de contexto. O scaffolder criará seu `DbContext` classe e sua conexão de cadeia de caracteres, bem como o controlador e os modos de exibição.
- O Visual Studio abrirá o *Controllers\StudentController.cs* arquivo. Você verá que foi criada uma variável de classe que instancia um objeto de contexto do banco de dados:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    O `Index` método de ação obtém uma lista dos alunos do *alunos* entidade definida pela leitura de `Students` propriedade da instância de contexto do banco de dados:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    O *Student\Index.cshtml* exibe essa lista em uma tabela:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- Pressione CTRL+F5 para executar o projeto. (Se você receber um erro "Não é possível criar a cópia de sombra", feche o navegador e tente novamente.)

    Clique o **alunos** guia para ver os dados de teste que o `Seed` método inserido. Dependendo de como estreita a janela do navegador é, você verá o link do guia do aluno na barra de endereços superior ou você terá de clicar o canto superior direito para ver o link.

    ![Botão de menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![Página de índice do aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Exibir o banco de dados

Quando você executou a página de alunos e o aplicativo tentou acessar o banco de dados, o EF viu que não houve nenhum banco de dados e e criou um, em seguida, executar o método de propagação para preencher o banco de dados.

Você pode usar o **Server Explorer** ou **Pesquisador de objetos do SQL Server** (SSOX) para exibir o banco de dados no Visual Studio. Neste tutorial você usará **Server Explorer**. (No Visual Studio Express edições anteriores a 2013, **Server Explorer** é chamado **Pesquisador de objetos de banco de dados**.)

1. Feche o navegador.
2. Em **Server Explorer**, expanda **conexões de dados**, expanda **escola contexto (ContosoUniversity)**e, em seguida, expanda **tabelas** para ver as tabelas no novo banco de dados.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Clique com botão direito do **aluno** de tabela e clique em **Mostrar dados da tabela** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

    ![Tabela do aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Fechar o **Server Explorer** conexão.

O *ContosoUniversity1.mdf* e *. ldf* arquivos de banco de dados estão no `C:\Users\<yourusername>` pasta.

Porque você está usando o `DropCreateDatabaseIfModelChanges` inicializador, agora você pode fazer uma alteração de `Student` classe, executar novamente o aplicativo e o banco de dados será automaticamente ser recriado para corresponder a alteração. Por exemplo, se você adicionar um `EmailAddress` propriedade para o `Student` classe, execute novamente a página de alunos e, em seguida, examine a tabela novamente, você verá um novo `EmailAddress` coluna.

## <a name="conventions"></a>Convenções

A quantidade de código, você precisava criar para que o Entity Framework para poder criar um banco de dados completo para você é mínima devido ao uso de *convenções*, ou suposições que faz com que o Entity Framework. Algumas delas já foi observadas ou eram usadas sem seu conhecimento deles sendo:

- Os formulários pluralized de nomes de classes de entidade são usados como nomes de tabela.
- Nomes de propriedade de entidade são usados para nomes de coluna.
- Propriedades de entidade que são nomeadas `ID` ou *classname* `ID` são reconhecidos como propriedades de chave primárias.
- Uma propriedade é interpretada como uma propriedade de chave estrangeira, se ele é nomeado  *&lt;nome da propriedade de navegação&gt;&lt;nome de propriedade de chave primária&gt;*  (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` chave primária da entidade é `ID`). Propriedades de chave estrangeira podem também ser o mesmo nome simplesmente &lt;nome de propriedade de chave primária&gt; (por exemplo, `EnrollmentID` desde o `Enrollment` chave primária da entidade é `EnrollmentID`).

Você viu que as convenções podem ser substituídas. Por exemplo, você especificou que os nomes de tabela não devem ser pluralized, e você verá posteriormente como marcar explicitamente uma propriedade como uma propriedade de chave estrangeira. Você aprenderá mais sobre as convenções e como substituí-las no [criando um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial posteriormente na série. Para obter mais informações sobre convenções, consulte [convenções de código primeiro](https://msdn.microsoft.com/en-us/data/jj679962).

## <a name="summary"></a>Resumo

Agora você criou um aplicativo simples que usa o Entity Framework e o SQL Server Express LocalDB para armazenar e exibir dados. O tutorial a seguir, você aprenderá como executar básica CRUD (criar, ler, atualizar e excluir) operações.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Avançar](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
