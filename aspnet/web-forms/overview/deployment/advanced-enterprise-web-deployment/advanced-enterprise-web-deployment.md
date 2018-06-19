---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Enterprise Web implantação avançada | Microsoft Docs
author: jrjlee
description: Este tutorial mostram como executar várias tarefas que são necessárias ou desejável em muitos cenários de implantação corporativa. Para um translati italiano...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879927"
---
<a name="advanced-enterprise-web-deployment"></a>Implantação de Web Enterprise avançadas
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostram como executar várias tarefas que são necessárias ou desejável em muitos cenários de implantação corporativa.
> 
> Para obter uma tradução italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Isso faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="scenario-overview"></a>Visão geral do cenário

O cenário de alto nível para esses tutoriais é descrito em [implantação de Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). É recomendável que você examine este tópico antes de começar este tutorial.

## <a name="how-to-use-this-tutorial"></a>Como usar esse tutorial

- Cada um dos tópicos neste tutorial é autossuficiente e corrige um problema que ocorre em cenários de implantação corporativa ou desafio em particular. Você não precisa trabalhar com esses tópicos em nenhuma ordem específica. No entanto, este tutorial aborda algumas tarefas avançadas. Como tal, você deve se familiarizar com os conceitos e técnicas que o [Web implantação na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial aborda para obter o máximo benefício deste conteúdo.
- Este tutorial inclui os seguintes tópicos:
- [Executando uma implantação "E se"](performing-a-what-if-deployment.md). Em muitos cenários, convém determinar o impacto de uma implantação proposto em um ambiente de destino ou todo o conteúdo existente antes de realmente fazer as alterações. Este tópico descreve como você pode executar uma implantação "e se" para gerar arquivos de log e scripts de atualização de banco de dados como se for usado para implantar conteúdo em um ambiente de destino, sem fazer alterações. Analisar esses recursos pode ajudá-lo a identificar problemas potenciais com antecedência sobre uma implantação em tempo real.
- [Personalizando as implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md). Quando você implanta um projeto de banco de dados para vários destinos, geralmente você desejará personalizar as propriedades de implantação para cada ambiente de destino. Por exemplo, em ambientes de teste você seria normalmente recriar o banco de dados em todas as implantações, enquanto em ambientes de produção ou preparo será muito mais provável que faça atualizações incrementais para preservar seus dados. Este tópico descreve como você pode incorporar essas alterações de propriedade em sua lógica de implantação criando um arquivo de configuração (.sqldeployment) específico do ambiente de implantação para cada ambiente de destino.
- Implantando as associações de função de banco de dados em ambientes de teste. Quando você recriar o banco de dados de cada implantação&#x2014;por exemplo, como parte de uma CI (integração contínua) criar e implantar em um ambiente de teste&#x2014;normalmente você precisará configurar associações de função de banco de dados de cada vez. Por exemplo, você geralmente precisará conceder permissões para a identidade do pool de aplicativos associado ao seu aplicativo web. Este tópico descreve como você pode automatizar esse processo, adicionando um script SQL de pós-implantação à sua lógica de implantação.
- [Implantação de bancos de dados de associação em ambientes corporativos](deploying-membership-databases-to-enterprise-environments.md). Bancos de dados de associação ASP.NET têm várias características que podem complicar o processo de implantação. Por exemplo, uma implantação somente de esquema deixará o banco de dados em um estado não-operacional. Na maioria dos cenários, é preferível para criar um banco de dados de associação diretamente em cada ambiente de destino. No entanto, se você precisa implantar um banco de dados de associação, este tópico descreve algumas das abordagens que você pode usar para atender aos desafios inerentes.
- [Excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md). Em alguns cenários, você desejará personalizar o conteúdo do pacote da web para ambientes de destino específico. Por exemplo, você talvez queira incluem versões completas de bibliotecas JavaScript ao implantar em um ambiente de teste, para oferecer suporte à depuração do lado do cliente, mas use minimizadas versões das bibliotecas de quando você implanta em um ambiente de preparo ou produção. Este tópico descreve como você pode excluir arquivos e pastas específicos do processo de criação de pacote.
- [Implantar aplicativos de Web do colocando Offline com o Web](taking-web-applications-offline-with-web-deploy.md). Quando você implantar soluções em um ambiente de preparo ou produção, geralmente você desejará levar seus aplicativos web offline durante o processo de implantação. Este tópico descreve como você pode adicionar uma *aplicativo\_offline.htm* o arquivo para seu aplicativo da web no início do processo de implantação e removê-lo no final. Enquanto o *aplicativo\_offline.htm* arquivo está em vigor, qualquer usuário que acesse o aplicativo da web sejam redirecionado automaticamente para o *aplicativo\_offline.htm* arquivo.
- [Executando Scripts do Windows PowerShell do MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Muitos cenários de implantação exigem ações de pós-implantação mais complexas, como adicionar fontes de evento personalizado para o registro ou configurar a replicação entre instâncias do SQL Server. Geralmente, essas ações são realizadas por meio de scripts do Windows PowerShell. Este tópico descreve como executar scripts do Windows PowerShell de um arquivo de projeto do Microsoft Build Engine (MSBuild) como parte do processo de compilação e implantação.
- [O processo de empacotamento de solução de problemas](troubleshooting-the-packaging-process.md). O Pipeline de publicação de Web (WPP) define uma propriedade de MSBuild chamada **EnablePackageProcessLoggingAndAssert** que você pode usar para gerar informações detalhadas sobre o processo de empacotamento para projetos de aplicativo web. Este tópico descreve o que a propriedade faz e como usá-lo.

## <a name="key-technologies"></a>Tecnologias de chave

Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte a compilação automatizada e implantação da web:

- Visual Studio 2010 e o Team Foundation Server (TFS) 2010
- MSBuild e compilação de equipe do TFS
- Serviços de informações da Internet (IIS) 7.5
- Ferramenta de implantação da Web do IIS (implantação da Web) 2.1
- O utilitário de implantação de banco de dados VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais nesta série

Isso faz parte de uma série de cinco tutoriais na implantação da web de grande porte. Estes são os outros tutoriais na série:

- [Implantando aplicativos da Web em cenários corporativos](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Descreve o cenário do tutorial e ilustra como as tarefas e descrita em toda a série explicações passo a passo se encaixam um processo de gerenciamento de ciclo de vida do aplicativo (ALM) mais amplo.
- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual para arquivos de projeto do MSBuild, o WPP, Web Deploy e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas em conjunto para gerenciar processos complexos de implantação.
- [Configurando ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar os servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web e implantação de banco de dados remoto. Fornece orientação sobre como escolher o método de implantação apropriadas para seu próprio ambiente, e descreve como usar o Web Farm Framework (WFF) para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparada manualmente as implantações de compilações específicas.

> [!div class="step-by-step"]
> [Avançar](performing-a-what-if-deployment.md)
