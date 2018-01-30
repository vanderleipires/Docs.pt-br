---
title: "Referência de erros comuns para o serviço de aplicativo do Azure e o IIS com o ASP.NET Core"
author: guardrex
description: "Distingui erros comuns ao hospedar aplicativos ASP.NET Core no serviço de aplicativos do Azure e no IIS."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 214f8c616aa65077690757e7805983a77ec4249e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Referência de erros comuns para o serviço de aplicativo do Azure e o IIS com o ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

A lista a seguir não é uma lista completa de erros. Se você encontrar um erro não listado aqui, [abrir um novo problema](https://github.com/aspnet/Docs/issues/new) com instruções detalhadas para reproduzir o erro.

## <a name="installer-unable-to-obtain-vc-redistributable"></a>O instalador não pode obter os Pacotes Redistribuíveis do VC++

* **Exceção do Instalador:** 0x80072efd ou 0x80072f76 – erro não especificado

* **Exceção do Log do Instalador&#8224;:** erro 0x80072efd ou 0x80072f76: falha ao executar o pacote EXE

  &#8224;O log está localizado em C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Solução de problemas:

* Se o sistema não tiver acesso à Internet durante a instalação do servidor que hospeda o pacote, essa exceção ocorrerá quando o instalador for impedido de obter os *Pacotes redistribuíveis do Microsoft Visual C++ 2015*. Obter um instalador do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Se o instalador falhar, o servidor pode não receber o tempo de execução do .NET Core necessário para hospedar uma implantação de framework dependente (FDD). Se hospedando um FDD, confirme que o tempo de execução é instalado em programas &amp; recursos. Se necessário, obtenha um instalador de tempo de execução de [.NET Downloads](https://www.microsoft.com/net/download/core). Depois de instalar o tempo de execução, reinicie o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>O upgrade do sistema operacional removeu o Módulo do ASP.NET Core de 32 bits

* **Log do Aplicativo:** a DLL do Módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** falhou ao ser carregada. Os dados são o erro.

Solução de problemas:

* Arquivos que não são do sistema operacional no diretório **C:\Windows\SysWOW64\inetsrv** não são preservados durante um upgrade do sistema operacional. Se o módulo de núcleo do ASP.NET está instalado antes de uma atualização do sistema operacional e, em seguida, qualquer AppPool é executado no modo de 32 bits depois de uma atualização do sistema operacional, esse problema é encontrado. Após um upgrade do sistema operacional, repare o Módulo do ASP.NET Core. Consulte [Instalar o pacote de hospedagem do Windows Server do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Selecione **reparo** quando o instalador é executado.

## <a name="platform-conflicts-with-rid"></a>Conflitos de plataforma com o RID

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\{caminho}\' Falha ao iniciar o processo com linha de comando ' "c:\\{caminho} {assembly}. { exe | dll} "', código de erro = ' 0x80004005: ff.

* **Log de módulo do ASP.NET Core:** exceção sem tratamento: System. BadImageFormatException: não foi possível carregar arquivo ou assembly '{assembly}. dll'. Foi feita uma tentativa de carregar um programa com um formato incorreto.

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme se o `<PlatformTarget>` no *. csproj* não entra em conflito com o RID. Por exemplo, não especifique um `<PlatformTarget>` de `x86` e publicar com uma RID da `win10-x64`, usando *dotnet publicar - c - r de versão para win10-x64* ou definindo o `<RuntimeIdentifiers>` no *. csproj*  para `win10-x64`. O projeto é publicado sem avisos nem erros, mas falha com as exceções registradas acima no sistema.

* Se essa exceção ocorre para uma implantação de aplicativos do Azure ao atualizar um aplicativo e implantando assemblies mais recentes, exclua manualmente todos os arquivos de implantação anterior. Assemblies incompatíveis remanescentes podem resultar em uma exceção `System.BadImageFormatException` durante a implantação de um aplicativo atualizado.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Ponto de extremidade de URI incorreto ou site interrompido

* **Navegador:** ERR_CONNECTION_REFUSED

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confirme se que o ponto de extremidade URI correto para o aplicativo está sendo usado. Verifique as associações.

* Confirme que o site do IIS não está no *parado* estado.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Recursos do servidor CoreWebEngine ou W3SVC desabilitados

* **Exceção do Sistema Operacional:** os recursos CoreWebEngine e W3SVC do IIS 7.0 devem ser instalados para usar o Módulo do ASP.NET Core.

Solução de problemas:

* Confirme se a função adequada e os recursos estão habilitados. Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Caminho físico do site incorreto ou ausente do aplicativo

* **Navegador:** 403 Proibido – acesso negado **OU** 403.14 Proibido – o servidor Web está configurado para não listar o conteúdo deste diretório.

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Verifique o site do IIS **configurações básicas** e a pasta física do aplicativo. Confirme se o aplicativo está na pasta em que o site do IIS **caminho físico**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Função incorreta, módulo não instalado ou permissões incorretas

* **Navegador:** 500.19 Erro interno do servidor – a página solicitada não pode ser acessada porque os dados de configuração relacionados da página são inválidos.

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confirme que a função adequada está habilitada. Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Verifique **Programas &amp; Recursos** e confirme se o **Módulo do Microsoft ASP.NET Core** foi instalado. Se o **Módulo do Microsoft ASP.NET Core** não estiver presente na lista de programas instalados, instale o módulo. Consulte [Instalar o pacote de hospedagem do Windows Server do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).

* Verifique se o **Pool de aplicativos** > **modelo de processo** > **identidade** é definido como **ApplicationPoolIdentity** ou a identidade personalizada tem as permissões corretas para acessar a pasta de implantação do aplicativo.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath incorreto, variável de PATH ausente, pacote de hospedagem não instalado, sistema/IIS não reiniciado, Pacotes Redistribuíveis do VC++ não instalados ou violação de acesso de dotnet.exe

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' Falha ao iniciar o processo com linha de comando ' ".\{ assembly} .exe"', código de erro = ' 0x80070002: 0.

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Verifique o *processPath* atributo no `<aspNetCore>` elemento *Web. config* para confirmar se ele está *dotnet* para uma implantação do framework dependente (FDD) ou *. \{assembly} .exe* para uma implantação autossuficiente (SCD).

* Para uma FDD, o *dotnet.exe* pode não estar acessível por meio das configurações de PATH. Confirme se *C:\Program Files\dotnet\* existe nas configurações de PATH do Sistema.

* Para uma FDD, o *dotnet.exe* pode não estar acessível para a identidade do usuário do Pool de aplicativos. Confirme se a identidade do usuário do AppPool tem acesso ao diretório *C:\Program Files\dotnet*. Confirme que não há nenhuma regra de negação configurada para a identidade do usuário AppPool no *Files\dotnet C:\Program* e diretórios do aplicativo.

* Um FDD pode ter sido implantado e .NET Core instalado sem a reinicialização do IIS. Reinicie o servidor ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Um FDD pode ter sido implantado sem instalar o tempo de execução do .NET Core no sistema de hospedagem. Se o tempo de execução do .NET Core ainda não foi instalado, execute o **instalador do pacote de hospedagem do .NET Core Windows Server** no sistema. Consulte [Instalar o pacote de hospedagem do Windows Server do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Se você tentar instalar o runtime do .NET Core em um sistema sem uma conexão de Internet, obter o tempo de execução de [.NET Downloads](https://www.microsoft.com/net/download/core) e execute o instalador de pacote de hospedagem para instalar o módulo do ASP.NET Core. Conclua a instalação reiniciando o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Um FDD pode ter sido implantado e o *Microsoft Visual C++ 2015 redistribuível (x64)* não está instalado no sistema. Obter um instalador do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorretos do elemento \<aspNetCore\>

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' Falha ao iniciar o processo com linha de comando ' "dotnet".\{ . dll de assembly}', código de erro = ' 0x80004005: 80008081.

* **Log de módulo do ASP.NET Core:** a execução do aplicativo não existe: ' caminho\{assembly}. dll '

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Examine o *argumentos* atributo no `<aspNetCore>` elemento *Web. config* para confirmar se ele é (a) *.\{ . dll de assembly}* para uma implantação de framework dependente (FDD); ou (b) não está presente, uma cadeia de caracteres vazia (*argumentos = ""*), ou uma lista de argumentos do aplicativo (*argumentos = "arg1, arg2,..."*) para uma implantação autossuficiente (SCD).

## <a name="missing-net-framework-version"></a>Versão do .NET Framework ausente

* **Navegador:** 502.3 Gateway incorreto – Erro de conexão ao tentar encaminhar a solicitação.

* **Log de aplicativo:** ErrorCode = aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' Falha ao iniciar o processo com linha de comando ' "dotnet".\{ . dll de assembly}', código de erro = ' 0x80004005: 80008081.

* **Log do Módulo do ASP.NET Core:** exceção do método, arquivo ou assembly ausente. O método, o arquivo ou o assembly especificado na exceção é um método, arquivo ou assembly do .NET Framework.

Solução de problemas:

* Instale a versão do .NET Framework ausente no sistema.

* Para uma implantação do framework dependente (FDD), confirme que o tempo de execução correto instalada no sistema. Se o projeto é atualizado de 1.1 para 2.0, o sistema host, implantados e os resultados dessa exceção, verifique se o framework 2.0 está no sistema de hospedagem.

## <a name="stopped-application-pool"></a>Pool de aplicativos interrompido

* **Navegador:** 503 Serviço não disponível

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas

* Confirme que o Pool de aplicativos não está no *parado* estado.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware de Integração do IIS não implementado

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' criou um processo com linha de comando ' "c:\\{caminho}\{assembly}. { exe | dll} "' mas o falhou ou foi resposta não ou não escutar na porta determinada '{PORT}', código de erro = '0x800705b4'

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal.

Solução de problemas

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme se ou:
  * O middleware de integração de IIS for referencedby chamando o `UseIISIntegration` método do aplicativo `WebHostBuilder` (ASP.NET Core 1. x)
  * As aplicativos usa o `CreateDefaultBuilder` método (ASP.NET Core 2. x).
  
  Consulte [Hospedando no ASP.NET Core](xref:fundamentals/hosting) para obter mais detalhes.

## <a name="sub-application-includes-a-handlers-section"></a>O subaplicativo inclui uma seção \<manipuladores\>

* **Navegador:** 500.19 Erro HTTP – erro interno do servidor

* **Log do Aplicativo:** nenhuma entrada

* **Log de módulo do ASP.NET Core:** arquivo de Log criado e mostra uma operação normal para o aplicativo raiz. Arquivo de log não foi criado para o aplicativo sub.

Solução de problemas

* Confirme se o arquivo *web.config* do subaplicativo não inclui uma seção `<handlers>`.

## <a name="application-configuration-general-issue"></a>Problema geral de configuração do aplicativo

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' criou um processo com linha de comando ' "c:\\{caminho}\{assembly}. { exe | dll} "' mas o falhou ou foi resposta não ou não escutar na porta determinada '{PORT}', código de erro = '0x800705b4'

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio

Solução de problemas

* Essa exceção geral indica que o processo não pôde iniciar, provavelmente devido a um problema de configuração do aplicativo. Referindo-se a [estrutura de diretórios](xref:host-and-deploy/directory-structure), confirme que o aplicativo implantado arquivos e pastas são apropriadas e que os arquivos de configuração do aplicativo estão presentes e conter as configurações corretas para o aplicativo e o ambiente. Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).
