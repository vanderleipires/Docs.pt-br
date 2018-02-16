---
title: "Adicionar recursos do aplicativo usando uma configuração específica de plataforma no núcleo do ASP.NET"
author: guardrex
description: "Saiba como adicionar recursos a um aplicativo ASP.NET Core de um assembly externo usando uma implementação de IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: 2663cd1e05be9e8695966df959082e6e574d0b4a
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/15/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a>Adicionar recursos do aplicativo usando uma configuração específica de plataforma no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

Um [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementação permite adicionar recursos a um aplicativo durante a inicialização de um assembly externo fora do aplicativo `Startup` classe. Por exemplo, uma biblioteca de ferramentas externas pode usar um `IHostingStartup` implementação para fornecer serviços a um aplicativo ou provedores de configuração adicionais. `IHostingStartup` *está disponível no ASP.NET Core 2.0 e posterior.*

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Descobrir assemblies carregados de hospedagem inicialização

Para descobrir a hospedagem assemblies de inicialização carregados pelo aplicativo ou bibliotecas, habilite o log e verifique os logs de aplicativo. Erros que ocorrem durante o carregamento de assemblies são registrados. Assemblies de inicialização de hospedagem carregados são registrados no nível de depuração e todos os erros são registrados.

As leituras de aplicativo de exemplo do [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) em um `string` de matriz e exibe o resultado na página de índice do aplicativo:

[!code-csharp[Main](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Desabilitar o carregamento automático de hospedagem assemblies de inicialização

Há duas maneiras para desabilitar o carregamento automático de hospedagem assemblies de inicialização:

* Definir o [impedir a inicialização de hospedagem](xref:fundamentals/hosting#prevent-hosting-startup) configuração de host.
* Definir o `ASPNETCORE_preventHostingStartup` variável de ambiente.

Quando a configuração do host ou a variável de ambiente é definida como `true` ou `1`, hospedagem assemblies de inicialização não são carregados automaticamente. Se ambas estiverem definidas, a configuração do host controla o comportamento.

Desabilitando hospedagem assemblies de inicialização usando a variável de ambiente ou configuração de host desabilita globalmente e poderá desabilitar os vários recursos de um aplicativo. Não é possível desabilitar seletivamente um assembly de inicialização de hospedagem adicionado por uma biblioteca, a menos que a biblioteca oferece sua própria opção de configuração no momento. Uma versão futura oferecem a capacidade de desabilitar seletivamente a hospedagem assemblies de inicialização (consulte [GitHub emitir aspnet/hospedagem #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>Implementar recursos de IHostingStartup

### <a name="create-the-assembly"></a>Criar o assembly

Um `IHostingStartup` recurso é implantado como um assembly com base em um aplicativo de console sem um ponto de entrada. As referências de assembly a [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pacote:

[!code-xml[Main](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

Um [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atributo identifica uma classe como uma implementação de `IHostingStartup` para carregamento e execução ao compilar o [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). O exemplo a seguir, o namespace é `StartupFeature`, e a classe é `StartupFeatureHostingStartup`:

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

Uma classe implementa `IHostingStartup`. A classe [configurar](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) método usa um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para adicionar recursos a um aplicativo:

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

Ao criar um `IHostingStartup` do projeto, o arquivo de dependências (*\*. deps.json*) define o `runtime` localização do assembly para o *bin* pasta:

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Apenas uma parte do arquivo é mostrada. É o nome do assembly no exemplo `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Atualizar o arquivo de dependências

O local de tempo de execução é especificado no  *\*. deps.json* arquivo. Ativa o recurso de `runtime` elemento deve especificar o local do assembly de tempo de execução do recurso. Prefixo de `runtime` local com `lib/netcoreapp2.0/`:

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

No aplicativo de exemplo, a modificação do  *\*. deps.json* arquivo é executado por um [PowerShell](/powershell/scripting/powershell-scripting) script. O script do PowerShell é acionado automaticamente por um destino de compilação no arquivo de projeto.

### <a name="feature-activation"></a>Ativação de recurso

**Coloque o arquivo de assembly**

O `IHostingStartup` arquivo de assembly de implementação deve ser *bin*-implantada no aplicativo ou colocadas no [repositório tempo de execução](/dotnet/core/deploying/runtime-store):

Para uso por usuário, coloque o assembly no repositório de tempo de execução do perfil do usuário em:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Para uso global, coloque o assembly no repositório de tempo de execução da instalação de núcleo do .NET:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Ao implantar o assembly no armazenamento de tempo de execução, o arquivo de símbolos pode ser implantado também, mas não é necessário para o recurso funcione.

**Coloque o arquivo de dependências**

A implementação  *\*. deps.json* arquivo deve estar em um local acessível.

Para uso por usuário, coloque o arquivo no `additonalDeps` pasta do perfil do usuário `.dotnet` configurações: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Para uso global, coloque o arquivo no `additonalDeps` pasta de instalação do núcleo do .NET:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Observe a versão `2.0.0`, reflete a versão de tempo de execução compartilhada que usa o aplicativo de destino. O tempo de execução compartilhado é mostrado no  *\*. runtimeconfig.json* arquivo. O aplicativo de exemplo, o tempo de execução compartilhado é especificado no *HostingStartupSample.runtimeconfig.json* arquivo.

**Conjunto de variáveis de ambiente**

Defina as seguintes variáveis de ambiente no contexto do aplicativo que usa o recurso.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Apenas assemblies de inicialização de hospedagem são verificados para o `HostingStartupAttribute`. O nome do assembly da implementação é fornecido nessa variável de ambiente. O aplicativo de exemplo define esse valor como `StartupDiagnostics`.

O valor também pode ser definido usando o [Assemblies de inicialização de hospedagem](xref:fundamentals/hosting#hosting-startup-assemblies) configuração de host.

DOTNET\_ADICIONAIS\_DEPS

O local da implementação do  *\*. deps.json* arquivo.

Se o arquivo será colocado no perfil do usuário *.dotnet* pasta para uso por usuário:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Se o arquivo é colocado na instalação do núcleo do .NET para uso global, forneça o caminho completo para o arquivo:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

O aplicativo de exemplo define esse valor como:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Para obter exemplos de como definir variáveis de ambiente para vários sistemas operacionais, consulte [trabalhando com vários ambientes](xref:fundamentals/environments).

## <a name="sample-app"></a>Aplicativo de exemplo

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para criar uma ferramenta de diagnóstico. A ferramenta adiciona dois middlewares para o aplicativo na inicialização que fornecem informações de diagnóstico:

* Serviços registrados
* Endereço: esquema, host, caminho de base, caminho, cadeia de caracteres de consulta
* Conexão: IP remoto, a porta remota, IP local, porta local, o certificado de cliente
* Cabeçalhos de solicitação
* Variáveis de ambiente

Para executar o exemplo:

1. O projeto de inicialização de diagnóstico usa [PowerShell](/powershell/scripting/powershell-scripting) para modificar seu *StartupDiagnostics.deps.json* arquivo. PowerShell é instalado por padrão em um sistema operacional do Windows a partir do Windows 7 SP1 e Windows Server 2008 R2 SP1. Para obter o PowerShell em outras plataformas, consulte [instalando o Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Compile o projeto de inicialização de diagnóstico. Um destino de compilação no arquivo de projeto:
   * Move o assembly e símbolos de arquivos para o repositório de tempo de execução do perfil do usuário.
   * Aciona o script do PowerShell para modificar o *StartupDiagnostics.deps.json* arquivo.
   * Move o *StartupDiagnostics.deps.json* arquivo para o perfil de usuário `additionalDeps` pasta.
3. Defina as variáveis de ambiente:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Execute o aplicativo de exemplo.
5. Solicitar o `/services` ponto de extremidade para ver o aplicativo registrado serviços. Solicitar o `/diag` ponto de extremidade para ver as informações de diagnóstico.
