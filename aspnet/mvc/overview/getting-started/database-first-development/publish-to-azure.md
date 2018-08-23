---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publicar o site MVC banco de dados primeiro no Azure | Microsoft Docs
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 45dd2c127e3ba0644e8168e293006fa9eadd776d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831050"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publicar o site Database First do MVC para o Azure
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra em publicar o aplicativo web e o banco de dados no Azure. Você pode ler este tópico para saber mais sobre como publicar um aplicativo web e o banco de dados, mas, na verdade, executar as etapas que você deve começar do início do tutorial. Ver [Introdução ao](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Implantar seu aplicativo web no Azure

Você precisa de uma conta do Azure para concluir este tutorial:

- Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
- Você pode [ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.

Para publicar seu aplicativo web, clique com botão direito no projeto e selecione **publicar**.

![Publicar site](publish-to-azure/_static/image1.png)

Selecione os sites do Microsoft Azure.

![Selecione o Azure](publish-to-azure/_static/image2.png)

Se você não estiver conectado ao Azure, forneça suas credenciais de conta do Azure. Em seguida, selecione Novo para criar um novo aplicativo web.

![novo site](publish-to-azure/_static/image3.png)

Crie um nome exclusivo para seu aplicativo web. Você saberá que o nome é exclusivo se você vir uma marca de seleção verde à direita do campo nome. Selecione uma região para seu aplicativo web. Selecione **criar novo servidor** para o banco de dados e forneça um nome de usuário e senha para esse novo servidor de banco de dados. Quando terminar, clique em **criar**.

![Criar site](publish-to-azure/_static/image4.png)

Os valores de conexão estão agora pronto. Você pode deixar esses valores inalterados.

![conexão](publish-to-azure/_static/image5.png)

Clique em **Avançar**.

Para configurações, observe que dois banco de dados as conexões são especificados - ApplicationDBContext e ContosoUniversityDataEntities. ApplicationDBContext é a conexão de tabelas de conta de usuário. Esses valores mostram apenas as cadeias de caracteres de conexão para bancos de dados. Isso não significa que esses bancos de dados serão publicados quando você publica seu site. Depois de concluir a publicação do aplicativo web, você publicará seu projeto de banco de dados.

![](publish-to-azure/_static/image6.png)

As reticências (...) ao lado de conexão de banco de dados mostra os detalhes da cadeia de caracteres de conexão. Clique no botão de reticências ao lado de ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Anote o nome do servidor de banco de dados e o banco de dados. O nome do servidor é gerado aleatoriamente. O nome do banco de dados é simple o nome do seu site com  **\_db** acrescentado. Você precisará mais tarde os dois nomes ao publicar seu banco de dados.

Clique em **Okey** para fechar a janela de cadeia de caracteres de conexão de banco de dados.

Na janela Publicar Web, clique em **próxima** para exibir a visualização.

![](publish-to-azure/_static/image8.png)

Você pode clicar em Iniciar visualização para ver uma lista dos arquivos a publicar. Como essa é a primeira vez que você publicou este site, a lista é todos os arquivos relevantes no projeto.

Clique em **Publicar**.

O painel de saída exibirá o resultado da publicação.

![](publish-to-azure/_static/image9.png)

Após a publicação, o site é immedialely iniciado em um navegador da web. O site tiver sido implantado e você pode registrar um novo usuário para o site; No entanto, suas tabelas no projeto ContosoUniversityData ainda não foram publicadas. Se você clicar na lista de link de alunos, você receberá um erro.

## <a name="publish-database-to-sql-azure"></a>Publicar banco de dados do SQL Azure

Antes de publicar seu banco de dados, você deve verificar se o que computador local pode se conectar ao servidor de banco de dados. O firewall para o servidor de banco de dados restringe quais computadores poderão se conectar ao banco de dados. Você precisará adicionar o endereço IP do seu computador para endereços IP para o firewall.

Faça logon em sua conta do Azure por meio do portal do Azure.

Selecione o novo banco de dados e selecione **gerenciar**.

![Gerenciar](publish-to-azure/_static/image10.png)

Você deve configurar seu servidor de banco de dados para permitir conexões do seu computador. Quando você seleciona a gerenciar, você precisará adicionar o endereço IP atual, conforme permitido para o servidor de banco de dados. Selecione Sim.

![Adicionar endereço ip](publish-to-azure/_static/image11.png)

Há uma chance de que o endereço IP que você adicionou na etapa anterior não é o único endereço IP que você precisa configurar para conexões. Você pode tentar fazer logon no banco de dados para ver se as conexões foram corretamente configuradas. Forneça o usuário e senha que você criou anteriormente.

![Logon](publish-to-azure/_static/image12.png)

Se você receber uma mensagem de erro, você precisará adicionar outro endereço IP. Clique na mensagem de erro para ver mais detalhes sobre o erro. Os detalhes, você verá o endereço IP que você precisa adicionar. Observe que esse endereço IP.

![não permitido](publish-to-azure/_static/image13.png)

Fechar esta janela de logon e retornar ao portal do Azure.

Navegue até o painel do banco de dados. Clique em **gerenciar endereços IP permitidos**.

![gerenciar endereços ip](publish-to-azure/_static/image14.png)

Agora você deve adicionar o endereço IP da mensagem de erro. Se alterar o intervalo de endereços IP permitidos para incluir o item da mensagem de erro ou adicione esse endereço IP como uma entrada separada.

![Adicionar novo endereço](publish-to-azure/_static/image15.png)

Salve a alteração de endereços IP permitidos.

Clique em gerenciar e tente fazer logon novamente no banco de dados. Você precisará aguardar alguns minutos antes que os endereços IP permitidos estão configurados corretamente para o firewall. Quando você pode fazer logon com êxito no banco de dados, você terminar de configurar sua conexão ao banco de dados.

Você pode deixar a janela de gerenciamento aberta porque você verificará o resultado da sua implantação de banco de dados em breve.

Retornar ao seu projeto de banco de dados. Clique com botão direito no projeto e selecione **publicar**.

![Publicar banco de dados](publish-to-azure/_static/image16.png)

Na janela Publicar banco de dados, selecione **editar**.

![editar](publish-to-azure/_static/image17.png)

Forneça o nome do servidor de banco de dados e suas credenciais de autenticação para o servidor. Depois de fornecer as credenciais, selecione o banco de dados que você criou na lista de bancos de dados disponíveis. Por padrão, o Visual Studio define o nome do campo de banco de dados para o nome do seu projeto que pode não ser o mesmo que o banco de dados que você criou.

![](publish-to-azure/_static/image18.png)

Clique em OK.

Você provavelmente desejará salvar esse perfil para que você possa publicar atualizações no futuro sem precisar reinserir todas as informações de conexão. Selecione **Criar Perfil**.

![Salvar perfil](publish-to-azure/_static/image19.png)

Ele criará um arquivo em seu projeto chamado **ContosoUniversityData.publish.xml**. Na próxima vez em que você deseja publicar o banco de dados no Azure, basta carrega perfil.

Agora, clique em **publicar** para criar o banco de dados no Azure.

![publicar](publish-to-azure/_static/image20.png)

Depois da execução por algum tempo, a publicação de resultados é exibido.

![resultados](publish-to-azure/_static/image21.png)

Agora, você pode voltar ao portal de gerenciamento do banco de dados. Atualizar a exibição de design e observe que as 3 tabelas com dados previamente preenchidos foram implantadas.

![novas tabelas](publish-to-azure/_static/image22.png)

Agora você está pronto para testar o aplicativo web que é implantado no Azure. Navegue até o aplicativo web no Azure (como http://contosositeexample.azurewebsites.net/). Clique no link para a lista de alunos e você deverá ver a exibição de índice para alunos.

![modo de exibição](publish-to-azure/_static/image23.png)

Ocasionalmente, o banco de dados e a conexão precisam de algum tempo para ser configurado corretamente. Se você receber um erro, aguarde alguns minutos e, em seguida, tente novamente.

## <a name="conclusion"></a>Conclusão

Esta série fornecido um exemplo simples de como gerar o código de um banco de dados existente que permite aos usuários editar, atualizar, criar e excluir dados. Ele usado ASP.NET MVC 5, Entity Framework e Scaffolding do ASP.NET para criar o projeto.

Para obter um exemplo introdutório de desenvolvimento Code First, consulte [Introdução ao ASP.NET MVC 5](../introduction/getting-started.md).

Para obter um exemplo mais avançado, consulte [criando um modelo de dados do Entity Framework para um aplicativo do ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Observe que a API DbContext que você usa para trabalhar com dados no banco de dados primeiro é o mesmo que a API que você pode usar para trabalhar com dados no Code First. Mesmo se você pretende usar o primeiro banco de dados, você pode aprender como lidar com cenários mais complexos, como ler e atualizar dados relacionados, tratamento de conflitos de simultaneidade, e assim por diante de um tutorial de Code First. A única diferença está em como o banco de dados, classe de contexto e classes de entidade são criadas.

> [!div class="step-by-step"]
> [Anterior](enhancing-data-validation.md)
