---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integração contínua e entrega contínua (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 40687c490fdfb7764ac9ca8af6fffd054362d552
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819402"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integração contínua e entrega contínua (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


As duas primeiras recomendado padrões do processo de desenvolvimento foram [automatizar tudo](automate-everything.md) e [controle do código-fonte](source-control.md), e o terceiro padrão do processo combina-os. Integração contínua (CI) significa que, sempre que um desenvolvedor verifica no código para o repositório de origem, uma compilação é disparada automaticamente. Entrega contínua (CD) leva isso um passo além: depois que uma compilação e testes de unidade automatizados forem bem-sucedidas, você deseja implantar automaticamente o aplicativo para um ambiente onde você pode fazer o teste mais detalhado.

A nuvem permite minimizar o custo de manter um ambiente de teste, porque você paga apenas pelos recursos de ambiente desde que você está usando-os. O processo de CD pode configurar o ambiente de teste quando você precisa dele e pode desativar o ambiente quando você terminar de teste.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Fluxo de trabalho de integração contínua e entrega contínua

Em geral, recomendamos que você faça a entrega contínua para seu desenvolvimento e ambientes de preparo. A maioria das equipes, até mesmo na Microsoft, exigem um processo de revisão e aprovação manual para implantação de produção. Para uma produção implantação, que talvez você queira garantir que ele acontece quando principais pessoas com quem na equipe de desenvolvimento estão disponíveis para o suporte, ou durante os períodos de baixo tráfego. Mas não há nada para impedir que você automatizar completamente seus ambientes de desenvolvimento e teste para que tudo o que um desenvolvedor precisa fazer é verificar em um ambiente e uma alteração é configurado para o teste de aceitação.

O diagrama a seguir da [um Microsoft Patterns and Practices livro eletrônico sobre a entrega contínua](http://aka.ms/ReleasePipeline) ilustra um fluxo de trabalho típico. Clique na imagem para vê-la em tamanho completo em seu contexto original.

[![Fluxo de trabalho de entrega contínua](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Como a nuvem permite econômica CI e CD

Automatizar esses processos no Azure é fácil. Porque você está executando tudo na nuvem, você não precisa comprar ou gerenciar servidores para seus ambientes de teste ou de suas compilações. E você não precise esperar por um servidor esteja disponível para fazer o teste no. Com cada compilação que você faz, você poderia acelerar um ambiente de teste no Azure usando o script de automação, testes de aceitação de execução ou mais testes detalhados em relação a ela e, em seguida, quando você terminar apenas subdividi-lo. E se você executar o servidor somente por 2 horas ou 8 horas ou um dia, a quantidade de dinheiro que você precisa pagar por ele é mínima, porque você está pagando apenas para a hora em que um computador está realmente em execução. Por exemplo, o ambiente necessário para a correção de aplicativo basicamente custa aproximadamente US $ 1 por hora se ir um nível acima do nível gratuito. No decorrer de um mês, se você executou somente o ambiente de uma hora por vez, seu ambiente de teste seria provavelmente custam menos do que um café com leite que você compra na Starbucks.

## <a name="visual-studio-team-services-vsts"></a>VSTS (Visual Studio Team Services)

O VSTS fornece uma série de recursos para ajudá-lo com o desenvolvimento de aplicativos desde o planejamento de implantação.

- Ele dá suporte a Git (distribuído) e controle de origem TFVC (centralizado).
- Ele oferece um serviço de build Elástico, o que significa que ele cria servidores de compilação quando forem necessários e desativa quando elas são feitas dinamicamente. Você poderá iniciar uma compilação automaticamente quando alguém faz check-in das alterações de código fonte, e você não precisa ter alocar e pagar pelos seus próprios servidores de compilação ficam ociosos na maioria das vezes. O serviço de compilação é gratuito, desde que você não exceda um determinado número de compilações. Se você espera fazer um alto volume de compilações, você pode pagar um pouco mais para servidores de compilação reservado.
- Ele dá suporte a entrega contínua no Azure.
- Ele dá suporte a testes de carga automatizada. Teste de carga é essencial para um aplicativo de nuvem, mas muitas vezes é negligenciado até que seja tarde demais. Teste de carga simula o uso intenso de um aplicativo por milhares de usuários, permitindo que você Encontre afunilamentos e melhorar a taxa de transferência — antes de liberar o aplicativo para produção.
- Ele dá suporte a colaboração da sala da equipe, que facilita a comunicação em tempo real e colaboração para pequenas equipes agile.
- Ele dá suporte a gerenciamento de projeto agile.


Para obter mais informações sobre os recursos de entrega do VSTS e integração contínua, consulte [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Se você estiver procurando um gerenciamento de projetos de turnkey, colaboração em equipe e solução de controle do código-fonte, confira o VSTS. O serviço é gratuito para até 5 usuários, e você pode se inscrever no [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Resumo

Os padrões de desenvolvimento de três nuvem primeiro tem sido sobre como implementar um processo de desenvolvimento repetível, confiável e previsível com o tempo de ciclo de baixa. No [próximo capítulo](web-development-best-practices.md) começamos a observar padrões de arquiteturas e codificação.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte [implantar um aplicativo web no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Consulte também os seguintes recursos:

- [Criando um Pipeline de lançamento com o Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Livro eletrônico, laboratórios práticos e código de exemplo pelo Microsoft Patterns and Practices, fornece uma introdução detalhada para entrega contínua. Aborda o uso do Visual Studio Lab Management e o gerenciamento de versão do Visual Studio.
- [ALM Rangers DevOps ferramentas e orientações](https://aka.ms/vsarsolutions/). ALM Rangers introduziu a Bancada de trabalho de DevOps exemplo complementar de solução e orientações práticas em colaboração com os padrões &amp; livro sobre práticas *criando um Pipeline de lançamento com o TFS 2012*, como uma ótima maneira de começar aprender os conceitos de DevOps &amp; Release Management para TFS 2012 e antes de se comprometer. A diretriz mostra como compilar uma vez e implantar em vários ambientes.
- [Testando para entrega contínua com Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Livro eletrônico por Microsoft Patterns and Practices, explica como integrar testes automatizados com entrega contínua.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Código-fonte para uma ferramenta projetada para capturar uma compilação do TFS (com base em um rótulo), compilá-lo, empacotá-lo, permitir que alguém na função de DevOps para configurar aspectos específicos dele e enviar por push para o Azure. A ferramenta acompanha o processo de implantação para permitir operações "reverter" para uma versão previamente implantada. A ferramenta não tem dependências externas e pode funcionar autônomo usando APIs do TFS e o SDK do Azure.
- [Entrega contínua: Versões de Software confiável por meio de automação de implantação, teste e compilação](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Livro de Jez humilde.
- [Liberá-lo! Projetar e implantar Software pronto para produção](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Livro de Michael T. Nygard.

> [!div class="step-by-step"]
> [Anterior](source-control.md)
> [Próximo](web-development-best-practices.md)
