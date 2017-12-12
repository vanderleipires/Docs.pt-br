---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: "Integração contínua e fornecimento contínuo (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs"
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 0af5f7e841bb43fa41fa0daa4ad8d59ee0596404
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integração contínua e fornecimento contínuo (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Os dois primeiros recomendado padrões do processo de desenvolvimento foram [automatizar tudo](automate-everything.md) e [controle de origem](source-control.md), e o terceiro padrão do processo combina-os. Integração contínua (CI) significa que, sempre que um desenvolvedor verifica no código para o repositório de origem, uma compilação é acionada automaticamente. Fornecimento contínuo (CD) vai adicional: depois de uma compilação e testes de unidade automatizados forem bem-sucedidas, você implanta automaticamente o aplicativo em um ambiente onde você pode fazer o teste mais detalhado.

A nuvem permite minimizar o custo de manter um ambiente de teste, porque você paga apenas para os recursos do ambiente desde que você está usando. O processo de CD pode configurar o ambiente de teste quando precisar dele e você pode executar o ambiente quando terminar testes.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Integração contínua e fornecimento contínuo de fluxo de trabalho

Em geral, recomendamos que você faça a entrega contínua para o desenvolvimento e ambientes de preparo. A maioria das equipes, mesmo na Microsoft, exigem um processo manual de revisão e aprovação para implantação de produção. Para um de produção a implantação, que talvez você queira verificar se ele ocorre quando chave pessoas da equipe de desenvolvimento estão disponíveis para o suporte, ou durante os períodos de tráfego baixo. Mas não há nada para impedir a automatizar completamente a seus ambientes de desenvolvimento e teste para que um desenvolvedor precisa fazer check-in de uma alteração e um ambiente está configurado para testes de aceitação.

O diagrama a seguir de [um Microsoft Patterns and Practices livro eletrônico sobre a entrega contínua](http://aka.ms/ReleasePipeline) ilustra um fluxo de trabalho típico. Clique na imagem para vê-lo em tamanho completo em seu contexto original.

[![Fluxo de trabalho de entrega contínua](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/en-us/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Como a nuvem permite econômico CI e CD

Automatizando esses processos no Azure é fácil. Como você está executando tudo na nuvem, você não precisa comprar ou gerenciar servidores para suas compilações ou seus ambientes de teste. E você não precisa esperar um servidor esteja disponível para seus testes no. Com todas as compilações que fazer, você poderia criar um ambiente de teste no Azure usando o script de automação, testes de aceitação de execução ou mais testes detalhados em relação a ela e, em seguida, quando você terminar apenas subdividi-lo. E se você executar o servidor somente de 2 horas ou 8 horas ou um dia, a quantidade de dinheiro que você precisa pagar por ele é mínima, porque você está pagando somente durante o tempo que uma máquina está realmente em execução. Por exemplo, o ambiente necessário para a correção aplicativo basicamente custa cerca de 1 centavos por hora se ir um nível acima do nível livre. No decorrer de um mês, se você executou o ambiente de hora somente por vez, seu ambiente de teste será provavelmente custam menos do que um expresso que você comprar na Starbucks.

## <a name="visual-studio-team-services-vsts"></a>VSTS (Visual Studio Team Services)

VSTS fornece uma série de recursos para ajudá-lo com o desenvolvimento de aplicativos de planejamento de implantação.

- Ele suporta Git (distribuído) e controle de origem TFVC (centralizado).
- Ele oferece um serviço de compilação Elástico, que significa que ele cria servidores de compilação quando forem necessários dinamicamente e leva para baixo quando ele terminar. Pode automaticamente disparar uma compilação quando alguém faz check-in de alterações de código de origem, e você não precisa ter alocar e pagar pelos seus próprios servidores de compilação ficam ociosos na maioria das vezes. O serviço de compilação é gratuito, desde que não exceda um determinado número de compilações. Se você pretende fazer um alto volume de compilações, você pode pagar um pouco mais para servidores de compilação reservado.
- Ele dá suporte à entrega contínua para o Azure.
- Ele oferece suporte a testes de carga automatizada. Teste de carga é essencial para um aplicativo de nuvem, mas geralmente é inativa até que seja muito tarde. Teste de carga simula o uso intenso de um aplicativo por milhares de usuários, permitindo que você encontrar afunilamentos e melhorar a taxa de transferência — antes de liberar o aplicativo para produção.
- Ele dá suporte a colaboração da sala da equipe, que facilita a comunicação em tempo real e colaboração para pequenas equipes agile.
- Ele dá suporte a gerenciamento de projeto agile.


Para obter mais informações sobre a integração contínua e recursos de entrega do VSTS, consulte [do Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Se você estiver procurando um gerenciamento de projeto turnkey, colaboração em equipe e solução de controle de origem, confira VSTS. O serviço é livre para até 5 usuários, e você pode se inscrever para ele no [do Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Resumo

Os padrões de desenvolvimento de três nuvem primeiro tem sido sobre como implementar um processo de desenvolvimento repetível, confiável e previsível com o tempo de ciclo de baixa. No [próximo capítulo](web-development-best-practices.md) começar a examinar padrões de arquitetura e codificação.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte [implantar um aplicativo web no serviço de aplicativo do Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-deploy/).

Consulte também os seguintes recursos:

- [Criando um Pipeline de versão com o Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Livro eletrônico, laboratórios práticos e código de exemplo Microsoft Patterns e práticas recomendadas, fornece uma introdução detalhada para fornecimento contínuo. Abrange o uso do Visual Studio Lab Management e o gerenciamento de versão do Visual Studio.
- [ALM Rangers DevOps ferramentas e diretrizes](https://aka.ms/vsarsolutions/). ALM Rangers introduziu o DevOps Workbench exemplo complementares de solução e orientação prática em colaboração com os padrões &amp; catálogo práticas *criando um Pipeline de versão com o TFS 2012*, como uma ótima maneira de iniciar aprender os conceitos de DevOps &amp; Release Management para TFS 2012 e iniciar os pneus. O guia mostra como compilar uma vez e implantar em vários ambientes.
- [Testando para entrega contínua com o Visual Studio 2012](https://msdn.microsoft.com/en-us/library/jj159345.aspx). Livro eletrônico Microsoft Patterns e práticas recomendadas, explica como integrar o teste automatizado com o fornecimento contínuo.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Código-fonte para uma ferramenta projetada para capturar uma compilação do TFS (com base em um rótulo), compilá-lo, empacotá-lo, permitir que alguém na função DevOps configurar aspectos específicos e enviar por push para o Azure. A ferramenta controla o processo de implantação para permitir operações de "reverter" para uma versão previamente implantada. A ferramenta não tem dependências externas e pode funcionar autônomo usando APIs do TFS e o SDK do Azure.
- [Entrega contínua: Versões de Software confiável com por meio de compilação, teste e automação de implantação](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Catálogo por Jez humilde.
- [Liberá-lo! Criar e implantar o Software pronto para produção](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Catálogo por Michael T. Nygard.

>[!div class="step-by-step"]
[Anterior](source-control.md)
[Próximo](web-development-best-practices.md)
