---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoramento e telemetria (compilando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f4dae827627103e5cfb9981b6c3b9342cdc34c13
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577997"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoramento e telemetria (compilando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


Muitas pessoas contam com os clientes para que ele saiba quando seu aplicativo está inativo. Que não é realmente uma prática recomendada em qualquer lugar e especialmente não na nuvem. Não há nenhuma garantia de notificação rápida, e quando você seja notificado, você geralmente pode obter dados falsos ou mínimo sobre o que aconteceu. Com bons sistemas de telemetria e registro em log que você pode estar informado do que está acontecendo com seu aplicativo e quando algo dá errado você descobre imediatamente e tem informações úteis de solução de problemas para trabalhar com.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar ou alugar uma solução de telemetria

> [!NOTE]
> Este artigo foi escrito antes [Application Insights](/azure/application-insights/app-insights-overview) foi lançado. O Application Insights é a abordagem preferencial para soluções de telemetria no Azure. Ver [configurar o Application Insights para seu site ASP.NET](/azure/application-insights/app-insights-asp-net) para obter mais informações.


Uma das coisas que é excelente sobre o ambiente de nuvem é que é muito fácil comprar ou alugar sua maneira de vitória. Telemetria é um exemplo. Sem muito esforço, você pode obter um sistema de telemetria muito bom para cima e em execução, muito econômico. Há um monte de parceiros excelentes que se integram com o Azure e alguns deles têm camadas gratuitas – portanto, você pode obter Telemetria básica para nada. Aqui estão apenas alguns daqueles disponíveis atualmente no Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) também inclui recursos de monitoramento.

Examinaremos rapidamente a configuração do New Relic para mostrar como isso pode ser fácil usar um sistema de telemetria.

No portal de gerenciamento do Azure, inscreva-se para o serviço. Clique em **New**e, em seguida, clique em **Store**. O **escolher um complemento** caixa de diálogo é exibida. Role para baixo e clique em **New Relic**.

![Escolher um complemento](monitoring-and-telemetry/_static/image1.png)

Clique na seta à direita e escolha a camada de serviço que você deseja. Para esta demonstração, usaremos a camada gratuita.

![Personalizar complemento](monitoring-and-telemetry/_static/image2.png)

Clique na seta à direita, confirme a "comprar" e New Relic agora aparece como um complemento no portal.

![Revisar compra](monitoring-and-telemetry/_static/image3.png)

![Complemento de Relíquia de novo no portal de gerenciamento](monitoring-and-telemetry/_static/image4.png)

Clique em **informações de Conexão**e copie a chave de licença.

![Informações de Conexão](monitoring-and-telemetry/_static/image5.png)

Vá para o **configurar** guia para seu aplicativo web no portal, defina **monitoramento de desempenho** para **complemento**e defina o **escolher complemento** a lista suspensa para **New Relic**. Em seguida, clique em **salvar**.

![New Relic na guia Configurar](monitoring-and-telemetry/_static/image6.png)

No Visual Studio, instale o pacote do New Relic NuGet em seu aplicativo.

![Análise do desenvolvedor na guia Configurar](monitoring-and-telemetry/_static/image7.png)

Implantar o aplicativo no Azure e começar a usá-lo. Crie alguns Fix It tarefas para fornecer alguma atividade New relic monitorar.

Em seguida, volte para o **New Relic** página na **complementos** guia do portal e clique em **gerenciar**. O portal envia para o portal de gerenciamento do New Relic, usando o logon único para autenticação para que você não precise digitar suas credenciais novamente. A página de visão geral apresenta uma variedade de estatísticas de desempenho. (Clique na imagem para ver o tamanho total da página Visão geral).

[![Guia de monitoramento do Relíquia nova](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Aqui estão algumas das estatísticas que você pode ver:

- Tempo médio de resposta em diferentes momentos do dia.

    ![Tempo de resposta](monitoring-and-telemetry/_static/image10.png)
- Taxas de transferência (em solicitações por minuto) em diferentes momentos do dia.

    ![Taxa de transferência](monitoring-and-telemetry/_static/image11.png)
- Tempo de CPU do servidor gasto no tratamento de solicitações HTTP diferentes.

    ![Tempos de transação da Web](monitoring-and-telemetry/_static/image12.png)
- Tempo de CPU gasto em diferentes partes do código do aplicativo:

    ![Detalhes de rastreamento](monitoring-and-telemetry/_static/image13.png)
- Estatísticas de desempenho histórico.

    ![Histórico de desempenho](monitoring-and-telemetry/_static/image14.png)
- Chamadas para serviços externos, como o serviço Blob e estatísticas sobre como confiável e responsivo em que o serviço tiver sido.

    ![Serviços externos](monitoring-and-telemetry/_static/image15.png)

    ![Serviços externos](monitoring-and-telemetry/_static/image16.png)

    ![Serviço externo](monitoring-and-telemetry/_static/image17.png)
- Informações sobre onde no mundo ou local no aplicativo web dos EUA tráfego de origem.

    ![Geografia](monitoring-and-telemetry/_static/image18.png)

Você também pode configurar relatórios e eventos. Por exemplo, você pode dizer a qualquer momento que você começar a ver erros, envie um email à equipe de suporte de alerta para o problema.

![Relatórios](monitoring-and-telemetry/_static/image19.png)

O New Relic é apenas um exemplo de um sistema de telemetria. Você pode obter tudo isso de outros serviços também. A beleza da nuvem é que, sem precisar escrever nenhum código e para mínima ou nenhuma despesa, repentinamente pode obter muito mais informações sobre como seu aplicativo está sendo usado e o que seus clientes, na verdade, estão enfrentando.

<a id="log"></a>
## <a name="log-for-insight"></a>Log para mais informações

Um pacote de telemetria é uma boa primeira etapa, mas você ainda precisa instrumentar seu próprio código. O serviço de telemetria informa quando há um problema e informa o que os clientes estão experimentando, mas ele pode não lhe muitas informações sobre o que está acontecendo em seu código.

Você não quer ter remotamente um servidor de produção para ver o que está fazendo o seu aplicativo. Quando você tem um servidor, mas e quando você tiver dimensionado para centenas de servidores e você não sabe quais delas precisam remotamente, que pode ser prático? O registro em log deve fornecer informações suficientes para que você nunca precisará remoto em servidores de produção para analisar e depurar problemas. Você deve ser registro em log informações suficientes para que você pode isolar problemas exclusivamente por meio dos logs.

### <a name="log-in-production"></a>Faça logon em produção

Muitas pessoas ativar o rastreamento em produção somente quando há um problema e eles desejam depurar. Isso pode introduzir um atraso significativo entre a hora em que você esteja ciente de um problema e a hora em que você obtenha informações úteis de solução de problemas sobre ele. E as informações que você obtenha não possam ser úteis para erros intermitentes.

No ambiente de nuvem em que o armazenamento é barato, é recomendável é que você deixe sempre fazendo logon em produção. Dessa forma, quando erros ocorrerem, você ainda tiver registrado e você tiver dados históricos que podem ajudar a você analisar problemas que desenvolvem ao longo do tempo ou ocorrem regularmente em momentos diferentes. Você pode automatizar um processo de limpeza para excluir logs antigos, mas você pode achar que é mais caro configurar um processo do que manter os logs.

A despesa adicionada de registro em log é trivial em comparação com a quantidade de tempo e dinheiro, que você pode salvar fazendo com que todas as informações que você precisa já está disponível quando algo dá errado de solução de problemas. Em seguida, quando alguém que eles tinham um erro aleatórias em algum momento aproximadamente 8:00 ontem à noite, mas eles não se lembra o erro, prontamente pode descobrir qual era o problema.

Para menos de US $4 um mês, você pode manter 50 gigabytes de logs por lado e o impacto no desempenho de registro em log é trivial desde que você mantenha uma coisa em mente, a fim de evitar gargalos de desempenho, verifique se sua biblioteca de registro em log é assíncrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar os logs que informam de logs que exigem ação

Os logs devem informar (quero saber algo) ou o ACT (que eu quero fazer alguma coisa). Tenha cuidado para gravar apenas os logs do ACT para problemas que exigem genuinamente uma pessoa ou um processo automatizado para agir. Muitos logs do ACT criará o ruído, que exigem muito trabalho a serem analisados tudo isso para localizar problemas originais. E se os logs do ACT disparam automaticamente alguma ação, como o envio de email para a equipe de suporte, evite permitindo que milhares de tais ações ser disparado por um único problema.

No rastreamento de System. Diagnostics do .NET, os logs podem ser atribuídos nível de erro, aviso, informações e depuração/detalhado. É possível diferenciar ACT de logs de informar reservando o nível de erro para os logs do ACT e usando os níveis inferiores para informar logs.

![Níveis de registro em log](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurar níveis de log em tempo de execução

Enquanto que vale a pena ter sempre fazendo logon em produção, outra prática recomendada é implementar uma estrutura de log que permite que você ajuste em tempo de execução, o nível de detalhe que você está fazendo, sem reimplantar ou reiniciar o aplicativo. Por exemplo quando você usa o recurso de rastreamento `System.Diagnostics` você pode criar o erro, aviso, informações e logs de depuração/detalhado. Recomendamos que você sempre que você fizer o erro, aviso, informações de logs em produção e você desejará ser capaz de adicionar dinamicamente o log de depuração/detalhado para solução de problemas caso a caso.

Aplicativos Web no serviço de aplicativo do Azure têm suporte interno para gravação `System.Diagnostics` logs para o sistema de arquivos, armazenamento de tabela ou armazenamento de BLOBs. Você pode selecionar os níveis de log diferentes para cada destino de armazenamento, e você pode alterar o nível de log em tempo real sem reiniciar o aplicativo. O suporte ao armazenamento de Blob torna mais fácil de executar [HDInsight](https://docs.microsoft.com/azure/hdinsight/) trabalhos de análise em seus logs de aplicativo, porque HDInsight sabe como trabalhar diretamente com o armazenamento de BLOBs.

### <a name="log-exceptions"></a>Exceções de log

Não coloque apenas *exceção. ToString ()* em seu código de registro em log. Isso deixa as informações contextuais. No caso de erros do SQL, ele deixa de fora o número do erro SQL. Para todas as exceções, incluem informações de contexto, a própria exceção e exceções internas para certificar-se de que você está fornecendo tudo o que serão necessário para a solução de problemas. Por exemplo, informações de contexto podem incluir o nome do servidor, um identificador de transação e um nome de usuário (mas não a senha ou os segredos!).

Se você confiar em cada desenvolvedor deve fazer a coisa certa com exceção de registro em log, alguns deles não. Para garantir que seja feito o direito de forma toda vez, manipulação de exceções diretamente em sua interface de agente de compilação: passar o próprio objeto de exceção para a classe de agente e registrar os dados de exceção corretamente na classe de agente.

### <a name="log-calls-to-services"></a>Chamadas de log para serviços

É altamente recomendável que você grava um log sempre que seu aplicativo chama um serviço, se é para um banco de dados ou uma API REST ou qualquer serviço externo. Inclua em seus logs não só uma indicação de sucesso ou falha, mas quanto tempo levou cada solicitação. No ambiente de nuvem, verá com frequência problemas relacionados a problemas de lentidão em vez de interrupções completas. Algo que normalmente leva de 10 milissegundos, de repente, pode começar a fazer um segundo. Quando alguém diz ao que seu aplicativo está lento, você deseja ser capaz de examinar o New Relic ou qualquer serviço de telemetria, você tem e valida sua experiência e, em seguida, você deseja ser capaz de procurar são seus próprios logs de se aprofundar nos detalhes de por que está lenta.

### <a name="use-an-ilogger-interface"></a>Usar uma interface ILogger

O que é recomendável fazer quando você cria um aplicativo de produção é criar um simples *ILogger* de interface e coloque a alguns métodos nele. Isso torna mais fácil alterar posteriormente a implementação de registro em log e não precisa passar por todo o seu código para fazê-lo. Podemos pode estar usando o `System.Diagnostics.Trace` classe em todo o aplicativo para corrigi-lo, mas em vez disso, estamos usando-nos bastidores em uma classe de registro em log que implementa *ILogger*, e fazemos *ILogger* chamadas de método em todo o aplicativo.

Dessa forma, se você quiser fazer o registro em log mais avançada, você pode substituir [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) com qualquer mecanismo de registro em log que você deseja. Por exemplo, à medida que aumenta de seu aplicativo pode decidir que deseja usar um pacote de registro em log mais abrangente, como [NLog](http://nlog-project.org/) ou [Enterprise Library Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) é outra estrutura de log populares, mas ele não faz o registro em log assíncrono.)

Um motivo possível para usar uma estrutura, como NLog é facilitar a dividir a saída de log em armazenamentos de dados separados de alto volume e de alto valor. Que ajuda a armazenar de forma eficiente grandes volumes de dados informar que você não precisa executar consultas rápidas, mantendo acesso rápido aos dados do ACT.

### <a name="semantic-logging"></a>Registro em log semântico

Para uma maneira relativamente nova fazer o registro em log que pode gerar informações de diagnóstico mais úteis, consulte [Enterprise Library registro semântico aplicativo bloco (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Usa SLAB [rastreamento de eventos para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) e [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) suporte no .NET 4.5 para que você possa criar mais logs que podem ser consultados e não estruturados. Você define um método diferente para cada tipo de evento que você fizer logon, o que permite que você personalize as informações que você escreve. Por exemplo, para registrar um erro de banco de dados SQL você pode chamar um `LogSQLDatabaseError` método. Para esse tipo de exceção, você sabe que uma parte importante de informações é o número de erro, portanto, você poderia incluir um parâmetro de número do erro na assinatura do método e registre o número de erro como um campo separado no registro de log que você escreve. Porque o número está em um campo separado é possível mais fácil e confiável obter relatórios com base nos números de erro SQL do que seria possível se você apenas foram concatenando o número do erro em uma cadeia de caracteres de mensagem.

## <a name="logging-in-the-fix-it-app"></a>Log a correção aplicativo

### <a name="the-ilogger-interface"></a>A interface ILogger

Aqui está o *ILogger* interface no aplicativo corrigi-lo.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Esses métodos permitem que você gravar logs com os mesmos níveis de quatro com suporte pelo *System. Diagnostics*. Os métodos de TraceApi são para registrar em log as chamadas de serviço externo, com informações sobre a latência. Você também pode adicionar um conjunto de métodos para o nível de depuração/detalhado.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>A implementação da interface ILogger do agente

A implementação da interface é muito simple. Ele basicamente apenas chama no padrão *System. Diagnostics* métodos. O trecho a seguir mostra as três dos métodos de informações e um dos outros.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Chamar os métodos de ILogger

Sempre que o código no aplicativo Fix It captura uma exceção, ele chama um *ILogger* método para registrar em log os detalhes da exceção. E toda vez que ele faz uma chamada para o banco de dados, serviço Blob ou uma API REST, ele inicia um cronômetro antes da chamada, que interrompe o cronômetro quando o serviço retorna e registra o tempo decorrido, juntamente com informações sobre o êxito ou falha.

Observe que a mensagem de log inclui o nome de classe e o nome do método. É uma boa prática para certificar-se de que as mensagens de log identificam qual parte do código do aplicativo de existência.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Portanto, agora para toda vez que o aplicativo Fix It fez uma chamada para o banco de dados SQL, você pode ver a chamada, o método que chamou, e exatamente quanto tempo ela levou.

![Consulta de banco de dados SQL nos logs](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se você ir navegando por meio dos logs, você pode ver que o tempo necessário de chamadas de banco de dados é variável. Que informações podem ser úteis: porque o aplicativo faz tudo isso, você pode analisar as tendências históricas no desempenho do serviço do banco de dados ao longo do tempo. Por exemplo, um serviço pode ser rápido na maioria das vezes, mas as solicitações podem falhar ou respostas lentas em determinados momentos do dia.

Você pode fazer a mesma coisa para o serviço Blob – para sempre que o aplicativo carrega um novo arquivo, há um log e você pode ver exatamente quanto tempo demorou para carregar cada arquivo.

![Log de upload de blob](monitoring-and-telemetry/_static/image23.png)

Ele é apenas duas linhas extras de código para escrever toda vez que você chama um serviço agora sempre que alguém diz que tiveram um problema, você sabe exatamente qual foi o problema, se ele foi um erro, ou mesmo se ele foi apenas sendo executado lentamente. Você pode identificar a origem do problema sem precisar acessar remotamente um servidor ou ativar o registro em log depois que o erro ocorre e espero que recriá-lo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Injeção de dependência na correção-app

Você talvez esteja se perguntando como o construtor de repositório no exemplo mostrado acima obtém o agente de implementação de interface:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar a interface para a implementação do aplicativo usa [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection)(DI) com [AutoFac](http://autofac.org/). Injeção de dependência permite que você use um objeto com base em uma interface em vários locais em todo o código e só precisa especificar em um só lugar, a implementação que é usada quando a interface é instanciada. Isso torna mais fácil alterar a implementação: por exemplo, você talvez queira substituir o agente do System. Diagnostics por um agente de log NLog. Ou, para testes automatizados você talvez queira substituir por uma versão fictícia do agente de log.

O aplicativo Fix It usa DI em todos os repositórios e todos os controladores. Os construtores de classes do controlador é obter um *ITaskRepository* interface da mesma forma que o repositório obtém uma interface de agente:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

O aplicativo usa a biblioteca de DI AutoFac para fornecer automaticamente *TaskRepository* e *agente* instâncias para esses construtores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Esse código diz, basicamente, que precisa de um construtor em qualquer lugar uma *ILogger* interface, passe em uma instância das *agente* classe, e sempre que precisar um *IFixItTaskRepository*da interface, passe em uma instância das *FixItTaskRepository* classe.

[AutoFac](http://autofac.org/) é uma das várias estruturas de injeção de dependência que você pode usar. É outra popular [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), que é recomendado e com suporte pelo Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Suporte de registro em log internos no Azure

Azure suporta os seguintes tipos de [registro em log para aplicativos Web no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Rastreamento de System. Diagnostics (você pode ativar e desativar e definir níveis imediatamente sem reiniciar o site).
- Eventos do Windows.
- Logs do IIS (HTTP/FREB).

Azure suporta os seguintes tipos de [registro em log em serviços de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Rastreamento de System. Diagnostics.
- Contadores de desempenho.
- Eventos do Windows.
- Logs do IIS (HTTP/FREB).
- Monitoramento de diretório personalizado.

O aplicativo Fix It usa o rastreamento de System. Diagnostics. Tudo o que você precisa fazer para habilitar o registro em log em um aplicativo web de System. Diagnostics é mudar uma opção no portal ou chamar a API REST. No portal, clique o **Configuration** guia para o seu site e role para baixo para ver a **Application Diagnostics** seção. Você pode ativar ou desativar o registro em log e selecione o nível de log desejado. Você pode fazer com que o Azure gravar os logs para o sistema de arquivos ou para uma conta de armazenamento.

![Diagnóstico de aplicativo e o diagnóstico de site na guia Configurar](monitoring-and-telemetry/_static/image24.png)

Depois de habilitar o registro em log no Azure, você pode ver os logs na janela de saída do Visual Studio, conforme eles são criados.

![Menu de logs de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu de logs de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Você também pode ter logs gravados em sua conta de armazenamento e exiba-os com qualquer ferramenta que pode acessar o serviço de tabela de armazenamento do Azure, tais como **Gerenciador de servidores** no Visual Studio ou [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Logs no Gerenciador de servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumo

É muito simple implementar um sistema de telemetria, instrumentar o registro em log em seu próprio código e configurar o log no Azure. E quando você tiver problemas de produção, a combinação de um sistema de telemetria e logs personalizados ajudará a resolver problemas rapidamente antes que se tornem problemas importantes para seus clientes.

No [capítulo seguinte](transient-fault-handling.md) , examinaremos como tratar erros transitórios para que eles não se tornem problemas de produção que você deve investigar.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos.

Documentação principalmente sobre a telemetria:

- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte orientações de instrumentação e telemetria, orientação de medição de serviço, monitoramento de ponto de extremidade de integridade padrão e padrão de reconfiguração de tempo de execução.
- [Centavos apertar na nuvem: habilitar o monitoramento em sites do Azure de desempenho de Relíquia nova](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy. Consulte a seção de telemetria e diagnóstico.
- [Desenvolvimento de última geração com o Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artigo da MSDN Magazine.

Documentação principalmente sobre registro em log:

- [O bloco de aplicativo de registro em log semântico (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie apresenta o caso para registro em log semântico com a sessão.
- [Criação de Logs significativos e não estruturados com registro em log semântico](https://channel9.msdn.com/Events/Build/2013/3-336). (Vídeo) Juliano Dominguez apresenta o caso para registro em log semântico com a sessão.
- [EF6 Log de SQL – parte 1: registro em log Simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers mostra como registrar em log consultas executadas pelo Entity Framework no EF 6.
- [Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Em quarto lugar em uma série de tutoriais de nove partes, mostra como usar o recurso de interceptação de comando do EF 6 para registrar os comandos SQL enviados ao banco de dados pelo Entity Framework.
- [Melhorar o log usando o c# 5.0 atributos de informações](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Como registrar facilmente o nome do método de chamada sem embuti-la em literais ou usando a reflexão para obtê-lo manualmente.

Documentação principalmente sobre solução de problemas:

- [Solução de problemas do Azure &amp; depuração blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – o utilitário de diagnóstico usado pela equipe de suporte do Azure Developer](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Apresenta e fornece um link de download de uma ferramenta que pode ser usada em uma VM do Azure para baixar e executar uma ampla variedade de ferramentas de diagnóstico e monitoramento. É útil quando você precisa diagnosticar um problema em uma VM específica.
- [Solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Um tutorial passo a passo de Introdução ao System. Diagnostics rastreamento e depuração remota.

Vídeos:

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, histórias desenhadas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Episódios 4 e 9 são sobre monitoramento e telemetria. Episódio 9 inclui uma visão geral do monitoramento de serviços MetricsHub e PagerDuty, New Relic e AppDynamics.
- [Grande de construção: Lições aprendidas de clientes do Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre design para a falha e instrumentar tudo. Semelhante a série à prova de falhas, mas entra em mais detalhes de instruções.

Exemplo de código:

- [Conceitos fundamentais do serviço no Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo criado pelo Microsoft Azure Customer Advisory Team. Demonstra a telemetria e as práticas recomendadas de registro em log, conforme explicado nos artigos a seguir. O exemplo implementa o log de aplicativo por meio [NLog](http://nlog-project.org/). Para documentação relacionada, consulte o [série de quatro artigos wiki do TechNet sobre o registro em log e telemetria](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md)
> [Próximo](transient-fault-handling.md)
