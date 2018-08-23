---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configuração de permissões para o Team Build implantação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como configurar permissões para permitir que o servidor de compilação implantar conteúdo em servidores web e servidores de banco de dados como parte de um b automatizado...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0142be694a4e7d601625022f6fbfe39971823d03
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825375"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Configuração de permissões para o Team Build de implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar permissões para permitir que o servidor de compilação implantar conteúdo em servidores web e servidores de banco de dados como parte de um processo de compilação automatizado.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Quando você instala o serviço de compilação de 2010 do Team Foundation Server (TFS), você pode especificar a identidade com o qual você deseja que o serviço seja executado. Por padrão, essa é a conta de serviço de rede. Como alternativa, você pode configurar o serviço de compilação para ser executado usando uma conta de domínio.

As tarefas de implantação que exigem a autenticação do Windows e que você pretende automatizar usando o Team Build, serão executado usando a identidade de serviço de compilação. Como tal, você precisará conceder a identidade do serviço de compilação quaisquer permissões necessárias em seus servidores web e os servidores de banco de dados.

> [!NOTE]
> A conta de serviço de rede usa a conta do computador para autenticar em outros computadores. Contas de computador assumem a forma * [nome do domínio]\[nome do computador] ***$**&#x2014;, por exemplo, **FABRIKAM\TFSBUILD$**. Dessa forma, se seu serviço de compilação é executada usando a identidade do serviço de rede, você deve conceder as permissões necessárias para a identidade da conta de computador para seu servidor de compilação.


## <a name="configuring-web-server-permissions"></a>Configurando permissões do servidor Web

Conforme descrito em [escolhendo a abordagem da direita para a implantação da Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), há duas abordagens principais que você pode usar para implantar pacotes da web em um servidor web remoto:

- Implantar o aplicativo de um local remoto direcionando o *serviço de agente de implantação da Web* (também conhecido como o agente remoto) no servidor de destino.
- Implantar o aplicativo de um local remoto direcionando o *serviços de informações da Internet* (*IIS) Web implantar manipulador* no servidor de destino.

O agente remoto tem duas limitações importantes nesse caso:

- O agente remoto oferece suporte somente a autenticação NTLM. Em outras palavras, a implantação deve usar a identidade de serviço de compilação&#x2014;você não pode representar outra conta.
- Para usar o agente remoto, a conta que executa a implantação deve ser um administrador no servidor de destino.

Juntas, essas duas limitações fazem a abordagem do agente remoto indesejável para uma implantação automatizada do Team Build. Para usar essa abordagem, você precisa fazer o serviço de compilação um administrador de conta em qualquer servidor web de destino.

Por outro lado, a abordagem do manipulador de implantação da Web oferece várias vantagens:

- O manipulador de implantação da Web dá suporte à autenticação básica, via HTTPS, que permite que você passe as credenciais de uma conta alternativa para a ferramenta de implantação da Web de IIS (implantação da Web).
- Você pode configurar servidores de web de destino para permitir que os usuários não-administrador implantar conteúdo em sites específicos do IIS usando o manipulador de implantação da Web.

Como resultado, é preferível claramente para o manipulador de implantação da Web de destino quando você automatiza a implantação de pacote de web do Team Build. Este é o processo recomendado:

1. Crie uma conta de domínio com poucos privilégios para usar para a implantação.
2. Configurar o manipulador de implantação da Web e conceda à conta as permissões necessárias para implantar conteúdo para um site do IIS específico, conforme descrito em [Configurando um servidor Web para publicação de implantação do Web (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Invocar a implantação da Web e o manipulador de implantação da Web de destino, usando a autenticação básica e fornecendo as credenciais da conta de domínio que você criou, para executar a implantação.

No [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) exemplo de solução, você especifica o tipo de autenticação (básica ou NTLM), as credenciais de implantação da Web e o endereço do ponto de extremidade (agente remoto ou manipulador de implantação da Web) no arquivo de projeto específicas do ambiente. Esses valores são usados para formular e executar um comando de implantação da Web quando o arquivo de projeto é executado. Para obter mais informações, consulte [Implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Para obter mais informações sobre como configurar o manipulador implantar de Web, incluindo como configurar permissões, consulte [Configurando um servidor Web para publicação de implantação do Web (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Para obter mais informações sobre como configurar o agente remoto, consulte [Configurando um servidor Web para publicação de implantação do Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurando permissões de servidor de banco de dados

Para implantar um banco de dados do SQL Server, faça o seguinte:

- Crie um logon para a conta de implantação na instância do SQL Server.
- Concede ao logon **DBCreator** permissões na instância do SQL Server.
- Após a implantação inicial, adicione o logon para o **db\_proprietário** no banco de dados de destino. Isso é necessário porque em implantações subsequentes, você está modificando um banco de dados existente em vez de criar um novo banco de dados.

Você pode autenticar em uma instância do SQL Server usando a autenticação NTLM ou autenticação do SQL Server:

- Se você usar a autenticação NTLM, você precisa conceder as permissões descritas acima para a conta de serviço de compilação.
- Se você usar a autenticação do SQL Server, você precisa conceder as permissões descritas acima para a conta do SQL Server. Você também precisa incluir o nome de usuário do SQL Server e a senha na cadeia de conexão que você usa para implantar o banco de dados.

Para obter detalhes passo a passo sobre como configurar permissões para a implantação de banco de dados, consulte [Configurando um servidor de banco de dados para publicação de implantação na Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusão

Neste ponto, você deve compreender as permissões necessárias, junto com as opções de autenticação abertas para você, quando você automatizar implantações de aplicativo e o banco de dados da web do Team Build. Você também deve ser capaz de implementar as permissões necessárias nos servidores web IIS e servidores de banco de dados do SQL Server.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como configurar ambientes de servidor do Windows para dar suporte à implantação remota, consulte [Configurando ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](deploying-a-specific-build.md)
