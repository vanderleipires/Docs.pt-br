---
title: Referência de configuração de módulo principal do ASP.NET
author: guardrex
description: Saiba como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Referência de configuração de módulo principal do ASP.NET

Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este documento fornece instruções sobre como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos ASP.NET Core. Para obter uma introdução ao ASP.NET Core Module e instruções de instalação, consulte o [visão geral do módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configuração com a Web. config

O módulo de núcleo do ASP.NET está configurado com o `aspNetCore` seção o `system.webServer` nó do site *Web. config* arquivo.

O seguinte *Web. config* arquivo é publicado para uma [implantação dependentes de framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura o módulo de núcleo do ASP.NET para manipular solicitações de site:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

O seguinte *Web. config* é publicado para uma [implantação autossuficiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Quando um aplicativo é implantado em [do serviço de aplicativo do Azure](https://azure.microsoft.com/services/app-service/), o `stdoutLogFile` caminho é definido como `\\?\%home%\LogFiles\stdout`. O caminho salva stdout logs para o *LogFiles* pasta, que é um local automaticamente criada pelo serviço.

Consulte [configuração abaixo aplicativo](xref:host-and-deploy/iis/index#sub-application-configuration) para uma nota importante relativas à configuração do *Web. config* arquivos em subpastas de aplicativos.

### <a name="attributes-of-the-aspnetcore-element"></a>Atributos do elemento aspNetCore

::: moniker range="<= aspnetcore-2.0"
| Atributo | Descrição | Padrão |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atributo de cadeia de caracteres opcional.</p><p>Argumentos para o executável especificado em **processPath**.</p>| |
| `disableStartUpErrorPage` | verdadeiro ou falso.</p><p>Se for true, o **502.5 - falha do processo** página é suprimida, e a página de código de 502 status configuradas no *Web. config* terá precedência.</p> | `false` |
| `forwardWindowsAuthToken` | verdadeiro ou falso.</p><p>Se for true, o token é encaminhado para o processo filho escuta em % ASPNETCORE_PORT % como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação. É responsabilidade do processo para chamar CloseHandle neste token por solicitação.</p> | `true` |
| `processPath` | <p>Atributo de cadeia de caracteres obrigatória.</p><p>Caminho para o executável que inicia um processo que escuta solicitações HTTP. Há suporte para caminhos relativos. Se o caminho começa com `.`, o caminho é considerado em relação à raiz do site.</p> | |
| `rapidFailsPerMinute` | <p>Atributo inteiro opcional.</p><p>Especifica o número de vezes que o processo especificado na **processPath** pode falhar por minuto. Se esse limite for excedido, o módulo para iniciar o processo para o restante do minuto.</p> | `10` |
| `requestTimeout` | <p>Atributo timespan opcional.</p><p>Especifica a duração para a qual o módulo do ASP.NET Core aguarda uma resposta do processo que escuta em % ASPNETCORE_PORT %.</p><p>Em versões do módulo ASP.NET Core que acompanha a versão do ASP.NET Core 2.0 ou anterior, o `requestTimeout` deve ser especificado em minutos inteiros, caso contrário, o padrão é 2 minutos.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atributo inteiro opcional.</p><p>Duração em segundos que o módulo espera para o executável para desligar normalmente quando o *app_offline.htm* arquivo é detectado.</p> | `10` |
| `startupTimeLimit` | <p>Atributo inteiro opcional.</p><p>Duração em segundos que o módulo espera para o arquivo executável iniciar um processo que escuta na porta. Se esse tempo limite for excedido, o módulo elimina o processo. O módulo tentará reiniciar o processo quando ele recebe uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo não pode ser iniciado **rapidFailsPerMinute** vezes nos últimos minuto sem interrupção.</p> | `120` |
| `stdoutLogEnabled` | <p>Atributo booliano opcional.</p><p>Se true, **stdout** e **stderr** para o processo especificado na **processPath** são redirecionadas para o arquivo especificado em **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atributo de cadeia de caracteres opcional.</p><p>Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado na **processPath** são registrados em log. Caminhos relativos são relativo à raiz do site. Qualquer caminho começando com `.` estão em relação ao site raiz e todos os outros caminhos são tratados como caminhos absolutos. As pastas fornecidas no caminho devem existir para que o módulo criar o arquivo de log. Usando os delimitadores de sublinhado, um carimbo de hora, ID de processo e extensão de arquivo (*. log*) são adicionados para o último segmento do **stdoutLogFile** caminho. Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* no *logs* pasta quando salvos em 5/2/2018 às 19:41:32 com uma ID de processo de 1934.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Atributo | Descrição | Padrão |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atributo de cadeia de caracteres opcional.</p><p>Argumentos para o executável especificado em **processPath**.</p>| |
| `disableStartUpErrorPage` | verdadeiro ou falso.</p><p>Se for true, o **502.5 - falha do processo** página é suprimida, e a página de código de 502 status configuradas no *Web. config* terá precedência.</p> | `false` |
| `forwardWindowsAuthToken` | verdadeiro ou falso.</p><p>Se for true, o token é encaminhado para o processo filho escuta em % ASPNETCORE_PORT % como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação. É responsabilidade do processo para chamar CloseHandle neste token por solicitação.</p> | `true` |
| `processPath` | <p>Atributo de cadeia de caracteres obrigatória.</p><p>Caminho para o executável que inicia um processo que escuta solicitações HTTP. Há suporte para caminhos relativos. Se o caminho começa com `.`, o caminho é considerado em relação à raiz do site.</p> | |
| `rapidFailsPerMinute` | <p>Atributo inteiro opcional.</p><p>Especifica o número de vezes que o processo especificado na **processPath** pode falhar por minuto. Se esse limite for excedido, o módulo para iniciar o processo para o restante do minuto.</p> | `10` |
| `requestTimeout` | <p>Atributo timespan opcional.</p><p>Especifica a duração para a qual o módulo do ASP.NET Core aguarda uma resposta do processo que escuta em % ASPNETCORE_PORT %.</p><p>Em versões do módulo ASP.NET Core que acompanha a versão do ASP.NET Core 2.1 ou posterior, o `requestTimeout` é especificado em horas, minutos e segundos.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atributo inteiro opcional.</p><p>Duração em segundos que o módulo espera para o executável para desligar normalmente quando o *app_offline.htm* arquivo é detectado.</p> | `10` |
| `startupTimeLimit` | <p>Atributo inteiro opcional.</p><p>Duração em segundos que o módulo espera para o arquivo executável iniciar um processo que escuta na porta. Se esse tempo limite for excedido, o módulo elimina o processo. O módulo tentará reiniciar o processo quando ele recebe uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo não pode ser iniciado **rapidFailsPerMinute** vezes nos últimos minuto sem interrupção.</p> | `120` |
| `stdoutLogEnabled` | <p>Atributo booliano opcional.</p><p>Se true, **stdout** e **stderr** para o processo especificado na **processPath** são redirecionadas para o arquivo especificado em **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atributo de cadeia de caracteres opcional.</p><p>Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado na **processPath** são registrados em log. Caminhos relativos são relativo à raiz do site. Qualquer caminho começando com `.` estão em relação ao site raiz e todos os outros caminhos são tratados como caminhos absolutos. As pastas fornecidas no caminho devem existir para que o módulo criar o arquivo de log. Usando os delimitadores de sublinhado, um carimbo de hora, ID de processo e extensão de arquivo (*. log*) são adicionados para o último segmento do **stdoutLogFile** caminho. Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* no *logs* pasta quando salvos em 5/2/2018 às 19:41:32 com uma ID de processo de 1934.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Definir variáveis de ambiente

Variáveis de ambiente podem ser especificadas para o processo no `processPath` atributo. Especificar uma variável de ambiente com o `environmentVariable` elemento filho de um `environmentVariables` elemento da coleção. Variáveis de ambiente definidas nesta seção têm precedência sobre o sistema, variáveis de ambiente.

O exemplo a seguir define duas variáveis de ambiente. `ASPNETCORE_ENVIRONMENT` configura o ambiente do aplicativo para `Development`. Um desenvolvedor pode definir esse valor temporariamente *Web. config* arquivo para forçar o [página de exceção de desenvolvedor](xref:fundamentals/error-handling) carregar ao depurar uma exceção de aplicativo. `CONFIG_DIR` é um exemplo de uma variável de ambiente definidas pelo usuário, em que o desenvolvedor tenha gravado código que lê o valor de inicialização para formar um caminho para carregar o arquivo de configuração do aplicativo.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> Apenas definir o `ASPNETCORE_ENVIRONMENT` envirnonment variável `Development` no preparo e teste de servidores que não estão acessíveis a redes não confiáveis, como a Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Se um arquivo com o nome *app_offline.htm* é detectado no diretório raiz de um aplicativo, o módulo do ASP.NET Core tenta desligar normalmente o aplicativo e parar o processamento de solicitações de entrada. Se o aplicativo ainda está em execução após o número de segundos definido no `shutdownTimeLimit`, o módulo do ASP.NET Core elimina o processo em execução.

Enquanto o *app_offline.htm* arquivo estiver presente, o módulo do ASP.NET Core responde às solicitações enviando o conteúdo de *app_offline.htm* arquivo. Quando o *app_offline.htm* arquivo é removido, a próxima solicitação inicia o aplicativo.

## <a name="start-up-error-page"></a>Página de erro de inicialização

Se o módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou o início do processo de back-end, mas não escutar na porta configurada, um *502.5 falha no processo* página de código de status será exibida. Para omitir esta página e reverter para a página de código de status de IIS 502 padrão, use o `disableStartUpErrorPage` atributo. Para obter mais informações sobre como configurar mensagens de erro personalizadas, consulte [erros HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 página de código de Status de falha do processo](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Criação de log e de redirecionamento

O módulo do ASP.NET Core redireciona stdout e stderr logs no disco se o `stdoutLogEnabled` e `stdoutLogFile` atributos do `aspNetCore` elemento são definidas. As pastas no `stdoutLogFile` caminho deve existir para que o módulo criar o arquivo de log. O pool de aplicativos deve ter acesso de gravação para o local onde os logs são gravados (usar `IIS AppPool\<app_pool_name>` para fornecer permissão de gravação).

Logs não são girados, a menos que ocorra a reciclagem de processo/reinício. É responsabilidade do hoster para limitar o espaço em disco que consomem os logs.

Usando o log de stdout é recomendado apenas para solucionar problemas de inicialização do aplicativo. Não use o log de stdout para fins de registro do aplicativo geral. Para log de rotina no aplicativo do ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e gira logs. Para obter mais informações, consulte [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).

Uma extensão de arquivo e o carimbo de hora são adicionados automaticamente quando o arquivo de log é criado. O nome do arquivo de log é composto por meio do acréscimo de carimbo de hora, a ID do processo e a extensão de arquivo (*. log*) para o último segmento do `stdoutLogFile` caminho (normalmente *stdout*) delimitados por sublinhados. Se o `stdoutLogFile` caminho termina com *stdout*, um log para um aplicativo com um PID de 1934 criada em 5/2/2018 às 19:42:32 tem o nome de arquivo *stdout_20180205194132_1934.log*.

O exemplo a seguir `aspNetCore` elemento configura stdout registro em log para um aplicativo hospedado no serviço de aplicativo do Azure. Um caminho local ou um caminho de compartilhamento de rede é aceitável para o registro local. Confirme se a identidade do usuário AppPool tem permissão para gravar o caminho fornecido.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Consulte [configuração com a Web. config](#configuration-with-webconfig) para obter um exemplo de `aspNetCore` elemento no *Web. config* arquivo.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>A configuração de proxy usa o protocolo HTTP e um token de emparelhamento

O proxy criado entre o módulo do ASP.NET Core e Kestrel usa o protocolo HTTP. Usando HTTP é uma otimização de desempenho, onde o tráfego entre o módulo e Kestrel ocorre em um endereço de loopback da interface de rede. Não há nenhum risco de espionagem o tráfego entre o módulo e Kestrel de um local fora do servidor.

Um token de emparelhamento é usado para assegurar que as solicitações recebidas pelo Kestrel foram transmitidas por proxy pelo IIS e que não são provenientes de outra origem. O token de emparelhamento é criado e definido em uma variável de ambiente (`ASPNETCORE_TOKEN`) pelo módulo. O token de emparelhamento também é definido em um cabeçalho (`MSAspNetCoreToken`) em cada solicitação com proxy. O Middleware do IIS verifica cada solicitação recebida para confirmar se o valor de cabeçalho do token de emparelhamento corresponde ao valor da variável de ambiente. Se os valores do token forem incompatíveis, a solicitação será registrada em log e rejeitada. A variável de ambiente token emparelhamento e o tráfego entre o módulo e Kestrel não são acessíveis a partir de um local fora do servidor. Sem saber o valor do token de emparelhamento, um invasor não pode enviar solicitações que ignoram a verificação no Middleware do IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Configuração do módulo principal do ASP.NET com um IIS compartilhada

O instalador do módulo de núcleo do ASP.NET é executado com os privilégios do **sistema** conta. Como a conta sistema local não ter modificar permissões para o caminho do compartilhamento usado por configuração compartilhada de IIS, o instalador atinge um erro de acesso negado ao tentar definir as configurações de módulo em *applicationHost. config* no compartilhamento. Ao usar uma configuração compartilhada de IIS, siga estas etapas:

1. Desabilite a configuração compartilhada de IIS.
1. Execute o instalador.
1. Exportar atualizado *applicationHost. config* arquivo para o compartilhamento.
1. Habilite novamente a configuração compartilhada de IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Versão do módulo e logs do instalador do pacote de hospedagem

Para determinar a versão do ASP.NET Core módulo instalado:

1. No sistema de hospedagem, navegue até *%windir%\system32\inetsrv*.
1. Localize o *aspnetcore.dll* arquivo.
1. Clique no arquivo e selecione **propriedades** no menu contextual.
1. Selecione o **detalhes** guia. O **versão do arquivo** e **versão do produto** representar a versão instalada do módulo.

Os logs de instalador do pacote de hospedagem para o módulo são encontrados no *c:\\usuários\\% UserName %\\AppData\\Local\\Temp*. O arquivo é nomeado *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Locais de arquivo do módulo, o esquema e a configuração

### <a name="module"></a>Módulo

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**O IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Esquema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Configuração

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Os arquivos podem ser encontrados pesquisando *aspnetcore.dll* no *applicationHost. config* arquivo. Para o IIS Express, o *applicationHost. config* arquivo não existe por padrão. O arquivo é criado no  *\<application_root >\\VS\\config* ao iniciar qualquer projeto de aplicativo web na solução do Visual Studio.
