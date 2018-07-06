---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurando um TFS servidor de compilação para implantação da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como preparar um servidor de compilação do Team Foundation Server (TFS) para compilar e implantar suas soluções usando o Team Build e a Internet Informat...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7f57ad0392a068964bb910fbbaafea105fdbb3d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841335"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurando um servidor de compilação do TFS para implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como preparar um servidor de compilação do Team Foundation Server (TFS) para compilar e implantar suas soluções usando o Team Build e a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web).


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Para preparar um servidor de compilação para compilar e implantar suas soluções, você precisará:

- Instalar e configurar o serviço de compilação do TFS.
- Instale o Visual Studio 2010.
- Instale quaisquer produtos ou componentes que são necessários para compilar sua solução, como nas versões do .NET Framework ou do ASP.NET MVC.
- Instale a implantação da Web 2.0 ou posterior.

Este tópico mostra como executar esses procedimentos ou apontar para outros recursos em que elas existem. As tarefas e instruções passo a passo neste tópico pressupõem que:

- Você está começando com uma compilação limpa do servidor executando o Windows Server 2008 R2 Service Pack 1.
- O servidor está ingressado no domínio com um endereço IP estático.
- Você instalou a camada de aplicativo do TFS em um servidor separado, conforme descrito em [implantação da Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Na maioria dos casos, um administrador do TFS será responsável por configurar servidores de compilação. Em alguns casos, a equipe de desenvolvedores pode assumir a propriedade dos servidores de compilação específica.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalar e configurar o serviço de compilação do TFS

Quando você configura um servidor de compilação, a primeira tarefa é instalar e configurar o serviço de compilação do TFS. Como parte desse processo, você precisará:

- Instalar o serviço de compilação do TFS e configurar uma conta de serviço. Tarefas de build, incluindo a implantação, serão executado usando a identidade da conta de serviço de compilação.
- Criar uma *controlador de compilação* e um ou mais *agentes de compilação*. Cada controlador de compilação gerencia um conjunto de agentes de compilação. Quando você enfileira uma compilação, o controlador de compilação atribui a tarefa de build para um agente de compilação disponíveis. Cada coleção de projetos de equipe no TFS é mapeada para um controlador de compilação único.
- Configure uma pasta-depósito para as saídas de compilação. Este é um compartilhamento de rede. Qualquer compilação saídas, como pacotes de implantação da web, são enviadas para a pasta-depósito.

O [administrando o Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) capítulo no MSDN contém todos os recursos que você precisa para executar estas tarefas:

- Para obter uma visão geral conceitual do Team Foundation Build, incluindo o serviço de compilação, controladores de compilação e agentes de compilação, consulte [Noções básicas sobre um sistema de compilação do Team Foundation](https://msdn.microsoft.com/library/dd793166.aspx).
- Para obter informações sobre como instalar e configurar o serviço de compilação, consulte [configurar um computador de compilação](https://msdn.microsoft.com/library/ms181712.aspx).
- Para obter informações sobre a criação de controladores de compilação, consulte [criar e trabalhar com um controlador de compilação](https://msdn.microsoft.com/library/ee330987.aspx).
- Para obter informações sobre como criar agentes de compilação, consulte [criar e trabalhar com agentes de compilação](https://msdn.microsoft.com/library/bb399135.aspx).
- Para obter informações sobre como criar e configurar pastas-depósito, consulte [definir pastas de Drop](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalar produtos necessários e componentes

Para habilitar o servidor de compilação Compilar suas soluções, você deve instalar qualquer produtos, componentes ou assemblies que sua solução exige. Antes de instalar quaisquer componentes de plataforma da web, você deve instalar o Visual Studio 2010 (qualquer versão) no servidor de compilação. Isso garante que os arquivos de destino principais Microsoft Build Engine (MSBuild) e os arquivos de destino do Pipeline de publicação (WPP) de Web estão disponíveis para o serviço de compilação. O instalador do Visual Studio também deve instalar a implantação da Web, será necessário se você planeja implantar os pacotes da web como parte do processo de compilação.

A melhor maneira de instalar componentes de plataforma da web comum é usar o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118). Isso garante que você estiver instalando a versão mais recente de cada produto, e ele detecta automaticamente e instala todos os pré-requisitos para cada produto. No caso do [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução, você deve usar o Web Platform Installer para instalar esses produtos e componentes:

- **.NET Framework 4.0**. Isso é necessário para executar aplicativos que foram criados com esta versão do .NET Framework.
- **Web Deployment Tool 2.1 ou posterior**. Isso instala a implantação da Web (e seu arquivo executável subjacente, MSDeploy.exe) em seu servidor. Como parte desse processo, ele instala e inicia o serviço de agente de implantação da Web. Esse serviço permite implantar pacotes da web de um computador remoto.
- **ASP.NET MVC 3**. Isso instala os assemblies que você precisa para executar aplicativos ASP.NET MVC 3.

**Para instalar os componentes e produtos necessários**

1. Instale o Visual Studio 2010. Quando solicitado a selecionar os recursos a serem instalados, você deve incluir:

    1. Qualquer linguagem de programação que você precisa para compilar.
    2. Visual Web Developer. Isso garante que os destinos WPP são adicionados ao seu servidor de compilação.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Quando a instalação do Visual Studio 2010 for concluída, baixe e instale [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (se ainda não estiver incluído na mídia de instalação).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 resolve um bug que pode impedir a localizar o MSDeploy executável do MSBuild.
3. Baixe e inicie o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
4. Na parte superior do **Web Platform Installer 3.0** janela, clique em **produtos**.
5. No lado esquerdo da janela, no painel de navegação, clique em **estruturas**.
6. No **Microsoft .NET Framework 4** linha, se o .NET Framework já não estiver instalado, clique em **Add**.

    > [!NOTE]
    > Você já pode ter instalado o .NET Framework 4.0 por meio do Windows Update. Se um produto ou componente já estiver instalado, o Web Platform Installer indicará isso substituindo o **Add** botão com o texto **instalado**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. No **ASP.NET MVC 3 (Visual Studio 2010)** de linhas, clique em **Add**.
8. No painel de navegação, clique em **Server**.
9. No **ferramenta de implantação da Web 2.1** de linhas, clique em **Add**.
10. Clique em **Instalar**. O Web Platform Installer mostrará uma lista de produtos&#x2014;junto com quaisquer dependências associadas&#x2014;a serem instalados e solicitará que você aceite os termos de licença.
11. Examine os termos de licença e se você concordar com os termos, clique em **aceito**.
12. Quando a instalação for concluída, clique em **terminar**e, em seguida, feche o **Web Platform Installer 3.0** janela.

> [!NOTE]
> Se seu processo de implantação inclui o uso de ferramentas como VSDBCMD.exe ou SQLCMD.exe, você precisará garantir que eles são instalados no seu servidor de compilação. VSDBCMD.exe é uma ferramenta do Visual Studio e normalmente é adicionado ao servidor quando você instala o Team Foundation Build. SQLCMD.exe é uma ferramenta do SQL Server. Você pode baixar uma versão autônoma do SQLCMD.exe do [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) página.


## <a name="conclusion"></a>Conclusão

Neste ponto, seu servidor de compilação está pronto para começar a criar e implantar seus projetos de aplicativo web. Próximo tópico [criação de uma implantação de dá suporte a que definição de compilação](creating-a-build-definition-that-supports-deployment.md), descreve como criar e configurar uma definição de compilação para controlar quando e como seus projetos são criados e implantados.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação geral sobre como trabalhar com o Team Build, consulte [administrando o Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Anterior](adding-content-to-source-control.md)
> [Próximo](creating-a-build-definition-that-supports-deployment.md)
