---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configurar a solução de Gerenciador de contato | Microsoft Docs
author: jrjlee
description: Este tópico descreve como baixar e configurar a solução de Gerenciador de contato para ser executada localmente em uma estação de trabalho do desenvolvedor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="setting-up-the-contact-manager-solution"></a>Configurar a solução de Gerenciador de contato
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como baixar e configurar a solução de Gerenciador de contato para ser executada localmente em uma estação de trabalho do desenvolvedor.


## <a name="system-requirements"></a>Requisitos do sistema

Para executar a solução de Contact Manager localmente e executar outras tarefas descritas neste tutorial, você precisará instalar esse software em sua estação de trabalho do desenvolvedor:

- Visual Studio 2010 Service Pack 1, Premium ou Ultimate Edition
- Serviços de informações da Internet (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS Web ferramenta de implantação (implantação da Web) 2.1 ou posterior
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3,5 SP1

Com exceção do Visual Studio 2010, você pode baixar e instalar as versões mais recentes de todos esses produtos e componentes por meio de [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Baixe e extraia a solução

Você pode baixar o aplicativo de exemplo do Gerenciador de contato da Galeria de códigos do MSDN [aqui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configurar e executar a solução

Para configurar e executar a solução de Gerenciador de contato em seu computador local, você precisará executar essas etapas de alto nível:

1. Se você não tiver um já, crie um banco de dados de serviços de aplicativo ASP.NET local com os recursos de gerenciamento de associação e funções habilitados.
2. Editar cadeias de caracteres de conexão no *Web. config* arquivos para apontar para sua instância local do SQL Server Express.
3. Execute a solução do Visual Studio 2010.

O restante desta seção fornece mais orientações sobre como concluir cada uma dessas tarefas.

**Para criar o banco de dados de serviços de aplicativo**

1. Abra um prompt de comando Visual Studio 2010. Para fazer isso, no **iniciar** , aponte para **todos os programas**, clique em **Microsoft Visual Studio 2010**, clique em **ferramentas do Visual Studio**e, em seguida, Clique em **Prompt de comando do Visual Studio (2010)**.
2. No prompt de comando, digite o seguinte comando e pressione Enter:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Use o **– C** para especificar a cadeia de caracteres de conexão para o servidor de banco de dados.
    2. Use o **– um** para especificar o aplicativo de serviços de recursos que você deseja adicionar ao banco de dados. Nesse caso, **m** indica que você deseja adicionar suporte para o provedor de associação e **r** indica que você deseja adicionar suporte para o Gerenciador de função.
    3. Use o **– d** para especificar um nome para seu banco de dados de serviços de aplicativo. Se esta opção for omitida, o utilitário criará um banco de dados com o nome padrão do **aspnetdb**.
3. Quando o banco de dados foi criado com êxito, o prompt de comando mostrará uma confirmação.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Para obter mais informações sobre o aspnet\_regsql utilitário, consulte [ferramenta de registro de servidor de SQL do ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


A próxima etapa é certificar-se de que as cadeias de caracteres de conexão na solução entre em contato com o Gerenciador de ponto para sua instância local do SQL Server Express.

**Para atualizar as cadeias de caracteres de conexão**

1. Abra a solução de Gerenciador de contato no Visual Studio 2010.
2. No **Solution Explorer** janela, expanda o **ContactManager.Mvc** de projeto e, em seguida, clique duas vezes o **Web. config** nó.

    > [!NOTE]
    > O projeto ContactManager.Mvc inclui dois *Web. config* arquivos. Você precisa editar o arquivo de nível de projeto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. No **connectionStrings** elemento, verifique se que a cadeia de caracteres de conexão denominada **ApplicationServices** aponta para o banco de dados de serviços de aplicativo ASP.NET local.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. No **Solution Explorer** janela, expanda o **ContactManager.Service** de projeto e, em seguida, clique duas vezes o **Web. config** nó.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. No **connectionStrings** elemento na cadeia de conexão denominada **ContactManagerContext**, verifique o **fonte de dados** propriedade for definida como sua instância local do SQL Server Express. Você não precisa alterar nada na cadeia de conexão.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Salve todos os arquivos abertos.

Você deve agora estar pronto para executar a solução de Gerenciador de contato em seu computador local.

> [!NOTE]
> Se você seguir estas etapas sem primeiro criar um banco de dados de serviços de aplicativo, o ASP.NET criará o banco de dados na primeira vez que você tentar criar um usuário. No entanto, criar manualmente o banco de dados fornece muito mais controle sobre o conjunto de recursos de serviços de aplicativo que você deseja dar suporte.


**Para executar a solução de Gerenciador de contato**

1. No Visual Studio 2010, pressione F5.
2. Internet Explorer é iniciado e solicita a URL do aplicativo entre em contato com o Gerenciador ASP.NET MVC 3. Por padrão, o aplicativo exibe o **todos os contatos** página.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Adicione alguns contatos e, em seguida, verifique se que o aplicativo funciona conforme o esperado.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Navegue até `http://localhost:50114/Account/Register` (ajuste a URL se você estiver hospedando o aplicativo em uma porta diferente). Adicionar um nome de usuário, o endereço de email e a senha e confirmar que você é capaz de registrar uma conta com êxito.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Navegue até `http://localhost:50114/Account/LogOn` (ajuste a URL se você estiver hospedando o aplicativo em uma porta diferente). Verifique se que você é capaz de fazer logon usando a conta que você acabou de criar.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Feche o Internet Explorer para parar a depuração.

## <a name="conclusion"></a>Conclusão

Neste ponto, a solução de Gerenciador de contato deve ser totalmente configurada para ser executado em seu computador local. Você pode usar a solução como referência quando você trabalha com os outros tópicos neste tutorial.

O próximo tópico, [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), explica como você pode usar os arquivos do projeto Microsoft Build Engine (MSBuild) personalizados dentro da solução de Gerenciador de contato para controlar o processo de implantação.

> [!div class="step-by-step"]
> [Anterior](the-contact-manager-solution.md)
> [Próximo](understanding-the-project-file.md)
