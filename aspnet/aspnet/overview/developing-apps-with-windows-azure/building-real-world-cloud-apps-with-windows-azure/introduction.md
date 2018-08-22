---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Criando aplicativos de nuvem do mundo Real com o Azure | Microsoft Docs
author: MikeWasson
description: Este livro eletrônico o orienta através de uma abordagem baseada em padrões para criar soluções de nuvem do mundo real. Os padrões aplicam-se ao processo de desenvolvimento, bem como para um...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: eade14bc27e2bface84fb0bdd2f3c5bf8ef28432
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824672"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Criando aplicativos de nuvem do mundo Real com o Azure
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Este livro eletrônico o orienta através de uma abordagem baseada em padrões para criar soluções de nuvem do mundo real. Os padrões aplicam-se ao processo de desenvolvimento, bem como a arquitetura e práticas de codificação.
> 
> O conteúdo baseia-se em uma apresentação desenvolvido por Scott Guthrie e entregues por ele na Norwegian Developers Conference (NDC) em junho de 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e no Microsoft Tech Ed Austrália no Setembro de 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Muitos outros](more-patterns-and-guidance.md#acknowledgments) atualizaram e ampliaram o conteúdo ao fazer a transição do vídeo para a forma escrita.


## <a name="intended-audience"></a>Público-alvo

Os desenvolvedores que estiver curioso sobre o desenvolvimento para a nuvem, considerando uma mudança para a nuvem, ou for novo no desenvolvimento de nuvem encontrará aqui uma visão geral concisa dos mais importantes conceitos e práticas recomendadas que precisam saber. Os conceitos são ilustrados com exemplos concretos, hiperlinks e cada capítulo para outros recursos para obter informações mais detalhadas. Os exemplos e os links para recursos adicionais são para serviços e estruturas Microsoft, mas os princípios ilustrados se aplicam a outras estruturas de desenvolvimento da web e também para ambientes de nuvem.

Os desenvolvedores que já estão desenvolvendo para a nuvem podem achar ideias aqui que o ajudará a torná-los mais bem-sucedida. Cada capítulo da série pode ser lido independentemente, portanto, você pode selecionar e escolher tópicos que você está interessado.

Qualquer pessoa que assistiu Guthrie *aos aplicativos de nuvem Real mundo de construção com o Azure* apresentação e deseja obter mais detalhes e informações atualizadas poderá encontrá-lo aqui.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Padrões de desenvolvimento de nuvem

Este livro eletrônico explica que treze recomendado padrões para desenvolvimento em nuvem. "Padrão" é usado aqui em um sentido mais amplo para significar uma maneira recomendada de fazer as coisas: a melhor maneira de desenvolver, criar e codificar aplicativos de nuvem. Esses são os principais padrões que irá ajudá-lo "entram na pit de sucesso" Se você segui-los.

- [Automatizar tudo](automate-everything.md).

    - Use scripts para maximizar a eficiência e minimizar os erros em processos repetitivos.
    - Demonstração: Scripts de gerenciamento do Azure.
- [Controle de fonte](source-control.md). 

    - Configure a estrutura de ramificação no controle de origem para facilitar o fluxo de trabalho de DevOps.
    - Demonstração: Adicione scripts ao controle de origem.
    - Demonstração: manter os dados confidenciais fora do controle do código-fonte.
    - Demonstração: use o Git no Visual Studio.
- [Integração contínua e entrega](continuous-integration-and-continuous-delivery.md). 

    - Automatize a compilação e implantação com cada check-in de controle de origem.
- [Práticas recomendadas de desenvolvimento de Web](web-development-best-practices.md). 

    - Mantenha a camada da web sem monitoração de estado.
    - Demonstração: dimensionamento e o dimensionamento automático em aplicativos Web no serviço de aplicativo do Azure.
    - Evite o estado de sessão.
    - Use uma CDN com um fallback quando o CDN não estiver disponível.
    - Use o modelo de programação assíncrona.
    - Demonstração: assíncronos no ASP.NET MVC e ao Entity Framework.
- [Logon único](single-sign-on.md). 

    - Introdução ao Azure Active Directory.
    - Demonstração: Crie um aplicativo ASP.NET que usa o Azure Active Directory.
- [Opções de armazenamento de dados](data-storage-options.md). 

    - Tipos de armazenamentos de dados.
    - Como escolher o armazenamento de dados correto.
    - Demonstração: Banco de dados SQL do Azure.
- [Estratégias de particionamento de dados](data-partitioning-strategies.md). 

    - Particionar os dados verticalmente, horizontalmente, ou ambos para facilitar o dimensionamento de um banco de dados relacional.
- [Armazenamento de blob não estruturados](unstructured-blob-storage.md). 

    - Store arquivos na nuvem usando o serviço blob.
    - Demonstração: usando o armazenamento de BLOBs no aplicativo corrigi-lo.
- [Design para resistir a falhas](design-to-survive-failures.md). 

    - Tipos de falhas.
    - Escopo da falha.
    - Noções básicas sobre SLAs.
- [Monitoramento e telemetria](monitoring-and-telemetry.md). 

    - Por que você deve comprar um aplicativo de telemetria tanto escrever seu próprio código para instrumentar seu aplicativo.
    - Demonstração: New Relic para o Azure
    - Demonstração: código de registro no aplicativo corrigi-lo.
    - Demonstração: injeção de dependência no aplicativo corrigi-lo.
    - Demonstração: o suporte de registro em log internos no Azure.
- [Tratamento de falhas transitórias](transient-fault-handling.md). 

    - Use a lógica de repetição/retirada inteligente para reduzir o efeito de falhas transitórias.
    - Demonstração: repetição/retirada do Entity Framework 6.
- [Cache distribuído](distributed-caching.md). 

    - Melhorar a escalabilidade e reduzir os custos de transação de banco de dados usando o armazenamento em cache distribuído.
- [Padrão centrado em fila](queue-centric-work-pattern.md). 

    - Habilitar a alta disponibilidade e melhorar a escalabilidade aliando livremente as camadas web e de trabalho.
    - Demonstração: Filas de armazenamento do Azure no aplicativo corrigi-lo.
- [Mais orientações e padrões de aplicativo de nuvem](more-patterns-and-guidance.md).
- [Apêndice: o aplicativo de exemplo Fix It](the-fix-it-sample-application.md)

    - Problemas Conhecidos
    - Práticas recomendadas
    - Como baixar, compilar, executar e implantar.

Esses padrões se aplicam a todos os ambientes de nuvem, mas que ilustraremos usando exemplos com base em tecnologias da Microsoft e serviços, como o Visual Studio, Team Foundation Service, ASP.NET e do Azure.

O restante deste capítulo apresenta o aplicativo de exemplo Fix It e os aplicativos Web no ambiente de nuvem do serviço de aplicativo do Azure que executa o aplicativo corrigi-lo no.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>A correção do exemplo de aplicativo

A maioria das capturas de tela e exemplos de código mostrados neste livro se baseiam no aplicativo Fix It originalmente desenvolvido pela [Scott Guthrie](https://weblogs.asp.net/scottgu/) para demonstrar as práticas e padrões de desenvolvimento de aplicativo de nuvem recomendada.

![Corrigi-lo a home page do aplicativo](introduction/_static/image1.png)

O aplicativo de exemplo é um item de trabalho simples sistema de tíquetes. Quando você precisa de algo fixo, você cria um tíquete e atribuir a alguém e outras pode fazer logon e ver os tíquetes atribuídos a eles e marca tíquetes como concluído quando o trabalho estiver concluído.

É um projeto de web padrão do Visual Studio. Ele se baseia no ASP.NET MVC e usa um banco de dados do SQL Server. Ele pode ser executado localmente no IIS Express e pode ser implantado para um Site da Web do Azure para ser executado na nuvem. Você pode fazer logon usando autenticação de formulários e um banco de dados local ou usando um provedor social, como Google. (Posteriormente, também mostraremos como fazer logon com uma conta organizacional do Active Directory.)

![Página de logon](introduction/_static/image2.png)

Quando estiver conectado em você pode criar um tíquete, atribuí-la a alguém e carregue uma imagem do que você deseja que seja corrigido.

![Criar uma tarefa Fix It](introduction/_static/image3.png)

![Corrigi-lo a tarefa criada](introduction/_static/image4.png)

Você pode acompanhar o progresso dos itens de trabalho que você criou, consulte tíquetes atribuídos a você, exibir detalhes de tíquete e marcar itens como concluídos.

Esse é um aplicativo muito simple de uma perspectiva de recurso, mas você verá como criá-lo para que ele pode ser dimensionado para milhões de usuários e seja resiliente a coisas como falhas de banco de dados e encerramentos de conexão. Você também verá como criar um fluxo de trabalho de desenvolvimento ágil e automatizada, que permite que você inicie com simplicidade e fazer com que o aplicativo melhor e melhor pela iteração rápida e eficiente de ciclo de desenvolvimento.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplicativos Web no serviço de aplicativo do Azure

O ambiente de nuvem usado para o aplicativo Fix It é um serviço do Azure que chamamos de Sites da Web. Esse serviço é uma maneira que você pode hospedar seu próprio aplicativo web no Azure sem a necessidade de criar VMs e mantê-los atualizados, instalar e configurar o IIS, etc. Podemos hospedar seu site em nossas VMs e fornecem automaticamente o backup e recuperação e outros serviços para você. O serviço de Sites da Web funciona com ASP.NET, Node. js, PHP e Python. Ele permite que você implante rapidamente usando o Visual Studio, implantação da Web, FTP, Git ou TFS. Normalmente, é apenas alguns segundos entre a hora em que você iniciar uma implantação e a hora em que a atualização está disponível na Internet. É tudo gratuito começar a usar, e você pode escalar verticalmente conforme seu tráfego aumenta.

Nos bastidores, aplicativos Web no serviço de aplicativo do Azure fornece muitos componentes de arquitetura e recursos que você precisaria criar por conta própria, se você fosse para hospedar um site da web usando o IIS em suas próprias VMs. Um componente é um ponto de extremidade de implantação que configura o IIS automaticamente e instala o aplicativo em várias VMs que deseja executar o seu site.

![Serviço de implantação](introduction/_static/image5.png)

Quando um usuário acessa o site da web, eles não atinjam as VMs do IIS diretamente, eles passam [roteamento ARR (Application Request)](https://www.iis.net/downloads/microsoft/application-request-routing) balanceadores de carga. Você pode usá-los com seus próprios servidores, mas a vantagem aqui é que eles configurá-lo para você automaticamente. Eles usam uma heurística inteligente que leva em consideração fatores como afinidade de sessão, profundidade da fila no IIS, e o uso da CPU em cada computador para direcionar o tráfego para as máquinas virtuais que hospedam seu site da web.

![Balanceador de carga do ARR](introduction/_static/image6.png)

Se um computador ficar inoperante, Azure automaticamente extrai de rotação gira uma nova instância VM e inicia o direcionamento do tráfego para a nova instância – tudo com nenhum tempo de inatividade para seu aplicativo.

![Recuperação automática de falha do computador](introduction/_static/image7.png)

Tudo isso ocorre automaticamente. Tudo o que você precisa fazer é criar um site da web e implantar seu aplicativo a ele, usando o Windows PowerShell, Visual Studio ou o portal de gerenciamento do Azure.

Para obter um tutorial de passo a passo de rápida e fácil que mostra como criar um aplicativo web no Visual Studio e implantá-lo para um Site da Web do Azure, consulte [Introdução ao Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Resumo

Esta introdução forneceu uma lista de tópicos que abordará o livro, capturas de tela do aplicativo de exemplo e uma breve visão geral dos aplicativos Web no ambiente de nuvem do serviço de aplicativo do Azure. Uma das grandes vantagens do desenvolvimento de aplicativos no e para a nuvem é que ele é fácil de automatizar tarefas repetitivas de desenvolvimento, como criar um ambiente de teste e implantar seu código para ele. Como fazer isso, o assunto do [próximo capítulo](automate-everything.md).

## <a name="resources"></a>Recursos

Para obter mais informações sobre os tópicos abordados neste capítulo, consulte os seguintes recursos.

Documentação:

- [Aplicativos Web no serviço de aplicativo do Azure](https://azure.microsoft.com/services/app-service/web/). Página do portal para obter a documentação sobre os aplicativos Web do Azure.
- [Web de aplicativos, serviços de nuvem e VMs: quando usar qual?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, conforme mostrado neste capítulo é apenas um dos três modos que você pode executar aplicativos web no Azure. Este artigo explica as diferenças entre as três maneiras e fornece orientação sobre como escolher qual delas é adequada para seu cenário. Como Sites, serviços de nuvem é um recurso de PaaS do Azure. As VMs são um recurso de IaaS. Para obter uma explicação de PaaS em vez de IaaS, consulte o [opções de dados](data-storage-options.md#paasiaas) capítulo.

Vídeos:

- [Scott Guthrie começa na etapa 0 – o que é o sistema operacional em nuvem do Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Arquitetura de Sites da Web - com Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Recursos internos de Sites do Azure com Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Avançar](automate-everything.md)
