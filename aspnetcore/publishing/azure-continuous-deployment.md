---
title: "Implantação contínua no Azure com o Visual Studio e o Git"
author: rick-anderson
description: "Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Implantação contínua no Azure para o ASP.NET Core, com o Visual Studio e o Git

Por [Erik Reitan](https://github.com/Erikre)

Este tutorial mostra como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo por meio do Visual Studio no Serviço de Aplicativo do Azure usando a implantação contínua. 

Consulte também [Usar o VSTS para criar e publicar um Aplicativo Web do Azure com a implantação contínua](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), que mostra como configurar um fluxo de trabalho de CD (entrega contínua) para o [Serviço de Aplicativo do Azure](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) usando o Visual Studio Team Services. A Entrega Contínua do Azure no Team Services simplifica a configuração de um pipeline de implantação robusta para publicar atualizações do aplicativo no Serviço de Aplicativo do Azure. O pipeline pode ser configurado no portal do Azure para criar, executar testes, implantar em um slot de preparo e, em seguida, implantar na produção.

> [!NOTE]
> Para concluir este tutorial, você precisa de uma conta do Microsoft Azure. Caso não tenha uma conta, [ative seus benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) ou [inscreva-se em uma avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial pressupõe que você já instalou o seguinte:

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://www.microsoft.com/net/download/core) (tempo de execução e ferramentas)

* [Git](https://git-scm.com/downloads) para Windows

## <a name="create-an-aspnet-core-web-app"></a>Criar um aplicativo Web ASP.NET Core

1. Inicie o Visual Studio.

2. No menu **Arquivo**, selecione **Novo** > **Projeto**.

3. Selecione o modelo de projeto **Aplicativo Web ASP.NET Core**. Ele será exibido em **Instalado** > **Modelos** > **Visual C#** > **.NET Core**. Nomeie o projeto `SampleWebAppDemo`. Selecione a opção **Criar novo repositório Git** e clique em **OK**.

   ![Caixa de diálogo Novo Projeto](azure-continuous-deployment/_static/01-new-project.png)

4. Na caixa de diálogo **Novo Projeto ASP.NET Core**, selecione o modelo **Vazio** do ASP.NET Core e clique em **OK**.

   ![Caixa de diálogo Novo Projeto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    >A versão mais recente do .NET Core é a 2.0

### <a name="running-the-web-app-locally"></a>Executando o aplicativo Web localmente

1. Depois que o Visual Studio concluir a criação do aplicativo, execute o aplicativo selecionando **Depurar** -> **Iniciar Depuração**. Como alternativa, você pode pressionar **F5**.

   Talvez seja necessário alguns instantes para inicializar o Visual Studio e o novo aplicativo. Quando isso for concluído, o navegador mostrará o aplicativo em execução.

   ![Janela do navegador mostrando o aplicativo em execução que exibe “Olá, Mundo!”](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Depois de examinar o aplicativo Web em execução, feche o navegador e clique no ícone “Parar depuração” na barra de ferramentas do Visual Studio para interromper o aplicativo.

## <a name="create-a-web-app-in-the-azure-portal"></a>Criar um aplicativo Web no Portal do Azure

As etapas a seguir orientarão você pela criação de um aplicativo Web no Portal do Azure.

1. Entre no [Portal do Azure](https://portal.azure.com)
2. Toque em **NOVO** na parte superior esquerda do Portal
3. Toque em **Web + Móvel** > **Aplicativo Web**

    ![Portal do Microsoft Azure: botão Novo: Web + Móvel em Marketplace: botão Aplicativo Web em Aplicativos em Destaque](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  Na folha **Aplicativo Web**, insira um valor exclusivo para o **Nome do Serviço de Aplicativo**.

    ![Folha Aplicativo Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >O **Nome do Serviço de Aplicativo** precisa ser exclusivo. O portal imporá essa regra quando você tentar inserir o nome. Depois de inserir outro valor, você precisará substituir esse valor por cada ocorrência do **SampleWebAppDemo** vista neste tutorial.

    &nbsp;
    
    Também na folha **Aplicativo Web**, selecione um **Plano do Serviço de Aplicativo/Localização** existente ou crie um novo. Se você criar um novo plano, selecione o tipo de preço, o local e outras opções. Para obter mais informações sobre os planos do Serviço de Aplicativo, consulte [Visão geral detalhada dos planos do Serviço de Aplicativo do Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  Clique em **Criar**. O Azure provisionará e iniciará o aplicativo Web.

    ![Portal do Azure: folha Conceitos Básicos da Demonstração de Aplicativo Web de Exemplo 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Habilitar a publicação do Git no novo aplicativo Web

O Git é um sistema de controle de versão distribuída que pode ser usado para implantar o aplicativo Web do Serviço de Aplicativo do Azure. Você armazenará o código escrito no aplicativo Web em um repositório Git local e implantará o código no Azure enviando-o por push para um repositório remoto.

1. Faça logon no [Portal do Azure](https://portal.azure.com), caso ainda não esteja conectado.

2. Clique em **Serviços do Aplicativo** para exibir uma lista de serviços do aplicativo associados à sua assinatura do Azure.

3. Selecione o aplicativo Web criado na seção anterior deste tutorial.

4. Na folha **Implantação**, selecione **Opções de implantação** > **Escolher Origem** > **Repositório Git Local**.

   ![Folha Configurações: folha Origem de implantação: folha Escolher origem](azure-continuous-deployment/_static/deployment-options.png)

5. Clique em **OK**.

6. Caso você não tenha configurado anteriormente as credenciais de implantação para publicar um aplicativo Web ou outro aplicativo do Serviço de Aplicativo, configure-as agora:

   * Clique em **Configurações** > **Credenciais de implantação**. A folha **Definir credenciais de implantação** será exibida.

   * Crie um nome de usuário e uma senha.  Você precisará dessa senha mais tarde ao configurar o Git.

   * Clique em **Salvar**.

7. Na folha **Aplicativo Web**, clique em **Configurações** > **Propriedades**. A URL do repositório Git remoto no qual você implantará é mostrada em **URL do GIT**.

8. Copie o valor **URL do GIT** para uso posterior no tutorial.

   ![Portal do Azure: folha Propriedades do aplicativo](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Publicar o aplicativo Web no Serviço de Aplicativo do Azure

Nesta seção, você criará um repositório Git local usando o Visual Studio e enviará por push desse repositório para o Azure para implantar o aplicativo Web. As etapas envolvidas incluem as seguintes:

   * Adicione a configuração do repositório remoto usando o valor de URL do GIT, de modo que você possa implantar seu repositório local no Azure.

   * Confirme as alterações do projeto.

   * Envie as alterações do projeto por push do repositório local para o repositório remoto no Azure.

&nbsp;
   
1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**. O **Team Explorer** será exibido.

    ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

2.  No **Team Explorer**, selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações do Repositório**.

3.  Na seção **Remotos** das **Configurações do Repositório**, selecione **Adicionar**. A caixa de diálogo **Adicionar Remoto** será exibida.

4.  Defina o **Nome** do remoto como **Azure-SampleApp**.

5.  Defina o valor de **Buscar** como a **URL do Git** copiada do Azure anteriormente neste tutorial. Observe que essa é a URL que termina com **.git**.

    ![Caixa de diálogo Editar Remoto](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >Como alternativa, você pode especificar o repositório remoto na **Janela Comando** abrindo a **Janela Comando**, alterando para o diretório do projeto e inserindo o comando. Por exemplo: `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações Globais**. Verifique se você tem o nome e o endereço de email definidos. Talvez você também precise selecionar **Atualizar**.

7.  Selecione **Página Inicial** > **Alterações** para retornar à exibição **Alterações**.

8.  Escreva uma mensagem de confirmação, como **Push Inicial nº 1** e clique em **Confirmar**. Essa ação criará uma *confirmação* localmente. Em seguida, você precisa *sincronizar* com o Azure.

    ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >Como alternativa, você pode confirmar as alterações na **Janela Comando** abrindo a **Janela Comando**, alterando para o diretório do projeto e inserindo os comandos do Git. Por exemplo:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Selecione **Página Inicial** > **Sincronização** > **Ações** > **Abrir Prompt de Comando**. O prompt de comando será aberto no diretório do projeto.

10.  Insira o seguinte comando na janela de comando:

    `git push -u Azure-SampleApp master`

11.  Insira a senha das **credenciais de implantação** do Azure criada anteriormente no Azure.

    >[!NOTE]
    >Sua senha não será visível enquanto você a digita.

    Esse comando iniciará o processo de envio por push dos arquivos de projeto locais para o Azure. O resultado do comando acima termina com uma mensagem informando que a implantação foi bem-sucedida.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Se precisar colaborar em um projeto, considere a possibilidade de envio por push para o [GitHub](https://github.com) entre o envio por push para o Azure.
 
### <a name="verify-the-active-deployment"></a>Verificar a implantação ativa

Verifique se você transferiu com êxito o aplicativo Web do ambiente local para o Azure. Você verá a implantação bem-sucedida listada.

1. No [Portal do Azure](https://portal.azure.com), selecione o aplicativo Web. Em seguida, selecione **Implantação** > **Opções de implantação**.

   ![Portal do Azure: folha Configurações: folha Implantações mostrando a implantação bem-sucedida](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Executar o aplicativo no Azure

Agora que você implantou o aplicativo Web para o Azure, poderá executar o aplicativo.

Isso pode ser feito de duas maneiras:

* No Portal do Azure, localize a folha do aplicativo Web e clique em **Procurar** para exibir o aplicativo no navegador padrão.

* Abra um navegador e insira a URL do aplicativo Web. Por exemplo:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Atualizar o aplicativo Web e publicá-lo novamente

Depois de fazer alterações no código local, você poderá republicá-lo.

1.  No **Gerenciador de Soluções** do Visual Studio, abra o arquivo *Startup.cs*.

2.  No método `Configure`, modifique o método `Response.WriteAsync` para que ele seja exibido da seguinte maneira:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Salve as alterações em *Startup.cs*.

4.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**. O **Team Explorer** será exibido.

5.  Escreva uma mensagem de confirmação, como:

    ```none
    Update #2
    ```

6.  Pressione o botão **Confirmar** para confirmar as alterações do projeto.

7.  Selecione **Página Inicial** > **Sincronização** > **Ações** > **Enviar por Push**.

>[!NOTE]
>Como alternativa, você pode enviar as alterações por push na **Janela Comando** abrindo a **Janela Comando**, alterando para o diretório do projeto e inserindo um comando do Git. Por exemplo:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Exibir o aplicativo Web atualizado no Azure

Exiba o aplicativo Web atualizado selecionando **Procurar** na folha do aplicativo Web no Portal do Azure ou abrindo um navegador e inserindo a URL do aplicativo Web. Por exemplo:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Recursos adicionais

* [Publicação e implantação](index.md)

* [Kudu do projeto](https://github.com/projectkudu/kudu/wiki)
