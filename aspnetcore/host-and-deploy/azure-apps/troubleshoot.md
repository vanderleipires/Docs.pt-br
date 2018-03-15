---
title: "Solucionar problemas de ASP.NET Core no serviço de aplicativo do Azure"
author: guardrex
description: "Saiba como diagnosticar problemas com implantações do Serviço de Aplicativo do Azure do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: e6a8404d3fe96a0136d7f874107b2cdf63e8e890
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Solucionar problemas de ASP.NET Core no serviço de aplicativo do Azure

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Este artigo fornece instruções sobre como diagnosticar uma ASP.NET Core problema de inicialização do aplicativo usando ferramentas de diagnóstico do serviço de aplicativo do Azure. Para avisos de solução de problemas adicionais, consulte [visão geral do serviço de aplicativo do Azure diagnostics](/azure/app-service/app-service-diagnostics) e [como: monitorar aplicativos no serviço de aplicativo do Azure](/azure/app-service/web-sites-monitor) na documentação do Azure.

## <a name="app-startup-errors"></a>Erros de inicialização do aplicativo

**502.5 falha de processo**  
O processo de trabalho falha. O aplicativo não foi iniciada.

O [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) tentativas de iniciar o processo de trabalho, mas ele não pode ser iniciado. Examinar o Log de eventos do aplicativo geralmente ajuda a solucionar esse tipo de problema. Acessar o log é explicado no [Log de eventos do aplicativo](#application-event-log) seção.

O *502.5 falha no processo* página de erro é retornada quando um aplicativo configurado incorretamente faz com que a falha do processo de trabalho:

![Janela do navegador mostrando a página de falha do processo 502.5](troubleshoot/_static/process-failure-page.png)

**Erro de servidor interno 500**  
O aplicativo é iniciado, mas um erro impede que o servidor de atender à solicitação.

Esse erro ocorre no código do aplicativo durante a inicialização ou durante a criação de uma resposta. A resposta não poderá conter nenhum conteúdo ou a resposta pode ser exibido como um *500 Erro interno do servidor* no navegador. O Log de eventos do aplicativo geralmente indica que o aplicativo é iniciado normalmente. Da perspectiva do servidor, que está correta. O aplicativo foi iniciado, mas não pode gerar uma resposta válida. [Execute o aplicativo no console do Kudu](#run-the-app-in-the-kudu-console) ou [habilite o log do módulo do ASP.NET Core stdout](#aspnet-core-module-stdout-log) para solucionar o problema.

**Redefinição de Conexão**

Se um erro ocorrer após os cabeçalhos são enviados, é muito tarde para o servidor enviar um **500 Erro interno do servidor** quando ocorre um erro. Isso geralmente acontece quando ocorre um erro durante a serialização de objetos complexos por uma resposta. Esse tipo de erro é exibida como uma *redefinição de conexão* erro no cliente. [Log de aplicativo](xref:fundamentals/logging/index) pode ajudar a solucionar esses tipos de erros.

## <a name="default-startup-limits"></a>Limites de inicialização padrão

O módulo de núcleo do ASP.NET está configurado com um padrão *startupTimeLimit* de 120 segundos. Quando deixada no valor padrão, um aplicativo pode levar até dois minutos para ser iniciado antes do módulo faz uma falha do processo. Para obter informações sobre como configurar o módulo, consulte [atributos do elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Solucionar problemas de inicialização do aplicativo

### <a name="application-event-log"></a>Log de eventos do aplicativo

Para acessar o Log de eventos do aplicativo, use o **diagnosticar e resolver problemas** folha no portal do Azure:

1. No portal do Azure, abra a folha do aplicativo no **serviços de aplicativos** folha.
1. Selecione o **diagnosticar e resolver problemas** folha.
1. Em **Selecionar categoria de problema**, selecione o **aplicativo Web para baixo** botão.
1. Em **soluções sugeridas**, abra o painel para **abrir Logs de eventos do aplicativo**. Selecione o **abrir Logs de eventos de aplicativo** botão.
1. Examine o erro mais recente fornecido pelo *IIS AspNetCoreModule* no **fonte** coluna.

Uma alternativa ao uso de **diagnosticar e resolver problemas** folha é examinar o arquivo de Log de eventos do aplicativo diretamente usando [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Selecione o **ferramentas avançadas de** folha no **ferramentas de desenvolvimento** área. Selecione o **vá&rarr;**  botão. O console do Kudu é aberto em uma janela ou nova guia do navegador.
1. Usando a barra de navegação na parte superior da página, abra **console de depuração** e selecione **CMD**.
1. Abra o **LogFiles** pasta.
1. Selecione o ícone de lápis ao lado de *eventlog.xml* arquivo.
1. Examine o log. Role até o final do log para ver os eventos mais recentes.

### <a name="run-the-app-in-the-kudu-console"></a>Execute o aplicativo no console do Kudu

Muitos erros de inicialização não produzem informações úteis no Log de eventos do aplicativo. Você pode executar o aplicativo no [Kudu](https://github.com/projectkudu/kudu/wiki) Console remoto de execução para descobrir o erro:

1. Selecione o **ferramentas avançadas de** folha no **ferramentas de desenvolvimento** área. Selecione o **vá&rarr;**  botão. O console do Kudu é aberto em uma janela ou nova guia do navegador.
1. Usando a barra de navegação na parte superior da página, abra **console de depuração** e selecione **CMD**.
1. Abra as pastas no caminho **site** > **wwwroot**.
1. No console do, execute o aplicativo com a execução de assembly do aplicativo.
   * Se o aplicativo for um [implantação dependentes de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), executar o assembly do aplicativo com *dotnet.exe*. No comando a seguir, substitua o nome do assembly do aplicativo para `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Se o aplicativo for um [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd), execute o aplicativo do executável. No comando a seguir, substitua o nome do assembly do aplicativo para `<assembly_name>`: `<assembly_name>.exe`
1. A saída do aplicativo, mostrando os erros do console está conectado ao console Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Log de stdout de módulo principal do ASP.NET

O módulo do ASP.NET Core stdout geralmente registra mensagens de erro úteis não encontradas no Log de eventos do aplicativo. Para habilitar e exibir logs de stdout:

1. Navegue até o **diagnosticar e resolver problemas** folha no portal do Azure.
1. Em **Selecionar categoria de problema**, selecione o **aplicativo Web para baixo** botão.
1. Em **soluções sugeridas** > **habilitar o redirecionamento de Log Stdout**, selecione o botão de **abrir o Console do Kudu para editar o Web. config**.
1. No Kudu **Console diagnóstico**, abra as pastas no caminho **site** > **wwwroot**. Role para baixo para revelar o *Web. config* arquivo na parte inferior da lista.
1. Clique no ícone de lápis ao lado de *Web. config* arquivo.
1. Definir **stdoutLogEnabled** para `true` e altere o **stdoutLogFile** caminho: `\\?\%home%\LogFiles\stdout`.
1. Selecione **salvar** para salvar o documento atualizado *Web. config* arquivo.
1. Fazer uma solicitação para o aplicativo.
1. Retorne ao portal do Azure. Selecione o **ferramentas avançadas de** folha no **ferramentas de desenvolvimento** área. Selecione o **vá&rarr;**  botão. O console do Kudu é aberto em uma janela ou nova guia do navegador.
1. Usando a barra de navegação na parte superior da página, abra **console de depuração** e selecione **CMD**.
1. Selecione o **LogFiles** pasta.
1. Inspecione o **modificadas** coluna e selecione o ícone de lápis para editar o stdout, faça logon com a data da última modificação.
1. Quando o arquivo de log é aberto, o erro é exibido.

**Importante!** Desabilite stdout registro em log quando o problema for solucionado.

1. No Kudu **Console diagnóstico**, retorne para o caminho **site** > **wwwroot** para revelar o *Web. config* arquivo. Abra o **Web. config** arquivo novamente, selecionando o ícone de lápis.
1. Definir **stdoutLogEnabled** para `false`.
1. Selecione **salvar** para salvar o arquivo.

> [!WARNING]
> Falha ao desabilitar o log de stdout pode levar a falhas de aplicativo ou servidor. Não há limites para o tamanho do arquivo de log ou para o número de arquivos de log criados.
>
> Para log de rotina no aplicativo do ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e gira logs. Para obter mais informações, consulte [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Erros comuns de inicialização 

Consulte o [referência de erros comuns do ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). A maioria dos problemas comuns que impedem a inicialização do aplicativo é abordada no tópico de referência.

## <a name="slow-or-hanging-app"></a>Aplicativo lento ou deslocado

Quando um aplicativo responde lentamente ou trava em uma solicitação, consulte [solucionar problemas de desempenho de aplicativo web lenta no serviço de aplicativo do Azure](/azure/app-service/app-service-web-troubleshoot-performance-degradation) para orientação de depuração.

## <a name="remote-debugging"></a>Depuração remota

Consulte os tópicos a seguir:

* [Seção de aplicativos da web de solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio de depuração remota](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (documentação do Azure)
* [Remota de depuração ASP.NET Core no IIS no Azure no Visual Studio de 2017](/visualstudio/debugger/remote-debugging-azure) (documentação do Visual Studio)

## <a name="application-insights"></a>Informações do aplicativo

[Application Insights](https://azure.microsoft.com/services/application-insights/) fornece a telemetria de aplicativos hospedados no serviço de aplicativo do Azure, incluindo o log de erros e recursos de relatório. Application Insights só pode relatar erros ocorridos depois que o aplicativo é iniciado quando recursos de log do aplicativo se tornar disponíveis. Para obter mais informações, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Folhas de monitoramentos

Folhas de monitoramentos fornecem uma alternativa experiência para os métodos descritos anteriormente no tópico de solução de problemas. Essas folhas podem ser usadas para diagnosticar erros 500.

Confirme que as extensões de núcleo do ASP.NET estão instaladas. Se as extensões não estiverem instaladas, instale-os manualmente:

1. No **ferramentas de desenvolvimento** seção de folha, selecione o **extensões** folha.
1. O **ASP.NET Core extensões** devem aparecer na lista.
1. Se as extensões não estão instaladas, selecione o **adicionar** botão.
1. Escolha o **ASP.NET Core extensões** da lista.
1. Selecione **Okey** para aceitar os termos legais.
1. Selecione **Okey** no **Adicionar extensão** folha.
1. Uma mensagem pop-up informativa indica quando as extensões são instaladas com êxito.

Se o log de stdout não está habilitado, siga estas etapas:

1. No portal do Azure, selecione o **ferramentas avançadas de** folha no **ferramentas de desenvolvimento** área. Selecione o **vá&rarr;**  botão. O console do Kudu é aberto em uma janela ou nova guia do navegador.
1. Usando a barra de navegação na parte superior da página, abra **console de depuração** e selecione **CMD**.
1. Abra as pastas no caminho **site** > **wwwroot** e role para baixo para revelar o *Web. config* arquivo na parte inferior da lista.
1. Clique no ícone de lápis ao lado de *Web. config* arquivo.
1. Definir **stdoutLogEnabled** para `true` e altere o **stdoutLogFile** caminho: `\\?\%home%\LogFiles\stdout`.
1. Selecione **salvar** para salvar o documento atualizado *Web. config* arquivo.

Vá para ativar o log de diagnóstico:

1. No portal do Azure, selecione o **logs de diagnóstico** folha.
1. Selecione o **na** alternar **(sistema de arquivos) de log de aplicativo** e **mensagens de erro detalhadas**. Selecione o **salvar** botão na parte superior da folha.
1. Para incluir o rastreamento de solicitação com falha, também conhecido como registro em log de falha na solicitação evento buffer (FREB), selecione o **na** alternar **o rastreamento de solicitação com falha**. 
1. Selecione o **fluxo de Log** folha, que é listada imediatamente sob o **logs de diagnóstico** folha no portal.
1. Fazer uma solicitação para o aplicativo.
1. Dentro dos dados de fluxo de log, a causa do erro é indicada.

**Importante!** Certifique-se de desabilitar stdout registro em log quando o problema for solucionado. Consulte as instruções de [log do módulo do ASP.NET Core stdout](#aspnet-core-module-stdout-log) seção.

Para exibir os logs de rastreamento de solicitação com falha (logs FREB):

1. Navegue até o **diagnosticar e resolver problemas** folha no portal do Azure.
1. Selecione **falha os Logs de rastreamento de solicitação** do **ferramentas de suporte** área de barra lateral.

Consulte [seção o habilite o log de diagnóstico para aplicativos web no tópico de serviço de aplicativo do Azure de rastreamentos de solicitação com falha](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) e [perguntas frequentes do desempenho do aplicativo para aplicativos Web no Azure: como ativar o rastreamento de solicitação com falha?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Para obter mais informações.

Para obter mais informações, consulte [habilitar o log de diagnóstico para aplicativos web no serviço de aplicativo do Azure](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Falha ao desabilitar o log de stdout pode levar a falhas de aplicativo ou servidor. Não há limites para o tamanho do arquivo de log ou para o número de arquivos de log criados.
>
> Para log de rotina no aplicativo do ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e gira logs. Para obter mais informações, consulte [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Recursos adicionais

* [Introdução ao tratamento de erro no ASP.NET Core](xref:fundamentals/error-handling)
* [Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Solucionar problemas de erros HTTP de "502 gateway incorreto" e "503 Serviço indisponível" em seus aplicativos web do Azure](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Solucionar problemas de desempenho do aplicativo web lenta no serviço de aplicativo do Azure](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Perguntas frequentes do desempenho do aplicativo para aplicativos Web no Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Área restrita do aplicativo Web do Azure (limitações de execução de tempo de execução do serviço de aplicativo)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: experiência de diagnóstico e solução de problemas do Serviço de Aplicativo do Azure (vídeo com 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
