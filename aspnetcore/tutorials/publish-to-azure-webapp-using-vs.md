---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: df22852d2daddb2a3faef8404d0d250a6a1697a5
ms.sourcegitcommit: e987c950caae7af9c4ece8a82228caa364e0a5df
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)

## <a name="set-up"></a>Configurar

* Abra uma [conta do Azure gratuita](https://aka.ms/K5y5yh) se você não tiver uma. 

## <a name="create-a-web-app"></a>Criar um aplicativo Web

Na página inicial do Visual Studio, selecione **Arquivo > Novo > Projeto...**

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Preencha a caixa de diálogo **Novo Projeto**:

* No painel esquerdo, selecione **.NET Core**.

* No painel central, toque em **Aplicativo Web ASP.NET Core**.

* Selecione **OK**.

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:

* Selecione **Aplicativo Web**.

* Selecione **Mudar Autenticação**.

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

A caixa de diálogo **Mudar Autenticação** é exibida. 

* Selecione **Contas de Usuário Individuais**.

* Selecione **OK** para retornar para o **Novo Aplicativo Web do ASP.NET Core**, em seguida, selecione **OK** novamente.

![Caixa de diálogo Nova autenticação da Web do ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

O Visual Studio cria a solução.

## <a name="run-the-app-locally"></a>Executar o aplicativo localmente

* Selecione **Depurar** e **Iniciar sem Depurar** para executar o serviço.

* Clique nos links **Sobre** e **Contato** para verificar se o aplicativo Web funciona.

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* Selecione **Registrar** e registre um novo usuário. Você pode usar um endereço de email fictício. Ao enviar, a página exibirá o seguinte erro:

    *"Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema."*

* Selecione **Aplicar Migrações** e, depois que a página for atualizada, atualize a página.

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema.](publish-to-azure-webapp-using-vs/_static/mig.png)

O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logout**.

![Aplicativo Web aberto no Microsoft Edge. O link Registrar é substituído pelo texto Olá, email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Implantar o aplicativo no Azure

Feche a página da Web, retorne ao Visual Studio e selecione **Parar Depuração** no menu **Depurar**.

Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

Na caixa de diálogo **Publicar**, selecione **Serviço de Aplicativo do Microsoft Azure** e clique em **Publicar**.

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* Nomeie o aplicativo com um nome exclusivo. 

* Selecione uma assinatura.

* Selecione **Novo...** no grupo de recursos e insira um nome para o novo grupo de recursos.

* Selecione **Novo...** no plano do serviço de aplicativo e selecione um local próximo a você. Você pode manter o nome que é gerado por padrão.

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Selecione a guia **Serviços** para criar um novo banco de dados.

* Selecione o ícone verde **+** para criar um novo Banco de Dados SQL

![Novo Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* Selecione **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo banco de dados.

![Novo servidor e Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

A caixa de diálogo **Configurar o SQL Server** é exibida.

* Insira um nome de usuário do administrador e a senha e, em seguida, selecione **OK**. Não se esqueça do nome de usuário e da senha criados nesta etapa. Você pode manter o **Nome do Servidor** padrão. 

* Insira nomes para a cadeia de conexão e de banco de dados.

> [!NOTE]
> “admin” não é permitido como o nome de usuário administrador.

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Selecione **OK**.

O Visual Studio retorna para a caixa de diálogo **Criar Serviço de Aplicativo**.

* Selecione **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**.

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Clique no link **Configurações** na caixa de diálogo **Publicar**.

![Caixa de diálogo Publicar: painel Conexão](publish-to-azure-webapp-using-vs/_static/pubc.png)

Na página **Configurações** da caixa de diálogo **Publicar**:

  * Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão no tempo de execução**.

  * Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**.

* Selecione **Salvar**. O Visual Studio retorna para a caixa de diálogo **Publicar**. 

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

Clique em **Publicar**. O Visual Studio publicará o aplicativo no Azure e iniciará o aplicativo na nuvem no navegador.

### <a name="test-your-app-in-azure"></a>Testar o aplicativo no Azure

* Teste os links **Sobre** e **Contato**

* Registrar um novo usuário

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Atualizar o aplicativo

* Edite a página do Razor *Pages/About.cshtml* e altere seu conteúdo. Por exemplo, você pode modificar o parágrafo para dizer "Hello ASP.NET Core!":

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Clique com o botão direito do mouse no projeto e selecione **Publicar...** novamente.

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure.

![Verifique se a tarefa está concluída](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Limpar

Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.

* Selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos criado.

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* Na página **Grupos de recursos**, selecione **Excluir**.

![Portal do Azure: página Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Insira o nome do grupo de recursos e selecione **Excluir**. O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure.

### <a name="next-steps"></a>Próximas etapas

* [Implantação contínua no Azure com o Visual Studio e o Git](../publishing/azure-continuous-deployment.md)