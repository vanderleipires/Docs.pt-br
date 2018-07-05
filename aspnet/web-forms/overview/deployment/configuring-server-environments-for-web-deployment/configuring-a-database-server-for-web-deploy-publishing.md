---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configurando um servidor de banco de dados para a Web de publicação de implantação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como configurar um servidor de banco de dados do SQL Server 2008 R2 para dar suporte à publicação e implantação da web. As tarefas descritas neste tópico são co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 5563b737e3fd200070951f8d281ed8015137ae35
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384453"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configurando um servidor de banco de dados para publicação de implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar um servidor de banco de dados do SQL Server 2008 R2 para dar suporte à publicação e implantação da web.
> 
> As tarefas descritas neste tópico são comuns a todos os cenários de implantação&#x2014;não importa se os servidores web são configurados para usar o serviço de agente remoto de ferramenta de implantação da Web de IIS (implantação da Web), o manipulador de implantação da Web ou implantação offline ou seu aplicativo está em execução em um único servidor web ou um farm de servidores. Da maneira que implanta o banco de dados pode ser alterado de acordo com os requisitos de segurança e outras considerações. Por exemplo, você pode implantar o banco de dados com ou sem dados de exemplo e você pode implantar os mapeamentos de função de usuário ou configurá-los manualmente após a implantação. No entanto, a maneira como você configura o servidor de banco de dados permanece o mesmo.


Você não precisa instalar quaisquer produtos adicionais ou ferramentas para configurar um servidor de banco de dados para dar suporte à implantação da web. Supondo que o servidor de banco de dados e seu servidor web executados em máquinas diferentes, você simplesmente precisa:

- Permitir que o SQL Server se comunique usando TCP/IP.
- Permitir o tráfego do SQL Server por meio de firewalls.
- Dê a conta de computador do servidor web em um logon do SQL Server.
- Mapear o logon da conta de máquina para todas as funções de banco de dados necessários.
- Conceda à conta que executará a implantação de um permissões de criador de logon e um banco de dados do SQL Server.
- Para dar suporte a implantações repetidas, mapear o logon de conta de implantação para o **db\_proprietário** função de banco de dados.

Este tópico mostra como executar cada um desses procedimentos. As tarefas e instruções passo a passo neste tópico pressupõem que você está começando com uma instância padrão do SQL Server 2008 R2 em execução no Windows Server 2008 R2. Antes de continuar, verifique se:

- Windows Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis serão instaladas.
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.
- SQL Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis serão instaladas.

Instância do SQL Server só precisa incluir o **serviços de mecanismo de banco de dados** função, que é incluída automaticamente em qualquer instalação do SQL Server. No entanto, para facilitar a configuração e manutenção, recomendamos que você inclua o **ferramentas de gerenciamento – básicas** e **ferramentas de gerenciamento – completas** funções de servidor.

> [!NOTE]
> Para obter mais informações sobre como adicionar computadores a um domínio, consulte [ingressando computadores no domínio e fazendo logon](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Para obter mais informações sobre como instalar o SQL Server, consulte [instalando o SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Habilitar o acesso remoto para o SQL Server

SQL Server usa TCP/IP para se comunicar com computadores remotos. Se seu servidor de banco de dados e o servidor web estiverem em computadores diferentes, você precisa:

- Defina as configurações de rede do SQL Server para permitir a comunicação via TCP/IP.
- Configure os firewalls de hardware ou software para permitir o tráfego TCP (e em alguns casos de protocolo UDP (User Datagram) o tráfego) nas portas que usa a instância do SQL Server.

Para habilitar o SQL Server se comunique via TCP/IP, use o SQL Server Configuration Manager para alterar a configuração de rede para sua instância do SQL Server.

**Para habilitar o SQL Server para se comunicar usando TCP/IP**

1. Sobre o **começar** , aponte para **todos os programas**, clique em **Microsoft SQL Server 2008 R2**, clique em **ferramentas de configuração**e, em seguida, clique em **SQL Server Configuration Manager**.
2. No painel de exibição de árvore, expanda **configuração de rede do SQL Server**e, em seguida, clique em **protocolos para MSSQLSERVER**.

   > [!NOTE]
   > Se você tiver instalado várias instâncias do SQL Server, você verá uma <strong>protocolos para</strong><em>[nome da instância]</em> item para cada instância. Você precisa configurar as configurações de rede em uma base por instância.
3. No painel de detalhes, clique com botão direito do **TCP/IP** de linha e, em seguida, clique em **habilitar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. No **aviso** caixa de diálogo, clique em **Okey**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Você precisa reiniciar o serviço MSSQLSERVER para a nova configuração de rede entrem em vigor. Você pode fazer isso em um prompt de comando, no console Serviços, ou do SQL Server Management Studio. Neste procedimento, você usará o SQL Server Management Studio.
6. Feche o SQL Server Configuration Manager.
7. Sobre o **inicie** , aponte para **todos os programas**, clique em **Microsoft SQL Server 2008 R2**e, em seguida, clique em **SQL Server Management Studio**.
8. No **conectar ao servidor** na caixa de **nome do servidor** caixa, digite o nome do servidor de banco de dados e, em seguida, clique em **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. No **Pesquisador de objetos** painel, clique com botão direito no nó do servidor pai (por exemplo, **TESTDB1**) e, em seguida, clique em **reiniciar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. No **Microsoft SQL Server Management Studio** caixa de diálogo, clique em **Sim**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Quando o serviço for reiniciado, feche o SQL Server Management Studio.

Para permitir o tráfego do SQL Server por meio de um firewall, primeiro você precisa saber quais portas na instância do SQL Server está usando. Isso dependerá como a instância do SQL Server foi criada e configurada:

- Um *instância padrão* do SQL Server escuta (e responde a) solicitações na porta TCP 1433.
- Um *instância nomeada* do SQL Server escuta (e responde a) solicitações em uma porta TCP dinamicamente atribuída.
- Se o serviço navegador do SQL Server está habilitado, os clientes podem consultar o serviço na porta UDP 1434 para descobrir qual porta TCP a ser usado para uma determinada instância do SQL Server. No entanto, esse serviço geralmente é desabilitado por motivos de segurança.

Supondo que você está usando uma instância padrão do SQL Server, você precisa configurar o firewall para permitir o tráfego.

| Direção | De porta | A porta | Tipo de porta |
| --- | --- | --- | --- |
| De entrada | Qualquer | 1433 | TCP |
| Saída | 1433 | Qualquer | TCP |
  

> [!NOTE]
> Tecnicamente, um computador cliente usará uma porta atribuída aleatoriamente do TCP entre 1024 e 5000 para se comunicar com o SQL Server, e você pode restringir suas regras de firewall adequadamente. Para obter mais informações sobre firewalls e portas do SQL Server, consulte [números de porta de TCP/IP necessários para se comunicar ao SQL por um firewall](https://go.microsoft.com/?linkid=9805125) e [como: configurar um servidor para escutar em uma porta de TCP específica (configuração do SQL Server Gerenciador de)](https://msdn.microsoft.com/library/ms177440.aspx).


Na maioria dos ambientes do Windows Server, provavelmente você precisará configurar o Firewall do Windows no servidor de banco de dados. Por padrão, o Firewall do Windows permite todo o tráfego de saída, a menos que uma regra proíbe especificamente. Para habilitar o servidor web para acessar seu banco de dados, você precisará configurar uma regra de entrada que permita o tráfego TCP no número da porta usada pela instância do SQL Server. Se você estiver usando uma instância padrão do SQL Server, você pode usar o procedimento a seguir para configurar essa regra.

**Para configurar o Firewall do Windows para permitir a comunicação com uma instância do SQL Server padrão**

1. No servidor de banco de dados, sobre o **inicie** , aponte para **ferramentas administrativas**e, em seguida, clique em **Firewall do Windows com segurança avançada**.
2. No painel de exibição de árvore, clique em **regras de entrada**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. No **ações** painel, em **regras de entrada**, clique em **nova regra**.
4. No novo Assistente de regra de entrada, sobre o **tipo de regra** página, selecione **porta**e, em seguida, clique em **próxima**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. No **protocolo e portas** página, certifique-se de que **TCP** está selecionado e, na **portas locais específicas** , digite **1433**e, em seguida, clique em **Próxima**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Sobre o **ação** página, deixe **permitir a conexão** selecionado e clique em **próxima**.
7. No **perfil** página, deixe **domínio** selecionada, desmarque as **privada** e **público** caixas de seleção e, em seguida, clique em  **Próxima**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Sobre o **nome** página, dê um nome descritivo adequadamente para a regra (por exemplo, **instância padrão do SQL Server – acesso à rede**) e, em seguida, clique em **concluir**.

Para obter mais informações sobre como configurar o Firewall do Windows para o SQL Server, especialmente se você precisar se comunicar com o SQL Server nas portas não padrão ou dinâmicas, consulte [como: configurar um Firewall do Windows para acesso ao mecanismo de banco de dados](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurar logons e permissões de banco de dados

Quando você implanta um aplicativo web para serviços de informações da Internet (IIS), o aplicativo é executado usando a identidade do pool de aplicativos. Em um ambiente de domínio, identidades do pool de aplicativos usam a conta do computador do servidor no qual executa para acessar os recursos de rede. Contas de computador assumem a forma <em>[nome do domínio]</em><strong>\</ strong ><em>[nome do computador]</em><strong>$</strong>&#x2014;, por exemplo, <strong>FABRIKAM\TESTWEB1$</strong>. Para permitir que seu aplicativo web para acessar um banco de dados pela rede, você precisa:

- Adicione um logon para a conta de computador do servidor web para a instância do SQL Server.
- Mapear o logon da conta de máquina para todas as funções de banco de dados necessário (normalmente **db\_datareader** e **db\_datawriter**).

Se seu aplicativo web estiver em execução em um farm de servidores, em vez de um único servidor, você precisará repetir os procedimentos para todos os servidores web no farm de servidores.

> [!NOTE]
> Para obter mais informações sobre as identidades de pool de aplicativos e acessando recursos de rede, consulte [identidades do Pool de aplicativos](https://go.microsoft.com/?linkid=9805123).


Você pode abordar essas tarefas de várias maneiras. Para criar o logon, você pode:

- Crie o logon manualmente no servidor de banco de dados, usando o Transact-SQL ou SQL Server Management Studio.
- Use um projeto de servidor do SQL Server 2008 no Visual Studio para criar e implantar o logon.

Um logon do SQL Server é um objeto de nível de servidor, em vez de um objeto de nível de banco de dados, portanto, não é dependente no banco de dados que você deseja implantar. Como tal, você pode criar o logon a qualquer momento, e a abordagem mais fácil é muitas vezes criar o logon manualmente no servidor de banco de dados antes de iniciar a implantação de bancos de dados. Você pode usar o procedimento a seguir para criar um logon no SQL Server Management Studio.

**Para criar um logon do SQL Server para a conta de computador do servidor web**

1. No servidor de banco de dados, sobre o **inicie** , aponte para **todos os programas**, clique em **Microsoft SQL Server 2008 R2**e, em seguida, clique em **SQL Server Management Studio** .
2. No **conectar ao servidor** na caixa de **nome do servidor** caixa, digite o nome do servidor de banco de dados e, em seguida, clique em **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. No **Pesquisador de objetos** painel, clique com botão direito **segurança**, aponte para **New**e, em seguida, clique em **logon**.
4. No **logon – novo** na caixa de **nome de logon** , digite o nome da sua conta de computador do servidor web (por exemplo, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Clique em **OK**.

Neste ponto, seu servidor de banco de dados está pronto para publicação de implantação da Web. No entanto, soluções que você implanta não funcionarão até que você mapear o logon da conta de computador para as funções de banco de dados necessários. Mapeamento do logon para funções de banco de dados requer muito mais considerado, como você não podem funções de mapa até depois que você implantou o banco de dados. Para mapear o logon da conta de computador para as funções de banco de dados necessários, você pode:

- Atribua as funções de banco de dados para o logon manualmente, depois que você implantou o banco de dados pela primeira vez.
- Use um script de pós-implantação para atribuir as funções de banco de dados para o logon.

Para obter mais informações sobre como automatizar a criação de logons e mapeamentos de função de banco de dados, consulte [Implantando associações de função de banco de dados em ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Como alternativa, você pode usar o procedimento a seguir para mapear o logon da conta de computador para as funções de banco de dados necessário manualmente. Lembre-se de que você não pode executar esse procedimento até *depois de* que você implantou o banco de dados.

**Para mapear as funções de banco de dados para o logon de conta de computador de servidor web**

1. Abra o SQL Server Management Studio como antes.
2. No **Pesquisador de objetos** painel, expanda o **segurança** nó, expanda o **logons** nó e, em seguida, clique duas vezes o logon da conta de computador (por exemplo,  **$ FABRIKAM\TESTWEB1**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. No **propriedades de logon** caixa de diálogo, clique em **mapeamento de usuário**.
4. No **usuários mapeados para este logon** da tabela, selecione o nome do seu banco de dados (por exemplo, **ContactManager**).
5. No **associação de função para o banco de dados:** *[nome do banco de dados]* , selecione as permissões necessárias. No caso da solução de exemplo do Contact Manager, você deve selecionar o **db\_datareader** e **db\_datawriter** funções.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Clique em **OK**.

Enquanto o mapeamento manual de funções de banco de dados geralmente é mais do que adequado para ambientes de teste, é menos recomendado para implantações automatizadas ou com um clique para ambientes de preparo ou produção. Você pode encontrar mais informações sobre como automatizar esse tipo de tarefa usando scripts de pós-implantação em [Implantando associações de função de banco de dados em ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Para obter mais informações sobre projetos de banco de dados e servidor, consulte [projetos de banco de dados do Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Configurar permissões para a conta de implantação

Se a conta que você usará para executar a implantação não for um administrador do SQL Server, também precisará criar um logon para essa conta. Para criar o banco de dados, a conta deve ser um membro do **dbcreator** função de servidor ou ter permissões equivalentes.

> [!NOTE]
> Quando você usa a implantação da Web ou VSDBCMD para implantar um banco de dados, você pode usar as credenciais do Windows ou credenciais do SQL Server (se a instância do SQL Server está configurada para dar suporte à autenticação de modo misto). O procedimento a seguir pressupõe que você deseja usar as credenciais do Windows, mas não há nada que impeça você especificar um nome de usuário do SQL Server e uma senha em sua cadeia de conexão quando você configura a implantação.


**Para configurar as permissões para a conta de implantação**

1. Abra o SQL Server Management Studio como antes.
2. No **Pesquisador de objetos** painel, clique com botão direito **segurança**, aponte para **New**e, em seguida, clique em **logon**.
3. No **logon – novo** na caixa de **nome de logon** , digite o nome da sua conta de implantação (por exemplo, **FABRIKAM\matt**).
4. No **selecionar uma página** painel, clique em **funções de servidor**.
5. Selecione **dbcreator**e, em seguida, clique em **Okey**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Para dar suporte a implantações subsequentes, você também precisará adicionar a conta de implantação para o **db\_proprietário** no banco de dados após a primeira implantação. Isso ocorre porque, em implantações subsequentes você está modificando o esquema do banco de dados existente, em vez de criar um novo banco de dados. Conforme descrito na seção anterior, você não pode adicionar um usuário a uma função de banco de dados até que você criou o banco de dados, por motivos óbvios.

**Para mapear o logon da conta de implantação para o banco de dados\_função de proprietário de banco de dados**

1. Abra o SQL Server Management Studio como antes.
2. No **Pesquisador de objetos** janela, expanda o **segurança** nó, expanda o **logons** nó e, em seguida, clique duas vezes o logon da conta de computador (por exemplo,  **FABRIKAM\matt**).
3. No **propriedades de logon** caixa de diálogo, clique em **mapeamento de usuário**.
4. No **usuários mapeados para este logon** da tabela, selecione o nome do seu banco de dados (por exemplo, **ContactManager**).
5. No **associação de função para o banco de dados:** *[nome do banco de dados]* lista, selecione o **db\_proprietário** função.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Clique em **OK**.

## <a name="conclusion"></a>Conclusão

Seu servidor de banco de dados deve agora estar pronto para aceitar as implantações de banco de dados remoto e para permitir que os servidores de web IIS remotos acessar seus bancos de dados. Antes de tentar implantar e usar bancos de dados, você talvez queira verificar estes pontos-chave:

- Você configurou o SQL Server para aceitar conexões remotas do TCP/IP?
- Você configurou os firewalls para permitir o tráfego do SQL Server?
- Você criou um logon de conta de computador para todos os servidores web que acessarão o SQL Server?
- A implantação de banco de dados inclui um script para criar mapeamentos de função de usuário, ou você precisa criá-los manualmente depois de implantar o banco de dados pela primeira vez?
- Você criou um logon para a conta de implantação e adicionou-o para o **dbcreator** função de servidor?

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como implantar projetos de banco de dados, consulte [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter orientação sobre como criar associações de função de banco de dados executando um script de pós-implantação, consulte [Implantando associações de função de banco de dados em ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obter orientação sobre como superar os desafios de implantação exclusiva que representem de bancos de dados de associação, consulte [implantando bancos de dados de associação para ambientes empresariais](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Próximo](creating-a-server-farm-with-the-web-farm-framework.md)
