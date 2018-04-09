# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Aplicativo de exemplo do provedor de configuração do Cofre de chaves do prefixo (ASP.NET Core 1. x)

Este exemplo ilustra o uso do provedor de configuração do Cofre de chave do Azure para o ASP.NET Core 1. x usando prefixos de nome da chave. Para o exemplo do ASP.NET Core 2. x, consulte [aplicativo de exemplo do provedor de configuração do Cofre de chave do prefixo (ASP.NET Core 2. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x).

> [!NOTE]
> O provedor de configuração não está disponível para o ASP.NET Core 1.0. Se você deseja implementar o provedor de configuração e o aplicativo é um aplicativo do ASP.NET Core 1.0, atualize o aplicativo pela primeira 1.1 ou posterior.

Para obter mais informações sobre como funciona a amostra, consulte o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration) tópico.

## <a name="using-the-sample"></a>Usando o exemplo
1. Criar um cofre de chaves e configurar o Azure Active Directory (AD do Azure) para o aplicativo seguindo as orientações em [Introdução ao Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Adicione segredos no cofre de chaves usando o módulo do PowerShell do Azure, a API de gerenciamento do Azure ou o Portal do Azure. Os segredos são criados como *Manual* ou *certificado* segredos. *Certificado* segredos são certificados para uso por aplicativos e serviços, mas não são suportados pelo provedor de configuração. Você deve usar o *Manual* opção para criar os segredos do par nome-valor para uso com o provedor de configuração.
     * Usam valores hierárquicos (seções de configuração) `--` (dois traços) como separador.
     * Para o aplicativo de exemplo, crie dois *Manual* segredos com os seguintes pares de nome-valor:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registre o aplicativo de exemplo no Active Directory do Azure.
   * Autorize o aplicativo para acessar o Cofre de chaves. Quando você usa o `Set-AzureRmKeyVaultAccessPolicy` cmdlet do PowerShell para autorizar o aplicativo para acessar o Cofre de chave, fornecer `List` e `Get` acesso para segredos com `-PermissionsToSecrets list,get`.

2. Atualizar o aplicativo *appSettings. JSON* arquivo com os valores de `Vault`, `ClientId`, e `ClientSecret`.
3. Executar o aplicativo de exemplo, que obtém seus valores de configuração de `IConfigurationRoot` com o mesmo nome que o nome do segredo prefixado. Neste exemplo, o prefixo é a versão do aplicativo, o que você forneceu para o `PrefixKeyVaultSecretManager` quando você adicionou o provedor de configuração do Cofre de chaves do Azure. O valor de `AppSecret` é obtido com `config["AppSecret"]`.
4. Alterar a versão do assembly no arquivo de projeto do aplicativo `5.0.0.0` para `5.1.0.0` e execute o aplicativo novamente. Neste momento, o valor de segredo retornado é `5.1.0.0_secret_value`.
