---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, você fazer uma alteração de banco de dados e alterações de código relacionadas, teste as alterações no Visual Studio, em seguida, implante a atualização para os ambientes de teste, preparação e produção.

Primeiro, o tutorial mostra como atualizar um banco de dados que é gerenciado pelo migrações do Code First e, em seguida, mais tarde mostra como atualizar um banco de dados usando o provedor dbDacFx.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Implantar uma atualização de banco de dados usando migrações do Code First

Nesta seção, você adiciona uma coluna de data de nascimento para o `Person` a classe base para o `Student` e `Instructor` entidades. Em seguida, você pode atualizar a página que exibe dados de instrutor para que ele exibe a nova coluna. Por fim, você deve implantar as alterações para teste, preparação e produção.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Adicionar uma coluna a uma tabela no banco de dados de aplicativo

1. No *ContosoUniversity.DAL* projeto, abra *Person.cs* e adicione a propriedade a seguir no final o `Person` classe (deve haver duas chaves após ele):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Em seguida, atualize o `Seed` método para que ele fornece um valor para a nova coluna. Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compile a solução e, em seguida, abra o **Package Manager Console** janela. Certifique-se de que ContosoUniversity.DAL ainda está selecionado como o **projeto padrão**.
3. No **Package Manager Console** janela, selecione **ContosoUniversity.DAL** como o **projeto padrão**e, em seguida, digite o seguinte comando:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` classe e no `Up` método, você pode ver o código que cria a nova coluna. O `Up` método cria a coluna quando você estiver implementando a alteração e o `Down` método exclui a coluna quando você está revertendo a alteração.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compile a solução e, em seguida, digite o seguinte comando no **Package Manager Console** janela (Verifique se o projeto ContosoUniversity.DAL ainda está selecionado):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    O Entity Framework executa o `Up` método e, em seguida, executa o `Seed` método.

### <a name="display-the-new-column-in-the-instructors-page"></a>Exibir a nova coluna na página instrutores

1. No projeto ContosoUniversity, abra *Instructors.aspx* e adicionar um novo campo de modelo para exibir a data de nascimento. Adicione-o entre os para atribuição de contratação data e do office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Se o recuo de código obtém fora de sincronia, você pode pressionar CTRL-K e CTRL-D para reformatar automaticamente o arquivo.)
2. Execute o aplicativo e clique no **instrutores** link.

    Quando a página for carregada, você verá que ele tem o novo campo de data de nascimento.

    ![Página de instrutores com data de nascimento](deploying-a-database-update/_static/image2.png)
3. Feche o navegador.

### <a name="deploy-the-database-update"></a>Implantar a atualização de banco de dados

1. Em **Solution Explorer** selecione o projeto ContosoUniversity.
2. No **Web um clique em publicar** barra de ferramentas, clique no **teste** perfil de publicação e, em seguida, clique em **Publicar Web**. (Se a barra de ferramentas estiver desabilitada, selecione o projeto de ContosoUniversity na **Solution Explorer**.)

    O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.
3. Execute o **instrutores** página para verificar se a atualização foi implantada com êxito.

    Quando o aplicativo tentar acessar o banco de dados para essa página, Code First para atualizar o esquema de banco de dados e executa o `Seed` método. Quando a página é exibida, você verá o esperado **data de nascimento** coluna com datas nele.
4. No **Web um clique em publicar** barra de ferramentas, clique no **preparo** perfil de publicação e, em seguida, clique em **Publicar Web**.
5. Execute o **instrutores** página no preparo para verificar se a atualização foi implantada com êxito.
6. No **Web um clique em publicar** barra de ferramentas, clique no **produção** perfil de publicação e, em seguida, clique em **Publicar Web**.
7. Execute o **instrutores** página em produção para verificar se a atualização foi implantada com êxito.

    Para uma atualização do aplicativo real de produção que inclui uma banco de dados alteração levaria normalmente também o aplicativo offline durante a implantação usando *aplicativo\_offline.htm*, como você viu no tutorial anterior.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Implantar uma atualização de banco de dados usando o provedor dbDacFx

Nesta seção, você adiciona um *comentários* coluna para o *usuário* de tabela no banco de dados de associação e criar uma página que lhe permite exibir e editar comentários para cada usuário. Em seguida, implante as alterações para teste, preparação e produção.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Adicionar uma coluna a uma tabela no banco de dados de associação

1. No Visual Studio, abra **Pesquisador de objetos do SQL Server**.
2. Expanda **(localdb) \v11.0**, expanda **bancos de dados**, expanda **aspnet ContosoUniversity** (não **aspnet-ContosoUniversity-Prod**) e, em seguida, expanda **tabelas**.

    Se você não vir **\v11.0 (localdb)** sob o **do SQL Server** nó, com o botão direito do **do SQL Server** nó e clique em **adicionar SQL Server**. No **conectar ao servidor** caixa de diálogo Inserir *(localdb) \v11.0* como o **nome do servidor**e, em seguida, clique em **conectar**.

    Se você não vir **aspnet ContosoUniversity**, execute o projeto e faça logon usando o *admin* credenciais (a senha é *devpwd*) e, em seguida, atualize o  **Pesquisador de objetos do SQL Server** janela.
3. Clique com botão direito do **usuários** de tabela e, em seguida, clique em **Designer de exibição**.

    ![Designer de exibição SSOX](deploying-a-database-update/_static/image3.png)
4. No designer, adicione um *comentários* coluna e torná-lo *nvarchar (128)* e anulável e, em seguida, clique em **atualização**.

    ![Adicionando a coluna Comments](deploying-a-database-update/_static/image4.png)
5. No **atualizações de banco de dados de visualização** , clique em **Atualizar banco de dados**.

    ![Visualizar atualizações de banco de dados](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Criar uma página para exibir e editar a nova coluna

1. Em **Solution Explorer**, com o botão direito do **conta** pasta no projeto ContosoUniversity, clique em **adicionar**e, em seguida, clique em **Novo Item** .
2. Criar um novo **página mestra usando dos Web formulário** e nomeie-o *UserInfo.aspx*. Aceite o padrão *Site.Master* arquivo como a página mestra.
3. Copie a seguinte marcação para o `MainContent` `Content` elemento (a última do 3 `Content` elementos):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Clique com botão direito do *UserInfo.aspx* página e clique em **exibir no navegador**.
5. Faça logon com sua *admin* as credenciais do usuário (a senha é *devpwd*) e adicionar alguns comentários a um usuário para verificar se a página funciona corretamente.

    ![Página UserInfo](deploying-a-database-update/_static/image6.png)
6. Feche o navegador.

## <a name="deploy-the-database-update"></a>Implantar a atualização de banco de dados

Para implantar usando o provedor dbDacFx, basta selecionar o **Atualizar banco de dados** opção no perfil de publicação. No entanto, para a implantação inicial quando você usar essa opção é também configurado para executar alguns scripts SQL adicionais: esses são ainda no perfil e você precisa impedir a execução novamente.

1. Abra o **Publicar Web** assistente clicando duas vezes no projeto ContosoUniversity e clicando em **publicar**.
2. Selecione o **teste** perfil.
3. Clique o **configurações** guia.
4. Em **DefaultConnection**, selecione **Atualizar banco de dados**.
5. Desabilite os scripts adicionais que você configurou para executar a implantação inicial:

    1. Clique em **configurar atualizações de banco de dados**.
    2. No **configurar atualizações de banco de dados** caixa de diálogo, desmarque as caixas de seleção ao lado de *Grant.sql* e *aspnet de dados de dev.sql*.
    3. Clique em **Fechar**.
6. Clique o **visualização** guia.
7. Em **bancos de dados** e à direita do **DefaultConnection**, clique no **banco de dados de visualização** link.

    ![Visualização de banco de dados](deploying-a-database-update/_static/image7.png)

    A janela de visualização mostra o script será executado no banco de dados de destino para tornar esse esquema de banco de dados corresponde ao esquema do banco de dados de origem. O script inclui um comando ALTER TABLE que adiciona a nova coluna.
8. Fechar o **visualização de banco de dados** caixa de diálogo e clique **publicar**.

    O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.
9. Execute a página UserInfo (adicionar *Account/UserInfo.aspx* para a URL da home page) para verificar se a atualização foi implantada com êxito. Você precisará fazer logon digitando *admin* e *devpwd*.

    Dados em tabelas não são implantados por padrão, e você não configurou um script de implantação de dados em execução, para que você não encontrará o comentário que você adicionou no desenvolvimento. Você pode adicionar um novo comentário agora no preparo para verificar se a alteração foi implantada no banco de dados e a página está funcionando corretamente.
10. Siga o mesmo procedimento para implantar em preparação e produção.

    Não se esqueça de desativar os scripts adicionais. A única diferença em comparação comparada o perfil de teste é que você desativará o somente um script na migração de dados e perfis de produção porque eles foram configurados para executar apenas *aspnet de produção de data.sql*.

    As credenciais para preparação e produção são prodpwd e o administrador.

    Para uma atualização do aplicativo real de produção que inclui uma banco de dados alteração levaria normalmente também o aplicativo offline durante a implantação, carregando *aplicativo\_offline.htm* antes de publicar e excluí-lo Depois disso, como você viu na [tutorial anterior](deploying-a-code-update.md).

## <a name="summary"></a>Resumo

Agora que você implantou uma atualização de aplicativo que incluía uma alteração de banco de dados usando o provedor dbDacFx e migrações do Code First.

![Página de instrutores com data de nascimento](deploying-a-database-update/_static/image8.png)

![Página UserInfo](deploying-a-database-update/_static/image9.png)

O seguinte tutorial mostra como executar as implantações por meio da linha de comando.

>[!div class="step-by-step"]
[Anterior](deploying-a-code-update.md)
[Próximo](command-line-deployment.md)
