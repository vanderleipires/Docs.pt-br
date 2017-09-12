---
title: Substituindo '< machineKey >' no ASP.NET
author: rick-anderson
description: Substituindo '< machineKey >' no ASP.NET
keywords: "ASP.NET Core, segurança, '< machineKey >'"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 29229b9507ece6aff8278b0ad66169c9e4e7498b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="replacing-machinekey-in-aspnet"></a>Substituindo `<machineKey>` no ASP.NET

<a name=compatibility-replacing-machinekey></a>

A implementação de `<machineKey>` elemento no ASP.NET [é substituível](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Isso permite que a maioria das chamadas para rotinas criptográficas ASP.NET para ser roteada por meio de um mecanismo de proteção de dados de substituição, incluindo o novo sistema de proteção de dados.

## <a name="package-installation"></a>Instalação do pacote

> [!NOTE]
> O novo sistema de proteção de dados só pode ser instalado em um aplicativo ASP.NET existente direcionado ao .NET 4.5.1 ou posterior. Instalação irá falhar se o aplicativo tem como destino .NET 4.5 ou inferior.

Para instalar o novo sistema de proteção de dados em um projeto de 4.5.1+ ASP.NET existente, instale o pacote Microsoft.AspNetCore.DataProtection.SystemWeb. Isso criará uma instância de sistema de proteção de dados usando o [configuração padrão](../configuration/default-settings.md#data-protection-default-settings) configurações.

Quando você instala o pacote, ele insere uma linha em *Web. config* que diz ao ASP.NET para usá-la para [mais operações criptográficas](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), incluindo autenticação de formulários, o estado de exibição e chamadas para Protect. A linha é inserida lê da seguinte maneira.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Você pode determinar se o novo sistema de proteção de dados está ativo inspecionando campos como `__VIEWSTATE`, que deve começar com "CfDJ8" como no exemplo a seguir. "CfDJ8" é a representação de base64 do cabeçalho magic "09 F0 C9 F0" que identifica um conteúdo protegido pelo sistema de proteção de dados.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Configuração de pacote

O sistema de proteção de dados é instanciado com uma configuração de zero a instalação padrão. No entanto, uma vez que por padrão as chaves são mantidas para o sistema de arquivos local, isso não funcionará para aplicativos que são implantados em um farm. Para resolver esse problema, você pode fornecer uma configuração por meio da criação de um tipo que herda DataProtectionStartup e substitui o método ConfigureServices.

Abaixo está um exemplo de um tipo de inicialização de proteção de dados personalizados que configurado onde as chaves são persistentes e como eles são criptografados em repouso. Ela também substitui a política de isolamento de aplicativo padrão, fornecendo seu próprio nome de aplicativo.

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
> Você também pode usar `<machineKey applicationName="my-app" ... />` no lugar de uma chamada explícita para SetApplicationName. Esse é um mecanismo de conveniência para evitar forçar o desenvolvedor para criar um tipo derivado de DataProtectionStartup se todos os quisessem configurar foi definindo o nome do aplicativo.

Para habilitar essa configuração personalizada, volte para a Web. config e procure o `<appSettings>` elemento que instala o pacote adicionado ao arquivo de configuração. Ele se parecerá com a seguinte marcação:

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

Preencha o valor em branco com o nome qualificado do assembly do tipo derivado DataProtectionStartup que você acabou de criar. Se o nome do aplicativo é DataProtectionDemo, isso seria semelhante a abaixo.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

O sistema de proteção de dados recém-configurada agora está pronto para uso dentro do aplicativo.
