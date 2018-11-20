---
title: Configuração no ASP.NET Core
author: guardrex
description: Saiba como usar a API de configuração para configurar um aplicativo do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/15/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 766ac77a2af01509f8e4bc646a18f7dfbc923511
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818389"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="1ee03-103">Configuração no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ee03-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="1ee03-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1ee03-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1ee03-105">A configuração de aplicativos no ASP.NET Core se baseia em pares chave-valor estabelecidos por *provedores de configuração*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="1ee03-106">Os provedores de configuração leem os dados de configuração em pares chave-valor de várias fontes de configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="1ee03-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ee03-107">Azure Key Vault</span></span>
* <span data-ttu-id="1ee03-108">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-108">Command-line arguments</span></span>
* <span data-ttu-id="1ee03-109">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="1ee03-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1ee03-110">Arquivos de diretório</span><span class="sxs-lookup"><span data-stu-id="1ee03-110">Directory files</span></span>
* <span data-ttu-id="1ee03-111">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-111">Environment variables</span></span>
* <span data-ttu-id="1ee03-112">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-112">In-memory .NET objects</span></span>
* <span data-ttu-id="1ee03-113">Arquivos de configurações</span><span class="sxs-lookup"><span data-stu-id="1ee03-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="1ee03-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ee03-114">Azure Key Vault</span></span>
* <span data-ttu-id="1ee03-115">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-115">Command-line arguments</span></span>
* <span data-ttu-id="1ee03-116">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="1ee03-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1ee03-117">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-117">Environment variables</span></span>
* <span data-ttu-id="1ee03-118">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-118">In-memory .NET objects</span></span>
* <span data-ttu-id="1ee03-119">Arquivos de configurações</span><span class="sxs-lookup"><span data-stu-id="1ee03-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="1ee03-120">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-120">Command-line arguments</span></span>
* <span data-ttu-id="1ee03-121">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="1ee03-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1ee03-122">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-122">Environment variables</span></span>
* <span data-ttu-id="1ee03-123">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-123">In-memory .NET objects</span></span>
* <span data-ttu-id="1ee03-124">Arquivos de configurações</span><span class="sxs-lookup"><span data-stu-id="1ee03-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="1ee03-125">O *padrão de opções* é uma extensão dos conceitos de configuração descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="1ee03-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="1ee03-126">As opções usam classes para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="1ee03-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="1ee03-127">Para saber mais sobre como usar o padrão de opções, confira <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1ee03-128">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ee03-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1ee03-129">Os exemplos apresentados neste tópico dependem do seguinte:</span><span class="sxs-lookup"><span data-stu-id="1ee03-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="1ee03-130">Definição do caminho de base do aplicativo com <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ee03-131">`SetBasePath` é disponibilizado para um aplicativo fazendo referência ao pacote [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="1ee03-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="1ee03-132">Resolução de seções dos arquivos de configuração com <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="1ee03-133">`GetSection` é disponibilizado a um aplicativo fazendo referência ao pacote [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="1ee03-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="1ee03-134">Configuração de associação a classes .NET com <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> e [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="1ee03-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="1ee03-135">`Bind` e `Get<T>` são disponibilizados a um aplicativo fazendo referência ao pacote [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="1ee03-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="1ee03-136">`Get<T>` está disponível no ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="1ee03-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-137">Esses três pacotes estão incluídos no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ee03-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-138">Esses três pacotes estão incluídos no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="1ee03-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="1ee03-139">Configuração do host versus aplicativo</span><span class="sxs-lookup"><span data-stu-id="1ee03-139">Host vs. app configuration</span></span>

<span data-ttu-id="1ee03-140">Antes do aplicativo ser configurado e iniciado, um *host* é configurado e iniciado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="1ee03-141">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1ee03-142">O aplicativo e o host são configurados usando os provedores de configuração descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="1ee03-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="1ee03-143">Os pares chave-valor de configuração do host se tornam parte da configuração global do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="1ee03-144">Para saber mais sobre como os provedores de configuração são usados quando o host for compilado, e como as fontes de configuração afetam o host e a configuração, confira <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="1ee03-145">Segurança</span><span class="sxs-lookup"><span data-stu-id="1ee03-145">Security</span></span>

<span data-ttu-id="1ee03-146">Adote as melhores práticas a seguir:</span><span class="sxs-lookup"><span data-stu-id="1ee03-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="1ee03-147">Nunca armazene senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="1ee03-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="1ee03-148">Não use segredos de produção em ambientes de teste ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="1ee03-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="1ee03-149">Especifique segredos fora do projeto para que eles não sejam acidentalmente comprometidos com um repositório de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="1ee03-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="1ee03-150">Saiba mais sobre [como usar vários ambientes](xref:fundamentals/environments) e gerenciar o [armazenamento seguro de segredos de aplicativo em desenvolvimento com o Gerenciador de Segredo](xref:security/app-secrets) (inclui recomendações sobre como usar variáveis de ambiente para armazenar dados confidenciais).</span><span class="sxs-lookup"><span data-stu-id="1ee03-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="1ee03-151">O Gerenciador de Segredo usa o Provedor de Configuração de Arquivo para armazenar segredos do usuário em um arquivo JSON no sistema local.</span><span class="sxs-lookup"><span data-stu-id="1ee03-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="1ee03-152">O Provedor de Configuração de Arquivo será descrito mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="1ee03-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="1ee03-153">O [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) é uma opção para o armazenamento seguro de segredos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="1ee03-154">Para obter mais informações, consulte <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="1ee03-155">Dados de configuração hierárquica</span><span class="sxs-lookup"><span data-stu-id="1ee03-155">Hierarchical configuration data</span></span>

<span data-ttu-id="1ee03-156">A API de Configuração é capaz de manter dados de configuração hierárquica nivelando os dados hierárquicos com o uso de um delimitador nas chaves de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="1ee03-157">No seguinte arquivo JSON, há quatro chaves em uma hierarquia estruturada com duas seções:</span><span class="sxs-lookup"><span data-stu-id="1ee03-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="1ee03-158">Quando o arquivo é lido na configuração, ocorre a criação de chaves exclusivas para manter a estrutura hierárquica de dados original da fonte de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="1ee03-159">As seções e as chaves são niveladas usando dois-pontos (`:`) para manter a estrutura original:</span><span class="sxs-lookup"><span data-stu-id="1ee03-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="1ee03-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-160">section0:key0</span></span>
* <span data-ttu-id="1ee03-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-161">section0:key1</span></span>
* <span data-ttu-id="1ee03-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-162">section1:key0</span></span>
* <span data-ttu-id="1ee03-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-163">section1:key1</span></span>

<span data-ttu-id="1ee03-164">Os métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> estão disponíveis para isolar as seções e os filhos de uma seção nos dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="1ee03-165">Esses métodos serão descritos posteriormente em [GetSection, GetChildren e Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="1ee03-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="1ee03-166">Convenções</span><span class="sxs-lookup"><span data-stu-id="1ee03-166">Conventions</span></span>

<span data-ttu-id="1ee03-167">Na inicialização do aplicativo, as fontes de configuração são lidas na ordem especificada pelos provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="1ee03-168">Os Provedores de Configuração de Arquivo têm a capacidade de recarregar a configuração quando um arquivo de configurações subjacente é alterado após a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="1ee03-169">O Provedor de Configuração de Arquivo será descrito mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="1ee03-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="1ee03-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponível no contêiner [DI (injeção de dependência)](xref:fundamentals/dependency-injection) do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1ee03-171">Os provedores de configuração não podem utilizar a DI, pois ela não é disponibilizada quando eles são configurados pelo host.</span><span class="sxs-lookup"><span data-stu-id="1ee03-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="1ee03-172">As chaves de configuração adotam as convenções a seguir:</span><span class="sxs-lookup"><span data-stu-id="1ee03-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="1ee03-173">As chaves não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1ee03-173">Keys are case-insensitive.</span></span> <span data-ttu-id="1ee03-174">Por exemplo, `ConnectionString` e `connectionstring` são tratados como chaves equivalentes.</span><span class="sxs-lookup"><span data-stu-id="1ee03-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="1ee03-175">Se um valor para a mesma chave for definido pelos mesmos provedores de configuração, ou por outros, o último valor definido na chave será o valor usado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="1ee03-176">Chaves hierárquicas</span><span class="sxs-lookup"><span data-stu-id="1ee03-176">Hierarchical keys</span></span>
  * <span data-ttu-id="1ee03-177">Ao interagir com a API de configuração, um separador de dois-pontos (`:`) funciona em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="1ee03-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="1ee03-178">Nas variáveis de ambiente, talvez um separador de dois-pontos não funcione em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="1ee03-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="1ee03-179">Um sublinhado duplo (`__`) é compatível com todas as plataformas e é convertido em dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="1ee03-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="1ee03-180">No Azure Key Vault, as chaves hierárquicas usam `--` (dois traços) como separador.</span><span class="sxs-lookup"><span data-stu-id="1ee03-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="1ee03-181">Você deve fornecer o código para substituir os traços por dois-pontos quando os segredos forem carregados na configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="1ee03-182">O <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> dá suporte a matrizes de associação para objetos usando os índices em chaves de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1ee03-183">A associação de matriz está descrita na seção [Associar uma matriz a uma classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="1ee03-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="1ee03-184">Os valores de configuração adotam as convenções a seguir:</span><span class="sxs-lookup"><span data-stu-id="1ee03-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="1ee03-185">Os valores são cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1ee03-185">Values are strings.</span></span>
* <span data-ttu-id="1ee03-186">Não é possível armazenar valores nulos na configuração ou associá-los a objetos.</span><span class="sxs-lookup"><span data-stu-id="1ee03-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="1ee03-187">Provedores</span><span class="sxs-lookup"><span data-stu-id="1ee03-187">Providers</span></span>

<span data-ttu-id="1ee03-188">A tabela a seguir mostra os provedores de configuração disponíveis para aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ee03-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="1ee03-189">Provider</span><span class="sxs-lookup"><span data-stu-id="1ee03-189">Provider</span></span> | <span data-ttu-id="1ee03-190">Fornece a configuração de &hellip;</span><span class="sxs-lookup"><span data-stu-id="1ee03-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="1ee03-191">[Provedor de Configuração do Azure Key Vault](xref:security/key-vault-configuration) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="1ee03-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="1ee03-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ee03-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="1ee03-193">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1ee03-194">Parâmetros de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-194">Command-line parameters</span></span> |
| [<span data-ttu-id="1ee03-195">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="1ee03-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1ee03-196">Fonte personalizada</span><span class="sxs-lookup"><span data-stu-id="1ee03-196">Custom source</span></span> |
| [<span data-ttu-id="1ee03-197">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1ee03-198">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-198">Environment variables</span></span> |
| [<span data-ttu-id="1ee03-199">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="1ee03-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1ee03-200">Arquivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="1ee03-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1ee03-201">Provedor de Configuração de Chave por Arquivo</span><span class="sxs-lookup"><span data-stu-id="1ee03-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="1ee03-202">Arquivos de diretório</span><span class="sxs-lookup"><span data-stu-id="1ee03-202">Directory files</span></span> |
| [<span data-ttu-id="1ee03-203">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1ee03-204">Coleções na memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-204">In-memory collections</span></span> |
| <span data-ttu-id="1ee03-205">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="1ee03-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1ee03-206">Arquivo no diretório de perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="1ee03-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="1ee03-207">Provider</span><span class="sxs-lookup"><span data-stu-id="1ee03-207">Provider</span></span> | <span data-ttu-id="1ee03-208">Fornece a configuração de &hellip;</span><span class="sxs-lookup"><span data-stu-id="1ee03-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="1ee03-209">[Provedor de Configuração do Azure Key Vault](xref:security/key-vault-configuration) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="1ee03-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="1ee03-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ee03-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="1ee03-211">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1ee03-212">Parâmetros de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-212">Command-line parameters</span></span> |
| [<span data-ttu-id="1ee03-213">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="1ee03-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1ee03-214">Fonte personalizada</span><span class="sxs-lookup"><span data-stu-id="1ee03-214">Custom source</span></span> |
| [<span data-ttu-id="1ee03-215">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1ee03-216">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-216">Environment variables</span></span> |
| [<span data-ttu-id="1ee03-217">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="1ee03-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1ee03-218">Arquivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="1ee03-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1ee03-219">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1ee03-220">Coleções na memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-220">In-memory collections</span></span> |
| <span data-ttu-id="1ee03-221">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="1ee03-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1ee03-222">Arquivo no diretório de perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="1ee03-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="1ee03-223">Provider</span><span class="sxs-lookup"><span data-stu-id="1ee03-223">Provider</span></span> | <span data-ttu-id="1ee03-224">Fornece a configuração de &hellip;</span><span class="sxs-lookup"><span data-stu-id="1ee03-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="1ee03-225">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1ee03-226">Parâmetros de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-226">Command-line parameters</span></span> |
| [<span data-ttu-id="1ee03-227">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="1ee03-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1ee03-228">Fonte personalizada</span><span class="sxs-lookup"><span data-stu-id="1ee03-228">Custom source</span></span> |
| [<span data-ttu-id="1ee03-229">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1ee03-230">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-230">Environment variables</span></span> |
| [<span data-ttu-id="1ee03-231">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="1ee03-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1ee03-232">Arquivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="1ee03-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1ee03-233">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1ee03-234">Coleções na memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-234">In-memory collections</span></span> |
| <span data-ttu-id="1ee03-235">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (tópicos de *Segurança*)</span><span class="sxs-lookup"><span data-stu-id="1ee03-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1ee03-236">Arquivo no diretório de perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="1ee03-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="1ee03-237">Na inicialização, as fontes de configuração são lidas na ordem especificada pelos provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="1ee03-238">Os provedores de configuração descritos neste tópico estão descritos em ordem alfabética, não na ordem na qual seu código pode organizá-los.</span><span class="sxs-lookup"><span data-stu-id="1ee03-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="1ee03-239">Organize os provedores de configuração em seu código para atender às suas prioridades para as fontes de configuração subjacentes.</span><span class="sxs-lookup"><span data-stu-id="1ee03-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="1ee03-240">Uma sequência comum de provedores de configuração é:</span><span class="sxs-lookup"><span data-stu-id="1ee03-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="1ee03-241">Arquivos (*appsettings.json*, *appsettings.{Environment}.json*, em que `{Environment}` é o ambiente de hospedagem atual do aplicativo)</span><span class="sxs-lookup"><span data-stu-id="1ee03-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="1ee03-242">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ee03-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="1ee03-243">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (apenas no ambiente de desenvolvimento)</span><span class="sxs-lookup"><span data-stu-id="1ee03-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="1ee03-244">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-244">Environment variables</span></span>
1. <span data-ttu-id="1ee03-245">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-245">Command-line arguments</span></span>

<span data-ttu-id="1ee03-246">É comum posicionar o Provedor de Configuração de Linha de Comando por último em uma série de provedores, a fim de permitir que os argumentos de linha de comando substituam a configuração definida por outros provedores.</span><span class="sxs-lookup"><span data-stu-id="1ee03-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-247">Essa sequência de provedores é aplicada quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="1ee03-248">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ee03-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-249">Essa sequência de provedores pode ser criada para o aplicativo (não o host) com um <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> e uma chamada para seu método <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="1ee03-250">No exemplo anterior, o nome do ambiente (`env.EnvironmentName`) e o nome do assembly de aplicativo (`env.ApplicationName`) são fornecidos pelo <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="1ee03-251">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="1ee03-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1ee03-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1ee03-253">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o Host da Web para especificar os provedores de configuração do aplicativo, além daqueles adicionados automaticamente por <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="1ee03-254">Provedor de Configuração de Linha de Comando</span><span class="sxs-lookup"><span data-stu-id="1ee03-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="1ee03-255">O <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carrega a configuração dos pares chave-valor do argumento de linha de comando em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1ee03-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="1ee03-256">Para ativar a configuração de linha de comando, o método de extensão <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> é chamado em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-257">`AddCommandLine` é chamado automaticamente quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="1ee03-258">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ee03-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="1ee03-259">`CreateDefaultBuilder` também carrega:</span><span class="sxs-lookup"><span data-stu-id="1ee03-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1ee03-260">Configuração opcional de *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="1ee03-261">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (no ambiente de desenvolvimento).</span><span class="sxs-lookup"><span data-stu-id="1ee03-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="1ee03-262">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ee03-262">Environment variables.</span></span>

<span data-ttu-id="1ee03-263">`CreateDefaultBuilder` adiciona o Provedor de Configuração de Linha de Comando por último.</span><span class="sxs-lookup"><span data-stu-id="1ee03-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="1ee03-264">Os argumentos de linha de comando passados em tempo de execução substituem a configuração definida por outros provedores.</span><span class="sxs-lookup"><span data-stu-id="1ee03-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="1ee03-265">`CreateDefaultBuilder` age quando o host é construído.</span><span class="sxs-lookup"><span data-stu-id="1ee03-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="1ee03-266">Portanto, a configuração de linha de comando ativada por `CreateDefaultBuilder` pode afetar como o host é configurado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-267">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1ee03-268">`AddCommandLine` já foi chamado por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1ee03-269">Se precisar oferecer configuração de aplicativo e ainda poder substituir essa configuração por argumentos de linha de comando, chame os provedores adicionais do aplicativo em <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chame `AddCommandLine` por fim.</span><span class="sxs-lookup"><span data-stu-id="1ee03-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-270">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-271">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="1ee03-272">`AddCommandLine` já foi chamado por `CreateDefaultBuilder` quando `UseConfiguration` é chamado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="1ee03-273">Se precisar oferecer configuração de aplicativo e ainda poder substituir essa configuração por argumentos de linha de comando, chame os provedores adicionais do aplicativo em um `ConfigurationBuilder` e chame `AddCommandLine` por fim.</span><span class="sxs-lookup"><span data-stu-id="1ee03-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="1ee03-274">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-275">Para ativar a configuração de linha de comando, chame o método de extensão <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-276">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="1ee03-277">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="1ee03-278">**Exemplo**</span><span class="sxs-lookup"><span data-stu-id="1ee03-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-279">O aplicativo de exemplo 2.x aproveita a vantagem do método de conveniência estático `CreateDefaultBuilder` para criar o host, que inclui uma chamada para <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-280">O aplicativo de exemplo 1.x chama <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> em um <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="1ee03-281">Abra um prompt de comando no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="1ee03-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="1ee03-282">Forneça um argumento de linha de comando para o comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="1ee03-283">Quando o aplicativo estiver em execução, abra um navegador para o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1ee03-284">Observe que a saída contém o par chave-valor do argumento de linha de comando de configuração fornecido para `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="1ee03-285">Arguments</span><span class="sxs-lookup"><span data-stu-id="1ee03-285">Arguments</span></span>

<span data-ttu-id="1ee03-286">O valor deve vir após um sinal de igual (`=`), ou a chave deve ter um prefixo (`--` ou `/`) quando o valor vier após um espaço.</span><span class="sxs-lookup"><span data-stu-id="1ee03-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="1ee03-287">O valor pode ser nulo se um sinal de igual for usado (por exemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="1ee03-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="1ee03-288">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-288">Key prefix</span></span>               | <span data-ttu-id="1ee03-289">Exemplo</span><span class="sxs-lookup"><span data-stu-id="1ee03-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="1ee03-290">Nenhum prefixo</span><span class="sxs-lookup"><span data-stu-id="1ee03-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="1ee03-291">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="1ee03-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="1ee03-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="1ee03-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="1ee03-293">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="1ee03-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="1ee03-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="1ee03-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="1ee03-295">No mesmo comando, não combine pares chave-valor do argumento de linha de comando que usam um sinal de igual com pares chave-valor que usam um espaço.</span><span class="sxs-lookup"><span data-stu-id="1ee03-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="1ee03-296">Exemplo de comandos:</span><span class="sxs-lookup"><span data-stu-id="1ee03-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="1ee03-297">Mapeamentos de comutador</span><span class="sxs-lookup"><span data-stu-id="1ee03-297">Switch mappings</span></span>

<span data-ttu-id="1ee03-298">Os mapeamentos de comutador permitem fornecer a lógica de substituição do nome da chave.</span><span class="sxs-lookup"><span data-stu-id="1ee03-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="1ee03-299">Quando você cria manualmente a configuração com um <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, pode fornecer um dicionário de substituições de opções para o método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="1ee03-300">Ao ser usado, o dicionário de mapeamentos de comutador é verificado para oferecer uma chave que corresponda à chave fornecida por um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="1ee03-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1ee03-301">Se a chave de linha de comando for encontrada no dicionário, o valor do dicionário (a substituição da chave) será passado de volta para definir o par chave-valor na configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="1ee03-302">Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixada com um traço único (`-`).</span><span class="sxs-lookup"><span data-stu-id="1ee03-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1ee03-303">Regras de chave do dicionário de mapeamentos de comutador:</span><span class="sxs-lookup"><span data-stu-id="1ee03-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1ee03-304">Os comutadores devem começar com um traço (`-`) ou traço duplo (`--`).</span><span class="sxs-lookup"><span data-stu-id="1ee03-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1ee03-305">O dicionário de mapeamentos de comutador chave não deve conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="1ee03-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-306">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1ee03-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-307">Conforme mostrado no exemplo anterior, a chamada para `CreateDefaultBuilder` não deve passar argumentos ao usar mapeamentos de opção.</span><span class="sxs-lookup"><span data-stu-id="1ee03-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="1ee03-308">A chamada `AddCommandLine` do método `CreateDefaultBuilder` não inclui opções mapeadas, e não é possível passar o dicionário de mapeamento de opções para `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1ee03-309">Se os argumentos incluírem uma opção mapeada e forem passados para `CreateDefaultBuilder`, o provedor `AddCommandLine` não inicializará com um <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="1ee03-310">A solução não é passar os argumentos para `CreateDefaultBuilder`, mas, em vez disso, permitir que o método `AddCommandLine` do método `ConfigurationBuilder` processe os dois argumentos e o dicionário de mapeamento de opções.</span><span class="sxs-lookup"><span data-stu-id="1ee03-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="1ee03-311">Conforme mostrado no exemplo anterior, a chamada para `CreateDefaultBuilder` não deve passar argumentos ao usar mapeamentos de opção.</span><span class="sxs-lookup"><span data-stu-id="1ee03-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="1ee03-312">A chamada `AddCommandLine` do método `CreateDefaultBuilder` não inclui opções mapeadas, e não é possível passar o dicionário de mapeamento de opções para `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1ee03-313">Se os argumentos incluírem uma opção mapeada e forem passados para `CreateDefaultBuilder`, o provedor `AddCommandLine` não inicializará com um <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="1ee03-314">A solução não é passar os argumentos para `CreateDefaultBuilder`, mas, em vez disso, permitir que o método `AddCommandLine` do método `ConfigurationBuilder` processe os dois argumentos e o dicionário de mapeamento de opções.</span><span class="sxs-lookup"><span data-stu-id="1ee03-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="1ee03-315">Depois que o dicionário de mapeamentos de comutador for criado, ele conterá os dados mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee03-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="1ee03-316">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-316">Key</span></span>       | <span data-ttu-id="1ee03-317">Valor</span><span class="sxs-lookup"><span data-stu-id="1ee03-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="1ee03-318">Se as chaves mapeadas para opção forem usadas ao iniciar o aplicativo, a configuração receberá o valor de configuração na chave fornecida pelo dicionário:</span><span class="sxs-lookup"><span data-stu-id="1ee03-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="1ee03-319">Após a execução do comando anterior, a configuração conterá os valores mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee03-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="1ee03-320">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-320">Key</span></span>               | <span data-ttu-id="1ee03-321">Valor</span><span class="sxs-lookup"><span data-stu-id="1ee03-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="1ee03-322">Provedor de Configuração de Variáveis de Ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="1ee03-323">O <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carrega a configuração dos pares chave-valor da variável de ambiente em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1ee03-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="1ee03-324">Para ativar a configuração das variáveis de ambiente, chame o método de extensão <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-325">Ao trabalhar com chaves hierárquicas nas variáveis de ambiente, talvez um separador de dois-pontos (`:`) não funcione em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="1ee03-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="1ee03-326">Um sublinhado duplo (`__`) é compatível com todas as plataformas e é substituído por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="1ee03-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="1ee03-327">O [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) permite que você defina variáveis de ambiente no portal do Azure que podem substituir a configuração do aplicativo usando o Provedor de Configuração de Variáveis de Ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ee03-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="1ee03-328">Para saber mais, confira [Aplicativos do Azure: substituir a configuração do aplicativo usando o portal do Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="1ee03-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-329">`AddEnvironmentVariables` é chamado automaticamente para as variáveis de ambiente prefixadas com `ASPNETCORE_` quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-329">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="1ee03-330">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ee03-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="1ee03-331">`CreateDefaultBuilder` também carrega:</span><span class="sxs-lookup"><span data-stu-id="1ee03-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1ee03-332">Configuração de aplicativo em variáveis de ambiente sem prefixos chamando `AddEnvironmentVariables` sem um prefixo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="1ee03-333">Configuração opcional de *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="1ee03-334">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (no ambiente de desenvolvimento).</span><span class="sxs-lookup"><span data-stu-id="1ee03-334">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="1ee03-335">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="1ee03-335">Command-line arguments.</span></span>

<span data-ttu-id="1ee03-336">O Provedor de Configuração de Variáveis de Ambiente é chamado depois que a configuração é estabelecida por meio dos segredos do usuário e dos arquivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="1ee03-337">Chamar o provedor nessa posição permite que as variáveis de ambiente sejam lidas em tempo de execução para substituir a configuração definida por segredos do usuário e arquivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-338">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-338">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1ee03-339">Se precisar fornecer configuração do aplicativo com base em variáveis de ambiente adicionais, chame os provedores adicionais do aplicativo no <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>e chame `AddEnvironmentVariables` com o prefixo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-340">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-341">Chame o método de extensão `AddEnvironmentVariables` em uma instância de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1ee03-342">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="1ee03-343">Se precisar fornecer configuração do aplicativo com base em variáveis de ambiente adicionais, chame os provedores adicionais do aplicativo no <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>e chame `AddEnvironmentVariables` com o prefixo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-343">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="1ee03-344">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-344">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-345">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-345">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="1ee03-346">**Exemplo**</span><span class="sxs-lookup"><span data-stu-id="1ee03-346">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-347">O aplicativo de exemplo 2.x aproveita a vantagem do método de conveniência estático `CreateDefaultBuilder` para criar o host, que inclui uma chamada para `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-347">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-348">O aplicativo de exemplo 1.x chama `AddEnvironmentVariables` em um `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-348">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="1ee03-349">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-349">Run the sample app.</span></span> <span data-ttu-id="1ee03-350">Abra um navegador para o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-350">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1ee03-351">Observe que a saída contém o par chave-valor da variável de ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-351">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="1ee03-352">O valor reflete o ambiente no qual o aplicativo está em execução, normalmente `Development` ao executar localmente.</span><span class="sxs-lookup"><span data-stu-id="1ee03-352">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="1ee03-353">Para encurtar a lista de variáveis de ambiente renderizadas pelo aplicativo, o aplicativo filtra as variáveis de ambiente para mostrar as que começam com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1ee03-353">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="1ee03-354">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="1ee03-354">ASPNETCORE_</span></span>
* <span data-ttu-id="1ee03-355">urls</span><span class="sxs-lookup"><span data-stu-id="1ee03-355">urls</span></span>
* <span data-ttu-id="1ee03-356">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="1ee03-356">Logging</span></span>
* <span data-ttu-id="1ee03-357">AMBIENTE</span><span class="sxs-lookup"><span data-stu-id="1ee03-357">ENVIRONMENT</span></span>
* <span data-ttu-id="1ee03-358">contentRoot</span><span class="sxs-lookup"><span data-stu-id="1ee03-358">contentRoot</span></span>
* <span data-ttu-id="1ee03-359">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="1ee03-359">AllowedHosts</span></span>
* <span data-ttu-id="1ee03-360">applicationName</span><span class="sxs-lookup"><span data-stu-id="1ee03-360">applicationName</span></span>
* <span data-ttu-id="1ee03-361">CommandLine</span><span class="sxs-lookup"><span data-stu-id="1ee03-361">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-362">Se você quiser expor todas as variáveis de ambiente disponíveis para o aplicativo, altere o `FilteredConfiguration` em *Pages/Index.cshtml.cs* para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1ee03-362">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-363">Se você quiser expor todas as variáveis de ambiente disponíveis para o aplicativo, altere o `FilteredConfiguration` em *Controllers/HomeController.cs* para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1ee03-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="1ee03-364">Prefixos</span><span class="sxs-lookup"><span data-stu-id="1ee03-364">Prefixes</span></span>

<span data-ttu-id="1ee03-365">Variáveis de ambiente carregadas na configuração do aplicativo são filtradas quando você fornece um prefixo para o método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-365">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="1ee03-366">Por exemplo, para filtrar as variáveis de ambiente no prefixo `CUSTOM_`, forneça o prefixo para o provedor de configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-366">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="1ee03-367">O prefixo é removido na criação dos pares chave-valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-367">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-368">O método de conveniência estático `CreateDefaultBuilder` cria um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> para estabelecer o host do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-368">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="1ee03-369">Quando `WebHostBuilder` é criado, ele encontra a configuração de seu host nas variáveis de ambiente prefixadas com `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-369">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="1ee03-370">**Prefixos de cadeia de conexão**</span><span class="sxs-lookup"><span data-stu-id="1ee03-370">**Connection string prefixes**</span></span>

<span data-ttu-id="1ee03-371">A API de configuração tem regras de processamento especiais para quatro variáveis de ambiente de cadeia de conexão envolvidas na configuração de cadeias de conexão do Azure para o ambiente de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-371">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="1ee03-372">As variáveis de ambiente com os prefixos mostrados na tabela são carregadas no aplicativo se nenhum prefixo for fornecido para `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-372">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="1ee03-373">Prefixo da cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="1ee03-373">Connection string prefix</span></span> | <span data-ttu-id="1ee03-374">Provider</span><span class="sxs-lookup"><span data-stu-id="1ee03-374">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="1ee03-375">Provedor personalizado</span><span class="sxs-lookup"><span data-stu-id="1ee03-375">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="1ee03-376">MySQL</span><span class="sxs-lookup"><span data-stu-id="1ee03-376">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="1ee03-377">Banco de Dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="1ee03-377">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="1ee03-378">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1ee03-378">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="1ee03-379">Quando uma variável de ambiente for descoberta e carregada na configuração com qualquer um dos quatro prefixos mostrados na tabela:</span><span class="sxs-lookup"><span data-stu-id="1ee03-379">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="1ee03-380">A chave de configuração é criada removendo o prefixo da variável de ambiente e adicionando uma seção de chave de configuração (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="1ee03-380">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="1ee03-381">Um novo par chave-valor de configuração é criado para representar o provedor de conexão de banco de dados (exceto para `CUSTOMCONNSTR_`, que não tem um provedor indicado).</span><span class="sxs-lookup"><span data-stu-id="1ee03-381">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="1ee03-382">Chave de variável de ambiente</span><span class="sxs-lookup"><span data-stu-id="1ee03-382">Environment variable key</span></span> | <span data-ttu-id="1ee03-383">Chave de configuração convertida</span><span class="sxs-lookup"><span data-stu-id="1ee03-383">Converted configuration key</span></span> | <span data-ttu-id="1ee03-384">Entrada de configuração do provedor</span><span class="sxs-lookup"><span data-stu-id="1ee03-384">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ee03-385">Entrada de configuração não criada.</span><span class="sxs-lookup"><span data-stu-id="1ee03-385">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ee03-386">Chave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-386">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1ee03-387">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="1ee03-387">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ee03-388">Chave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-388">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1ee03-389">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1ee03-389">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ee03-390">Chave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-390">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1ee03-391">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1ee03-391">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="1ee03-392">Provedor de Configuração de Arquivo</span><span class="sxs-lookup"><span data-stu-id="1ee03-392">File Configuration Provider</span></span>

<span data-ttu-id="1ee03-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> é a classe base para carregamento da configuração do sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="1ee03-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="1ee03-394">Os provedores de configuração a seguir são dedicados a tipos específicos de arquivos:</span><span class="sxs-lookup"><span data-stu-id="1ee03-394">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="1ee03-395">Provedor de Configuração INI</span><span class="sxs-lookup"><span data-stu-id="1ee03-395">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="1ee03-396">Provedor de Configuração JSON</span><span class="sxs-lookup"><span data-stu-id="1ee03-396">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="1ee03-397">Provedor de Configuração XML</span><span class="sxs-lookup"><span data-stu-id="1ee03-397">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="1ee03-398">Provedor de Configuração INI</span><span class="sxs-lookup"><span data-stu-id="1ee03-398">INI Configuration Provider</span></span>

<span data-ttu-id="1ee03-399">O <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carrega a configuração de pares chave-valor do arquivo INI em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1ee03-399">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="1ee03-400">Para ativar a configuração de arquivo INI, chame o método de extensão <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-400">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-401">Os dois-pontos podem ser usados como um delimitador de seção na configuração do arquivo INI.</span><span class="sxs-lookup"><span data-stu-id="1ee03-401">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="1ee03-402">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="1ee03-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ee03-403">Se o arquivo é opcional.</span><span class="sxs-lookup"><span data-stu-id="1ee03-403">Whether the file is optional.</span></span>
* <span data-ttu-id="1ee03-404">Se a configuração será recarregada caso o arquivo seja alterado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1ee03-405">O <xref:Microsoft.Extensions.FileProviders.IFileProvider> usado para acessar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-406">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1ee03-406">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-407">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-407">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-408">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-408">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ee03-409">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-409">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-410">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-410">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="1ee03-411">Um exemplo genérico de um arquivo de configuração INI:</span><span class="sxs-lookup"><span data-stu-id="1ee03-411">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="1ee03-412">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-412">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ee03-413">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-413">section0:key0</span></span>
* <span data-ttu-id="1ee03-414">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-414">section0:key1</span></span>
* <span data-ttu-id="1ee03-415">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="1ee03-415">section1:subsection:key</span></span>
* <span data-ttu-id="1ee03-416">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="1ee03-416">section2:subsection0:key</span></span>
* <span data-ttu-id="1ee03-417">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="1ee03-417">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="1ee03-418">Provedor de Configuração JSON</span><span class="sxs-lookup"><span data-stu-id="1ee03-418">JSON Configuration Provider</span></span>

<span data-ttu-id="1ee03-419">O <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carrega a configuração de pares chave-valor do arquivo JSON em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1ee03-419">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="1ee03-420">Para ativar a configuração de arquivo JSON, chame o método de extensão <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-420">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-421">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="1ee03-421">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ee03-422">Se o arquivo é opcional.</span><span class="sxs-lookup"><span data-stu-id="1ee03-422">Whether the file is optional.</span></span>
* <span data-ttu-id="1ee03-423">Se a configuração será recarregada caso o arquivo seja alterado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-423">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1ee03-424">O <xref:Microsoft.Extensions.FileProviders.IFileProvider> usado para acessar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-424">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-425">`AddJsonFile` é chamado automaticamente duas vezes quando você inicializa um novo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-425">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="1ee03-426">O método é chamado para carregar a configuração de:</span><span class="sxs-lookup"><span data-stu-id="1ee03-426">The method is called to load configuration from:</span></span>

* <span data-ttu-id="1ee03-427">*appsettings.json* &ndash; Esse arquivo é lido primeiro.</span><span class="sxs-lookup"><span data-stu-id="1ee03-427">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="1ee03-428">A versão do ambiente do arquivo pode substituir os valores fornecidos pelo arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-428">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="1ee03-429">*appsettings.{Environment}.json* &ndash; A versão de ambiente do arquivo é carregada com base em [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="1ee03-429">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="1ee03-430">Para saber mais, veja o tópico [Host da Web: configurar um host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ee03-430">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="1ee03-431">`CreateDefaultBuilder` também carrega:</span><span class="sxs-lookup"><span data-stu-id="1ee03-431">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1ee03-432">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ee03-432">Environment variables.</span></span>
* <span data-ttu-id="1ee03-433">[Segredos do usuário (Gerenciador de Segredo)](xref:security/app-secrets) (no ambiente de desenvolvimento).</span><span class="sxs-lookup"><span data-stu-id="1ee03-433">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="1ee03-434">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="1ee03-434">Command-line arguments.</span></span>

<span data-ttu-id="1ee03-435">O Provedor de Configuração JSON é estabelecido primeiro.</span><span class="sxs-lookup"><span data-stu-id="1ee03-435">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="1ee03-436">Portanto, os segredos do usuário, as variáveis de ambiente e os argumentos de linha de comando substituem a configuração definida pelos arquivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-436">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-437">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo para arquivos que não sejam *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-437">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-438">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-438">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-439">Você pode chamar diretamente o método de extensão `AddJsonFile` em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-439">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-440">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-440">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ee03-441">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-441">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-442">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-442">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="1ee03-443">**Exemplo**</span><span class="sxs-lookup"><span data-stu-id="1ee03-443">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-444">O aplicativo de exemplo 2.x aproveita a vantagem do método de conveniência estático `CreateDefaultBuilder` para criar o host, que inclui duas chamadas para `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-444">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="1ee03-445">A configuração é carregada de *appsettings.json* e de *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-445">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-446">O aplicativo de exemplo 1.x chama `AddJsonFile` duas vezes em um `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-446">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="1ee03-447">A configuração é carregada de *appsettings.json* e de *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-447">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="1ee03-448">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-448">Run the sample app.</span></span> <span data-ttu-id="1ee03-449">Abra um navegador para o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-449">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1ee03-450">Observe que a saída contém pares chave-valor para a configuração mostrada na tabela de acordo com o ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ee03-450">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="1ee03-451">Chaves de configuração de registro usam os dois-pontos (`:`) como um separador hierárquico.</span><span class="sxs-lookup"><span data-stu-id="1ee03-451">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="1ee03-452">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-452">Key</span></span>                        | <span data-ttu-id="1ee03-453">Valor de Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="1ee03-453">Development Value</span></span> | <span data-ttu-id="1ee03-454">Valor de Produção</span><span class="sxs-lookup"><span data-stu-id="1ee03-454">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="1ee03-455">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="1ee03-455">Logging:LogLevel:System</span></span>    | <span data-ttu-id="1ee03-456">Informações</span><span class="sxs-lookup"><span data-stu-id="1ee03-456">Information</span></span>       | <span data-ttu-id="1ee03-457">Informações</span><span class="sxs-lookup"><span data-stu-id="1ee03-457">Information</span></span>      |
| <span data-ttu-id="1ee03-458">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="1ee03-458">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="1ee03-459">Informações</span><span class="sxs-lookup"><span data-stu-id="1ee03-459">Information</span></span>       | <span data-ttu-id="1ee03-460">Informações</span><span class="sxs-lookup"><span data-stu-id="1ee03-460">Information</span></span>      |
| <span data-ttu-id="1ee03-461">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="1ee03-461">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="1ee03-462">Depurar</span><span class="sxs-lookup"><span data-stu-id="1ee03-462">Debug</span></span>             | <span data-ttu-id="1ee03-463">Erro</span><span class="sxs-lookup"><span data-stu-id="1ee03-463">Error</span></span>            |
| <span data-ttu-id="1ee03-464">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="1ee03-464">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="1ee03-465">Provedor de Configuração XML</span><span class="sxs-lookup"><span data-stu-id="1ee03-465">XML Configuration Provider</span></span>

<span data-ttu-id="1ee03-466">O <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carrega a configuração de pares chave-valor do arquivo XML em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1ee03-466">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="1ee03-467">Para ativar a configuração de arquivo XML, chame o método de extensão <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-467">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-468">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="1ee03-468">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ee03-469">Se o arquivo é opcional.</span><span class="sxs-lookup"><span data-stu-id="1ee03-469">Whether the file is optional.</span></span>
* <span data-ttu-id="1ee03-470">Se a configuração será recarregada caso o arquivo seja alterado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-470">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1ee03-471">O <xref:Microsoft.Extensions.FileProviders.IFileProvider> usado para acessar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-471">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1ee03-472">O nó raiz do arquivo de configuração é ignorado quando os pares chave-valor da configuração são criados.</span><span class="sxs-lookup"><span data-stu-id="1ee03-472">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="1ee03-473">Não especifique um DTD (definição de tipo de documento) ou um namespace no arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-473">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-474">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1ee03-474">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-475">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-475">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-476">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-476">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ee03-477">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-477">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-478">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-478">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="1ee03-479">Os arquivos de configuração XML podem usar nomes de elemento distintos para seções repetidas:</span><span class="sxs-lookup"><span data-stu-id="1ee03-479">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="1ee03-480">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-480">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ee03-481">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-481">section0:key0</span></span>
* <span data-ttu-id="1ee03-482">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-482">section0:key1</span></span>
* <span data-ttu-id="1ee03-483">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-483">section1:key0</span></span>
* <span data-ttu-id="1ee03-484">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-484">section1:key1</span></span>

<span data-ttu-id="1ee03-485">Elementos repetidos que usam o mesmo nome de elemento funcionarão se o atributo `name` for usado para diferenciar os elementos:</span><span class="sxs-lookup"><span data-stu-id="1ee03-485">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="1ee03-486">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-486">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ee03-487">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-487">section:section0:key:key0</span></span>
* <span data-ttu-id="1ee03-488">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-488">section:section0:key:key1</span></span>
* <span data-ttu-id="1ee03-489">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-489">section:section1:key:key0</span></span>
* <span data-ttu-id="1ee03-490">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-490">section:section1:key:key1</span></span>

<span data-ttu-id="1ee03-491">É possível usar atributos para fornecer valores:</span><span class="sxs-lookup"><span data-stu-id="1ee03-491">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="1ee03-492">O arquivo de configuração anterior carrega as seguintes chaves com `value`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-492">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ee03-493">key:attribute</span><span class="sxs-lookup"><span data-stu-id="1ee03-493">key:attribute</span></span>
* <span data-ttu-id="1ee03-494">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="1ee03-494">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="1ee03-495">Provedor de Configuração de Chave por Arquivo</span><span class="sxs-lookup"><span data-stu-id="1ee03-495">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="1ee03-496">O <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa arquivos do diretório como pares chave-valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-496">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="1ee03-497">A chave é o nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-497">The key is the file name.</span></span> <span data-ttu-id="1ee03-498">O valor contém o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-498">The value contains the file's contents.</span></span> <span data-ttu-id="1ee03-499">O Provedor de Configuração de Chave por arquivo é usado em cenários de hospedagem do Docker.</span><span class="sxs-lookup"><span data-stu-id="1ee03-499">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="1ee03-500">Para ativar a configuração de chave por arquivo, chame o método de extensão <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-500">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1ee03-501">O `directoryPath` para os arquivos deve ser um caminho absoluto.</span><span class="sxs-lookup"><span data-stu-id="1ee03-501">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="1ee03-502">As sobrecargas permitem especificar:</span><span class="sxs-lookup"><span data-stu-id="1ee03-502">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ee03-503">Um delegado `Action<KeyPerFileConfigurationSource>` que configura a origem.</span><span class="sxs-lookup"><span data-stu-id="1ee03-503">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="1ee03-504">Se o diretório é opcional, e o caminho para o diretório.</span><span class="sxs-lookup"><span data-stu-id="1ee03-504">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="1ee03-505">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1ee03-505">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-506">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-506">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="1ee03-507">Provedor de Configuração de Memória</span><span class="sxs-lookup"><span data-stu-id="1ee03-507">Memory Configuration Provider</span></span>

<span data-ttu-id="1ee03-508">O <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa uma coleção na memória como pares chave-valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-508">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="1ee03-509">Para ativar a configuração de coleção na memória, chame o método de extensão <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> em uma instância do <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-509">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ee03-510">O provedor de configuração pode ser inicializado com um `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-510">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1ee03-511">Chame <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ao criar o host para especificar a configuração do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1ee03-511">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ee03-512">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-512">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1ee03-513">Ao chamar `CreateDefaultBuilder`, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-513">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ee03-514">Ao criar um <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> diretamente, chame <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> com a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-514">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-515">Aplique a configuração ao <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> com o método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-515">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="1ee03-516">GetValue</span><span class="sxs-lookup"><span data-stu-id="1ee03-516">GetValue</span></span>

<span data-ttu-id="1ee03-517">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrai um valor de configuração com uma chave especificada e o converte para o tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-517">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="1ee03-518">Uma sobrecarga permite que você forneça um valor padrão se a chave não for encontrada.</span><span class="sxs-lookup"><span data-stu-id="1ee03-518">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="1ee03-519">O exemplo a seguir extrai o valor de cadeia de caracteres da configuração com a chave `NumberKey`, digita o valor como um `int` e armazena o valor na variável `intValue`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-519">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="1ee03-520">Se `NumberKey` não for encontrado em chaves de configuração, `intValue` recebe o valor padrão de `99`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-520">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="1ee03-521">GetSection, GetChildren e Exists</span><span class="sxs-lookup"><span data-stu-id="1ee03-521">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="1ee03-522">Para os exemplos a seguir, considere o seguinte arquivo JSON.</span><span class="sxs-lookup"><span data-stu-id="1ee03-522">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="1ee03-523">Há quatro chaves em duas seções, cada uma delas inclui um par de subseções:</span><span class="sxs-lookup"><span data-stu-id="1ee03-523">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="1ee03-524">Quando o arquivo é lido na configuração, as seguintes chaves hierárquicas exclusivas são criadas para conter os valores de configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-524">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="1ee03-525">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-525">section0:key0</span></span>
* <span data-ttu-id="1ee03-526">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-526">section0:key1</span></span>
* <span data-ttu-id="1ee03-527">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-527">section1:key0</span></span>
* <span data-ttu-id="1ee03-528">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-528">section1:key1</span></span>
* <span data-ttu-id="1ee03-529">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-529">section2:subsection0:key0</span></span>
* <span data-ttu-id="1ee03-530">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-530">section2:subsection0:key1</span></span>
* <span data-ttu-id="1ee03-531">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="1ee03-531">section2:subsection1:key0</span></span>
* <span data-ttu-id="1ee03-532">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="1ee03-532">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="1ee03-533">GetSection</span><span class="sxs-lookup"><span data-stu-id="1ee03-533">GetSection</span></span>

<span data-ttu-id="1ee03-534">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrai uma subseção de configuração com a chave de subseção especificada.</span><span class="sxs-lookup"><span data-stu-id="1ee03-534">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="1ee03-535">Para retornar um <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contenha apenas os pares chave-valor em `section1`, chame `GetSection` e forneça o nome da seção:</span><span class="sxs-lookup"><span data-stu-id="1ee03-535">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="1ee03-536">Da mesma forma, para obter os valores de chaves em `section2:subsection0`, chame `GetSection` e forneça o caminho de seção:</span><span class="sxs-lookup"><span data-stu-id="1ee03-536">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="1ee03-537">`GetSection` nunca retorna `null`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-537">`GetSection` never returns `null`.</span></span> <span data-ttu-id="1ee03-538">Se uma seção correspondente não for encontrada, um `IConfigurationSection` vazio será retornado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-538">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="1ee03-539">GetChildren</span><span class="sxs-lookup"><span data-stu-id="1ee03-539">GetChildren</span></span>

<span data-ttu-id="1ee03-540">Uma chamada para [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) em `section2` obtém um `IEnumerable<IConfigurationSection>` que inclui:</span><span class="sxs-lookup"><span data-stu-id="1ee03-540">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="1ee03-541">Exists</span><span class="sxs-lookup"><span data-stu-id="1ee03-541">Exists</span></span>

<span data-ttu-id="1ee03-542">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar se há uma seção de configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-542">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="1ee03-543">Considerando os dados de exemplo, `sectionExists` é `false`, pois não existe uma seção `section2:subsection2` nos dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-543">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="1ee03-544">Associar a uma classe</span><span class="sxs-lookup"><span data-stu-id="1ee03-544">Bind to a class</span></span>

<span data-ttu-id="1ee03-545">A configuração pode ser associada a classes que representam grupos de configurações relacionadas usando o *padrão de opções*.</span><span class="sxs-lookup"><span data-stu-id="1ee03-545">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="1ee03-546">Para obter mais informações, consulte <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-546">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1ee03-547">Os valores de configuração retornam como cadeias de caracteres, mas chamar <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite a construção de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="1ee03-547">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="1ee03-548">O aplicativo de exemplo contém um modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="1ee03-548">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-549">A seção `starship` do arquivo *starship.json* cria a configuração quando o aplicativo de exemplo usa o Provedor de Configuração JSON para carregar a configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-549">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="1ee03-550">Os seguintes pares chave-valor de configuração são criados:</span><span class="sxs-lookup"><span data-stu-id="1ee03-550">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="1ee03-551">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-551">Key</span></span>                   | <span data-ttu-id="1ee03-552">Valor</span><span class="sxs-lookup"><span data-stu-id="1ee03-552">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="1ee03-553">starship:name</span><span class="sxs-lookup"><span data-stu-id="1ee03-553">starship:name</span></span>         | <span data-ttu-id="1ee03-554">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="1ee03-554">USS Enterprise</span></span>                                    |
| <span data-ttu-id="1ee03-555">starship:registry</span><span class="sxs-lookup"><span data-stu-id="1ee03-555">starship:registry</span></span>     | <span data-ttu-id="1ee03-556">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="1ee03-556">NCC-1701</span></span>                                          |
| <span data-ttu-id="1ee03-557">starship:class</span><span class="sxs-lookup"><span data-stu-id="1ee03-557">starship:class</span></span>        | <span data-ttu-id="1ee03-558">Constituição</span><span class="sxs-lookup"><span data-stu-id="1ee03-558">Constitution</span></span>                                      |
| <span data-ttu-id="1ee03-559">starship:length</span><span class="sxs-lookup"><span data-stu-id="1ee03-559">starship:length</span></span>       | <span data-ttu-id="1ee03-560">304,8</span><span class="sxs-lookup"><span data-stu-id="1ee03-560">304.8</span></span>                                             |
| <span data-ttu-id="1ee03-561">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="1ee03-561">starship:commissioned</span></span> | <span data-ttu-id="1ee03-562">False</span><span class="sxs-lookup"><span data-stu-id="1ee03-562">False</span></span>                                             |
| <span data-ttu-id="1ee03-563">marca</span><span class="sxs-lookup"><span data-stu-id="1ee03-563">trademark</span></span>             | <span data-ttu-id="1ee03-564">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="1ee03-564">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="1ee03-565">O aplicativo de exemplo chama `GetSection` com a chave `starship`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-565">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="1ee03-566">Os pares chave-valor `starship` são isolados.</span><span class="sxs-lookup"><span data-stu-id="1ee03-566">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="1ee03-567">O método `Bind` é chamado na subseção passando uma instância da classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-567">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="1ee03-568">Depois de associar os valores de instância, a instância é atribuída a uma propriedade para renderização:</span><span class="sxs-lookup"><span data-stu-id="1ee03-568">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1ee03-569">Associar a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="1ee03-569">Bind to an object graph</span></span>

<span data-ttu-id="1ee03-570"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> é capaz de associar um grafo de objeto POCO inteiro.</span><span class="sxs-lookup"><span data-stu-id="1ee03-570"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="1ee03-571">O exemplo contém um modelo `TvShow` cujo grafo do objeto inclui as classes `Metadata` e `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="1ee03-571">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-572">O aplicativo de exemplo tem um arquivo *tvshow.xml* que contém os dados de configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-572">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="1ee03-573">A configuração está associada ao método do grafo de objeto `TvShow` inteiro com o método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-573">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="1ee03-574">A instância associada é atribuída a uma propriedade para renderização:</span><span class="sxs-lookup"><span data-stu-id="1ee03-574">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="1ee03-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e retorna o tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="1ee03-576">O `Get<T>` é mais conveniente do que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-576">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="1ee03-577">O código a seguir mostra como usar `Get<T>` com o exemplo anterior, que permite que a instância associada seja diretamente atribuída à propriedade usada para renderização:</span><span class="sxs-lookup"><span data-stu-id="1ee03-577">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="1ee03-578">Associar uma matriz a uma classe</span><span class="sxs-lookup"><span data-stu-id="1ee03-578">Bind an array to a class</span></span>

<span data-ttu-id="1ee03-579">*O aplicativo de exemplo demonstra os conceitos explicados nesta seção.*</span><span class="sxs-lookup"><span data-stu-id="1ee03-579">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="1ee03-580">O <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> dá suporte a matrizes de associação para objetos usando os índices em chaves de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-580">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1ee03-581">Qualquer formato de matriz que exponha um segmento de chave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) é capaz de associar matrizes a uma matriz de classe POCO.</span><span class="sxs-lookup"><span data-stu-id="1ee03-581">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="1ee03-582">A associação é fornecida por convenção.</span><span class="sxs-lookup"><span data-stu-id="1ee03-582">Binding is provided by convention.</span></span> <span data-ttu-id="1ee03-583">Provedores de configuração personalizados não são necessários para implementar a associação de matriz.</span><span class="sxs-lookup"><span data-stu-id="1ee03-583">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="1ee03-584">**Processamento de matriz na memória**</span><span class="sxs-lookup"><span data-stu-id="1ee03-584">**In-memory array processing**</span></span>

<span data-ttu-id="1ee03-585">Considere as chaves de configuração e os valores mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee03-585">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="1ee03-586">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-586">Key</span></span>             | <span data-ttu-id="1ee03-587">Valor</span><span class="sxs-lookup"><span data-stu-id="1ee03-587">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1ee03-588">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="1ee03-588">array:entries:0</span></span> | <span data-ttu-id="1ee03-589">value0</span><span class="sxs-lookup"><span data-stu-id="1ee03-589">value0</span></span> |
| <span data-ttu-id="1ee03-590">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="1ee03-590">array:entries:1</span></span> | <span data-ttu-id="1ee03-591">value1</span><span class="sxs-lookup"><span data-stu-id="1ee03-591">value1</span></span> |
| <span data-ttu-id="1ee03-592">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="1ee03-592">array:entries:2</span></span> | <span data-ttu-id="1ee03-593">value2</span><span class="sxs-lookup"><span data-stu-id="1ee03-593">value2</span></span> |
| <span data-ttu-id="1ee03-594">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="1ee03-594">array:entries:4</span></span> | <span data-ttu-id="1ee03-595">value4</span><span class="sxs-lookup"><span data-stu-id="1ee03-595">value4</span></span> |
| <span data-ttu-id="1ee03-596">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="1ee03-596">array:entries:5</span></span> | <span data-ttu-id="1ee03-597">value5</span><span class="sxs-lookup"><span data-stu-id="1ee03-597">value5</span></span> |

<span data-ttu-id="1ee03-598">Essas chaves e valores são carregados no aplicativo de exemplo usando o Provedor de Configuração de Memória:</span><span class="sxs-lookup"><span data-stu-id="1ee03-598">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="1ee03-599">A matriz ignora um valor para o índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="1ee03-599">The array skips a value for index &num;3.</span></span> <span data-ttu-id="1ee03-600">O associador de configuração não é capaz de associar valores nulos ou criar entradas nulas em objetos associados, o que fica claro em um momento quando o resultado da associação dessa matriz a um objeto é demonstrado.</span><span class="sxs-lookup"><span data-stu-id="1ee03-600">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="1ee03-601">No aplicativo de exemplo, uma classe POCO está disponível para armazenar os dados de configuração associados:</span><span class="sxs-lookup"><span data-stu-id="1ee03-601">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-602">Os dados de configuração estão associados ao objeto:</span><span class="sxs-lookup"><span data-stu-id="1ee03-602">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="1ee03-603">A sintaxe [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) também pode ser usada, o que resulta em um código mais compacto:</span><span class="sxs-lookup"><span data-stu-id="1ee03-603">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="1ee03-604">O objeto associado, uma instância de `ArrayExample`, recebe os dados da matriz de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-604">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="1ee03-605">Índice `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ee03-605">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1ee03-606">Valor `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ee03-606">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1ee03-607">0</span><span class="sxs-lookup"><span data-stu-id="1ee03-607">0</span></span>                            | <span data-ttu-id="1ee03-608">value0</span><span class="sxs-lookup"><span data-stu-id="1ee03-608">value0</span></span>                       |
| <span data-ttu-id="1ee03-609">1</span><span class="sxs-lookup"><span data-stu-id="1ee03-609">1</span></span>                            | <span data-ttu-id="1ee03-610">value1</span><span class="sxs-lookup"><span data-stu-id="1ee03-610">value1</span></span>                       |
| <span data-ttu-id="1ee03-611">2</span><span class="sxs-lookup"><span data-stu-id="1ee03-611">2</span></span>                            | <span data-ttu-id="1ee03-612">value2</span><span class="sxs-lookup"><span data-stu-id="1ee03-612">value2</span></span>                       |
| <span data-ttu-id="1ee03-613">3</span><span class="sxs-lookup"><span data-stu-id="1ee03-613">3</span></span>                            | <span data-ttu-id="1ee03-614">value4</span><span class="sxs-lookup"><span data-stu-id="1ee03-614">value4</span></span>                       |
| <span data-ttu-id="1ee03-615">4</span><span class="sxs-lookup"><span data-stu-id="1ee03-615">4</span></span>                            | <span data-ttu-id="1ee03-616">value5</span><span class="sxs-lookup"><span data-stu-id="1ee03-616">value5</span></span>                       |

<span data-ttu-id="1ee03-617">O índice &num;3 no objeto associado contém os dados de configuração para a chave de configuração `array:4` e seu valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-617">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="1ee03-618">Quando os dados de configuração que contêm uma matriz são associados, os índices da matriz nas chaves de configuração são simplesmente usados para iterar os dados de configuração ao criar o objeto.</span><span class="sxs-lookup"><span data-stu-id="1ee03-618">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="1ee03-619">Um valor nulo não pode ser mantido nos dados de configuração, e uma entrada de valor nulo não será criada em um objeto de associação quando uma matriz nas chaves de configuração ignorar um ou mais índices.</span><span class="sxs-lookup"><span data-stu-id="1ee03-619">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="1ee03-620">O item de configuração ausente para o índice &num;3 pode ser fornecido antes da associação à instância `ArrayExample` por qualquer provedor de configuração que produza o par chave-valor correto na configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-620">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="1ee03-621">Se o exemplo incluir um Provedor de Configuração JSON adicional com o par chave-valor ausente, o `ArrayExample.Entries` coincidirá com a matriz de configuração completa:</span><span class="sxs-lookup"><span data-stu-id="1ee03-621">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="1ee03-622">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-622">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1ee03-623">No <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ee03-623">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1ee03-624">No construtor `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1ee03-624">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="1ee03-625">O par chave-valor mostrado na tabela é carregado na configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-625">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="1ee03-626">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-626">Key</span></span>             | <span data-ttu-id="1ee03-627">Valor</span><span class="sxs-lookup"><span data-stu-id="1ee03-627">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1ee03-628">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="1ee03-628">array:entries:3</span></span> | <span data-ttu-id="1ee03-629">value3</span><span class="sxs-lookup"><span data-stu-id="1ee03-629">value3</span></span> |

<span data-ttu-id="1ee03-630">Se a instância da classe `ArrayExample` for associada após o Provedor de Configuração JSON incluir a entrada para o índice &num;3, a matriz `ArrayExample.Entries` incluirá o valor.</span><span class="sxs-lookup"><span data-stu-id="1ee03-630">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="1ee03-631">Índice `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ee03-631">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1ee03-632">Valor `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ee03-632">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1ee03-633">0</span><span class="sxs-lookup"><span data-stu-id="1ee03-633">0</span></span>                            | <span data-ttu-id="1ee03-634">value0</span><span class="sxs-lookup"><span data-stu-id="1ee03-634">value0</span></span>                       |
| <span data-ttu-id="1ee03-635">1</span><span class="sxs-lookup"><span data-stu-id="1ee03-635">1</span></span>                            | <span data-ttu-id="1ee03-636">value1</span><span class="sxs-lookup"><span data-stu-id="1ee03-636">value1</span></span>                       |
| <span data-ttu-id="1ee03-637">2</span><span class="sxs-lookup"><span data-stu-id="1ee03-637">2</span></span>                            | <span data-ttu-id="1ee03-638">value2</span><span class="sxs-lookup"><span data-stu-id="1ee03-638">value2</span></span>                       |
| <span data-ttu-id="1ee03-639">3</span><span class="sxs-lookup"><span data-stu-id="1ee03-639">3</span></span>                            | <span data-ttu-id="1ee03-640">value3</span><span class="sxs-lookup"><span data-stu-id="1ee03-640">value3</span></span>                       |
| <span data-ttu-id="1ee03-641">4</span><span class="sxs-lookup"><span data-stu-id="1ee03-641">4</span></span>                            | <span data-ttu-id="1ee03-642">value4</span><span class="sxs-lookup"><span data-stu-id="1ee03-642">value4</span></span>                       |
| <span data-ttu-id="1ee03-643">5</span><span class="sxs-lookup"><span data-stu-id="1ee03-643">5</span></span>                            | <span data-ttu-id="1ee03-644">value5</span><span class="sxs-lookup"><span data-stu-id="1ee03-644">value5</span></span>                       |

<span data-ttu-id="1ee03-645">**Processamento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="1ee03-645">**JSON array processing**</span></span>

<span data-ttu-id="1ee03-646">Se um arquivo JSON contiver uma matriz, as chaves de configuração serão criadas para os elementos da matriz com um índice de seção de base zero.</span><span class="sxs-lookup"><span data-stu-id="1ee03-646">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="1ee03-647">No arquivo de configuração a seguir, `subsection` é uma matriz:</span><span class="sxs-lookup"><span data-stu-id="1ee03-647">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="1ee03-648">O Provedor de Configuração JSON lê os dados de configuração para os seguintes pares chave-valor:</span><span class="sxs-lookup"><span data-stu-id="1ee03-648">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="1ee03-649">Chave</span><span class="sxs-lookup"><span data-stu-id="1ee03-649">Key</span></span>                     | <span data-ttu-id="1ee03-650">Valor</span><span class="sxs-lookup"><span data-stu-id="1ee03-650">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="1ee03-651">json_array:key</span><span class="sxs-lookup"><span data-stu-id="1ee03-651">json_array:key</span></span>          | <span data-ttu-id="1ee03-652">valueA</span><span class="sxs-lookup"><span data-stu-id="1ee03-652">valueA</span></span> |
| <span data-ttu-id="1ee03-653">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="1ee03-653">json_array:subsection:0</span></span> | <span data-ttu-id="1ee03-654">valueB</span><span class="sxs-lookup"><span data-stu-id="1ee03-654">valueB</span></span> |
| <span data-ttu-id="1ee03-655">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="1ee03-655">json_array:subsection:1</span></span> | <span data-ttu-id="1ee03-656">valueC</span><span class="sxs-lookup"><span data-stu-id="1ee03-656">valueC</span></span> |
| <span data-ttu-id="1ee03-657">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="1ee03-657">json_array:subsection:2</span></span> | <span data-ttu-id="1ee03-658">valueD</span><span class="sxs-lookup"><span data-stu-id="1ee03-658">valueD</span></span> |

<span data-ttu-id="1ee03-659">No aplicativo de exemplo, a seguinte classe POCO está disponível para associar os pares chave-valor de configuração:</span><span class="sxs-lookup"><span data-stu-id="1ee03-659">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-660">Após a associação, `JsonArrayExample.Key` contém o valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-660">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="1ee03-661">Os valores de subseção são armazenados na propriedade matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-661">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="1ee03-662">Índice `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1ee03-662">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="1ee03-663">Valor `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1ee03-663">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="1ee03-664">0</span><span class="sxs-lookup"><span data-stu-id="1ee03-664">0</span></span>                                   | <span data-ttu-id="1ee03-665">valueB</span><span class="sxs-lookup"><span data-stu-id="1ee03-665">valueB</span></span>                              |
| <span data-ttu-id="1ee03-666">1</span><span class="sxs-lookup"><span data-stu-id="1ee03-666">1</span></span>                                   | <span data-ttu-id="1ee03-667">valueC</span><span class="sxs-lookup"><span data-stu-id="1ee03-667">valueC</span></span>                              |
| <span data-ttu-id="1ee03-668">2</span><span class="sxs-lookup"><span data-stu-id="1ee03-668">2</span></span>                                   | <span data-ttu-id="1ee03-669">valueD</span><span class="sxs-lookup"><span data-stu-id="1ee03-669">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="1ee03-670">Provedor de Configuração personalizado</span><span class="sxs-lookup"><span data-stu-id="1ee03-670">Custom configuration provider</span></span>

<span data-ttu-id="1ee03-671">O aplicativo de exemplo demonstra como criar um provedor de configuração básico que lê os pares chave-valor da configuração de um banco de dados usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="1ee03-671">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="1ee03-672">O provedor tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="1ee03-672">The provider has the following characteristics:</span></span>

* <span data-ttu-id="1ee03-673">O banco de dados EF na memória é usado para fins de demonstração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-673">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="1ee03-674">Para usar um banco de dados que exija uma cadeia de conexão, implemente um `ConfigurationBuilder` secundário para fornecer a cadeia de conexão de outro provedor de configuração.</span><span class="sxs-lookup"><span data-stu-id="1ee03-674">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="1ee03-675">O provedor lê uma tabela de banco de dados na configuração na inicialização.</span><span class="sxs-lookup"><span data-stu-id="1ee03-675">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="1ee03-676">O provedor não consulta o banco de dados em uma base por chave.</span><span class="sxs-lookup"><span data-stu-id="1ee03-676">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="1ee03-677">O recarregamento na alteração não está implementado, portanto, a atualização do banco de dados após a inicialização do aplicativo não tem efeito sobre a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-677">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="1ee03-678">Defina uma entidade `EFConfigurationValue` para armazenar valores de configuração no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ee03-678">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="1ee03-679">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-679">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-680">Adicione um `EFConfigurationContext` para armazenar e acessar os valores configurados.</span><span class="sxs-lookup"><span data-stu-id="1ee03-680">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1ee03-681">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-681">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-682">Crie uma classe que implementa <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-682">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1ee03-683">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-683">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-684">Crie o provedor de configuração personalizado através da herança de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-684">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1ee03-685">O provedor de configuração inicializa o banco de dados quando ele está vazio.</span><span class="sxs-lookup"><span data-stu-id="1ee03-685">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="1ee03-686">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-686">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-687">Um método de extensão `AddEFConfiguration` permite adicionar a fonte de configuração a um `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-687">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1ee03-688">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-688">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1ee03-689">O código a seguir mostra como usar o `EFConfigurationProvider` personalizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ee03-689">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="1ee03-690">Acessar a configuração durante a inicialização</span><span class="sxs-lookup"><span data-stu-id="1ee03-690">Access configuration during startup</span></span>

<span data-ttu-id="1ee03-691">Injete `IConfiguration` no construtor `Startup` para acessar os valores de configuração em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1ee03-691">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1ee03-692">Para acessar a configuração em `Startup.Configure`, injete `IConfiguration` diretamente no método ou use a instância do construtor:</span><span class="sxs-lookup"><span data-stu-id="1ee03-692">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="1ee03-693">Para obter um exemplo de como acessar a configuração usando os métodos de conveniência de inicialização, consulte [Inicialização do aplicativo: métodos de conveniência](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="1ee03-693">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="1ee03-694">Acessar a configuração em uma página do Razor Pages ou exibição do MVC</span><span class="sxs-lookup"><span data-stu-id="1ee03-694">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="1ee03-695">Para acessar definições de configuração em uma página do Razor Pages ou uma exibição do MVC, adicione [usando diretiva](xref:mvc/views/razor#using) ([referência de C#: usando diretiva](/dotnet/csharp/language-reference/keywords/using-directive)) para o [namespace Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e injete <xref:Microsoft.Extensions.Configuration.IConfiguration> na página ou na exibição.</span><span class="sxs-lookup"><span data-stu-id="1ee03-695">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="1ee03-696">Em uma página do Razor:</span><span class="sxs-lookup"><span data-stu-id="1ee03-696">In a Razor Pages page:</span></span>

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

<span data-ttu-id="1ee03-697">Em uma exibição do MVC:</span><span class="sxs-lookup"><span data-stu-id="1ee03-697">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="1ee03-698">Adicionar configuração de um assembly externo</span><span class="sxs-lookup"><span data-stu-id="1ee03-698">Add configuration from an external assembly</span></span>

<span data-ttu-id="1ee03-699">Uma implementação <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite adicionar melhorias a um aplicativo durante a inicialização de um assembly externo fora da classe `Startup` do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ee03-699">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="1ee03-700">Para obter mais informações, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1ee03-700">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ee03-701">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1ee03-701">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="1ee03-702">Detalhes da configuração da Microsoft</span><span class="sxs-lookup"><span data-stu-id="1ee03-702">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
