---
title: Provedor de configuração de Cofre de chaves do Azure no ASP.NET Core
author: guardrex
description: Saiba como usar o provedor de configuração do Cofre de chave do Azure para configurar um aplicativo usando pares de nome-valor carregados no tempo de execução.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 933f4fb1f2c1c412d318af5974cc9653805242ca
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927981"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Provedor de configuração de Cofre de chaves do Azure no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)

Este documento explica como usar o [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provedor de configuração para carregar valores de configuração de aplicativo de segredos do Cofre de chaves do Azure. O Azure Key Vault é um serviço baseado em nuvem que ajuda você a proteger chaves criptográficas e segredos usados por aplicativos e serviços. Cenários comuns incluem controlar o acesso aos dados de configuração confidenciais e atender o requisito para FIPS 140-2 nível 2 validados módulos de segurança de Hardware (HSM) ao armazenar dados de configuração. Esse recurso está disponível para aplicativos que usam o ASP.NET Core 1.1 ou superior.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="package"></a>Pacote

Para usar o provedor, adicione uma referência para o [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacote.

## <a name="app-configuration"></a>Configuração de aplicativo

Você pode explorar o provedor com o [aplicativos de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Depois de estabelecer um cofre de chaves e crie os segredos no cofre, os aplicativos de exemplo carregar os valores secretos em suas configurações e exibem-las em páginas da Web com segurança.

O provedor é adicionado à configuração do aplicativo com o `AddAzureKeyVault` extensão. Em aplicativos de exemplo, a extensão usa três valores de configuração carregados a partir de *appSettings. JSON* arquivo.

| Configuração de aplicativo    | Descrição                    | Exemplo                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nome do Cofre de chaves do Azure           | contosovault                                 |
| `ClientId`     | Id do aplicativo do Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Chave do aplicativo do Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Crie os segredos do Cofre de chaves e carregar valores de configuração (exemplo basic)

1. Criar um cofre de chaves e configurar o Azure Active Directory (Azure AD) para o aplicativo seguindo as orientações em [Introdução ao Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Adicionar segredos no cofre de chaves usando o [módulo do PowerShell do AzureRM chave cofre](/powershell/module/azurerm.keyvault) disponível na [Galeria do PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), o [API REST do Azure Key Vault](/rest/api/keyvault/), ou o [Portal do azure](https://portal.azure.com/). Segredos são criados como um *Manual* ou *certificado* segredos. *Certificado* segredos são certificados para uso por aplicativos e serviços, mas não são suportados pelo provedor de configuração. Você deve usar o *Manual* opção para criar os segredos de par nome-valor para uso com o provedor de configuração.
     * Segredos simples são criados como pares nome-valor. Nomes de segredos de Cofre de chaves do Azure são limitados a caracteres alfanuméricos e traços.
     * Usam valores hierárquicos (seções de configuração) `--` (dois traços) como um separador no exemplo. Dois-pontos, que normalmente são usadas para delimitar uma seção de uma subchave no [configuração do ASP.NET Core](xref:fundamentals/configuration/index), não são permitidos em nomes de segredos. Portanto, os dois traços são usados e trocados para dois-pontos quando os segredos são carregados para a configuração do aplicativo.
     * Criar duas *Manual* segredos com os seguintes pares de nome-valor. O segredo do primeiro é um nome simples e um valor, e o segredo do segundo cria um valor do segredo com uma seção e a subchave no nome do segredo:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registre o aplicativo de exemplo com o Azure Active Directory.
   * Autorize o aplicativo para acessar o Cofre de chaves. Quando você usa o `Set-AzureRmKeyVaultAccessPolicy` cmdlet do PowerShell para autorizar o aplicativo para acessar o Cofre de chaves, forneça `List` e `Get` acesso aos segredos com `-PermissionsToSecrets list,get`.

2. Atualizar o aplicativo *appSettings. JSON* arquivo com os valores de `Vault`, `ClientId`, e `ClientSecret`.
3. Executar o aplicativo de exemplo que obtém seus valores de configuração de `IConfigurationRoot` com o mesmo nome que o nome do segredo.
   * Valores não são hierárquicos: O valor para `SecretName` é obtido com `config["SecretName"]`.
   * Valores hierárquicos (seções): Use `:` notação (dois-pontos) ou o `GetSection` método de extensão. Use qualquer uma dessas abordagens para obter o valor de configuração:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Quando você executa o aplicativo, uma página da Web mostra os valores secretos carregados:

![Janela do navegador mostrando os valores secretos carregado via o provedor de configuração do Azure Key Vault](key-vault-configuration/_static/sample1.png)

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Segredos do Cofre de chaves prefixados de criar e carregar valores de configuração (chave-nome-prefixo-sample)

`AddAzureKeyVault` também fornece uma sobrecarga que aceita uma implementação de `IKeyVaultSecretManager`, que permite que você controle como os principais segredos do cofre são convertidas em chaves de configuração. Por exemplo, você pode implementar a interface para carregar os valores do segredo com base em um valor de prefixo que você fornece na inicialização do aplicativo. Isso permite que você, por exemplo, para carregar segredos de acordo com a versão do aplicativo.

> [!WARNING]
> Não usar prefixos de segredos do Cofre de chaves para colocar segredos para vários aplicativos no mesmo Cofre de chaves ou para colocar segredos ambientais (por exemplo, *development* versus *produção* segredos) no mesmo cofre. É recomendável que diferentes aplicativos e ambientes de desenvolvimento/produção usam cofres de chaves separados para isolar os ambientes de aplicativo para o nível mais alto de segurança.

Usando o segundo aplicativo de exemplo, criar um segredo no cofre de chaves para `5000-AppSecret` (períodos não são permitidos em nomes de segredos do Cofre de chaves) que representa um segredo do aplicativo para versão Version=5.0.0.0 do seu aplicativo. Para obter outra versão, 5.1.0.0, você cria um segredo para `5100-AppSecret`. Cada versão do aplicativo carrega seu próprio valor do segredo em sua configuração como `AppSecret`, remoção desativar a versão ele carrega o segredo. A implementação da amostra é mostrada abaixo:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

O `Load` método é chamado por um algoritmo de provedor que itera os segredos do cofre para encontrar aqueles que têm o prefixo de versão. Quando um prefixo de versão é encontrado com `Load`, o algoritmo usa o `GetKey` método para retornar o nome da configuração do nome do segredo. Ele ignora o prefixo de versão do nome do segredo e retorna o restante do nome do segredo para carregamento na configuração do aplicativo pares nome-valor.

Ao implementar essa abordagem:

1. Os segredos do Cofre de chaves são carregados.
2. O segredo de cadeia de caracteres para `5000-AppSecret` é correspondido.
3. A versão `5000` (com o traço) é removidos de deixar o nome da chave `AppSecret` carregar com o valor do segredo para a configuração do aplicativo.

> [!NOTE]
> Você também pode fornecer seus próprios `KeyVaultClient` implementação para `AddAzureKeyVault`. Fornecer um cliente personalizado permite que você compartilhe uma única instância do cliente entre o provedor de configuração e outras partes do seu aplicativo.

1. Criar um cofre de chaves e configurar o Azure Active Directory (Azure AD) para o aplicativo seguindo as orientações em [Introdução ao Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Adicionar segredos no cofre de chaves usando o [módulo do PowerShell do AzureRM chave cofre](/powershell/module/azurerm.keyvault) disponível na [Galeria do PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), o [API REST do Azure Key Vault](/rest/api/keyvault/), ou o [Portal do azure](https://portal.azure.com/). Segredos são criados como um *Manual* ou *certificado* segredos. *Certificado* segredos são certificados para uso por aplicativos e serviços, mas não são suportados pelo provedor de configuração. Você deve usar o *Manual* opção para criar os segredos de par nome-valor para uso com o provedor de configuração.
     * Usam valores hierárquicos (seções de configuração) `--` (dois traços) como um separador.
     * Criar duas *Manual* segredos com os seguintes pares de nome-valor:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registre o aplicativo de exemplo com o Azure Active Directory.
   * Autorize o aplicativo para acessar o Cofre de chaves. Quando você usa o `Set-AzureRmKeyVaultAccessPolicy` cmdlet do PowerShell para autorizar o aplicativo para acessar o Cofre de chaves, forneça `List` e `Get` acesso aos segredos com `-PermissionsToSecrets list,get`.

2. Atualizar o aplicativo *appSettings. JSON* arquivo com os valores de `Vault`, `ClientId`, e `ClientSecret`.
3. Executar o aplicativo de exemplo que obtém seus valores de configuração de `IConfigurationRoot` com o mesmo nome que o nome do segredo prefixado. Neste exemplo, o prefixo é a versão do aplicativo, que você forneceu para o `PrefixKeyVaultSecretManager` quando você adicionou o provedor de configuração do Azure Key Vault. O valor para `AppSecret` é obtido com `config["AppSecret"]`. A página da Web gerada pelo aplicativo mostra o valor carregado:

   ![Janela do navegador mostrando um valor do segredo carregados por meio do provedor de configuração da chave cofre do Azure, quando a versão do aplicativo é Version=5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Alterar a versão do assembly no arquivo de projeto de aplicativo `5.0.0.0` para `5.1.0.0` e execute o aplicativo novamente. Esse tempo, o valor do segredo retornado é `5.1.0.0_secret_value`. A página da Web gerada pelo aplicativo mostra o valor carregado:

   ![Janela do navegador mostrando um valor do segredo carregados por meio do provedor de configuração da chave cofre do Azure, quando a versão do aplicativo é 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>Controlar o acesso para o segredo do cliente

Use o [ferramenta Secret Manager](xref:security/app-secrets) para manter o `ClientSecret` fora de sua árvore de origem do projeto. Com o Secret Manager, você associar a um projeto específico de segredos do aplicativo e compartilhá-los em vários projetos.

Ao desenvolver um aplicativo do .NET Framework em um ambiente que dá suporte a certificados, você pode autenticar para o Azure Key Vault com um certificado X.509. Chave privada do certificado X.509 é gerenciado pelo sistema operacional. Para obter mais informações, consulte [autenticar com um certificado em vez de um segredo do cliente](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Use o `AddAzureKeyVault` sobrecarga que aceita um `X509Certificate2` (`_env` no exemplo a seguir:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>Recarregue os segredos

Segredos são armazenados em cache até `IConfigurationRoot.Reload()` é chamado. Expirado, desabilitado, e atualizados segredos no cofre de chaves não serão respeitados pelo aplicativo até `Reload` é executado.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Segredos desabilitados e expirados

Segredos desabilitados e expirados lançar um `KeyVaultClientException`. Para impedir que seu aplicativo acione, substitua o seu aplicativo ou atualizar o segredo desabilitado/expirado.

## <a name="troubleshoot"></a>Solução de problemas

Quando o aplicativo falha ao carregar a configuração usando o provedor, uma mensagem de erro é gravada para o [infra-estrutura de log do ASP.NET Core](xref:fundamentals/logging/index). As seguintes condições impedirá que a configuração de carregamento:

* O aplicativo não está configurado corretamente no Azure Active Directory.
* O Cofre de chaves não existe no Azure Key Vault.
* O aplicativo não está autorizado a acessar o Cofre de chaves.
* A política de acesso não inclui `Get` e `List` permissões.
* No cofre de chaves, os dados de configuração (par nome-valor) estão nomeados incorretamente, ausente, desabilitado ou expirado.
* O aplicativo tem o nome do cofre da chave incorreta (`Vault`), Id do aplicativo do Azure AD (`ClientId`), ou a chave do Azure AD (`ClientSecret`).
* A chave do AD do Azure (`ClientSecret`) expirou.
* A chave de configuração (nome) está incorreta no aplicativo para o valor que você está tentando carregar.

## <a name="additional-resources"></a>Recursos adicionais

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Cofre de chaves](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Documentação de Cofre de chaves](/azure/key-vault/)
* [Como gerar e transferir protegida por HSM chaves para o Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Classe KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
