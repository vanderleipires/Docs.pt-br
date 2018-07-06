---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Implantando aplicativos Web em cenários corporativos usando o Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Este conjunto de tutoriais descreve ferramentas e técnicas que você pode usar para implantar aplicativos da web em vários cenários empresariais. Ele explica como fazer melhor uso...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: b55aeb863694ca3ac71f2a1a46d11981e178dcb4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816581"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Implantando aplicativos Web em cenários corporativos usando o Visual Studio 2010
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriais descreve ferramentas e técnicas que você pode usar para implantar aplicativos da web em vários cenários empresariais. Ele explica como fazer melhor uso de tecnologias, como o Visual Studio 2010, o Microsoft Build Engine (MSBuild), serviços de informações da Internet (IIS) 7.5, a ferramenta de implantação da Web de IIS (implantação da Web), o Web Farm Framework (WFF) e utilitários, como VSDBCMD.exe para Simplifique e gerenciar o processo de implantação. Ela inclui uma visão geral conceitual e orientados a tarefas de diretrizes que ajudarão você a:
> 
> - Examine e estabelecer os requisitos de implantação para um aplicativo web de escala empresarial.
> - Configure ambientes de servidor web teste, preparação e produção para dar suporte à implantação da web.
> - Configure processos de CI (integração contínua) do Team Foundation Server (TFS) para dar suporte à implantação da web automatizados.
> - Implante aplicativos web em escala empresarial para ambientes de servidor diferentes com diferentes requisitos e restrições.
> - Implante as alterações em aplicativos da web que são executados em ambientes de servidor diferente.
> 
> > [!NOTE]
> > Embora esses tutoriais descrevem o uso do TFS como um servidor de CI, a orientação é adaptada facilmente a qualquer servidor de CI. Não é necessário um conhecimento detalhado do TFS para entender e aproveitar os tutoriais.
> 
> 
> Para obter uma tradução de italiana destes tutoriais, visite [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Sobre os autores

Jason Lee é tecnólogo com [conteúdo mestre](http://www.contentmaster.com/) em que ele tem trabalhado com tecnologias, especialmente o SharePoint e o ASP.NET e produtos da Microsoft por vários anos. Jason é PhD em computação e está atualmente MCPD e MCTS certificada. Você pode ler o blog técnico do Jason [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry é tecnólogo com [conteúdo mestre](http://www.contentmaster.com/) quem escreveu white papers, documentação do SDK, apresentações do PowerPoint e cursos de treinamento ministrado por instrutor e online durante sua carreira. Um membro original da equipe de documentação do ASP.NET, ele tem trabalhado com tecnologias web da Microsoft mais de uma década.

## <a name="target-audience"></a>Público-alvo

Esse conjunto de tutoriais é para desenvolvedores de aplicativos web ASP.NET e arquitetos de solução que usam o Visual Studio 2010 para criar aplicativos web em escala empresarial. Para obter o valor máximo do conteúdo, você deve estar familiarizado com o uso do Visual Studio 2010 e tem uma familiaridade básica com o TFS, junto com o reconhecimento das tecnologias do Microsoft web platform, como o ASP.NET MVC 3, o Windows Communication Foundation (WCF), o IIS, o SQL Servidor e projetos de banco de dados do Visual Studio. No entanto, você não precisa estar familiarizado com as tecnologias e ferramentas de implantação ou precisa saber como configurar sistemas de CI.

## <a name="requirements"></a>Requisitos

Para seguir a instruções passo a passo e executar as tarefas que descrevem esses tutoriais, você precisará instalar esse software em seu computador de desenvolvimento:

- Visual Studio 2010 Premium ou Ultimate Edition com Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 com Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Para executar as etapas de implantação descritas em toda essa explicações passo a passo, você precisará ter acesso aos ambientes de implantação do aplicativo de Web de exemplo. Para obter melhores resultados, esses ambientes devem refletir o padrão de implantação de empresa da sua organização. Em seguida, você pode modificar as orientações fornecidas nesta documentação para refletir os requisitos de sua organização e ambientes de implantação.

## <a name="series-contents"></a>Série de conteúdo

Esta seção introdutória consiste em dois tópicos adicionais. Elas foram desenvolvidas para fornecer algum contexto mais amplo para os tutoriais a seguir:

- [Implantação da Web corporativa: Visão geral do cenário](enterprise-web-deployment-scenario-overview.md). Este tópico descreve o cenário que serve de base para cada um dos tutoriais desta série. O cenário se concentra nos requisitos de gerenciamento de ciclo de vida de aplicativos (ALM) de uma empresa fictícia chamada Fabrikam, Inc. como ele desenvolve um aplicativo web de escala empresarial.
- [Gerenciamento de ciclo de vida do aplicativo: Do desenvolvimento à produção](application-lifecycle-management-from-development-to-production.md). Este tópico fornece uma visão geral de alto nível, de ponta a ponta de um processo de implantação. Ele ilustra como a Fabrikam, Inc. move um aplicativo web ASP.NET escala empresarial por meio de ambientes de teste, preparação e produção como parte de um processo de desenvolvimento contínuo.

A série inclui quatro conjuntos de tutorial. Cada um se concentra nos diferentes aspectos da implantação da web:

- [Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual ao arquivos de projeto MSBuild, o Pipeline de publicação na Web, implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas juntos para gerenciar processos de implantação complexa.
- [Configurar ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores do Windows para dar suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o "agente remoto") ou o manipulador de implantação da Web e a implantação de banco de dados remoto. Ele fornece orientações sobre como escolher o método de implantação apropriadas para seu próprio ambiente e descreve como usar o WFF para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.
- [Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparado manualmente as implantações de compilações específicas.
- [Avançadas de implantação da Web corporativa](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar diversas tarefas de implantação mais avançadas, como personalizar as implantações de banco de dados para vários ambientes, excluindo arquivos e pastas de implantação e colocar aplicativos web em offline durante o processo de implantação .

## <a name="where-to-start"></a>Por onde começar

Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação da empresa fictícia, para fornecer uma implementação de referência e conceder a tarefas e instruções passo a passo um contexto comuns. Próximo tópico [implantação da Web corporativa: Visão geral do cenário](enterprise-web-deployment-scenario-overview.md), apresenta o cenário e a solução de exemplo. A partir daí, você pode trabalhar por meio de tutoriais e tópicos que melhor corresponder às suas necessidades.

> [!div class="step-by-step"]
> [Avançar](enterprise-web-deployment-scenario-overview.md)
