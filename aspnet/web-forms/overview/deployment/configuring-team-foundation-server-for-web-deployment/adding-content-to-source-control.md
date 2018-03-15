---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Adicionar conteúdo ao controle do código-fonte | Microsoft Docs"
author: jrjlee
description: "Este tópico explica como adicionar conteúdo ao controle do código-fonte no Team Foundation Server (TFS) 2010. Descreve como adicionar soluções e projetos para um projeto de equipe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="adding-content-to-source-control"></a>Adicionar conteúdo ao controle de origem
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico explica como adicionar conteúdo ao controle do código-fonte no Team Foundation Server (TFS) 2010. Descreve como adicionar soluções e projetos para um projeto de equipe no TFS e explica como adicionar dependências externas como estruturas ou assemblies ao controle de origem.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

## <a name="task-overview"></a>Visão geral da tarefa

Na maioria dos casos, cada membro da equipe do desenvolvedor deve ser capaz de adicionar conteúdo ao controle de origem. Para adicionar uma solução ao controle de origem no TFS, você precisará concluir essas etapas de alto nível:

- Conecte a um projeto de equipe.
- Mapeie a estrutura de pastas do projeto de equipe no servidor para uma estrutura de pastas no computador local.
- Adicione a solução e seu conteúdo ao controle de origem.
- Adicione quaisquer dependências externas ao controle de origem.

Neste tópico mostram como executar esses procedimentos.

As tarefas e instruções passo a passo neste tópico pressupõem que você já criou um novo projeto de equipe do TFS para gerenciar o conteúdo. Para obter mais informações sobre como criar um novo projeto de equipe, consulte [criando um projeto de equipe no TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Na maioria dos casos, cada membro da equipe do desenvolvedor deve ser capaz de adicionar e modificar o conteúdo dentro de projetos de equipe específico.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Conectar a um projeto de equipe e criar um mapeamento de pasta

Antes de adicionar conteúdo ao controle de origem, você precisa se conectar a um projeto de equipe e criar um mapeamento entre a estrutura de pastas no servidor e o sistema de arquivos em seu computador local.

**Para se conectar a um projeto de equipe e mapear um caminho de local**

1. Em sua estação de trabalho do desenvolvedor, abra o Visual Studio 2010.
2. No Visual Studio, no **equipe** menu, clique em **conectar ao Team Foundation Server**.

    > [!NOTE]
    > Se você já tiver configurado uma conexão a um servidor do TFS, você poderá omitir as etapas 3 a 6.
3. No **Conexão ao projeto de equipe** caixa de diálogo, clique em **servidores**.
4. No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **adicionar**.
5. No **adicionar Team Foundation Server** caixa de diálogo, forneça os detalhes de sua instância do TFS e, em seguida, clique em **Okey**.

    ![](adding-content-to-source-control/_static/image1.png)
6. No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **fechar**.
7. No **conectar-se ao projeto de equipe** caixa de diálogo, selecione a instância do TFS que deseja se conectar, selecione a equipe de projeto coleção, selecione o projeto de equipe que você deseja adicionar a e clique **conectar**.

    ![](adding-content-to-source-control/_static/image2.png)
8. No **Team Explorer** janela, expanda seu projeto de equipe e, em seguida, clique duas vezes em **controle de origem**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Sobre o **Gerenciador de controle do código-fonte** , clique em **não mapeado**.

    ![](adding-content-to-source-control/_static/image4.png)
10. No **mapa** na caixa de **pasta Local** caixa, navegue até (ou crie) uma pasta local para agir como a pasta raiz do projeto de equipe e, em seguida, clique em **mapa**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Quando você for solicitado a baixar arquivos de origem, clique em **Sim**.

    ![](adding-content-to-source-control/_static/image6.png)

Neste ponto, você mapeou na pasta do servidor para o projeto de equipe para uma pasta local em sua estação de trabalho do desenvolvedor. Você também baixar todo o conteúdo existente do projeto de equipe para sua estrutura de pasta local. Agora, você pode começar a adicionar seus próprios dados ao controle de origem.

## <a name="add-projects-and-solutions-to-source-control"></a>Adicionar soluções e projetos ao controle de origem

Para adicionar soluções e projetos ao controle de origem, primeiro será necessário movê-los para a pasta mapeada para o projeto de equipe em seu computador local. Em seguida, você pode verificar no conteúdo para sincronizar suas inclusões com o servidor.

**Para adicionar projetos ao controle de origem**

1. Em sua estação de trabalho do desenvolvedor, mova seus projetos e soluções em um local apropriado dentro da estrutura de pasta mapeada para o projeto de equipe.

    > [!NOTE]
    > Muitas organizações terão uma abordagem preferencial como projetos e soluções devem ser organizados em controle de origem. Para obter orientação sobre como pastas de estrutura, consulte [como: controle de pastas de origem sua estrutura no Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Abra a solução no Visual Studio 2010.
3. No **Solution Explorer** janela, clique com botão direito a solução e, em seguida, clique em **adicionar solução ao controle de origem**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > Em alguns casos, dependendo de como sua organização gosta de conteúdo da estrutura no TFS, talvez seja necessário adicionar projetos ao controle de origem individualmente para fornecer um controle mais refinado sobre como o seu código-fonte é organizado.
4. Verifique o **Gerenciador de controle do código-fonte** guia exibe o conteúdo que você adicionou na estrutura de pastas do servidor para o projeto de equipe.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > O **Gerenciador de controle do código-fonte** guia exibe seu conteúdo com nenhum outro aviso porque você adicionou a solução em uma pasta mapeada no sistema de arquivos local. Se sua solução estava em um local não mapeado, você será solicitado para especificar locais de pasta no TFS e o sistema de arquivos local.
5. No **Gerenciador de controle do código-fonte** guia o **pastas** painel, clique o projeto de equipe (por exemplo, **ContactManager**) e, em seguida, clique em **Check-In As alterações pendentes**.
6. No **Check-In – arquivos de origem** caixa de diálogo, digite um comentário e, em seguida, clique em **Check-In**.

    ![](adding-content-to-source-control/_static/image9.png)

Neste ponto, você adicionou sua solução ao controle de origem no TFS.

## <a name="add-external-dependencies-to-source-control"></a>Adicionar dependências externas ao controle de origem

Quando você adiciona um projeto ou solução ao controle de origem, todos os arquivos e pastas dentro de seu projeto ou solução também serão adicionadas. No entanto, em muitos casos, projetos e soluções também dependem de dependências externas, como assemblies locais, para funcionar corretamente. Você precisa adicionar todos esses recursos ao controle de origem para permitir que o Team Build e outros membros da equipe de desenvolvedor compilar seu código com êxito.

Por exemplo, a estrutura de pasta para a solução de exemplo Contact Manager inclui uma pasta chamada pacotes. Contém o assembly e vários recursos de suporte para o ADO.NET Entity Framework 4.1. A pasta de pacotes não é parte da solução de Gerenciador de contato, mas a solução não será criado com êxito sem eles. Para habilitar o Team Build compilar a solução, você precisa adicionar a pasta pacotes ao controle de origem.

> [!NOTE]
> A inclusão de uma pasta de pacotes é típica do que acontece quando você adiciona o Entity Framework ou recursos semelhantes para a solução usando a extensão do NuGet para Visual Studio 2010.


**Para adicionar o conteúdo do projeto ao controle de origem**

1. Certifique-se de que os itens que você deseja adicionar (por exemplo, a pasta de pacotes) estão em um local apropriado dentro de uma pasta mapeada no sistema de arquivos local.
2. No Visual Studio 2010, no **Team Explorer** janela, expanda seu projeto de equipe e, em seguida, clique duas vezes em **controle de origem**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Sobre o **Gerenciador de controle do código-fonte** guia o **pastas** painel, selecione a pasta que contém o item ou itens que você deseja adicionar.
4. Clique o **adicionar itens à pasta** botão.

    ![](adding-content-to-source-control/_static/image11.png)
5. No **adicionar ao controle de origem** caixa de diálogo, selecione a pasta ou itens que você deseja adicionar e, em seguida, clique em **próximo**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Sobre o **excluídos itens** , selecione todos os itens necessários foram excluídos automaticamente (por exemplo, assemblies) e, em seguida, clique em **incluem itens**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Sobre o **itens para adicionar** guia, verifique se todos os arquivos que você deseja incluir são listados e, em seguida, clique em **concluir**.

    ![](adding-content-to-source-control/_static/image14.png)
8. No **Gerenciador de controle do código-fonte** janela, clique no **Check-In** botão.

    ![](adding-content-to-source-control/_static/image15.png)
9. No **Check-In – arquivos de origem** caixa de diálogo, digite um comentário e, em seguida, clique em **Check-In**.

Neste ponto, você adicionou as dependências externas para sua solução ao controle de origem.

## <a name="conclusion"></a>Conclusão

Este tópico descrita como se conectar a um projeto de equipe, mapear uma estrutura de pasta e adicionar conteúdo ao controle de origem. Para obter mais informações sobre como trabalhar com itens sob controle de origem, consulte [usando o controle de versão](https://msdn.microsoft.com/library/ms181368.aspx).

O próximo tópico, [Configurando um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md), descreve como preparar um servidor TFS Team Build para compilar e implantar sua solução.

## <a name="further-reading"></a>Leitura adicional

Para obter informações mais abrangentes sobre como trabalhar com o controle de origem no TFS, consulte [usando o controle de versão](https://msdn.microsoft.com/library/ms181368.aspx).

>[!div class="step-by-step"]
[Anterior](creating-a-team-project-in-tfs.md)
[Próximo](configuring-a-tfs-build-server-for-web-deployment.md)
