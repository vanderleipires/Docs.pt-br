---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Implantação da Web corporativa: Visão geral do cenário | Microsoft Docs'
author: jrjlee
description: Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação da empresa fictícia, para fornecer uma ref...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890015"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Implantação da Web de empresa: Visão geral do cenário
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação da empresa fictícia, para fornecer uma implementação de referência e conceder a tarefas e instruções passo a passo um contexto de comum. Este tópico descreve o cenário do tutorial e apresenta a solução de exemplo.


## <a name="scenario-description"></a>Descrição do cenário

A Fabrikam, Inc., uma empresa fictícia, está criando uma solução que permite que as equipes de vendas remotas armazenar e recuperar informações de contato de uma interface da web.

Os processos de gerenciamento de ciclo de vida do aplicativo (ALM) na Fabrikam, Inc. exigem a solução para ser implantado nos três ambientes de servidor em vários estágios do processo de desenvolvimento de software:

- Um ambiente de teste ou "área restrita" de desenvolvedor.
- Um ambiente de preparo baseado na intranet.
- Um ambiente de produção para a Internet.

Cada ambiente tem diferente requisitos de configuração e segurança, e cada um é difícil de implantação exclusiva.

### <a name="the-fabrikam-inc-server-infrastructure"></a>A Fabrikam, Inc. Infraestrutura de servidor

Isso é que a infra-estrutura de desenvolvimento e implantação de alto nível na Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

As estações de trabalho do desenvolvedor, infraestrutura de controle de origem, o ambiente de teste do desenvolvedor e o ambiente de preparo que residem na rede intranet dentro do domínio Fabrikam.net. O ambiente de produção reside em uma rede de perímetro (também conhecida como DMZ, zona desmilitarizada e sub-rede filtrada), que é isolada da rede intranet por um firewall. Este é um cenário de implantação comum: você normalmente isolar os servidores de web voltados para Internet da sua infraestrutura de servidor interno com o uso de firewalls ou servidores de gateway.

Neste exemplo:

- Um servidor do Team Foundation Server (TFS) 2010 com um servidor de compilação separado fornece controle de origem e a funcionalidade de CI (integração contínua).
- O ambiente de teste do desenvolvedor inclui um servidor web de serviços de informações da Internet (IIS) 7.5 e um servidor de banco de dados do SQL Server 2008 R2.
- O ambiente de produção inclui vários servidores de web IIS 7.5 sincronizados com um servidor de controlador do Web Farm Framework (WFF), junto com um servidor de banco de dados do SQL Server 2008 R2. Na prática, o servidor de banco de dados pode usar clustering ou o espelhamento para melhorar a escalabilidade e disponibilidade.
- O ambiente de preparo foi projetado para replicar a configuração do ambiente de produção mais próximo possível.
- As políticas de isolamento de rede e firewall não é possível fazer a implantação automatizada, direta da intranet para a rede de perímetro.

A configuração de cada um desses ambientes é descrita mais detalhadamente no tutorial de segundo, [configurar ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Funções da equipe do ALM

Esses usuários estão envolvidos na criação, gerenciamento, criação e a solução entre em contato com o Gerenciador de publicação:

- Matt Hink é um desenvolvedor de aplicativos web na Fabrikam, Inc. Ele faz parte da equipe que desenvolveu a solução Contact Manager usando o Visual Studio 2010. Matt tem direitos totais de administrador nos servidores no ambiente de teste de desenvolvedor, que permite configurar o ambiente para atender às suas necessidades. Ele também tem acesso de usuário à instância do Visual Studio 2010 TFS onde ele armazena o código-fonte para a solução do gerente do contato.
- Rob Walters é um administrador do servidor para a equipe de desenvolvimento da Fabrikam, Inc. Rob tem acesso administrativo no servidor do TFS para que ele possa configurar todos os aspectos do TFS e Team Build. Rob também tem acesso administrativo para os servidores de teste da web e de teste e atua como o administrador de banco de dados (DBA) para os servidores de banco de dados no teste e ambientes de preparo. Rob tiver configurado o Team Build no servidor do TFS para executar estas tarefas:

    - Compilar e executar testes de unidade do aplicativo sempre que verifica se um usuário em um arquivo para o TFS. Isso é chamado CI.
    - Implante automaticamente o aplicativo Gerenciador de contato para o ambiente de teste depois que o aplicativo passa testes de unidade. Isso inclui o banco de dados de publicação para os servidores de teste na implantação inicial e todas as atualizações para o banco de dados após a implantação inicial.
    - Implante o aplicativo Gerenciador de contato ao ambiente de preparo no processo de um única etapa.
    - Crie um pacote da Web que um administrador do servidor Web e um DBA podem usar para publicar o aplicativo no ambiente de produção.
- Lisa Andrews é responsável pela implantação de aplicativos para servidores de produção Fabrikam, Inc. um administrador de servidor. Ela tem acesso de leitura ao compartilhamento em que a equipe do TFS Build armazena o pacote de implantação da web quando ele cria o aplicativo Gerenciador de contato. Ela também tem acesso administrativo aos servidores web de produção para que ela possa implantar o aplicativo para produção. Além disso, ela atua como o DBA que implanta atualizações de banco de dados e bancos de dados para o servidor de banco de dados no ambiente de produção.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>A solução de Gerenciador de contato

A solução Contact Manager foi projetada para permitir que os usuários registrados, conectado, adicionar e editar as informações de contato por meio de uma interface da web. A solução de Contact Manager consiste em quatro projetos individuais:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Este é um projeto de aplicativo web ASP.NET MVC3 que representa o ponto de entrada para a solução. Ele oferece algumas funcionalidades de aplicativo web básico, como usuários, fornecendo a capacidade de criar e exibir detalhes de contato. O aplicativo depende de um serviço do Windows Communication Foundation (WCF) para gerenciar contatos e um banco de dados do serviços de aplicativo ASP.NET para gerenciar a autenticação e autorização.
- **ContactManager.Database**. Este é um projeto de banco de dados do Visual Studio 2010. O projeto define o esquema de banco de dados que armazena os detalhes de contato.
- **ContactManager.Service**. Este é um projeto de serviço web WCF. Expõe o WCF que criar um ponto de extremidade que permite que os chamadores executar, recuperar, atualizar e excluir operações CRUD () no banco de dados do gerente do contato. O serviço depende de um banco de dados e o assembly ContactManager.Common.dll.
- **ContactManager.Common**. Este é um projeto de biblioteca de classe. O serviço WCF depende de tipos definidos neste assembly.

Um exame completo da solução e seus requisitos de implantação é fornecido no primeiro tutorial nesta série [Web implantação na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tarefas de implantação

Há várias tarefas distintas envolvidas na implantação de aplicativos para ambientes diferentes em uma organização de grande porte. Estas são as principais tarefas que os tutoriais abordam:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Aqui está uma lista de cada etapa no processo de implantação da perspectiva dos usuários descritos anteriormente neste documento:

1. Todos os membros da equipe de análise da solução de Gerenciador de contato no Visual Studio 2010 para determinar problemas e requisitos de implantação de chave.
2. Matt Hink pode implantar a solução de Gerenciador de contato diretamente da estação de trabalho de desenvolvedor para o ambiente de teste do desenvolvedor, para realizar um teste inicial da lógica de implantação.
3. Matt Hink adiciona o aplicativo ao controle de origem no TFS.
4. Rob Walters cria várias definições de compilação para a solução de Gerenciador de contato no Team Build. Uma definição de compilação usa CI para implantar a solução para o ambiente de teste do desenvolvedor, sempre que um usuário faz check-in novo código. Outra definição de compilação permite que os usuários implantações de gatilho para o ambiente de preparo conforme necessário.
5. Sempre que um usuário faz check-in novo código, Team Build automaticamente cria os componentes da solução, executa testes de unidade e implanta a solução para o ambiente de teste do desenvolvedor se a compilação for bem-sucedida e a fase de testes de unidade.
6. Quando um usuário dispara uma implantação no ambiente de preparo, a solução é empacotada e implantada no processo de um única etapa. Esse processo também gera um pacote para implantação manual para o ambiente de produção.
7. Lisa Andrews implanta o aplicativo no ambiente de produção, importe manualmente o pacote da web criado na etapa 6.

### <a name="key-deployment-issues"></a>Problemas de implantação de chave

A solução de Gerenciador de contato e o cenário da Fabrikam, Inc. realce vários problemas comuns e desafios que você pode encontrar ao implantar complexos, soluções de nível corporativo. Por exemplo:

- Você precisa ser capaz de implantar projetos em vários ambientes, como desenvolvedor ou ambientes de teste de preparo plataformas e servidores de produção. A solução precisa ser implantada com configurações diferentes para cada ambiente.
- Você precisa implantar vários projetos dependentes simultaneamente como parte de um processo de compilação e implantação única etapa ou automatizado.
- Você precisa ser capaz de implantação de unidade em um processo automatizado. Por exemplo, você deseja usar um processo de CI para implantar aplicativos da web para um ambiente de preparo quando o novo código de check-in.
- Você precisa ser capaz de controlar o processo de implantação e definir variáveis de implantação de fora do Visual Studio, assim como os desenvolvedores provavelmente não terão as configurações corretas de configuração ou as credenciais necessárias para cada ambiente de destino.
- Você precisa implantar projetos de banco de dados baseada em esquema e preservar os dados existentes em implantações subsequentes.
- Você precisa implantar bancos de dados de associação em uma base ad hoc sem implantar dados da conta de usuário. Você também precisará atualizar o esquema de bancos de dados de associação implantado sem perda de dados de conta de usuário existente.
- Você precisará excluir determinados arquivos ou pastas quando você implanta conteúdo em vários ambientes de destino.

Além disso, gerenciamento de implantação quando as atualizações estiverem completos e incrementais frequentes lança a alguns desafios adicionais. Por exemplo:

- Executa testes de unidade toda vez que um desenvolvedor verifica no novo código. Você deseja implantar a solução, se o código passa os testes de unidade.
- Quando você implanta um aplicativo da web em um ambiente de preparo ou de produção, você deseja redirecionar os usuários para um *aplicativo\_offline.htm* arquivo durante o processo de implantação.
- Você deseja registrar atividades de implantação. O processo de implantação deve enviar notificações por email de implantações com êxito ou falhas para os destinatários.
- Se uma implantação automatizada falhar, o processo de implantação deve repetir a implantação atual ou implantar o pacote da web anterior em vez disso.

> [!div class="step-by-step"]
> [Anterior](deploying-web-applications-in-enterprise-scenarios.md)
> [Próximo](application-lifecycle-management-from-development-to-production.md)
