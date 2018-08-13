---
title: DevOps com ASP.NET Core e Azure
author: CamSoper
description: Um guia que fornece orientação de ponta a ponta sobre a criação de um pipeline de DevOps para um aplicativo ASP.NET Core hospedado no Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722523"
---
# <a name="devops-with-aspnet-core-and-azure"></a>DevOps com ASP.NET Core e Azure

Bem-vindo ao guia de ciclo de vida de desenvolvimento do Azure para .NET. Este guia apresenta os conceitos básicos da criação de um ciclo de vida de desenvolvimento para o Azure usando processos e ferramentas do .NET. Depois de concluí-lo, você aproveitará os benefícios de uma cadeia de ferramentas madura do DevOps.

## <a name="who-this-guide-is-for"></a>A quem esse guia se destina

Você deve ser um desenvolvedor experiente do ASP.NET (nível 200 a 300). Você não precisa saber nada sobre o Azure, uma vez que abordaremos isso nesta introdução. Esse guia também pode ser útil para engenheiros de DevOps que estão mais focados nas operações do que no desenvolvimento.

Esse guia destina-se aos desenvolvedores do Windows. No entanto, Linux e macOS são totalmente compatíveis com .NET Core. Para adaptar esse guia para Linux/macOS, fique atento a textos explicativos para diferenças do Linux/macOS.

## <a name="what-this-guide-doesnt-cover"></a>O que este guia não aborda

Esse guia se concentra em uma experiência de implantação contínua de ponta a ponta para desenvolvedores do .NET. Ele não é um guia completo para tudo relacionado ao Azure e não se concentra extensivamente em APIs .NET para serviços do Azure. A ênfase está totalmente concentrada na integração contínua, na implantação, no monitoramento e na depuração. No final do guia, são oferecidas recomendações para as próximas etapas. Serviços da plataforma do Azure que úteis para desenvolvedores do ASP.NET estão incluídos nas sugestões.

## <a name="whats-in-this-guide"></a>O que há neste guia

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Ferramentas e downloads](xref:azure/devops/tools-and-downloads)

Saiba onde obter as ferramentas usadas neste guia.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Implantar no Serviço de Aplicativo](xref:azure/devops/deploy-to-app-service)

Aprenda os vários métodos para implantar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Integração contínua e implantação](xref:azure/devops/cicd)

Crie uma solução de implantação e integração contínua de ponta a ponta para seu aplicativo ASP.NET Core com o GitHub, o VSTS e o Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Monitorar e depurar](xref:azure/devops/monitor)

Use as ferramentas do Azure para monitorar, solucionar problemas e ajustar seu aplicativo.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Próximas etapas](xref:azure/devops/next-steps)

Outros roteiros de aprendizado para desenvolvedores do ASP.NET Core que estão aprendendo sobre o Azure.

## <a name="acknowledgments"></a>Agradecimentos

Obrigado a todos na comunidade do .NET que contribuíram para esse guia com sugestões úteis. Gostaríamos de agradecer especificamente os seguintes membros da comunidade que contribuíram para a revisão final deste material:

* [Sam Wronski](https://www.youtube.com/c/worldofzerodevelopment)
* [Jeffrey Palermo](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Conclusão

Esse guia prepara você para criar um ciclo de vida de desenvolvimento de integração contínua criado em torno do ASP.NET Core e do Serviço de Aplicativo do Azure.

## <a name="additional-reading"></a>Leitura adicional

* [O que é a computação em nuvem?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Exemplos de computação em nuvem](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [O que é IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)
* [O que é PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
