---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Cenário: Configuração de um ambiente de teste para implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário de implantação da web para o desenvolvedor ou ambientes de teste e explica as tarefas que você precisa concluir para configurar um si...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3c6c8850e77f4a3cbb71db4353487c8a5e9c097d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832541"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Cenário: Configuração de um ambiente de teste para implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da web para o desenvolvedor ou ambientes de teste e explica as tarefas que você precisa concluir para configurar um ambiente semelhante.


Quando os desenvolvedores trabalham em aplicativos da web, geralmente recebem acesso para um ambiente de servidor que eles podem usar para testar as alterações em seus aplicativos em uma configuração realista. Normalmente, esse tipo de ambiente de desenvolvimento ou teste tem as seguintes características:

- O ambiente consiste em um único servidor web e um servidor de banco de dados individual.
- Os desenvolvedores geralmente têm privilégios de administrador nos servidores, para que ele configure o ambiente para os requisitos de seus aplicativos.
- Alterações em aplicativos são implantadas com frequência, portanto, o ambiente precisa dar suporte a única etapa ou implantação automatizada.

Por exemplo, no nosso [cenário do tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink é um desenvolvedor na Fabrikam, Inc. Matt está trabalhando para a solução de Gerenciador de contatos e regularmente precisa implantar as alterações em um ambiente de teste. Matt é um administrador no servidor web de teste e o servidor de banco de dados de teste. Inicialmente, Matt precisa ser capaz de implantar a solução ao ambiente de teste diretamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Conforme o trabalho progride e mais desenvolvedores Junte-se a equipe, Gerenciador de contatos solução está configurada para integração contínua (CI) no Team Foundation Server (TFS). Sempre que um desenvolvedor verifica no conteúdo, Team Build deve compilar a solução, execute os testes de unidade e implantar automaticamente a solução para o ambiente de teste.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Visão geral da solução

O ambiente de teste precisa dar suporte a única etapa ou a implantação automatizada de um computador remoto, portanto, você tem a opção de duas abordagens principais. Você pode:

- Configure o servidor web de teste para dar suporte à implantação usando o serviço de agente de implantação da Web (o "agente remoto").
- Configure o servidor web de teste para dar suporte à implantação usando o manipulador de implantação da Web.

> [!NOTE]
> Você também pode usar [implantar sob demanda Web](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (o "agente temp"). Isso é semelhante à abordagem do agente remoto em termos de requisitos e restrições.


Nesse caso, os desenvolvedores tenham privilégios de administrador nos servidores de destino e o ambiente de teste não está sujeita às restrições de segurança estrita, portanto, a escolha lógica é configurar o servidor web de teste para dar suporte à implantação usando o agente remoto. Isso é menos complexo e requer a configuração inicial menor que a abordagem do manipulador de implantação da Web. Você também precisará configurar seu servidor de banco de dados para dar suporte à implantação e acesso remoto.

Estes tópicos fornecem todas as informações necessárias para concluir essas tarefas:

- [Configurar um servidor Web para publicação (agente remoto) de implantação da Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Este tópico descreve como criar um servidor web que dá suporte à implantação da Web de publicação, usando a abordagem de agente remoto, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configurar um servidor de banco de dados para publicação de implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte a acesso remoto e a implantação, a partir de uma instalação padrão do SQL Server 2008 R2.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente típico de preparo, consulte [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente de produção típica, consulte [cenário: configurar um ambiente de produção para implantação da Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](choosing-the-right-approach-to-web-deployment.md)
> [Próximo](scenario-configuring-a-staging-environment-for-web-deployment.md)
