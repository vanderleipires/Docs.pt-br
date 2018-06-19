---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Código primeiro migrações e implantação com o Entity Framework em um aplicativo ASP.NET MVC | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879550"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Código primeiro migrações e implantação com o Entity Framework em um aplicativo ASP.NET MVC
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Até o aplicativo está sendo executado localmente no IIS Express no computador de desenvolvimento. Para disponibilizar um aplicativo real para outros usuários na Internet, você precisa implantá-lo em um provedor de hospedagem na web. Neste tutorial, você implantará o aplicativo Contoso University para a nuvem no Azure.

O tutorial contém as seguintes seções:

- Habilite migrações do Code First. O recurso de migrações permite alterar o modelo de dados e implantar as alterações na produção, atualizando o esquema de banco de dados sem a necessidade de descartar e recriar o banco de dados.
- Implante no Azure. Esta etapa é opcional. Você pode continuar com os tutoriais restantes sem ter implantado o projeto.

É recomendável que você usar um processo de integração contínua com o controle de origem para a implantação, mas este tutorial não abrange os tópicos. Para obter mais informações, consulte o [controle de origem](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) e [integração contínua](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) capítulos a [criando aplicativos de nuvem do mundo Real com o Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) livro eletrônico.

## <a name="enable-code-first-migrations"></a>Habilitar migrações do Code First

Quando você desenvolve um novo aplicativo, o modelo de dados é alterado com frequência e, sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dados. Você configurou o Entity Framework para descartar e recriar o banco de dados cada vez que você alterar o modelo de dados automaticamente. Quando você adicionar, remover, ou alterar as classes de entidade ou alterar seu `DbContext` classe, na próxima vez que você executar o aplicativo automaticamente exclui o banco de dados existente, cria um novo que corresponde ao modelo e propaga-lo com dados de teste.

Esse método de manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implante o aplicativo em produção. Quando o aplicativo é executado em produção, normalmente ele está armazenando dados que você deseja manter, e você não quiser perder tudo o que cada vez que você fizer uma alteração, como adicionar uma nova coluna. O [migrações do Code First](https://msdn.microsoft.com/data/jj591621) recurso resolve esse problema, permitindo Code First atualizar o esquema de banco de dados em vez de descartar e recriar o banco de dados. Neste tutorial, você implantará o aplicativo e para se preparar para isso habilitará as migrações.

1. Desabilitar o inicializador de que você configurou anteriormente comentar ou excluindo o `contexts` elemento que você adicionou ao arquivo Web. config do aplicativo.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Também no aplicativo *Web. config* arquivo, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Essa alteração configura o projeto, de modo que a primeira migração crie um novo banco de dados. Isso não é obrigatório, mas você verá posteriormente por que ele é uma boa ideia.
3. Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote** e **Package Manager Console**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. No `PM>` prompt digite os seguintes comandos:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![comando enable-migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    O `enable-migrations` comando cria um *migrações* pasta no projeto ContosoUniversity e ele coloca na pasta um *Configuration.cs* arquivo que você pode editar para configurar as migrações.

    (Caso você tenha perdido a etapa acima que direciona você para alterar o nome do banco de dados, migrações encontrará o banco de dados existente e faz automaticamente o `add-migration` comando. Okey, significa apenas que você não executa um teste do código migrações antes de implantar o banco de dados. Posteriormente, quando você executar o `update-database` comando nada acontece porque o banco de dados já existirá.)

    ![Pasta de migrações](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Como a classe de inicializador que vimos anteriormente, o `Configuration` classe inclui um `Seed` método.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    A finalidade de [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método é para que você possa inserir ou atualizar dados de teste depois que cria ou atualiza o banco de dados Code First. O método é chamado quando o banco de dados é criado e sempre que o esquema de banco de dados é atualizado após a alteração de um modelo de dados.

### <a name="set-up-the-seed-method"></a>Configurar o método de propagação

Quando você está descartando e recriando o banco de dados para cada alteração do modelo de dados, use a classe de inicializador `Seed` método para inserir os dados de teste, porque depois de cada alteração de modelo de banco de dados é descartado e todos os dados de teste serão perdidos. Com migrações do Code First, teste os dados são mantidos após as alterações do banco de dados, portanto, incluindo dados de teste no [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método normalmente não é necessário. Na verdade, você não deseja o `Seed` método para inserir dados de teste, se você estiver usando migrações para implantar o banco de dados em produção, pois o `Seed` método será executado em produção. Nesse caso, você deseja o `Seed` método para inserir o banco de dados somente os dados que você precisa em produção. Por exemplo, convém que o banco de dados para incluir nomes de departamento real no `Department` tabela quando o aplicativo está disponível em produção.

Para este tutorial, você usará as migrações para implantação, mas sua `Seed` método irá inserir dados de teste mesmo assim para tornar mais fácil ver como a funcionalidade do aplicativo funciona sem a necessidade de inserir manualmente muitos dados.

1. Substitua o conteúdo do *Configuration.cs* arquivo com o código a seguir, que irá carregar dados de teste para o novo banco de dados. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    O [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método usa o objeto de contexto do banco de dados como um parâmetro de entrada e o código no método usa esse objeto para adicionar novas entidades no banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades, adiciona-os ao apropriado [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) propriedade e, em seguida, salva as alterações no banco de dados. Não é necessário chamar o [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método depois de cada grupo de entidades, como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto estiver gravando o código para o banco de dados.

    Algumas das instruções que inserem dados usam o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método para executar uma operação "upsert". Porque o `Seed` método é executado sempre que você executa o `update-database` comando normalmente após cada migração, você não pode inserir apenas os dados, porque as linhas que você está tentando adicionar já estará lá após a migração primeiro que cria o banco de dados. A operação "upsert" evita erros que acontecem se você tentar inserir uma linha que já existe, mas ***substitui*** quaisquer alterações nos dados que você fez ao testar o aplicativo. Com dados de teste em algumas tabelas talvez você não queira que isso aconteça: em alguns casos quando você altera dados ao testar deseja suas alterações para permanecer após as atualizações do banco de dados. Nesse caso você deseja fazer uma operação de inserção condicional: inserir uma linha apenas se ele ainda não existir. O método de propagação usa as duas abordagens.

    O primeiro parâmetro passado para o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método Especifica a propriedade a ser usado para verificar se uma linha já existe. Para os dados de aluno de teste que você fornecer, o `LastName` propriedade pode ser usada para essa finalidade, pois cada sobrenome na lista é exclusivo:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Esse código supõe que os sobrenomes sejam exclusivos. Se você adicionar manualmente um aluno com um nome duplicado de último, você receberá a seguinte exceção na próxima vez que você executar uma migração.

    A sequência contém mais de um elemento

    Para obter informações sobre como lidar com dados redundantes, como dois alunos denominados "Alexander Carson", consulte [Seeding e bancos de dados de depuração Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) no blog de Rick Anderson. Para obter mais informações sobre o `AddOrUpdate` método, consulte [tome cuidado com EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

    O código que cria `Enrollment` entidades pressupõe que você tenha o `ID` valor nas entidades no `students` coleção, embora você não definir essa propriedade no código que cria a coleção.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Você pode usar o `ID` propriedade aqui porque o `ID` valor é definido quando você chamar `SaveChanges` para o `students` coleção. EF automaticamente obtém o valor de chave primária quando insere uma entidade no banco de dados e atualiza o `ID` propriedade da entidade na memória.

    O código que adiciona cada `Enrollment` entidade para o `Enrollments` não usa o conjunto de entidades de `AddOrUpdate` método. Ele verifica se uma entidade já existe e insere a entidade se ele não existir. Essa abordagem preserva as alterações feitas em um nível de registro usando o interface do usuário do aplicativo. O código executa um loop em cada membro de `Enrollment` [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) e se o registro não foi encontrado no banco de dados, ele adiciona o registro no banco de dados. Na primeira vez que você atualizar o banco de dados, o banco de dados estará vazio, portanto ele adicionará cada registro.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Compile o projeto.

### <a name="execute-the-first-migration"></a>Executar a migração primeiro

Quando você executou o `add-migration` de comando, migrações gerado o código que cria o banco de dados do zero. Esse código também está no *migrações* pasta, no arquivo nomeado  *&lt;timestamp&gt;\_InitialCreate.cs*. O `Up` método o `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidade do modelo de dados, e o `Down` método exclui-los.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, as Migrações chamam o método `Down`.

Isso é a migração inicial que foi criada quando você inseriu o `add-migration InitialCreate` comando. O parâmetro (`InitialCreate` no exemplo) é usado para o arquivo de nome e pode ser tudo o que você quiser; normalmente escolher uma palavra ou frase que resume o que está sendo feito a migração. Por exemplo, você pode nomear uma migração posterior &quot;AddDepartmentTable&quot;.

Se você criou a migração inicial quando o banco de dados já existia, o código de criação de banco de dados é gerado, mas ele não precisa ser executado porque o banco de dados já corresponde ao modelo de dados. Quando você implantar o aplicativo em outro ambiente no qual o banco de dados ainda não existe, esse código será executado para criar o banco de dados; portanto, é uma boa ideia testá-lo primeiro. É por isso que você alterou o nome do banco de dados na cadeia de conexão anteriormente – para que as migrações possam criar um novo do zero.

1. No **Package Manager Console** janela, digite o seguinte comando:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    O `update-database` comando executa o `Up` método para criar o banco de dados e, em seguida, ele é executado o `Seed` para popular o banco de dados. O mesmo processo será executado automaticamente em produção depois que você implantar o aplicativo, como você verá na seção a seguir.
2. Use **Server Explorer** para inspecionar o banco de dados como você fez no primeiro tutorial e executar o aplicativo para verificar se tudo ainda funciona da mesma forma como antes.

## <a name="deploy-to-azure"></a>Implantar no Azure

Até o aplicativo está sendo executado localmente no IIS Express no computador de desenvolvimento. Para torná-lo disponível para outros usuários na Internet, você precisa implantá-lo em um provedor de hospedagem na web. Nesta seção do tutorial vai implantá-lo no Azure. Esta seção é opcional. Você pode ignorar e continuar com o tutorial a seguir, ou você pode adaptar as instruções nesta seção para outro provedor de hospedagem de sua escolha.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Usando migrações do Code First para implantar o banco de dados

Para implantar o banco de dados, você usará migrações do Code First. Quando você cria o perfil de publicação que você usa para definir as configurações de implantação do Visual Studio, você selecionará uma caixa de seleção **Atualizar banco de dados**. Essa configuração faz com que o processo de implantação configurar automaticamente o aplicativo *Web. config* arquivos no servidor de destino, de forma que usa o Code First a `MigrateDatabaseToLatestVersion` classe inicializador.

O Visual Studio não faz nada com o banco de dados durante o processo de implantação quando ele é copiar seu projeto para o servidor de destino. Quando você executa o aplicativo implantado e acessa o banco de dados pela primeira vez após a implantação, o código primeiro verifica de se o banco de dados coincide com o modelo de dados. Se houver uma incompatibilidade, Code First cria automaticamente o banco de dados (se ela ainda não existir) ou atualiza o esquema de banco de dados para a versão mais recente (se um banco de dados existe, mas não coincide com o modelo). Se o aplicativo implementa uma migrações `Seed` método, o método é executado depois que o banco de dados é criado ou o esquema é atualizado.

Suas migrações `Seed` método insere dados de teste. Se você foram implantando em um ambiente de produção, você precisa alterar o `Seed` método para que ele somente insere dados que você deseja ser inseridos em seu banco de dados de produção. Por exemplo, em seu modelo de dados atual convém ter cursos reais, mas os alunos fictícios no banco de dados de desenvolvimento. Você pode escrever um `Seed` método para carregar no desenvolvimento e comentar os alunos fictícios antes de implantar na produção. Ou você pode escrever um `Seed` método carregar somente os cursos e insira os alunos fictícios no banco de dados de teste manualmente usando a interface do usuário do aplicativo.

### <a name="get-an-azure-account"></a>Obter uma conta do Azure

Você precisará de uma conta do Azure. Se você ainda não tiver uma, mas você tem uma assinatura do Visual Studio, você pode [ativar os benefícios de assinatura](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Caso contrário, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Criar um site da web e um banco de dados SQL no Azure

O aplicativo web no Azure será executado em um ambiente de hospedagem compartilhado, o que significa que ele é executado em máquinas virtuais (VMs) que são compartilhadas com outros clientes do Azure. Um ambiente de hospedagem compartilhado é uma maneira de baixo custo para começar na nuvem. Posteriormente, se aumenta o tráfego da web, o aplicativo pode ser dimensionado para satisfazer a necessidade, executando em VMs dedicadas. Para saber mais sobre as opções de preços para o serviço de aplicativo do Azure, leia a documentação no [documentos do Azure](https://azure.microsoft.com/pricing/details/app-service/)

Você implantará o banco de dados para o banco de dados do SQL Azure. Banco de dados SQL é um serviço de banco de dados relacional baseado em nuvem que se baseia em tecnologias do SQL Server. Ferramentas e aplicativos que funcionam com o SQL Server também funcionam com o banco de dados SQL.

1. No [Portal de gerenciamento](https://portal.azure.com), clique em **novo** na guia à esquerda, clique em **ver todos os** na nova folha e clique **aplicativo Web & SQL** no **Web** seção e, finalmente, **criar**.

    ![Novo botão no Portal de gerenciamento](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   O **novo aplicativo Web & SQL - criar** assistente é aberto.

2. Na folha, insira uma cadeia de caracteres de **nome do aplicativo** caixa para usar como a URL exclusiva para seu aplicativo. A URL completa consistirá em que você digitar aqui e o domínio padrão dos serviços de aplicativo do Azure (. azurewebsites.net). Se o **nome do aplicativo** já está em uso, o Assistente para notificá-lo com um vermelho *o nome do aplicativo não está disponível* mensagem. Se o **nome do aplicativo** estiver disponível, você terá uma marca de seleção verde.

    ![Criar com link do banco de dados no Portal de gerenciamento](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. No **assinatura** lista suspensa, escolha a assinatura do Azure no qual você deseja o **do serviço de aplicativo** residirá.

4. No **grupo de recursos** caixa de texto, escolha um grupo de recursos ou crie um novo. Essa configuração especifica que seu site da web será executado do data center. Para obter mais informações sobre grupos de recursos, leia a documentação em [documentos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Criar um novo **plano do serviço de aplicativo** clicando o *seção do serviço de aplicativo*, **criar novo**e preencha **plano de serviço de aplicativo** (pode ser mesmo nome Serviço de aplicativo), **local**, e **preço** (há uma opção livre).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Clique o **banco de dados SQL**e escolha *criar novo* ou selecione um banco de dados existente

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. No **nome** , digite um nome para seu banco de dados.
8. Clique o **servidor de destino** selecione **criar um novo servidor**. Como alternativa, se você criou um servidor, você pode selecionar o servidor da lista de servidores disponíveis.
9. Escolha **preço** , escolha *livre*. Se forem necessários recursos adicionais, o banco de dados pode ser expandido a qualquer momento. Para saber mais sobre os preços do SQL Azure, leia a documentação em [documentos do Azure](https://azure.microsoft.com/pricing/details/sql-database/).
10. Modificar [agrupamento](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) conforme necessário.
11. Insira um administrador **nome de usuário de administrador de SQL** e **senha do administrador do SQL**. Se você selecionou **servidor novo banco de dados do SQL**, você não digitar um nome existente e a senha aqui, você está inserindo um novo nome e uma senha que você está definindo agora para usar mais tarde, quando você acessa o banco de dados. Se você tiver selecionado um servidor que você criou anteriormente, você vai inserir credenciais para o servidor.
12. Coleção de telemetria pode ser habilitada para o serviço de aplicativo usando o Application Insights. Application Insights com pouca configuração coleta eventos importantes, exceções, dependência, solicitação e as informações de rastreamento. Para saber mais sobre o Application Insights, Introdução ao [documentos do Azure](https://azure.microsoft.com/services/application-insights/).
13. Clique em **criar** na parte inferior da folha para indicar que você tiver terminado.
  
    O Portal de gerenciamento retorna para a página de painéis e o **notificações** folha na parte superior da página mostra que o site está sendo criado. Após alguns instantes (geralmente menor que um minuto), haverá uma notificação de que a implantação foi bem-sucedida. Na barra de navegação à esquerda, o novo **do serviço de aplicativo** aparece no *serviços de aplicativos* seção e a nova **banco de dados SQL** aparece no *bancos de dados SQL*  seção.

### <a name="deploy-the-application-to-azure"></a>Implantar o aplicativo no Azure

1. No Visual Studio, clique com botão direito no projeto no **Solution Explorer** e selecione **publicar** no menu de contexto.
  
    ![Publicar no menu de contexto do projeto](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. No **perfil** guia do **Publicar Web** assistente, clique em **serviço de aplicativo do Microsoft Azure**.
  
    ![Importar configurações de publicação](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Se você não adicionou anteriormente sua assinatura do Azure no Visual Studio, execute as etapas na tela. Essas etapas habilitam o Visual Studio para se conectar à sua assinatura do Azure assim que a lista de **serviços de aplicativos** incluirá o seu site da web.
 
4. Selecione o **assinatura** você adicionou o aplicativo de serviço para, em seguida, o **plano do serviço de aplicativo** pasta seu serviço de aplicativo é uma parte, e, finalmente, o **do serviço de aplicativo** próprio seguido por **Okey**.

    ![Selecione o serviço de aplicativo](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Depois que o perfil foi configurado, o **Conexão** guia será mostrada. Clique em **Conexão validar** para certificar-se de que as configurações estão corretas

    ![Validar a conexão](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Quando a conexão foi validada, uma marca de seleção verde é exibida ao lado de **Conexão validar** botão. Clique em **Avançar**.
  
    ![Conexão validada com êxito](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Abra o **cadeia de caracteres de conexão remota** lista suspensa em **SchoolContext** e selecione a cadeia de caracteres de conexão para o banco de dados que você criou.
8. Selecione **Atualizar banco de dados**.

    ![Guia Configurações](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Essa configuração faz com que o processo de implantação configurar automaticamente o aplicativo *Web. config* arquivos no servidor de destino, de forma que usa o Code First a `MigrateDatabaseToLatestVersion` classe inicializador.
9. Clique em **Avançar**.
10. No **visualização** , clique em **visualização iniciar**.
  
    ![Botão de StartPreview na guia de visualização](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    A guia exibe uma lista dos arquivos que serão copiados para o servidor. Exibir a visualização não é necessário para publicar o aplicativo, mas é uma função útil estar atento. Nesse caso, você não precisa fazer nada com a lista de arquivos que é exibida. Na próxima vez que você implantar esse aplicativo, somente os arquivos que foram alterados será nesta lista.
    ![Saída de arquivo StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Clique em **Publicar**.
    O Visual Studio inicia o processo de copiar os arquivos para o servidor do Azure.
12. O **saída** janela mostra quais ações de implantação foram realizadas e relata a conclusão com êxito da implantação.
  
    ![Relatório de implantação bem-sucedida de janela de saída](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Após a implantação bem-sucedida, o navegador padrão é aberto automaticamente para a URL do site da web implantados.
    O aplicativo que você criou agora está em execução na nuvem. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

Neste ponto o *SchoolContext* banco de dados foi criado no banco de dados do SQL Azure, porque você selecionou **executar migrações do Code First (executado na inicialização do aplicativo)**. O *Web. config* arquivo no site da web implantados foi alterado para que o [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador é executado na primeira vez em que o código lê ou grava dados no banco de dados (o que aconteceu Se você selecionou o **alunos** guia):

![](https://asp.net/media/4367421/mig.png)

O processo de implantação também criou uma nova cadeia de caracteres de conexão *(SchoolContext\_DatabasePublish*) para migrações do Code First a ser usado para atualizar o esquema de banco de dados e a propagação do banco de dados.

![Cadeia de caracteres de conexão Database_Publish](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Você pode encontrar a versão implantada do arquivo Web. config em seu próprio computador *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Você pode acessar o implantado *Web. config* próprio arquivo, usando o FTP. Para obter instruções, consulte [implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Siga as instruções que começam com "para usar uma ferramenta FTP, é necessário que três coisas: a URL de FTP, o nome de usuário e a senha."

> [!NOTE]
> O aplicativo web não implementa a segurança, para que qualquer pessoa que localiza a URL pode alterar os dados. Para obter instruções sobre como proteger o site da web, consulte [implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e o banco de dados do SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Você pode impedir que outras pessoas usando o site usando o Portal de gerenciamento ou **Server Explorer** no Visual Studio para parar o site.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Cenários de migrações avançadas

Se você implantar um banco de dados executando migrações automaticamente, conforme mostrado neste tutorial, e você estiver implantando em um site da web que é executado em vários servidores, você pode obter vários servidores tentar executar migrações ao mesmo tempo. As migrações são atômicas, portanto, se dois servidores tentarem executar a migração do mesmo, um terá êxito e o outro falhará (supondo que as operações não podem ser feitas de duas vezes). Nesse cenário se você quiser evitar esses problemas, você pode chamar migrações manualmente e definir seu próprio código para que ele só acontece uma vez. Para obter mais informações, consulte [em execução e migrações de script do código](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) no blog do Rowan Miller e [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (para executar migrações de linha de comando) no MSDN.

Para obter informações sobre outros cenários de migração, consulte [migrações Screencast série](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Inicializadores de primeiro do código

Na seção de implantação, você viu o [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador que está sendo usado. Código primeiro também fornece outros inicializadores, incluindo [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (o padrão), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (que você usou anteriormente) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). O `DropCreateAlways` inicializador pode ser útil para configurar condições para testes de unidade. Você também pode escrever seus próprios inicializadores e você pode chamar um inicializador explicitamente se não desejar aguardar até que o aplicativo lê de ou grava no banco de dados. No momento em que este tutorial está sendo gravado em novembro de 2013, você pode usar somente os inicializadores de criar e DropCreate antes de habilitar migrações. A equipe do Entity Framework está trabalhando para tornar esses inicializadores utilizável com migrações também.

Para obter mais informações sobre inicializadores, consulte [inicializadores de banco de dados de Conhecimento no Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) e o capítulo 6 do catálogo de [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) por Julie Lerman e Rowan Miller.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como habilitar migrações e implantar o aplicativo. O seguinte tutorial, você começará a examinar os tópicos mais avançados, expandindo o modelo de dados.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Anterior](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [Próximo](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
