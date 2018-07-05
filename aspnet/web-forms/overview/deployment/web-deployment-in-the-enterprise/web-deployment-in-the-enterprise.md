---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Implantação na empresa Web | Microsoft Docs
author: jrjlee
description: Este tutorial descreve como atender a muitos dos desafios que você encontrará ao gerenciar a implantação de aplicativos para desenvolvedores da web em escala empresarial...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 07c1ea9728c0130b860c0e0a64eb0751245ff840
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371136"
---
<a name="web-deployment-in-the-enterprise"></a>Implantação da Web na empresa
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial descreve como atender a muitos dos desafios que você encontrará ao gerenciar a implantação de aplicativos web em escala empresarial para ambientes de desenvolvimento, teste, preparação e produção. O tutorial inclui uma solução de referência junto com uma mistura de conteúdo conceitual e orientados a tarefas para orientá-lo por meio de várias tarefas comuns e procedimentos.
> 
> Para obter uma tradução de italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Desafios de implantação do Enterprise

As organizações geralmente encontram esses desafios ao procurar o para gerenciar a implantação de soluções complexas, de escala empresarial:

- Você precisa ser capaz de implantar projetos em vários ambientes, como desenvolvedor ou ambientes de teste de preparação plataformas e servidores de produção. A solução precisa ser implantado com configurações diferentes para cada ambiente.
- Você precisará implantar vários projetos dependentes simultaneamente como parte de um processo de compilação e implantação passo a passo ou automatizado.
- Você precisa ser capaz de implantação de unidade de um processo automatizado. Por exemplo, você deseja usar um processo de CI (integração contínua) para implantar aplicativos da web para um ambiente de teste quando o novo código é verificado.
- Você precisa ser capaz de controlar o processo de implantação e definir variáveis de implantação de fora do Visual Studio, como os desenvolvedores podem não ter as definições de configuração correto ou as credenciais necessárias para cada ambiente de destino.
- Você precisa implantar projetos com base no esquema de banco de dados e preservar os dados existentes nas implantações subsequentes.
- Você precisa implantar bancos de dados de associação em uma base ad hoc sem implantar dados da conta de usuário. Talvez você também precise atualizar o esquema de bancos de dados de associação implantado sem perda de dados de conta de usuário existente.
- Você precisará excluir determinados arquivos ou pastas quando você implanta conteúdo em vários ambientes de destino.

## <a name="overview-of-approach"></a>Visão geral da abordagem

Neste tutorial, junto com os outros tutoriais nesta série, usa essa abordagem de alto nível para atender aos desafios descritos acima.

- **Use arquivos de projeto personalizados do Microsoft Build Engine (MSBuild) para controlar o processo geral de compilação e implantação.**
- Isso lhe permite criar e implantar todos os projetos na solução como parte de uma única operação programável por script.
- Configurações específicas do ambiente são configuradas usando arquivos de projeto simples de específicas do ambiente. Em contraste com a Visual Studio – centrado em abordagem do uso de configurações da solução e perfis de publicação para configurar implantações para ambientes diferentes, essa abordagem permite configurar e gerenciar o processo de implantação de fora do Visual Studio. Isso significa que os desenvolvedores não precisam avance o conhecimento de cadeias de caracteres de conexão, pontos de extremidade de serviço, credenciais do servidor e outras variáveis de implantação para ambientes de destino.
- Os arquivos de projeto personalizado podem ser invocados pelo Team Build como parte de um fluxo de trabalho do Team Foundation Server (TFS). Isso lhe permite configurar a implantação automatizada para cenários de CI.

**Use a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para empacotar e implantar projetos de aplicativos web.**

- A implantação da Web fornece uma estrutura que permite que você empacotar e implantar o conteúdo do aplicativo web em um servidor de web IIS de destino, junto com todos os outros requisitos, as definições de configuração, configurações de segurança e as dependências.
- Você pode controlar todo processo de empacotamento e implantação de dentro de seus arquivos de projeto do MSBuild personalizados. Você também pode manipular as definições de configuração que acompanham o pacote de implantação da web, como cadeias de caracteres de conexão, pontos de extremidade de serviço e detalhes de destino do IIS.
- A implantação da Web, juntamente com o Pipeline de publicação na Web, oferece vários pontos de extensibilidade que permitem que você personalize suas implantações. Por exemplo, é fácil excluir arquivos indesejados e pastas de seus pacotes de implantação da web.

**Use o utilitário VSDBCMD.exe para implantar e atualizar esquemas de banco de dados.**

- VSDBCMD permite que você implante bancos de dados de um arquivo de esquema de banco de dados (.dbschema), que é gerada quando você compila um projeto de banco de dados do Visual Studio. Por outro lado, a funcionalidade de implantação de banco de dados incluída na implantação da Web é mais adequada à implantação de bancos de dados existentes de uma instância do SQL Server local.
- Ao contrário da funcionalidade do Visual Studio para implantação de projetos de banco de dados, o VSDBCMD permite que você implante atualizações diferenciais para um banco de dados de destino existente. Isso permite que você preserve os dados existentes, enquanto você atualiza o esquema de banco de dados.
- Você pode executar comandos VSDBCMD de dentro de seus arquivos de projeto do MSBuild personalizados.

## <a name="content-map"></a>Mapa de conteúdo

Este tutorial inclui tópicos que se enquadram em quatro áreas principais.

Estes tópicos apresentam a solução de referência&#x2014;a solução do Contact Manager&#x2014;e descrevem como baixá-lo e configurá-lo em seu computador local:

- [A solução Gerenciador de Contatos](the-contact-manager-solution.md)
- [Configuração da solução Gerenciador de Contatos](setting-up-the-contact-manager-solution.md)

Estes tópicos apresentam os arquivos de projeto MSBuild, descrevem como você pode criar e usar arquivos de projeto personalizado e percorrer o processo de implantação para a solução de Gerenciador de contatos:

- [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md)
- [Noções básicas sobre o processo de build](understanding-the-build-process.md)

Estes tópicos descrevem a implantação do aplicativo web, incluindo como a compilação e empacotamento o processo funciona, como o processo de compilação se integra com o Pipeline de publicação na Web, como modificar os parâmetros de implantação e como implantar pacotes da web para o destino ambientes:

- [Criação e a empacotamento de projetos de aplicativos Web](building-and-packaging-web-application-projects.md)
- [Configuração de parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md)
- [Implantando pacotes da Web](deploying-web-packages.md)

- [Implantação de projetos de banco de dados](deploying-database-projects.md) descreve as diferentes técnicas que você pode usar para implantar projetos de banco de dados do Visual Studio, junto com as vantagens e desvantagens de cada abordagem. [Criando e executando um arquivo de comando de implantação](creating-and-running-a-deployment-command-file.md) descreve como criar um arquivo de comando simples que encapsula a lógica de implantação e permite que você implante soluções complexas como um processo passo a passo.
- Por fim, [manualmente instalando os pacotes da Web](manually-installing-web-packages.md) conclui o tutorial mostrando a você importar pacotes da web para o IIS.

## <a name="key-technologies"></a>Tecnologias-chave

Os tópicos neste tutorial usam essas tecnologias para gerenciar a compilação e implantação:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- O utilitário de implantação de banco de dados VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais nesta série

Isso faz parte de uma série de tutoriais de cinco na implantação da web de escala empresarial. Estes são os outros tutoriais na série:

- [Implantando aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Ele descreve o cenário do tutorial e ilustra como as tarefas e instruções passo a passo descrita em toda a série se encaixam um processo mais amplo do gerenciamento de ciclo de vida de aplicativos (ALM).
- [Configurar ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores do Windows para dar suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web e a implantação de banco de dados remoto. Ele fornece orientações sobre como escolher o método de implantação apropriadas para seu próprio ambiente e descreve como usar o Web Farm Framework (WFF) para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparado manualmente as implantações de compilações específicas.
- [Avançadas de implantação da Web corporativa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar diversas tarefas de implantação mais avançadas, como personalizar as implantações de banco de dados para vários ambientes, excluindo arquivos e pastas de implantação e colocar aplicativos web em offline durante o processo de implantação .

> [!div class="step-by-step"]
> [Avançar](the-contact-manager-solution.md)
