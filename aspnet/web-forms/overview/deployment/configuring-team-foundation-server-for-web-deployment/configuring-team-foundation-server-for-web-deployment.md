---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "Configurando o Team Foundation Server para a implantação da Web | Microsoft Docs"
author: jrjlee
description: "Este tutorial mostra como configurar o Team Foundation Server (TFS) 2010 para criar soluções e implantar o conteúdo da web em vários ambientes de destino. Isso..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Configurando o Team Foundation Server para a implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostra como configurar o Team Foundation Server (TFS) 2010 para criar soluções e implantar o conteúdo da web em vários ambientes de destino. Isso inclui cenários de integração contínua (CI), onde você implanta conteúdo automaticamente toda vez que um desenvolvedor fizer uma alteração. Ele também pode incluir os cenários de gatilho manual, onde um administrador pode querer disparar a implantação de uma compilação específica para um ambiente de preparo quando a compilação foi verificada e validada no ambiente de teste. Os tópicos neste tutorial orientará o processo de configuração inteira, incluindo:
> 
> - Como criar um novo projeto de equipe no TFS.
> - Como adicionar conteúdo ao controle de origem.
> - Como configurar um servidor de compilação para dar suporte à implantação e IC.
> - Como criar uma definição de compilação que inclui a lógica de implantação.
> - Como configurar permissões para a implantação automática.
> 
> Para obter uma tradução italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Este tutorial presume que você instalou o TFS 2010 e criou uma coleção de projeto de equipe como parte do processo de configuração inicial. O [guia de instalação do Team Foundation para Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) fornece uma orientação abrangente sobre essas tarefas.

## <a name="context"></a>Contexto

Isso faz parte de uma série de tutoriais com base nos requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) #x 2014; & solução para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos & #x 2014; projeto contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="scenario-overview"></a>Visão geral do cenário

O cenário de alto nível para esses tutoriais é descrito em [implantação de Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). É recomendável que você examine este tópico antes de começar este tutorial.

## <a name="how-to-use-this-tutorial"></a>Como usar esse tutorial

Se esta for a primeira vez que você realizou as tarefas descritas neste tutorial, ou se você desejar acompanhar os exemplos usando a solução de exemplo, você deve trabalhar com os tópicos de tutorial em ordem. Como alternativa, você pode usar os tópicos individuais como orientação para tarefas específicas. Este tutorial inclui os seguintes tópicos:

- [Criando um projeto de equipe no TFS](creating-a-team-project-in-tfs.md). Um projeto de equipe é a unidade principal para o controle de origem, gerenciamento de processos e compilação do TFS. Você precisa criar um projeto de equipe antes de adicionar conteúdo ao controle de origem ou criar definições de compilação.
- [Adicionar conteúdo ao controle do código-fonte](adding-content-to-source-control.md). Depois de criar um projeto de equipe, você pode começar a adicionar conteúdo ao controle de origem. Você precisará adicionar seus projetos e soluções, juntamente com quaisquer dependências externas, antes de começar a configurar compilações.
- [Configurando um TFS criar servidor de implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md). Se você deseja criar o conteúdo do projeto de equipe, você precisará configurar um servidor de compilação. Na maioria dos casos, isso deve ocorrer em um computador separado da sua instalação do TFS. Para configurar um servidor de compilação, você precisa instalar e configurar o serviço de compilação do TFS, instalar o Visual Studio 2010, criar controladores de compilação e agentes de compilação, instale quaisquer produtos ou componentes que seu código precisa para criar com êxito e instalar o Ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web).
- [Criando uma definição de compilação que dá suporte à implantação](creating-a-build-definition-that-supports-deployment.md). Antes de iniciar o enfileiramento de mensagens ou o disparo compilações no TFS, você precisa criar a definição de pelo menos uma compilação para seu projeto de equipe. A definição de compilação define todos os aspectos da compilação, incluindo quais itens devem ser incluídos na compilação, o que deve disparar a compilação e onde o Team Build deve enviar as saídas de compilação. Você pode configurar uma definição de compilação para executar arquivos de projeto Microsoft Build Engine (MSBuild) personalizados, que permite que você inclua lógica de implantação em compilações automatizadas.
- [Implantando uma determinada compilação](deploying-a-specific-build.md). Em muitos cenários, você vai querer implantar uma compilação específica, em vez da compilação mais recente, em um ambiente de destino. Nesse caso, você pode configurar uma definição de compilação que implanta o conteúdo de uma pasta de armazenamento específico.
- [Configurar permissões para a equipe de implantação da compilação](configuring-permissions-for-team-build-deployment.md). Se o serviço de compilação é implantar o conteúdo como parte de um processo de compilação automatizado, você precisa conceder várias permissões para a conta de serviço de compilação em todos servidores da web de destino e de servidores de banco de dados.

## <a name="key-technologies"></a>Tecnologias de chave

Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte a compilação automatizada e implantação da web:

- Visual Studio Team Foundation Server 2010
- Compilação de equipe e o MSBuild
- Implantação da Web

O tutorial também aborda o uso do ASP.NET MVC 3, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 e Windows Server 2008 R2.

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais nesta série

Isso faz parte de uma série de cinco tutoriais na implantação da web de grande porte. Estes são os outros tutoriais na série:

- [Implantando aplicativos da Web em cenários corporativos](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Descreve o cenário do tutorial e ilustra como as tarefas e descrita em toda a série explicações passo a passo se encaixam um processo de gerenciamento de ciclo de vida do aplicativo (ALM) mais amplo.
- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual para arquivos de projeto do MSBuild, o Pipeline de publicação de Web (WPP), Web Deploy e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas em conjunto para gerenciar processos complexos de implantação.
- [Configurando ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar os servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web e implantação de banco de dados remoto. Fornece orientação sobre como escolher o método de implantação apropriadas para seu próprio ambiente, e descreve como usar o Web Farm Framework (WFF) para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Enterprise Web implantação avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar as implantações de banco de dados para vários ambientes, excluindo arquivos e pastas de implantação e que os aplicativos de web offline durante o processo de implantação .

>[!div class="step-by-step"]
[Avançar](creating-a-team-project-in-tfs.md)
