---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: "Implantando aplicativos da Web em cenários corporativos usando o Visual Studio 2010 | Microsoft Docs"
author: jrjlee
description: "Esse conjunto de tutoriais descreve ferramentas e técnicas que você pode usar para implantar aplicativos da web em vários cenários corporativos. Explica como fazer o melhor uso..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Implantando aplicativos da Web em cenários corporativos usando o Visual Studio 2010
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Esse conjunto de tutoriais descreve ferramentas e técnicas que você pode usar para implantar aplicativos da web em vários cenários corporativos. Explica como fazer melhor uso de tecnologias como o Visual Studio 2010, a Microsoft Build Engine (MSBuild), serviços de informações da Internet (IIS) 7.5, a ferramenta de implantação da Web de IIS (implantação da Web), Web Farm Framework (WFF) e utilitários, como VSDBCMD.exe para simplificar e gerenciar o processo de implantação. Ele inclui uma visão geral conceitual e orientação e orientada a tarefas que ajudarão você a:
> 
> - Examine e estabelecer os requisitos de implantação para um aplicativo da web de grande porte.
> - Configure ambientes de servidor web teste, preparação e produção para dar suporte à implantação da web.
> - Configure os processos de CI (integração contínua) do Team Foundation Server (TFS) para dar suporte à implantação de web automatizada.
> - Implante aplicativos da web de nível corporativo para ambientes de servidor diferentes com diferentes requisitos e restrições.
> - Implante alterações em aplicativos da web que são executados em ambientes de servidor diferente.
> 
> > [!NOTE]
> > Enquanto esses tutoriais descrevem o uso do TFS, como um servidor de CI, a orientação é facilmente adaptada para qualquer servidor de CI. Não é necessário um conhecimento detalhado do TFS para compreender e aproveitar os tutoriais.
> 
> 
> Para obter uma tradução italiana destes tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Sobre os autores

Jason Lee é tecnólogo principal com [conteúdo mestre](http://www.contentmaster.com/) onde ele trabalha com produtos e tecnologias, especialmente o SharePoint e o ASP.NET, Microsoft por vários anos. Jason possui PhD na computação e está atualmente Credenciais e MCTS certificados. Você pode ler o blog de técnica de Jason em [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry é tecnólogo principal com [conteúdo mestre](http://www.contentmaster.com/) que gravou white papers, documentação do SDK, apresentações do PowerPoint e cursos online e instrutor durante sua carreiras. Um membro original da equipe de documentação do ASP.NET, ele trabalha com tecnologias web da Microsoft mais de uma década.

## <a name="target-audience"></a>Público-alvo

Esse conjunto de tutoriais é para desenvolvedores de aplicativos web ASP.NET e arquitetos de soluções que usam o Visual Studio 2010 para criar aplicativos da web de grande porte. Para obter o valor máximo do conteúdo, você deve estar familiarizado com o Visual Studio 2010 e tem uma familiaridade básica com o TFS, junto com reconhecimento de tecnologias da plataforma Microsoft web ASP.NET MVC 3, o Windows Communication Foundation (WCF), o IIS, o SQL O servidor e projetos de banco de dados do Visual Studio. No entanto, você não precisa estar familiarizado com as tecnologias e ferramentas de implantação ou precisa saber como configurar sistemas de CI.

## <a name="requirements"></a>Requisitos

Para seguir a instruções passo a passo e executar as tarefas que descrevem esses tutoriais, você precisará instalar esse software em seu computador de desenvolvimento:

- Visual Studio 2010 Premium ou Ultimate Edition com Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 com Service Pack 1
- ASP.NET MVC 3.0
- O IIS 7.5 Express
- SQL Server Express 2008 R2

Para executar as etapas de implantação descritas em todo esse passo a passo, você precisará ter acesso aos ambientes de implantação do aplicativo de Web de exemplo. Para obter melhores resultados, esses ambientes devem refletir o padrão de implantação de empresa da sua organização. Você pode modificar as orientações fornecidas nesta documentação para refletir a ambientes de implantação e os requisitos da sua organização.

## <a name="series-contents"></a>Conteúdo de série

Esta seção Introdução consiste em dois tópicos adicionais. Eles são projetados para fornecer algum contexto mais amplo para os tutoriais a seguir:

- [Implantação da Web corporativa: Visão geral do cenário](enterprise-web-deployment-scenario-overview.md). Este tópico descreve o cenário que serve de base para cada um dos tutoriais na série. O cenário se concentra nos requisitos de gerenciamento de ciclo de vida do aplicativo (ALM) de uma empresa fictícia chamada Fabrikam, Inc. como ele desenvolve um aplicativo da web de grande porte.
- [Gerenciamento de ciclo de vida do aplicativo: De desenvolvimento para produção](application-lifecycle-management-from-development-to-production.md). Este tópico fornece uma visão geral de alto nível, de ponta a ponta de um processo de implantação. Ele ilustra como a Fabrikam, Inc. se move um aplicativo web ASP.NET escala corporativa por meio de ambientes de teste, preparação e produção como parte de um processo de desenvolvimento contínuo.

A série inclui quatro conjuntos de tutorial. Cada uma se concentra nos diferentes aspectos da implantação da web:

- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual para arquivos de projeto do MSBuild, o Pipeline de publicação na Web, implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas em conjunto para gerenciar processos complexos de implantação.
- [Configurando ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar os servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o "agente remoto") ou o manipulador de implantação da Web e implantação de banco de dados remoto. Fornece orientação sobre como escolher o método de implantação apropriadas para seu próprio ambiente, e descreve como usar o WFF para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparada manualmente as implantações de compilações específicas.
- [Enterprise Web implantação avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar as implantações de banco de dados para vários ambientes, excluindo arquivos e pastas de implantação e que os aplicativos de web offline durante o processo de implantação .

## <a name="where-to-start"></a>Onde começar

Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação da empresa fictícia, para fornecer uma implementação de referência e conceder a tarefas e instruções passo a passo um contexto de comum. O próximo tópico, [implantação de Web corporativa: Visão geral do cenário](enterprise-web-deployment-scenario-overview.md), apresenta o cenário e a solução de exemplo. A partir daí, você pode trabalhar nos tutoriais e tópicos que melhor atendem às suas necessidades.

>[!div class="step-by-step"]
[Avançar](enterprise-web-deployment-scenario-overview.md)
