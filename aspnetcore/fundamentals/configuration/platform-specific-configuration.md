---
title: Aprimorar um aplicativo por meio de um assembly externo no ASP.NET Core com IHostingStartup
author: guardrex
description: Descubra como aprimorar um aplicativo ASP.NET Core por meio de um assembly externo usando uma implementação de IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Aprimorar um aplicativo por meio de um assembly externo no ASP.NET Core com IHostingStartup

Por [Luke Latham](https://github.com/guardrex)

Uma implementação [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite adicionar melhorias a um aplicativo durante a inicialização de um assembly externo fora da classe `Startup` do aplicativo. Por exemplo, uma biblioteca de ferramentas externas pode usar uma implementação de `IHostingStartup` para fornecer serviços ou provedores de configuração adicionais a um aplicativo. `IHostingStartup` *está disponível no ASP.NET Core 2.0 e posterior.*

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Descobrir assemblies de inicialização de hospedagem carregados

Para descobrir os assemblies de inicialização de hospedagem carregados pelo aplicativo ou por bibliotecas, habilite o log e verifique os logs do aplicativo. Erros que ocorrem quando os assemblies carregados são registrados em log. Os assemblies de inicialização de hospedagem carregados são registrados em log no nível Depuração e todos os erros são registrados.

O aplicativo de exemplo lê a [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) em uma matriz `string` e exibe o resultado na página Índice do aplicativo:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Desabilitar o carregamento automático de assemblies de inicialização de hospedagem

Há duas maneiras para desabilitar o carregamento automático de assemblies de inicialização de hospedagem:

* Definir a configuração do host [Impedir Inicialização de Hospedagem](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Definir a variável de ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

Quando a configuração do host ou a variável de ambiente é definida como `true` ou `1`, os assemblies de inicialização de hospedagem não são carregados automaticamente. Se ambas estiverem definidas, a configuração do host controlará o comportamento.

A desabilitação de assemblies de inicialização de hospedagem usando a configuração do host ou a variável de ambiente desabilita-os globalmente e poderá desabilitar várias características de um aplicativo. No momento, não é possível desabilitar seletivamente um assembly de inicialização de hospedagem adicionado por uma biblioteca, a menos que a biblioteca ofereça sua própria opção de configuração. Uma versão futura oferecerá a capacidade de desabilitar seletivamente os assemblies de inicialização de hospedagem (confira [Problema do GitHub aspnet/Hospedagem nº 1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implementar IHostingStartup

### <a name="create-the-assembly"></a>Criar o assembly

Uma melhoria de `IHostingStartup` é implantada como um assembly com base em um aplicativo de console sem um ponto de entrada. O assembly referencia o pacote [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

Um atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica uma classe como uma implementação de `IHostingStartup` para o carregamento e a execução durante a criação do [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). No seguinte exemplo, o namespace é `StartupEnhancement` e a classe é `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Uma classe implementa `IHostingStartup`. O método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) da classe usa um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para adicionar melhorias a um aplicativo:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Ao criar um projeto `IHostingStartup`, o arquivo de dependências (*\*.deps.json*) define o local `runtime` do assembly como a pasta *bin*:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Apenas uma parte do arquivo é mostrada. O nome do assembly no exemplo é `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Atualizar o arquivo de dependências

O local do tempo de execução é especificado no arquivo *\*.deps.json*. Para ativar a melhoria, o elemento `runtime` deve especificar o local do assembly de tempo de execução da melhoria. Preceda o local `runtime` com `lib/netcoreapp2.0/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

No aplicativo de exemplo, a modificação do arquivo *\*.deps.json* é executada por um script do [PowerShell](/powershell/scripting/powershell-scripting). O script do PowerShell é disparado automaticamente por um destino de build no arquivo de projeto.

### <a name="enhancement-activation"></a>Ativação da melhoria

**Colocar o arquivo do assembly**

O arquivo do assembly da implementação de `IHostingStartup` deve estar implantado em *bin* no aplicativo ou colocado no [repositório de tempo de execução](/dotnet/core/deploying/runtime-store):

Para uso por usuário, coloque o assembly no repositório de tempo de execução do perfil do usuário em:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

Para uso global, coloque o assembly no repositório de tempo de execução da instalação do .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

Ao implantar o assembly no repositório de tempo de execução, o arquivo de símbolos pode ser implantado também, mas ele não é necessário para que a extensão funcione.

**Colocar o arquivo de dependências**

O arquivo *\*.deps.json* da implementação deve estar em um local acessível.

Para uso por usuário, coloque o arquivo na pasta `additonalDeps` das configurações `.dotnet` do perfil do usuário: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Para uso global, coloque o arquivo na pasta `additonalDeps` da instalação do .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Observe que a versão, `2.0.0`, reflete a versão do tempo de execução compartilhado usada pelo aplicativo de destino. O tempo de execução compartilhado é mostrado no arquivo *\*.runtimeconfig.json*. No aplicativo de exemplo, o tempo de execução compartilhado é especificado no arquivo *HostingStartupSample.runtimeconfig.json*.

**Definir variáveis de ambiente**

Defina as variáveis de ambiente a seguir no contexto do aplicativo que usa a melhoria.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Apenas assemblies de inicialização de hospedagem são verificados quanto ao `HostingStartupAttribute`. O nome do assembly da implementação é fornecido nessa variável de ambiente. O aplicativo de exemplo define esse valor como `StartupDiagnostics`.

O valor também pode ser definido usando a configuração do host [Assemblies de Inicialização de Hospedagem](xref:fundamentals/host/web-host#hosting-startup-assemblies).

DOTNET\_ADDITIONAL\_DEPS

O local do arquivo *\*.deps.json* da implementação.

Se o arquivo for colocado na pasta *.dotnet* do perfil do usuário para uso por usuário:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Se o arquivo for colocado na instalação do .NET Core para uso global, forneça o caminho completo para o arquivo:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

O aplicativo de exemplo define esse valor como:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Para obter exemplos de como definir variáveis de ambiente para vários sistemas operacionais, confira [Usar vários ambientes](xref:fundamentals/environments).

## <a name="sample-app"></a>Aplicativo de exemplo

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para criar uma ferramenta de diagnóstico. A ferramenta adiciona dois middlewares ao aplicativo na inicialização que fornecem informações de diagnóstico:

* Serviços registrados
* Endereço: esquema, host, base do caminho, caminho, cadeia de caracteres de consulta
* Conexão: IP remoto, porta remota, IP local, porta local, certificado do cliente
* Cabeçalhos de solicitação
* Variáveis de ambiente

Para executar a amostra:

1. O projeto de Diagnóstico de Inicialização usa o [PowerShell](/powershell/scripting/powershell-scripting) para modificar seu arquivo *StartupDiagnostics.deps.json*. O PowerShell é instalado por padrão em um sistema operacional Windows começando no Windows 7 SP1 e no Windows Server 2008 R2 SP1. Para obter o PowerShell em outras plataformas, confira [Instalando o Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Compile o projeto de Diagnóstico de Inicialização. Um destino de build no arquivo de projeto:
   * Move o assembly e os arquivos de símbolos para o repositório de tempo de execução do perfil do usuário.
   * Dispara o script do PowerShell para modificar o arquivo *StartupDiagnostics.deps.json*.
   * Move o arquivo *StartupDiagnostics.deps.json* para a pasta `additionalDeps` do perfil do usuário.
3. Defina as variáveis de ambiente:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Execute o aplicativo de exemplo.
5. Solicite o ponto de extremidade `/services` para ver os serviços registrados do aplicativo. Solicite o ponto de extremidade `/diag` para ver as informações de diagnóstico.
