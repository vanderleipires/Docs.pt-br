---
uid: config-builder
title: Construtores de configuração para o ASP.NET
author: rick-anderson
description: Saiba como obter dados de configuração de fontes diferentes valores de Web. config de fontes externas.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: d5a3916c3df9778d14be80342bafbc3456a69a03
ms.sourcegitcommit: f2d14a7518d6ee51aca9333818ac1276e7b5ecef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134529"
---
# <a name="configuration-builders-for-aspnet"></a>Construtores de configuração para o ASP.NET

Por [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Construtores de configuração fornecem um mecanismo ágil e modernos para aplicativos ASP.NET obtenham os valores de configuração de fontes externas.

Construtores de configuração:

* Estão disponíveis no .NET Framework 4.7.1 e posterior.
* Fornece um mecanismo flexível para ler valores de configuração.
* Aborda algumas das necessidades básicas de aplicativos, como eles se movam em um contêiner e com foco do ambiente de nuvem.
* Pode ser usado para melhorar a proteção de dados de configuração pelo desenho de fontes não está disponíveis anteriormente (por exemplo, variáveis de ambiente e o Azure Key Vault) no sistema de configuração do .NET.

## <a name="keyvalue-configuration-builders"></a>Construtores de configuração de chave/valor

Um cenário comum que pode ser tratado por construtores de configuração é fornecer um mecanismo de substituição de chave/valor básico para seções de configuração que seguem um padrão de chave/valor. O conceito do .NET Framework de ConfigurationBuilders não está limitado a seções de configuração específico ou padrões. No entanto, muitos dos construtores de configuração no `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) funcionam dentro do padrão de chave/valor.

## <a name="keyvalue-configuration-builders-settings"></a>Definições de construtores de configuração de chave/valor

As configurações a seguir se aplicam a todos os construtores de configuração de chave/valor no `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modo

Os construtores de configuração usam uma fonte externa de informações de chave/valor para preencher os elementos de chave/valor selecionado de sistema de configuração. Especificamente, o `<appSettings/>` e `<connectionStrings/>` seções recebem tratamento especial de construtores de configuração. Os construtores de trabalho em três modos:

* `Strict` -O modo padrão. Nesse modo, o construtor de configuração só funciona em seções de configuração de chave/valor-centrado em bem conhecidos. `Strict` modo enumera cada chave na seção. Se uma chave correspondente for encontrada na origem externa:

   * Os construtores de configuração substitua o valor na seção de configuração resultante com o valor da fonte externa.
* `Greedy` -Esse modo está intimamente relacionado ao `Strict` modo. Em vez de ficar limitado às chaves que já existem na configuração original:

  * Os construtores de configuração adiciona todos os pares de chave/valor da fonte externa para a seção de configuração resultante.

* `Expand` -Opera em XML brutos antes que ele é analisado em um objeto da seção de configuração. Ele pode ser pensado como uma expansão de tokens em uma cadeia de caracteres. Qualquer parte da cadeia de caracteres XML brutos que corresponde ao padrão de `${token}` é um candidato para expansão de token. Se nenhum valor correspondente for encontrado na fonte externa, o token não é alterado. Os construtores nesse modo não estão limitados à `<appSettings/>` e `<connectionStrings/>` seções.

A seguinte marcação de *Web. config* permite que o [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) em `Strict` modo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` mostrada anteriormente *Web. config* arquivo:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

O código anterior definirá os valores de propriedade:

* Os valores de *Web. config* arquivo se as chaves não forem definidas nas variáveis de ambiente.
* Os valores do ambiente variável, se definido.

Por exemplo, `ServiceID` conterá:

* "Valor ServiceID do Web. config", se a variável de ambiente `ServiceID` não está definido.
* O valor da `ServiceID` se variável de ambiente definida.

A imagem a seguir mostra a `<appSettings/>` chaves/valores de anterior *Web. config* conjunto de arquivos no editor de ambiente:

![editor de ambiente](config-builder/static/env.png)

Observação: Talvez você precise sair e reiniciar o Visual Studio para ver as alterações nas variáveis de ambiente.

### <a name="prefix-handling"></a>Tratamento de prefixo

Prefixos de chave podem simplificar as chaves de configuração porque:

* Configuração do .NET Framework é complexo e aninhada.
* Fontes externas de chave/valor são comumente básica e simples por natureza. Por exemplo, as variáveis de ambiente não estão aninhadas.

Use qualquer uma das abordagens a seguir para inserir tanto `<appSettings/>` e `<connectionStrings/>` para a configuração por meio de variáveis de ambiente:

* Com o `EnvironmentConfigBuilder` no padrão `Strict` modo e os nomes de chave apropriados no arquivo de configuração. O código e a marcação anterior usa essa abordagem. Usando essa abordagem, você pode **não** têm nomes idênticos chaves em ambos `<appSettings/>` e `<connectionStrings/>`.
* Use dois `EnvironmentConfigBuilder`s no `Greedy` modo com prefixos distintos e `stripPrefix`. Com essa abordagem, o aplicativo pode ler `<appSettings/>` e `<connectionStrings/>` sem a necessidade de atualizar o arquivo de configuração. A próxima seção, [stripPrefix](#stripprefix), mostra como fazer isso.
* Use dois `EnvironmentConfigBuilder`s em `Greedy` modo com prefixos distintos. Com essa abordagem, você não pode ter nomes duplicados de chave como nomes de chave devem ser diferentes por prefixo.  Por exemplo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Com a marcação anterior, a mesma fonte de chave/valor simples pode ser usada para preencher a configuração para duas seções diferentes.

A imagem a seguir mostra a `<appSettings/>` e `<connectionStrings/>` chaves/valores de anterior *Web. config* conjunto de arquivos no editor de ambiente:

![editor de ambiente](config-builder/static/prefix.png)

O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` chaves/valores contidos no anterior *Web. config* arquivo:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

O código anterior definirá os valores de propriedade:

* Os valores de *Web. config* arquivo se as chaves não forem definidas nas variáveis de ambiente.
* Os valores do ambiente variável, se definido.

Por exemplo, o uso anterior *Web. config* arquivos, chaves/valores na imagem anterior do editor de ambiente e o código anterior, os seguintes valores são definidos:

|  Chave              | Valor |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID de variáveis de env|
|    AppSetting_default            | AppSetting_default valor de env |
|       ConnStr_default         | Val ConnStr_default de env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: booliano, o padrão é `false`. 

A marcação XML anterior separa as configurações do aplicativo de cadeias de caracteres de conexão, mas exige que todas as chaves na *Web. config* arquivo para usar o prefixo especificado. Por exemplo, o prefixo `AppSetting` deve ser adicionado para o `ServiceID` chave ("AppSetting_ServiceID"). Com o `stripPrefix`, o prefixo não é usado o *Web. config* arquivo. O prefixo é necessária na fonte do construtor de configuração (por exemplo, no ambiente). Prevemos que a maioria dos desenvolvedores usará `stripPrefix`.

Aplicativos normalmente removem o prefixo. O seguinte *Web. config* retira o prefixo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Na anterior *Web. config* arquivo, o `default` chave está em ambos os `<appSettings/>` e `<connectionStrings/>`.

A imagem a seguir mostra a `<appSettings/>` e `<connectionStrings/>` chaves/valores de anterior *Web. config* conjunto de arquivos no editor de ambiente:

![editor de ambiente](config-builder/static/prefix.png)

O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` chaves/valores contidos no anterior *Web. config* arquivo:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

O código anterior definirá os valores de propriedade:

* Os valores de *Web. config* arquivo se as chaves não forem definidas nas variáveis de ambiente.
* Os valores do ambiente variável, se definido.

Por exemplo, o uso anterior *Web. config* arquivos, chaves/valores na imagem anterior do editor de ambiente e o código anterior, os seguintes valores são definidos:

|  Chave              | Valor |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID de variáveis de env|
|    default            | AppSetting_default valor de env |
|    default         | Val ConnStr_default de env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Cadeia de caracteres, o padrão é `@"\$\{(\w+)\}"`

O `Expand` comportamento dos construtores de procura o XML bruto para tokens que se parecem com `${token}`. Pesquisando é feito com a expressão regular do padrão `@"\$\{(\w+)\}"`. O conjunto de caracteres que corresponde ao `\w` é mais estrita que permitem que XML e muitas fontes de configuração. Use `tokenPattern` quando mais caracteres que `@"\$\{(\w+)\}"` são necessárias no nome do token.

`tokenPattern`: Cadeia de caracteres:

* Permite aos desenvolvedores alterar a expressão regular que é usada para correspondência de token.
* Nenhuma validação é feita para verificar se que ele é um regex bem formado, não pode ser perigoso.
* Ele deve conter um grupo de captura. O regex inteira deve corresponder o token inteiro. A primeira captura deve ser o nome do token para pesquisar na fonte de configuração.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Construtores de configuração em Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

O [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* É o mais simples dos construtores de configuração.
* Lê os valores do ambiente.
* Não tem nenhuma opção de configuração adicional.
* O `name` valor do atributo é arbitrário.

**Observação:** em um ambiente de contêiner do Windows, as variáveis definidas em tempo de execução só são injetadas no ambiente do processo de ponto de entrada. Aplicativos que são executados como um serviço ou um processo não EntryPoint não detectam essas variáveis, a menos que caso contrário, eles são injetados por meio de um mecanismo no contêiner. Para [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-baseados em contêineres, a versão atual do [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) lida com isso no *DefaultAppPool* somente. Talvez seja necessário desenvolver seu próprio mecanismo de injeção para processos de ponto de entrada não outras variantes do contêiner com base em Windows.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nunca armazenar senhas, cadeias de caracteres de conexão confidenciais ou outros dados confidenciais no código-fonte. Segredos de produção não devem ser usados para desenvolvimento ou teste.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Esse construtor de configuração fornece um recurso semelhante ao [Secret Manager do ASP.NET Core](/aspnet/core/security/app-secrets).

O [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) pode ser usado em projetos do .NET Framework, mas deve ser especificado um arquivo de segredos. Como alternativa, você pode definir o `UserSecretsId` propriedade no projeto do arquivo e cria o arquivo bruto segredos no local correto para leitura. Para manter as dependências externas fora do seu projeto, o arquivo de segredo é XML formatado. A formatação XML é um detalhe de implementação e o formato não deve ser usado. Se você precisa compartilhar um *Secrets* do arquivo com projetos do .NET Core, considere usar o [SimpleJsonConfigBuilder](#simplejsonconfig). O `SimpleJsonConfigBuilder` para .NET Core também deve ser considerado um detalhe de implementação, sujeito a alterações de formato.

Configuração de atributos para `UserSecretsConfigBuilder`:

* `userSecretsId` -Este é o método preferencial para identificar um arquivo de segredos XML. Ele funciona de forma semelhante ao .NET Core, que usa um `UserSecretsId` propriedade para armazenar esse identificador do projeto. A cadeia de caracteres deve ser exclusiva, ele não precisa ser um GUID. Com esse atributo, o `UserSecretsConfigBuilder` examinar uma localização local conhecida (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) para um arquivo de segredos que pertencem a esse identificador.
* `userSecretsFile` -Um atributo opcional especificando o arquivo que contém os segredos. O `~` caractere pode ser usado no início para fazer referência a raiz do aplicativo. Qualquer um desse atributo ou o `userSecretsId` atributo é necessário. Se ambos forem especificados, `userSecretsFile` terá precedência.
* `optional`: boolean, valor de padrão `true` -impede que uma exceção se o arquivo de segredos não pode ser encontrado. 
* O `name` valor do atributo é arbitrário.

O arquivo de segredos tem o seguinte formato:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

O [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lê os valores armazenados na [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` é necessário (o nome do cofre) ou um URI para o cofre. Os outros atributos permitem o controle sobre qual cofre para se conectar ao, mas serão necessárias apenas se o aplicativo não está em execução em um ambiente que funciona com `Microsoft.Azure.Services.AppAuthentication`. A biblioteca de autenticação de serviços do Azure é usada para selecionar automaticamente as informações de conexão do ambiente de execução se possível. Você pode substituir automaticamente pegar as informações de conexão, fornecendo uma cadeia de caracteres de conexão.

* `vaultName` -Necessário se `uri` no não fornecido. Especifica o nome do cofre em sua assinatura do Azure do qual ler os pares chave/valor.
* `connectionString` -Uma cadeia de caracteres de conexão utilizável por [o AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Conecta-se para outros provedores de Cofre de chaves especificado `uri` valor. Se não for especificado, do Azure (`vaultName`) é o provedor do cofre.
* `version` -O azure Key Vault fornece um recurso de controle de versão para segredos. Se `version` for especificado, o construtor apenas recupera segredos compatível com esta versão.
* `preloadSecretNames` -Por padrão, esse construtor querys **todos os** chave nomes no cofre de chaves quando ele é inicializado. Para evitar a leitura de todos os valores de chave, defina esse atributo como `false`. Definir isso como `false` lê segredos um por vez. Lendo um de cada vez pode útil se o cofre permite o acesso de "Get", mas não a "lista" acessar os segredos. **Observação:** ao usar `Greedy` modo `preloadSecretNames` deve ser `true` (o padrão).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) é um construtor de configuração básica que usa arquivos de um diretório como uma fonte de valores. Nome de um arquivo é a chave e o conteúdo é o valor. Esse construtor de configuração pode ser útil ao executar em um ambiente de contêiner orquestrada. Sistemas, como Docker Swarm e Kubernetes fornecem `secrets` para seus contêineres do windows orquestrada dessa maneira chave por arquivo.

Detalhes do atributo:

* `directoryPath` -Required. Especifica um caminho para procurar valores. Docker para Windows segredos são armazenados na *C:\ProgramData\Docker\secrets* diretório por padrão.
* `ignorePrefix` -Arquivos que começam com esse prefixo serão excluídas. O padrão é "ignore".
* `keyDelimiter` -O valor padrão é `null`. Se for especificado, o construtor de configuração atravessa vários níveis de diretório, a criação de nomes de chave com este delimitador. Se esse valor for `null`, o construtor de configuração olha apenas para o nível superior do diretório.
* `optional` -O valor padrão é `false`. Especifica se o construtor de configuração deve causar erros se o diretório de origem não existe.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nunca armazenar senhas, cadeias de caracteres de conexão confidenciais ou outros dados confidenciais no código-fonte. Segredos de produção não devem ser usados para desenvolvimento ou teste.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Projetos do .NET core frequentemente usam arquivos JSON para a configuração. O [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) construtor permite que os arquivos de JSON do .NET Core a ser usado no .NET Framework. Esse construtor de configuração é fornece um mapeamento básico de uma fonte de chave/valor simples em áreas específicas de chave/valor de configuração do .NET Framework. Esse construtor de configuração faz **não** fornecem configurações hierárquica. O arquivo de backup do JSON é semelhante a um dicionário, não um objeto hierárquico complexo. Um arquivo hierárquico multinível pode ser usado. Esse provedor `flatten`s a profundidade, acrescentando o nome da propriedade em cada nível usando `:` como um delimitador.

Detalhes do atributo:

* `jsonFile` -Required. Especifica o arquivo JSON para ler. O `~` caractere pode ser usado no início para fazer referência a raiz do aplicativo.
* `optional` -Booliano, o valor padrão é `true`. Impede a gerar exceções se o arquivo JSON não pode ser encontrado.
* `jsonMode` - `[Flat|Sectional]`. `Flat` é o padrão. Quando `jsonMode` é `Flat`, o arquivo JSON é uma fonte de chave/valor único simples. O `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` também são fontes de chave/valor único simples. Quando o `SimpleJsonConfigBuilder` está configurado no `Sectional` modo:

  * O arquivo JSON é dividido conceitualmente apenas no nível superior em vários dicionários.
  * Cada um dos dicionários só será aplicada à seção de configuração que corresponde ao nome de propriedade de nível superior anexado a eles. Por exemplo:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementando um construtor de configuração de chave/valor personalizados

Se os construtores de configuração não atenderem às suas necessidades, você pode escrever um personalizado. O `KeyValueConfigBuilder` classe base manipula a maioria das preocupações de prefixo e modos de substituição. Um projeto de implementação só são necessárias:

* Herdar da classe base e implementar uma fonte básica de pares chave/valor por meio de `GetValue` e `GetAllValues`:
* Adicione a [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) ao projeto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

O `KeyValueConfigBuilder` classe base fornece grande parte do comportamento do trabalho e consistente entre chave/valor construtores de configuração.

## <a name="additional-resources"></a>Recursos adicionais

* [Repositório GitHub de construtores de configuração](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticação serviço a serviço para o Azure Key Vault usando o .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
