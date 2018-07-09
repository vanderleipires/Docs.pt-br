---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Implantação da Web corporativa: Visão geral do cenário | Microsoft Docs'
author: jrjlee
description: Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação da empresa fictícia, para fornecer uma ref...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ce533b246b14c9e041c89a778c022784f09d01a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836740"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Implantação da Web corporativa: Visão geral do cenário
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação da empresa fictícia, para fornecer uma implementação de referência e conceder a tarefas e instruções passo a passo um contexto comuns. Este tópico descreve o cenário do tutorial e apresenta a solução de exemplo.


## <a name="scenario-description"></a>Descrição do cenário

A Fabrikam, Inc., uma empresa fictícia, está criando uma solução que permite que as equipes de vendas remotas armazenar e recuperar informações de contato de uma interface da web.

Os processos de gerenciamento de ciclo de vida de aplicativos (ALM) na Fabrikam, Inc. exigem a solução ser implantada em três ambientes de servidor em vários estágios do processo de desenvolvimento de software:

- Um ambiente de teste ou "área restrita" de desenvolvedor.
- Um ambiente de preparo baseado na intranet.
- Um ambiente de produção para a Internet.

Cada um desses ambientes tem diferente requisitos de configuração e segurança, e cada um apresenta desafios de implantação exclusivo.

### <a name="the-fabrikam-inc-server-infrastructure"></a>A Fabrikam, Inc. Infraestrutura de servidor

Isso é a infraestrutura de desenvolvimento e implantação de alto nível da Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

As estações de trabalho do desenvolvedor, a infra-estrutura de controle do código-fonte, o ambiente de teste do desenvolvedor e o ambiente de preparo que residem na rede intranet dentro do domínio Fabrikam.net. O ambiente de produção reside em uma rede de perímetro (também conhecida como DMZ, zona desmilitarizada e sub-rede filtrada), que é isolada da rede intranet por um firewall. Esse é um cenário de implantação comum: você normalmente isolar seus servidores da web para a Internet de sua infraestrutura de servidor interno com o uso de firewalls ou servidores de gateway.

Neste exemplo:

- Um servidor do Team Foundation Server (TFS) 2010 com um servidor de compilação separado fornece controle de origem e a funcionalidade de CI (integração contínua).
- O ambiente de teste do desenvolvedor inclui um servidor web de serviços de informações da Internet (IIS) 7.5 e um servidor de banco de dados do SQL Server 2008 R2.
- O ambiente de produção inclui vários servidores de web IIS 7.5 sincronizados por um servidor de controlador do Web Farm Framework (WFF), junto com um servidor de banco de dados do SQL Server 2008 R2. Na prática, o servidor de banco de dados pode usar clustering ou o espelhamento para melhorar a escalabilidade e disponibilidade.
- O ambiente de preparo foi projetado para replicar a configuração do ambiente de produção mais próximo possível.
- As políticas de isolamento de rede e firewall não permitem a implantação direta e automatizada da intranet para a rede de perímetro.

A configuração de cada um desses ambientes é descrita mais detalhadamente no segundo tutorial [Configurando ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Funções de equipe para o ALM

Esses usuários estão envolvidos na criação, gerenciar, criar e publicar a solução de Gerenciador de contatos:

- Matt Hink é um desenvolvedor de aplicativo da web na Fabrikam, Inc. Ele faz parte da equipe que desenvolveu a solução do Contact Manager usando o Visual Studio 2010. Matt tem direitos totais de administrador nos servidores em que o ambiente de teste do desenvolvedor, que permite a configurar o ambiente para atender às suas necessidades. Ele também tem acesso de usuário à instância do Visual Studio 2010 TFS em que ele armazena o código-fonte para a solução de Gerenciador de contatos.
- Rob Walters é um administrador do servidor para a equipe de desenvolvimento Fabrikam, Inc. Rob tem acesso administrativo no servidor do TFS para que ele possa configurar todos os aspectos do TFS e Team Build. Rob também tem acesso administrativo para o teste e os servidores de web de preparo e atua como o administrador de banco de dados (DBA) para os servidores de banco de dados no teste e ambientes de preparo. Rob tiver configurado o Team Build no servidor do TFS para executar estas tarefas:

    - Compilar e executar testes de unidade no aplicativo, sempre que verifica se um usuário em um arquivo para o TFS. Isso é chamado de CI.
    - Implante o aplicativo Contact Manager ao ambiente de teste automaticamente depois que o aplicativo aprovado em testes de unidade. Isso inclui o banco de dados de publicação para os servidores de teste na implantação inicial e todas as atualizações para o banco de dados após a implantação inicial.
    - Implante o aplicativo Contact Manager ao ambiente de preparo em um processo passo a passo.
    - Crie um pacote da Web que um administrador do servidor Web e um DBA podem usar para publicar o aplicativo no ambiente de produção.
- Lisa Andrews é responsável pela implantação de aplicativos para servidores de produção Fabrikam, Inc. um administrador do servidor. Ela tem acesso de leitura ao compartilhamento em que o TFS Team Build armazena o pacote de implantação da web depois que ele cria o aplicativo Gerenciador de contatos. Ela também tem acesso administrativo aos servidores web de produção para que ela possa implantar o aplicativo em produção. Além disso, ela atua como o DBA que implanta atualizações de banco de dados e bancos de dados para o servidor de banco de dados no ambiente de produção.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>A solução de Gerenciador de contatos

A solução de Gerenciador de contatos é projetada para permitir que os usuários registrados, conectado, adicionar e editar as informações de contato por meio de uma interface da web. A solução do Contact Manager consiste em quatro projetos individuais:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Isso é um projeto de aplicativo web do ASP.NET MVC3 que representa o ponto de entrada para a solução. Ele oferece algumas funcionalidades do aplicativo web básico, como fornecer aos usuários a capacidade de criar e exibir detalhes de contato. O aplicativo se baseia em um serviço do Windows Communication Foundation (WCF) para gerenciar contatos e um banco de dados do serviços de aplicativo ASP.NET para gerenciar a autenticação e autorização.
- **ContactManager.Database**. Este é um projeto de banco de dados do Visual Studio 2010. O projeto define o esquema para um banco de dados que armazena os detalhes de contato.
- **ContactManager.Service**. Isso é um projeto de serviço web WCF. Expõe o WCF que cria um ponto de extremidade que permite que os chamadores executar, recuperar, atualizar e excluir (CRUD) no banco de dados do Gerenciador de contatos. O serviço depende do banco de dados do Gerenciador de contatos e o assembly ContactManager.Common.dll.
- **ContactManager.Common**. Este é um projeto de biblioteca de classe. O serviço WCF se baseia em tipos definidos nesse assembly.

Uma revisão completa da solução e seus requisitos de implantação é fornecida no primeiro tutorial nesta série [implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tarefas de implantação

Há várias tarefas distintas envolvidas na implantação de aplicativos em ambientes diferentes em uma grande organização. Estas são as principais tarefas que os tutoriais abordam:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Aqui está uma lista de cada etapa no processo de implantação da perspectiva dos usuários descrito anteriormente neste documento:

1. Todos os membros da equipe de examinar a solução do Gerenciador de contatos no Visual Studio 2010 para determinar problemas e requisitos de implantação da chave.
2. Matt Hink pode implantar a solução de Gerenciador de contatos diretamente da estação de trabalho de desenvolvedor para o ambiente de teste do desenvolvedor, conduzir um teste inicial da lógica de implantação.
3. Matt Hink adiciona o aplicativo ao controle de origem no TFS.
4. Rob Walters cria várias definições de compilação para a solução de Gerenciador de contatos no Team Build. Uma definição de compilação usa CI para implantar a solução para o ambiente de teste do desenvolvedor, sempre que verifica se um usuário no novo código. Outra definição de compilação permite implantações de gatilho de usuários ao ambiente de preparo conforme necessário.
5. Sempre que um usuário faz check-in do novo código, Team Build automaticamente cria os componentes da solução, executa testes de unidade e implanta a solução para o ambiente de teste do desenvolvedor se a compilação foi bem-sucedida e os testes de unidade aprovados.
6. Quando um usuário dispara uma implantação para o ambiente de preparo, a solução é empacotada e implantada em um processo passo a passo. Esse processo também gera um pacote para implantação manual para o ambiente de produção.
7. Lisa Andrews implanta o aplicativo no ambiente de produção, importe manualmente o pacote da web criado na etapa 6.

### <a name="key-deployment-issues"></a>Problemas de implantação de chave

A solução de Gerenciador de contatos e o cenário do Fabrikam, Inc. realçar vários problemas comuns e desafios que você pode encontrar quando você implanta complexo, soluções de escala empresarial. Por exemplo:

- Você precisa ser capaz de implantar projetos em vários ambientes, como desenvolvedor ou ambientes de teste de preparação plataformas e servidores de produção. A solução precisa ser implantado com configurações diferentes para cada ambiente.
- Você precisará implantar vários projetos dependentes simultaneamente como parte de um processo de compilação e implantação passo a passo ou automatizado.
- Você precisa ser capaz de implantação de unidade de um processo automatizado. Por exemplo, você deseja usar um processo de CI para implantar aplicativos da web para um ambiente de preparo quando o novo código é verificado.
- Você precisa ser capaz de controlar o processo de implantação e definir variáveis de implantação de fora do Visual Studio, como os desenvolvedores podem não ter as definições de configuração correto ou as credenciais necessárias para cada ambiente de destino.
- Você precisa implantar projetos com base no esquema de banco de dados e preservar os dados existentes nas implantações subsequentes.
- Você precisa implantar bancos de dados de associação em uma base ad hoc sem implantar dados da conta de usuário. Talvez você também precise atualizar o esquema de bancos de dados de associação implantado sem perda de dados de conta de usuário existente.
- Você precisará excluir determinados arquivos ou pastas quando você implanta conteúdo em vários ambientes de destino.

Além disso, o gerenciamento de implantação quando as atualizações são incrementais e frequentes levanta alguns desafios adicionais. Por exemplo:

- Você executar testes de unidade toda vez que um desenvolvedor faz check-in do novo código. Você deseja implantar a solução, se o código passa os testes de unidade.
- Quando você implanta um aplicativo web em um ambiente de preparo ou produção, você deseja redirecionar os usuários para um *app\_offline.htm* arquivo durante o processo de implantação.
- Você deseja fazer logon atividades de implantação. O processo de implantação deve enviar notificações por email de implantações com êxito ou falhas para destinatários designados.
- Se uma implantação automatizada falhar, o processo de implantação deve tentar novamente a implantação atual ou implantar o pacote da web anterior em vez disso.

> [!div class="step-by-step"]
> [Anterior](deploying-web-applications-in-enterprise-scenarios.md)
> [Próximo](application-lifecycle-management-from-development-to-production.md)
