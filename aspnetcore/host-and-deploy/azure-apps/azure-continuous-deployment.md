---
title: "Implantação contínua no Azure com o Visual Studio e o Git"
author: rick-anderson
description: "Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: c1d25b109bbf211eb476860ff77b649565960b62
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Implantação contínua para o Azure para o ASP.NET Core com o Visual Studio e Git

Por [Erik Reitan](https://github.com/Erikre)

Este tutorial mostra como criar um aplicativo web do ASP.NET Core usando o Visual Studio e implantá-lo do Visual Studio para o serviço de aplicativo do Azure usando a implantação contínua.

Consulte também [Usar o VSTS para criar e publicar um Aplicativo Web do Azure com a implantação contínua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), que mostra como configurar um fluxo de trabalho de CD (entrega contínua) para o [Serviço de Aplicativo do Azure](/azure/app-service/app-service-web-overview) usando o Visual Studio Team Services. Entrega contínua do Azure no Team Services simplifica a configuração de um pipeline de implantação robusta para publicar atualizações para aplicativos hospedados no serviço de aplicativo do Azure. O pipeline pode ser configurado no portal do Azure para criar, executar testes, implantar em um slot de preparo e, em seguida, implantar na produção.

> [!NOTE]
> Para concluir este tutorial, é necessária uma conta do Microsoft Azure. Para obter uma conta, [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) ou [inscrever-se para uma avaliação gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial pressupõe que o seguinte software está instalado:

* [Visual Studio](https://www.visualstudio.com)
* [SDK do .NET core](https://www.microsoft.com/net/download/core) (tempo de execução e as ferramentas)
* [Git](https://git-scm.com/downloads) para Windows

## <a name="create-an-aspnet-core-web-app"></a>Criar um aplicativo Web ASP.NET Core

1. Inicie o Visual Studio.

1. No menu **Arquivo**, selecione **Novo** > **Projeto**.

1. Selecione o modelo de projeto **Aplicativo Web ASP.NET Core**. Ele será exibido em **Instalado** > **Modelos** > **Visual C#** > **.NET Core**. Nomeie o projeto `SampleWebAppDemo`. Selecione a opção **Criar novo repositório Git** e clique em **OK**.

   ![Caixa de diálogo Novo Projeto](azure-continuous-deployment/_static/01-new-project.png)

1. Na caixa de diálogo **Novo Projeto ASP.NET Core**, selecione o modelo **Vazio** do ASP.NET Core e clique em **OK**.

   ![Caixa de diálogo Novo Projeto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> A versão mais recente do .NET Core é 2.0.

### <a name="running-the-web-app-locally"></a>Executando o aplicativo Web localmente

1. Depois que o Visual Studio concluir a criação do aplicativo, execute o aplicativo selecionando **Depurar** > **Iniciar Depuração**. Como alternativa, pressione **F5**.

   Talvez seja necessário alguns instantes para inicializar o Visual Studio e o novo aplicativo. Quando ele for concluído, o navegador mostra o aplicativo em execução.

   ![Janela do navegador mostrando o aplicativo em execução que exibe “Olá, Mundo!”](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Depois de revisar o aplicativo Web em execução, feche o navegador e selecione o ícone de "Parar depuração" na barra de ferramentas do Visual Studio para interromper o aplicativo.

## <a name="create-a-web-app-in-the-azure-portal"></a>Criar um aplicativo Web no Portal do Azure

As seguintes etapas criam um aplicativo web no Portal do Azure:

1. Faça logon na [Portal do Azure](https://portal.azure.com).

1. Selecione **novo** na parte superior esquerda da interface do portal.

1. Selecione **Web + móvel** > **aplicativo da Web**.

   ![Portal do Microsoft Azure: botão Novo: Web + Móvel em Marketplace: botão Aplicativo Web em Aplicativos em Destaque](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. Na folha **Aplicativo Web**, insira um valor exclusivo para o **Nome do Serviço de Aplicativo**.

   ![Folha Aplicativo Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > O **nome do serviço de aplicativo** nome deve ser exclusivo. O portal impõe essa regra quando o nome for fornecido. Se fornecer um valor diferente, substitua esse valor para cada ocorrência do **SampleWebAppDemo** neste tutorial.

   Também na folha **Aplicativo Web**, selecione um **Plano do Serviço de Aplicativo/Localização** existente ou crie um novo. Se criar um novo plano, selecione a camada de preços, local e outras opções. Para obter mais informações sobre planos de serviço de aplicativo, consulte [visão geral detalhada de planos de serviço de aplicativo do Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Selecione **Criar**. Azure provisionará e iniciar o aplicativo da web.

   ![Portal do Azure: folha Conceitos Básicos da Demonstração de Aplicativo Web de Exemplo 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Habilitar a publicação do Git no novo aplicativo Web

Git é um sistema de controle de versão distribuídos que pode ser usado para implantar um aplicativo da web do serviço de aplicativo do Azure. Código de aplicativo da Web é armazenado em um repositório Git local e o código é implantado no Azure por push para um repositório remoto.

1. Faça logon na [Portal do Azure](https://portal.azure.com).

1. Selecione **serviços de aplicativos** para exibir uma lista dos serviços de aplicativo associado à assinatura do Azure.

1. Selecione o aplicativo web criado na seção anterior deste tutorial.

1. Na folha **Implantação**, selecione **Opções de implantação** > **Escolher Origem** > **Repositório Git Local**.

   ![Folha Configurações: folha Origem de implantação: folha Escolher origem](azure-continuous-deployment/_static/deployment-options.png)

1. Selecione **OK**.

1. Se as credenciais de implantação para publicar um aplicativo web ou outro aplicativo de serviço de aplicativo ainda não foram configuradas anteriormente, configurá-los agora:

   * Selecione **configurações** > **credenciais de implantação**. O **definir credenciais de implantação** folha é exibida.
   * Crie um nome de usuário e uma senha. Salve a senha para uso posterior ao configurar o Git.
   * Selecione **Salvar**.

1. No **aplicativo Web** folha, selecione **configurações** > **propriedades**. A URL do repositório Git remoto para implantar é mostrada em **URL do GIT**.

1. Copie o valor **URL do GIT** para uso posterior no tutorial.

   ![Portal do Azure: folha Propriedades do aplicativo](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Publicar o aplicativo web para o serviço de aplicativo do Azure

Nesta seção, crie um repositório Git local usando Visual Studio e envio desse repositório para o Azure para implantar o aplicativo web. As etapas envolvidas incluem as seguintes:

* Adicione a configuração do repositório remoto usando o valor da URL de GIT, para que o repositório local pode ser implantado no Azure.
* Confirme as alterações do projeto.
* Enviar por push as alterações do projeto do repositório local para o repositório remoto no Azure.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**. O **Team Explorer** é exibido.

   ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. No **Team Explorer**, selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações do Repositório**.

1. No **programáveis** seção o **configurações do repositório**, selecione **adicionar**. O **adicionar remoto** caixa de diálogo é exibida.

1. Defina o **Nome** do remoto como **Azure-SampleApp**.

1. Definir o valor de **buscar** para o **URL de Git** que copiados do Azure anteriormente neste tutorial. Observe que essa é a URL que termina com **.git**.

   ![Caixa de diálogo Editar Remoto](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Como alternativa, especifique o repositório remoto do **janela comando** abrindo o **janela de comando**, alterar para o diretório do projeto e inserir o comando. Exemplo:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações Globais**. Confirme se o nome e endereço de email estão definidas. Selecione **atualização** se necessário.

1. Selecione **Página Inicial** > **Alterações** para retornar à exibição **Alterações**.

1. Insira uma mensagem de confirmação, como **inicial Push #1** e selecione **confirmação**. Essa ação cria um *confirmação* localmente.

   ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Como alternativa, confirmação muda do **janela comando** abrindo o **janela de comando**, alterar para o diretório do projeto e digitar os comandos do git. Exemplo:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Selecione **Página Inicial** > **Sincronização** > **Ações** > **Abrir Prompt de Comando**. Abre o prompt de comando para o diretório do projeto.

1. Insira o seguinte comando na janela de comando:

   `git push -u Azure-SampleApp master`

1. Insira o Azure **credenciais de implantação** senha criados anteriormente no Azure.

   Esse comando inicia o processo de envio por push os arquivos do projeto local para o Azure. A saída do comando acima termina com uma mensagem de que a implantação foi bem-sucedida.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Se a colaboração no projeto é necessária, considere a possibilidade de envio por push para [GitHub](https://github.com) antes de enviar por push para o Azure.
 
### <a name="verify-the-active-deployment"></a>Verificar a implantação ativa

Verifique se a transferência de aplicativos web do ambiente local para o Azure é bem-sucedida.

No [Portal do Azure](https://portal.azure.com), selecione o aplicativo web. Selecione **implantação** > **opções de implantação**.

![Portal do Azure: folha Configurações: folha Implantações mostrando a implantação bem-sucedida](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Executar o aplicativo no Azure

Agora que o aplicativo web é implantado no Azure, execute o aplicativo.

Isso pode ser feito de duas maneiras:

* No Portal do Azure, localize a folha do aplicativo web para o aplicativo web. Selecione **procurar** para exibir o aplicativo no navegador padrão.
* Abra um navegador e digite a URL do aplicativo web. Exemplo: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Atualizar o aplicativo web e publicar novamente

Depois de fazer alterações no código local, republicar:

1. No **Gerenciador de Soluções** do Visual Studio, abra o arquivo *Startup.cs*.

1. No método `Configure`, modifique o método `Response.WriteAsync` para que ele seja exibido da seguinte maneira:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Salvar as alterações em *Startup.cs*.

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**. O **Team Explorer** é exibido.

1. Insira uma mensagem de confirmação, como `Update #2`.

1. Pressione o botão **Confirmar** para confirmar as alterações do projeto.

1. Selecione **Página Inicial** > **Sincronização** > **Ações** > **Enviar por Push**.

> [!NOTE]
> Como alternativa, enviar por push as alterações do **janela comando** abrindo o **janela de comando**, alterar para o diretório do projeto e inserir um comando do git. Exemplo:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Exibir o aplicativo Web atualizado no Azure

Exibir o aplicativo web atualizadas selecionando **procurar** na folha de aplicativo web no Portal do Azure ou abrindo um navegador e digitando a URL do aplicativo web. Exemplo: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Recursos adicionais

* [Usar o VSTS para criar e publicar um aplicativo Web do Azure com implantação contínua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Kudu do projeto](https://github.com/projectkudu/kudu/wiki)
