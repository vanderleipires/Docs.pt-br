---
title: "Referência de configuração de módulo principal do ASP.NET"
author: guardrex
description: "Como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos do ASP.NET Core."
keywords: "ASP.NET Core, ancm, módulo principal, iis, o log stdout, variável de ambiente, env var, subapplication, subapp, appoffline, app_offline, 502, de esquema"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: 44fc8bd647ad869dd029d8ca4ced782962d71020
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-module-configuration-reference"></a>Referência de configuração de módulo principal do ASP.NET

Por [Luke Latham](https://github.com/GuardRex), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este documento fornece detalhes sobre como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos do ASP.NET Core. Para obter uma introdução ao ASP.NET Core Module e instruções de instalação, consulte o [visão geral do módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Configuração por meio do Web. config

O módulo do ASP.NET Core é configurado por meio de um site ou aplicativo *Web. config* de arquivo e tem seu próprio `aspNetCore` seção de configuração em `system.webServer`. Aqui está um exemplo *Web. config* de arquivos que o `Microsoft.NET.Sdk.Web` SDK serão fornecidas quando o projeto é publicado para uma [implantação dependente do framework](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) com espaços reservados para o `processPath` e `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

O *Web. config* exemplo a seguir é para um [implantação autossuficiente](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) para o [do serviço de aplicativo do Azure](https://azure.microsoft.com/services/app-service/). Para obter mais informações, consulte [publicar no IIS](xref:publishing/iis). Consulte [configuração dos aplicativos sub](xref:publishing/iis#configuration-of-sub-applications) para uma nota importante relativas à configuração do *Web. config* arquivos em subdiretórios.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Atributos do elemento aspNetCore

| Atributo | Descrição |
| --- | --- |
| processPath | <p>Atributo de cadeia de caracteres obrigatória.</p><p>Caminho para o executável que iniciará um processo que escuta solicitações HTTP. Há suporte para caminhos relativos. Se o caminho começa com '.', o caminho é considerado em relação à raiz do site.</p><p>Nenhum valor padrão.</p> |
| arguments | <p>Atributo de cadeia de caracteres opcional.</p><p>Argumentos para o executável especificado em **processPath**.</p><p>O valor padrão é uma cadeia de caracteres vazia.</p> |
| startupTimeLimit | <p>Atributo inteiro opcional.</p><p>Duração em segundos que o módulo irá aguardar o executável iniciar um processo que escuta na porta. Se esse tempo limite for excedido, o módulo irá interromper o processo. O módulo tentará iniciar o processo novamente quando ele recebe uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhar ao iniciar **rapidFailsPerMinute** número vezes no último minuto sem interrupção.</p><p>O valor padrão é 120.</p> |
| shutdownTimeLimit | <p>Atributo inteiro opcional.</p><p>Duração em segundos para o qual o módulo irá aguardar o executável para desligar normalmente quando o *app_offline.htm* arquivo é detectado.</p><p>O valor padrão é 10.</p> |
| rapidFailsPerMinute | <p>Atributo inteiro opcional.</p><p>Especifica o número de vezes que o processo especificado na **processPath** pode falhar por minuto. Se esse limite for excedido, o módulo interromperá a inicialização do processo para o restante do minuto.</p><p>O valor padrão é 10.</p> |
| requestTimeout | <p>Atributo timespan opcional.</p><p>Especifica a duração para a qual o módulo do ASP.NET Core aguardará por uma resposta do processo que escuta em % ASPNETCORE_PORT %.</p><p>O valor padrão é "00: 02:00".</p> |
| stdoutLogEnabled | <p>Atributo booliano opcional.</p><p>Se true, **stdout** e **stderr** para o processo especificado na **processPath** será redirecionado para o arquivo especificado em **stdoutLogFile**.</p><p>O valor padrão é false.</p> |
| stdoutLogFile | <p>Atributo de cadeia de caracteres opcional.</p><p>Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado na **processPath** será registrado. Caminhos relativos são relativo à raiz do site. Qualquer caminho começando com '.' será relativo à raiz do site e todos os outros caminhos serão tratados como caminhos absolutos. As pastas fornecidas no caminho devem existir para que o módulo criar o arquivo de log. A ID de processo, o carimbo de hora (*yyyyMdhms*) e a extensão de arquivo (*. log*) com sublinhado delimitadores são adicionados para o último segmento do **stdoutLogFile** fornecido.</p><p>O valor padrão é `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | verdadeiro ou falso.</p><p>Se for true, o token será encaminhado para o processo filho escuta em % ASPNETCORE_PORT % como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação. É responsabilidade do processo para chamar CloseHandle neste token por solicitação.</p><p>O valor padrão é true.</p> |
| disableStartUpErrorPage | verdadeiro ou falso.</p><p>Se for true, o **502.5 - falha do processo** página será eliminada, e a página de código de 502 status configuradas no seu *Web. config* terá precedência.</p><p>O valor padrão é false.</p> |

### <a name="setting-environment-variables"></a>Definir variáveis de ambiente

O módulo de núcleo do ASP.NET permite que você especificar variáveis de ambiente para o processo especificado no `processPath` atributo especificando-os em uma ou mais `environmentVariable` elementos filho de um `environmentVariables` elemento de coleção no `aspNetCore` elemento. Variáveis de ambiente definidas nesta seção têm precedência sobre o sistema, variáveis de ambiente para o processo.

O exemplo a seguir define duas variáveis de ambiente. `ASPNETCORE_ENVIRONMENT`Configurar o ambiente do aplicativo para `Development`. Um desenvolvedor pode definir esse valor temporariamente *Web. config* arquivo para forçar o [página de exceção de desenvolvedor](xref:fundamentals/error-handling) carregar ao depurar uma exceção de aplicativo. `CONFIG_DIR`é um exemplo de uma variável de ambiente definidas pelo usuário, em que o desenvolvedor tenha gravado código que lê o valor na inicialização para formar um caminho para carregar o arquivo de configuração do aplicativo.

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

## <a name="appofflinehtm"></a>App_offline.htm

Se você colocar um arquivo com o nome *app_offline.htm* na raiz de um diretório de aplicativo web, o módulo do ASP.NET Core tentar desligar normalmente o aplicativo e parar o processamento de solicitações de entrada. Se o aplicativo ainda está em execução `shutdownTimeLimit` número de segundos, o módulo do ASP.NET Core finalizará o processo em execução.

Enquanto o *app_offline.htm* arquivo estiver presente, o módulo do ASP.NET Core responderá a solicitações enviando o conteúdo de *app_offline.htm* arquivo. Uma vez o *app_offline.htm* arquivo é removido, a próxima solicitação carrega o aplicativo, que responde às solicitações.

## <a name="start-up-error-page"></a>Página de erro de inicialização

Se o módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou o início do processo de back-end, mas não escutar na porta configurada, você verá uma página de código de status HTTP 502.5. Para omitir esta página e reverter para a página de código de status de IIS 502 padrão, use o `disableStartUpErrorPage` atributo. Para obter mais informações sobre como configurar mensagens de erro personalizadas, consulte [erros HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![Página de status 502](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Criação de log e de redirecionamento

O módulo do ASP.NET Core redireciona `stdout` e `stderr` logs no disco se você definir o `stdoutLogEnabled` e `stdoutLogFile` atributos do `aspNetCore` elemento. As pastas no `stdoutLogFile` caminho deve existir para que o módulo criar o arquivo de log. Uma extensão de arquivo e o carimbo de hora será adicionada automaticamente quando o arquivo de log é criado. Logs não são girados, a menos que ocorra a reciclagem de processo/reinício. É responsabilidade do hoster para limitar o espaço em disco que consomem os logs. Usando o `stdout` log só é recomendado para solução de problemas de inicialização do aplicativo e não gerais do aplicativo para fins de registro.

O nome do arquivo de log é composto, acrescentando o ID de processo (PID), timestamp (*yyyyMdhms*) e a extensão de arquivo (*. log*) para o último segmento do `stdoutLogFile` caminho (normalmente *stdout *) delimitados por sublinhados. Por exemplo se o `stdoutLogFile` caminho termina com *stdout*, um log para um aplicativo com uma identificação de 10652 criada em 10/8/2017 às 12:05:02 tem o nome de arquivo *stdout_10652_20178101252.log*.

Aqui está um exemplo `aspNetCore` elemento configura `stdout` log. O `stdoutLogFile` mostrado no exemplo de caminho é apropriado para o serviço de aplicativo do Azure. Um caminho local ou um caminho de compartilhamento de rede é aceitável para o registro local. Confirme se a identidade do usuário AppPool tem permissão para gravar o caminho fornecido.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Configuração do módulo principal do ASP.NET com um IIS compartilhada

O instalador do módulo de núcleo do ASP.NET é executado com os privilégios do **sistema** conta. Como a conta sistema local não ter modificar permissões para o caminho de compartilhamento que é usado pela configuração compartilhada de IIS, o instalador atingirá um erro de acesso negado ao tentar definir as configurações de módulo em * applicationHost. config* no compartilhamento.

A solução sem suporte é desabilitar a configuração compartilhada de IIS, execute o instalador, exportar atualizado *applicationHost. config* para o compartilhamento de arquivos e habilitar novamente a configuração compartilhada de IIS.

## <a name="module-schema-and-configuration-file-locations"></a>Locais de arquivo do módulo, o esquema e a configuração

### <a name="module"></a>Módulo

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**O IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

### <a name="schema"></a>Esquema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Configuração

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Você pode procurar *aspnetcore.dll* no *applicationHost. config* arquivo. Para o IIS Express, o *applicationHost. config* arquivo não existe por padrão. O arquivo é criado no *{raiz do aplicativo}\.vs\config* quando você iniciar qualquer projeto de aplicativo web na solução do Visual Studio.
