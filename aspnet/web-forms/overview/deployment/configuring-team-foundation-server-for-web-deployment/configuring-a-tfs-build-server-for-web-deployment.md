---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurando um TFS criar servidor de implantação da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como preparar um servidor de compilação do Team Foundation Server (TFS) para criar e implantar suas soluções usando o Team Build e a Internet Informat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b3130ca7d36ffec457e1871fa62c1077b5e3174
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurando um servidor de compilação do TFS para implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como preparar um servidor de compilação do Team Foundation Server (TFS) para criar e implantar suas soluções usando o Team Build e a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web).


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Para preparar um servidor de compilação para criar e implantar suas soluções, você precisará:

- Instalar e configurar o serviço de compilação do TFS.
- Instale o Visual Studio 2010.
- Instale quaisquer produtos ou componentes que são necessárias para criar sua solução, como nas versões do .NET Framework ou do ASP.NET MVC.
- Instale a implantação da Web 2.0 ou posterior.

Neste tópico mostram como executar esses procedimentos ou apontar para outros recursos em que elas existem. As tarefas e instruções passo a passo neste tópico supõem que:

- Você está iniciando com uma compilação limpa de servidor executando o Windows Server 2008 R2 Service Pack 1.
- O servidor está ingressado no domínio com um endereço IP estático.
- Você instalou a camada de aplicativo do TFS em um servidor separado, conforme descrito em [implantação de Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Na maioria dos casos, um administrador do TFS será responsável por configurar servidores de compilação. Em alguns casos, a equipe de desenvolvedores pode apropriar-se dos servidores de compilação específica.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalar e configurar o serviço de compilação do TFS

Quando você configura um servidor de compilação, a primeira tarefa é instalar e configurar o serviço de compilação do TFS. Como parte desse processo, você precisará:

- Instale o serviço de compilação do TFS e configure uma conta de serviço. As tarefas de compilação, incluindo a implantação, serão executado usando a identidade da conta de serviço de compilação.
- Criar um *controlador de compilação* e um ou mais *agentes de compilação*. Cada controlador de compilação gerencia um conjunto de agentes de compilação. Quando você enfileirar uma compilação, o controlador de compilação atribui a tarefa de compilação para um agente de compilação disponível. Cada coleção de projetos de equipe no TFS é mapeada para um controlador de compilação único.
- Configure uma pasta-depósito para suas saídas de compilação. Este é um compartilhamento de rede. Qualquer saídas, como pacotes de implantação da web de compilação, são enviadas para a pasta-depósito.

O [administrando o Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) capítulo no MSDN contém todos os recursos que você precisa para executar estas tarefas:

- Para obter uma visão geral conceitual do Team Foundation Build, incluindo o serviço de compilação, compilação controladores e agentes de compilação, consulte [Noções básicas sobre um sistema de compilação do Team Foundation](https://msdn.microsoft.com/library/dd793166.aspx).
- Para obter informações sobre como instalar e configurar o serviço de compilação, consulte [configurar uma máquina de compilação](https://msdn.microsoft.com/library/ms181712.aspx).
- Para obter informações sobre a criação de controladores de compilação, consulte [criar e trabalhar com um controlador de compilação](https://msdn.microsoft.com/library/ee330987.aspx).
- Para obter informações sobre a criação de agentes de compilação, consulte [criar e trabalhar com os agentes de compilação](https://msdn.microsoft.com/library/bb399135.aspx).
- Para obter informações sobre como criar e configurar pastas depósitos, consulte [definir pastas de Drop](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalar produtos necessários e componentes

Para habilitar o servidor de compilação criar soluções, você deve instalar qualquer produtos, componentes ou assemblies que sua solução exige. Antes de instalar os componentes de plataforma da web, você deve instalar o Visual Studio 2010 (qualquer versão) no servidor de compilação. Isso garante que os principais arquivos de destino Microsoft Build Engine (MSBuild) e os arquivos de destino do Pipeline de publicação (WPP) de Web estão disponíveis para o serviço de compilação. O instalador do Visual Studio também deve instalar a implantação da Web, você precisará se você planeja implantar pacotes da web como parte do processo de compilação.

A melhor maneira de instalar componentes de plataforma da web comum é usar o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118). Isso garante que você está instalando a versão mais recente de cada produto, e também automaticamente detecta e instala todos os pré-requisitos para cada produto. No caso do [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução, você deve usar o Web Platform Installer para instalar esses produtos e componentes:

- **.NET Framework 4.0**. Isso é necessário para executar aplicativos que foram criados com esta versão do .NET Framework.
- **Web Deployment Tool 2.1 ou posterior**. Isso instala a implantação da Web (e seu executável subjacente, MSDeploy.exe) em seu servidor. Como parte desse processo, ele instala e inicia o serviço de agente de implantação da Web. Esse serviço permite que você implante pacotes da web de um computador remoto.
- **ASP.NET MVC 3**. Isso instala os assemblies que você precisa executar aplicativos ASP.NET MVC 3.

**Para instalar os componentes e produtos necessários**

1. Instale o Visual Studio 2010. Quando solicitado a selecionar os recursos a serem instalados, você deve incluir:

    1. Linguagens de programação que você precisa compilar.
    2. Visual Web Developer. Isso garante que os destinos WPP são adicionados ao seu servidor de compilação.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Quando a instalação do Visual Studio 2010 for concluída, baixe e instale [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (se ainda não estiver incluído na mídia de instalação).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 resolve um erro que pode impedir a localizar o executável do MSDeploy do MSBuild.
3. Baixe e inicie o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
4. Na parte superior do **Web Platform Installer 3.0** janela, clique em **produtos**.
5. No lado esquerdo da janela, no painel de navegação, clique em **estruturas**.
6. No **Microsoft .NET Framework 4** de linha, se o .NET Framework já não estiver instalado, clique em **adicionar**.

    > [!NOTE]
    > Você já pode ter instalado o .NET Framework 4.0 através do Windows Update. Se um produto ou componente já está instalado, o Web Platform Installer indicará isso substituindo o **adicionar** botão com o texto **instalado**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. No **ASP.NET MVC 3 (Visual Studio 2010)** de linha, clique em **adicionar**.
8. No painel de navegação, clique em **Server**.
9. No **ferramenta de implantação da Web 2.1** de linha, clique em **adicionar**.
10. Clique em **Instalar**. O Web Platform Installer mostrará uma lista de produtos&#x2014;juntamente com quaisquer dependências associadas&#x2014;a serem instalados e solicitará que você aceite os termos de licença.
11. Leia os termos de licença e se você concordar com os termos, clique em **aceito**.
12. Quando a instalação for concluída, clique em **concluir**e, em seguida, feche o **Web Platform Installer 3.0** janela.

> [!NOTE]
> Se o processo de implantação inclui o uso de ferramentas como VSDBCMD.exe ou SQLCMD.exe, você precisará garantir que eles são instalados no seu servidor de compilação. VSDBCMD.exe é uma ferramenta do Visual Studio e normalmente é adicionada ao servidor quando você instalar o Team Foundation Build. SQLCMD.exe é uma ferramenta do SQL Server. Você pode baixar uma versão autônoma do SQLCMD.exe do [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) página.


## <a name="conclusion"></a>Conclusão

Neste ponto, seu servidor de compilação está pronto para começar a criar e implantar seus projetos de aplicativo da web. O próximo tópico, [criar uma implantação de dá suporte a que definição de compilação](creating-a-build-definition-that-supports-deployment.md), descreve como criar e configurar uma definição de compilação para controlar quando e como os projetos são criados e implantados.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação geral sobre como trabalhar com o Team Build, consulte [administrando o Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Anterior](adding-content-to-source-control.md)
> [Próximo](creating-a-build-definition-that-supports-deployment.md)
