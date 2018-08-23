---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Criando um projeto de equipe no TFS | Microsoft Docs
author: jrjlee
description: Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 9218a22ff221dc7067662c58ccd3e758fca493b7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830850"
---
<a name="creating-a-team-project-in-tfs"></a>Criando um projeto de equipe no TFS
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

## <a name="task-overview"></a>Visão geral da tarefa

Para provisionar e usar um novo projeto de equipe no TFS, você precisa concluir estas etapas de alto nível:

- Conceder permissões ao usuário que irá criar um novo projeto de equipe.
- Crie o projeto de equipe.
- Conceder permissões a membros da equipe que funcionarão no projeto.
- Fazer check-in algum conteúdo.

Este tópico mostra como executar esses procedimentos, e ele identificará os usuários e funções de trabalho que provavelmente serão responsáveis por cada procedimento. Lembre-se de que, dependendo da estrutura da sua organização, cada uma dessas tarefas pode ser a responsabilidade de uma pessoa diferente.

As tarefas e instruções passo a passo neste tópico pressupõem que você instalou e configurou o TFS e que você criou uma coleção de projetos de equipe como parte do processo de configuração. Para obter mais informações sobre essas suposições e para obter mais informações em segundo plano no cenário, consulte [configurar um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Conceder permissões para o criador do projeto de equipe

Para criar um novo projeto de equipe, você precisará dessas permissões:

- Você deve ter o **criar novos projetos** permissão na camada de aplicativo do TFS. Você normalmente concede essa permissão, adicionando usuários para o **administradores da coleção de projeto** grupo do TFS. O **administradores do Team Foundation** grupo global também inclui essa permissão.
- Você deve ter permissão para criar novos sites de equipe dentro da coleção de site do SharePoint que corresponde à coleção de projeto de equipe do TFS. Você normalmente concede essa permissão, adicionando o usuário a um grupo do SharePoint com **controle total** conjunto de sites de direitos no SharePoint.
- Se você estiver usando recursos do SQL Server Reporting Services, você deve ser um membro do **Team Foundation Content Manager** função no Reporting Services.

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Normalmente, a pessoa ou grupo que administra a implantação de TFS também executa esses procedimentos.

Como esse é um conjunto altamente privilegiado de permissões, novos projetos de equipe normalmente são criados por um pequeno subconjunto de usuários com a responsabilidade de administrar uma implantação do TFS. Os desenvolvedores não normalmente recebem as permissões necessárias para criar novos projetos de equipe.

### <a name="grant-permissions-in-tfs"></a>Conceder permissões no TFS

Se você quiser permitir que um usuário criar novos projetos de equipe, a primeira tarefa de alto nível é adicionar o usuário para o **administradores da coleção de projeto** grupo para a coleção de projeto de equipe.

**Para adicionar um usuário ao grupo Administradores de coleção de projeto**

1. No servidor do TFS, sobre o **inicie** , aponte para **todos os programas**, clique em **Microsoft Team Foundation Server 2010**e, em seguida, clique em **Team Foundation Console de administração**.
2. Na exibição de árvore de navegação, expanda **camada de aplicativo**e, em seguida, clique em **coleções de projetos de equipe**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. No **coleções de projetos de equipe** painel, selecione o projeto de equipe coleção que você deseja gerenciar.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Sobre o **gerais** , clique em **associação de grupo**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. No **grupos globais** caixa de diálogo, selecione o **administradores de coleção de projetos** agrupar e, em seguida, clique em **propriedades**.
6. No **propriedades de grupo do Team Foundation Server** caixa de diálogo, selecione **usuário do Windows ou grupo**e, em seguida, clique em **adicionar**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. No **selecionar usuários, computadores ou grupos** caixa de diálogo, digite o nome de usuário do usuário que você deseja ser capaz de criar novos projetos de equipe, clique em **verificar nomes**e, em seguida, clique em **Okey** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. No **propriedades de grupo do Team Foundation Server** caixa de diálogo, clique em **Okey**.
9. No **grupos globais** caixa de diálogo, clique em **fechar**.

### <a name="grant-permissions-in-sharepoint-services"></a>Conceder permissões no SharePoint Services

Em seguida, você precisa dar permissão ao usuário para criar novos sites de equipe na coleção de sites do SharePoint que corresponde à sua coleção de projeto de equipe do TFS.

**Para conceder permissões de controle total sobre o conjunto de sites do SharePoint**

1. No Console de administração do Team Foundation Server, sobre o **coleções de projetos de equipe** , selecione a coleção de projetos de equipe que você deseja gerenciar.
2. Sobre o **Site do SharePoint** guia, observe o valor da **local atual do Site padrão** URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Abra o Internet Explorer e, em seguida, vá para a URL que você anotou na etapa 2.

    > [!NOTE]
    > Se você não estiver conectado ao Windows como o usuário que criou a coleção de projetos de equipe, você precisará entrar no SharePoint como esse usuário para continuar.
4. Sobre o **ações do Site** menu, clique em **configurações de Site**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Sobre o **configurações de Site** página, em **usuários e permissões**, clique em **pessoas e grupos**.
6. No painel de navegação à esquerda, clique em **grupos**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Sobre o **pessoas e grupos: todos os grupos** , clique em **configurar grupos para esse Site**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Você pode receber um <strong>HTTP 404 não encontrado</strong> erro devido a um bug de codificação duplo do HTTP. Se isso ocorrer, substitua a URL com este:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Por exemplo:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Sobre o **configurar grupos para esse Site** página, adicione o usuário que criarão projetos de equipe para o **proprietários** de grupo e, em seguida, clique em **Okey**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Para obter mais informações sobre como habilitar usuários criar novos projetos de equipe em uma coleção de projeto de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Criar um novo projeto de equipe e adicionar usuários

Quando você tiver as permissões necessárias, você pode usar o **Team Explorer** janela no Visual Studio 2010 para criar um novo projeto de equipe. Essa abordagem oferece um assistente que coleta todas as informações necessárias e executa as tarefas necessárias no TFS, SharePoint e SQL Server Reporting Services. Você também precisará conceder permissões em um novo projeto de equipe aos membros da equipe do desenvolvedor, a fim de habilitá-los adicionar e modificar o conteúdo.

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Normalmente, um administrador do TFS ou um líder de equipe do desenvolvedor executa esses procedimentos.

### <a name="create-a-new-team-project"></a>Criar um novo projeto de equipe

O procedimento a seguir descreve como criar um novo projeto de equipe no TFS 2010.

**Para criar um novo projeto de equipe**

1. Sobre o **iniciar** , aponte para **todos os programas**, clique em **Microsoft Visual Studio 2010**, clique com botão direito **Microsoft Visual Studio 2010**, e, em seguida, clique em **executar como administrador**.

    > [!NOTE]
    > Se você não executar o Visual Studio 2010 como administrador, o Assistente do novo projeto de equipe falhará na última etapa.
2. Se o **User Account Control** caixa de diálogo for exibida, clique em **Sim**.
3. No Visual Studio, sobre o **Team** menu, clique em **conectar ao Team Foundation Server**.

    > [!NOTE]
    > Se você já tiver configurado uma conexão a um servidor TFS, você poderá omitir as etapas 4 a 7.
4. No **Conexão ao projeto de equipe** caixa de diálogo, clique em **servidores**.
5. No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **Add**.
6. No **adicionar Team Foundation Server** caixa de diálogo, forneça os detalhes da sua instância do TFS e, em seguida, clique em **Okey**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **fechar**.
8. No **conectar-se ao projeto de equipe** caixa de diálogo, selecione a instância do TFS que você deseja se conectar, selecione a equipe de projeto coleção que você deseja adicionar a e, em seguida, clique em **Connect**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. No **Team Explorer** janela, o botão direito do mouse, a equipe de coleção de projeto e, em seguida, clique em **novo projeto de equipe**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. No **novo projeto de equipe** caixa de diálogo, forneça um nome e uma descrição para o projeto de equipe e, em seguida, clique em **próxima**.

    > [!NOTE]
    > Se seu projeto de equipe contiver espaços, você pode enfrentar alguns problemas quando você chega a usar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para implantar pacotes do caminho de saída. Espaços no caminho podem fazer muito mais difícil executar comandos de implantação da Web.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Sobre o **selecione um modelo de processo** , selecione o modelo de processo que você deseja usar para gerenciar o processo de desenvolvimento e, em seguida, clique em **próxima**.

    > [!NOTE]
    > Para obter mais informações sobre modelos de processo do TFS, consulte [modelos de processo e ferramentas](https://msdn.microsoft.com/vstudio/aa718795).
12. Sobre o **configurações de Site de equipe** página, deixe as configurações padrão inalteradas e, em seguida, clique em **próxima**.
13. Essa configuração cria ou identifica um site de equipe do SharePoint que está associado com o projeto de equipe do TFS. Sua equipe de desenvolvimento pode usar este site para gerenciar a documentação, participar de discussões, criar páginas wiki e realizar várias outras tarefas que não estão relacionadas ao código. Para obter mais informações, consulte [interações entre produtos do SharePoint e o Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Sobre o **especificar configurações de controle do código-fonte** página, deixe as configurações padrão inalteradas e, em seguida, clique em **próxima**.
15. Esta configuração identifica ou cria o local na hierarquia de pastas TFS que atuará como uma pasta raiz para o seu conteúdo.
16. Sobre o **Confirmar configurações de projeto de equipe** , clique em **concluir**.
17. Quando o novo projeto de equipe é criado com êxito, diante a **projeto de equipe criado** , clique em **fechar**.

### <a name="add-users-to-a-team-project"></a>Adicionar usuários a um projeto de equipe

Agora que você criou um novo projeto de equipe, você pode conceder permissões aos usuários para que eles possam começar a adicionar e colaborar no conteúdo.

**Para adicionar usuários a um projeto de equipe**

1. No Visual Studio 2010, nos **Team Explorer** janela, o projeto de equipe com o botão direito, aponte para **configurações de projeto de equipe**e, em seguida, clique em **associação de grupo**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Para habilitar um usuário adicionar, modificar e remover o código sob controle do código-fonte, você precisará adicioná-lo para o **colaboradores** grupo.
3. No **grupos de projetos** caixa de diálogo, selecione o **colaboradores** agrupar e, em seguida, clique em **propriedades**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. No **propriedades de grupo do Team Foundation Server** caixa de diálogo, selecione **usuário do Windows ou grupo**e, em seguida, clique em **adicionar**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. No **selecionar usuários, computadores ou grupos** caixa de diálogo, digite o nome de usuário do usuário que você deseja adicionar ao projeto de equipe, clique em **verificar nomes**e, em seguida, clique em **Okey**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. No **propriedades de grupo do Team Foundation Server** caixa de diálogo, clique em **Okey**.
7. No **grupos de projetos** caixa de diálogo, clique em **fechar**.

## <a name="conclusion"></a>Conclusão

Neste ponto, seu novo projeto de equipe está pronto para uso, e sua equipe de desenvolvedores pode iniciar a adição de conteúdo e colaborar em processo de desenvolvimento.

Próximo tópico [adicionando conteúdo ao controle do código-fonte](adding-content-to-source-control.md), descreve como adicionar conteúdo ao controle de origem.

## <a name="further-reading"></a>Leitura adicional

Para obter mais ampla orientação sobre como criar projetos de equipe no TFS, consulte [criar um projeto de equipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Para obter mais informações sobre como habilitar usuários criar novos projetos de equipe em uma coleção de projeto de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx). Para obter mais informações sobre como adicionar usuários a projetos de equipe, consulte [adicionar usuários a projetos de equipe](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Próximo](adding-content-to-source-control.md)
