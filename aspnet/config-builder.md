---
uid: config-builder
title: Construtores de configuração para o ASP.NET
author: rick-anderson
description: Saiba como obter dados de configuração de fontes diferentes valores de Web. config de fontes externas.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148831"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="4ea49-103">Construtores de configuração para o ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4ea49-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="4ea49-104">Por [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ea49-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ea49-105">Construtores de configuração fornecem um mecanismo ágil e modernos para aplicativos ASP.NET obtenham os valores de configuração de fontes externas.</span><span class="sxs-lookup"><span data-stu-id="4ea49-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="4ea49-106">Construtores de configuração:</span><span class="sxs-lookup"><span data-stu-id="4ea49-106">Configuration builders:</span></span>

* <span data-ttu-id="4ea49-107">Estão disponíveis no .NET Framework 4.7.1 e posterior.</span><span class="sxs-lookup"><span data-stu-id="4ea49-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="4ea49-108">Fornece um mecanismo flexível para ler valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="4ea49-109">Aborda algumas das necessidades básicas de aplicativos, como eles se movam em um contêiner e com foco do ambiente de nuvem.</span><span class="sxs-lookup"><span data-stu-id="4ea49-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="4ea49-110">Pode ser usado para melhorar a proteção de dados de configuração pelo desenho de fontes não está disponíveis anteriormente (por exemplo, variáveis de ambiente e o Azure Key Vault) no sistema de configuração do .NET.</span><span class="sxs-lookup"><span data-stu-id="4ea49-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="4ea49-111">Construtores de configuração de chave/valor</span><span class="sxs-lookup"><span data-stu-id="4ea49-111">Key/value configuration builders</span></span>

<span data-ttu-id="4ea49-112">Um cenário comum que pode ser tratado por construtores de configuração é fornecer um mecanismo de substituição de chave/valor básico para seções de configuração que seguem um padrão de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="4ea49-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="4ea49-113">O conceito do .NET Framework de ConfigurationBuilders não está limitado a seções de configuração específico ou padrões.</span><span class="sxs-lookup"><span data-stu-id="4ea49-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="4ea49-114">No entanto, muitos dos construtores de configuração no `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionam dentro do padrão de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="4ea49-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="4ea49-115">Definições de construtores de configuração de chave/valor</span><span class="sxs-lookup"><span data-stu-id="4ea49-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="4ea49-116">As configurações a seguir se aplicam a todos os construtores de configuração de chave/valor no `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="4ea49-117">Modo</span><span class="sxs-lookup"><span data-stu-id="4ea49-117">Mode</span></span>

<span data-ttu-id="4ea49-118">Os construtores de configuração usam uma fonte externa de informações de chave/valor para preencher os elementos de chave/valor selecionado de sistema de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="4ea49-119">Especificamente, o `<appSettings/>` e `<connectionStrings/>` seções recebem tratamento especial de construtores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="4ea49-120">Os construtores de trabalho em três modos:</span><span class="sxs-lookup"><span data-stu-id="4ea49-120">The builders work in three modes:</span></span>

* <span data-ttu-id="4ea49-121">`Strict` -O modo padrão.</span><span class="sxs-lookup"><span data-stu-id="4ea49-121">`Strict` - The default mode.</span></span> <span data-ttu-id="4ea49-122">Nesse modo, o construtor de configuração só funciona em seções de configuração de chave/valor-centrado em bem conhecidos.</span><span class="sxs-lookup"><span data-stu-id="4ea49-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="4ea49-123">`Strict` modo enumera cada chave na seção.</span><span class="sxs-lookup"><span data-stu-id="4ea49-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="4ea49-124">Se uma chave correspondente for encontrada na origem externa:</span><span class="sxs-lookup"><span data-stu-id="4ea49-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="4ea49-125">Os construtores de configuração substitua o valor na seção de configuração resultante com o valor da fonte externa.</span><span class="sxs-lookup"><span data-stu-id="4ea49-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="4ea49-126">`Greedy` -Esse modo está intimamente relacionado ao `Strict` modo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="4ea49-127">Em vez de ficar limitado às chaves que já existem na configuração original:</span><span class="sxs-lookup"><span data-stu-id="4ea49-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="4ea49-128">Os construtores de configuração adiciona todos os pares de chave/valor da fonte externa para a seção de configuração resultante.</span><span class="sxs-lookup"><span data-stu-id="4ea49-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="4ea49-129">`Expand` -Opera em XML brutos antes que ele é analisado em um objeto da seção de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="4ea49-130">Ele pode ser pensado como uma expansão de tokens em uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4ea49-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="4ea49-131">Qualquer parte da cadeia de caracteres XML brutos que corresponde ao padrão de `${token}` é um candidato para expansão de token.</span><span class="sxs-lookup"><span data-stu-id="4ea49-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="4ea49-132">Se nenhum valor correspondente for encontrado na fonte externa, o token não é alterado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="4ea49-133">Os construtores nesse modo não estão limitados à `<appSettings/>` e `<connectionStrings/>` seções.</span><span class="sxs-lookup"><span data-stu-id="4ea49-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="4ea49-134">A seguinte marcação de *Web. config* permite que o [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) em `Strict` modo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="4ea49-135">O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` mostrada anteriormente *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="4ea49-136">O código anterior definirá os valores de propriedade:</span><span class="sxs-lookup"><span data-stu-id="4ea49-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="4ea49-137">Os valores de *Web. config* arquivo se as chaves não forem definidas nas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4ea49-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="4ea49-138">Os valores do ambiente variável, se definido.</span><span class="sxs-lookup"><span data-stu-id="4ea49-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="4ea49-139">Por exemplo, `ServiceID` conterá:</span><span class="sxs-lookup"><span data-stu-id="4ea49-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="4ea49-140">"Valor ServiceID do Web. config", se a variável de ambiente `ServiceID` não está definido.</span><span class="sxs-lookup"><span data-stu-id="4ea49-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="4ea49-141">O valor da `ServiceID` se variável de ambiente definida.</span><span class="sxs-lookup"><span data-stu-id="4ea49-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="4ea49-142">A imagem a seguir mostra a `<appSettings/>` chaves/valores de anterior *Web. config* conjunto de arquivos no editor de ambiente:</span><span class="sxs-lookup"><span data-stu-id="4ea49-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![editor de ambiente](config-builder/static/env.png)

<span data-ttu-id="4ea49-144">Observação: Talvez você precise sair e reiniciar o Visual Studio para ver as alterações nas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4ea49-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="4ea49-145">Tratamento de prefixo</span><span class="sxs-lookup"><span data-stu-id="4ea49-145">Prefix handling</span></span>

<span data-ttu-id="4ea49-146">Prefixos de chave podem simplificar as chaves de configuração porque:</span><span class="sxs-lookup"><span data-stu-id="4ea49-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="4ea49-147">Configuração do .NET Framework é complexo e aninhada.</span><span class="sxs-lookup"><span data-stu-id="4ea49-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="4ea49-148">Fontes externas de chave/valor são comumente básica e simples por natureza.</span><span class="sxs-lookup"><span data-stu-id="4ea49-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="4ea49-149">Por exemplo, as variáveis de ambiente não estão aninhadas.</span><span class="sxs-lookup"><span data-stu-id="4ea49-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="4ea49-150">Use qualquer uma das abordagens a seguir para inserir tanto `<appSettings/>` e `<connectionStrings/>` para a configuração por meio de variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="4ea49-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="4ea49-151">Com o `EnvironmentConfigBuilder` no padrão `Strict` modo e os nomes de chave apropriados no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="4ea49-152">O código e a marcação anterior usa essa abordagem.</span><span class="sxs-lookup"><span data-stu-id="4ea49-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="4ea49-153">Usando essa abordagem, você pode **não** têm nomes idênticos chaves em ambos `<appSettings/>` e `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="4ea49-154">Use dois `EnvironmentConfigBuilder`s no `Greedy` modo com prefixos distintos e `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="4ea49-155">Com essa abordagem, o aplicativo pode ler `<appSettings/>` e `<connectionStrings/>` sem a necessidade de atualizar o arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="4ea49-156">A próxima seção, [stripPrefix](#stripprefix), mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="4ea49-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="4ea49-157">Use dois `EnvironmentConfigBuilder`s em `Greedy` modo com prefixos distintos.</span><span class="sxs-lookup"><span data-stu-id="4ea49-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="4ea49-158">Com essa abordagem, você não pode ter nomes duplicados de chave como nomes de chave devem ser diferentes por prefixo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="4ea49-159">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="4ea49-160">Com a marcação anterior, a mesma fonte de chave/valor simples pode ser usada para preencher a configuração para duas seções diferentes.</span><span class="sxs-lookup"><span data-stu-id="4ea49-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="4ea49-161">A imagem a seguir mostra a `<appSettings/>` e `<connectionStrings/>` chaves/valores de anterior *Web. config* conjunto de arquivos no editor de ambiente:</span><span class="sxs-lookup"><span data-stu-id="4ea49-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![editor de ambiente](config-builder/static/prefix.png)

<span data-ttu-id="4ea49-163">O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` chaves/valores contidos no anterior *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="4ea49-164">O código anterior definirá os valores de propriedade:</span><span class="sxs-lookup"><span data-stu-id="4ea49-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="4ea49-165">Os valores de *Web. config* arquivo se as chaves não forem definidas nas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4ea49-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="4ea49-166">Os valores do ambiente variável, se definido.</span><span class="sxs-lookup"><span data-stu-id="4ea49-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="4ea49-167">Por exemplo, o uso anterior *Web. config* arquivos, chaves/valores na imagem anterior do editor de ambiente e o código anterior, os seguintes valores são definidos:</span><span class="sxs-lookup"><span data-stu-id="4ea49-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="4ea49-168">Chave</span><span class="sxs-lookup"><span data-stu-id="4ea49-168">Key</span></span>              | <span data-ttu-id="4ea49-169">Valor</span><span class="sxs-lookup"><span data-stu-id="4ea49-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="4ea49-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="4ea49-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="4ea49-171">AppSetting_ServiceID de variáveis de env</span><span class="sxs-lookup"><span data-stu-id="4ea49-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="4ea49-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="4ea49-172">AppSetting_default</span></span>            | <span data-ttu-id="4ea49-173">AppSetting_default valor de env</span><span class="sxs-lookup"><span data-stu-id="4ea49-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="4ea49-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="4ea49-174">ConnStr_default</span></span>         | <span data-ttu-id="4ea49-175">Val ConnStr_default de env</span><span class="sxs-lookup"><span data-stu-id="4ea49-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="4ea49-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="4ea49-176">stripPrefix</span></span>

<span data-ttu-id="4ea49-177">`stripPrefix`: booliano, o padrão é `false`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="4ea49-178">A marcação XML anterior separa as configurações do aplicativo de cadeias de caracteres de conexão, mas exige que todas as chaves na *Web. config* arquivo para usar o prefixo especificado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="4ea49-179">Por exemplo, o prefixo `AppSetting` deve ser adicionado para o `ServiceID` chave ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="4ea49-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="4ea49-180">Com o `stripPrefix`, o prefixo não é usado o *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="4ea49-181">O prefixo é necessária na fonte do construtor de configuração (por exemplo, no ambiente). Prevemos que a maioria dos desenvolvedores usará `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="4ea49-182">Aplicativos normalmente removem o prefixo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="4ea49-183">O seguinte *Web. config* retira o prefixo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="4ea49-184">Na anterior *Web. config* arquivo, o `default` chave está em ambos os `<appSettings/>` e `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="4ea49-185">A imagem a seguir mostra a `<appSettings/>` e `<connectionStrings/>` chaves/valores de anterior *Web. config* conjunto de arquivos no editor de ambiente:</span><span class="sxs-lookup"><span data-stu-id="4ea49-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![editor de ambiente](config-builder/static/prefix.png)

<span data-ttu-id="4ea49-187">O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` chaves/valores contidos no anterior *Web. config* arquivo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="4ea49-188">O código anterior definirá os valores de propriedade:</span><span class="sxs-lookup"><span data-stu-id="4ea49-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="4ea49-189">Os valores de *Web. config* arquivo se as chaves não forem definidas nas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4ea49-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="4ea49-190">Os valores do ambiente variável, se definido.</span><span class="sxs-lookup"><span data-stu-id="4ea49-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="4ea49-191">Por exemplo, o uso anterior *Web. config* arquivos, chaves/valores na imagem anterior do editor de ambiente e o código anterior, os seguintes valores são definidos:</span><span class="sxs-lookup"><span data-stu-id="4ea49-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="4ea49-192">Chave</span><span class="sxs-lookup"><span data-stu-id="4ea49-192">Key</span></span>              | <span data-ttu-id="4ea49-193">Valor</span><span class="sxs-lookup"><span data-stu-id="4ea49-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="4ea49-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="4ea49-194">ServiceID</span></span>           | <span data-ttu-id="4ea49-195">AppSetting_ServiceID de variáveis de env</span><span class="sxs-lookup"><span data-stu-id="4ea49-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="4ea49-196">default</span><span class="sxs-lookup"><span data-stu-id="4ea49-196">default</span></span>            | <span data-ttu-id="4ea49-197">AppSetting_default valor de env</span><span class="sxs-lookup"><span data-stu-id="4ea49-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="4ea49-198">default</span><span class="sxs-lookup"><span data-stu-id="4ea49-198">default</span></span>         | <span data-ttu-id="4ea49-199">Val ConnStr_default de env</span><span class="sxs-lookup"><span data-stu-id="4ea49-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="4ea49-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="4ea49-200">tokenPattern</span></span>

<span data-ttu-id="4ea49-201">`tokenPattern`: Cadeia de caracteres, o padrão é `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="4ea49-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="4ea49-202">O `Expand` comportamento dos construtores de procura o XML bruto para tokens que se parecem com `${token}`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="4ea49-203">Pesquisando é feito com a expressão regular do padrão `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="4ea49-204">O conjunto de caracteres que corresponde ao `\w` é mais estrita que permitem que XML e muitas fontes de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="4ea49-205">Use `tokenPattern` quando mais caracteres que `@"\$\{(\w+)\}"` são necessárias no nome do token.</span><span class="sxs-lookup"><span data-stu-id="4ea49-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="4ea49-206">`tokenPattern`: Cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="4ea49-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="4ea49-207">Permite aos desenvolvedores alterar a expressão regular que é usada para correspondência de token.</span><span class="sxs-lookup"><span data-stu-id="4ea49-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="4ea49-208">Nenhuma validação é feita para verificar se que ele é um regex bem formado, não pode ser perigoso.</span><span class="sxs-lookup"><span data-stu-id="4ea49-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="4ea49-209">Ele deve conter um grupo de captura.</span><span class="sxs-lookup"><span data-stu-id="4ea49-209">It must contain a capture group.</span></span> <span data-ttu-id="4ea49-210">O regex inteira deve corresponder o token inteiro.</span><span class="sxs-lookup"><span data-stu-id="4ea49-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="4ea49-211">A primeira captura deve ser o nome do token para pesquisar na fonte de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="4ea49-212">Construtores de configuração em Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="4ea49-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="4ea49-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="4ea49-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="4ea49-214">O [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="4ea49-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="4ea49-215">É o mais simples dos construtores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="4ea49-216">Lê os valores do ambiente.</span><span class="sxs-lookup"><span data-stu-id="4ea49-216">Reads values from the environment.</span></span>
* <span data-ttu-id="4ea49-217">Não tem nenhuma opção de configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="4ea49-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="4ea49-218">O `name` valor do atributo é arbitrário.</span><span class="sxs-lookup"><span data-stu-id="4ea49-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="4ea49-219">**Observação:** em um ambiente de contêiner do Windows, as variáveis definidas em tempo de execução só são injetadas no ambiente do processo de ponto de entrada.</span><span class="sxs-lookup"><span data-stu-id="4ea49-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="4ea49-220">Aplicativos que são executados como um serviço ou um processo não EntryPoint não detectam essas variáveis, a menos que caso contrário, eles são injetados por meio de um mecanismo no contêiner.</span><span class="sxs-lookup"><span data-stu-id="4ea49-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="4ea49-221">Para [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-baseados em contêineres, a versão atual do [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) lida com isso no *DefaultAppPool* somente.</span><span class="sxs-lookup"><span data-stu-id="4ea49-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="4ea49-222">Talvez seja necessário desenvolver seu próprio mecanismo de injeção para processos de ponto de entrada não outras variantes do contêiner com base em Windows.</span><span class="sxs-lookup"><span data-stu-id="4ea49-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="4ea49-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="4ea49-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="4ea49-224">Nunca armazenar senhas, cadeias de caracteres de conexão confidenciais ou outros dados confidenciais no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="4ea49-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="4ea49-225">Segredos de produção não devem ser usados para desenvolvimento ou teste.</span><span class="sxs-lookup"><span data-stu-id="4ea49-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="4ea49-226">Esse construtor de configuração fornece um recurso semelhante ao [Secret Manager do ASP.NET Core](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4ea49-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="4ea49-227">O [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) pode ser usado em projetos do .NET Framework, mas deve ser especificado um arquivo de segredos.</span><span class="sxs-lookup"><span data-stu-id="4ea49-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="4ea49-228">Como alternativa, você pode definir o `UserSecretsId` propriedade no projeto do arquivo e cria o arquivo bruto segredos no local correto para leitura.</span><span class="sxs-lookup"><span data-stu-id="4ea49-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="4ea49-229">Para manter as dependências externas fora do seu projeto, o arquivo de segredo é XML formatado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="4ea49-230">A formatação XML é um detalhe de implementação e o formato não deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="4ea49-231">Se você precisa compartilhar um *Secrets* do arquivo com projetos do .NET Core, considere usar o [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="4ea49-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="4ea49-232">O `SimpleJsonConfigBuilder` para .NET Core também deve ser considerado um detalhe de implementação, sujeito a alterações de formato.</span><span class="sxs-lookup"><span data-stu-id="4ea49-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="4ea49-233">Configuração de atributos para `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4ea49-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="4ea49-234">`userSecretsId` -Este é o método preferencial para identificar um arquivo de segredos XML.</span><span class="sxs-lookup"><span data-stu-id="4ea49-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="4ea49-235">Ele funciona de forma semelhante ao .NET Core, que usa um `UserSecretsId` propriedade para armazenar esse identificador do projeto.</span><span class="sxs-lookup"><span data-stu-id="4ea49-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="4ea49-236">A cadeia de caracteres deve ser exclusiva, ele não precisa ser um GUID.</span><span class="sxs-lookup"><span data-stu-id="4ea49-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="4ea49-237">Com esse atributo, o `UserSecretsConfigBuilder` examinar uma localização local conhecida (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) para um arquivo de segredos que pertencem a esse identificador.</span><span class="sxs-lookup"><span data-stu-id="4ea49-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="4ea49-238">`userSecretsFile` -Um atributo opcional especificando o arquivo que contém os segredos.</span><span class="sxs-lookup"><span data-stu-id="4ea49-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="4ea49-239">O `~` caractere pode ser usado no início para fazer referência a raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="4ea49-240">Qualquer um desse atributo ou o `userSecretsId` atributo é necessário.</span><span class="sxs-lookup"><span data-stu-id="4ea49-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="4ea49-241">Se ambos forem especificados, `userSecretsFile` terá precedência.</span><span class="sxs-lookup"><span data-stu-id="4ea49-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="4ea49-242">`optional`: boolean, valor de padrão `true` -impede que uma exceção se o arquivo de segredos não pode ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="4ea49-243">O `name` valor do atributo é arbitrário.</span><span class="sxs-lookup"><span data-stu-id="4ea49-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="4ea49-244">O arquivo de segredos tem o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="4ea49-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="4ea49-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="4ea49-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="4ea49-246">O [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lê os valores armazenados na [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="4ea49-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="4ea49-247">`vaultName` é necessário (o nome do cofre) ou um URI para o cofre.</span><span class="sxs-lookup"><span data-stu-id="4ea49-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="4ea49-248">Os outros atributos permitem o controle sobre qual cofre para se conectar ao, mas serão necessárias apenas se o aplicativo não está em execução em um ambiente que funciona com `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="4ea49-249">A biblioteca de autenticação de serviços do Azure é usada para selecionar automaticamente as informações de conexão do ambiente de execução se possível.</span><span class="sxs-lookup"><span data-stu-id="4ea49-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="4ea49-250">Você pode substituir automaticamente pegar as informações de conexão, fornecendo uma cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="4ea49-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="4ea49-251">`vaultName` -Necessário se `uri` no não fornecido.</span><span class="sxs-lookup"><span data-stu-id="4ea49-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="4ea49-252">Especifica o nome do cofre em sua assinatura do Azure do qual ler os pares chave/valor.</span><span class="sxs-lookup"><span data-stu-id="4ea49-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="4ea49-253">`connectionString` -Uma cadeia de caracteres de conexão utilizável por [o AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="4ea49-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="4ea49-254">`uri` -Conecta-se para outros provedores de Cofre de chaves especificado `uri` valor.</span><span class="sxs-lookup"><span data-stu-id="4ea49-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="4ea49-255">Se não for especificado, do Azure (`vaultName`) é o provedor do cofre.</span><span class="sxs-lookup"><span data-stu-id="4ea49-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="4ea49-256">`version` -O azure Key Vault fornece um recurso de controle de versão para segredos.</span><span class="sxs-lookup"><span data-stu-id="4ea49-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="4ea49-257">Se `version` for especificado, o construtor apenas recupera segredos compatível com esta versão.</span><span class="sxs-lookup"><span data-stu-id="4ea49-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="4ea49-258">`preloadSecretNames` -Por padrão, esse construtor querys **todos os** chave nomes no cofre de chaves quando ele é inicializado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="4ea49-259">Para evitar a leitura de todos os valores de chave, defina esse atributo como `false`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="4ea49-260">Definir isso como `false` lê segredos um por vez.</span><span class="sxs-lookup"><span data-stu-id="4ea49-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="4ea49-261">Lendo um de cada vez pode útil se o cofre permite o acesso de "Get", mas não a "lista" acessar os segredos.</span><span class="sxs-lookup"><span data-stu-id="4ea49-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="4ea49-262">**Observação:** ao usar `Greedy` modo `preloadSecretNames` deve ser `true` (o padrão).</span><span class="sxs-lookup"><span data-stu-id="4ea49-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="4ea49-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="4ea49-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="4ea49-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) é um construtor de configuração básica que usa arquivos de um diretório como uma fonte de valores.</span><span class="sxs-lookup"><span data-stu-id="4ea49-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="4ea49-265">Nome de um arquivo é a chave e o conteúdo é o valor.</span><span class="sxs-lookup"><span data-stu-id="4ea49-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="4ea49-266">Esse construtor de configuração pode ser útil ao executar em um ambiente de contêiner orquestrada.</span><span class="sxs-lookup"><span data-stu-id="4ea49-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="4ea49-267">Sistemas, como Docker Swarm e Kubernetes fornecem `secrets` para seus contêineres do windows orquestrada dessa maneira chave por arquivo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="4ea49-268">Detalhes do atributo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-268">Attribute details:</span></span>

* <span data-ttu-id="4ea49-269">`directoryPath` -Required.</span><span class="sxs-lookup"><span data-stu-id="4ea49-269">`directoryPath` - Required.</span></span> <span data-ttu-id="4ea49-270">Especifica um caminho para procurar valores.</span><span class="sxs-lookup"><span data-stu-id="4ea49-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="4ea49-271">Docker para Windows segredos são armazenados na *C:\ProgramData\Docker\secrets* diretório por padrão.</span><span class="sxs-lookup"><span data-stu-id="4ea49-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="4ea49-272">`ignorePrefix` -Arquivos que começam com esse prefixo serão excluídas.</span><span class="sxs-lookup"><span data-stu-id="4ea49-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="4ea49-273">O padrão é "ignore".</span><span class="sxs-lookup"><span data-stu-id="4ea49-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="4ea49-274">`keyDelimiter` -O valor padrão é `null`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="4ea49-275">Se for especificado, o construtor de configuração atravessa vários níveis de diretório, a criação de nomes de chave com este delimitador.</span><span class="sxs-lookup"><span data-stu-id="4ea49-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="4ea49-276">Se esse valor for `null`, o construtor de configuração olha apenas para o nível superior do diretório.</span><span class="sxs-lookup"><span data-stu-id="4ea49-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="4ea49-277">`optional` -O valor padrão é `false`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="4ea49-278">Especifica se o construtor de configuração deve causar erros se o diretório de origem não existe.</span><span class="sxs-lookup"><span data-stu-id="4ea49-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="4ea49-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="4ea49-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="4ea49-280">Nunca armazenar senhas, cadeias de caracteres de conexão confidenciais ou outros dados confidenciais no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="4ea49-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="4ea49-281">Segredos de produção não devem ser usados para desenvolvimento ou teste.</span><span class="sxs-lookup"><span data-stu-id="4ea49-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="4ea49-282">Projetos do .NET core frequentemente usam arquivos JSON para a configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="4ea49-283">O [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) construtor permite que os arquivos de JSON do .NET Core a ser usado no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4ea49-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="4ea49-284">Esse construtor de configuração é fornece um mapeamento básico de uma fonte de chave/valor simples em áreas específicas de chave/valor de configuração do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4ea49-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="4ea49-285">Esse construtor de configuração faz **não** fornecem configurações hierárquica.</span><span class="sxs-lookup"><span data-stu-id="4ea49-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="4ea49-286">O arquivo de backup do JSON é semelhante a um dicionário, não um objeto hierárquico complexo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="4ea49-287">Um arquivo hierárquico multinível pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="4ea49-288">Esse provedor `flatten`s a profundidade, acrescentando o nome da propriedade em cada nível usando `:` como um delimitador.</span><span class="sxs-lookup"><span data-stu-id="4ea49-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="4ea49-289">Detalhes do atributo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-289">Attribute details:</span></span>

* <span data-ttu-id="4ea49-290">`jsonFile` -Required.</span><span class="sxs-lookup"><span data-stu-id="4ea49-290">`jsonFile` - Required.</span></span> <span data-ttu-id="4ea49-291">Especifica o arquivo JSON para ler.</span><span class="sxs-lookup"><span data-stu-id="4ea49-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="4ea49-292">O `~` caractere pode ser usado no início para fazer referência a raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4ea49-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="4ea49-293">`optional` -Booliano, o valor padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="4ea49-294">Impede a gerar exceções se o arquivo JSON não pode ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="4ea49-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="4ea49-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="4ea49-296">`Flat` é o padrão.</span><span class="sxs-lookup"><span data-stu-id="4ea49-296">`Flat` is the default.</span></span> <span data-ttu-id="4ea49-297">Quando `jsonMode` é `Flat`, o arquivo JSON é uma fonte de chave/valor único simples.</span><span class="sxs-lookup"><span data-stu-id="4ea49-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="4ea49-298">O `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` também são fontes de chave/valor único simples.</span><span class="sxs-lookup"><span data-stu-id="4ea49-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="4ea49-299">Quando o `SimpleJsonConfigBuilder` está configurado no `Sectional` modo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="4ea49-300">O arquivo JSON é dividido conceitualmente apenas no nível superior em vários dicionários.</span><span class="sxs-lookup"><span data-stu-id="4ea49-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="4ea49-301">Cada um dos dicionários só será aplicada à seção de configuração que corresponde ao nome de propriedade de nível superior anexado a eles.</span><span class="sxs-lookup"><span data-stu-id="4ea49-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="4ea49-302">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4ea49-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="4ea49-303">Implementando um construtor de configuração de chave/valor personalizados</span><span class="sxs-lookup"><span data-stu-id="4ea49-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="4ea49-304">Se os construtores de configuração não atenderem às suas necessidades, você pode escrever um personalizado.</span><span class="sxs-lookup"><span data-stu-id="4ea49-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="4ea49-305">O `KeyValueConfigBuilder` classe base manipula a maioria das preocupações de prefixo e modos de substituição.</span><span class="sxs-lookup"><span data-stu-id="4ea49-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="4ea49-306">Um projeto de implementação só são necessárias:</span><span class="sxs-lookup"><span data-stu-id="4ea49-306">An implementing project need only:</span></span>

* <span data-ttu-id="4ea49-307">Herdar da classe base e implementar uma fonte básica de pares chave/valor por meio de `GetValue` e `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="4ea49-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="4ea49-308">Adicione a [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) ao projeto.</span><span class="sxs-lookup"><span data-stu-id="4ea49-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="4ea49-309">O `KeyValueConfigBuilder` classe base fornece grande parte do comportamento do trabalho e consistente entre chave/valor construtores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4ea49-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ea49-310">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4ea49-310">Additional resources</span></span>

* [<span data-ttu-id="4ea49-311">Repositório GitHub de construtores de configuração</span><span class="sxs-lookup"><span data-stu-id="4ea49-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="4ea49-312">Autenticação serviço a serviço para o Azure Key Vault usando o .NET</span><span class="sxs-lookup"><span data-stu-id="4ea49-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
