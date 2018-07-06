---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: A solução de Gerenciador de contatos | Microsoft Docs
author: jrjlee
description: Esta série de tutoriais usa uma solução de exemplo&#x2014;a solução do Contact Manager&#x2014;para representar um aplicativo de escala empresarial com uma leve realista...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: c8044c37738e9d23ca83648a6b571059338dc1a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835153"
---
<a name="the-contact-manager-solution"></a>A solução de Gerenciador de contatos
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Isso [série de tutoriais](web-deployment-in-the-enterprise.md) usa uma solução de exemplo&#x2014;a solução do Contact Manager&#x2014;para representar um aplicativo de escala empresarial com um nível de complexidade de realista. Este tópico apresenta a solução do Contact Manager, descreve os principais componentes da solução e identifica os desafios de implantação desse tipo de aplicativo para várias plataformas de destino em um ambiente corporativo.
> 
> Como trabalhar com os tópicos nesses tutoriais, você pode usar a solução de Gerenciador de contatos como uma implementação de referência que demonstra como você pode atender aos desafios específicos em cenários de implantação do enterprise. Próximo tópico [configuração de solução de Gerenciador de contatos a](setting-up-the-contact-manager-solution.md), descreve como baixar e executar a solução em sua estação de trabalho do desenvolvedor.


## <a name="solution-overview"></a>Visão geral da solução

A solução do Contact Manager consiste em quatro projetos individuais:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Isso é um projeto de aplicativo web ASP.NET MVC 3 que representa o ponto de entrada para a solução. Ele oferece algumas funcionalidades do aplicativo web básico, como fornecer aos usuários a capacidade de criar e exibir detalhes de contato. O aplicativo se baseia em um serviço do Windows Communication Foundation (WCF) para gerenciar contatos e um banco de dados do serviços de aplicativo ASP.NET para gerenciar a autenticação e autorização.
- **ContactManager.Database**. Este é um projeto de banco de dados do Visual Studio. O projeto define o esquema para um banco de dados que armazena os detalhes de contato.
- **ContactManager.Service**. Isso é um projeto de serviço web WCF. O expõe de serviço do WCF cria um ponto de extremidade que permite que os chamadores executar, recuperar, atualizar e excluir operações (CRUD Create) a **ContactManager** banco de dados. O serviço depende de **ContactManager** banco de dados e o **ContactManager.Common.dll** assembly.
- **ContactManager.Common**. Este é um projeto de biblioteca de classe. O serviço WCF se baseia em tipos definidos nesse assembly.

A solução também inclui uma pasta de solução chamada publicar. Isso contém vários arquivos de projeto personalizado e arquivos de comando que demonstram como você pode controlar e manipular o processo de compilação e implantação. Estes são abordados em mais detalhes posteriormente neste tutorial.

Em um nível conceitual, os componentes da solução se encaixam como este:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Enquanto o aplicativo web ASP.NET MVC 3 usa o provedor de associação do ASP.NET, todas as páginas dentro do aplicativo web permitem o acesso anônimo. Isso claramente não é uma configuração realista. No entanto, a solução é configurada dessa forma, para tornar mais fácil para você implantar e testar a solução sem configurar funções e contas de usuário.


## <a name="deployment-challenges"></a>Desafios de implantação

A solução de Gerenciador de contatos ilustra vários desafios de implantação que são comuns a muitos dos cenários de implantação do enterprise:

- A solução consiste em vários projetos dependentes. Você precisa implantar estes projetos ao mesmo tempo.
- Cadeias de caracteres de Conexão e pontos de extremidade de serviço precisam ser atualizados para cada ambiente e, em muitos casos essa informação não estará disponível para o desenvolvedor.
- Quando você implanta o **ContactManager** banco de dados para ambientes de preparo e produção, você precisa preservar os dados existentes nas implantações subsequentes.
- Quando você implanta o banco de dados de serviços de aplicativo do ASP.NET, você precisará implantar alguns dados de configuração, mas omita quaisquer dados de conta de usuário.
- Os projetos incluem alguns arquivos e pastas que não devem ser implantadas. Você precisará excluir esses arquivos e pastas do processo de implantação.
- A solução precisa dar suporte à implantação automatizada de um servidor de compilação do Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral da solução Gerenciador de contatos e identificado alguns dos desafios inerentes à implantação que são comuns a muitos dos cenários de implantação do enterprise. Os tópicos restantes neste tutorial descrevem algumas das técnicas que você pode usar para atender a esses desafios.

Próximo tópico [configuração de solução de Gerenciador de contatos a](setting-up-the-contact-manager-solution.md), descreve como baixar e executar a solução em sua estação de trabalho do desenvolvedor.

> [!div class="step-by-step"]
> [Anterior](web-deployment-in-the-enterprise.md)
> [Próximo](setting-up-the-contact-manager-solution.md)
