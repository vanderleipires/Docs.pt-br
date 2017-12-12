---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "Cenário: Configurar um ambiente de teste para a implantação da Web | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve um cenário de implantação da web típico para desenvolvedores ou ambientes de teste e explica as tarefas que precisam ser concluídas para configurar um si..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 008b9cd081152e6a378d0fa2e08497a6771fd9b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Cenário: Configurar um ambiente de teste para a implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da web típico para desenvolvedores ou ambientes de teste e explica as tarefas que precisam ser concluídas para configurar um ambiente semelhante.


Quando os desenvolvedores trabalham em aplicativos da web, eles geralmente recebe acesso para um ambiente de servidor que podem ser usadas para testar alterações em seus aplicativos em uma configuração realista. Normalmente, esse tipo de ambiente de desenvolvimento ou teste tem as seguintes características:

- O ambiente consiste em um único servidor web e um servidor de banco de dados único.
- Os desenvolvedores geralmente têm privilégios de administrador nos servidores que você configurar o ambiente para os requisitos de seus aplicativos.
- Alterações em aplicativos são implantadas com frequência, assim o ambiente precisa dar suporte a apenas uma etapa ou a implantação automatizada.

Por exemplo, em nosso [cenário do tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink é um desenvolvedor na Fabrikam, Inc. Matt está trabalhando na solução de Gerenciador de contato e regularmente precisa implantar alterações em um ambiente de teste. Matt é um administrador no servidor web de teste e o servidor de banco de dados de teste. Inicialmente, Matt precisa ser capaz de implantar a solução para o ambiente de teste diretamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Como trabalho progride e desenvolvedores unir a equipe, o gerente do contato solução está configurada para integração contínua (CI) no Team Foundation Server (TFS). Sempre que um desenvolvedor verifica no conteúdo, Team Build deve compilar a solução, executar testes de unidade e implantar a solução automaticamente para o ambiente de teste.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Visão geral da solução

O ambiente de teste precisa dar suporte a apenas uma etapa ou automatizado implantação de um computador remoto, você tem a opção de duas abordagens principais. Você pode:

- Configure o servidor web de teste para dar suporte à implantação usando o serviço de agente de implantação da Web (o "agente remoto").
- Configure o servidor web de teste para dar suporte à implantação usando o manipulador de implantação da Web.

> [!NOTE]
> Você também pode usar [Web implantar sob demanda](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) (o "agente temp"). Isso é semelhante à abordagem de agente remoto em termos de requisitos e restrições.


Nesse caso, os desenvolvedores têm privilégios de administrador no servidor de destino e o ambiente de teste não está sujeito a restrições de segurança estrita, a opção lógica configurar o servidor web de teste para dar suporte à implantação usando o agente remoto. Isso é menos complexo e exige a configuração inicial menor que a abordagem de manipulador de implantação da Web. Você também precisará configurar o servidor de banco de dados para dar suporte à implantação e acesso remoto.

Esses tópicos fornecem todas as informações necessárias para concluir essas tarefas:

- [Configurar um servidor Web para publicação (agente remoto) de implantação da Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Este tópico descreve como criar um servidor web que dá suporte à implantação da Web de publicação, usando a abordagem de agente remoto, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configurar um servidor de banco de dados para publicação de implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte à implantação, a partir de uma instalação padrão do SQL Server 2008 R2 e acesso remoto.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente típico de preparo, consulte [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente de produção típicos, consulte [cenário: configurar um ambiente de produção para implantação de Web](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Anterior](choosing-the-right-approach-to-web-deployment.md)
[Próximo](scenario-configuring-a-staging-environment-for-web-deployment.md)
