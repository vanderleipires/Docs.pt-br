---
title: "Solucionar problemas de núcleo do ASP.NET no IIS"
author: guardrex
description: "Saiba como diagnosticar problemas com implantações de serviços de informações da Internet (IIS) de aplicativos do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Solucionar problemas de núcleo do ASP.NET no IIS

Por [Luke Latham](https://github.com/guardrex)

Este artigo fornece instruções sobre como diagnosticar uma ASP.NET Core problema de inicialização do aplicativo ao hospedar com [serviços de informações da Internet (IIS)](/iis). As informações neste artigo se aplica ao hospedar no IIS no Windows Server e Windows Desktop.

No Visual Studio, um projeto do ASP.NET Core padrão [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hospedagem durante a depuração. Um *502.5 falha no processo* que ocorre quando a depuração local pode ser troubleshooted usando o aviso neste tópico.

Tópicos de solução de problemas adicionais:

[Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure](xref:host-and-deploy/azure-apps/troubleshoot)  
Embora o serviço de aplicativo usa o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) e IIS para hospedar aplicativos, consulte o tópico dedicado para obter instruções específicas para o serviço de aplicativo.

[Tratamento de erro](xref:fundamentals/error-handling)  
Descobrir como tratar erros em aplicativos do ASP.NET Core durante o desenvolvimento em um sistema local.

[Aprenda a depurar usando o Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
Este tópico apresenta os recursos do depurador do Visual Studio.

## <a name="app-startup-errors"></a>Erros de inicialização do aplicativo

**502.5 falha de processo**  
O processo de trabalho falha. O aplicativo não foi iniciada.

O módulo do ASP.NET Core tenta iniciar o processo de trabalho, mas falhar ao iniciar. A causa de uma falha de inicialização do processo geralmente pode ser determinada de entradas a [Log de eventos do aplicativo](#application-event-log) e [log do módulo do ASP.NET Core stdout](#aspnet-core-module-stdout-log).

O *502.5 falha no processo* página de erro é retornada quando um erro de configuração de hospedagem ou o aplicativo faz com que a falha do processo de trabalho:

![Janela do navegador mostrando a página de falha do processo 502.5](troubleshoot/_static/process-failure-page.png)

**Erro de servidor interno 500**  
O aplicativo é iniciado, mas um erro impede que o servidor de atender à solicitação.

Esse erro ocorre no código do aplicativo durante a inicialização ou durante a criação de uma resposta. A resposta não poderá conter nenhum conteúdo ou a resposta pode ser exibido como um *500 Erro interno do servidor* no navegador. O Log de eventos do aplicativo geralmente indica que o aplicativo é iniciado normalmente. Da perspectiva do servidor, que está correta. O aplicativo foi iniciado, mas não pode gerar uma resposta válida. [Executar o aplicativo em um prompt de comando](#run-the-app-at-a-command-prompt) no servidor ou [habilite o log do módulo do ASP.NET Core stdout](#aspnet-core-module-stdout-log) para solucionar o problema.

**Redefinição de Conexão**

Se um erro ocorrer após os cabeçalhos são enviados, é muito tarde para o servidor enviar um **500 Erro interno do servidor** quando ocorre um erro. Isso geralmente acontece quando ocorre um erro durante a serialização de objetos complexos por uma resposta. Esse tipo de erro é exibida como uma *redefinição de conexão* erro no cliente. [Log de aplicativo](xref:fundamentals/logging/index) pode ajudar a solucionar esses tipos de erros.

## <a name="default-startup-limits"></a>Limites de inicialização padrão

O módulo de núcleo do ASP.NET está configurado com um padrão *startupTimeLimit* de 120 segundos. Quando deixada no valor padrão, um aplicativo pode levar até dois minutos para ser iniciado antes do módulo faz uma falha do processo. Para obter informações sobre como configurar o módulo, consulte [atributos do elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Solucionar problemas de inicialização do aplicativo

### <a name="application-event-log"></a>Log de eventos do aplicativo

Acesse o Log de eventos do aplicativo:

1. Abra o menu Iniciar, procure **Visualizador de eventos**e, em seguida, selecione o **Visualizador de eventos** aplicativo.
1. Em **Visualizador de eventos**, abra o **Logs do Windows** nó.
1. Selecione **aplicativo** para abrir o Log de eventos do aplicativo.
1. Procure erros associados com o aplicativo com falha. Erros têm um valor de *IIS AspNetCore módulo* ou *módulo do IIS Express AspNetCore* no *fonte* coluna.

### <a name="run-the-app-at-a-command-prompt"></a>Executar o aplicativo em um prompt de comando

Muitos erros de inicialização não produzem informações úteis no Log de eventos do aplicativo. Você pode encontrar a causa de alguns erros ao executar o aplicativo em um prompt de comando no sistema de hospedagem.

**Dependente de estrutura de implantação**

Se o aplicativo for um [implantação dependentes de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Em um prompt de comando, navegue até a pasta de implantação e executar o aplicativo com a execução de assembly do aplicativo com *dotnet.exe*. No comando a seguir, substitua o nome do assembly do aplicativo para \<nome_do_assembly >: `dotnet .\<assembly_name>.dll`.
1. A saída do aplicativo, mostrando os erros do console é escrito na janela de console.
1. Se o erro ocorrer ao fazer uma solicitação para o aplicativo, fazer uma solicitação para o host e a porta onde Kestrel escuta. Usando o host padrão e o post, fazer uma solicitação para `http://localhost:5000/`. Se o aplicativo normalmente responde no endereço de ponto de extremidade Kestrel, o problema é mais provável relacionados à configuração de proxy reverso e menos provável dentro do aplicativo.

**Independente de implantação**

Se o aplicativo for um [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Em um prompt de comando, navegue até a pasta de implantação e execute o executável do aplicativo. No comando a seguir, substitua o nome do assembly do aplicativo para \<nome_do_assembly >: `<assembly_name>.exe`.
1. A saída do aplicativo, mostrando os erros do console é escrito na janela de console.
1. Se o erro ocorrer ao fazer uma solicitação para o aplicativo, fazer uma solicitação para o host e a porta onde Kestrel escuta. Usando o host padrão e o post, fazer uma solicitação para `http://localhost:5000/`. Se o aplicativo normalmente responde no endereço de ponto de extremidade Kestrel, o problema é mais provável relacionados à configuração de proxy reverso e menos provável dentro do aplicativo.

### <a name="aspnet-core-module-stdout-log"></a>Log de stdout de módulo principal do ASP.NET

Para habilitar e exibir logs de stdout:

1. Navegue até a pasta de implantação do site no sistema de hospedagem.
1. Se o *logs* pasta não estiver presente, crie a pasta. Para obter instruções sobre como habilitar o MSBuild criar o *logs* pasta na implantação automaticamente, consulte o [estrutura de diretórios](xref:host-and-deploy/directory-structure) tópico.
1. Editar o *Web. config* arquivo. Definir **stdoutLogEnabled** para `true` e altere o **stdoutLogFile** path para apontar para o *logs* pasta (por exemplo, `.\logs\stdout`). `stdout`o caminho é o prefixo de nome de arquivo de log. Um carimbo de hora, a id do processo e a extensão de arquivo são adicionadas automaticamente quando o log é criado. Usando `stdout` como o prefixo de nome de arquivo, um arquivo de log típico é nomeado *stdout_20180205184032_5412.log*. 
1. Salvar o documento atualizado *Web. config* arquivo.
1. Fazer uma solicitação para o aplicativo.
1. Navegue até o *logs* pasta. Localize e abra o log de stdout mais recente.
1. Estude o log de erros.

**Importante!** Desabilite stdout registro em log quando o problema for solucionado.

1. Editar o *Web. config* arquivo.
1. Definir **stdoutLogEnabled** para `false`.
1. Salve o arquivo.

> [!WARNING]
> Falha ao desabilitar o log de stdout pode levar a falhas de aplicativo ou servidor. Não há nenhum limite no tamanho do arquivo de log ou o número de arquivos de log criados.
>
> Para log de rotina no aplicativo do ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e gira logs. Para obter mais informações, consulte [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Habilitando a página de exceção do desenvolvedor

O `ASPNETCORE_ENVIRONMENT` [variável de ambiente pode ser adicionada ao Web. config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) para executar o aplicativo no ambiente de desenvolvimento. Como o ambiente não é substituído na inicialização do aplicativo por `UseEnvironment` no construtor de host, definindo a variável de ambiente permite que o [página de exceção de desenvolvedor](xref:fundamentals/error-handling) aparecem quando o aplicativo é executado.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

Definir a variável de ambiente para `ASPNETCORE_ENVIRONMENT` só é recomendado para uso no preparo e teste de servidores que não são expostos à Internet. Remova a variável de ambiente a *Web. config* arquivo após a solução de problemas. Para obter informações sobre como definir variáveis de ambiente *Web. config*, consulte [environmentVariables elemento filho aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Erros comuns de inicialização 

Consulte o [referência de erros comuns do ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). A maioria dos problemas comuns que impedem a inicialização do aplicativo é abordada no tópico de referência.

## <a name="slow-or-hanging-app"></a>Aplicativo lento ou deslocado

Quando um aplicativo responde lentamente ou trava em uma solicitação, obter e analisar um [arquivo de despejo](/visualstudio/debugger/using-dump-files). Arquivos de despejo podem ser obtidos usando qualquer uma das ferramentas a seguir:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [baixar as ferramentas de depuração para Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [depuração usando o WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Depuração remota

Consulte [remota de depuração ASP.NET Core em um computador de IIS remoto no Visual Studio de 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) na documentação do Visual Studio.

## <a name="application-insights"></a>Informações do aplicativo

[Application Insights](/azure/application-insights/) fornece a telemetria de aplicativos hospedados pelo IIS, incluindo recursos de relatório e log de erros. Application Insights só pode relatar erros ocorridos depois que o aplicativo é iniciado quando recursos de log do aplicativo se tornar disponíveis. Para obter mais informações, consulte [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Avisos de solução de problemas adicionais

Às vezes, um aplicativo está funcionando falha imediatamente após a atualização ou o SDK do .NET Core nas versões de pacote ou de máquina de desenvolvimento dentro do aplicativo. Em alguns casos, pacotes incoerentes podem interromper um aplicativo ao executar atualizações principais. A maioria desses problemas pode ser corrigida seguindo estas instruções:

1. Excluir o *bin* e *obj* pastas.
1. Limpe o pacote armazena em cache em *% UserProfile %\\. NuGet\\pacotes* e *% LocalAppData %\\Nuget\\v3 cache*.
1. Restaurar e recompilar o projeto.
1. Confirme que a implantação anterior no servidor foi completamente excluída antes de reimplantar o aplicativo.

> [!TIP]
> Uma maneira conveniente para Limpar caches do pacote é executar `dotnet nuget locals all --clear` em um prompt de comando.
> 
> Limpar os caches de pacote também pode ser feito usando o [nuget.exe](https://www.nuget.org/downloads) ferramenta e executar o comando `nuget locals all -clear`. *NuGet.exe* não é uma instalação de pacote com o sistema operacional da área de trabalho do Windows e devem ser obtidos separadamente do [NuGet site](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Recursos adicionais

* [Introdução ao tratamento de erro no ASP.NET Core](xref:fundamentals/error-handling)
* [Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure](xref:host-and-deploy/azure-apps/troubleshoot)
