---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC (1 a 10) | Microsoft Docs
author: tdykstra
description: Uma versão mais recente desta série tutorial está disponível, para o Visual Studio 2013, Entity Framework 6 e MVC 5. A definição de aplicativo de web de exemplo Contoso University...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877925"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC (1 a 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Um [versão mais recente do que esta série de tutoriais](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) está disponível para o Visual Studio 2013, Entity Framework 6 e MVC 5.
> 
> 
> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 e o Visual Studio 2012. O aplicativo de exemplo é um site de uma Contoso University fictícia. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor. Esta série de tutorial explica como construir o aplicativo de exemplo Contoso University. Você pode [Baixe o aplicativo concluído](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Há três maneiras que você pode trabalhar com dados no Entity Framework: *Database First*, *Model First*, e *Code First*. Este tutorial destina Code First. Para obter informações sobre as diferenças entre esses fluxos de trabalho e orientação sobre como escolher a melhor para seu cenário, consulte [fluxos de trabalho de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> O aplicativo de exemplo se baseia no [ASP.NET MVC](../../../index.md). Se você preferir trabalhar com o modelo de Web Forms do ASP.NET, consulte o [modelo de associação e formulários da Web](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) série de tutoriais e [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versões de software
> 
> | **Mostrado no tutorial** | **Também funciona com** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | O Visual Studio 2012 Express para Web. Isso é instalado automaticamente pelo SDK do Windows Azure se você ainda não tiver VS 2012 ou VS 2012 Express para Web. Visual Studio 2013 deve funcionar, mas o tutorial não foi testado com ele, e algumas opções de menu e caixas de diálogo são diferentes. O [VS 2013 versão do SDK do Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323510) é necessário para a implantação do Windows Azure. |
> | .NET 4.5 | A maioria dos recursos mostrados funcionará no .NET 4, mas alguns não. Por exemplo, o suporte a enum no EF requer o .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Se você ignorar as etapas de implantação do Windows Azure, o SDK não é necessário. Quando uma nova versão do SDK for lançada, o link instalará a versão mais recente. Nesse caso, você talvez precise adaptar algumas das instruções para a nova interface do usuário e recursos. |
> 
> ## <a name="questions"></a>Perguntas
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), o [do Entity Framework e LINQ to Fórum de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Agradecimentos
> 
> Consulte o tutorial último na série de [confirmações e uma observação sobre VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Versão original do tutorial
> 
> A versão original do tutorial está disponível na [EF 4.1 / livro eletrônico do MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>O aplicativo da Web da Contoso University

O aplicativo que você criará nestes tutoriais é um site simples de uma universidade.

Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores. Estas são algumas das telas que você criará.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

O estilo de interface do usuário desse site foi mantido perto do que é gerado pelos modelos internos, de modo que o tutorial possa se concentrar principalmente em como usar o Entity Framework.

## <a name="prerequisites"></a>Pré-requisitos

As instruções e capturas de tela neste tutorial presumem que você está usando [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) ou [Visual Studio 2012 Express para Web](https://go.microsoft.com/fwlink/?LinkID=275131)com a atualização mais recente e o SDK do Azure para .NET instalado a partir de julho, 2013. Você pode obter tudo isso com o seguinte link:

[SDK do Azure para .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Se você tiver instalado o Visual Studio, no link acima instalará todos os componentes ausentes. Se você não tiver o Visual Studio, o link será instalado o Visual Studio 2012 Express para Web. Você pode usar o Visual Studio 2013, mas alguns dos procedimentos necessários e telas serão diferentes.

## <a name="create-an-mvc-web-application"></a>Criar um aplicativo MVC

Abra o Visual Studio e crie um novo projeto c# chamado "ContosoUniversity" usando o **aplicativo Web do ASP.NET MVC 4** modelo. Verifique se você direcionar **.NET Framework 4.5** (você usará [ `enum` propriedades](https://msdn.microsoft.com/data/hh859576.aspx), e que requer o .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

No **novo projeto ASP.NET MVC 4** select da caixa de diálogo de **aplicativo de Internet** modelo.

Deixe o **Razor** exibir mecanismo selecionado e deixar o **criar um projeto de teste de unidade** caixa de seleção está desmarcada.

Clique em **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Definir o estilo de Site

Algumas alterações simples configurarão o menu do site, o layout e a home page.

Abra *exibições \ compartilhadas\\cshtml*e substitua o conteúdo do arquivo com o código a seguir. As alterações são realçadas.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Este código faz as seguintes alterações:

- Substitui as instâncias do modelo de "Meu aplicativo ASP.NET MVC" e "seu logotipo aqui" por "Contoso University".
- Adiciona vários links de ação que serão usados posteriormente no tutorial.

Em *Views\Home\Index.cshtml*, substitua o conteúdo do arquivo com o código a seguir para eliminar os parágrafos do modelo sobre o ASP.NET e MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Em *Controllers\HomeController.cs*, altere o valor de `ViewBag.Message` no `Index` método de ação "Bem-vindo à Contoso University!", conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Pressione CTRL + F5 para executar o site. Você ver a página inicial com o menu principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo Contoso University. Comece com as três seguintes entidades:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`, e uma relação um-para-muitos entre as entidades `Course` e `Enrollment`. Em outras palavras, um aluno pode ser registrado em qualquer quantidade de cursos e um curso pode ter qualquer quantidade de alunos registrados.

Nas seções a seguir, você criará uma classe para cada uma dessas entidades.

> [!NOTE]
> Se você tentar compilar o projeto antes de terminar a criação de todas essas classes de entidade, você receberá erros do compilador.


### <a name="the-student-entity"></a>A entidade do aluno

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

No *modelos* pasta, criar *Student.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

A propriedade `StudentID` se tornará a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o Entity Framework interpreta uma propriedade denominada `ID` ou *classname* `ID` como a chave primária.

O `Enrollments` propriedade é um *propriedade de navegação*. As propriedades de navegação armazenam outras entidades que estão relacionadas a essa entidade. Nesse caso, o `Enrollments` propriedade de um `Student` entidade conterá todos os `Enrollment` entidades relacionadas ao `Student` entidade. Em outras palavras, se um determinado `Student` linha no banco de dados tem duas relacionadas `Enrollment` linhas (valor de linhas que contêm a chave primária que student em seus `StudentID` coluna de chave estrangeira), que `Student` da entidade `Enrollments` propriedade de navegação conterá dois `Enrollment` entidades.

Propriedades de navegação costumam ser definidas como `virtual` para que eles podem tirar proveito de alguns recursos do Entity Framework, como *carregamento preguiçoso*. (Carregamento preguiçoso será explicado mais adiante, a [dados relacionados de leitura](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente na série.

Se uma propriedade de navegação pode armazenar várias entidades (como em relações muitos para muitos ou um-para-muitos), o tipo precisa ser uma lista na qual entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection`.

### <a name="the-enrollment-entity"></a>A entidade de registro

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Na pasta *Models*, crie *Enrollment.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

A propriedade de classificação é uma [enum](https://msdn.microsoft.com/data/hh859576.aspx). O ponto de interrogação após o `Grade` declaração de tipo indica que o `Grade` é de propriedade [anulável](https://msdn.microsoft.com/library/2cf62fcy.aspx). Uma classificação que é null é diferente de uma classificação zero — null significa que um nível não é conhecida ou ainda não foi atribuído.

A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Uma entidade `Enrollment` é associada a uma entidade `Student`, de modo que a propriedade possa armazenar apenas uma única entidade `Student` (ao contrário da propriedade de navegação `Student.Enrollments` que você viu anteriormente, que pode armazenar várias entidades `Enrollment`).

A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Uma entidade `Enrollment` está associada a uma entidade `Course`.

### <a name="the-course-entity"></a>A entidade de curso

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

No *modelos* pasta, criar *Course.cs*, substituindo o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

A propriedade `Enrollments` é uma propriedade de navegação. Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.

Podemos dizer mais sobre o [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). None)] o atributo no tutorial Avançar. Basicamente, esse atributo permite que você insira a chave primária do curso, em vez de fazer com que ela seja gerada pelo banco de dados.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena a funcionalidade do Entity Framework para um modelo de dados é o *contexto de banco de dados* classe. Crie esta classe derivando de [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. No código, especifique quais entidades são incluídas no modelo de dados. Também personalize o comportamento específico do Entity Framework. Neste projeto, a classe é chamada `SchoolContext`.

Crie uma pasta chamada *DAL* (para a camada de acesso a dados). Nessa pasta, crie um novo arquivo de classe chamado *SchoolContext.cs*e substitua o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Esse código cria um [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) propriedade para cada conjunto de entidades. Na terminologia do Entity Framework, um *conjunto de entidades* normalmente corresponde a uma tabela de banco de dados e um *entidade* corresponde a uma linha na tabela.

O `modelBuilder.Conventions.Remove` instrução o [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impede que os nomes de tabela que está sendo pluralized. Se você não tiver feito isso, as tabelas geradas serão chamadas `Students`, `Courses`, e `Enrollments`. Em vez disso, os nomes de tabela serão `Student`, `Course`, e `Enrollment`. Os desenvolvedores não concordam sobre se os nomes de tabela devem ser pluralizados ou não. Este tutorial usa a forma singular, mas o ponto importante é que você pode selecionar qualquer formato que preferir, incluindo ou omitir esta linha de código.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) é uma versão leve do SQL Server Express Database Engine que é iniciado sob demanda e executado no modo de usuário. LocalDB é executado em um modo especial de execução do SQL Server Express que permite que você trabalhe com bancos de dados como *. mdf* arquivos. Normalmente, os arquivos de banco de dados LocalDB são mantidos no *aplicativo\_dados* pasta de um projeto da web. O recurso de instância de usuário do SQL Server Express também permite que você trabalhe com *. mdf* arquivos, mas o recurso de instância de usuário são preteridos; portanto, o LocalDB é recomendado para trabalhar com *. mdf* arquivos.

Normalmente o SQL Server Express não é usado para aplicativos da web de produção. LocalDB em particular não é recomendável para uso em produção com um aplicativo web porque ele não foi projetado para trabalhar com o IIS.

No Visual Studio 2012 e versões posteriores, o LocalDB é instalado por padrão com o Visual Studio. No Visual Studio 2010 e versões anteriores, o SQL Server Express (sem LocalDB) é instalado por padrão com o Visual Studio; Você precisa instalá-lo manualmente, se você estiver usando o Visual Studio 2010.

Neste tutorial você trabalhará com LocalDB para que o banco de dados pode ser armazenado na *aplicativo\_dados* pasta como um *. mdf* arquivo. Abra a raiz *Web. config* de arquivos e adicionar uma nova cadeia de conexão para o `connectionStrings` coleção, conforme mostrado no exemplo a seguir. (Certifique-se de atualizar o *Web. config* arquivo na pasta raiz do projeto. Também há um *Web. config* arquivo está no *exibições* subpasta que você não precisa atualizar.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Por padrão, o Entity Framework procura uma cadeia de caracteres de conexão que o mesmo nomeada que o `DbContext` classe (`SchoolContext` para este projeto). A cadeia de caracteres de conexão que você adicionou Especifica um banco de dados LocalDB denominado *ContosoUniversity.mdf* localizado no *aplicativo\_dados* pasta. Para obter mais informações, consulte [cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Na verdade, você não precisa especificar a cadeia de caracteres de conexão. Se você não fornecer uma cadeia de caracteres de conexão, o Entity Framework criará um para você. No entanto, o banco de dados não pode estar no *aplicativo\_dados* pasta do seu aplicativo. Para obter informações sobre onde o banco de dados será criado, consulte [Code First para um novo banco de dados](https://msdn.microsoft.com/data/jj193542).

O `connectionStrings` coleção também tem uma cadeia de caracteres de conexão chamada `DefaultConnection` que é usada para o banco de dados de associação. Você não irá usar o banco de dados de associação neste tutorial. A única diferença entre as cadeias de caracteres de dois conexão é o nome do banco de dados e o valor do atributo de nome.

## <a name="set-up-and-execute-a-code-first-migration"></a>Configurar e executar uma migração de primeiro do código

Quando você começa a desenvolver um aplicativo, seus dados de alterações do modelo com frequência e cada vez que as alterações do modelo obtém fora de sincronia com o banco de dados. Você pode configurar o Entity Framework para descartar e recriar o banco de dados cada vez que você alterar o modelo de dados automaticamente. Isso não é um problema no início do desenvolvimento porque os dados de teste são facilmente recriados, mas depois de ter implantado para produção normalmente você deseja atualizar o esquema de banco de dados sem descartar o banco de dados. O recurso de migrações permite Code First atualizar o banco de dados sem descartar e recriá-lo. No início do ciclo de desenvolvimento de um novo projeto, você talvez queira usar [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) para descarte, recriar e propagar novamente o banco de dados cada vez que as alterações do modelo. Você obtém pronto para implantar seu aplicativo, você pode converter para a abordagem de migrações. Neste tutorial você usará apenas migrações. Para obter mais informações, consulte [migrações do Code First](https://msdn.microsoft.com/data/jj591621) e [migrações Screencast série](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Habilitar migrações do Code First

1. Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote** e **Package Manager Console**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. No `PM>` prompt digite o seguinte comando:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![comando enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Este comando cria um *migrações* pasta no projeto ContosoUniversity e ele coloca na pasta um *Configuration.cs* arquivo que você pode editar para configurar as migrações.

    ![Pasta de migrações](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    O `Configuration` classe inclui um `Seed` método é chamado quando o banco de dados é criado e sempre que ele for atualizado depois que um modelo de dados de alterações.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    O objetivo desse `Seed` método é habilitar a inserção de dados de teste no banco de dados depois que cria ou atualiza Code First.

### <a name="set-up-the-seed-method"></a>Configurar o método de propagação

O [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método é executado quando migrações do Code First cria o banco de dados e toda vez que ele atualiza o banco de dados de migração mais recente. A finalidade do método de propagação é para que você possa inserir dados em tabelas antes que o aplicativo acessa o banco de dados pela primeira vez.

Em versões anteriores do Code First, antes de migrações foi lançado, era comum `Seed` métodos para inserir dados de teste, porque cada alteração de modelo durante o desenvolvimento de banco de dados completamente excluído e recriado do zero. Com migrações do Code First, teste os dados são mantidos após as alterações do banco de dados, portanto, incluindo dados de teste no [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método normalmente não é necessário. Na verdade, você não deseja o `Seed` método para inserir dados de teste, se você estiver usando migrações para implantar o banco de dados em produção, pois o `Seed` método será executado em produção. Nesse caso, você deseja o `Seed` método para inserir o banco de dados somente os dados que você deseja ser inserida na produção. Por exemplo, convém que o banco de dados para incluir nomes de departamento real no `Department` tabela quando o aplicativo está disponível em produção.

Para este tutorial, você usará as migrações para implantação, mas sua `Seed` método irá inserir dados de teste mesmo assim para tornar mais fácil ver como a funcionalidade do aplicativo funciona sem a necessidade de inserir manualmente muitos dados.

1. Substitua o conteúdo do *Configuration.cs* arquivo com o código a seguir, que irá carregar dados de teste para o novo banco de dados. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    O [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método usa o objeto de contexto do banco de dados como um parâmetro de entrada e o código no método usa esse objeto para adicionar novas entidades no banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades, adiciona-os ao apropriado [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) propriedade e, em seguida, salva as alterações no banco de dados. Não é necessário chamar o [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método depois de cada grupo de entidades, como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto estiver gravando o código para o banco de dados.

    Algumas das instruções que inserem dados usam o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método para executar uma operação "upsert". Porque o `Seed` método é executado com cada migração, você não pode inserir apenas os dados, porque as linhas que você está tentando adicionar já estará lá após a migração primeiro que cria o banco de dados. A operação "upsert" evita erros que acontecem se você tentar inserir uma linha que já existe, mas ***substitui*** quaisquer alterações nos dados que você fez ao testar o aplicativo. Com dados de teste em algumas tabelas talvez você não queira que isso aconteça: em alguns casos quando você altera dados ao testar deseja suas alterações para permanecer após as atualizações do banco de dados. Nesse caso você deseja fazer uma operação de inserção condicional: inserir uma linha apenas se ele ainda não existir. O método de propagação usa as duas abordagens.

    O primeiro parâmetro passado para o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método Especifica a propriedade a ser usado para verificar se uma linha já existe. Para os dados de aluno de teste que você fornecer, o `LastName` propriedade pode ser usada para essa finalidade, pois cada sobrenome na lista é exclusivo:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Esse código supõe que os sobrenomes sejam exclusivos. Se você adicionar manualmente um aluno com um nome duplicado de último, você receberá a seguinte exceção na próxima vez que você executar uma migração.

    A sequência contém mais de um elemento

    Para obter mais informações sobre o `AddOrUpdate` método, consulte [tome cuidado com EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

    O código que adiciona `Enrollment` entidades não usa o `AddOrUpdate` método. Ele verifica se uma entidade já existe e insere a entidade se ele não existir. Essa abordagem preserva as alterações feitas em um nível de registro ao executar migrações. O código executa um loop em cada membro de `Enrollment` [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) e se o registro não foi encontrado no banco de dados, ele adiciona o registro no banco de dados. Na primeira vez que você atualizar o banco de dados, o banco de dados estará vazio, portanto ele adicionará cada registro.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Para obter informações sobre como depurar o `Seed` método e a manipulação de dados redundantes, como dois alunos denominados "Alexander Carson", consulte [Seeding e bancos de dados de depuração Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) no blog de Rick Anderson.
2. Compile o projeto.

### <a name="create-and-execute-the-first-migration"></a>Criar e executar a migração primeiro

1. Na janela do Console do Gerenciador de pacotes, digite os seguintes comandos: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    O `add-migration` comando adiciona a pasta Migrations um *[DateStamp]\_InitialCreate.cs* arquivo que contém o código que cria o banco de dados. O primeiro parâmetro (`InitialCreate)` é usado para o arquivo de nome e pode ser tudo o que você quiser; normalmente escolher uma palavra ou frase que resume o que está sendo feito a migração. Por exemplo, você pode nomear uma migração posterior &quot;AddDepartmentTable&quot;.

    ![Pasta Migrations com migração inicial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    O `Up` método o `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidade do modelo de dados, e o `Down` método exclui-los. As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, as Migrações chamam o método `Down`. O código a seguir mostra o conteúdo do `InitialCreate` arquivo:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    O `update-database` comando executa o `Up` método para criar o banco de dados e, em seguida, ele é executado o `Seed` para popular o banco de dados.

Um banco de dados do SQL Server foi criado para o modelo de dados. O nome do banco de dados é *ContosoUniversity*e o *. mdf* arquivo está no seu projeto *aplicativo\_dados* pasta porque esse é o que você especificou no seu cadeia de caracteres de conexão.

Você pode usar o **Server Explorer** ou **Pesquisador de objetos do SQL Server** (SSOX) para exibir o banco de dados no Visual Studio. Neste tutorial você usará **Server Explorer**. No Visual Studio Express 2012 para Web, **Server Explorer** é chamado **Pesquisador de objetos de banco de dados**.

1. Do **exibição** menu, clique em **Server Explorer**.
2. Clique o **Adicionar Conexão** ícone.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Se você for solicitado com o **Escolher fonte de dados** caixa de diálogo, clique em **Microsoft SQL Server**e, em seguida, clique em **continuar**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. No **Adicionar Conexão** caixa de diálogo, digite **(localdb) \v11.0** para o **nome do servidor**. Em **selecionar ou digitar um nome de banco de dados**, selecione **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Clique em **OK**
6. Expanda **SchoolContext** e, em seguida, expanda **tabelas**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Clique com botão direito do **aluno** de tabela e clique em **Mostrar dados da tabela** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

    ![Tabela do aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Criando um controlador de estudante e exibições

A próxima etapa é criar um ASP.NET MVC controlador e modos de exibição em seu aplicativo que possa trabalhar com uma dessas tabelas.

1. Para criar um `Student` controlador, clique o **controladores** pasta **Solution Explorer**, selecione **adicionar**e, em seguida, clique em **controlador** . No **Adicionar controlador** caixa de diálogo caixa, faça as seguintes seleções e, em seguida, clique em **adicionar**: 

   - Nome do controlador: **StudentController**.
   - Modelo: **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework**.
   - Classe de modelo: **aluno (ContosoUniversity.Models)**. (Se você não vir essa opção na lista suspensa, compile o projeto e tente novamente.)
   - Classe de contexto de dados: **SchoolContext (ContosoUniversity.Models)**.
   - Modos de exibição: **Razor (CSHTML)**. (O padrão).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. O Visual Studio abrirá o *Controllers\StudentController.cs* arquivo. Você verá que foi criada uma variável de classe que instancia um objeto de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     O `Index` método de ação obtém uma lista dos alunos do *alunos* entidade definida pela leitura de `Students` propriedade da instância de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     O *Student\Index.cshtml* exibe essa lista em uma tabela:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Pressione CTRL+F5 para executar o projeto.

     Clique o **alunos** guia para ver os dados de teste que o `Seed` método inserido.

     ![Página de índice do aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Convenções

A quantidade de código, você precisava criar para que o Entity Framework para poder criar um banco de dados completo para você é mínima devido ao uso de *convenções*, ou suposições que faz com que o Entity Framework. Algumas delas já foram observadas:

- Os formulários pluralized de nomes de classes de entidade são usados como nomes de tabela.
- Os nomes de propriedade de entidade são usados para nomes de coluna.
- Propriedades de entidade que são nomeadas `ID` ou *classname* `ID` são reconhecidos como propriedades de chave primárias.

Você viu que as convenções podem ser substituídas (por exemplo, especificado que os nomes de tabela não devem ser pluralized), e você aprenderá mais sobre as convenções e como substituí-las no [criando um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial mais tarde na série. Para obter mais informações, consulte [convenções de código primeiro](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Resumo

Agora você criou um aplicativo simples que usa o Entity Framework e o SQL Server Express para armazenar e exibir dados. O tutorial a seguir, você aprenderá como executar básica CRUD (criar, ler, atualizar e excluir) operações. Você pode deixar os comentários na parte inferior desta página. Informe-nos como você gostou esta parte do tutorial e como podemos pode melhorá-lo.

Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Avançar](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
