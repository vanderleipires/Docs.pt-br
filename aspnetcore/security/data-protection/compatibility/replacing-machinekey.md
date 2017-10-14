---
title: Substituindo '< machineKey >' no ASP.NET
author: rick-anderson
description: Substituindo '< machineKey >' no ASP.NET
keywords: "ASP.NET Core, segurança, < machineKey > machineKey"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2c2f6e04c42d547592cc1c2e6e66da7133b8bbef
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="replacing-machinekey-in-aspnet"></a><span data-ttu-id="0217d-104">Substituindo `<machineKey>` no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0217d-104">Replacing `<machineKey>` in ASP.NET</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="0217d-105">A implementação de `<machineKey>` elemento no ASP.NET [é substituível](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="0217d-105">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="0217d-106">Isso permite que a maioria das chamadas para rotinas criptográficas ASP.NET para ser roteada por meio de um mecanismo de proteção de dados de substituição, incluindo o novo sistema de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="0217d-106">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="0217d-107">Instalação do pacote</span><span class="sxs-lookup"><span data-stu-id="0217d-107">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="0217d-108">O novo sistema de proteção de dados só pode ser instalado em um aplicativo ASP.NET existente direcionado ao .NET 4.5.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="0217d-108">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="0217d-109">Instalação irá falhar se o aplicativo tem como destino .NET 4.5 ou inferior.</span><span class="sxs-lookup"><span data-stu-id="0217d-109">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="0217d-110">Para instalar o novo sistema de proteção de dados em um projeto de 4.5.1+ ASP.NET existente, instale o pacote Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="0217d-110">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="0217d-111">Isso criará uma instância de sistema de proteção de dados usando o [configuração padrão](../configuration/default-settings.md#data-protection-default-settings) configurações.</span><span class="sxs-lookup"><span data-stu-id="0217d-111">This will instantiate the data protection system using the [default configuration](../configuration/default-settings.md#data-protection-default-settings) settings.</span></span>

<span data-ttu-id="0217d-112">Quando você instala o pacote, ele insere uma linha em *Web. config* que diz ao ASP.NET para usá-la para [mais operações criptográficas](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), incluindo autenticação de formulários, o estado de exibição e chamadas para Protect.</span><span class="sxs-lookup"><span data-stu-id="0217d-112">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="0217d-113">A linha é inserida lê da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="0217d-113">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="0217d-114">Você pode determinar se o novo sistema de proteção de dados está ativo inspecionando campos como `__VIEWSTATE`, que deve começar com "CfDJ8" como no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="0217d-114">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="0217d-115">"CfDJ8" é a representação de base64 do cabeçalho magic "09 F0 C9 F0" que identifica um conteúdo protegido pelo sistema de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="0217d-115">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="0217d-116">Configuração de pacote</span><span class="sxs-lookup"><span data-stu-id="0217d-116">Package configuration</span></span>

<span data-ttu-id="0217d-117">O sistema de proteção de dados é instanciado com uma configuração de zero a instalação padrão.</span><span class="sxs-lookup"><span data-stu-id="0217d-117">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="0217d-118">No entanto, uma vez que por padrão as chaves são mantidas para o sistema de arquivos local, isso não funcionará para aplicativos que são implantados em um farm.</span><span class="sxs-lookup"><span data-stu-id="0217d-118">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="0217d-119">Para resolver esse problema, você pode fornecer uma configuração por meio da criação de um tipo que herda DataProtectionStartup e substitui o método ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="0217d-119">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="0217d-120">Abaixo está um exemplo de um tipo de inicialização de proteção de dados personalizados que configurado onde as chaves são persistentes e como eles são criptografados em repouso.</span><span class="sxs-lookup"><span data-stu-id="0217d-120">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="0217d-121">Ela também substitui a política de isolamento de aplicativo padrão, fornecendo seu próprio nome de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0217d-121">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="0217d-122">Você também pode usar `<machineKey applicationName="my-app" ... />` no lugar de uma chamada explícita para SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="0217d-122">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="0217d-123">Esse é um mecanismo de conveniência para evitar forçar o desenvolvedor para criar um tipo derivado de DataProtectionStartup se todos os quisessem configurar foi definindo o nome do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0217d-123">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="0217d-124">Para habilitar essa configuração personalizada, volte para a Web. config e procure o `<appSettings>` elemento que instala o pacote adicionado ao arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="0217d-124">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="0217d-125">Ele se parecerá com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="0217d-125">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="0217d-126">Preencha o valor em branco com o nome qualificado do assembly do tipo derivado DataProtectionStartup que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="0217d-126">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="0217d-127">Se o nome do aplicativo é DataProtectionDemo, isso seria semelhante a abaixo.</span><span class="sxs-lookup"><span data-stu-id="0217d-127">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="0217d-128">O sistema de proteção de dados recém-configurada agora está pronto para uso dentro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0217d-128">The newly-configured data protection system is now ready for use inside the application.</span></span>
