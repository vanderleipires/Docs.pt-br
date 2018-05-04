---
title: Provedor de configuração do Cofre de chaves do Azure no núcleo do ASP.NET
author: guardrex
description: Saiba como usar o provedor de configuração do Cofre de chave do Azure para configurar um aplicativo usando pares de nome-valor no tempo de execução.
manager: wpickett
ms.author: riande
ms.date: 08/09/2017
ms.prod: asp.net-core
ms.topic: article
uid: security/key-vault-configuration
ms.openlocfilehash: 78a00e04e260863af17d7888ca6bf77d3f915ce1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Provedor de configuração do Cofre de chaves do Azure no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-enfermeiro](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Exibir ou baixar o código de exemplo para 2. x:

* [Exemplo básico](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([como baixar](xref:tutorials/index#how-to-download-a-sample))-lê valores secretos em um aplicativo.
* [Exemplo de prefixo do nome da chave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) - leituras valores secretos usando um prefixo de nome da chave que representa a versão de um aplicativo, que permite que você carregue um conjunto diferente de valores secretos para cada versão do aplicativo.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Exibir ou baixar o exemplo de código 1. x:

* [Exemplo básico](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([como baixar](xref:tutorials/index#how-to-download-a-sample))-lê valores secretos em um aplicativo.
* [Exemplo de prefixo do nome da chave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) - leituras valores secretos usando um prefixo de nome da chave que representa a versão de um aplicativo, que permite que você carregue um conjunto diferente de valores secretos para cada versão do aplicativo. 

---

Este documento explica como usar o [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provedor de configuração para carregar valores de configuração do aplicativo de segredos de Cofre de chaves do Azure. Cofre de chaves do Azure é um serviço baseado em nuvem que ajuda a proteger as chaves de criptografia e segredos usados por aplicativos e serviços. Cenários comuns incluem a controlar o acesso aos dados de configuração confidenciais e atendem ao requisito para FIPS 140-2 nível 2 validados módulos de segurança de Hardware (HSM) ao armazenar os dados de configuração. Esse recurso está disponível para aplicativos voltados para ASP.NET Core 1.1 ou posterior.

## <a name="package"></a>Pacote
Para usar o provedor, adicione uma referência para o [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacote.

## <a name="application-configuration"></a>Configuração do aplicativo
Você pode explorar o provedor com o [aplicativos de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Depois de estabelecer um cofre de chaves e criar segredos no cofre, os aplicativos de amostra carregar os valores de segredo em suas configurações e exibem-los em páginas da Web com segurança.

O provedor é adicionado para o `ConfigurationBuilder` com o `AddAzureKeyVault` extensão. Os aplicativos de exemplo, a extensão usa três valores de configuração carregados a partir de *appSettings. JSON* arquivo.

| Configuração do aplicativo    | Descrição                    | Exemplo                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nome do Cofre de chaves do Azure           | contosovault                                 |
| `ClientId`     | Id de aplicativo do Active Directory do Azure  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Chave de aplicativo do Active Directory do Azure | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Criando segredos de Cofre de chaves e carregar valores de configuração (exemplo basic)
1. Criar um cofre de chaves e configurar o Azure Active Directory (AD do Azure) para o aplicativo seguindo as orientações em [Introdução ao Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Adicionar segredos no cofre de chaves usando o [AzureRM chave cofre PowerShell módulo](/powershell/module/azurerm.keyvault) disponíveis no [Galeria do PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), o [API de REST do Cofre de chaves do Azure](/rest/api/keyvault/), ou o [Portal do azure](https://portal.azure.com/). Os segredos são criados como *Manual* ou *certificado* segredos. *Certificado* segredos são certificados para uso por aplicativos e serviços, mas não são suportados pelo provedor de configuração. Você deve usar o *Manual* opção para criar os segredos do par nome-valor para uso com o provedor de configuração.
     * Segredos simples são criados como pares nome-valor. Nomes de segredo de Cofre de chaves do Azure são limitados a caracteres alfanuméricos e traços.
     * Usam valores hierárquicos (seções de configuração) `--` (dois traços) como um separador no exemplo. Dois-pontos, que normalmente são usadas para delimitar uma seção de uma subchave no [configuração do ASP.NET Core](xref:fundamentals/configuration/index), não são permitidos em nomes de segredo. Portanto, dois traços são usados e trocados para dois-pontos quando os segredos são carregados na configuração do aplicativo.
     * Criar dois *Manual* segredos com os seguintes pares de nome-valor. O segredo primeiro é um nome simples e um valor e o segredo do segundo cria um valor secreto com uma seção e uma subchave no nome do segredo:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registre o aplicativo de exemplo no Active Directory do Azure.
   * Autorize o aplicativo para acessar o Cofre de chaves. Quando você usa o `Set-AzureRmKeyVaultAccessPolicy` cmdlet do PowerShell para autorizar o aplicativo para acessar o Cofre de chave, fornecer `List` e `Get` acesso para segredos com `-PermissionsToSecrets list,get`.

2. Atualizar o aplicativo *appSettings. JSON* arquivo com os valores de `Vault`, `ClientId`, e `ClientSecret`.
3. Executar o aplicativo de exemplo, que obtém seus valores de configuração de `IConfigurationRoot` com o mesmo nome que o nome do segredo.
   * Valores não hierárquicos: O valor de `SecretName` é obtido com `config["SecretName"]`.
   * Valores hierárquicos (seções): Use `:` notação (vírgula) ou o `GetSection` método de extensão. Use qualquer uma dessas abordagens para obter o valor de configuração:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Quando você executa o aplicativo, uma página da Web mostra os valores de segredo carregados:

![Janela do navegador mostrando valores secretos carregados por meio do provedor de configuração da chave de cofre do Azure](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Criar Cofre de chaves prefixados segredos e carregar valores de configuração (chave de nome-prefixo-amostra)
`AddAzureKeyVault` também fornece uma sobrecarga que aceita uma implementação de `IKeyVaultSecretManager`, que permite que você controle como chave segredos do cofre são convertidos em chaves de configuração. Por exemplo, você pode implementar a interface para carregar valores secretos com base em um valor de prefixo que você fornece durante a inicialização do aplicativo. Isso permite que você, por exemplo, para carregar os segredos com base na versão do aplicativo.

> [!WARNING]
> Não use prefixos em segredos de Cofre de chaves para colocar os segredos para vários aplicativos no mesmo Cofre de chaves ou colocar segredos ambientais (por exemplo, *desenvolvimento* versus *produção* segredos) no mesmo cofre. É recomendável que diferentes aplicativos e ambientes de desenvolvimento/produção usam cofres chave separados para isolar os ambientes de aplicativo para o nível mais alto de segurança.

Usando o segundo aplicativo de exemplo, criar um segredo no cofre de chaves para `5000-AppSecret` (períodos não são permitidos em nomes de Cofre de chave secreta) que representa um segredo do aplicativo para a versão 5.0.0.0 do seu aplicativo. Para outra versão, 5.1.0.0, você cria um segredo para `5100-AppSecret`. Cada versão do aplicativo carrega seu próprio valor secreto em sua configuração como `AppSecret`, remoção desativar a versão que ele carrega o segredo. Implementação do exemplo é mostrada abaixo:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

O `Load` método é chamado por um algoritmo de provedor que itera os segredos do cofre para localizar os que têm o prefixo de versão. Quando um prefixo de versão é encontrado com `Load`, o algoritmo usa o `GetKey` método para retornar o nome da configuração do nome do segredo. Ele ignora o prefixo de versão do nome do segredo e retorna o restante do nome do segredo para carregar a configuração do aplicativo pares nome-valor.

Quando você implementar essa abordagem:

1. Os segredos do Cofre de chaves são carregados.
2. O segredo de cadeia de caracteres para `5000-AppSecret` for correspondida.
3. A versão `5000` (com o traço) é removidos de deixar o nome da chave `AppSecret` para carregar com o valor de segredo na configuração do aplicativo.

> [!NOTE]
> Você também pode fornecer sua própria `KeyVaultClient` implementação `AddAzureKeyVault`. Fornecer um cliente personalizado permite que você compartilhe uma única instância do cliente entre o provedor de configuração e outras partes do seu aplicativo.

1. Criar um cofre de chaves e configurar o Azure Active Directory (AD do Azure) para o aplicativo seguindo as orientações em [Introdução ao Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Adicionar segredos no cofre de chaves usando o [AzureRM chave cofre PowerShell módulo](/powershell/module/azurerm.keyvault) disponíveis no [Galeria do PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), o [API de REST do Cofre de chaves do Azure](/rest/api/keyvault/), ou o [Portal do azure](https://portal.azure.com/). Os segredos são criados como *Manual* ou *certificado* segredos. *Certificado* segredos são certificados para uso por aplicativos e serviços, mas não são suportados pelo provedor de configuração. Você deve usar o *Manual* opção para criar os segredos do par nome-valor para uso com o provedor de configuração.
     * Usam valores hierárquicos (seções de configuração) `--` (dois traços) como separador.
     * Criar dois *Manual* segredos com os seguintes pares de nome-valor:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registre o aplicativo de exemplo no Active Directory do Azure.
   * Autorize o aplicativo para acessar o Cofre de chaves. Quando você usa o `Set-AzureRmKeyVaultAccessPolicy` cmdlet do PowerShell para autorizar o aplicativo para acessar o Cofre de chave, fornecer `List` e `Get` acesso para segredos com `-PermissionsToSecrets list,get`.

2. Atualizar o aplicativo *appSettings. JSON* arquivo com os valores de `Vault`, `ClientId`, e `ClientSecret`.
3. Executar o aplicativo de exemplo, que obtém seus valores de configuração de `IConfigurationRoot` com o mesmo nome que o nome do segredo prefixado. Neste exemplo, o prefixo é a versão do aplicativo, o que você forneceu para o `PrefixKeyVaultSecretManager` quando você adicionou o provedor de configuração do Cofre de chaves do Azure. O valor de `AppSecret` é obtido com `config["AppSecret"]`. A página da Web gerada pelo aplicativo mostra o valor carregado:

   ![Janela do navegador mostrando um valor secreto carregado por meio do provedor de configuração da chave de cofre do Azure quando a versão do aplicativo é 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Alterar a versão do assembly no arquivo de projeto do aplicativo `5.0.0.0` para `5.1.0.0` e execute o aplicativo novamente. Neste momento, o valor de segredo retornado é `5.1.0.0_secret_value`. A página da Web gerada pelo aplicativo mostra o valor carregado:

   ![Janela do navegador mostrando um valor secreto carregado por meio do provedor de configuração da chave de cofre do Azure quando a versão do aplicativo é 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Controlando o acesso para o ClientSecret
Use o [ferramenta Gerenciador de segredo](xref:security/app-secrets) para manter o `ClientSecret` fora da árvore de origem do projeto. Com o Gerenciador de segredo, que você associe os segredos do aplicativo um projeto específico e compartilhá-los em vários projetos.

Ao desenvolver um aplicativo do .NET Framework em um ambiente que oferece suporte a certificados, você pode autenticar para o Cofre de chaves do Azure com um certificado x. 509. Chave privada do certificado x. 509 é gerenciado pelo sistema operacional. Para obter mais informações, consulte [autenticar com um certificado em vez de um segredo do cliente](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Use o `AddAzureKeyVault` sobrecarga que aceita um `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Recarregar segredos
Os segredos são armazenados em cache até `IConfigurationRoot.Reload()` é chamado. Expirado, desabilitado, e atualizados segredos no cofre de chaves não são respeitados pelo aplicativo até que `Reload` é executado.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Segredos desabilitados e expirados
Segredos desabilitados e expirados lançam um `KeyVaultClientException`. Para impedir que seu aplicativo lançando, substitua o seu aplicativo ou atualizar o segredo desabilitado/expirado.

## <a name="troubleshooting"></a>Solução de problemas
Quando o aplicativo falhar ao carregar a configuração usando o provedor, uma mensagem de erro é gravada para o [infraestrutura ASP.NET log](xref:fundamentals/logging/index). As seguintes condições impedirá que a configuração de carregamento:
* O aplicativo não está configurado corretamente no Active Directory do Azure.
* O Cofre de chaves não existe no cofre de chaves do Azure.
* O aplicativo não está autorizado a acessar o Cofre de chaves.
* A política de acesso não inclui `Get` e `List` permissões.
* No cofre de chaves, os dados de configuração (par de nome-valor) são nomeados incorretamente, ausentes, desabilitado ou expirou.
* O aplicativo tem o nome do Cofre de chave incorreta (`Vault`), Id de aplicativo do Azure AD (`ClientId`), ou a chave do Azure AD (`ClientSecret`).
* A chave do AD do Azure (`ClientSecret`) expirou.
* A chave de configuração (nome) está incorreta no aplicativo para o valor que você está tentando carregar.

## <a name="additional-resources"></a>Recursos adicionais
* [Configuração](xref:fundamentals/configuration/index)
* [Microsoft Azure: Cofre de chaves](https://azure.microsoft.com/services/key-vault/)
* [Do Microsoft Azure: Documentação do Cofre de chaves](https://docs.microsoft.com/azure/key-vault/)
* [Chaves de como gerar e transferir protegida por HSM do Cofre de chaves do Azure](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [Classe KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
