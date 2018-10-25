---
title: Configuração no ASP.NET Core
author: guardrex
description: Saiba como usar a API de configuração para configurar um aplicativo do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 35f283becd156da22a4d9d2034055ee79b75ffda
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326167"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="4fbc4-103">Configuração no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fbc4-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="4fbc4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4fbc4-105">A configuração de aplicativos no ASP.NET Core se baseia em pares chave-valor estabelecidos por *provedores de configuração*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="4fbc4-106">Os provedores de configuração leem os dados de configuração em pares chave-valor de várias fontes de configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="4fbc4-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4fbc4-107">Azure Key Vault</span></span>
* <span data-ttu-id="4fbc4-108">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-108">Command-line arguments</span></span>
* <span data-ttu-id="4fbc4-109">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4fbc4-110">Arquivos de diretório</span><span class="sxs-lookup"><span data-stu-id="4fbc4-110">Directory files</span></span>
* <span data-ttu-id="4fbc4-111">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-111">Environment variables</span></span>
* <span data-ttu-id="4fbc4-112">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-112">In-memory .NET objects</span></span>
* <span data-ttu-id="4fbc4-113">Arquivos de configurações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="4fbc4-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4fbc4-114">Azure Key Vault</span></span>
* <span data-ttu-id="4fbc4-115">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-115">Command-line arguments</span></span>
* <span data-ttu-id="4fbc4-116">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4fbc4-117">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-117">Environment variables</span></span>
* <span data-ttu-id="4fbc4-118">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-118">In-memory .NET objects</span></span>
* <span data-ttu-id="4fbc4-119">Arquivos de configurações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="4fbc4-120">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-120">Command-line arguments</span></span>
* <span data-ttu-id="4fbc4-121">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4fbc4-122">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-122">Environment variables</span></span>
* <span data-ttu-id="4fbc4-123">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-123">In-memory .NET objects</span></span>
* <span data-ttu-id="4fbc4-124">Arquivos de configurações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="4fbc4-125">O *padrão de opções* é uma extensão dos conceitos de configuração descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="4fbc4-126">As opções usam classes para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="4fbc4-127">Para saber mais sobre como usar o padrão de opções, confira <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4fbc4-128">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4fbc4-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4fbc4-129">Os exemplos apresentados neste tópico dependem do seguinte:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="4fbc4-130">Definição do caminho de base do aplicativo com <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4fbc4-131">`SetBasePath` é disponibilizado para um aplicativo fazendo referência ao pacote [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="4fbc4-132">Resolução de seções dos arquivos de configuração com <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="4fbc4-133">`GetSection` é disponibilizado a um aplicativo fazendo referência ao pacote [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="4fbc4-134">Configuração de associação a classes .NET com <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> e [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="4fbc4-135">`Bind` e `Get<T>` são disponibilizados a um aplicativo fazendo referência ao pacote [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="4fbc4-136">`Get<T>` está disponível no ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-137">Esses três pacotes estão incluídos no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-138">Esses três pacotes estão incluídos no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="4fbc4-139">Configuração do host versus aplicativo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-139">Host vs. app configuration</span></span>

<span data-ttu-id="4fbc4-140">Antes do aplicativo ser configurado e iniciado, um *host* é configurado e iniciado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="4fbc4-141">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4fbc4-142">O aplicativo e o host são configurados usando os provedores de configuração descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="4fbc4-143">Os pares chave-valor de configuração do host se tornam parte da configuração global do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="4fbc4-144">Para saber mais sobre como os provedores de configuração são usados quando o host for compilado, e como as fontes de configuração afetam o host e a configuração, confira <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="4fbc4-145">Segurança</span><span class="sxs-lookup"><span data-stu-id="4fbc4-145">Security</span></span>

<span data-ttu-id="4fbc4-146">Adote as melhores práticas a seguir:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="4fbc4-147">Nunca armazene senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="4fbc4-148">Não use segredos de produção em ambientes de teste ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="4fbc4-149">Especifique segredos fora do projeto para que eles não sejam acidentalmente comprometidos com um repositório de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="4fbc4-150">Saiba mais sobre [como usar vários ambientes](xref:fundamentals/environments) e gerenciar o [armazenamento seguro de segredos de aplicativo em desenvolvimento com o Gerenciador de Segredo](xref:security/app-secrets) (inclui recomendações sobre como usar variáveis de ambiente para armazenar dados confidenciais).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="4fbc4-151">O Gerenciador de Segredo usa o Provedor de Configuração de Arquivo para armazenar segredos do usuário em um arquivo JSON no sistema local.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="4fbc4-152">O Provedor de Configuração de Arquivo será descrito mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4fbc4-153">O [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) é uma opção para o armazenamento seguro de segredos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="4fbc4-154">Para obter mais informações, consulte <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="4fbc4-155">Dados de configuração hierárquica</span><span class="sxs-lookup"><span data-stu-id="4fbc4-155">Hierarchical configuration data</span></span>

<span data-ttu-id="4fbc4-156">A API de Configuração é capaz de manter dados de configuração hierárquica nivelando os dados hierárquicos com o uso de um delimitador nas chaves de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="4fbc4-157">No seguinte arquivo JSON, há quatro chaves em uma hierarquia estruturada com duas seções:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="4fbc4-158">Quando o arquivo é lido na configuração, ocorre a criação de chaves exclusivas para manter a estrutura hierárquica de dados original da fonte de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="4fbc4-159">As seções e as chaves são niveladas usando dois-pontos (`:`) para manter a estrutura original:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="4fbc4-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-160">section0:key0</span></span>
* <span data-ttu-id="4fbc4-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-161">section0:key1</span></span>
* <span data-ttu-id="4fbc4-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-162">section1:key0</span></span>
* <span data-ttu-id="4fbc4-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-163">section1:key1</span></span>

<span data-ttu-id="4fbc4-164">Os métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> estão disponíveis para isolar as seções e os filhos de uma seção nos dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="4fbc4-165">Esses métodos serão descritos posteriormente em [GetSection, GetChildren e Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="4fbc4-166">Convenções</span><span class="sxs-lookup"><span data-stu-id="4fbc4-166">Conventions</span></span>

<span data-ttu-id="4fbc4-167">Na inicialização do aplicativo, as fontes de configuração são lidas na ordem especificada pelos provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="4fbc4-168">Os Provedores de Configuração de Arquivo têm a capacidade de recarregar a configuração quando um arquivo de configurações subjacente é alterado após a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="4fbc4-169">O Provedor de Configuração de Arquivo será descrito mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4fbc4-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponível no contêiner [DI (injeção de dependência)](xref:fundamentals/dependency-injection) do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4fbc4-171">Os provedores de configuração não podem utilizar a DI, pois ela não é disponibilizada quando eles são configurados pelo host.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="4fbc4-172">As chaves de configuração adotam as convenções a seguir:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="4fbc4-173">As chaves não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-173">Keys are case-insensitive.</span></span> <span data-ttu-id="4fbc4-174">Por exemplo, `ConnectionString` e `connectionstring` são tratados como chaves equivalentes.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="4fbc4-175">Se um valor para a mesma chave for definido pelos mesmos provedores de configuração, ou por outros, o último valor definido na chave será o valor usado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="4fbc4-176">Chaves hierárquicas</span><span class="sxs-lookup"><span data-stu-id="4fbc4-176">Hierarchical keys</span></span>
  * <span data-ttu-id="4fbc4-177">Ao interagir com a API de configuração, um separador de dois-pontos (`:`) funciona em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="4fbc4-178">Nas variáveis de ambiente, talvez um separador de dois-pontos não funcione em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="4fbc4-179">Um sublinhado duplo (`__`) é compatível com todas as plataformas e é convertido em dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="4fbc4-180">No Azure Key Vault, as chaves hierárquicas usam `--` (dois traços) como separador.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="4fbc4-181">Você deve fornecer o código para substituir os traços por dois-pontos quando os segredos forem carregados na configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="4fbc4-182">O <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> dá suporte a matrizes de associação para objetos usando os índices em chaves de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4fbc4-183">A associação de matriz está descrita na seção [Associar uma matriz a uma classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="4fbc4-184">Os valores de configuração adotam as convenções a seguir:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="4fbc4-185">Os valores são cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-185">Values are strings.</span></span>
* <span data-ttu-id="4fbc4-186">Não é possível armazenar valores nulos na configuração ou associá-los a objetos.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="4fbc4-187">Provedores</span><span class="sxs-lookup"><span data-stu-id="4fbc4-187">Providers</span></span>

<span data-ttu-id="4fbc4-188">A tabela a seguir mostra os provedores de configuração disponíveis para aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="4fbc4-189">Provider</span><span class="sxs-lookup"><span data-stu-id="4fbc4-189">Provider</span></span> | <span data-ttu-id="4fbc4-190">Fornece a configuração de &hellip;</span><span class="sxs-lookup"><span data-stu-id="4fbc4-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="4fbc4-191">[Provedor de Configuração do Azure Key Vault](xref:security/key-vault-configuration) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="4fbc4-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4fbc4-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="4fbc4-193">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4fbc4-194">Parâmetros de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-194">Command-line parameters</span></span> |
| [<span data-ttu-id="4fbc4-195">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="4fbc4-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4fbc4-196">Fonte personalizada</span><span class="sxs-lookup"><span data-stu-id="4fbc4-196">Custom source</span></span> |
| [<span data-ttu-id="4fbc4-197">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4fbc4-198">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-198">Environment variables</span></span> |
| [<span data-ttu-id="4fbc4-199">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4fbc4-200">Arquivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4fbc4-201">Provedor de Configuração de Chave por Arquivo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="4fbc4-202">Arquivos de diretório</span><span class="sxs-lookup"><span data-stu-id="4fbc4-202">Directory files</span></span> |
| [<span data-ttu-id="4fbc4-203">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4fbc4-204">Coleções na memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-204">In-memory collections</span></span> |
| <span data-ttu-id="4fbc4-205">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4fbc4-206">Arquivo no diretório de perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="4fbc4-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="4fbc4-207">Provider</span><span class="sxs-lookup"><span data-stu-id="4fbc4-207">Provider</span></span> | <span data-ttu-id="4fbc4-208">Fornece a configuração de &hellip;</span><span class="sxs-lookup"><span data-stu-id="4fbc4-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="4fbc4-209">[Provedor de Configuração do Azure Key Vault](xref:security/key-vault-configuration) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="4fbc4-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4fbc4-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="4fbc4-211">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4fbc4-212">Parâmetros de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-212">Command-line parameters</span></span> |
| [<span data-ttu-id="4fbc4-213">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="4fbc4-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4fbc4-214">Fonte personalizada</span><span class="sxs-lookup"><span data-stu-id="4fbc4-214">Custom source</span></span> |
| [<span data-ttu-id="4fbc4-215">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4fbc4-216">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-216">Environment variables</span></span> |
| [<span data-ttu-id="4fbc4-217">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4fbc4-218">Arquivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4fbc4-219">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4fbc4-220">Coleções na memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-220">In-memory collections</span></span> |
| <span data-ttu-id="4fbc4-221">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4fbc4-222">Arquivo no diretório de perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="4fbc4-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="4fbc4-223">Provider</span><span class="sxs-lookup"><span data-stu-id="4fbc4-223">Provider</span></span> | <span data-ttu-id="4fbc4-224">Fornece a configuração de &hellip;</span><span class="sxs-lookup"><span data-stu-id="4fbc4-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="4fbc4-225">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4fbc4-226">Parâmetros de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-226">Command-line parameters</span></span> |
| [<span data-ttu-id="4fbc4-227">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="4fbc4-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4fbc4-228">Fonte personalizada</span><span class="sxs-lookup"><span data-stu-id="4fbc4-228">Custom source</span></span> |
| [<span data-ttu-id="4fbc4-229">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4fbc4-230">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-230">Environment variables</span></span> |
| [<span data-ttu-id="4fbc4-231">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4fbc4-232">Arquivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4fbc4-233">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4fbc4-234">Coleções na memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-234">In-memory collections</span></span> |
| <span data-ttu-id="4fbc4-235">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4fbc4-236">Arquivo no diretório de perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="4fbc4-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="4fbc4-237">Na inicialização, as fontes de configuração são lidas na ordem especificada pelos provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="4fbc4-238">Os provedores de configuração descritos neste tópico estão descritos em ordem alfabética, não na ordem na qual seu código pode organizá-los.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="4fbc4-239">Organize os provedores de configuração em seu código para atender às suas prioridades para as fontes de configuração subjacentes.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="4fbc4-240">Uma sequência comum de provedores de configuração é:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="4fbc4-241">Arquivos (*appsettings.json*, *appsettings.{Environment}.json*, em que `{Environment}` é o ambiente de hospedagem atual do aplicativo)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="4fbc4-242">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4fbc4-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="4fbc4-243">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (apenas no ambiente de desenvolvimento)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="4fbc4-244">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-244">Environment variables</span></span>
1. <span data-ttu-id="4fbc4-245">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-245">Command-line arguments</span></span>

<span data-ttu-id="4fbc4-246">É comum posicionar o Provedor de Configuração de Linha de Comando por último em uma série de provedores, a fim de permitir que os argumentos de linha de comando substituam a configuração definida por outros provedores.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-247">Essa sequência de provedores é aplicada quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4fbc4-248">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-249">Essa sequência de provedores pode ser criada para o aplicativo (não o host) com um <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> e uma chamada para seu método <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="4fbc4-250">No exemplo anterior, o nome do ambiente (`env.EnvironmentName`) e o nome do assembly de aplicativo (`env.ApplicationName`) são fornecidos pelo <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="4fbc4-251">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="4fbc4-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="4fbc4-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="4fbc4-253">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o Host da Web para especificar os provedores de configuração do aplicativo, além daqueles adicionados automaticamente por <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="4fbc4-254">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="4fbc4-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="4fbc4-255">O <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carrega a configuração dos pares chave-valor do argumento de linha de comando em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="4fbc4-256">Para ativar a configuração de linha de comando, o método de extensão <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> é chamado em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-257">`AddCommandLine` é chamado automaticamente quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4fbc4-258">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4fbc4-259">`CreateDefaultBuilder` também carrega:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4fbc4-260">Configuração opcional de *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4fbc4-261">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (no ambiente de desenvolvimento).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4fbc4-262">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-262">Environment variables.</span></span>

<span data-ttu-id="4fbc4-263">`CreateDefaultBuilder` adiciona o Provedor de Configuração de Linha de Comando por último.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="4fbc4-264">Os argumentos de linha de comando passados em tempo de execução substituem a configuração definida por outros provedores.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="4fbc4-265">`CreateDefaultBuilder` age quando o host é construído.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="4fbc4-266">Portanto, a configuração de linha de comando ativada por `CreateDefaultBuilder` pode afetar como o host é configurado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-267">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4fbc4-268">`AddCommandLine` já foi chamado por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4fbc4-269">Se precisar oferecer configuração de aplicativo e ainda poder substituir essa configuração por argumentos de linha de comando, chame os provedores adicionais do aplicativo em <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chame `AddCommandLine` por fim.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-270">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-271">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="4fbc4-272">`AddCommandLine` já foi chamado por `CreateDefaultBuilder` quando `UseConfiguration` é chamado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="4fbc4-273">Se precisar oferecer configuração de aplicativo e ainda poder substituir essa configuração por argumentos de linha de comando, chame os provedores adicionais do aplicativo em um `ConfigurationBuilder` e chame `AddCommandLine` por fim.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-274">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-275">Para ativar a configuração de linha de comando, chame o método de extensão <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-276">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="4fbc4-277">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4fbc4-278">**Exemplo**</span><span class="sxs-lookup"><span data-stu-id="4fbc4-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-279">O aplicativo de exemplo 2.x aproveita a vantagem do método de conveniência estático `CreateDefaultBuilder` para criar o host, que inclui uma chamada para <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-280">O aplicativo de exemplo 1.x chama <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> em um <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="4fbc4-281">Abra um prompt de comando no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="4fbc4-282">Forneça um argumento de linha de comando para o comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="4fbc4-283">Quando o aplicativo estiver em execução, abra um navegador para o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4fbc4-284">Observe que a saída contém o par chave-valor do argumento de linha de comando de configuração fornecido para `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="4fbc4-285">Arguments</span><span class="sxs-lookup"><span data-stu-id="4fbc4-285">Arguments</span></span>

<span data-ttu-id="4fbc4-286">O valor deve vir após um sinal de igual (`=`), ou a chave deve ter um prefixo (`--` ou `/`) quando o valor vier após um espaço.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="4fbc4-287">O valor pode ser nulo se um sinal de igual for usado (por exemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="4fbc4-288">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-288">Key prefix</span></span>               | <span data-ttu-id="4fbc4-289">Exemplo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="4fbc4-290">Nenhum prefixo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="4fbc4-291">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="4fbc4-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="4fbc4-293">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="4fbc4-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="4fbc4-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="4fbc4-295">No mesmo comando, não combine pares chave-valor do argumento de linha de comando que usam um sinal de igual com pares chave-valor que usam um espaço.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="4fbc4-296">Exemplo de comandos:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="4fbc4-297">Mapeamentos de comutador</span><span class="sxs-lookup"><span data-stu-id="4fbc4-297">Switch mappings</span></span>

<span data-ttu-id="4fbc4-298">Os mapeamentos de comutador permitem fornecer a lógica de substituição do nome da chave.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="4fbc4-299">Quando você cria manualmente a configuração com um <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, pode fornecer um dicionário de substituições de opções para o método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="4fbc4-300">Ao ser usado, o dicionário de mapeamentos de comutador é verificado para oferecer uma chave que corresponda à chave fornecida por um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="4fbc4-301">Se a chave de linha de comando for encontrada no dicionário, o valor do dicionário (a substituição da chave) será passado de volta para definir o par chave-valor na configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="4fbc4-302">Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixada com um traço único (`-`).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="4fbc4-303">Regras de chave do dicionário de mapeamentos de comutador:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="4fbc4-304">Os comutadores devem começar com um traço (`-`) ou traço duplo (`--`).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="4fbc4-305">O dicionário de mapeamentos de comutador chave não deve conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-306">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-307">Conforme mostrado no exemplo anterior, a chamada para `CreateDefaultBuilder` não deve passar argumentos ao usar mapeamentos de opção.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="4fbc4-308">A chamada `AddCommandLine` do método `CreateDefaultBuilder` não inclui opções mapeadas, e não é possível passar o dicionário de mapeamento de opções para `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4fbc4-309">Se os argumentos incluírem uma opção mapeada e forem passados para `CreateDefaultBuilder`, o provedor `AddCommandLine` não inicializará com um <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="4fbc4-310">A solução não é passar os argumentos para `CreateDefaultBuilder`, mas, em vez disso, permitir que o método `AddCommandLine` do método `ConfigurationBuilder` processe os dois argumentos e o dicionário de mapeamento de opções.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-311">Conforme mostrado no exemplo anterior, a chamada para `CreateDefaultBuilder` não deve passar argumentos ao usar mapeamentos de opção.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="4fbc4-312">A chamada `AddCommandLine` do método `CreateDefaultBuilder` não inclui opções mapeadas, e não é possível passar o dicionário de mapeamento de opções para `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4fbc4-313">Se os argumentos incluírem uma opção mapeada e forem passados para `CreateDefaultBuilder`, o provedor `AddCommandLine` não inicializará com um <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="4fbc4-314">A solução não é passar os argumentos para `CreateDefaultBuilder`, mas, em vez disso, permitir que o método `AddCommandLine` do método `ConfigurationBuilder` processe os dois argumentos e o dicionário de mapeamento de opções.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="4fbc4-315">Depois que o dicionário de mapeamentos de comutador for criado, ele conterá os dados mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="4fbc4-316">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-316">Key</span></span>       | <span data-ttu-id="4fbc4-317">Valor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="4fbc4-318">Se as chaves mapeadas para opção forem usadas ao iniciar o aplicativo, a configuração receberá o valor de configuração na chave fornecida pelo dicionário:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="4fbc4-319">Após a execução do comando anterior, a configuração conterá os valores mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="4fbc4-320">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-320">Key</span></span>               | <span data-ttu-id="4fbc4-321">Valor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="4fbc4-322">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="4fbc4-323">O <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carrega a configuração dos pares chave-valor da variável de ambiente em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="4fbc4-324">Para ativar a configuração das variáveis de ambiente, chame o método de extensão <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-325">Ao trabalhar com chaves hierárquicas nas variáveis de ambiente, talvez um separador de dois-pontos (`:`) não funcione em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="4fbc4-326">Um sublinhado duplo (`__`) é compatível com todas as plataformas e é substituído por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="4fbc4-327">O [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) permite que você defina variáveis de ambiente no portal do Azure que podem substituir a configuração do aplicativo usando o Provedor de Configuração de Variáveis de Ambiente.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="4fbc4-328">Para saber mais, confira [Aplicativos do Azure: substituir a configuração do aplicativo usando o portal do Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-329">`AddEnvironmentVariables` é chamado automaticamente quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-329">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4fbc4-330">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4fbc4-331">`CreateDefaultBuilder` também carrega:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4fbc4-332">Configuração opcional de *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-332">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4fbc4-333">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (no ambiente de desenvolvimento).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-333">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4fbc4-334">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-334">Command-line arguments.</span></span>

<span data-ttu-id="4fbc4-335">O Provedor de Configuração de Variável de Ambiente é chamado após o estabelecimento da configuração a partir dos segredos do usuário e dos arquivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-335">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="4fbc4-336">Chamar o provedor nessa posição permite que as variáveis de ambiente sejam lidas em tempo de execução para substituir a configuração definida por segredos do usuário e arquivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-336">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-337">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-337">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4fbc4-338">`AddEnvironmentVariables` para variáveis de ambiente com o prefixo `ASPNETCORE_` já foi chamado por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-338">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4fbc4-339">Se precisar fornecer configuração do aplicativo com base em variáveis de ambiente adicionais, chame os provedores adicionais do aplicativo no <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>e chame `AddEnvironmentVariables` com o prefixo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_")
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-340">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-341">Chame o método de extensão `AddEnvironmentVariables` em uma instância de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="4fbc4-342">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="4fbc4-343">`AddEnvironmentVariables` para variáveis de ambiente com o prefixo `ASPNETCORE_` já foi chamado por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-343">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4fbc4-344">Se precisar fornecer configuração do aplicativo com base em variáveis de ambiente adicionais, chame os provedores adicionais do aplicativo no <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>e chame `AddEnvironmentVariables` com o prefixo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-344">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-345">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-345">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-346">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-346">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4fbc4-347">**Exemplo**</span><span class="sxs-lookup"><span data-stu-id="4fbc4-347">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-348">O aplicativo de exemplo 2.x aproveita a vantagem do método de conveniência estático `CreateDefaultBuilder` para criar o host, que inclui uma chamada para `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-348">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-349">O aplicativo de exemplo 1.x chama `AddEnvironmentVariables` em um `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-349">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="4fbc4-350">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-350">Run the sample app.</span></span> <span data-ttu-id="4fbc4-351">Abra um navegador para o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-351">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4fbc4-352">Observe que a saída contém o par chave-valor da variável de ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-352">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="4fbc4-353">O valor reflete o ambiente no qual o aplicativo está em execução, normalmente `Development` ao executar localmente.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-353">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="4fbc4-354">Para encurtar a lista de variáveis de ambiente renderizadas pelo aplicativo, o aplicativo filtra as variáveis de ambiente para mostrar as que começam com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-354">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="4fbc4-355">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="4fbc4-355">ASPNETCORE_</span></span>
* <span data-ttu-id="4fbc4-356">urls</span><span class="sxs-lookup"><span data-stu-id="4fbc4-356">urls</span></span>
* <span data-ttu-id="4fbc4-357">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="4fbc4-357">Logging</span></span>
* <span data-ttu-id="4fbc4-358">AMBIENTE</span><span class="sxs-lookup"><span data-stu-id="4fbc4-358">ENVIRONMENT</span></span>
* <span data-ttu-id="4fbc4-359">contentRoot</span><span class="sxs-lookup"><span data-stu-id="4fbc4-359">contentRoot</span></span>
* <span data-ttu-id="4fbc4-360">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4fbc4-360">AllowedHosts</span></span>
* <span data-ttu-id="4fbc4-361">applicationName</span><span class="sxs-lookup"><span data-stu-id="4fbc4-361">applicationName</span></span>
* <span data-ttu-id="4fbc4-362">CommandLine</span><span class="sxs-lookup"><span data-stu-id="4fbc4-362">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-363">Se você quiser expor todas as variáveis de ambiente disponíveis para o aplicativo, altere o `FilteredConfiguration` em *Pages/Index.cshtml.cs* para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-364">Se você quiser expor todas as variáveis de ambiente disponíveis para o aplicativo, altere o `FilteredConfiguration` em *Controllers/HomeController.cs* para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-364">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="4fbc4-365">Prefixos</span><span class="sxs-lookup"><span data-stu-id="4fbc4-365">Prefixes</span></span>

<span data-ttu-id="4fbc4-366">Variáveis de ambiente carregadas na configuração do aplicativo são filtradas quando você fornece um prefixo para o método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-366">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="4fbc4-367">Por exemplo, para filtrar as variáveis de ambiente no prefixo `CUSTOM_`, forneça o prefixo para o provedor de configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-367">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="4fbc4-368">O prefixo é removido na criação dos pares chave-valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-368">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-369">O método de conveniência estático `CreateDefaultBuilder` cria um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> para estabelecer o host do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-369">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="4fbc4-370">Quando `WebHostBuilder` é criado, ele encontra a configuração de seu host nas variáveis de ambiente prefixadas com `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-370">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="4fbc4-371">**Prefixos de cadeia de conexão**</span><span class="sxs-lookup"><span data-stu-id="4fbc4-371">**Connection string prefixes**</span></span>

<span data-ttu-id="4fbc4-372">A API de configuração tem regras de processamento especiais para quatro variáveis de ambiente de cadeia de conexão envolvidas na configuração de cadeias de conexão do Azure para o ambiente de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-372">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="4fbc4-373">As variáveis de ambiente com os prefixos mostrados na tabela são carregadas no aplicativo se nenhum prefixo for fornecido para `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-373">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="4fbc4-374">Prefixo da cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="4fbc4-374">Connection string prefix</span></span> | <span data-ttu-id="4fbc4-375">Provider</span><span class="sxs-lookup"><span data-stu-id="4fbc4-375">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="4fbc4-376">Provedor personalizado</span><span class="sxs-lookup"><span data-stu-id="4fbc4-376">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="4fbc4-377">MySQL</span><span class="sxs-lookup"><span data-stu-id="4fbc4-377">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="4fbc4-378">Banco de Dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="4fbc4-378">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="4fbc4-379">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4fbc4-379">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="4fbc4-380">Quando uma variável de ambiente for descoberta e carregada na configuração com qualquer um dos quatro prefixos mostrados na tabela:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-380">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="4fbc4-381">A chave de configuração é criada removendo o prefixo da variável de ambiente e adicionando uma seção de chave de configuração (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-381">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="4fbc4-382">Um novo par chave-valor de configuração é criado para representar o provedor de conexão de banco de dados (exceto para `CUSTOMCONNSTR_`, que não tem um provedor indicado).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-382">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="4fbc4-383">Chave de variável de ambiente</span><span class="sxs-lookup"><span data-stu-id="4fbc4-383">Environment variable key</span></span> | <span data-ttu-id="4fbc4-384">Chave de configuração convertida</span><span class="sxs-lookup"><span data-stu-id="4fbc4-384">Converted configuration key</span></span> | <span data-ttu-id="4fbc4-385">Entrada de configuração do provedor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-385">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4fbc4-386">Entrada de configuração não criada.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-386">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4fbc4-387">Chave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-387">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4fbc4-388">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-388">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4fbc4-389">Chave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-389">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4fbc4-390">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-390">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4fbc4-391">Chave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-391">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4fbc4-392">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-392">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="4fbc4-393">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-393">File Configuration Provider</span></span>

<span data-ttu-id="4fbc4-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> é a classe base para carregamento da configuração do sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="4fbc4-395">Os provedores de configuração a seguir são dedicados a tipos específicos de arquivos:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-395">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="4fbc4-396">Provedor de Configuração INI</span><span class="sxs-lookup"><span data-stu-id="4fbc4-396">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="4fbc4-397">Provedor de Configuração JSON</span><span class="sxs-lookup"><span data-stu-id="4fbc4-397">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="4fbc4-398">Provedor de Configuração XML</span><span class="sxs-lookup"><span data-stu-id="4fbc4-398">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="4fbc4-399">Provedor de Configuração INI</span><span class="sxs-lookup"><span data-stu-id="4fbc4-399">INI Configuration Provider</span></span>

<span data-ttu-id="4fbc4-400">O <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carrega a configuração de pares chave-valor do arquivo INI em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-400">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="4fbc4-401">Para ativar a configuração de arquivo INI, chame o método de extensão <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-401">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-402">Os dois-pontos podem ser usados como um delimitador de seção na configuração do arquivo INI.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-402">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="4fbc4-403">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-403">Overloads permit specifying:</span></span>

* <span data-ttu-id="4fbc4-404">Se o arquivo é opcional.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-404">Whether the file is optional.</span></span>
* <span data-ttu-id="4fbc4-405">Se a configuração será recarregada caso o arquivo seja alterado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-405">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4fbc4-406">O <xref:Microsoft.Extensions.FileProviders.IFileProvider> usado para acessar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-406">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-407">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-407">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-408">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-408">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-409">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-409">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-410">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-410">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-411">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-411">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4fbc4-412">Um exemplo genérico de um arquivo de configuração INI:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-412">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="4fbc4-413">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-413">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4fbc4-414">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-414">section0:key0</span></span>
* <span data-ttu-id="4fbc4-415">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-415">section0:key1</span></span>
* <span data-ttu-id="4fbc4-416">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="4fbc4-416">section1:subsection:key</span></span>
* <span data-ttu-id="4fbc4-417">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="4fbc4-417">section2:subsection0:key</span></span>
* <span data-ttu-id="4fbc4-418">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="4fbc4-418">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="4fbc4-419">Provedor de Configuração JSON</span><span class="sxs-lookup"><span data-stu-id="4fbc4-419">JSON Configuration Provider</span></span>

<span data-ttu-id="4fbc4-420">O <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carrega a configuração de pares chave-valor do arquivo JSON em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-420">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="4fbc4-421">Para ativar a configuração de arquivo JSON, chame o método de extensão <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-421">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-422">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-422">Overloads permit specifying:</span></span>

* <span data-ttu-id="4fbc4-423">Se o arquivo é opcional.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-423">Whether the file is optional.</span></span>
* <span data-ttu-id="4fbc4-424">Se a configuração será recarregada caso o arquivo seja alterado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-424">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4fbc4-425">O <xref:Microsoft.Extensions.FileProviders.IFileProvider> usado para acessar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-425">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-426">`AddJsonFile` é chamado automaticamente duas vezes quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-426">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4fbc4-427">O método é chamado para carregar a configuração de:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-427">The method is called to load configuration from:</span></span>

* <span data-ttu-id="4fbc4-428">*appsettings.json* &ndash; Esse arquivo é lido primeiro.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-428">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="4fbc4-429">A versão do ambiente do arquivo pode substituir os valores fornecidos pelo arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-429">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="4fbc4-430">*appsettings.{Environment}.json* &ndash; A versão de ambiente do arquivo é carregada com base em [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-430">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="4fbc4-431">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-431">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4fbc4-432">`CreateDefaultBuilder` também carrega:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-432">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4fbc4-433">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-433">Environment variables.</span></span>
* <span data-ttu-id="4fbc4-434">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (no ambiente de desenvolvimento).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-434">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4fbc4-435">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-435">Command-line arguments.</span></span>

<span data-ttu-id="4fbc4-436">O Provedor de Configuração JSON é estabelecido primeiro.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-436">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="4fbc4-437">Portanto, os segredos do usuário, as variáveis de ambiente e os argumentos de linha de comando substituem a configuração definida pelos arquivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-437">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-438">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo para arquivos que não sejam *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-438">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-439">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-439">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-440">Você pode chamar diretamente o método de extensão `AddJsonFile` em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-440">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-441">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-441">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-442">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-442">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-443">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-443">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4fbc4-444">**Exemplo**</span><span class="sxs-lookup"><span data-stu-id="4fbc4-444">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-445">O aplicativo de exemplo 2.x aproveita a vantagem do método de conveniência estático `CreateDefaultBuilder` para criar o host, que inclui duas chamadas para `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-445">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="4fbc4-446">A configuração é carregada de *appsettings.json* e de *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-446">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-447">O aplicativo de exemplo 1.x chama `AddJsonFile` duas vezes em um `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-447">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="4fbc4-448">A configuração é carregada de *appsettings.json* e de *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-448">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="4fbc4-449">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-449">Run the sample app.</span></span> <span data-ttu-id="4fbc4-450">Abra um navegador para o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-450">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4fbc4-451">Observe que a saída contém pares chave-valor para a configuração mostrada na tabela de acordo com o ambiente.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-451">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="4fbc4-452">Chaves de configuração de registro usam os dois-pontos (`:`) como um separador hierárquico.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-452">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="4fbc4-453">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-453">Key</span></span>                        | <span data-ttu-id="4fbc4-454">Valor de Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="4fbc4-454">Development Value</span></span> | <span data-ttu-id="4fbc4-455">Valor de Produção</span><span class="sxs-lookup"><span data-stu-id="4fbc4-455">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="4fbc4-456">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="4fbc4-456">Logging:LogLevel:System</span></span>    | <span data-ttu-id="4fbc4-457">Informações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-457">Information</span></span>       | <span data-ttu-id="4fbc4-458">Informações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-458">Information</span></span>      |
| <span data-ttu-id="4fbc4-459">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="4fbc4-459">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="4fbc4-460">Informações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-460">Information</span></span>       | <span data-ttu-id="4fbc4-461">Informações</span><span class="sxs-lookup"><span data-stu-id="4fbc4-461">Information</span></span>      |
| <span data-ttu-id="4fbc4-462">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="4fbc4-462">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="4fbc4-463">Depurar</span><span class="sxs-lookup"><span data-stu-id="4fbc4-463">Debug</span></span>             | <span data-ttu-id="4fbc4-464">Erro</span><span class="sxs-lookup"><span data-stu-id="4fbc4-464">Error</span></span>            |
| <span data-ttu-id="4fbc4-465">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4fbc4-465">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="4fbc4-466">Provedor de Configuração XML</span><span class="sxs-lookup"><span data-stu-id="4fbc4-466">XML Configuration Provider</span></span>

<span data-ttu-id="4fbc4-467">O <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carrega a configuração de pares chave-valor do arquivo XML em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-467">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="4fbc4-468">Para ativar a configuração de arquivo XML, chame o método de extensão <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-468">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-469">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-469">Overloads permit specifying:</span></span>

* <span data-ttu-id="4fbc4-470">Se o arquivo é opcional.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-470">Whether the file is optional.</span></span>
* <span data-ttu-id="4fbc4-471">Se a configuração será recarregada caso o arquivo seja alterado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-471">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4fbc4-472">O <xref:Microsoft.Extensions.FileProviders.IFileProvider> usado para acessar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-472">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4fbc4-473">O nó raiz do arquivo de configuração é ignorado quando os pares chave-valor da configuração são criados.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-473">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="4fbc4-474">Não especifique um DTD (definição de tipo de documento) ou um namespace no arquivo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-474">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-475">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-475">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-476">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-476">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-477">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-477">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-478">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-478">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-479">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-479">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4fbc4-480">Os arquivos de configuração XML podem usar nomes de elemento distintos para seções repetidas:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-480">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="4fbc4-481">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-481">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4fbc4-482">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-482">section0:key0</span></span>
* <span data-ttu-id="4fbc4-483">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-483">section0:key1</span></span>
* <span data-ttu-id="4fbc4-484">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-484">section1:key0</span></span>
* <span data-ttu-id="4fbc4-485">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-485">section1:key1</span></span>

<span data-ttu-id="4fbc4-486">Elementos repetidos que usam o mesmo nome de elemento funcionarão se o atributo `name` for usado para diferenciar os elementos:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-486">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="4fbc4-487">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-487">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4fbc4-488">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-488">section:section0:key:key0</span></span>
* <span data-ttu-id="4fbc4-489">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-489">section:section0:key:key1</span></span>
* <span data-ttu-id="4fbc4-490">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-490">section:section1:key:key0</span></span>
* <span data-ttu-id="4fbc4-491">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-491">section:section1:key:key1</span></span>

<span data-ttu-id="4fbc4-492">É possível usar atributos para fornecer valores:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-492">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="4fbc4-493">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-493">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4fbc4-494">key:attribute</span><span class="sxs-lookup"><span data-stu-id="4fbc4-494">key:attribute</span></span>
* <span data-ttu-id="4fbc4-495">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="4fbc4-495">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="4fbc4-496">Provedor de Configuração de Chave por Arquivo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-496">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="4fbc4-497">O <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa arquivos do diretório como pares chave-valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-497">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="4fbc4-498">A chave é o nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-498">The key is the file name.</span></span> <span data-ttu-id="4fbc4-499">O valor contém o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-499">The value contains the file's contents.</span></span> <span data-ttu-id="4fbc4-500">O Provedor de Configuração de Chave por arquivo é usado em cenários de hospedagem do Docker.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-500">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="4fbc4-501">Para ativar a configuração de chave por arquivo, chame o método de extensão <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-501">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="4fbc4-502">O `directoryPath` para os arquivos deve ser um caminho absoluto.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-502">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="4fbc4-503">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-503">Overloads permit specifying:</span></span>

* <span data-ttu-id="4fbc4-504">Um delegado `Action<KeyPerFileConfigurationSource>` que configura a origem.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-504">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="4fbc4-505">Se o diretório é opcional, e o caminho para o diretório.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-505">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="4fbc4-506">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-506">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddKeyPerFile(directoryPath: path, optional: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-507">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-507">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="4fbc4-508">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="4fbc4-508">Memory Configuration Provider</span></span>

<span data-ttu-id="4fbc4-509">O <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa uma coleção na memória como pares chave-valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-509">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="4fbc4-510">Para ativar a configuração de coleção na memória, chame o método de extensão <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-510">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4fbc4-511">O provedor de configuração pode ser inicializado com um `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-511">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4fbc4-512">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-512">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4fbc4-513">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-513">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4fbc4-514">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-514">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="4fbc4-515">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-515">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-516">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-516">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="4fbc4-517">GetValue</span><span class="sxs-lookup"><span data-stu-id="4fbc4-517">GetValue</span></span>

<span data-ttu-id="4fbc4-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrai um valor de configuração com uma chave especificada e o converte para o tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="4fbc4-519">Uma sobrecarga permite que você forneça um valor padrão se a chave não for encontrada.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-519">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="4fbc4-520">O exemplo a seguir extrai o valor de cadeia de caracteres da configuração com a chave `NumberKey`, digita o valor como um `int` e armazena o valor na variável `intValue`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-520">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="4fbc4-521">Se `NumberKey` não for encontrado em chaves de configuração, `intValue` recebe o valor padrão de `99`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-521">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="4fbc4-522">GetSection, GetChildren e Exists</span><span class="sxs-lookup"><span data-stu-id="4fbc4-522">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="4fbc4-523">Para os exemplos a seguir, considere o seguinte arquivo JSON.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-523">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="4fbc4-524">Há quatro chaves em duas seções, cada uma delas inclui um par de subseções:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-524">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="4fbc4-525">Quando o arquivo é lido na configuração, as seguintes chaves hierárquicas exclusivas são criadas para conter os valores de configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-525">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="4fbc4-526">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-526">section0:key0</span></span>
* <span data-ttu-id="4fbc4-527">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-527">section0:key1</span></span>
* <span data-ttu-id="4fbc4-528">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-528">section1:key0</span></span>
* <span data-ttu-id="4fbc4-529">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-529">section1:key1</span></span>
* <span data-ttu-id="4fbc4-530">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-530">section2:subsection0:key0</span></span>
* <span data-ttu-id="4fbc4-531">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-531">section2:subsection0:key1</span></span>
* <span data-ttu-id="4fbc4-532">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-532">section2:subsection1:key0</span></span>
* <span data-ttu-id="4fbc4-533">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-533">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="4fbc4-534">GetSection</span><span class="sxs-lookup"><span data-stu-id="4fbc4-534">GetSection</span></span>

<span data-ttu-id="4fbc4-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrai uma subseção de configuração com a chave de subseção especificada.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="4fbc4-536">Para retornar um <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contenha apenas os pares chave-valor em `section1`, chame `GetSection` e forneça o nome da seção:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-536">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="4fbc4-537">Da mesma forma, para obter os valores de chaves em `section2:subsection0`, chame `GetSection` e forneça o caminho de seção:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-537">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="4fbc4-538">`GetSection` nunca retorna `null`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-538">`GetSection` never returns `null`.</span></span> <span data-ttu-id="4fbc4-539">Se uma seção correspondente não for encontrada, um `IConfigurationSection` vazio será retornado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-539">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="4fbc4-540">GetChildren</span><span class="sxs-lookup"><span data-stu-id="4fbc4-540">GetChildren</span></span>

<span data-ttu-id="4fbc4-541">Uma chamada para [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) em `section2` obtém um `IEnumerable<IConfigurationSection>` que inclui:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-541">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="4fbc4-542">Exists</span><span class="sxs-lookup"><span data-stu-id="4fbc4-542">Exists</span></span>

<span data-ttu-id="4fbc4-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar se há uma seção de configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="4fbc4-544">Considerando os dados de exemplo, `sectionExists` é `false`, pois não existe uma seção `section2:subsection2` nos dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-544">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="4fbc4-545">Associar a uma classe</span><span class="sxs-lookup"><span data-stu-id="4fbc4-545">Bind to a class</span></span>

<span data-ttu-id="4fbc4-546">A configuração pode ser associada a classes que representam grupos de configurações relacionadas usando o *padrão de opções*.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-546">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="4fbc4-547">Para obter mais informações, consulte <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-547">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4fbc4-548">Os valores de configuração retornam como cadeias de caracteres, mas chamar <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite a construção de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-548">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="4fbc4-549">O aplicativo de exemplo contém um modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="4fbc4-549">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-550">A seção `starship` do arquivo *starship.json* cria a configuração quando o aplicativo de exemplo usa o Provedor de Configuração JSON para carregar a configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-550">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="4fbc4-551">Os seguintes pares chave-valor de configuração são criados:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-551">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="4fbc4-552">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-552">Key</span></span>                   | <span data-ttu-id="4fbc4-553">Valor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-553">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="4fbc4-554">starship:name</span><span class="sxs-lookup"><span data-stu-id="4fbc4-554">starship:name</span></span>         | <span data-ttu-id="4fbc4-555">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="4fbc4-555">USS Enterprise</span></span>                                    |
| <span data-ttu-id="4fbc4-556">starship:registry</span><span class="sxs-lookup"><span data-stu-id="4fbc4-556">starship:registry</span></span>     | <span data-ttu-id="4fbc4-557">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="4fbc4-557">NCC-1701</span></span>                                          |
| <span data-ttu-id="4fbc4-558">starship:class</span><span class="sxs-lookup"><span data-stu-id="4fbc4-558">starship:class</span></span>        | <span data-ttu-id="4fbc4-559">Constituição</span><span class="sxs-lookup"><span data-stu-id="4fbc4-559">Constitution</span></span>                                      |
| <span data-ttu-id="4fbc4-560">starship:length</span><span class="sxs-lookup"><span data-stu-id="4fbc4-560">starship:length</span></span>       | <span data-ttu-id="4fbc4-561">304,8</span><span class="sxs-lookup"><span data-stu-id="4fbc4-561">304.8</span></span>                                             |
| <span data-ttu-id="4fbc4-562">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="4fbc4-562">starship:commissioned</span></span> | <span data-ttu-id="4fbc4-563">False</span><span class="sxs-lookup"><span data-stu-id="4fbc4-563">False</span></span>                                             |
| <span data-ttu-id="4fbc4-564">marca</span><span class="sxs-lookup"><span data-stu-id="4fbc4-564">trademark</span></span>             | <span data-ttu-id="4fbc4-565">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="4fbc4-565">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="4fbc4-566">O aplicativo de exemplo chama `GetSection` com a chave `starship`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-566">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="4fbc4-567">Os pares chave-valor `starship` são isolados.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-567">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="4fbc4-568">O método `Bind` é chamado na subseção passando uma instância da classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-568">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="4fbc4-569">Depois de associar os valores de instância, a instância é atribuída a uma propriedade para renderização:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-569">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="4fbc4-570">Associar a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="4fbc4-570">Bind to an object graph</span></span>

<span data-ttu-id="4fbc4-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> é capaz de associar um grafo de objeto POCO inteiro.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="4fbc4-572">O exemplo contém um modelo `TvShow` cujo grafo do objeto inclui as classes `Metadata` e `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="4fbc4-572">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-573">O aplicativo de exemplo tem um arquivo *tvshow.xml* que contém os dados de configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-573">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="4fbc4-574">A configuração está associada ao método do grafo de objeto `TvShow` inteiro com o método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-574">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="4fbc4-575">A instância associada é atribuída a uma propriedade para renderização:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-575">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="4fbc4-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e retorna o tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="4fbc4-577">O `Get<T>` é mais conveniente do que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-577">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="4fbc4-578">O código a seguir mostra como usar `Get<T>` com o exemplo anterior, que permite que a instância associada seja diretamente atribuída à propriedade usada para renderização:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-578">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="4fbc4-579">Associar uma matriz a uma classe</span><span class="sxs-lookup"><span data-stu-id="4fbc4-579">Bind an array to a class</span></span>

<span data-ttu-id="4fbc4-580">*O aplicativo de exemplo demonstra os conceitos explicados nesta seção.*</span><span class="sxs-lookup"><span data-stu-id="4fbc4-580">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="4fbc4-581">O <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> dá suporte a matrizes de associação para objetos usando os índices em chaves de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-581">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4fbc4-582">Qualquer formato de matriz que exponha um segmento de chave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) é capaz de associar matrizes a uma matriz de classe POCO.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-582">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="4fbc4-583">A associação é fornecida por convenção.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-583">Binding is provided by convention.</span></span> <span data-ttu-id="4fbc4-584">Provedores de configuração personalizados não são necessários para implementar a associação de matriz.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-584">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="4fbc4-585">**Processamento de matriz na memória**</span><span class="sxs-lookup"><span data-stu-id="4fbc4-585">**In-memory array processing**</span></span>

<span data-ttu-id="4fbc4-586">Considere as chaves de configuração e os valores mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-586">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="4fbc4-587">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-587">Key</span></span>     | <span data-ttu-id="4fbc4-588">Valor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-588">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="4fbc4-589">array:0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-589">array:0</span></span> | <span data-ttu-id="4fbc4-590">value0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-590">value0</span></span> |
| <span data-ttu-id="4fbc4-591">array:1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-591">array:1</span></span> | <span data-ttu-id="4fbc4-592">value1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-592">value1</span></span> |
| <span data-ttu-id="4fbc4-593">array:2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-593">array:2</span></span> | <span data-ttu-id="4fbc4-594">value2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-594">value2</span></span> |
| <span data-ttu-id="4fbc4-595">array:4</span><span class="sxs-lookup"><span data-stu-id="4fbc4-595">array:4</span></span> | <span data-ttu-id="4fbc4-596">value4</span><span class="sxs-lookup"><span data-stu-id="4fbc4-596">value4</span></span> |
| <span data-ttu-id="4fbc4-597">array:5</span><span class="sxs-lookup"><span data-stu-id="4fbc4-597">array:5</span></span> | <span data-ttu-id="4fbc4-598">value5</span><span class="sxs-lookup"><span data-stu-id="4fbc4-598">value5</span></span> |

<span data-ttu-id="4fbc4-599">Essas chaves e valores são carregados no aplicativo de exemplo usando o Provedor de Configuração de Memória:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-599">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="4fbc4-600">A matriz ignora um valor para o índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-600">The array skips a value for index &num;3.</span></span> <span data-ttu-id="4fbc4-601">O associador de configuração não é capaz de associar valores nulos ou criar entradas nulas em objetos associados, o que fica claro em um momento quando o resultado da associação dessa matriz a um objeto é demonstrado.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-601">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="4fbc4-602">No aplicativo de exemplo, uma classe POCO está disponível para armazenar os dados de configuração associados:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-602">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-603">Os dados de configuração estão associados ao objeto:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-603">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="4fbc4-604">A sintaxe [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) também pode ser usada, o que resulta em um código mais compacto:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="4fbc4-605">O objeto associado, uma instância de `ArrayExample`, recebe os dados da matriz de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-605">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="4fbc4-606">Índice `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-606">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="4fbc4-607">Valor `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-607">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="4fbc4-608">0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-608">0</span></span>                             | <span data-ttu-id="4fbc4-609">value0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-609">value0</span></span>                        |
| <span data-ttu-id="4fbc4-610">1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-610">1</span></span>                             | <span data-ttu-id="4fbc4-611">value1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-611">value1</span></span>                        |
| <span data-ttu-id="4fbc4-612">2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-612">2</span></span>                             | <span data-ttu-id="4fbc4-613">value2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-613">value2</span></span>                        |
| <span data-ttu-id="4fbc4-614">3</span><span class="sxs-lookup"><span data-stu-id="4fbc4-614">3</span></span>                             | <span data-ttu-id="4fbc4-615">value4</span><span class="sxs-lookup"><span data-stu-id="4fbc4-615">value4</span></span>                        |
| <span data-ttu-id="4fbc4-616">4</span><span class="sxs-lookup"><span data-stu-id="4fbc4-616">4</span></span>                             | <span data-ttu-id="4fbc4-617">value5</span><span class="sxs-lookup"><span data-stu-id="4fbc4-617">value5</span></span>                        |

<span data-ttu-id="4fbc4-618">O índice &num;3 no objeto associado contém os dados de configuração para a chave de configuração `array:4` e seu valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-618">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="4fbc4-619">Quando os dados de configuração que contêm uma matriz são associados, os índices da matriz nas chaves de configuração são simplesmente usados para iterar os dados de configuração ao criar o objeto.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-619">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="4fbc4-620">Um valor nulo não pode ser mantido nos dados de configuração, e uma entrada de valor nulo não será criada em um objeto de associação quando uma matriz nas chaves de configuração ignorar um ou mais índices.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-620">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="4fbc4-621">O item de configuração ausente para o índice &num;3 pode ser fornecido antes da associação à instância `ArrayExamples` por qualquer provedor de configuração que produza o par chave-valor correto na configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-621">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="4fbc4-622">Se o exemplo incluir um Provedor de Configuração JSON adicional com o par chave-valor ausente, o `ArrayExamples.Entries` coincidirá com a matriz de configuração completa:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-622">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="4fbc4-623">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-623">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4fbc4-624">No <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4fbc4-625">No construtor `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-625">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="4fbc4-626">O par chave-valor mostrado na tabela é carregado na configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-626">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="4fbc4-627">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-627">Key</span></span>             | <span data-ttu-id="4fbc4-628">Valor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-628">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="4fbc4-629">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="4fbc4-629">array:entries:3</span></span> | <span data-ttu-id="4fbc4-630">value3</span><span class="sxs-lookup"><span data-stu-id="4fbc4-630">value3</span></span> |

<span data-ttu-id="4fbc4-631">Se a instância da classe `ArrayExamples` for associada após o Provedor de Configuração JSON incluir a entrada para o índice &num;3, a matriz `ArrayExamples.Entries` incluirá o valor.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-631">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="4fbc4-632">Índice `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-632">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="4fbc4-633">Valor `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-633">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="4fbc4-634">0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-634">0</span></span>                             | <span data-ttu-id="4fbc4-635">value0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-635">value0</span></span>                        |
| <span data-ttu-id="4fbc4-636">1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-636">1</span></span>                             | <span data-ttu-id="4fbc4-637">value1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-637">value1</span></span>                        |
| <span data-ttu-id="4fbc4-638">2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-638">2</span></span>                             | <span data-ttu-id="4fbc4-639">value2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-639">value2</span></span>                        |
| <span data-ttu-id="4fbc4-640">3</span><span class="sxs-lookup"><span data-stu-id="4fbc4-640">3</span></span>                             | <span data-ttu-id="4fbc4-641">value3</span><span class="sxs-lookup"><span data-stu-id="4fbc4-641">value3</span></span>                        |
| <span data-ttu-id="4fbc4-642">4</span><span class="sxs-lookup"><span data-stu-id="4fbc4-642">4</span></span>                             | <span data-ttu-id="4fbc4-643">value4</span><span class="sxs-lookup"><span data-stu-id="4fbc4-643">value4</span></span>                        |
| <span data-ttu-id="4fbc4-644">5</span><span class="sxs-lookup"><span data-stu-id="4fbc4-644">5</span></span>                             | <span data-ttu-id="4fbc4-645">value5</span><span class="sxs-lookup"><span data-stu-id="4fbc4-645">value5</span></span>                        |

<span data-ttu-id="4fbc4-646">**Processamento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="4fbc4-646">**JSON array processing**</span></span>

<span data-ttu-id="4fbc4-647">Se um arquivo JSON contiver uma matriz, as chaves de configuração serão criadas para os elementos da matriz com um índice de seção de base zero.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-647">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="4fbc4-648">No arquivo de configuração a seguir, `subsection` é uma matriz:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-648">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="4fbc4-649">O Provedor de Configuração JSON lê os dados de configuração para os seguintes pares chave-valor:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-649">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="4fbc4-650">Chave</span><span class="sxs-lookup"><span data-stu-id="4fbc4-650">Key</span></span>                     | <span data-ttu-id="4fbc4-651">Valor</span><span class="sxs-lookup"><span data-stu-id="4fbc4-651">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="4fbc4-652">json_array:key</span><span class="sxs-lookup"><span data-stu-id="4fbc4-652">json_array:key</span></span>          | <span data-ttu-id="4fbc4-653">valueA</span><span class="sxs-lookup"><span data-stu-id="4fbc4-653">valueA</span></span> |
| <span data-ttu-id="4fbc4-654">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-654">json_array:subsection:0</span></span> | <span data-ttu-id="4fbc4-655">valueB</span><span class="sxs-lookup"><span data-stu-id="4fbc4-655">valueB</span></span> |
| <span data-ttu-id="4fbc4-656">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-656">json_array:subsection:1</span></span> | <span data-ttu-id="4fbc4-657">valueC</span><span class="sxs-lookup"><span data-stu-id="4fbc4-657">valueC</span></span> |
| <span data-ttu-id="4fbc4-658">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-658">json_array:subsection:2</span></span> | <span data-ttu-id="4fbc4-659">valueD</span><span class="sxs-lookup"><span data-stu-id="4fbc4-659">valueD</span></span> |

<span data-ttu-id="4fbc4-660">No aplicativo de exemplo, a seguinte classe POCO está disponível para associar os pares chave-valor de configuração:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-660">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-661">Após a associação, `JsonArrayExample.Key` contém o valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-661">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="4fbc4-662">Os valores de subseção são armazenados na propriedade matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-662">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="4fbc4-663">Índice `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-663">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="4fbc4-664">Valor `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="4fbc4-664">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="4fbc4-665">0</span><span class="sxs-lookup"><span data-stu-id="4fbc4-665">0</span></span>                                   | <span data-ttu-id="4fbc4-666">valueB</span><span class="sxs-lookup"><span data-stu-id="4fbc4-666">valueB</span></span>                              |
| <span data-ttu-id="4fbc4-667">1</span><span class="sxs-lookup"><span data-stu-id="4fbc4-667">1</span></span>                                   | <span data-ttu-id="4fbc4-668">valueC</span><span class="sxs-lookup"><span data-stu-id="4fbc4-668">valueC</span></span>                              |
| <span data-ttu-id="4fbc4-669">2</span><span class="sxs-lookup"><span data-stu-id="4fbc4-669">2</span></span>                                   | <span data-ttu-id="4fbc4-670">valueD</span><span class="sxs-lookup"><span data-stu-id="4fbc4-670">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="4fbc4-671">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="4fbc4-671">Custom configuration provider</span></span>

<span data-ttu-id="4fbc4-672">O aplicativo de exemplo demonstra como criar um provedor de configuração básico que lê os pares chave-valor da configuração de um banco de dados usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-672">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="4fbc4-673">O provedor tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-673">The provider has the following characteristics:</span></span>

* <span data-ttu-id="4fbc4-674">O banco de dados EF na memória é usado para fins de demonstração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-674">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="4fbc4-675">Para usar um banco de dados que exija uma cadeia de conexão, implemente um `ConfigurationBuilder` secundário para fornecer a cadeia de conexão de outro provedor de configuração.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-675">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="4fbc4-676">O provedor lê uma tabela de banco de dados na configuração na inicialização.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-676">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="4fbc4-677">O provedor não consulta o banco de dados em uma base por chave.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-677">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="4fbc4-678">O recarregamento na alteração não está implementado, portanto, a atualização do banco de dados após a inicialização do aplicativo não tem efeito sobre a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-678">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="4fbc4-679">Defina uma entidade `EFConfigurationValue` para armazenar valores de configuração no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-679">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="4fbc4-680">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-680">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-681">Adicione um `EFConfigurationContext` para armazenar e acessar os valores configurados.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-681">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="4fbc4-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-683">Crie uma classe que implementa <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-683">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="4fbc4-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-685">Crie o provedor de configuração personalizado através da herança de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-685">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="4fbc4-686">O provedor de configuração inicializa o banco de dados quando ele está vazio.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-686">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="4fbc4-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-688">Um método de extensão `AddEFConfiguration` permite adicionar a fonte de configuração a um `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-688">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="4fbc4-689">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-689">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4fbc4-690">O código a seguir mostra como usar o `EFConfigurationProvider` personalizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-690">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="4fbc4-691">Acessar a configuração durante a inicialização</span><span class="sxs-lookup"><span data-stu-id="4fbc4-691">Access configuration during startup</span></span>

<span data-ttu-id="4fbc4-692">Injete `IConfiguration` no construtor `Startup` para acessar os valores de configuração em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-692">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4fbc4-693">Para acessar a configuração em `Startup.Configure`, injete `IConfiguration` diretamente no método ou use a instância do construtor:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-693">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="4fbc4-694">Para obter um exemplo de como acessar a configuração usando os métodos de conveniência de inicialização, consulte [Inicialização do aplicativo: métodos de conveniência](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="4fbc4-694">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="4fbc4-695">Acessar a configuração em uma página do Razor Pages ou exibição do MVC</span><span class="sxs-lookup"><span data-stu-id="4fbc4-695">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="4fbc4-696">Para acessar definições de configuração em uma página do Razor Pages ou uma exibição do MVC, adicione [usando diretiva](xref:mvc/views/razor#using) ([referência de C#: usando diretiva](/dotnet/csharp/language-reference/keywords/using-directive)) para o [namespace Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e injete <xref:Microsoft.Extensions.Configuration.IConfiguration> na página ou na exibição.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-696">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="4fbc4-697">Em uma página do Razor:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-697">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="4fbc4-698">Em uma exibição do MVC:</span><span class="sxs-lookup"><span data-stu-id="4fbc4-698">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="4fbc4-699">Adicionar configuração de um assembly externo</span><span class="sxs-lookup"><span data-stu-id="4fbc4-699">Add configuration from an external assembly</span></span>

<span data-ttu-id="4fbc4-700">Uma implementação <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite adicionar melhorias a um aplicativo durante a inicialização de um assembly externo fora da classe `Startup` do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-700">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="4fbc4-701">Para obter mais informações, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4fbc4-701">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fbc4-702">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4fbc4-702">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="4fbc4-703">Detalhes da configuração da Microsoft</span><span class="sxs-lookup"><span data-stu-id="4fbc4-703">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
