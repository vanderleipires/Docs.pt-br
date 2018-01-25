---
title: "Detectar alterações com tokens de alteração no núcleo do ASP.NET"
author: guardrex
description: "Saiba como usar tokens de alteração para controlar as alterações."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Detectar alterações com tokens de alteração no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

Um *alterar token* é um bloco de construção de finalidade geral de baixo nível usado para rastrear as alterações.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken interface

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga notificações que ocorreu uma alteração. `IChangeToken`reside no [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace. Para aplicativos que não usam o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, referência de [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pacote do NuGet no arquivo de projeto.

`IChangeToken`tem duas propriedades:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indique se o token de forma proativa gera retornos de chamada. Se `ActiveChangedCallbacks` é definido como `false`, um retorno de chamada nunca é chamado e o aplicativo deve sondar `HasChanged` para alterações. Também é possível que um token para nunca ser cancelada se não ocorrerem alterações ou a escuta de alteração subjacente é descartada ou desabilitada.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) obtém um valor que indica se uma alteração ocorreu.

A interface possui um método, [RegisterChangeCallback (ação&lt;objeto&gt;, objeto)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), que registra um retorno de chamada que é invocado quando o token foi alterado. `HasChanged`deve ser definido antes do retorno de chamada é invocado.

## <a name="changetoken-class"></a>Classe de token de alteração

`ChangeToken`uma classe estática é usada para propagar as notificações que ocorreu uma alteração. `ChangeToken`reside no [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace. Para aplicativos que não usam o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, referência de [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) pacote do NuGet no arquivo de projeto.

O `ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, ação)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) método registra um `Action` chamar sempre que o token é alterado:
* `Func<IChangeToken>`produz o token.
* `Action`é chamado quando o token é alterado.

`ChangeToken`tem um [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, ação&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) sobrecarga que utiliza um adicionais`TState`parâmetro que é passado para o consumidor token `Action`.

`OnChange`Retorna um [IDisposable](/dotnet/api/system.idisposable). Chamando [Dispose](/dotnet/api/system.idisposable.dispose) para o token de escuta para outras alterações e libera os recursos do token.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Exemplo de uso de tokens de alteração no núcleo do ASP.NET

Alterar tokens são usados nas áreas proeminentes de alterações em objetos de monitoramento principal do ASP.NET:

* Para monitorar as alterações nos arquivos, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)do [inspecionar](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) método cria um `IChangeToken` para os arquivos especificados ou pasta a ser examinada.
* `IChangeToken`tokens podem ser adicionados a entradas de cache para disparar remoções do cache de alterações.
* Para `TOptions` altera o padrão de [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementação de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) tem uma sobrecarga que aceita um ou mais [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)instâncias. Retorna cada instância de um `IChangeToken` para registrar um retorno de chamada de notificação de alteração para opções de controle de alterações.

## <a name="monitoring-for-configuration-changes"></a>Monitoramento de alterações de configuração

Por padrão, os modelos do ASP.NET Core usam [arquivos de configuração JSON](xref:fundamentals/configuration/index#json-configuration) (*appSettings. JSON*, *appsettings. Development.JSON*, e *appsettings. Production.JSON*) para carregar configurações do aplicativo.

Esses arquivos são configurados usando o [AddJsonFile (IConfigurationBuilder, String, Boolean, booleano)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) método de extensão no [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) que aceita um `reloadOnChange` parâmetro (ASP.NET Núcleo 1.1 e posterior). `reloadOnChange`Indica se a configuração deve ser recarregada nas alterações de arquivo. Consulte esta configuração de [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) método prático [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([fonte de referência](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Configuração baseada em arquivo é representada por [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource`usa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([fonte de referência](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) para monitorar os arquivos.

Por padrão, o `IFileMonitor` é fornecido por um [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([fonte de referência](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), que usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) para monitorar o arquivo de configuração alterações.

O aplicativo de exemplo demonstra duas implementações para monitorar alterações de configuração. Se o *appSettings. JSON* alterações de arquivo ou a versão do ambiente do arquivo é alterado, cada implementação executa código personalizado. O aplicativo de exemplo grava uma mensagem no console.

Um arquivo de configuração `FileSystemWatcher` pode disparar vários retornos de chamada de token para uma alteração de arquivo de configuração única. Implementação do exemplo protege contra esse problema verificando os hashes de arquivo nos arquivos de configuração. Verificação de hashes de arquivo garante que pelo menos um dos arquivos de configuração foi alterado antes de executar o código personalizado. O exemplo usa o hash de arquivo SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Uma nova tentativa é implementada com uma retirada exponencial. Tente novamente está presente porque o bloqueio de arquivos pode ocorrer que impede temporariamente um novo hash em um dos arquivos de computação.

### <a name="simple-startup-change-token"></a>Token de alteração de inicialização simples

Registrar um consumidor token `Action` retorno de chamada para notificações de alteração para o token de recarregamento de configuração (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`fornece o token. O retorno de chamada a `InvokeChanged` método:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

O `state` do retorno de chamada é usado para passar o `IHostingEnvironment`. Isso é útil para determinar o correto *appsettings* arquivo de configuração JSON para monitorar, *appsettings.&lt; Ambiente&gt;. JSON*. Hashes de arquivo são usados para evitar o `WriteConsole` instrução seja executado várias vezes devido às várias token retornos de chamada quando o arquivo de configuração mudou somente uma vez.

Esse sistema é executado, desde que o aplicativo está em execução e não pode ser desabilitado pelo usuário.

### <a name="monitoring-configuration-changes-as-a-service"></a>Monitoramento de alterações de configuração como um serviço

Implementa o exemplo:

* Monitoramento de token básicas de inicialização.
* Como um serviço de monitoramento.
* Um mecanismo para habilitar e desabilitar o monitoramento.

O exemplo estabelece um `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

O construtor da classe implementada, `ConfigurationMonitor`, registra um retorno de chamada para notificações de alteração:

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`fornece o token. `InvokeChanged`é o método de retorno de chamada. O `state` nesta instância é uma cadeia de caracteres que descreve o estado de monitoramento. Duas propriedades são usadas:

* `MonitoringEnabled`Indica se o retorno de chamada deve executar seu código personalizado.
* `CurrentState`Descreve o estado atual de monitoramento para uso na interface do usuário.

O `InvokeChanged` método é similar à abordagem anterior, exceto que ela:

* Não execute seu código, a menos que `MonitoringEnabled` é `true`.
* Define o `CurrentState` cadeia de caracteres de propriedade para uma mensagem descritiva que registra a hora em que o código foi executado.
* Notas atual `state` no seu `WriteConsole` saída.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Uma instância `ConfigurationMonitor` está registrado como um serviço no `ConfigureServices` de *Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

A página de índice oferece ao usuário controle sobre a configuração de monitoramento. A instância do `IConfigurationMonitor` é injetada o `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Um botão habilita e desabilita o monitoramento:

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Quando `OnPostStartMonitoring` é disparado, o monitoramento está habilitado e o estado atual está desmarcado. Quando `OnPostStopMonitoring` é disparado, o monitoramento está desabilitado e o estado é definido para refletir o monitoramento não está ocorrendo.

## <a name="monitoring-cached-file-changes"></a>Monitoramento de alterações de arquivo armazenado em cache

O conteúdo do arquivo pode ser armazenado em cache na memória usando [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Cache na memória é descrito no [na memória cache](xref:performance/caching/memory) tópico. Sem realizar etapas adicionais, como a implementação descrita abaixo, *obsoletos* dados (desatualizados) são retornados de um cache se os dados de origem são alterados.

Não levando em conta o status de um arquivo de origem em cache durante a renovação de um [a expiração deslizante](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) período resulta em cache dados obsoletos. O período de expiração deslizante renovada a cada solicitação de dados, mas nunca recarregar o arquivo no cache. Os recursos de aplicativos que usam o conteúdo do arquivo armazenado em cache estão sujeitos possivelmente receber conteúdo obsoleto.

Usando tokens de alteração em um cenário de cache de arquivo impede que o conteúdo do arquivo obsoletos no cache. O aplicativo de exemplo demonstra uma implementação do método.

O exemplo usa `GetFileContent` para:

* Retorna o conteúdo do arquivo.
* Implementa um algoritmo de repetição com retirada exponencial para cobrir casos em que um bloqueio de arquivo está impedindo temporariamente um arquivo que está sendo lido.

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Um `FileService` é criado para lidar com pesquisas de arquivo armazenado em cache. O `GetFileContent` chamada de método do serviço tenta obter o conteúdo do arquivo de cache na memória e retorná-la para o chamador (*Services/FileService.cs*).

Se o conteúdo armazenado em cache não for encontrado, usando a chave de cache, as seguintes ações são executadas:

1. O conteúdo do arquivo é obtido usando `GetFileContent`.
1. Um token de alteração é obtido do provedor de arquivo com [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Retorno de chamada do token é disparado quando o arquivo é modificado.
1. O conteúdo do arquivo é armazenado em cache com um [a expiração deslizante](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) período. O token de alteração é anexado ao [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) para remover a entrada do cache se o arquivo for alterada enquanto ele é armazenado em cache.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

O `FileService` está registrado no contêiner de serviço junto com o serviço de cache de memória (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

O modelo página carrega o conteúdo do arquivo usando o serviço (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Classe CompositeChangeToken

Para representar um ou mais `IChangeToken` instâncias em um único objeto, use o [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) classe ([fonte de referência](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged`nos relatórios de token compostos `true` se qualquer representados token `HasChanged` é `true`. `ActiveChangeCallbacks`nos relatórios de token compostos `true` se qualquer representados token `ActiveChangeCallbacks` é `true`. Se ocorrerem vários eventos de alteração simultâneas, o alteração composto de retorno de chamada é invocado exatamente uma vez.

## <a name="see-also"></a>Consulte também

* [Cache in-memory](xref:performance/caching/memory)
* [Trabalhando com um cache distribuído](xref:performance/caching/distributed)
* [Detectar alterações com tokens de alteração](xref:fundamentals/primitives/change-tokens)
* [Cache de resposta](xref:performance/caching/response)
* [Middleware de Cache de Resposta](xref:performance/caching/middleware)
* [Auxiliar de marca de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Auxiliar de marca de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
