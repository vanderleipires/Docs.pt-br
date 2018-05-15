---
title: Detectar alterações com tokens de alteração no ASP.NET Core
author: guardrex
description: Saiba como usar tokens de alteração para controlar alterações.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 3055eec91adc412b596d4cc73e8523e18ff63331
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/19/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="b198c-103">Detectar alterações com tokens de alteração no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b198c-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="b198c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b198c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b198c-105">Um *token de alteração* é um bloco de construção de uso geral e de baixo nível, usado para controlar as alterações.</span><span class="sxs-lookup"><span data-stu-id="b198c-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="b198c-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b198c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="b198c-107">Interface IChangeToken</span><span class="sxs-lookup"><span data-stu-id="b198c-107">IChangeToken interface</span></span>

<span data-ttu-id="b198c-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga notificações de que ocorreu uma alteração.</span><span class="sxs-lookup"><span data-stu-id="b198c-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="b198c-109">`IChangeToken` reside no namespace [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="b198c-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="b198c-110">Para aplicativos que não usam o metapacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), referencie o pacote NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="b198c-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="b198c-111">`IChangeToken` tem duas propriedades:</span><span class="sxs-lookup"><span data-stu-id="b198c-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="b198c-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indique se o token gera retornos de chamada de forma proativa.</span><span class="sxs-lookup"><span data-stu-id="b198c-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="b198c-113">Se `ActiveChangedCallbacks` é definido como `false`, um retorno de chamada nunca é chamado e o aplicativo precisa sondar `HasChanged` em busca de alterações.</span><span class="sxs-lookup"><span data-stu-id="b198c-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="b198c-114">Também é possível que um token nunca seja cancelado se não ocorrerem alterações ou o ouvinte de alteração subjacente for removido ou desabilitado.</span><span class="sxs-lookup"><span data-stu-id="b198c-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="b198c-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) obtém um valor que indica se uma alteração ocorreu.</span><span class="sxs-lookup"><span data-stu-id="b198c-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="b198c-116">A interface tem um método, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), que registra um retorno de chamada que é invocado quando o token é alterado.</span><span class="sxs-lookup"><span data-stu-id="b198c-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="b198c-117">`HasChanged` precisa ser definido antes de o retorno de chamada ser invocado.</span><span class="sxs-lookup"><span data-stu-id="b198c-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="b198c-118">Classe ChangeToken</span><span class="sxs-lookup"><span data-stu-id="b198c-118">ChangeToken class</span></span>

<span data-ttu-id="b198c-119">`ChangeToken` é uma classe estática usada para propagar notificações de que ocorreu uma alteração.</span><span class="sxs-lookup"><span data-stu-id="b198c-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="b198c-120">`ChangeToken` reside no namespace [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="b198c-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="b198c-121">Para aplicativos que não usam o metapacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), referencie o pacote NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="b198c-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="b198c-122">O método `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) registra um `Action` para chamar sempre que o token é alterado:</span><span class="sxs-lookup"><span data-stu-id="b198c-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="b198c-123">`Func<IChangeToken>` produz o token.</span><span class="sxs-lookup"><span data-stu-id="b198c-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="b198c-124">`Action` é chamado quando o token é alterado.</span><span class="sxs-lookup"><span data-stu-id="b198c-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="b198c-125">`ChangeToken` tem uma sobrecarga [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) que usa um parâmetro `TState` adicional que é passado para o consumidor de token `Action`.</span><span class="sxs-lookup"><span data-stu-id="b198c-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="b198c-126">`OnChange` retorna um [IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="b198c-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="b198c-127">A chamada a [Dispose](/dotnet/api/system.idisposable.dispose) interrompe a escuta do token de outras alterações e libera os recursos do token.</span><span class="sxs-lookup"><span data-stu-id="b198c-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="b198c-128">Usos de exemplo de tokens de alteração no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b198c-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="b198c-129">Tokens de alteração são usados nas áreas proeminentes do monitoramento do ASP.NET Core de alterações em objetos:</span><span class="sxs-lookup"><span data-stu-id="b198c-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="b198c-130">Para monitorar as alterações em arquivos, o método [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) de [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) cria um `IChangeToken` para os arquivos especificados ou para pasta a ser inspecionada.</span><span class="sxs-lookup"><span data-stu-id="b198c-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="b198c-131">Tokens `IChangeToken` podem ser adicionados a entradas de cache para disparar remoções do cache após as alterações.</span><span class="sxs-lookup"><span data-stu-id="b198c-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="b198c-132">Para alterações `TOptions`, a implementação padrão de [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) tem uma sobrecarga que aceita uma ou mais instâncias [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1).</span><span class="sxs-lookup"><span data-stu-id="b198c-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="b198c-133">Cada instância retorna um `IChangeToken` para registrar um retorno de chamada de notificação de alteração para o controle de alterações de opções.</span><span class="sxs-lookup"><span data-stu-id="b198c-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="b198c-134">Monitorando alterações de configuração</span><span class="sxs-lookup"><span data-stu-id="b198c-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="b198c-135">Por padrão, os modelos do ASP.NET Core usam [arquivos de configuração JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* e *appsettings.Production.json*) para carregar as definições de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b198c-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="b198c-136">Esses arquivos são configurados com o método de extensão [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) no [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) que aceita um parâmetro `reloadOnChange` (ASP.NET Core 1.1 e posterior).</span><span class="sxs-lookup"><span data-stu-id="b198c-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="b198c-137">`reloadOnChange` indica se a configuração deve ser recarregada após alterações de arquivo.</span><span class="sxs-lookup"><span data-stu-id="b198c-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="b198c-138">Confira essa configuração no método prático [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do [WebHost](/dotnet/api/microsoft.aspnetcore.webhost):</span><span class="sxs-lookup"><span data-stu-id="b198c-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="b198c-139">A configuração baseada em arquivo é representada por [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="b198c-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="b198c-140">A `FileConfigurationSource` usa o [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) para monitorar arquivos.</span><span class="sxs-lookup"><span data-stu-id="b198c-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="b198c-141">Por padrão, o `IFileMonitor` é fornecido por um [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), que usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) para monitorar as alterações no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="b198c-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="b198c-142">O aplicativo de exemplo demonstra duas implementações para monitorar as alterações de configuração.</span><span class="sxs-lookup"><span data-stu-id="b198c-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="b198c-143">Se o arquivo *appsettings.json* é alterado ou a versão do Ambiente do arquivo é alterada, cada implementação executa um código personalizado.</span><span class="sxs-lookup"><span data-stu-id="b198c-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="b198c-144">O aplicativo de exemplo grava uma mensagem no console.</span><span class="sxs-lookup"><span data-stu-id="b198c-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="b198c-145">O `FileSystemWatcher` de um arquivo de configuração pode disparar vários retornos de chamada de token para uma única alteração de arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="b198c-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="b198c-146">A implementação da amostra protege contra esse problema verificando os hashes de arquivo nos arquivos de configuração.</span><span class="sxs-lookup"><span data-stu-id="b198c-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="b198c-147">A verificação de hashes de arquivo garante que, pelo menos, um dos arquivos de configuração foi alterado antes de executar o código personalizado.</span><span class="sxs-lookup"><span data-stu-id="b198c-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="b198c-148">A amostra usa o hash de arquivo SHA1 (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="b198c-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="b198c-149">Uma nova tentativa é implementada com uma retirada exponencial.</span><span class="sxs-lookup"><span data-stu-id="b198c-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="b198c-150">A nova tentativa está presente porque o bloqueio de arquivo pode ocorrer, o que impede temporariamente a computação de um novo hash em um dos arquivos.</span><span class="sxs-lookup"><span data-stu-id="b198c-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="b198c-151">Token de alteração de inicialização simples</span><span class="sxs-lookup"><span data-stu-id="b198c-151">Simple startup change token</span></span>

<span data-ttu-id="b198c-152">Registre um retorno de chamada `Action` do consumidor de token para notificações de alteração no token de recarregamento de configuração (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="b198c-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="b198c-153">`config.GetReloadToken()` fornece o token.</span><span class="sxs-lookup"><span data-stu-id="b198c-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="b198c-154">O retorno de chamada é o método `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="b198c-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="b198c-155">O `state` do retorno de chamada é usado para passar o `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="b198c-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="b198c-156">Isso é útil para determinar o arquivo de configuração JSON *appsettings* correto a ser monitorado, *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="b198c-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="b198c-157">Hashes de arquivo são usados para impedir que a instrução `WriteConsole` seja executada várias vezes, devido a vários retornos de chamada de token quando o arquivo de configuração é alterado somente uma vez.</span><span class="sxs-lookup"><span data-stu-id="b198c-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="b198c-158">Esse sistema é executado, desde que o aplicativo esteja em execução e não possa ser desabilitado pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="b198c-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="b198c-159">Monitorando alterações de configuração como um serviço</span><span class="sxs-lookup"><span data-stu-id="b198c-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="b198c-160">A amostra implementa:</span><span class="sxs-lookup"><span data-stu-id="b198c-160">The sample implements:</span></span>

* <span data-ttu-id="b198c-161">Monitoramento de token de inicialização básica.</span><span class="sxs-lookup"><span data-stu-id="b198c-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="b198c-162">Monitoramento como serviço.</span><span class="sxs-lookup"><span data-stu-id="b198c-162">Monitoring as a service.</span></span>
* <span data-ttu-id="b198c-163">Um mecanismo para habilitar e desabilitar o monitoramento.</span><span class="sxs-lookup"><span data-stu-id="b198c-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="b198c-164">A amostra estabelece uma interface `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="b198c-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="b198c-165">O construtor da classe implementada, `ConfigurationMonitor`, registra um retorno de chamada para notificações de alteração:</span><span class="sxs-lookup"><span data-stu-id="b198c-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="b198c-166">`config.GetReloadToken()` fornece o token.</span><span class="sxs-lookup"><span data-stu-id="b198c-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="b198c-167">`InvokeChanged` é o método de retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="b198c-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="b198c-168">O `state` nesta instância é uma cadeia de caracteres que descreve o estado de monitoramento.</span><span class="sxs-lookup"><span data-stu-id="b198c-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="b198c-169">Duas propriedades são usadas:</span><span class="sxs-lookup"><span data-stu-id="b198c-169">Two properties are used:</span></span>

* <span data-ttu-id="b198c-170">`MonitoringEnabled` indica se o retorno de chamada deve executar seu código personalizado.</span><span class="sxs-lookup"><span data-stu-id="b198c-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="b198c-171">`CurrentState` descreve o estado atual de monitoramento para uso na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="b198c-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="b198c-172">O método `InvokeChanged` é semelhante à abordagem anterior, exceto que ele:</span><span class="sxs-lookup"><span data-stu-id="b198c-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="b198c-173">Não executa o código, a menos que `MonitoringEnabled` seja `true`.</span><span class="sxs-lookup"><span data-stu-id="b198c-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="b198c-174">Define a cadeia de caracteres de propriedade `CurrentState` com uma mensagem descritiva que registra a hora em que o código foi executado.</span><span class="sxs-lookup"><span data-stu-id="b198c-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="b198c-175">Anota o `state` atual e sua saída `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="b198c-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="b198c-176">Uma instância `ConfigurationMonitor` é registrada como um serviço em `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b198c-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="b198c-177">A página Índice oferece ao usuário o controle sobre o monitoramento de configuração.</span><span class="sxs-lookup"><span data-stu-id="b198c-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="b198c-178">A instância de `IConfigurationMonitor` é injetada no `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="b198c-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="b198c-179">Um botão habilita e desabilita o monitoramento:</span><span class="sxs-lookup"><span data-stu-id="b198c-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="b198c-180">Quando `OnPostStartMonitoring` é disparado, o monitoramento é habilitado e o estado atual é desmarcado.</span><span class="sxs-lookup"><span data-stu-id="b198c-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="b198c-181">Quando `OnPostStopMonitoring` é disparado, o monitoramento é desabilitado e o estado é definido para refletir que o monitoramento não está ocorrendo.</span><span class="sxs-lookup"><span data-stu-id="b198c-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="b198c-182">Monitorando alterações de arquivos armazenados em cache</span><span class="sxs-lookup"><span data-stu-id="b198c-182">Monitoring cached file changes</span></span>

<span data-ttu-id="b198c-183">O conteúdo do arquivo pode ser armazenado em cache em memória usando [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="b198c-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="b198c-184">O cache na memória é descrito no tópico [Cache na memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="b198c-184">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="b198c-185">Sem realizar etapas adicionais, como a implementação descrita abaixo, dados *obsoletos* (desatualizados) são retornados de um cache se os dados de origem são alterados.</span><span class="sxs-lookup"><span data-stu-id="b198c-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="b198c-186">Não levando em conta o status de um arquivo de origem armazenado em cache durante a renovação de um período de [expiração deslizante](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) resulta em dados de cache obsoletos.</span><span class="sxs-lookup"><span data-stu-id="b198c-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="b198c-187">Cada solicitação de dados renova o período de expiração deslizante, mas o arquivo nunca é recarregado no cache.</span><span class="sxs-lookup"><span data-stu-id="b198c-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="b198c-188">Os recursos do aplicativo que usam o conteúdo armazenado em cache do arquivo estão sujeitos ao possível recebimento de conteúdo obsoleto.</span><span class="sxs-lookup"><span data-stu-id="b198c-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="b198c-189">O uso de tokens de alteração em um cenário de cache de arquivo impede o conteúdo de arquivo obsoleto no cache.</span><span class="sxs-lookup"><span data-stu-id="b198c-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="b198c-190">O aplicativo de exemplo demonstra uma implementação da abordagem.</span><span class="sxs-lookup"><span data-stu-id="b198c-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="b198c-191">A amostra usa `GetFileContent` para:</span><span class="sxs-lookup"><span data-stu-id="b198c-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="b198c-192">Retornar o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="b198c-192">Return file content.</span></span>
* <span data-ttu-id="b198c-193">Implementar um algoritmo de repetição com retirada exponencial para abranger casos em que um bloqueio de arquivo está impedindo temporariamente a leitura de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="b198c-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="b198c-194">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="b198c-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="b198c-195">Um `FileService` é criado para manipular pesquisas de arquivos armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="b198c-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="b198c-196">A chamada de método `GetFileContent` do serviço tenta obter o conteúdo do arquivo do cache em memória e retorná-lo para o chamador (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="b198c-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="b198c-197">Se o conteúdo armazenado em cache não é encontrado com a chave de cache, as seguintes ações são executadas:</span><span class="sxs-lookup"><span data-stu-id="b198c-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="b198c-198">O conteúdo do arquivo é obtido com `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="b198c-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="b198c-199">Um token de alteração é obtido do provedor de arquivo com [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="b198c-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="b198c-200">O retorno de chamada do token é disparado quando o arquivo é modificado.</span><span class="sxs-lookup"><span data-stu-id="b198c-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="b198c-201">O conteúdo do arquivo é armazenado em cache com um período de [expiração deslizante](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration).</span><span class="sxs-lookup"><span data-stu-id="b198c-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="b198c-202">O token de alteração é anexado com [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) para remover a entrada do cache se o arquivo é alterado enquanto ele é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="b198c-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="b198c-203">O `FileService` é registrado no contêiner de serviço junto com o serviço de cache da memória (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="b198c-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="b198c-204">O modelo de página carrega o conteúdo do arquivo usando o serviço (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b198c-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="b198c-205">Classe CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="b198c-205">CompositeChangeToken class</span></span>

<span data-ttu-id="b198c-206">Para representar uma ou mais instâncias de `IChangeToken` em um único objeto, use a classe [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).</span><span class="sxs-lookup"><span data-stu-id="b198c-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="b198c-207">`HasChanged` nos relatórios de token compostos `true` se um token representado `HasChanged` é `true`.</span><span class="sxs-lookup"><span data-stu-id="b198c-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="b198c-208">`ActiveChangeCallbacks` nos relatórios de token compostos `true` se um token representado `ActiveChangeCallbacks` é `true`.</span><span class="sxs-lookup"><span data-stu-id="b198c-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="b198c-209">Se ocorrerem vários eventos de alteração simultâneos, o retorno de chamada de alteração composto será invocado exatamente uma vez.</span><span class="sxs-lookup"><span data-stu-id="b198c-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="b198c-210">Consulte também</span><span class="sxs-lookup"><span data-stu-id="b198c-210">See also</span></span>

* [<span data-ttu-id="b198c-211">Cache na memória</span><span class="sxs-lookup"><span data-stu-id="b198c-211">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="b198c-212">Trabalhar com um cache distribuído</span><span class="sxs-lookup"><span data-stu-id="b198c-212">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="b198c-213">Detectar alterações com tokens de alteração</span><span class="sxs-lookup"><span data-stu-id="b198c-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="b198c-214">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="b198c-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="b198c-215">Middleware de Cache de Resposta</span><span class="sxs-lookup"><span data-stu-id="b198c-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="b198c-216">Auxiliar de marca de cache</span><span class="sxs-lookup"><span data-stu-id="b198c-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="b198c-217">Auxiliar de marca de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="b198c-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
