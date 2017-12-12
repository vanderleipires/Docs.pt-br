---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publicar o site MVC banco de dados primeiro no Azure | Microsoft Docs
author: tfitzmac
description: "Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: f75b7192b4d97c88fcbcb4ad7fef88c83157c902
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publicar o site MVC banco de dados primeiro no Azure
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra em publicar o aplicativo web e o banco de dados no Azure. Você pode ler este tópico para saber mais sobre como publicar um aplicativo web e o banco de dados, mas, na verdade, execute as etapas que você deve iniciar no início do tutorial. Consulte [Introdução](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Implantar o aplicativo web no Azure

Você precisa de uma conta do Azure para concluir este tutorial:

- Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.
- Você pode [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.

Para publicar seu aplicativo web, clique com o botão direito e selecione **publicar**.

![publicar o site](publish-to-azure/_static/image1.png)

Selecione os sites do Microsoft Azure.

![Selecionar o Azure](publish-to-azure/_static/image2.png)

Se você não tiver entrado Azure, forneça suas credenciais de conta do Azure. Em seguida, selecione Novo para criar um novo aplicativo web.

![novo site](publish-to-azure/_static/image3.png)

Crie um nome exclusivo para seu aplicativo web. Você saberá que o nome é exclusivo se houver uma marca de seleção verde à direita do campo nome. Selecione uma região para seu aplicativo web. Selecione **criar novo servidor** para o banco de dados e forneça um nome de usuário e senha para o novo servidor de banco de dados. Quando terminar, clique em **criar**.

![Criar site](publish-to-azure/_static/image4.png)

Os valores de conexão estão agora definidos. Você pode deixar esses valores inalterados.

![conexão](publish-to-azure/_static/image5.png)

Clique em **Avançar**.

Para configurações, observe que dois banco de dados conexões são especificadas - ApplicationDBContext e ContosoUniversityDataEntities. ApplicationDBContext é a conexão para as tabelas de conta de usuário. Esses valores mostram apenas as cadeias de caracteres de conexão para bancos de dados. Isso não significa que esses bancos de dados serão publicados quando você publica seu site. Depois de publicar o aplicativo web, você publicará seu projeto de banco de dados.

![](publish-to-azure/_static/image6.png)

As reticências (...) ao lado de conexão de banco de dados mostra os detalhes da cadeia de caracteres de conexão. Clique no botão de reticências ao lado de ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Observe o nome do servidor de banco de dados e o banco de dados. O nome do servidor é gerado aleatoriamente. O nome do banco de dados é simple o nome do seu site com  **\_db** anexado. Você precisará mais tarde os dois nomes quando você publica seu banco de dados.

Clique em **Okey** para fechar a janela de cadeia de caracteres de conexão de banco de dados.

Na janela de publicar Web, clique em **próximo** para exibir a visualização.

![](publish-to-azure/_static/image8.png)

Você pode clicar em Iniciar a visualização para ver uma lista de arquivos para publicar. Como esta é na primeira vez que você publicou este site, a lista é todos os arquivos relevantes no projeto.

Clique em **Publicar**.

O painel de saída exibirá o resultado da publicação.

![](publish-to-azure/_static/image9.png)

Após a publicação, o site é immedialely iniciado em um navegador da web. O site foi implantado e você pode registrar um novo usuário para o site. No entanto, as tabelas no projeto ContosoUniversityData ainda não foram publicadas. Se você clicar na lista de link de alunos, você receberá um erro.

## <a name="publish-database-to-sql-azure"></a>Publicar o banco de dados para o SQL Azure

Antes de publicar seu banco de dados, você deve verificar se o que computador local pode se conectar ao servidor de banco de dados. O firewall para o servidor de banco de dados restringe quais computadores podem se conectar ao banco de dados. Você precisa adicionar o endereço IP do seu computador para os endereços IP permitidos para o firewall.

Faça logon em sua conta do Azure por meio do portal do Azure.

Selecione o novo banco de dados e selecione **gerenciar**.

![Gerenciar](publish-to-azure/_static/image10.png)

Você deve configurar o servidor de banco de dados para permitir conexões do seu computador. Quando você seleciona a gerenciar, você será solicitado a adicionar o endereço IP atual, conforme permitido para o servidor de banco de dados. Selecione Sim.

![Adicionar endereço ip](publish-to-azure/_static/image11.png)

Há uma chance de que o endereço IP que você adicionou na etapa anterior não é o endereço IP só que é necessário configurar para conexões. Você pode tentar fazer logon no banco de dados para ver se as conexões foram corretamente configuradas. Forneça o usuário e a senha que você criou anteriormente.

![Logon](publish-to-azure/_static/image12.png)

Se você receber uma mensagem de erro, você precisa adicionar outro endereço IP. Clique na mensagem de erro para ver mais detalhes sobre o erro. Os detalhes, você verá o endereço IP que você precisa adicionar. Observe que esse endereço IP.

![não permitido](publish-to-azure/_static/image13.png)

Feche esta janela de logon e retornar ao portal do Azure.

Navegue até o painel para seu banco de dados. Clique em **gerenciar endereços IP permitido**.

![gerenciar endereços ip](publish-to-azure/_static/image14.png)

Agora você deve adicionar o endereço IP da mensagem de erro. Altere o intervalo de endereços IP permitidos para incluir uma na mensagem de erro, ou adicionar esse endereço IP como uma entrada separada.

![Adicionar novo endereço](publish-to-azure/_static/image15.png)

Salve a alteração de endereços IP permitidos.

Clique em gerenciar e tente fazer logon novamente para o banco de dados. Talvez seja necessário aguardar alguns minutos antes dos endereços IP permitidos estão configurados corretamente para o firewall. Quando você pode fazer logon com êxito no banco de dados, você configurou sua conexão ao banco de dados.

Você pode deixar esta janela de gerenciamento aberta porque você verificará o resultado da implantação do banco de dados em breve.

Retornar ao seu projeto de banco de dados. Clique com o botão direito e selecione **publicar**.

![publicar o banco de dados](publish-to-azure/_static/image16.png)

Na janela do banco de dados de publicação, selecione **editar**.

![editar](publish-to-azure/_static/image17.png)

Forneça o nome do servidor de banco de dados e suas credenciais de autenticação para o servidor. Depois de fornecer as credenciais, selecione o banco de dados que você criou na lista de bancos de dados disponíveis. Por padrão, o Visual Studio define o nome do campo de banco de dados para o nome do projeto que pode não ser o mesmo que o banco de dados que você criou.

![](publish-to-azure/_static/image18.png)

Clique em OK.

Você provavelmente desejará salvar esse perfil, você pode publicar as atualizações no futuro sem inserir novamente todas as informações de conexão. Selecione **criar perfil**.

![Salvar perfil](publish-to-azure/_static/image19.png)

Ele criará um arquivo no seu projeto chamado **ContosoUniversityData.publish.xml**. Na próxima vez em que você deseja publicar o banco de dados no Azure, basta carrega perfil.

Agora, clique em **publicar** para criar o banco de dados no Azure.

![publicar](publish-to-azure/_static/image20.png)

Após ser executado por um tempo, a publicação de resultados é exibido.

![resultados](publish-to-azure/_static/image21.png)

Agora, você pode voltar ao portal de gerenciamento para seu banco de dados. Atualizar a exibição de design e observe que as 3 tabelas com dados previamente preenchidos foram implantadas.

![novas tabelas](publish-to-azure/_static/image22.png)

Agora você está pronto para testar o aplicativo web que é implantado no Azure. Navegue até o aplicativo web no Azure (como http://contosositeexample.azurewebsites.net/). Clique no link para a lista de alunos e você deverá ver a exibição do índice para estudantes.

![Modo de exibição](publish-to-azure/_static/image23.png)

Ocasionalmente, o banco de dados e a conexão precisam de algum tempo para ser configurado corretamente. Se você receber um erro, aguarde alguns minutos e tente novamente.

## <a name="conclusion"></a>Conclusão

Esta série fornecido um exemplo simples de como gerar código a partir de um banco de dados que permite aos usuários editar, atualizar, criar e excluir dados. Ele usado ASP.NET MVC 5, o Entity Framework e o ASP.NET Scaffolding para criar o projeto.

Para obter um exemplo de Introdução do desenvolvimento do Code First, consulte [guia de Introdução ao ASP.NET MVC 5](../introduction/getting-started.md).

Para obter um exemplo mais avançado, consulte [criando um modelo de dados do Entity Framework para um aplicativo do ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Observe que a API DbContext que você usa para trabalhar com dados no primeiro banco de dados é o mesmo que a API que você pode usar para trabalhar com dados no Code First. Mesmo se você pretende usar o primeiro banco de dados, você pode aprender a lidar com cenários mais complexos, como ler e atualizar dados relacionados, controlando conflitos de simultaneidade, e assim por diante de um tutorial Code First. A única diferença está em como o banco de dados, à classe de contexto e classes de entidade são criados.

>[!div class="step-by-step"]
[Anterior](enhancing-data-validation.md)
