# <a name="key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Aplicativo de exemplo do provedor de configuração do Cofre de chave (ASP.NET Core 1. x)

Este exemplo ilustra o uso do provedor de configuração do Cofre de chave do Azure para o ASP.NET Core 1. x. Para o exemplo do ASP.NET Core 2. x, consulte [aplicativo de exemplo do provedor de configuração do Cofre de chave (ASP.NET Core 2. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x).

> [!NOTE]
> O provedor de configuração não está disponível para o ASP.NET Core 1.0. Se você deseja implementar o provedor de configuração e o aplicativo é um aplicativo do ASP.NET Core 1.0, atualize o aplicativo pela primeira 1.1 ou posterior.

Para obter mais informações sobre como funciona a amostra, consulte o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration) tópico.

## <a name="using-the-sample"></a>Usando o exemplo
1. Criar um cofre de chaves e configurar o Azure Active Directory (AD do Azure) para o aplicativo seguindo as orientações em [Introdução ao Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Adicionar segredos no cofre de chaves usando o [AzureRM chave cofre PowerShell módulo](/powershell/module/azurerm.keyvault) disponíveis no [Galeria do PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), o [API de REST do Cofre de chaves do Azure](/rest/api/keyvault/), ou o [Portal do azure](https://portal.azure.com/). Os segredos são criados como *Manual* ou *certificado* segredos. *Certificado* segredos são certificados para uso por aplicativos e serviços, mas não são suportados pelo provedor de configuração. Você deve usar o *Manual* opção para criar os segredos do par nome-valor para uso com o provedor de configuração.
    * Segredos simples são criados como pares nome-valor. Nomes de segredo de Cofre de chaves do Azure são limitados a caracteres alfanuméricos e traços.
    * Usam valores hierárquicos (seções de configuração) `--` (dois traços) como um separador no exemplo. Dois-pontos, que normalmente são usadas para delimitar uma seção de uma subchave no [configuração do ASP.NET Core](xref:fundamentals/configuration), não são permitidos em nomes de segredo. Portanto, dois traços são usados e trocados para dois-pontos quando os segredos são carregados na configuração do aplicativo.
    * Criar dois *Manual* segredos com os seguintes pares de nome-valor. O segredo primeiro é um nome simples e um valor e o segredo do segundo cria um valor secreto com uma seção e uma subchave no nome do segredo:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Registre o aplicativo de exemplo no Active Directory do Azure.
  * Autorize o aplicativo para acessar o Cofre de chaves. Quando você usa o `Set-AzureRmKeyVaultAccessPolicy` cmdlet do PowerShell para autorizar o aplicativo para acessar o Cofre de chave, fornecer `List` e `Get` acesso para segredos com `-PermissionsToKeys list,get`.
2. Atualizar o aplicativo *appSettings. JSON* arquivo com os valores de `Vault`, `ClientId`, e `ClientSecret`.
3. Executar o aplicativo de exemplo, que obtém seus valores de configuração de `IConfigurationRoot` com o mesmo nome que o nome do segredo.
  * Valores não hierárquicos: O valor de `SecretName` é obtido com `config["SecretName"]`.
  * Valores hierárquicos (seções): Use `:` notação (vírgula) ou o `GetSection` método de extensão. Use qualquer uma dessas abordagens para obter o valor de configuração:
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`
