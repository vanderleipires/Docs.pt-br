---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Cenário: Configuração de um ambiente de produção para implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário de implantação da web típico para um ambiente de produção e explica as tarefas que você precisa concluir para configurar um semelhante...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: dc16b2fadec8051f78d13ffeb97abf2d9e6930f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806707"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Cenário: Configuração de um ambiente de produção para implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da web típico para um ambiente de produção e explica as tarefas que você precisa concluir para configurar um ambiente semelhante.


O ambiente de produção é o destino final para um aplicativo web ou um site. Nesse ponto, seu aplicativo tiver sido por meio de testes, tiver sido implantado em um ambiente de preparo e está pronto para "entrar no ar." As características de um ambiente de produção podem variar amplamente de acordo com a natureza e a finalidade do seu conteúdo da web, o tamanho da sua organização, seu público-alvo e muitos outros fatores. Em um cenário de escala empresarial, o ambiente de produção pode ter as seguintes características:

- O ambiente consiste em vários servidores web com balanceamento de carga e um ou mais servidores de banco de dados, geralmente com clustering de failover e o espelhamento de banco de dados.
- Se o ambiente é voltado para a Internet, é provável que sejam separadas da sua rede interna. Pode ser em uma sub-rede diferente em uma rede de perímetro, talvez eles estejam em um domínio diferente e pode ser em uma infraestrutura de rede completamente diferente.
- Os desenvolvedores e contas de processo do servidor de compilação são altamente improvável que tenha privilégios de administrador nos servidores de produção.
- Alterações em aplicativos são implantadas com menos frequência de teste ou implantações de preparo.

> [!NOTE]
> Escalar horizontalmente uma implantação de banco de dados em vários servidores está além do escopo deste tutorial. Para obter mais informações sobre essa área, consulte [Manuais Online do SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por exemplo, no nosso [cenário do tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), um servidor do Team Build inclui definições de compilação que permitem aos usuários criar a solução de Gerenciador de contatos e implantá-lo em um ambiente de preparo em uma única etapa. Quando o aplicativo está pronto para ser implantado para produção, devido às restrições impostas por requisitos de segurança e a infraestrutura de rede, o administrador do ambiente de produção deve copiar o pacote da web em um servidor web de produção e importar manualmente ele por meio do Gerenciador de serviços de informações da Internet (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Visão geral da solução

Nesse cenário, você pode deduzir destes fatos a partir de uma análise dos requisitos de implantação:

- Devido a restrições de segurança e a configuração de rede, é possível configurar o ambiente de produção para dar suporte à implantação automatizada ou de um clique. Implantação offline é a abordagem viável apenas nesse cenário.
- O ambiente de produção inclui vários servidores web, portanto, você pode usar o Web Farm Framework (WFF) para criar um farm de servidores. Usando essa abordagem, o administrador só precisa importar o aplicativo em um servidor web (o servidor primário) e WFF replicará a implantação em todos os outros servidores de web no ambiente de produção.

Estes tópicos fornecem todas as informações necessárias para concluir essas tarefas:

- [Criar um Farm de servidores com o Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como criar e configurar um farm de servidores usando WFF, para que os produtos de plataforma da web e componentes, definições de configuração e sites e aplicativos sejam replicados em vários servidores web com balanceamento de carga.
- [Configurar um servidor Web para publicação (implantação Offline) de implantação da Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Este tópico descreve como criar um servidor web que permite aos administradores importam e implantar os pacotes da web manualmente, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configurar um servidor de banco de dados para publicação de implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte a acesso remoto e a implantação, a partir de uma instalação padrão do SQL Server 2008 R2.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente de teste do desenvolvedor típico, consulte [cenário: configurar um ambiente de teste para implantação da Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente típico de preparo, consulte [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Próximo](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
