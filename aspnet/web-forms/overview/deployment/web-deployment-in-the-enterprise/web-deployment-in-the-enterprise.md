---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Implantação na empresa Web | Microsoft Docs
author: jrjlee
description: Este tutorial descreve como atender muitos dos desafios encontrados ao gerenciar a implantação de aplicativos da web de nível corporativo para devel...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: fc463cb689f4f63a12788b80958c9fc8fe20119d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890457"
---
<a name="web-deployment-in-the-enterprise"></a>Implantação da Web da empresa
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial descreve como atender muitos dos desafios encontrados ao gerenciar a implantação de aplicativos da web de nível corporativo para ambientes de desenvolvimento, teste, preparação e produção. O tutorial inclui uma solução de referência junto com uma mistura de conteúdo conceitual e orientada a tarefas para guiá-lo por meio de várias tarefas comuns e procedimentos.
> 
> Para obter uma tradução italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Desafios de implantação do Enterprise

As organizações geralmente encontram esses desafios ao procurar o para gerenciar a implantação de soluções complexas, de nível corporativo:

- Você precisa ser capaz de implantar projetos em vários ambientes, como desenvolvedor ou ambientes de teste de preparo plataformas e servidores de produção. A solução precisa ser implantada com configurações diferentes para cada ambiente.
- Você precisa implantar vários projetos dependentes simultaneamente como parte de um processo de compilação e implantação única etapa ou automatizado.
- Você precisa ser capaz de implantação de unidade em um processo automatizado. Por exemplo, você deseja usar um processo de integração contínua (CI) para implantar aplicativos da web para um ambiente de teste quando o novo código de check-in.
- Você precisa ser capaz de controlar o processo de implantação e definir variáveis de implantação de fora do Visual Studio, assim como os desenvolvedores provavelmente não terão as configurações corretas de configuração ou as credenciais necessárias para cada ambiente de destino.
- Você precisa implantar projetos de banco de dados baseada em esquema e preservar os dados existentes em implantações subsequentes.
- Você precisa implantar bancos de dados de associação em uma base ad hoc sem implantar dados da conta de usuário. Você também precisará atualizar o esquema de bancos de dados de associação implantado sem perda de dados de conta de usuário existente.
- Você precisará excluir determinados arquivos ou pastas quando você implanta conteúdo em vários ambientes de destino.

## <a name="overview-of-approach"></a>Visão geral da abordagem

Neste tutorial, junto com os outros tutoriais nesta série, usa essa abordagem de alto nível para enfrentar os desafios descritos acima.

- **Use arquivos de projeto Microsoft Build Engine (MSBuild) personalizados para controlar o processo geral de compilação e implantação.**
- Isso permite criar e implantar todos os projetos na solução como parte de uma única operação programável.
- Configurações específicas do ambiente são configuradas usando arquivos de projeto simples de ambiente específicas. Em contraste com a Visual Studio centralizado em abordagem de usando as configurações de solução e perfis de publicação para configurar implantações para ambientes diferentes, essa abordagem permite configurar e gerenciar o processo de implantação de fora do Visual Studio. Isso significa que os desenvolvedores não precisam avança o conhecimento de cadeias de caracteres de conexão, pontos de extremidade de serviço, as credenciais do servidor e outras variáveis de implantação para ambientes de destino.
- Os arquivos de projeto personalizados podem ser invocados pelo Team Build como parte de um fluxo de trabalho do Team Foundation Server (TFS). Isso lhe permite configurar a implantação automatizada para cenários de CI.

**Use a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) para empacotar e implantar projetos de aplicativo web.**

- A implantação da Web fornece uma estrutura que permite que você empacotar e implantar o conteúdo do aplicativo da web em um servidor de web IIS de destino, junto com as dependências, definições de configuração, configurações de segurança e todos os outros requisitos.
- Você pode controlar todo processo de empacotamento e implantação em seus arquivos de projeto MSBuild personalizados. Você também pode manipular as definições de configuração que acompanham seu pacote de implantação da web, como cadeias de caracteres de conexão, pontos de extremidade de serviço e detalhes de destino do IIS.
- A implantação da Web, junto com o Pipeline de publicação na Web, oferece vários pontos de extensibilidade que permitem personalizar suas implantações. Por exemplo, é fácil excluir arquivos indesejados e pastas de seus pacotes de implantação da web.

**Use o utilitário VSDBCMD.exe para implantar e atualizar esquemas de banco de dados.**

- VSDBCMD permite implantar bancos de dados de um arquivo de esquema de banco de dados (.dbschema), que é gerada quando você cria um projeto de banco de dados do Visual Studio. Por outro lado, a funcionalidade de implantação de banco de dados incluída na implantação da Web é mais adequada para a implantação de bancos de dados existentes de uma instância local do SQL Server.
- Ao contrário da funcionalidade do Visual Studio para implantar projetos de banco de dados, o VSDBCMD permite que você implante atualizações diferenciais para um banco de dados de destino existente. Isso permite que você preservar os dados existentes ao atualizar o esquema de banco de dados.
- Você pode executar comandos VSDBCMD de dentro de seus arquivos de projeto MSBuild personalizados.

## <a name="content-map"></a>Mapa de conteúdo

Este tutorial inclui tópicos que se enquadram em quatro áreas principais.

Estes tópicos apresentam a solução de referência&#x2014;a solução Contact Manager&#x2014;e descrevem como baixá-lo e configurá-lo em seu computador local:

- [A solução Gerenciador de Contatos](the-contact-manager-solution.md)
- [Configuração da solução Gerenciador de Contatos](setting-up-the-contact-manager-solution.md)

Estes tópicos apresentam os arquivos de projeto do MSBuild, descrevem como você pode criar e usar arquivos de projeto personalizados e percorrer o processo de implantação para a solução do gerente do contato:

- [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md)
- [Noções básicas sobre o processo de build](understanding-the-build-process.md)

Estes tópicos descrevem a implantação do aplicativo da web, incluindo como a compilação e empacotamento o processo funciona, como o processo de compilação se integra com o Pipeline de publicação na Web, como modificar parâmetros de implantação e como implantar pacotes da web para o destino ambientes:

- [Criação e a empacotamento de projetos de aplicativos Web](building-and-packaging-web-application-projects.md)
- [Configuração de parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md)
- [Implantando pacotes da Web](deploying-web-packages.md)

- [Implantação de projetos de banco de dados](deploying-database-projects.md) descreve as técnicas diferentes que você pode usar para implantar projetos de banco de dados do Visual Studio, junto com as vantagens e desvantagens de cada abordagem. [Criando e executando um arquivo de comando de implantação](creating-and-running-a-deployment-command-file.md) descreve como criar um arquivo de comando simples que encapsula a lógica de implantação e permite que você implante soluções complexas, como um processo de um única etapa.
- Por fim, [instalar pacotes de Web manualmente](manually-installing-web-packages.md) conclui o tutorial mostrando para importar pacotes da web no IIS.

## <a name="key-technologies"></a>Tecnologias de chave

Os tópicos neste tutorial principalmente usam essas tecnologias para gerenciar a compilação e implantação:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- O utilitário de implantação de banco de dados VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais nesta série

Isso faz parte de uma série de cinco tutoriais na implantação da web de grande porte. Estes são os outros tutoriais na série:

- [Implantando aplicativos da Web em cenários corporativos](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Descreve o cenário do tutorial e ilustra como as tarefas e descrita em toda a série explicações passo a passo se encaixam um processo de gerenciamento de ciclo de vida do aplicativo (ALM) mais amplo.
- [Configurando ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar os servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web e implantação de banco de dados remoto. Fornece orientação sobre como escolher o método de implantação apropriadas para seu próprio ambiente, e descreve como usar o Web Farm Framework (WFF) para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparada manualmente as implantações de compilações específicas.
- [Enterprise Web implantação avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar as implantações de banco de dados para vários ambientes, excluindo arquivos e pastas de implantação e que os aplicativos de web offline durante o processo de implantação .

> [!div class="step-by-step"]
> [Avançar](the-contact-manager-solution.md)
