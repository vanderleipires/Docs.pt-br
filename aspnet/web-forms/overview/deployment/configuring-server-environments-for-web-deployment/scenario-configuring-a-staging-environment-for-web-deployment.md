---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "Cenário: Configurar um ambiente de preparo para a implantação da Web | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve um cenário de implantação da web típico para um ambiente de preparo e explica as tarefas que precisam ser concluídas para configurar um env semelhante..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 683a0cf88225fee762e82925afe3785a2defd5bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Cenário: Configurar um ambiente de preparo para a implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da web típico para um ambiente de preparo e explica as tarefas que precisam ser concluídas para configurar um ambiente semelhante.


Muitas organizações usam ambientes de preparo para visualizar atualizações para sites ou aplicativos da web. Assim, as pessoas dentro da organização a chance de explorar e analisar a nova funcionalidade ou conteúdo antes do site "fica ativo", ou em outras palavras, é implantado em um ambiente de produção. O ambiente de preparo foi projetado para replicar o ambiente de produção mais próximo possível, para fornecer uma visualização realista. Normalmente, esse tipo de ambiente de preparo tem as seguintes características:

- O ambiente consiste em vários servidores web com balanceamento de carga e um ou mais servidores de banco de dados, geralmente com clustering de failover e o espelhamento de banco de dados.
- Aplicativos podem ser implantados manualmente por uma equipe de desenvolvimento ou automaticamente por um servidor do Team Build.
- Os usuários ou contas de processo que implantarem aplicativos são provavelmente não tem privilégios de administrador nos servidores de teste.
- Alterações em aplicativos são implantadas com frequência, assim o ambiente precisa dar suporte a apenas uma etapa ou a implantação automatizada.

> [!NOTE]
> Expansão de uma implantação de banco de dados em vários servidores está além do escopo deste tutorial. Para obter mais informações sobre essa área, consulte [Manuais Online do SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Por exemplo, em nosso [cenário do tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) gerencia a solução do gerente do contato. O administrador do TFS, Rob Walters, criou uma definição de compilação que permite que os desenvolvedores disparar uma implantação no ambiente de preparo conforme necessário.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Observe que na maioria dos casos, não necessariamente deseja implantar a compilação mais recente para o ambiente de preparo. Em vez disso, você provavelmente muito mais para implantar uma compilação específica que já passou por validação e verificação no ambiente de teste.

## <a name="solution-overview"></a>Visão geral da solução

Nesse cenário, você pode deduzir esses fatos de uma análise dos requisitos de implantação:

- A conta de usuário ou processo que executa a implantação não terá privilégios de administrador nos servidores de teste, para que os servidores web preparo devem dar suporte à implantação de não administrador. Como tal, você precisará configurar servidores de teste da web para usar o manipulador de implantação da Web em vez do agente remoto.
- O ambiente de preparo inclui vários servidores web, mas ela também precisa dar suporte à implantação de um clique ou automatizada, portanto, você precisará usar o Web Farm Framework (WFF) para criar um farm de servidores. Usando essa abordagem, você pode implantar um aplicativo em um servidor web (o servidor primário) e WFF replicará a implantação em todos os outros servidores de web no ambiente de preparo.
- A conta de usuário ou processo que executa a implantação deve ter permissões para criar bancos de dados. Como tal, você precisará adicionar a conta para o **dbcreator** a função de servidor no servidor de banco de dados, além de configurar o servidor de banco de dados para dar suporte a acesso remoto e a implantação.

Esses tópicos fornecem todas as informações necessárias para concluir essas tarefas:

- [Criar um Farm de servidores com o Framework do Web Farm](creating-a-server-farm-with-the-web-farm-framework.md). Este tópico descreve como criar e configurar um farm de servidores usando WFF, para que os produtos de plataforma da web e componentes, definições de configuração, sites e aplicativos são replicados em vários servidores web com balanceamento de carga.
- [Configurar um servidor Web para publicação de implantação da Web (manipulador de implantação da Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Este tópico descreve como criar um servidor web que dá suporte à implantação da Web de publicação, usando a abordagem de agente remoto, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configurar um servidor de banco de dados para publicação de implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte à implantação, a partir de uma instalação padrão do SQL Server 2008 R2 e acesso remoto.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente de teste típicas de desenvolvimento, consulte [cenário: configurar um ambiente de teste para implantação de Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente de produção típicos, consulte [cenário: configurar um ambiente de produção para implantação de Web](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Anterior](scenario-configuring-a-test-environment-for-web-deployment.md)
[Próximo](scenario-configuring-a-production-environment-for-web-deployment.md)
