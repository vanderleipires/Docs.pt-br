---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurar permissões para a equipe de implantação da compilação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como configurar permissões para ativar o servidor de compilação implantar conteúdo em servidores web e servidores de banco de dados como parte de um b automatizado...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890275"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Configurar permissões para a equipe de implantação da compilação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar permissões para ativar o servidor de compilação implantar conteúdo em servidores web e servidores de banco de dados como parte de um processo de compilação automatizado.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Quando você instala o serviço de compilação de 2010 do Team Foundation Server (TFS), você pode especificar a identidade com a qual você deseja que o serviço seja executado. Por padrão, essa é a conta de serviço de rede. Como alternativa, você pode configurar o serviço de compilação para ser executado usando uma conta de domínio.

As tarefas de implantação que exigem a autenticação do Windows e que você pretende automatizar usando o Team Build, serão executado usando a identidade de serviço de compilação. Como tal, você precisará conceder a identidade de serviço de compilação quaisquer permissões necessárias nos servidores web e os servidores de banco de dados.

> [!NOTE]
> A conta de serviço de rede usa a conta do computador para autenticar em outros computadores. Contas de computador assumem a forma * [nome de domínio]\[nome do computador] ***$**&#x2014;por exemplo, **FABRIKAM\TFSBUILD$**. Dessa forma, se seu serviço de compilação é executado usando a identidade do serviço de rede, você deverá conceder todas as permissões necessárias para a identidade da conta de máquina para o servidor de compilação.


## <a name="configuring-web-server-permissions"></a>Configurando permissões do servidor Web

Conforme descrito em [optar pela abordagem da direita para a implantação da Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), há duas abordagens principais que você pode usar para implantar pacotes da web em um servidor web remoto:

- Implantar o aplicativo de um local remoto direcionando o *o serviço de agente de implantação Web* (também conhecido como o agente remoto) no servidor de destino.
- Implantar o aplicativo de um local remoto direcionando o *Internet Information Services* (*IIS) Web implantar manipulador* no servidor de destino.

O agente remoto tem duas limitações de chave nesse caso:

- O agente remoto oferece suporte somente a autenticação NTLM. Em outras palavras, a implantação deve usar a identidade do serviço de compilação&#x2014;não é possível representar outra conta.
- Para usar o agente remoto, a conta que executa a implantação deve ser um administrador no servidor de destino.

Juntas, essas duas limitações tornar a abordagem de agente remoto indesejável para uma implantação automatizada Team Build. Para usar essa abordagem, você precisa fazer com que o serviço de compilação conta um administrador em qualquer servidor da web de destino.

Por outro lado, a abordagem de manipulador de implantação da Web oferece várias vantagens:

- O manipulador de implantação da Web dá suporte à autenticação básica via HTTPS, o que permite que você passe as credenciais de uma conta alternativa para a ferramenta de implantação da Web de IIS (implantação da Web).
- Você pode configurar servidores de web de destino para permitir que usuários não administradores implantar conteúdo em sites específicos do IIS usando o manipulador de implantação da Web.

Como resultado, é preferível claramente o manipulador de implantação da Web de destino quando você automatizar a implantação do pacote da web do Team Build. Este é o processo recomendado:

1. Crie uma conta de domínio com poucos privilégios a ser usado para a implantação.
2. Configurar o manipulador de implantação da Web e conceda à conta as permissões necessárias para implantar conteúdo para um site do IIS específico, conforme descrito em [Configurando um servidor Web para publicação de implantar do Web (Web implantar manipulador)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Invocar a implantação da Web e o manipulador de implantação da Web de destino, usando a autenticação básica e fornecer as credenciais da conta de domínio é criado, para executar a implantação.

No [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) exemplo de solução, você especificar o tipo de autenticação (básica ou NTLM), as credenciais de implantação da Web e o endereço de ponto de extremidade (agente remoto ou manipulador de implantação da Web) no arquivo de projeto específico do ambiente. Esses valores são usados para formular e executar um comando de implantação da Web quando o arquivo de projeto é executado. Para obter mais informações, consulte [Implantando pacotes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Para obter mais informações sobre como configurar o manipulador implantar de Web, incluindo como definir permissões, consulte [Configurando um servidor Web para publicação de implantar do Web (Web implantar manipulador)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Para obter mais informações sobre como configurar o agente remoto, consulte [Configurando um servidor Web para publicação de implantação do Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurar permissões de servidor de banco de dados

Para implantar um banco de dados do SQL Server, você deve:

- Crie um logon para a conta de implantação na instância do SQL Server.
- Concede ao logon **DBCreator** permissões na instância do SQL Server.
- Após a implantação inicial, adicione o logon para o **db\_proprietário** no banco de dados de destino. Isso é necessário porque em implantações subsequentes, você está modificando o banco de dados existente em vez de criar um novo banco de dados.

Você pode autenticar para uma instância do SQL Server usando a autenticação NTLM ou autenticação do SQL Server:

- Se você usar a autenticação NTLM, você precisa conceder as permissões descritas acima para a conta de serviço de compilação.
- Se você usar a autenticação do SQL Server, você precisa conceder as permissões descritas acima para a conta do SQL Server. Você também precisa incluir o nome de usuário do SQL Server e a senha na cadeia de conexão usada para implantar o banco de dados.

Para obter detalhes passo a passo sobre como configurar permissões para a implantação de banco de dados, consulte [Configurando um servidor de banco de dados para publicação de implantação da Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusão

Neste ponto, você deve compreender as permissões necessárias, junto com as opções de autenticação abertas para você, quando você automatizar as implantações de aplicativo e banco de dados da web do Team Build. Você também deve ser capaz de implementar as permissões necessárias nos servidores web do IIS e servidores de banco de dados do SQL Server.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como configurar ambientes de servidor do Windows para dar suporte à implantação remota, consulte [configurar ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](deploying-a-specific-build.md)
