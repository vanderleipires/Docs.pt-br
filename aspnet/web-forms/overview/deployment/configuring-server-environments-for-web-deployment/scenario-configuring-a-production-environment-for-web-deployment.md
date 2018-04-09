---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Cenário: Configurar um ambiente de produção para implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário de implantação da web típico para um ambiente de produção e explica as tarefas que precisam ser concluídas para configurar um semelhante...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Cenário: Configurar um ambiente de produção para implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da web típico para um ambiente de produção e explica as tarefas que precisam ser concluídas para configurar um ambiente semelhante.


O ambiente de produção é o destino final para um aplicativo web ou um site. Neste ponto, seu aplicativo tiver sido por meio de testes foi implantado em um ambiente de preparo e está pronto para "ficar ativo." As características de um ambiente de produção podem variar amplamente de acordo com a natureza e a finalidade de conteúdo da web, o tamanho da sua organização, seu público-alvo e muitos outros fatores. Em um cenário de grande porte, o ambiente de produção pode ter as seguintes características:

- O ambiente consiste em vários servidores web com balanceamento de carga e um ou mais servidores de banco de dados, geralmente com clustering de failover e o espelhamento de banco de dados.
- Se o ambiente é voltado para a Internet, é provável que seja separada da sua rede interna. Ele pode estar em uma sub-rede diferente em uma rede de perímetro, ele pode estar em um domínio diferente e pode ser em uma infraestrutura de rede completamente diferente.
- Os desenvolvedores e contas de processo do servidor de compilação são altamente improvável que tenha privilégios de administrador nos servidores de produção.
- Alterações em aplicativos são implantadas com menos frequência de teste ou implantações de preparo.

> [!NOTE]
> Expansão de uma implantação de banco de dados em vários servidores está além do escopo deste tutorial. Para obter mais informações sobre essa área, consulte [Manuais Online do SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por exemplo, em nosso [cenário do tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), um servidor do Team Build inclui definições de compilação que permitem aos usuários criar a solução de Gerenciador de contato e implantá-lo em um ambiente de preparo em uma única etapa. Quando o aplicativo está pronto para ser implantado para produção, devido a restrições impostas por requisitos de segurança e a infraestrutura de rede, o administrador do ambiente de produção deve copiar o pacote da web em um servidor web de produção e importar manualmente ele por meio do Gerenciador de serviços de informações da Internet (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Visão geral da solução

Nesse cenário, você pode deduzir esses fatos de uma análise dos requisitos de implantação:

- Devido a restrições de segurança e a configuração de rede, você não pode configurar o ambiente de produção para dar suporte à implantação de um clique ou automatizada. Implantação off-line é a abordagem viável apenas nesse cenário.
- O ambiente de produção inclui vários servidores web, então você pode usar o Web Farm Framework (WFF) para criar um farm de servidores. Usando essa abordagem, o administrador só deve importar o aplicativo em um servidor web (o servidor primário) e WFF replicará a implantação em todos os outros servidores de web no ambiente de produção.

Esses tópicos fornecem todas as informações necessárias para concluir essas tarefas:

- [Criar um Farm de servidores com o Framework do Web Farm](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como criar e configurar um farm de servidores usando WFF, para que os produtos de plataforma da web e componentes, definições de configuração, sites e aplicativos são replicados em vários servidores web com balanceamento de carga.
- [Configurar um servidor Web para publicação (implantação off-line) de implantação da Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Este tópico descreve como criar um servidor web que permite que os administradores importam e implantar pacotes de web manualmente, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configurar um servidor de banco de dados para publicação de implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte à implantação, a partir de uma instalação padrão do SQL Server 2008 R2 e acesso remoto.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente de teste típicas de desenvolvimento, consulte [cenário: configurar um ambiente de teste para implantação de Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente típico de preparo, consulte [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Próximo](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
