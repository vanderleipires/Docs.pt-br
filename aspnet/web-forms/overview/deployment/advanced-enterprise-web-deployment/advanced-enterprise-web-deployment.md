---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Avançadas de implantação da Web corporativa | Microsoft Docs
author: jrjlee
description: Este tutorial mostra como executar várias tarefas que são necessárias ou desejável em muitos cenários de implantação do enterprise. Para um translati italiano...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 34f1bf3bc2c37afc66f458a60a29fe5ce8f6c018
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825188"
---
<a name="advanced-enterprise-web-deployment"></a>Implantação da Web corporativa avançada
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostra como executar várias tarefas que são necessárias ou desejável em muitos cenários de implantação do enterprise.
> 
> Para obter uma tradução de italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Isso faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="scenario-overview"></a>Visão geral do cenário

O cenário de alto nível para esses tutoriais é descrito em [implantação da Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). É recomendável que você examine este tópico antes de começar este tutorial.

## <a name="how-to-use-this-tutorial"></a>Como usar esse tutorial

- Cada um dos tópicos neste tutorial é independente e resolve um problema que ocorre em cenários de implantação do enterprise ou o desafio em particular. Você não precisa trabalhar com esses tópicos em nenhuma ordem específica. No entanto, este tutorial aborda algumas tarefas avançadas. Como tal, você deve se familiarizar com os conceitos e técnicas que o [implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial aborda a fim de obter o melhor proveito deste conteúdo.
- Este tutorial inclui os seguintes tópicos:
- [Executando uma implantação "What If"](performing-a-what-if-deployment.md). Em muitos cenários, você desejará determinar o impacto de uma implantação proposto em um ambiente de destino ou todo o conteúdo existente antes de realmente fazer as alterações. Este tópico descreve como você pode executar uma implantação "what if" para gerar scripts de atualização de banco de dados e arquivos de log como se você implantou o conteúdo em um ambiente de destino, sem realmente fazer nenhuma alteração. Analisar esses recursos pode ajudar a identificar problemas potenciais com antecedência sobre uma implantação em tempo real.
- [Personalizando as implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md). Quando você implanta um projeto de banco de dados para vários destinos, muitas vezes você desejará personalizar as propriedades de implantação para cada ambiente de destino. Por exemplo, em ambientes de teste seria normalmente recriar o banco de dados em todas as implantações, ao passo que em ambientes de preparo ou produção, você seria muito mais provável que faça atualizações incrementais para preservar seus dados. Este tópico descreve como você pode incorporar essas alterações de propriedade em sua lógica de implantação, criando um arquivo de configuração (.sqldeployment) específicas do ambiente de implantação para cada ambiente de destino.
- Implantando associações de função de banco de dados em ambientes de teste. Quando você recriar um banco de dados em todas as implantações&#x2014;por exemplo, como parte de uma integração contínua (CI) compilar e implantar em um ambiente de teste&#x2014;normalmente você precisará configurar as associações de função de banco de dados cada vez. Por exemplo, você geralmente precisará conceder permissões para a identidade do pool de aplicativos associado ao seu aplicativo web. Este tópico descreve como você pode automatizar esse processo com a adição de um script SQL de pós-implantação para sua lógica de implantação.
- [Implantando bancos de dados de associação em ambientes empresariais](deploying-membership-databases-to-enterprise-environments.md). Bancos de dados de associação ASP.NET têm várias características que podem complicar o processo de implantação. Por exemplo, uma implantação somente de esquema deixará o banco de dados em um estado não operacional. Na maioria dos cenários, é preferível para criar um banco de dados de associação diretamente em cada ambiente de destino. No entanto, se você precisa implantar um banco de dados de associação, este tópico descreve algumas das abordagens que você pode usar para atender aos desafios inerentes.
- [Excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md). Em alguns cenários, você desejará personalizar o conteúdo do pacote da web para ambientes de destino específico. Por exemplo, você talvez queira incluir versões completas de bibliotecas JavaScript quando você implanta em um ambiente de teste para dar suporte à depuração do lado do cliente, mas use minificadas versões das bibliotecas quando você implanta em um ambiente de preparo ou produção. Este tópico descreve como excluir arquivos e pastas específicos do processo de criação de pacote.
- [Colocar aplicativos Web em Offline com Web implantar](taking-web-applications-offline-with-web-deploy.md). Quando você implanta soluções em um ambiente de preparo ou produção, você geralmente deseja levar seus aplicativos web offline durante o processo de implantação. Este tópico descreve como você pode adicionar um *App\_offline.htm* arquivo ao seu aplicativo web no início do processo de implantação e remova-o ao final. Enquanto o *App\_offline.htm* arquivo está em vigor, quaisquer usuários que navegam para o aplicativo web serão redirecionados automaticamente para o *App\_offline.htm* arquivo.
- [Executando Scripts do Windows PowerShell do MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Muitos cenários de implantação exigem ações de pós-implantação mais complexas, como adicionar fontes de evento personalizado para o registro ou configurando a replicação entre instâncias do SQL Server. Geralmente, essas ações são realizadas por meio de scripts do Windows PowerShell. Este tópico descreve como executar scripts do Windows PowerShell de um arquivo de projeto do Microsoft Build Engine (MSBuild) como parte do processo de compilação e implantação.
- [O processo de empacotamento da solução de problemas](troubleshooting-the-packaging-process.md). O Pipeline de publicação de Web (WPP) define uma propriedade de MSBuild denominada **EnablePackageProcessLoggingAndAssert** que você pode usar para gerar informações detalhadas sobre o processo de empacotamento de projetos de aplicativos web. Este tópico descreve o que a propriedade faz e como usá-lo.

## <a name="key-technologies"></a>Tecnologias-chave

Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte à compilação automatizada e implantação da web:

- Visual Studio 2010 e o Team Foundation Server (TFS) 2010
- MSBuild e o TFS Team Build
- Serviços de informações da Internet (IIS) 7.5
- Ferramenta de implantação da Web de IIS (implantação da Web) 2.1
- O utilitário de implantação de banco de dados VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais nesta série

Isso faz parte de uma série de tutoriais de cinco na implantação da web de escala empresarial. Estes são os outros tutoriais na série:

- [Implantando aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Ele descreve o cenário do tutorial e ilustra como as tarefas e instruções passo a passo descrita em toda a série se encaixam um processo mais amplo do gerenciamento de ciclo de vida de aplicativos (ALM).
- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual ao arquivos de projeto MSBuild, o WPP, implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas juntos para gerenciar processos de implantação complexa.
- [Configurar ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores do Windows para dar suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web e a implantação de banco de dados remoto. Ele fornece orientações sobre como escolher o método de implantação apropriadas para seu próprio ambiente e descreve como usar o Web Farm Framework (WFF) para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparado manualmente as implantações de compilações específicas.

> [!div class="step-by-step"]
> [Avançar](performing-a-what-if-deployment.md)
