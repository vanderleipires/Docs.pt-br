---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Gerenciamento de ciclo de vida do aplicativo: De desenvolvimento para produção | Microsoft Docs'
author: jrjlee
description: Este tópico ilustra como uma empresa fictícia gerencia a implantação de um aplicativo da web ASP.NET por meio de ambientes de teste, preparação e produção como par...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 8beeffb374df09c6695a1845199d30006ddcc1b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="application-lifecycle-management-from-development-to-production"></a>Gerenciamento de ciclo de vida do aplicativo: De desenvolvimento para produção
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico ilustra como uma empresa fictícia gerencia a implantação de um aplicativo da web ASP.NET por meio de ambientes de teste, preparação e produção como parte de um processo de desenvolvimento contínuo. Ao longo do tópico, são fornecidos links para obter mais informações e instruções passo a passo sobre como executar tarefas específicas.
> 
> O tópico foi criado para fornecer uma visão geral de alto nível para um [série de tutoriais](deploying-web-applications-in-enterprise-scenarios.md) na implantação da web da empresa. Não se preocupe se você não estiver familiarizado com alguns dos conceitos descritos aqui&#x2014;os tutoriais a seguir fornecem informações detalhadas sobre todas essas tarefas e técnicas.
> 
> > [!NOTE]
> > ComentárioscomentáriospesquisandoO bem de simplicidade, este tópico não aborda Atualizando bancos de dados como parte do processo de implantação. No entanto, atualizações incrementais para recursos de bancos de dados é um requisito de muitos cenários de implantação corporativa, e você pode encontrar diretrizes sobre como fazer isso mais tarde na série tutorial. Para obter mais informações, consulte [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Visão Geral

O processo de implantação ilustrado aqui é baseado no cenário de implantação Fabrikam, Inc., descrito em [implantação de Web corporativa: Visão geral do cenário](enterprise-web-deployment-scenario-overview.md). Leia a visão geral do cenário antes que você estude neste tópico. Essencialmente, o cenário examina como uma organização gerencia a implantação de um aplicativo web razoavelmente complexo, o [Contact Manager solução](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), por meio de várias fases em um ambiente corporativo típico.

Em um nível alto, a solução Contact Manager passa esses estágios como parte do processo de implantação e desenvolvimento:

1. Um desenvolvedor verifica algum código no Team Foundation Server (TFS) 2010.
2. TFS compila o código e executa um teste de unidade associado ao projeto de equipe.
3. TFS implanta a solução para o ambiente de teste.
4. A equipe de desenvolvedores verifica e valida a solução no ambiente de teste.
5. O administrador do ambiente de preparo executa uma implantação "e se" para o ambiente de preparo, para estabelecer se a implantação fará com que quaisquer problemas.
6. O administrador do ambiente de preparo executa uma implantação em tempo real no ambiente de preparo.
7. A solução é submetido a aceitação do usuário de teste no ambiente de preparo.
8. Os pacotes de implantação da web são importados manualmente no ambiente de produção.

Esses estágios fazem parte de um ciclo de desenvolvimento contínuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

Na prática, o processo é um pouco mais complicado do que isso, como você verá quando vamos examinar mais detalhadamente cada etapa. A Fabrikam, Inc. usa uma abordagem diferente para implantação para cada ambiente de destino.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

O restante deste tópico examina essas chaves estágios desse ciclo de vida de implantação:

- **Pré-requisitos**: como você precisa configurar sua infraestrutura de servidor antes de colocar sua lógica de implantação em vigor.
- **Desenvolvimento e implantação inicial**: o que você precisa fazer antes de implantar sua solução pela primeira vez.
- **Implantação de teste**: como empacotar e implantar conteúdo em um ambiente de teste automaticamente quando um desenvolvedor verifica no novo código.
- **Implantação de preparo**: como implantar específico cria um ambiente de preparo e como executar "what if" implantações para garantir que uma implantação não causa problemas.
- **Implantação em produção**: como importar pacotes da web em um ambiente de produção quando a infraestrutura de rede impede a implantação remota.

## <a name="prerequisites"></a>Pré-requisitos

A primeira tarefa em qualquer cenário de implantação é garantir que sua infraestrutura de servidor atende aos requisitos da sua técnicas e ferramentas de implantação. Nesse caso, a Fabrikam, Inc. configurou sua infraestrutura de servidor assim:

- TFS está configurado para incluir uma coleção de projetos de equipe, criar controladores e agentes de compilação. Consulte [Configurando o Team Foundation Server para a implantação automatizada do Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) para obter mais informações.
- O ambiente de teste está configurado para aceitar a implantações remotas usando o serviço de agente de implantação da Web (o "agente remoto"), conforme descrito em [cenário: configurar um ambiente de teste para implantação de Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) e [ Configurar um servidor Web para publicação (agente remoto) de implantação da Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- O ambiente de preparo está configurado para aceitar a implantações remotas usando o ponto de extremidade do manipulador de implantação da Web, conforme descrito em [cenário: configurar um ambiente de preparo para a implantação da Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) e [configurar um servidor Web Publicação da implantação para Web (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- O ambiente de produção está configurado para permitir que um administrador importar manualmente os pacotes de implantação da web no Internet Information Services (IIS), conforme descrito em [cenário: configurar um ambiente de produção para implantação de Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) e [configurar um servidor Web para publicação (implantação off-line) de implantação da Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Desenvolvimento inicial e implantação

Antes que a equipe de desenvolvimento da Fabrikam, Inc. pode implantar a solução Contact Manager pela primeira vez, ele precisa executar estas tarefas:

- Crie um novo projeto de equipe no TFS.
- Crie os arquivos de projeto Microsoft Build Engine (MSBuild) que contêm a lógica de implantação.
- Crie as definições de build do TFS que disparam os processos de implantação.

### <a name="create-a-new-team-project"></a>Criar um novo projeto de equipe

- O administrador do TFS, Rob Walters, cria um novo projeto de equipe para o aplicativo, conforme descrito em [criando um projeto de equipe no TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Em seguida, o desenvolvedor de cliente potencial, Matt Hink, cria uma solução de esqueleto. Ele verifica seus arquivos para o novo projeto de equipe no TFS, conforme descrito em [adicionar conteúdo ao controle de origem](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Criar a lógica de implantação

Matt Hink cria vários arquivos personalizados de projeto do MSBuild, usando a abordagem de arquivo de projeto de divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt cria:

- Um arquivo de projeto chamado *Publish.proj* que executa o processo de implantação. Este arquivo contém destinos do MSBuild compilar projetos na solução, criar pacotes da web e implantar os pacotes em um ambiente de servidor de destino.
- Arquivos de projeto específico do ambiente denominados *Env-Dev.proj* e *Stage.proj Env*. Eles contêm configurações que são específicas para o ambiente de teste e o ambiente de preparo respectivamente, como cadeias de caracteres de conexão, pontos de extremidade de serviço e os detalhes do serviço remoto que receberão o pacote da web. Para obter orientação sobre como escolher as configurações corretas para ambientes de destino específico, consulte [configurar propriedades de implantação de um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Para executar a implantação, um usuário executa o *Publish.proj* arquivo usando MSBuild ou Team Build e especifica o local do arquivo de projeto de ambiente específicas relevantes (*Dev.proj Env* ou *Stage.proj Env*) como um argumento de linha de comando. O *Publish.proj* arquivo, em seguida, importa o arquivo de projeto específico de ambiente para criar um conjunto completo de instruções para cada ambiente de destino de publicação.

> [!NOTE]
> A maneira como esses arquivos de projeto personalizados trabalham é independente do mecanismo de que você usar para chamar o MSBuild. Por exemplo, você pode usar a linha de comando do MSBuild diretamente, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Você pode executar os arquivos de projeto de um arquivo de comando, conforme descrito em [criar e executar um arquivo de comando de implantação](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Como alternativa, você pode executar os arquivos de projeto de uma definição de compilação no TFS, conforme descrito em [criando uma definição de compilação que dá suporte à implantação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> Em cada caso, o resultado final é o mesmo&#x2014;MSBuild executa o arquivo de projeto mesclada e implanta a solução para o ambiente de destino. Isso proporciona muita flexibilidade em como você pode disparar o processo de publicação.


Depois que ele criou os arquivos de projeto personalizados, Matt adiciona a uma pasta de solução e verifica-los no controle de origem.

### <a name="create-build-definitions"></a>Criar definições de compilação

Como uma tarefa de preparação final, Matt e Rob funcionam juntos para criar três definições de compilação para o novo projeto de equipe:

- **DeployToTest**. Isso compila a solução de Gerenciador de contato e implanta para o ambiente de teste sempre que um check-in ocorre.
- **DeployToStaging**. Isso implanta recursos de um build anterior especificado para o ambiente de preparo quando um desenvolvedor de filas a compilação.
- **DeployToStaging-WhatIf**. Isso executa uma implantação "e se" para o ambiente de preparo quando um desenvolvedor de filas a compilação.

As seções a seguir fornecem mais detalhes sobre cada uma dessas definições de compilação.

## <a name="deployment-to-test"></a>Implantação de teste

A equipe de desenvolvimento na Fabrikam, Inc. mantém ambientes de teste para realizar uma variedade de atividades, como verificação de validação, testes de usabilidade, teste de compatibilidade e teste exploratório ou ad hoc de teste de software.

A equipe de desenvolvimento criou uma definição de compilação no TFS denominado **DeployToTest**. Esta definição de compilação usa um gatilho de integração contínua, o que significa que o processo de compilação é executado sempre que um membro da equipe de desenvolvimento da Fabrikam, Inc. executa um check-in. Quando uma compilação é acionada, a definição de compilação será:

- Compile a solução ContactManager.sln. Por sua vez, isso cria todos os projetos na solução.
- Execute testes de unidade na estrutura de pasta de solução (se a solução seja criada com êxito).
- Execute os arquivos de projeto personalizados que controlam o processo de implantação (se a solução é compilado com êxito e passa o teste de unidade).

O resultado final é que se a solução é compilado com êxito e passa os testes de unidade, os pacotes de web e todos os outros recursos de implantação são implantados no ambiente de teste.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Como funciona o processo de implantação?

O **DeployToTest** fontes de definição de compilação esses argumentos para o MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


O **DeployOnBuild = true** e **DeployTarget = pacote** propriedades são usadas quando o Team Build cria os projetos na solução. Quando o projeto é um projeto de aplicativo web, essas propriedades instruem o MSBuild para criar um pacote de implantação da web para o projeto. O **TargetEnvPropsFile** propriedade instrui o *Publish.proj* onde encontrar o arquivo de projeto específico de ambiente para importar de arquivo.

> [!NOTE]
> Para obter instruções detalhadas sobre como criar uma definição de compilação como este, consulte [criando uma definição de compilação que dá suporte à implantação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


O *Publish.proj* arquivo contém destinos que compila cada projeto na solução. No entanto, ele também inclui lógica condicional que ignora esses destinos de compilação se você estiver executando o arquivo no Team Build. Isso permite aproveitar a funcionalidade de compilação adicionais Team Build oferece, como a capacidade de executar testes de unidade. Se a compilação da solução ou a unidade de testes falhar, o *Publish.proj* arquivo não será executado e o aplicativo não será implantado.

A lógica condicional é feita por meio da avaliação de **BuildingInTeamBuild** propriedade. Essa é uma propriedade de MSBuild é definida automaticamente como **true** quando você usa Team Build para compilar seus projetos.

## <a name="deployment-to-staging"></a>Implantação de preparo

Quando uma compilação atende aos requisitos da equipe de desenvolvedor no ambiente de teste, a equipe talvez queira implantar na mesma compilação em um ambiente de preparo. Ambientes de preparo geralmente são configurados de acordo com as características do ambiente de produção ou "ativo" como atentamente quanto possível, por exemplo, em termos de especificações de servidor, sistemas operacionais e software e configuração de rede. Ambientes de preparo geralmente são usados para teste de carga, testes de aceitação do usuário e revisões internas mais amplas. Compilações são implantadas no ambiente de preparo diretamente do servidor de compilação.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

As definições de compilação usadas para implantar a solução ao ambiente de preparo, **DeployToStaging-WhatIf** e **DeployToStaging**, compartilham as seguintes características:

- Na verdade, eles não criam tudo. Rob implanta a solução no ambiente de preparo, ele deseja implantar uma compilação específica, existente que já foi verificada e validada no ambiente de teste. As definições de build, basta executar os arquivos de projeto personalizados que controlam o processo de implantação.
- Quando Rob dispara uma compilação, ele usa os parâmetros de compilação para especificar qual versão contém os recursos que deseja implantar a partir do servidor de compilação.
- As definições de compilação não são disparadas automaticamente. Rob filas manualmente uma compilação quando ele deseja implantar a solução no ambiente de preparo.

Este é o processo de alto nível para uma implantação no ambiente de preparo:

1. O administrador ambiente de preparo, Rob Walters, filas de um build usando o **DeployToStaging-WhatIf** definição de compilação. Rob usa os parâmetros de definição de compilação para especificar qual versão deseja implantar.
2. O **DeployToStaging-WhatIf** execuções de definição de compilação os arquivos de projeto personalizados no modo "what if". Isso gera arquivos de log como se Rob estava executando uma implantação em tempo real, mas, na verdade, não faz qualquer alteração no ambiente de destino.
3. Rob analisa os arquivos de log para determinar os efeitos da implantação no ambiente de preparo. Em particular, Rob deseja verificar o que será adicionado, o que será atualizado e o que será excluído.
4. Se for Rob satisfeito de que a implantação não faça alterações indesejáveis dados ou recursos existentes, ele enfileira um build usando o **DeployToStaging** definição de compilação.
5. O **DeployToStaging** execuções de definição de compilação os arquivos de projeto personalizados. Esses publicar os recursos de implantação no servidor web principal no ambiente de preparo.
6. O controlador do Web Farm Framework (WFF) sincroniza os servidores web no ambiente de preparo. Isso torna o aplicativo disponível em todos os servidores web no farm de servidores.

### <a name="how-does-the-deployment-process-work"></a>Como funciona o processo de implantação?

O **DeployToStaging** fontes de definição de compilação esses argumentos para o MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


O **TargetEnvPropsFile** propriedade instrui o *Publish.proj* onde encontrar o arquivo de projeto específico de ambiente para importar de arquivo. O **OutputRoot** propriedade substitui o valor interno e indica o local da pasta de compilação que contém os recursos que você deseja implantar. Quando Rob enfileira a compilação, ele usa o **parâmetros** guia para fornecer um valor atualizado para o **OutputRoot** propriedade.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Para obter mais informações sobre como criar uma definição de compilação como este, consulte [implantar uma compilação específica](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


O **DeployToStaging-WhatIf** definição de compilação contém a mesma lógica de implantação, como o **DeployToStaging** definição de compilação. No entanto, ele inclui o argumento adicional **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


Dentro de *Publish.proj* arquivo, o **WhatIf** propriedade indica que todos os recursos de implantação devem ser publicados no modo "what if". Em outras palavras, os arquivos de log são gerados como se a implantação ficassem em frente, mas na verdade, nada será alterado no ambiente de destino. Isso lhe permite avaliar o impacto de uma implantação proposto&#x2014;em particular, o que será é adicionado, o que será atualizado e o que será excluído&#x2014;antes de realmente fazer as alterações.

> [!NOTE]
> Para obter mais informações sobre como configurar implantações "what if", consulte [executando uma implantação "E se"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Depois de implantar seu aplicativo para o servidor web principal no ambiente de preparo, o WFF sincronizará automaticamente o aplicativo em todos os servidores no farm de servidores.

> [!NOTE]
> Para obter mais informações sobre como configurar o WFF para sincronizar servidores web, consulte [criar um Farm de servidores com o Framework do Web Farm](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Implantação em produção

Quando uma compilação tiver sido aprovada no ambiente de preparo, a equipe da Fabrikam, Inc. pode publicar o aplicativo no ambiente de produção. O ambiente de produção é onde o aplicativo fique "ativo" e atinge seu público-alvo de usuários finais.

O ambiente de produção está em uma rede de perímetro da Internet. Isso é isolado da rede interna que contém o servidor de compilação. O administrador de ambiente de produção, Lisa Andrews manualmente deve copiar os pacotes de implantação da web do servidor de compilação e importá-los para o IIS no servidor web de produção primário.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Este é o processo de alto nível para uma implantação no ambiente de produção:

1. A equipe de desenvolvedores avisa Lisa que uma compilação está pronta para implantação de produção. A equipe avisa Lisa do local dos pacotes de implantação da web dentro da pasta de descarte no servidor de compilação.
2. Lisa coleta os pacotes da web do servidor de compilação e os copia para o servidor web principal no ambiente de produção.
3. Lisa usa o Gerenciador do IIS para importar e publicar os pacotes da web no servidor web principal.
4. O controlador WFF sincroniza os servidores web no ambiente de produção. Isso torna o aplicativo disponível em todos os servidores web no farm de servidores.

### <a name="how-does-the-deployment-process-work"></a>Como funciona o processo de implantação?

O Gerenciador do IIS inclui um Assistente de pacote de aplicativo de importação que torna mais fácil publicar pacotes da web em um site do IIS. Para obter instruções sobre como executar esse procedimento, consulte [instalar pacotes de Web manualmente](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma ilustração do ciclo de vida de implantação para um aplicativo da web de grande porte típico.

Este tópico faz parte de uma série de tutoriais que fornecem orientação sobre vários aspectos da implantação de aplicativos web. Na prática, há várias considerações e tarefas adicionais em cada estágio do processo de implantação e não é possível cobri-los em um único passo a passo. Para obter mais informações, consulte estes tutoriais:

- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução abrangente para técnicas de implantação da web usando MSBuild e a ferramenta de implantação da Web de IIS (implantação da Web).
- [Configurando ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial fornece orientação sobre como configurar ambientes de servidor do Windows para oferecer suporte a vários cenários de implantação.
- [Configurar o Team Foundation Server para a implantação de Web automatizada](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial fornece orientação sobre como integrar a lógica de implantação em processos de compilação do TFS.
- [Enterprise Web implantação avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial fornece orientação sobre como preencher os desafios de implantação mais complexos que as organizações enfrentam.

> [!div class="step-by-step"]
> [Anterior](enterprise-web-deployment-scenario-overview.md)
