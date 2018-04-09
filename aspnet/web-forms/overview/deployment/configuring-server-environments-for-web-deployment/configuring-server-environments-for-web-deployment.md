---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurando ambientes de servidor para a implantação da Web | Microsoft Docs
author: jrjlee
description: Este tutorial mostra como configurar ambientes de servidor para suporte de um clique ou automatizado, implantação de site da Web e publicação em vários cenário diferente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a>Configurando ambientes de servidor para a implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostrará a você como configurar ambientes de servidor para suporte de um clique ou automatizado, implantação de site da Web e publicação em vários cenários diferentes. O tutorial inclui tópicos para orientá-lo a concluir várias tarefas, como configurar um servidor web para dar suporte a abordagens específicas para implantação e configuração de um farm de servidores de Web Farm Framework (WFF), junto com a visão de geral baseada em cenário que fornecem orientação de ponta a ponta de nível superior.
> 
> O tutorial usa o cenário de implantação da Fabrikam, Inc. descrito em [implantação de Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) como um ponto de referência para obter exemplos e infraestrutura de rede.
> 
> Para obter uma tradução italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Este tutorial inclui os seguintes tópicos:

- [Escolha da abordagem correta para a Implantação da Web](choosing-the-right-approach-to-web-deployment.md)
- [Cenário: configuração de um ambiente de teste para a Implantação da Web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Cenário: configuração de um ambiente de preparo para a Implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Cenário: configuração de um ambiente de produção para a Implantação da Web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configuração de um servidor Web para publicação de Implantação da Web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configuração de um servidor Web para publicação de Implantação da Web (manipulador de Implantação da Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configuração de um servidor Web para publicação de Implantação da Web (implantação offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configuração de um servidor de banco de dados para publicação de Implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md)
- [Criação de um farm de servidores com o Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configuração de propriedades de implantação para um ambiente de destino](configuring-deployment-properties-for-a-target-environment.md)

O primeiro tópico, [optar pela abordagem da direita para a implantação da Web](choosing-the-right-approach-to-web-deployment.md), descreve as abordagens principais que você pode usar para publicar aplicativos web usando a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) 2.0. Ele também identifica os cenários que são mapeados para cada abordagem. A partir daqui, cada tópico de cenário fornece uma visão geral das tarefas que precisam ser concluídas e identifica os tópicos que você precisará trabalhar com para ajudá-lo a concluir essas tarefas.

Se você estiver usando a abordagem de arquivo de projeto de divisão descrita em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md) para criar e implantar sua solução, o tópico final, [configurando propriedades de implantação de um ambiente de destino](configuring-deployment-properties-for-a-target-environment.md), descreve como configurar arquivos de projeto de específico do ambiente de implantação para ambientes de destino diferente.

## <a name="key-technologies"></a>Tecnologias de chave

Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte à implantação da web:

- IIS 7.5
- 2. x de implantação da Web
- WFF 2.x
- Serviço de gerenciamento do IIS da Web (WMSvc)

O tutorial também aborda o uso do Windows Server 2008 R2, o SQL Server 2008 R2, o ASP.NET 4.0 e o ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais nesta série

Isso faz parte de uma série de cinco tutoriais na implantação da web de grande porte. Estes são os outros tutoriais na série:

- [Implantando aplicativos da Web em cenários corporativos](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Descreve o cenário do tutorial e ilustra como as tarefas e descrita em toda a série explicações passo a passo se encaixam um processo de gerenciamento de ciclo de vida do aplicativo (ALM) mais amplo.
- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual para arquivos de projeto do Microsoft Build Engine (MSBuild), o Pipeline de publicação na Web, implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas em conjunto para gerenciar processos complexos de implantação.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o Team Foundation Server (TFS) para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de integração contínua (CI) e disparada manualmente as implantações de versões específicas.
- [Enterprise Web implantação avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar as implantações de banco de dados para vários ambientes, excluindo arquivos e pastas de implantação e que os aplicativos de web offline durante o processo de implantação .

> [!div class="step-by-step"]
> [Avançar](choosing-the-right-approach-to-web-deployment.md)
