---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: A solução de Gerenciador de contato | Microsoft Docs
author: jrjlee
description: Esta série de tutoriais usa uma solução de exemplo&#x2014;a solução Contact Manager&#x2014;para representar um aplicativo de grande porte com um leve realista...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883694"
---
<a name="the-contact-manager-solution"></a>A solução de Gerenciador de contato
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Isso [série de tutoriais](web-deployment-in-the-enterprise.md) usa uma solução de exemplo&#x2014;a solução Contact Manager&#x2014;para representar um aplicativo de grande porte com um nível de complexidade de realista. Este tópico apresenta a solução de Gerenciador de contato, descreve os principais componentes da solução e identifica os desafios de implementar esse tipo de aplicativo para várias plataformas de destino em um ambiente corporativo.
> 
> Como trabalhar com os tópicos nos tutoriais, você pode usar a solução de Gerenciador de contato como uma implementação de referência que demonstra como você pode atender aos desafios específicos em cenários de implantação da empresa. O próximo tópico, [configuração a solução Contact Manager](setting-up-the-contact-manager-solution.md), descreve como baixar e executar a solução em sua estação de trabalho do desenvolvedor.


## <a name="solution-overview"></a>Visão geral da solução

A solução de Contact Manager consiste em quatro projetos individuais:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Este é um projeto de aplicativo web ASP.NET MVC 3 que representa o ponto de entrada para a solução. Ele oferece algumas funcionalidades de aplicativo web básico, como usuários, fornecendo a capacidade de criar e exibir detalhes de contato. O aplicativo depende de um serviço do Windows Communication Foundation (WCF) para gerenciar contatos e um banco de dados do serviços de aplicativo ASP.NET para gerenciar a autenticação e autorização.
- **ContactManager.Database**. Este é um projeto de banco de dados do Visual Studio. O projeto define o esquema de banco de dados que armazena os detalhes de contato.
- **ContactManager.Service**. Este é um projeto de serviço web WCF. O expõe de serviço WCF cria um ponto de extremidade que permite que os chamadores executar, recuperar, atualizar e excluir operações (CRUD) no **ContactManager** banco de dados. O serviço depende de **ContactManager** banco de dados e o **ContactManager.Common.dll** assembly.
- **ContactManager.Common**. Este é um projeto de biblioteca de classe. O serviço WCF depende de tipos definidos neste assembly.

A solução também inclui uma pasta de solução chamada publicar. Ele contém vários arquivos de projeto personalizados e arquivos de comando que demonstram como você pode controlar e manipular o processo de compilação e implantação. Esses são abordadas em mais detalhes posteriormente neste tutorial.

Em um nível conceitual, os componentes da solução se encaixam assim:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Enquanto o aplicativo web ASP.NET MVC 3 usa o provedor de associação do ASP.NET, todas as páginas do aplicativo da web permitem acesso anônimo. Isso claramente não é uma configuração realista. No entanto, a solução é configurada dessa forma, para tornar mais fácil para você implantar e testar a solução sem configurar funções e contas de usuário.


## <a name="deployment-challenges"></a>Desafios de implantação

A solução de Gerenciador de contato ilustra vários desafios de implantação que são comuns a vários cenários de implantação corporativa:

- A solução consiste em vários projetos dependentes. Você precisa implantar esses projetos ao mesmo tempo.
- Cadeias de caracteres de Conexão e pontos de extremidade de serviço precisam ser atualizado para cada ambiente e, em muitos casos essas informações não estará disponíveis para o desenvolvedor.
- Quando você implanta o **ContactManager** banco de dados para ambientes de preparo e produção, você precisar preservar os dados existentes em implantações subsequentes.
- Quando você implantar o banco de dados de serviços de aplicativo do ASP.NET, você precisa implantar alguns dados de configuração, mas omita quaisquer dados de conta de usuário.
- Os projetos incluem alguns arquivos e pastas que não devem ser implantadas. Você precisará excluir esses arquivos e pastas do processo de implantação.
- A solução precisa oferecer suporte à implantação automatizada de um servidor de compilação do Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusão

Este tópico fornecida uma visão geral da solução de Gerenciador de contato e identificado alguns dos desafios inerentes à implantação que são comuns a vários cenários de implantação corporativa. Os tópicos restantes neste tutorial descrevem algumas das técnicas que você pode usar para atender a esses desafios.

O próximo tópico, [configuração a solução Contact Manager](setting-up-the-contact-manager-solution.md), descreve como baixar e executar a solução em sua estação de trabalho do desenvolvedor.

> [!div class="step-by-step"]
> [Anterior](web-deployment-in-the-enterprise.md)
> [Próximo](setting-up-the-contact-manager-solution.md)
