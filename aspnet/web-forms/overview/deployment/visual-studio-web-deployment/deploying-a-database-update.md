---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 37b996452dfa619ba1276a1aba562ed7efc579b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389183"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, você fazer uma alteração de banco de dados e alterações de código relacionados, testar as alterações no Visual Studio e, em seguida, implante a atualização para os ambientes de teste, preparação e produção.

Primeiro, o tutorial mostra como atualizar um banco de dados que é gerenciado pelo migrações do Code First e, em seguida, posterior mostra como atualizar um banco de dados usando o provedor dbDacFx.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Implantar uma atualização de banco de dados por meio de migrações do Code First

Nesta seção, você adiciona uma coluna de data de nascimento para o `Person` a classe base para o `Student` e `Instructor` entidades. Em seguida, você pode atualizar a página que exibe dados de instrutor para que ele exibe a nova coluna. Por fim, você deve implantar as alterações para teste, preparação e produção.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Adicionar uma coluna a uma tabela no banco de dados de aplicativo

1. No *ContosoUniversity.DAL* projeto, abra *Person.cs* e adicione a propriedade a seguir no final o `Person` classe (deve haver duas chaves após ele):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Em seguida, atualize o `Seed` , de modo que ele fornece um valor para a nova coluna. Abra *Migrations\Configuration.cs* e substitua o bloco de código começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compile a solução e, em seguida, abra o **Package Manager Console** janela. Certifique-se de que ContosoUniversity.DAL ainda está selecionado como o **projeto padrão**.
3. No **Package Manager Console** janela, selecione **ContosoUniversity.DAL** como o **projeto padrão**e, em seguida, digite o seguinte comando:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Quando esse comando for concluído, o Visual Studio abre o arquivo de classe que define o novo `DbMIgration` classe e, no `Up` método, você pode ver o código que cria a nova coluna. O `Up` método cria a coluna quando você estiver implementando a alteração e o `Down` método exclui a coluna quando você está revertendo a alteração.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compile a solução e, em seguida, digite o seguinte comando na **Package Manager Console** janela (Verifique se o projeto ContosoUniversity.DAL ainda está selecionado):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    O Entity Framework é executado o `Up` método e, em seguida, executa o `Seed` método.

### <a name="display-the-new-column-in-the-instructors-page"></a>Exibir a nova coluna na página instrutores

1. No projeto ContosoUniversity, abra *Instructors.aspx* e adicione um novo campo de modelo para exibir a data de nascimento. Adicioná-la entre aqueles para a atribuição de contratação data e do office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Se o recuo do código fica fora de sincronizado, você pode pressionar CTRL-K e CTRL-D para reformatar automaticamente o arquivo.)
2. Execute o aplicativo e clique no **instrutores** link.

    Quando a página for carregada, você verá que ele tem o novo campo de data de nascimento.

    ![Página instrutores com data de nascimento](deploying-a-database-update/_static/image2.png)
3. Feche o navegador.

### <a name="deploy-the-database-update"></a>Implantar a atualização de banco de dados

1. Na **Gerenciador de soluções** selecione o projeto ContosoUniversity.
2. No **publicação Web com um clique em** barra de ferramentas, clique no **teste** perfil de publicação e, em seguida, clique em **publicar na Web**. (Se a barra de ferramentas estiver desabilitada, selecione o projeto de ContosoUniversity no **Gerenciador de soluções**.)

    O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.
3. Execute o **instrutores** página para verificar se a atualização foi implantada com êxito.

    Quando o aplicativo tenta acessar o banco de dados para essa página, Code First para atualizar o esquema de banco de dados e executa o `Seed` método. Quando a página for exibida, você verá o esperado **data de nascimento** coluna com datas nele.
4. No **publicação Web com um clique em** barra de ferramentas, clique no **preparo** perfil de publicação e, em seguida, clique em **publicar na Web**.
5. Execute o **instrutores** página no preparo para verificar se a atualização foi implantada com êxito.
6. No **publicação Web com um clique em** barra de ferramentas, clique no **produção** perfil de publicação e, em seguida, clique em **publicar na Web**.
7. Execute o **instrutores** página na produção para verificar se a atualização foi implantada com êxito.

    Para uma atualização de aplicativo de produção real que inclui uma alteração de banco de dados levaria normalmente também o aplicativo offline durante a implantação por meio *app\_offline.htm*, como você viu no tutorial anterior.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Implantar uma atualização de banco de dados usando o provedor dbDacFx

Nesta seção, você adiciona uma *comentários* coluna para o *usuário* de tabela no banco de dados de associação e criar uma página que permite exibir e editar comentários para cada usuário. Em seguida, você deve implantar as alterações para teste, preparação e produção.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Adicionar uma coluna a uma tabela no banco de dados de associação

1. No Visual Studio, abra **Pesquisador de objetos do SQL Server**.
2. Expandir **(localdb) \v11.0**, expanda **bancos de dados**, expanda **aspnet ContosoUniversity** (não **aspnet-ContosoUniversity-Prod**) e, em seguida, expanda **tabelas**.

    Se você não vir **(localdb) \v11.0** sob o **do SQL Server** nó, clique com botão direito a **do SQL Server** nó e clique em **adicionar SQL Server**. No **conectar ao servidor** Inserir caixa de diálogo *(localdb) \v11.0* como o **nome do servidor**e, em seguida, clique em **Connect**.

    Se você não vir **ContosoUniversity aspnet**, execute o projeto e faça logon usando o *admin* credenciais (senha está *devpwd*) e, em seguida, atualize o  **SQL Server Object Explorer** janela.
3. Com o botão direito do **os usuários** tabela e, em seguida, clique em **View Designer**.

    ![Designer de exibição do SSOX](deploying-a-database-update/_static/image3.png)
4. No designer, adicione uma *comentários* coluna e torná-lo *nvarchar (128)* e que permitem valor nulo e, em seguida, clique em **atualização**.

    ![Adicionando a coluna Comments](deploying-a-database-update/_static/image4.png)
5. No **atualizações de banco de dados de visualização** , clique em **Atualizar banco de dados**.

    ![Visualizar atualizações de banco de dados](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Crie uma página para exibir e editar a nova coluna

1. No **Gerenciador de soluções**, com o botão direito do **conta** pasta no projeto ContosoUniversity, clique em **Add**e, em seguida, clique em **Novo Item** .
2. Criar um novo **página mestra usando do Web Form** e nomeie-o *UserInfo.aspx*. Aceite o padrão *Master* arquivo como a página mestra.
3. Copie a seguinte marcação para o `MainContent` `Content` elemento (a última do 3 `Content` elementos):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Clique com botão direito do *UserInfo.aspx* da página e clique em **exibir no navegador**.
5. Faça logon com sua *admin* as credenciais do usuário (a senha é *devpwd*) e adicionar alguns comentários a um usuário para verificar se a página funciona corretamente.

    ![Página de UserInfo](deploying-a-database-update/_static/image6.png)
6. Feche o navegador.

## <a name="deploy-the-database-update"></a>Implantar a atualização de banco de dados

Para implantar usando o provedor dbDacFx, basta selecionar o **Atualizar banco de dados** opção no perfil de publicação. No entanto, para a implantação inicial quando você tiver usado essa opção você também configurou para executar alguns scripts SQL adicionais: aqueles ainda estão em perfil e você terá de impedir a execução novamente.

1. Abra o **Publicar Web** assistente clicando com botão direito no projeto ContosoUniversity e clicando em **publicar**.
2. Selecione o **teste** perfil.
3. Clique o **configurações** guia.
4. Sob **DefaultConnection**, selecione **Atualizar banco de dados**.
5. Desabilite os scripts adicionais que você configurou para executar a implantação inicial:

    1. Clique em **configurar atualizações de banco de dados**.
    2. No **configurar atualizações de banco de dados** caixa de diálogo, desmarque as caixas de seleção ao lado de *Grant.sql* e *aspnet-data-dev.sql*.
    3. Clique em **Fechar**.
6. Clique o **visualização** guia.
7. Sob **bancos de dados** e à direita da **DefaultConnection**, clique no **banco de dados de visualização** link.

    ![Visualização de banco de dados](deploying-a-database-update/_static/image7.png)

    A janela de visualização mostra o script que será executado no banco de dados de destino para tornar esse esquema de banco de dados corresponde ao esquema do banco de dados de origem. O script inclui um comando ALTER TABLE que adiciona a nova coluna.
8. Fechar o **visualização de banco de dados** caixa de diálogo e clique **publicar**.

    O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.
9. Execute a página UserInfo (adicione *Account/UserInfo.aspx* para a URL da home page) para verificar se a atualização foi implantada com êxito. Você terá que fazer logon inserindo *admin* e *devpwd*.

    Dados em tabelas não estiver implantados por padrão, e você não configurou um script de implantação de dados em execução, portanto, você não encontrará o comentário que você adicionou no desenvolvimento. Você pode adicionar um novo comentário agora no preparo para verificar se a alteração foi implantada no banco de dados e a página está funcionando corretamente.
10. Siga o mesmo procedimento para implantar em produção e preparo.

    Não se esqueça de desabilitar os scripts extras. A única diferença em comparação comparada o perfil de teste é que você desabilitará a somente um script na migração de dados e perfis de produção, pois eles foram configurados para executar apenas *aspnet-prod-data.sql*.

    As credenciais para a preparação e produção são admin e prodpwd.

    Para uma atualização de aplicativo de produção real que inclui uma alteração de banco de dados levaria normalmente também o aplicativo offline durante a implantação, carregando *app\_offline.htm* antes da publicação e excluí-lo Depois disso, como você viu no [tutorial anterior](deploying-a-code-update.md).

## <a name="summary"></a>Resumo

Agora, você implantou uma atualização de aplicativo que incluía uma alteração de banco de dados usando migrações do Code First e o provedor dbDacFx.

![Página instrutores com data de nascimento](deploying-a-database-update/_static/image8.png)

![Página de UserInfo](deploying-a-database-update/_static/image9.png)

O próximo tutorial mostra como executar implantações usando a linha de comando.

> [!div class="step-by-step"]
> [Anterior](deploying-a-code-update.md)
> [Próximo](command-line-deployment.md)
