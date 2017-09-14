---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Cesar Blum Silveira](https://github.com/cesarbs)

## <a name="set-up-the-development-environment"></a>Configurar o ambiente de desenvolvimento

* Instale a última versão do [SDK do Azure para Visual Studio](https://www.visualstudio.com/features/azure-tools-vs). O SDK instala o Visual Studio, caso você ainda não tenha feito isso.

> [!NOTE]
> A instalação do SDK poderá levar mais de 30 minutos, se o computador não tiver muitas das dependências.

* Instale o [.NET Core + ferramentas do Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306)

* Confirme sua [conta do Azure](https://portal.azure.com/). [Abra uma conta gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/) ou [ative os benefícios do assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="create-a-web-app"></a>Criar um aplicativo Web

No Página Inicial do Visual Studio, toque em **Novo Projeto...**.

![Start Page](publish-to-azure-webapp-using-vs/_static/new_project.png)

Como alternativa, você pode usar os menus para criar um novo projeto. Toque em **Arquivo > Novo > Projeto...**.

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

Preencha a caixa de diálogo **Novo Projeto**:

* No painel esquerdo, toque em **Web**

* No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**

* Toque em **OK**

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core (.NET Core)**:

* Toque em **Aplicativo Web**

* Verifique se a opção **Autenticação** está definida como **Contas de Usuário Individuais**

* Verifique se a opção **Hospedar na nuvem** **não** está marcada

* Toque em **OK**

![Caixa de diálogo Novo Aplicativo Web ASP.NET Core (.NET Core)](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a>Testar o aplicativo localmente

* Pressione **Ctrl-F5** para executar o aplicativo localmente

* Toque nos links **Sobre** e **Contato**. Dependendo do tamanho do dispositivo, talvez você precise tocar no ícone de navegação para mostrar os links

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* Toque em **Registrar** e registre um novo usuário. Você pode usar um endereço de email fictício. Ao enviar, você receberá o seguinte erro:

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema.](publish-to-azure-webapp-using-vs/_static/mig.png)

Você pode corrigir o problema de duas maneiras diferentes:

* Toque em **Aplicar Migrações** e, depois que a página for atualizada, atualize a página; ou

* Execute o seguinte em um prompt de comando no diretório do projeto:

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logoff**.

![Aplicativo Web aberto no Microsoft Edge. O link Registrar é substituído pelo texto Olá, abc@example.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Implantar o aplicativo no Azure

Confirme se o aplicativo publicado para implantação não está em execução. Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução. A implantação não pode ocorrer porque os arquivos bloqueados não podem ser copiados.

Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

Na caixa de diálogo **Publicar**, toque em **Serviço de Aplicativo do Microsoft Azure**.

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

Toque em **Novo...** para criar um novo grupo de recursos. A criação de um novo grupo de recursos facilitará a exclusão de todos os recursos do Azure criados neste tutorial.

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

Crie um novo grupo de recursos e um plano do serviço de aplicativo:

* Toque em **Novo...** no grupo de recursos e insira um nome para o novo grupo de recursos

* Toque em **Novo...** no plano do serviço de aplicativo e selecione um local próximo a você. Você pode manter o nome padrão gerado

* Toque em **Explorar serviços adicionais do Azure** para criar um novo banco de dados

![Caixa de diálogo Novo Grupo de Recursos: painel Hospedagem](publish-to-azure-webapp-using-vs/_static/cas.png)

* Toque no ícone verde **+** para criar um novo Banco de Dados SQL

![Caixa de diálogo Novo Grupo de Recursos: painel Serviços](publish-to-azure-webapp-using-vs/_static/sql.png)

* Toque em **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo servidor de banco de dados.

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

* Insira um nome de usuário administrador e a senha e, em seguida, toque em **OK**. Não se esqueça do nome de usuário e da senha criados nesta etapa. Você pode manter o **Nome do Servidor** padrão

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> “admin” não é permitido como o nome de usuário administrador.

* Toque em **OK** na caixa de diálogo **Configurar Banco de Dados SQL**

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Toque em **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**

![Caixa de diálogo Criar Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/create_as.png)

* Toque em **Avançar** na caixa de diálogo **Publicar**

![Caixa de diálogo Publicar: painel Conexão](publish-to-azure-webapp-using-vs/_static/pubc.png)

* No estágio **Configurações** da caixa de diálogo **Publicar**:

  * Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão em tempo de execução**

  * Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**

* Toque em **Publicar** e aguarde até que o Visual Studio conclua a publicação do aplicativo

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

O Visual Studio publicará o aplicativo no Azure e iniciará o aplicativo na nuvem no navegador.

### <a name="test-your-app-in-azure"></a>Testar o aplicativo no Azure

* Teste os links **Sobre** e **Contato**

* Registrar um novo usuário

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a>Atualizar o aplicativo

* Edite o arquivo de exibição `Views/Home/About.cshtml` do Razor e altere seu conteúdo. Por exemplo:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* Clique com o botão direito do mouse no projeto e toque em **Publicar...**  novamente

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure

### <a name="clean-up"></a>Limpar

Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.

* Selecione **Grupos de recursos** e, em seguida, toque no grupo de recursos criado

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* Na folha **Grupo de recursos**, toque em **Excluir**

![Portal do Azure: folha Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Insira o nome do grupo de recursos e toque em **Excluir**. O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure

### <a name="next-steps"></a>Próximas etapas

* [Introdução ao ASP.NET Core MVC e ao Visual Studio](first-mvc-app/start-mvc.md)

* [Introdução ao ASP.NET Core](../index.md)

* [Conceitos básicos](../fundamentals/index.md)
