---
title: Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core
author: guardrex
description: Distinga erros comuns ao hospedar aplicativos ASP.NET Core no Serviço de Aplicativos do Azure e no IIS.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277313"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

A lista a seguir não é uma lista completa de erros. Se você encontrar um erro não listado aqui, [abra um novo problema](https://github.com/aspnet/Docs/issues/new) com instruções detalhadas para reproduzir o erro.

Colete as seguintes informações:

* Comportamento do navegador
* Entradas do Log de Eventos do Aplicativo
* Entradas do log de stdout do Módulo do ASP.NET Core

Compare as informações para os erros comuns a seguir. Se uma correspondência for encontrada, siga o aviso de solução de problemas.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>O instalador não pode obter os Pacotes Redistribuíveis do VC++

* **Exceção do Instalador:** 0x80072efd ou 0x80072f76 – erro não especificado

* **Exceção do Log do Instalador&#8224;:** erro 0x80072efd ou 0x80072f76: falha ao executar o pacote EXE

  &#8224;O log está localizado em C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Solução de problemas:

* Se o sistema não tiver acesso à Internet durante a instalação do pacote de hospedagem, essa exceção ocorrerá quando o instalador for impedido de obter os *Pacotes Redistribuíveis do Microsoft Visual C++ 2015*. Obtenha um instalador do [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Se o instalador falhar, o servidor poderá não receber o tempo de execução do .NET Core necessário para hospedar uma FDD (implantação dependente de estrutura). Ao hospedar uma FDD, confirme se o tempo de execução está instalado em Programas &amp; Recursos. Se necessário, obtenha um instalador de tempo de execução de [Todos os Downloads do .NET](https://www.microsoft.com/net/download/all). Depois de instalar o tempo de execução, reinicie o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>O upgrade do sistema operacional removeu o Módulo do ASP.NET Core de 32 bits

* **Log do Aplicativo:** a DLL do Módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** falhou ao ser carregada. Os dados são o erro.

Solução de problemas:

* Arquivos que não são do sistema operacional no diretório **C:\Windows\SysWOW64\inetsrv** não são preservados durante um upgrade do sistema operacional. Se o Módulo do ASP.NET Core estiver instalado antes de uma atualização do sistema operacional e, em seguida, qualquer AppPool for executado no modo de 32 bits após uma atualização do sistema operacional, esse problema será encontrado. Após um upgrade do sistema operacional, repare o Módulo do ASP.NET Core. Veja [Instalar o pacote de Hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Selecione **Reparar** ao executar o instalador.

## <a name="platform-conflicts-with-rid"></a>Conflitos de plataforma com o RID

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\{PATH}\' falhou ao iniciar o processo com a linha de comando '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.

* **Log do Módulo do ASP.NET Core:** exceção sem tratamento: System.BadImageFormatException: não foi possível carregar o arquivo ou o assembly '{assembly}.dll'. Foi feita uma tentativa de carregar um programa com um formato incorreto.

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme que o `<PlatformTarget>` no *.csproj* não entra em conflito com o RID. Por exemplo, não especifique um `<PlatformTarget>` igual a `x86` e publique com um RID igual a `win10-x64`, usando *dotnet publish -c Release -r win10-x64* ou definindo o `<RuntimeIdentifiers>` no *.csproj* como `win10-x64`. O projeto é publicado sem avisos nem erros, mas falha com as exceções registradas acima no sistema.

* Se essa exceção ocorrer para uma implantação dos Aplicativos do Azure ao fazer upgrade de um aplicativo e implantar assemblies mais recentes, exclua manualmente todos os arquivos da implantação anterior. Assemblies incompatíveis remanescentes podem resultar em uma exceção `System.BadImageFormatException` durante a implantação de um aplicativo atualizado.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Ponto de extremidade de URI incorreto ou site interrompido

* **Navegador:** ERR_CONNECTION_REFUSED

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confirme se que o ponto de extremidade do URI correto para o aplicativo está sendo usado. Verifique as associações.

* Confirme que o site do IIS não está no estado *Parado*.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Recursos do servidor CoreWebEngine ou W3SVC desabilitados

* **Exceção do Sistema Operacional:** os recursos CoreWebEngine e W3SVC do IIS 7.0 devem ser instalados para usar o Módulo do ASP.NET Core.

Solução de problemas:

* Confirme que a função e os recursos apropriados estão habilitados. Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Caminho físico do site incorreto ou aplicativo ausente

* **Navegador:** 403 Proibido – acesso negado **OU** 403.14 Proibido – o servidor Web está configurado para não listar o conteúdo deste diretório.

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confira as **Configurações Básicas** no site do IIS e a pasta do aplicativo físico. Confirme que o aplicativo está na pasta no **Caminho físico** do site do IIS.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Função incorreta, módulo não instalado ou permissões incorretas

* **Navegador:** 500.19 Erro interno do servidor – a página solicitada não pode ser acessada porque os dados de configuração relacionados da página são inválidos.

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confirme que você habilitou a função apropriada. Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Verifique **Programas &amp; Recursos** e confirme se o **Módulo do Microsoft ASP.NET Core** foi instalado. Se o **Módulo do Microsoft ASP.NET Core** não estiver presente na lista de programas instalados, instale o módulo. Veja [Instalar o pacote de Hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Verifique se o **Pool de aplicativos** > **Modelo de processo** > **Identidade** está definido como **ApplicationPoolIdentity** ou se a identidade personalizada tem as permissões corretas para acessar a pasta de implantação do aplicativo.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath incorreto, variável de PATH ausente, pacote de hospedagem não instalado, sistema/IIS não reiniciado, Pacotes Redistribuíveis do VC++ não instalados ou violação de acesso de dotnet.exe

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' falhou ao iniciar o processo com a linha de comando '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Verifique o atributo *processPath* no elemento `<aspNetCore>` em *web.config* para confirmar se ele é *dotnet* para uma FDD (implantação dependente de estrutura) ou *.\{assembly}.exe* para uma SCD (implantação autossuficiente).

* Para uma FDD, o *dotnet.exe* pode não estar acessível por meio das configurações de PATH. Confirme se *C:\Program Files\dotnet\* existe nas configurações de PATH do Sistema.

* Para uma FDD, o *dotnet.exe* pode não estar acessível para a identidade do usuário do Pool de aplicativos. Confirme se a identidade do usuário do AppPool tem acesso ao diretório *C:\Program Files\dotnet*. Confirme se não há nenhuma regra de negação configurada para a identidade do usuário do AppPool no *C:\Arquivos de Programas\dotnet* e nos diretórios do aplicativo.

* Talvez você tenha implantado uma FDD e instalado o .NET Core sem reiniciar o IIS. Reinicie o servidor ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Você pode ter implantado uma FDD sem instalar o tempo de execução do .NET Core no sistema de hospedagem. Se o tempo de execução do .NET Core ainda não foi instalado, execute o **Instalador do Pacote de Hospedagem do .NET Core** no sistema. Veja [Instalar o pacote de Hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Se estiver tentando instalar o tempo de execução do .NET Core em um sistema sem uma conexão com a Internet, obtenha o tempo de execução em [Todos os Downloads do .NET](https://www.microsoft.com/net/download/all) e execute o instalador do pacote de hospedagem para instalar o Módulo do ASP.NET Core. Conclua a instalação reiniciando o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Talvez você tenha implantado uma FDD e os *Pacotes redistribuíveis do Microsoft Visual C++ 2015 (x64)* não estejam instalados no sistema. Obtenha um instalador do [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorretos do elemento \<aspNetCore\>

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' falhou ao iniciar o processo com a linha de comando '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.

* **Log do Módulo do ASP.NET Core:** o aplicativo a ser executado não existe: 'PATH\{assembly}.dll'

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Examine o atributo *arguments* no elemento `<aspNetCore>` no *web.config* para confirmar se ele: (a) é *.\{assembly}.dll* de uma FDD (implantação dependente de estrutura); ou (b) não está presente, é uma cadeia de caracteres vazia (*arguments=""*) ou uma lista de argumentos do aplicativo (*arguments="arg1, arg2, ..."*) para uma SCD (implantação autossuficiente).

## <a name="missing-net-framework-version"></a>Versão do .NET Framework ausente

* **Navegador:** 502.3 Gateway incorreto – Erro de conexão ao tentar encaminhar a solicitação.

* **Log do Aplicativo:** ErrorCode = O aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' falhou ao iniciar o processo com a linha de comando '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.

* **Log do Módulo do ASP.NET Core:** exceção do método, arquivo ou assembly ausente. O método, o arquivo ou o assembly especificado na exceção é um método, arquivo ou assembly do .NET Framework.

Solução de problemas:

* Instale a versão do .NET Framework ausente no sistema.

* Para uma FDD (implantação dependente de estrutura), confirme se você tem o tempo de execução correto instalado no sistema. Se o projeto foi atualizado de 1.1 para 2.0 e implantado no sistema de hospedagem e resultou nessa exceção, verifique se a estrutura 2.0 está no sistema de hospedagem.

## <a name="stopped-application-pool"></a>Pool de aplicativos interrompido

* **Navegador:** 503 Serviço não disponível

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas

* Confirme que o Pool de Aplicativos não está no estado *Parado*.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware de Integração do IIS não implementado

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' criou um processo com a linha de comando '"C:\\{PATH}\{assembly}.{exe|dll}"', mas falhou, não respondeu ou não escutou na porta '{PORT}' fornecida, ErrorCode = '0x800705b4'

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal.

Solução de problemas

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).

* Confirme que uma das seguintes condições é verdadeira:
  * O middleware de integração de IIS é referenciado chamando-se o método `UseIISIntegration` no `WebHostBuilder` do aplicativo (ASP.NET Core 1.x)
  * Os aplicativos usam o método `CreateDefaultBuilder` (ASP.NET Core 2.x).
  
  Veja [Hospedar no ASP.NET Core](xref:fundamentals/host/index) para obter mais detalhes.

## <a name="sub-application-includes-a-handlers-section"></a>O subaplicativo inclui uma seção \<manipuladores\>

* **Navegador:** 500.19 Erro HTTP – erro interno do servidor

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal do aplicativo raiz. O arquivo de log não foi criado para o subaplicativo.

Solução de problemas

* Confirme se o arquivo *web.config* do subaplicativo não inclui uma seção `<handlers>`.

## <a name="stdout-log-path-incorrect"></a>caminho do log de stdout incorreto

* **Navegador:** o aplicativo responde normalmente.

* **Log do Aplicativo:** Aviso: não foi possível criar o stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas

* O caminho `stdoutLogFile` especificado no elemento `<aspNetCore>` de *web.config* não existe. Para obter mais informações, veja a seção [Criação e redirecionamento de log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) do tópico de referência de configuração do Módulo do ASP.NET Core.

## <a name="application-configuration-general-issue"></a>Problema geral de configuração do aplicativo

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' criou um processo com a linha de comando '"C:\\{PATH}\{assembly}.{exe|dll}"', mas falhou, não respondeu ou não escutou na porta '{PORT}' fornecida, ErrorCode = '0x800705b4'

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio

Solução de problemas

* Essa exceção geral indica que o processo não pôde ser iniciado, provavelmente, devido a um problema de configuração do aplicativo. Consultando [Estrutura de diretório](xref:host-and-deploy/directory-structure), confirme se as pastas e os arquivos implantados do aplicativo são apropriados e se os arquivos de configuração do aplicativo estão presentes e contêm as configurações corretas para o aplicativo e o ambiente. Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).
