---
title: Provedores de armazenamento de chaves
author: rick-anderson
description: Provedores de armazenamento de chaves
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 4f0e29a94593cd1cbb9890d7ee8bd09cddb4f69c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="key-storage-providers"></a>Provedores de armazenamento de chaves

<a name="data-protection-implementation-key-storage-providers"></a>

Por padrão, o sistema de proteção de dados [emprega uma heurística](xref:security/data-protection/configuration/default-settings) para determinar onde o material de chave de criptografia deve ser persistente. O desenvolvedor pode substituir a heurística e especificar manualmente o local.

> [!NOTE]
> Se você especificar um local de persistência de chave explícita, o sistema de proteção de dados irá cancelar o registro a criptografia de chave padrão no mecanismo de rest que a heurística fornecida, para que as chaves não serão criptografadas em repouso. É recomendável que você adicionalmente [especificar um mecanismo de criptografia de chave explícita](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) para aplicativos de produção.

O sistema de proteção de dados é fornecido com vários provedores de armazenamento de chaves da caixa de entrada.

## <a name="file-system"></a>Sistema de arquivos

Estimamos que muitos aplicativos usarão um repositório chave baseado no sistema de arquivos. Para configurar isso, chame o [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rotina de configuração, conforme mostrado abaixo. Forneça um `DirectoryInfo` apontando para o repositório onde as chaves devem ser armazenadas.

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

O `DirectoryInfo` pode apontar para um diretório no computador local ou ela pode apontar para uma pasta em um compartilhamento de rede. Se apontando para um diretório no computador local (e o cenário é que apenas os aplicativos no computador local precisa usar esse repositório), considere o uso de [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para criptografar as chaves em repouso. Caso contrário, considere o uso de um [certificado x. 509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para criptografar as chaves em repouso.

## <a name="azure-and-redis"></a>Azure e Redis

O `Microsoft.AspNetCore.DataProtection.AzureStorage` e `Microsoft.AspNetCore.DataProtection.Redis` pacotes permitem armazenar as chaves de proteção de dados no armazenamento do Azure ou um cache Redis. As chaves podem ser compartilhadas entre várias instâncias de um aplicativo web. Seu aplicativo ASP.NET Core pode compartilhar os cookies de autenticação ou proteção CSRF entre vários servidores. Para configurar o Azure, chame um do [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) sobrecargas conforme mostrado abaixo.

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

Consulte também o [código de teste do Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).

Para configurar o Redis, chame um do [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) sobrecargas conforme mostrado abaixo.

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

Para obter mais informações, consulte o seguinte:

- [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [Cache Redis do Azure](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- [Código de teste de redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).

## <a name="registry"></a>Registro

Às vezes, o aplicativo talvez não tenha acesso de gravação para o sistema de arquivos. Considere um cenário em que um aplicativo está em execução como uma conta de serviço virtual (como a identidade do pool de aplicativos do w3wp.exe). Nesses casos, o administrador pode ter provisionado uma chave do registro ACLed apropriado para a identidade da conta de serviço. Chamar o [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rotina de configuração, conforme mostrado abaixo. Forneça um `RegistryKey` apontando para o local onde os valores/chaves de criptografia deve ser armazenados.

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

Se você usar o registro do sistema como um mecanismo de persistência, considere o uso de [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para criptografar as chaves em repouso.

## <a name="custom-key-repository"></a>Repositório de chave personalizado

Se os mecanismos de caixa de entrada não forem apropriados, o desenvolvedor pode especificar seu próprio mecanismo de persistência de chave, fornecendo um personalizado `IXmlRepository`.
