---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Criando aplicativos de nuvem do mundo Real com o Azure | Microsoft Docs
author: MikeWasson
description: "Este livro eletrônico orienta você por meio de uma abordagem baseada em padrões para criar soluções de nuvem do mundo real. Os padrões se aplicam ao processo de desenvolvimento, bem como para um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5054f932d05fb612a6e18a81274719d7e249b77b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="building-real-world-cloud-apps-with-azure"></a>Criando aplicativos de nuvem do mundo Real com o Azure
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Este livro eletrônico orienta você por meio de uma abordagem baseada em padrões para criar soluções de nuvem do mundo real. Os padrões se aplicam ao processo de desenvolvimento, bem como a arquitetura e práticas de codificação.
> 
> O conteúdo é baseado em uma apresentação desenvolvido por Scott Guthrie e entregue por ele na conferência de desenvolvedores norueguês (NDC) em junho de 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e no Microsoft Tech Ed Austrália no Setembro de 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Muitos outros](more-patterns-and-guidance.md#acknowledgments) atualizado e aumentada o conteúdo durante a transição-lo de vídeo para forma escrita.


## <a name="intended-audience"></a>Público-alvo

Os desenvolvedores que estiver curioso sobre o desenvolvimento para a nuvem, considerando uma mudança para a nuvem, ou que são novos para desenvolvimento em nuvem encontrará aqui uma visão geral dos conceitos mais importantes e práticas recomendadas que precisam saber. Os conceitos são ilustrados com exemplos concretos e cada capítulo para outros recursos para obter informações mais detalhadas. Os exemplos e os links para recursos adicionais são estruturas da Microsoft e serviços, mas os princípios ilustrados se aplicam a outras estruturas de desenvolvimento da web e também para ambientes de nuvem.

Os desenvolvedores que já estão desenvolvendo para a nuvem podem descobrir ideias aqui que ajudarão a torná-los mais bem-sucedida. Cada capítulo na série pode ser lido independentemente, para que você pode escolher e escolha tópicos que você está interessado.

Qualquer pessoa que é inspecionada de Scott Guthrie *nuvem aplicativos do mundo Real criando com o Azure* apresentação e deseja obter mais detalhes e as informações atualizadas descobrirá que aqui.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Padrões de desenvolvimento de nuvem

Este livro eletrônico explica que treze recomendadas padrões de desenvolvimento em nuvem. "Padrão" é usado aqui em um sentido mais amplo para significar uma maneira recomendada para fazer coisas: a melhor maneira de desenvolver, criar e codificar aplicativos de nuvem. Estes são os padrões de chave que ajudarão "entram na pit de sucesso" Se você segui-los.

- [Automatizar tudo](automate-everything.md).

    - Use scripts para maximizar a eficiência e minimizar erros em processos repetitivos.
    - Demonstração: Scripts de gerenciamento do Azure.
- [Controle de origem](source-control.md). 

    - Configure a estrutura de ramificação em controle de origem para facilitar o fluxo de trabalho do DevOps.
    - Demonstração: Adicione scripts ao controle de origem.
    - Demonstração: mantenha os dados confidenciais do controle de origem.
    - Demonstração: use o Git no Visual Studio.
- [Integração contínua e entrega](continuous-integration-and-continuous-delivery.md). 

    - Automatize a compilação e implantação com cada check-in de controle de origem.
- [Práticas recomendadas de desenvolvimento de Web](web-development-best-practices.md). 

    - Mantenha a camada da web sem monitoração de estado.
    - Demonstração: escala e o dimensionamento automático em aplicativos da Web no serviço de aplicativo do Azure.
    - Evite o estado da sessão.
    - Use um CDN.
    - Use o modelo de programação assíncrona.
    - Demonstração: assíncrona no ASP.NET MVC e o Entity Framework.
- [Logon único](single-sign-on.md). 

    - Introdução ao Azure Active Directory.
    - Demonstração: Crie um aplicativo ASP.NET que usa o Active Directory do Azure.
- [Opções de armazenamento de dados](data-storage-options.md). 

    - Tipos de armazenamentos de dados.
    - Como escolher o repositório de dados à direita.
    - Demonstração: Banco de dados SQL do Azure.
- [Estratégias de particionamento de dados](data-partitioning-strategies.md). 

    - Particionar os dados verticalmente, horizontalmente, ou ambos para facilitar o dimensionamento de um banco de dados relacional.
- [Armazenamento de blob não estruturados](unstructured-blob-storage.md). 

    - Armazenar arquivos na nuvem usando o serviço blob.
    - Demonstração: usando o armazenamento de blob no aplicativo corrigi-lo.
- [Design sobreviver a falhas](design-to-survive-failures.md). 

    - Tipos de falhas.
    - Escopo da falha.
    - Noções básicas sobre SLAs.
- [Monitoramento e telemetria](monitoring-and-telemetry.md). 

    - Por que você deve comprar um aplicativo de telemetria tanto escrever seu próprio código para instrumentar seu aplicativo.
    - Demonstração: New Relic para o Azure
    - Demonstração: log de código no aplicativo corrigi-lo.
    - Demonstração: injeção de dependência no aplicativo corrigi-lo.
    - Demonstração: o suporte de registro em log internos no Azure.
- [Tratamento de falhas transitórias](transient-fault-handling.md). 

    - Use uma lógica de repetição/retirada inteligente para reduzir o efeito de falhas transitórias.
    - Demonstração: repetição/retirada no Entity Framework 6.
- [Cache distribuído](distributed-caching.md). 

    - Melhorar a escalabilidade e reduzir os custos de transações do banco de dados usando o cache distribuído.
- [Padrão de trabalho centrado em fila](queue-centric-work-pattern.md). 

    - Habilitar a alta disponibilidade e melhorar a escalabilidade acoplamento flexível camadas web e de trabalho.
    - Demonstração: Filas de armazenamento do Azure no aplicativo corrigi-lo.
- [Mais diretrizes e padrões de aplicativo de nuvem](more-patterns-and-guidance.md).
- [Apêndice: A correção exemplo de aplicativo](the-fix-it-sample-application.md)

    - Problemas conhecidos
    - Práticas recomendadas
    - Como baixar, criar, executar e implantar.

Esses padrões se aplicam a todos os ambientes de nuvem, mas que ilustraremos usando exemplos com base em tecnologias da Microsoft e serviços, como o Visual Studio, o Team Foundation Service, o ASP.NET e o Azure.

Este restante deste capítulo apresenta o aplicativo de exemplo corrigir e os aplicativos Web no ambiente de nuvem do serviço de aplicativo do Azure que executa o aplicativo para corrigi-lo no.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>A exemplo de aplicativo de correção

A maioria dos exemplos de código mostrados neste livro eletrônico e capturas de tela baseiam-se o aplicativo corrigir originalmente desenvolvido pela [Scott Guthrie](https://weblogs.asp.net/scottgu/) para demonstrar as práticas e padrões de desenvolvimento de aplicativo de nuvem recomendado.

![Corrija-a página inicial do aplicativo](introduction/_static/image1.png)

O aplicativo de exemplo é um sistema de registro de item de trabalho simples. Quando você precisar de algo fixo, você cria um tíquete e atribuir a alguém e outras pode fazer logon e ver as permissões atribuídas a eles e marca tíquetes como concluído quando o trabalho é feito.

É um projeto de web padrão do Visual Studio. Ele se baseia no ASP.NET MVC e usa um banco de dados do SQL Server. Ele pode ser executada localmente no IIS Express e pode ser implantado para um Site do Azure para executar na nuvem. Você pode fazer logon usando autenticação de formulários e um banco de dados local ou por meio de um provedor social, como Google. (Posteriormente também mostraremos como fazer logon com uma conta organizacional do Active Directory.)

![Página de logon](introduction/_static/image2.png)

Depois que você está conectado em você pode criar um tíquete, atribuí-lo a outra e carregue uma imagem do que você deseja obter corrigido.

![Criar uma tarefa corrigir](introduction/_static/image3.png)

![Corrija-a tarefa criada](introduction/_static/image4.png)

Você pode acompanhar o progresso de itens de trabalho criados por você, consulte permissões atribuídas a você, exibir detalhes de tíquete e itens de marca como concluído.

Este é um aplicativo muito simples de uma perspectiva de recurso, mas você verá como criá-lo para que ele pode dimensionar para milhões de usuários e serão resiliente a coisas como falhas de banco de dados e encerramentos de conexão. Você também verá como criar um fluxo de trabalho de desenvolvimento ágil e automatizada, o que permite que você inicie simples e tornar o aplicativo melhor e melhor iteração do ciclo de desenvolvimento, rápida e eficiente.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplicativos Web no serviço de aplicativo do Azure

Usado para o aplicativo para corrigir o ambiente de nuvem é um serviço do Azure que chamamos de Sites da Web. Esse serviço é uma maneira que você pode hospedar seu próprio aplicativo web no Azure sem a necessidade de criar VMs e mantê-los atualizados, instalar e configurar o IIS, etc. Podemos hospedar seu site em nosso VMs e forneça automaticamente o backup e recuperação e outros serviços para você. O serviço de Sites da Web funciona com o ASP.NET, Node.js, PHP e Python. Ele permite que você implante rapidamente usando o Visual Studio, implantação da Web, FTP, Git ou TFS. É normalmente apenas alguns segundos entre a hora em que você iniciar uma implantação e a hora em que a atualização está disponível na Internet. É tudo gratuito começar, e você pode expandir conforme seu tráfego cresce.

Nos bastidores, aplicativos Web no serviço de aplicativo do Azure fornece muitos componentes de arquitetura e os recursos que você precisa criar por conta própria, se você for para hospedar um site da web usando o IIS em suas próprias VMs. Um componente é um ponto de extremidade de implantação que configura o IIS automaticamente e instala o aplicativo em como muitas máquinas virtuais que você deseja executar o seu site.

![Serviço de implantação](introduction/_static/image5.png)

Quando um usuário acessa o site da web, não atingem o IIS VMs diretamente, passam pelo [roteamento ARR (Application Request)](https://www.iis.net/downloads/microsoft/application-request-routing) balanceadores de carga. Você pode usá-los com seus próprios servidores, mas a vantagem aqui é que ele estão configurados para você automaticamente. Eles usam uma heurística inteligente que leva em consideração fatores como a afinidade de sessão, profundidade da fila no IIS, e o uso da CPU em cada computador direcione o tráfego para as máquinas virtuais que hospedam o site da web.

![Balanceador de carga do ARR](introduction/_static/image6.png)

Se uma máquina falhar, Azure automaticamente extrair da rotação, gira uma nova instância de máquina virtual e direcionar o tráfego para a nova instância – tudo sem nenhum tempo de inatividade para seu aplicativo é iniciado.

![Recuperação automática de falha do computador](introduction/_static/image7.png)

Tudo isso ocorre automaticamente. Tudo o que você precisa fazer é criar um site da web e implantar seu aplicativo, usando o Windows PowerShell, o Visual Studio ou o portal de gerenciamento do Azure.

Para obter um tutorial de passo a passo de rápida e fácil que mostra como criar um aplicativo web no Visual Studio e implantá-lo para um Site do Azure, consulte [Introdução ao Azure e ASP.NET](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Resumo

Esta introdução forneceu uma lista de tópicos aborda o catálogo, capturas de tela do aplicativo de exemplo e uma breve visão geral dos aplicativos Web no ambiente de nuvem do serviço de aplicativo do Azure. Uma das grandes vantagens do desenvolvimento de aplicativos de e para a nuvem é que ele é fácil de automatizar tarefas repetitivas de desenvolvimento, como criar um ambiente de teste e implantar seu código para ele. Como fazer isto é o assunto do [próximo capítulo](automate-everything.md).

## <a name="resources"></a>Recursos

Para obter mais informações sobre os tópicos abordados neste capítulo, consulte os seguintes recursos.

Documentação:

- [Aplicativos no serviço de aplicativo do Azure Web](https://azure.microsoft.com/en-us/services/app-service/web/). Página do portal para obter a documentação sobre os aplicativos Web do Azure.
- [Web de aplicativos, serviços de nuvem e VMs: quando usar o quê?](https://azure.microsoft.com/en-us/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, conforme mostrado neste capítulo é apenas uma das três maneiras que você pode executar aplicativos web no Azure. Este artigo explica as diferenças entre as três formas e fornece orientação sobre como escolher qual delas é adequada para seu cenário. Como Sites, serviços de nuvem é um recurso de PaaS do Azure. Máquinas virtuais são um recurso de IaaS. Para obter uma explicação de PaaS e IaaS, consulte o [opções de dados](data-storage-options.md#paasiaas) capítulo.

Vídeos:

- [Scott Guthrie começa na etapa 0 - o que é o sistema operacional em nuvem do Azure?](https://azure.microsoft.com/en-us/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Arquitetura de Sites da Web - com Stefan Schackow](https://azure.microsoft.com/en-us/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Recursos internos de Sites do Azure com Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

>[!div class="step-by-step"]
[Avançar](automate-everything.md)
