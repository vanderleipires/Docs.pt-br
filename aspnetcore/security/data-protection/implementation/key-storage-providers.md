---
title: Provedores de armazenamento de chaves no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre provedores de armazenamento de chaves no ASP.NET Core e como configurar locais de armazenamento de chaves.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0e64a65ab1d65efa9f2e4d36a23663b607f206d7
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402062"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Provedores de armazenamento de chaves no ASP.NET Core

O sistema de proteção de dados [emprega um mecanismo de descoberta por padrão](xref:security/data-protection/configuration/default-settings) para determinar onde as chaves de criptografia devem ser persistente. O desenvolvedor pode substituir o mecanismo de descoberta padrão e especificar manualmente o local.

> [!WARNING]
> Se você especificar um local de persistência de chave explícita, o sistema de proteção de dados cancela o registro a criptografia de chave padrão no mecanismo de rest, portanto, as chaves não são criptografadas em repouso. É recomendável que você adicionalmente [especificar um mecanismo de criptografia de chave explícita](xref:security/data-protection/implementation/key-encryption-at-rest) para implantações de produção.

## <a name="file-system"></a>Sistema de arquivos

Para configurar um repositório chave baseadas no sistema de arquivos, chame o [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) rotina de configuração, conforme mostrado abaixo. Fornecer um [DirectoryInfo](/dotnet/api/system.io.directoryinfo) apontando para o repositório em que as chaves devem ser armazenadas:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

O `DirectoryInfo` pode apontar para um diretório no computador local, ou ele pode apontar para uma pasta em um compartilhamento de rede. Se apontando para um diretório no computador local (e o cenário é que apenas os aplicativos no computador local exigem acesso para usar esse repositório), considere o uso de [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (no Windows) para criptografar as chaves em repouso. Caso contrário, considere o uso de um [certificado x. 509](xref:security/data-protection/implementation/key-encryption-at-rest) para criptografar as chaves em repouso.

## <a name="azure-and-redis"></a>O Azure e Redis

O [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) pacotes permitem armazenar chaves de proteção de dados no armazenamento do Azure ou um cache Redis. As chaves podem ser compartilhadas entre várias instâncias de um aplicativo web. Aplicativos podem compartilhar cookies de autenticação ou proteção CSRF em vários servidores. Para configurar o provedor de armazenamento de BLOBs do Azure, chame um dos [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) sobrecargas:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Para configurar o Redis, chame um dos [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) sobrecargas:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

Para mais informações, consulte os seguintes tópicos:

* [ConnectionMultiplexer stackexchange. Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Cache Redis do Azure](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [exemplos de ASPNET/DataProtection](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>Registro

**Só se aplica às implantações do Windows.**

Às vezes, o aplicativo pode não ter acesso de gravação para o sistema de arquivos. Considere um cenário em que um aplicativo é executado como uma conta de serviço virtual (como *w3wp.exe*da identidade do pool de aplicativo). Nesses casos, o administrador pode provisionar uma chave do registro que seja acessível pela identidade da conta de serviço. Chame o [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) método de extensão, conforme mostrado abaixo. Fornecer um [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) apontando para o local onde as chaves de criptografia devem ser armazenadas:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> É recomendável usar [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) para criptografar as chaves em repouso.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

O [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pacote fornece um mecanismo para armazenar chaves de proteção de dados para um banco de dados usando o Entity Framework Core. O `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` pacote do NuGet deve ser adicionado ao arquivo de projeto, ele não é parte do [metapacote Microsoft](xref:fundamentals/metapackage-app).

Com esse pacote, as chaves podem ser compartilhadas entre várias instâncias de um aplicativo web.

Para configurar o provedor do EF Core, chame o [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) método:

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

O parâmetro genérico, `TContext`, deve herdar de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a>Repositório de chave personalizado

Se os mecanismos de caixa de entrada não forem apropriados, o desenvolvedor pode especificar seu próprio mecanismo de persistência de chave, fornecendo um personalizado [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
