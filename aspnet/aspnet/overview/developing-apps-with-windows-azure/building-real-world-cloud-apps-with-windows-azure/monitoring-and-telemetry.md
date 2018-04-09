---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoramento e telemetria (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: d58c495b3888c146a2a9bc831865cf7cc0d94c7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoramento e telemetria (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Muitas pessoas dependem de clientes para informá-lo quando seu aplicativo está inativo. Que não é realmente uma prática recomendada em qualquer lugar e especialmente não na nuvem. Não há nenhuma garantia de notificação rápida e quando você é notificado, você normalmente obtém dados de falsos ou mínimo sobre o que aconteceu. Com bom telemetria e sistemas de registro em log, que você pode estar atento que está acontecendo com seu aplicativo e quando algo der errados, você descobrir imediatamente e têm informações úteis de solução de problemas para trabalhar com.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar ou alugar uma solução de telemetria

> [!NOTE]
> Este artigo foi escrito antes de [Application Insights](https://azure.microsoft.com/services/application-insights/) foi liberado. Informações do aplicativo é a abordagem preferencial para soluções de telemetria no Azure. Consulte [configurar o Application Insights para seu site da Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) para obter mais informações.


Uma das coisas excelente sobre o ambiente de nuvem é que é realmente fácil comprar ou alugar sua forma de vitória. Telemetria é um exemplo. Sem muito trabalho, você pode obter um sistema de telemetria realmente bons para cima e em execução, muito econômica. Há vários parceiros grandes que se integram com o Azure e algumas delas tem livres camadas –, portanto, você pode obter Telemetria básica para nada. Aqui estão apenas alguns dos disponíveis atualmente no Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

A partir de março de 2015, [Microsoft Application Insights para Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) ainda não está liberada, mas está disponível na visualização para experimentar. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) também inclui recursos de monitoramento.

Examinaremos rapidamente Configurando New Relic para mostrar como é fácil é usar um sistema de telemetria.

No portal de gerenciamento do Azure, inscreva-se para o serviço. Clique em **novo**e, em seguida, clique em **repositório**. O **escolher um complemento** caixa de diálogo é exibida. Role para baixo e clique em **New Relic**.

![Escolha um complemento](monitoring-and-telemetry/_static/image1.png)

Clique na seta à direita e escolha a camada de serviço que você deseja. Para esta demonstração, usaremos a camada gratuita.

![Personalizar o complemento](monitoring-and-telemetry/_static/image2.png)

Clique na seta à direita, confirme a compra"" e New Relic agora aparece como um complemento no portal.

![Examine a compra](monitoring-and-telemetry/_static/image3.png)

![Novo complemento Relic no portal de gerenciamento](monitoring-and-telemetry/_static/image4.png)

Clique em **informações de Conexão**e copie a chave de licença.

![Informações de Conexão](monitoring-and-telemetry/_static/image5.png)

Vá para o **configurar** guia para definir seu aplicativo web no portal, **monitoramento de desempenho** para **complemento**e defina o **escolher complemento** a lista suspensa para **New Relic**. Em seguida, clique em **salvar**.

![New Relic na guia Configurar](monitoring-and-telemetry/_static/image6.png)

No Visual Studio, instale o pacote do NuGet do New Relic em seu aplicativo.

![Análise do desenvolvedor na guia Configurar](monitoring-and-telemetry/_static/image7.png)

Implantar o aplicativo no Azure e começar a usá-lo. Crie alguns corrigir tarefas para fornecer uma atividade para o New Relic monitorar.

Em seguida, vá para o **New Relic** página o **complementos** guia do portal e clique em **gerenciar**. O portal envia para o portal de gerenciamento do New Relic, usando o logon único para autenticação, portanto você não precisa digitar suas credenciais novamente. A página de visão geral apresenta uma variedade de estatísticas de desempenho. (Clique na imagem para ver o tamanho total da página Visão geral).

[![Nova guia Monitoramento Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Aqui estão apenas alguns das estatísticas, que você pode ver:

- Tempo médio de resposta em diferentes momentos do dia.

    ![Tempo de resposta](monitoring-and-telemetry/_static/image10.png)
- Taxas de transferência (em solicitações por minuto) em diferentes momentos do dia.

    ![Taxa de transferência](monitoring-and-telemetry/_static/image11.png)
- Tempo de CPU do servidor gasto na manipulação de solicitações HTTP diferentes.

    ![O tempo de transação da Web](monitoring-and-telemetry/_static/image12.png)
- Tempo de CPU gasto em diferentes partes do código do aplicativo:

    ![Detalhes de rastreamento](monitoring-and-telemetry/_static/image13.png)
- Estatísticas de desempenho histórico.

    ![Histórico de desempenho](monitoring-and-telemetry/_static/image14.png)
- Chamadas para os serviços externos, como o serviço Blob e estatísticas sobre como confiável e responsivo foi o serviço.

    ![Serviços externos](monitoring-and-telemetry/_static/image15.png)

    ![Serviços externos](monitoring-and-telemetry/_static/image16.png)

    ![Serviço externo](monitoring-and-telemetry/_static/image17.png)
- Informações sobre onde no mundo ou local no aplicativo web EUA tráfego de origem.

    ![Geografia](monitoring-and-telemetry/_static/image18.png)

Você também pode configurar relatórios e eventos. Por exemplo, você pode dizer a qualquer momento você começar a ver erros, envie um email à equipe de suporte de alerta para o problema.

![Relatórios](monitoring-and-telemetry/_static/image19.png)

New Relic é apenas um exemplo de um sistema de telemetria. Você pode obter tudo isso de outros serviços também. A vantagem da nuvem é que, sem a necessidade de escrever nenhum código e para mínima ou nenhuma despesa, você, de repente, pode obter muito mais informações sobre como seu aplicativo está sendo usado e o que os clientes estão tendo.

<a id="log"></a>
## <a name="log-for-insight"></a>Log para mais informações

Um pacote de telemetria é um bom começo, mas você ainda precisa instrumentar seu próprio código. O serviço de telemetria informa quando há um problema e diz que os clientes estão experimentando, mas talvez não você terá muitas informações sobre o que está acontecendo em seu código.

Você não deseja precisa remoto em um servidor de produção para ver o que seu aplicativo está fazendo. Quando você tem um servidor, mas e quando você tiver dimensionado para centenas de servidores e você não souber quais você precisa remoto em, que pode ser prático? O registro em log fornecem as informações que você nunca deve remoto em servidores de produção para analisar e depurar problemas. Você deve estar registrando informações suficientes para que você pode isolar problemas apenas por meio de logs.

### <a name="log-in-production"></a>Faça logon em produção

Muitas pessoas ative o rastreamento em produção somente quando há um problema e que eles deseja depurar. Isso pode introduzir um atraso significativo entre a hora em que você está ciente de um problema e a hora em que você obtém informações úteis de solução de problemas sobre ele. E as informações que você obtém não podem ser útil para erros intermitentes.

É recomendável no ambiente de nuvem em que o armazenamento seja barato é sempre deixar fazendo logon em produção. Dessa forma, quando erros ocorrerem já registrá-los conectado e você tiver dados históricos que podem ajudar você analisar problemas que desenvolvem ao longo do tempo ou ocorrem regularmente em momentos diferentes. Você pode automatizar um processo de limpeza para excluir logs antigos, mas você pode achar que é mais caro para configurar um processo do que manter os logs.

A despesa adicional de registro em log é simples em comparação com a quantidade de tempo e dinheiro, que você pode salvar fazendo com que todas as informações que você precisa já está disponível quando algo dá errado de solução de problemas. Em seguida, quando alguém que tinham um erro aleatório em algum momento aproximadamente 8:00 noite anterior, mas eles não se lembra o erro, prontamente pode descobrir qual foi o problema.

Para menor que US $4 um mês em que você pode manter 50 gigabytes de logs disponíveis e o impacto no desempenho de registro em log é trivial, contanto que você mantenha uma coisa em mente – para evitar afunilamentos de desempenho, verifique se sua biblioteca de registro em log é assíncrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar os logs que informam de logs que exigem ação

Logs devem INFORM (quero saber algo) ou agir (quero fazer algo). Tenha cuidado para gravar somente os logs do ACT para problemas que exigem genuinamente uma pessoa ou um processo automatizado para agir. Muitos logs do ACT criará ruído, exigir muito trabalho a serem analisados tudo isso para encontrar problemas originais. E se os logs do ACT disparam automaticamente alguma ação, como enviar email para a equipe de suporte, para evitar deixar milhares de ações a ser disparado por um único problema.

Rastreamento de .NET System. Diagnostics logs podem ser atribuídos nível de erro, aviso, informações e depuração/log detalhado. Você pode diferenciar o ato de logs de informação reservar o nível de erro para logs do ACT e usando os níveis inferiores para logs de informação.

![Níveis de registro em log](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurar níveis de log em tempo de execução

Enquanto isso é útil ter logon sempre em produção, outra prática recomendada é implementar uma estrutura de log que permite ajustar o nível de detalhe que você estiver fazendo o logon, sem reimplantar ou reiniciar seu aplicativo em tempo de execução. Por exemplo quando você usa o recurso de rastreamento `System.Diagnostics` você pode criar o erro, aviso, informações e logs de depuração/log detalhado. Recomendamos que você sempre que você efetuar o erro, aviso, informações de logs em produção e você desejará ser capaz de adicionar dinamicamente o log de depuração/log detalhado para solução de problemas caso a caso.

Aplicativos Web no serviço de aplicativo do Azure tem suporte interno para gravação `System.Diagnostics` logs para o sistema de arquivos, armazenamento de tabela ou armazenamento de Blob. Você pode selecionar os níveis de log diferente para cada destino de armazenamento, e você pode alterar o nível de log em tempo real sem reiniciar o aplicativo. O suporte de armazenamento de Blob torna mais fácil executar [HDInsight](https://docs.microsoft.com/azure/hdinsight/) trabalhos de análise em seus logs de aplicativo, porque o HDInsight sabe trabalhar diretamente com o armazenamento de Blob.

### <a name="log-exceptions"></a>Exceções de log

Não coloque apenas *exceção. ToString ()* em seu código de registro em log. Isso deixa as informações contextuais. No caso de erros do SQL, ele deixa o número de erro do SQL. Para todas as exceções incluem informações de contexto, a própria exceção e exceções internas para certificar-se de que você está fornecendo tudo o que serão necessário para a solução de problemas. Por exemplo, informações de contexto podem incluir o nome do servidor, um identificador de transação e um nome de usuário (mas não a senha ou nenhum segredo!).

Se você confiar em cada desenvolvedor para fazer o certo com exceção do registro em log, algumas delas não. Para garantir que seja feito o direito de forma toda vez, diretamente na interface do agente de log de manipulação de exceção de compilação: transmitir o próprio objeto de exceção para a classe do agente de log e registre os dados de exceção corretamente na classe do agente de log.

### <a name="log-calls-to-services"></a>Registrar as chamadas para serviços

É altamente recomendável que você gravar um log toda vez que o aplicativo chama um serviço, se for um banco de dados ou uma API REST ou qualquer serviço externo. Inclua em seus logs não apenas uma indicação de sucesso ou falha, mas quanto tempo levou de cada solicitação. No ambiente de nuvem você geralmente verá problemas relacionados a redução de velocidade em vez de interrupções concluídas. Algo que normalmente leva 10 milissegundos, de repente, pode começar a fazer um segundo. Quando alguém que seu aplicativo está lento, você deseja ser capaz de pesquisar no New Relic ou qualquer serviço de telemetria ter e validar sua experiência e, em seguida, você deseja ser capaz de pesquisar são seus próprios logs para saber mais sobre os detalhes de por que está lenta.

### <a name="use-an-ilogger-interface"></a>Usar uma interface de ILogger

O que é recomendável fazer quando você cria um aplicativo de produção é criar um simples *ILogger* interface e coloque a alguns métodos nele. Isso facilita a alterar posteriormente a implementação do registro em log e não precisa passar por todo o seu código para fazer isso. Podemos pode estar usando o `System.Diagnostics.Trace` classe em todo o aplicativo para corrigi-lo, mas em vez disso, estamos usando ele em segundo plano em uma classe de log que implementa *ILogger*, e oferecemos *ILogger* chamadas de método em todo o aplicativo.

Dessa forma, se você quiser fazer o registro em log mais avançada, você pode substituir [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) com qualquer mecanismo de log desejado. Por exemplo, à medida que aumenta de seu aplicativo pode decidir que deseja usar um pacote de registro em log mais abrangente, como [NLog](http://nlog-project.org/) ou [Enterprise Library log Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) é outra estrutura de log populares, mas não faz log assíncrono.)

Um motivo possível para o uso de uma estrutura como NLog é facilitar dividir saída de log em repositórios de dados de alto valor e de alto volume separado. Que ajuda a armazenar de forma eficiente grandes volumes de dados de informação que você não precisa executar consultas rápidas, mantendo o acesso rápido aos dados do ACT.

### <a name="semantic-logging"></a>Registro em log semântico

Para uma maneira relativamente nova fazer o registro em log que pode gerar informações de diagnósticas mais úteis, consulte [Enterprise Library semântica log aplicativo bloco (sessão)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). SESSÃO usa [de rastreamento de eventos do Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) e [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) suporte no .NET 4.5 para que você possa criar mais logs estruturados e passível de consulta. Você define um método diferente para cada tipo de evento que você fizer logon, o que permite que você personalize as informações que você escrever. Por exemplo, para registrar um erro de banco de dados SQL pode chamar um `LogSQLDatabaseError` método. Para esse tipo de exceção, você sabe que uma parte importante de informações é o número do erro, para que você pode incluir um parâmetro de número de erro na assinatura do método e registre o número do erro como um campo separado no registro de log que você escreve. Porque o número está em um campo separado mais fácil e confiável conseguir relatórios com base nos números de erro SQL do que seria possível se você apenas foram concatenando o número do erro em uma cadeia de caracteres de mensagem.

## <a name="logging-in-the-fix-it-app"></a>Log de correção de aplicativo

### <a name="the-ilogger-interface"></a>A interface ILogger

Aqui está o *ILogger* interface no aplicativo corrigi-lo.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Esses métodos permitem que você gravar logs com os mesmos níveis de quatro suportados *System. Diagnostics*. Os métodos de TraceApi são para registrar em log as chamadas de serviço externo com informações sobre a latência. Você também pode adicionar um conjunto de métodos para o nível de depuração/log detalhado.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>A implementação de agente de log da interface ILogger

A implementação da interface é muito simple. Ela basicamente apenas chama o padrão *System. Diagnostics* métodos. O trecho a seguir mostra todos os três métodos de informações e um dos outros.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Chamar os métodos de ILogger

Sempre que o código no aplicativo corrigir captura uma exceção, ele chama uma *ILogger* método para registrar os detalhes da exceção. E toda vez que ele faz uma chamada para o banco de dados, o serviço Blob ou uma API REST, ele inicia um cronômetro antes da chamada, o cronômetro é interrompido quando o serviço retorna e registra o tempo decorrido, juntamente com informações sobre o êxito ou falha.

Observe que a mensagem de log inclui o nome da classe e o nome do método. É uma boa prática para certificar-se de que as mensagens de log identifiquem qual parte do código do aplicativo de existência.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Agora para toda vez que o aplicativo corrigir fez uma chamada para o banco de dados SQL, você pode ver a chamada, o método de chamada, e exatamente quanto tempo ele levou.

![Consulta de banco de dados SQL nos logs](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se você vá pesquisando os logs, você pode ver que o tempo necessário de chamadas de banco de dados é variável. Que informações podem ser úteis: porque o aplicativo fizer isso, você pode analisar as tendências históricas no desempenho do serviço de banco de dados ao longo do tempo. Por exemplo, um serviço pode ser rápido na maioria das vezes, mas solicitações podem falhar ou respostas podem lento em determinados momentos do dia.

Você pode fazer a mesma coisa para o serviço de Blob – para cada vez que o aplicativo carrega um novo arquivo, há um log e você pode ver exatamente quanto tempo demorou para carregar cada arquivo.

![Log de carregamento de blob](monitoring-and-telemetry/_static/image23.png)

Ele é apenas algumas linhas de código para gravar cada vez que você chamar um serviço, e agora sempre que alguém diz que tiveram um problema, você sabe exatamente o que o problema foi, se ele foi um erro, ou mesmo se ele foi apenas em execução lenta adicionais. Você pode identificar a origem do problema sem precisar remoto em um servidor ou ative o log depois que o erro ocorre e esperamos que criá-la novamente.

## <a name="dependency-injection-in-the-fix-it-app"></a>Injeção de dependência na correção-aplicativo

Você talvez esteja se perguntando como o construtor de repositório no exemplo mostrado acima obtém o agente de log de implementação de interface:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar a interface para a implementação do aplicativo usa [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection)(DI) com [AutoFac](http://autofac.org/). DI permite usar um objeto com base em uma interface em vários locais em todo o código e só precisa especificar, em um só lugar, a implementação que é usada quando a interface é instanciada. Isso torna mais fácil alterar a implementação: por exemplo, pode ser que você deseja substituir o agente do System. Diagnostics com um agente de log NLog. Ou, para testes automatizados você talvez queira substituir uma versão fictícia do agente de log.

O aplicativo corrigir usa DI em todos os repositórios e todos os controladores. Os construtores de classes do controlador obtém um *ITaskRepository* interface da mesma forma que o repositório obtém uma interface de agente de log:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

O aplicativo usa a biblioteca de DI AutoFac para fornecer automaticamente *TaskRepository* e *agente* instâncias para estes construtores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Esse código basicamente diz que precisa de um construtor em qualquer lugar um *ILogger* interface, passe em uma instância do *agente* classe, e sempre que é necessário um *IFixItTaskRepository*interface, passe em uma instância do *FixItTaskRepository* classe.

[AutoFac](http://autofac.org/) é uma das várias estruturas de injeção de dependência que você pode usar. É outra popular [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), que é recomendado e suportado pela Microsoft Patterns e práticas recomendadas.

## <a name="built-in-logging-support-in-azure"></a>Suporte de registro em log internos no Azure

Azure suporta os seguintes tipos de [o log de aplicativos Web no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Rastreamento de System. Diagnostics (você pode ativar e desativar e definir níveis em tempo real sem a reinicialização do site).
- Eventos do Windows.
- Logs do IIS (HTTP/FREB).

Azure suporta os seguintes tipos de [log em serviços de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Rastreamento de System. Diagnostics.
- Contadores de desempenho.
- Eventos do Windows.
- Logs do IIS (HTTP/FREB).
- Monitoramento do diretório personalizado.

O aplicativo corrigir usa o rastreamento de System. Diagnostics. Tudo o que você precisa fazer para habilitar o registro em log em um aplicativo web de System. Diagnostics é acionar uma chave no portal ou chamar a API REST. No portal, clique o **configuração** guia para seu site e role para baixo para ver o **Application Diagnostics** seção. Você pode ativar ou desativar o registro em log e selecione o nível de log desejado. Você pode fazer com que o Azure gravar os logs para o sistema de arquivos ou para uma conta de armazenamento.

![Diagnóstico de aplicativo e de diagnóstico de site na guia Configurar](monitoring-and-telemetry/_static/image24.png)

Depois de habilitar o registro em log no Azure, você pode ver logs na janela de saída do Visual Studio como eles são criados.

![Menu de logs de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu de logs de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Você também pode ter os logs sejam gravados em sua conta de armazenamento e exibição-los com qualquer ferramenta que pode acessar o serviço de tabela de armazenamento do Azure, como **Server Explorer** no Visual Studio ou [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Logs no Gerenciador de servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumo

É muito simple de implementar um sistema de telemetria de fora da caixa, instrumentar o registro em log em seu próprio código e configurar o log no Azure. E, quando você tiver problemas de produção, a combinação de um sistema de telemetria e logs personalizados irá ajudá-lo a resolver problemas rapidamente antes que se tornem problemas principais para os clientes.

No [próximo capítulo](transient-fault-handling.md) , examinaremos como tratar erros transitórios para que não se tornem problemas de produção que você precisa investigar.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos.

Documentação principalmente sobre a telemetria:

- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/library/dn568099.aspx). Consulte a orientação de instrumentação e telemetria, diretrizes de serviço de medição, monitoramento da integridade do ponto de extremidade padrão e reconfiguração de tempo de execução padrão.
- [Mínima esmagamento na nuvem: habilitação de desempenho do New Relic monitoramento em sites do Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy. Consulte a seção de telemetria e diagnóstico.
- [Desenvolvimento de última geração com o Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artigo da MSDN Magazine.

Documentação principalmente sobre registro em log:

- [Bloco de aplicativo do log semântica (sessão)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie apresenta o caso para registro em log semântico com a sessão.
- [Criando estruturados e significativos de Logs com o log de semântico](https://channel9.msdn.com/Events/Build/2013/3-336). (Vídeo) Juliano Dominguez apresenta o caso para registro em log semântico com a sessão.
- [EF6 Log de SQL – parte 1: log Simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers mostra como fazer consultas executadas pelo Entity Framework 6 EF.
- [Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto uma série de tutoriais de nove partes, mostra como usar o recurso de interceptação de comando EF 6 para fazer o logon de comandos SQL enviados ao banco de dados pelo Entity Framework.
- [Melhorar o registro em log usando c# 5.0 chamador informações atributos](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Como registrar o nome do método chamado facilmente sem embuti-lo em literais ou usando a reflexão para obtê-lo manualmente.

Documentação principalmente sobre solução de problemas:

- [Solução de problemas do Azure &amp; depuração blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – o utilitário de diagnóstico usado pela equipe de suporte do Azure Developer](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Apresenta e fornece um link de download para uma ferramenta que pode ser usada em uma VM do Azure para baixar e executar uma ampla variedade de ferramentas de diagnóstico e monitoramentos. Útil quando você precisa diagnosticar um problema em uma VM específica.
- [Solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Um tutorial passo a passo para começar a trabalhar com o rastreamento de System. Diagnostics e depuração remota.

Vídeos:

- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta os conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, com histórias extraídas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Episódios 4 e 9 são sobre monitoramento e telemetria. Episódio 9 inclui uma visão geral do monitoramento de serviços MetricsHub, AppDynamics, New Relic e PagerDuty.
- [Criando Big: Lições aprendidas de clientes do Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre a criação da falha e instrumentação tudo. Semelhante ao entrar em mais detalhes instruções mas série à prova de falhas.

Exemplo de código:

- [Conceitos fundamentais do serviço no Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo criado pela equipe de consultoria ao cliente da Microsoft Azure. Demonstra a telemetria e as práticas recomendadas de registro em log, como explicado nos artigos a seguir. O exemplo implementa o log de aplicativo usando [NLog](http://nlog-project.org/). Para obter a documentação relacionada, consulte o [série de quatro artigos wiki do TechNet sobre a telemetria e o registro em log](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md)
> [Próximo](transient-fault-handling.md)
