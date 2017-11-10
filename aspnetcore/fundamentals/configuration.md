---
title: "Configuração no ASP.NET Core"
author: rick-anderson
description: "Saiba como usar a API de configuração para configurar um aplicativo ASP.NET Core de várias fontes."
keywords: "ASP.NET Core, configuração, JSON, configuração"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: d626768fe1a485705e104a5c758cbdb0b46685a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="configuration-in-aspnet-core"></a>Configuração no ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)

A API de configuração fornece uma maneira de configurar um aplicativo baseado em uma lista de pares nome-valor. Configuração é lida em tempo de execução de várias fontes. Os pares nome-valor podem ser agrupados em uma hierarquia de vários níveis. Há provedores de configuração para:

* Formatos de arquivo (INI, JSON e XML)
* Argumentos de linha de comando
* Variáveis de ambiente
* Objetos do .NET na memória
* Um repositório de usuário criptografado
* [Cofre de chaves do Azure](xref:security/key-vault-configuration)
* Provedores personalizados, que você instala ou criar

Cada valor de configuração é mapeado para uma chave de cadeia de caracteres. Não há suporte de ligação interna ao desserializar as configurações em um personalizado [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (uma classe .NET simple com propriedades).

[Exibir ou baixar código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="simple-configuration"></a>Configuração simples

O aplicativo de console a seguir usa o provedor de configuração JSON:

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

O aplicativo lê e exibe as seguintes configurações:

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

Configuração consiste em uma lista hierárquica de pares nome-valor no qual os nós são separados por dois-pontos. Para recuperar um valor, acessar o `Configuration` indexador com a chave do item correspondente:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Para trabalhar com matrizes em fontes de configuração formatados em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois-pontos. O exemplo a seguir obtém o nome do primeiro item na anterior `wizards` matriz:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Pares de nome-valor gravados interna em `Configuration` provedores são **não** persistentes, no entanto, você pode criar um provedor personalizado que salva valores. Consulte [provedor de configuração personalizado](xref:fundamentals/configuration#custom-config-providers).

O exemplo anterior usa o indexador de configuração para ler valores. A configuração de acesso fora de `Startup`, use o [padrão de opções](xref:fundamentals/configuration#options-config-objects). O *padrão de opções* mostrado mais adiante neste artigo.

É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção. O `CreateDefaultBuilder` método de extensão em um aplicativo do ASP.NET Core 2. x (ou usando `AddJsonFile` e `AddEnvironmentVariables` diretamente em um aplicativo do ASP.NET Core 1. x) adiciona provedores de configuração para ler arquivos JSON e sistema de fontes de configuração:

* *appsettings.json*
* *appSettings. \<EnvironmentName >. JSON*
* variáveis de ambiente

Consulte [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obter uma explicação dos parâmetros. `reloadOnChange`só é suportado no ASP.NET Core 1.1 e superior. 

Fontes de configuração são lidos na ordem em que eles sejam especificados. No código acima, as variáveis de ambiente são lidos pela última. Quaisquer valores de configuração definido através do ambiente substituiria aquelas definidas em dois provedores anteriores.

O ambiente normalmente é definido como um dos `Development`, `Staging`, ou `Production`. Consulte [trabalhando com vários ambientes](environments.md) para obter mais informações.

Considerações de configuração:

* `IOptionsSnapshot`pode recarregar os dados de configuração quando ele é alterado. Use `IOptionsSnapshot` se você precisar recarregar os dados de configuração.  Consulte [IOptionsSnapshot](#ioptionssnapshot) para obter mais informações.
* Chaves de configuração diferenciam maiusculas de minúsculas.
* Uma prática recomendada é especificar variáveis de ambiente por último, para que o ambiente local pode substituir qualquer coisa definidas nos arquivos de configuração implantada.
* **Nunca** armazenar senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação. Não usa segredos de produção no desenvolvimento ou ambientes de teste. Em vez disso, especifique segredos fora da árvore de projeto para que eles não puderem ser acidentalmente confirmados para o repositório. Saiba mais sobre [trabalhando com vários ambientes](environments.md) e gerenciando [armazenamento seguro de segredos do aplicativo durante o desenvolvimento](../security/app-secrets.md).
* Se `:` não pode ser usado em variáveis de ambiente em seu sistema, substitua `:` com `__` (sublinhado duplo).

<a name="options-config-objects"></a>

## <a name="using-options-and-configuration-objects"></a>Usando opções e objetos de configuração

O padrão de opções usa as classes de opções personalizada para representar um grupo de configurações relacionadas. É recomendável que você crie classes separadas para cada recurso dentro de seu aplicativo. Execute as classes separadas:

* O [princípio de diferenciação de Interface (ISP)](http://deviq.com/interface-segregation-principle/) : Classes dependem apenas as definições de configuração que eles usam.
* [Separação de preocupações](http://deviq.com/separation-of-concerns/) : configurações para diferentes partes do seu aplicativo não são dependentes ou combinado com uma da outra.

A classe de opções deve ser não-abstrato com um construtor público sem parâmetros. Por exemplo:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name="options-example"></a>

No código a seguir, o provedor de configuração JSON está habilitado. O `MyOptions` classe é adicionada ao contêiner de serviço e associada à configuração.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

O seguinte [controlador](../mvc/controllers/index.md) usa [construtor injeção de dependência](xref:fundamentals/dependency-injection#what-is-dependency-injection) na [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) para acessar as configurações:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

Com o seguinte *appSettings. JSON* arquivo:

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

O `HomeController.Index` método retornará `option1 = value1_from_json, option2 = 2`.

Aplicativos típicos não associar toda a configuração para um arquivo de opções de único. Posteriormente, mostrarei como usar `GetSection` para vincular a uma seção.

No código a seguir, um segundo `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço. Ele usa um delegado para configurar a associação com `MyOptions`.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

Você pode adicionar vários provedores de configuração. Provedores de configuração estão disponíveis em pacotes do NuGet. Elas são aplicadas na ordem que eles são registrados.

Cada chamada para `Configure<TOptions>` adiciona um `IConfigureOptions<TOptions>` serviço ao contêiner de serviço. No exemplo anterior, os valores de `Option1` e `Option2` estão especificados na *appSettings. JSON* –, mas o valor de `Option1` é substituído pelo delegado configurado. 

Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificado "wins" (define o valor de configuração). No código anterior, o `HomeController.Index` método retornará `option1 = value1_from_action, option2 = 2`.

Quando você associa opções de configuração, cada propriedade no seu tipo de opções está associada a uma chave de configuração do formulário `property[:sub-property:]`. Por exemplo, o `MyOptions.Option1` propriedade está associada à chave `Option1`, que são lidos a partir de `option1` propriedade em *appSettings. JSON*. Um exemplo de subpropriedade é mostrado mais adiante neste artigo.

No código a seguir, um terceiro `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço. Ele é ligado `MySubOptions` a seção `subsection` do *appSettings. JSON* arquivo:

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

Observação: Esse método de extensão exige o `Microsoft.Extensions.Options.ConfigurationExtensions` pacote NuGet.

Usando o seguinte *appSettings. JSON* arquivo:

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

O `MySubOptions` classe:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

Com os seguintes `Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`é retornado.

Você também pode fornecer opções em um modelo de exibição ou injetar `IOptions<TOptions>` diretamente em um modo de exibição:

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name="in-memory-provider"></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*Exige o ASP.NET Core 1.1 ou posterior.*

`IOptionsSnapshot`dá suporte a recarregar os dados de configuração quando o arquivo de configuração foi alterada. Ele tem uma sobrecarga mínima. Usando `IOptionsSnapshot` com `reloadOnChange: true`, as opções são vinculadas a `IConfiguration` e recarregados quando alterado.

O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criada após *config. JSON* alterações. Solicitações para o servidor retornará o mesmo tempo quando *config. JSON* tem **não** alterado. A primeira solicitação após *config. JSON* alterações mostrará uma nova hora.

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

A imagem a seguir mostra a saída do servidor:

![mostrando de imagem do navegador "última atualização: 22/11/2016 16:43:00"](configuration/_static/first.png)

Atualizar o navegador não altera o valor de mensagem ou hora exibida (quando *config. JSON* não foi alterado).

Alterar e salvar o *config. JSON* e, em seguida, atualize o navegador:

![mostrando de imagem do navegador "última atualização para e: 22/11/2016 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Provedor na memória e a associação a uma classe POCO

O exemplo a seguir mostra como usar o provedor na memória e vincular a uma classe:

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite que a construção de objetos. Associação permite que você recupere POCO objetos ou gráficos de objeto inteiro até mesmo. O exemplo a seguir mostra como associar a `MyWindow` e usar o padrão de opções com um aplicativo MVC do ASP.NET Core:

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

Associar a classe personalizada em `ConfigureServices` quando estiver criando o host:

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

Exibir as configurações de `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

O exemplo a seguir demonstra o [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensão:

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

O ConfigurationBinder `GetValue<T>` método permite que você especifique um valor padrão (80 no exemplo). `GetValue<T>`para cenários simples e não vincular ao seções inteiras. `GetValue<T>`Obtém os valores escalares de `GetSection(key).Value` convertido em um tipo específico.

## <a name="binding-to-an-object-graph"></a>Associando a um gráfico de objeto

Você pode vincular recursivamente para cada objeto em uma classe. Considere o seguinte `AppOptions` classe:

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

O exemplo a seguir associa ao `AppOptions` classe:

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** e superior podem usar `Get<T>`, que trabalha com seções inteiras. `Get<T>`pode ser mais convienent que usar `Bind`. O código a seguir mostra como usar `Get<T>` com o exemplo acima:

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

Usando o seguinte *appSettings. JSON* arquivo:

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

O programa exibe `Height 11`.

O código a seguir pode ser usado para a unidade a configuração de teste:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Exemplo básico do provedor personalizado do Entity Framework

Nesta seção, é criado um provedor de configuração básica que lê os pares nome-valor de um banco de dados usando EF. 

Definir um `ConfigurationValue` entidade para armazenar valores de configuração no banco de dados:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Adicionar um `ConfigurationContext` para armazenar e acessar os valores configurados:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Criar uma classe que implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Criar o provedor de configuração personalizado herdando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  O provedor de configuração inicializa o banco de dados quando ela estiver vazia:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo for executado.

Você pode adicionar um `EFConfigSource` método de extensão para adicionar a fonte de configuração:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

O código a seguir mostra como usar personalizado `EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Observe que o exemplo adiciona personalizado `EFConfigProvider` depois que o provedor JSON, então todas as configurações do banco de dados irá substituir as configurações do *appSettings. JSON* arquivo.

Usando o seguinte *appSettings. JSON* arquivo:

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

O seguinte é exibido:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Provedor de configuração de linha de comando

O [provedor de configuração de linha de comando](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recebe pares de chave-valor do argumento de linha de comando para a configuração em tempo de execução.

[Exibir ou baixar o exemplo de configuração de linha de comando](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>Configurando o provedor

# <a name="basic-configurationtabbasicconfiguration"></a>[Configuração básica](#tab/basicconfiguration)

Para ativar a configuração de linha de comando, chame o `AddCommandLine` método de extensão em uma instância do [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

Executar o código, a seguinte saída é exibida:

```console
MachineName: MairaPC
Left: 1980
```

Passando pares chave-valor do argumento na linha de comando altera os valores de `Profile:MachineName` e `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Exibe a janela do console:

```console
MachineName: BartPC
Left: 1979
```

Para substituir a configuração fornecida por outros provedores de configuração com a configuração de linha de comando, chame `AddCommandLine` último em `ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aplicativos típicos de 2. x ASP.NET Core usam o método estático de conveniência `CreateDefaultBuilder` para criar o host:

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`carrega a configuração opcional de *appSettings. JSON*, *appsettings. { . JSON de ambiente}*, [segredos do usuário](xref:security/app-secrets) (no `Development` ambiente), variáveis de ambiente e os argumentos de linha de comando. O provedor de configuração de linha de comando é chamado por último. Chamando o provedor última permite que os argumentos de linha de comando passados em tempo de execução para substituir a configuração definida por outros provedores de configuração chamado anteriormente.

Observe que para *appsettings* arquivos `reloadOnChange` está habilitado. Argumentos de linha de comando são substituídos se o valor de configuração correspondente em uma *appsettings* arquivo for alterado depois que o aplicativo é iniciado.

> [!NOTE]
> Como uma alternativa ao uso de `CreateDefaultBuilder` método, criando um host usando [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) e criar manualmente a configuração com [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) é suportado no ASP.NET Core 2. x. Consulte a guia de 1. x do ASP.NET Core para obter mais informações.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Criar um [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chamar o `AddCommandLine` método para usar o provedor de configuração de linha de comando. Chamando o provedor última permite que os argumentos de linha de comando passados em tempo de execução para substituir a configuração definida por outros provedores de configuração chamado anteriormente. Aplicar a configuração de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) com o `UseConfiguration` método:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Argumentos passados na linha de comando devem estar de acordo com um dos dois formatos mostrados na tabela a seguir.

| Formato de argumento                                                     | Exemplo        |
| ------------------------------------------------------------------- | :------------: |
| Um único argumento: um par chave-valor separados por um sinal de igual (`=`) | `key1=value`   |
| Sequência de dois argumentos: um par chave-valor separados por um espaço    | `/key1 value1` |

**Argumento único**

O valor deve seguir um sinal de igual (`=`). O valor pode ser nulo (por exemplo, `mykey=`).

A chave pode ter um prefixo.

| Prefixo da chave               | Exemplo         |
| ------------------------ | :-------------: |
| Nenhum prefixo                | `key1=value1`   |
| Um único traço (`-`) &#8224; | `-key2=value2`  |
| Dois traços (`--`)        | `--key3=value3` |
| Barra (`/`)      | `/key4=value4`  |

&#8224; Uma chave com um prefixo de traço único (`-`) devem ser fornecidos em [alternar mapeamentos](#switch-mappings), descrito abaixo.

Exemplo de comando:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Observação: Se `-key1` não está presente no [alternar mapeamentos](#switch-mappings) fornecido para o provedor de configuração, um `FormatException` é gerada.

**Sequência de dois argumentos**

O valor não pode ser nulo e deve seguir a chave separada por um espaço.

A chave deve ter um prefixo.

| Prefixo da chave               | Exemplo         |
| ------------------------ | :-------------: |
| Um único traço (`-`) &#8224; | `-key1 value1`  |
| Dois traços (`--`)        | `--key2 value2` |
| Barra (`/`)      | `/key3 value3`  |

&#8224; Uma chave com um prefixo de traço único (`-`) devem ser fornecidos em [alternar mapeamentos](#switch-mappings), descrito abaixo.

Exemplo de comando:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Observação: Se `-key1` não está presente no [alternar mapeamentos](#switch-mappings) fornecido para o provedor de configuração, um `FormatException` é gerada.

### <a name="duplicate-keys"></a>Chaves duplicadas

Se as chaves duplicadas são fornecidas, o último par chave-valor é usado.

### <a name="switch-mappings"></a>Mapeamentos de comutador

Ao criar manualmente a configuração com `ConfigurationBuilder`, opcionalmente, você pode fornecer um dicionário de mapeamentos de comutador para o `AddCommandLine` método. Mapeamentos de comutador permitem que você forneça a lógica de substituição do nome da chave.

Quando o dicionário de mapeamentos de comutador é usado, o dicionário é verificado para uma chave que corresponde à chave fornecida por um argumento de linha de comando. Se a chave de linha de comando é encontrada no dicionário, o valor do dicionário (de substituição de chave) é passado para definir a configuração. Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixado com um único traço (`-`).

Regras de chave de dicionário de mapeamentos do comutador:

* Comutadores devem começar com um traço (`-`) ou traço duplo (`--`).
* O dicionário de mapeamentos de chave não deve conter chaves duplicadas.

No exemplo a seguir, o `GetSwitchMappings` método permite que seus argumentos de linha de comando usar um único traço (`-`) chave prefixo e evitar prefixos subchaves à esquerda.

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

Sem fornecer argumentos de linha de comando, o dicionário fornecido para `AddInMemoryCollection` define os valores de configuração. Execute o aplicativo com o seguinte comando:

```console
dotnet run
```

Exibe a janela do console:

```console
MachineName: RickPC
Left: 1980
```

Use o seguinte para passar parâmetros de configuração:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Exibe a janela do console:

```console
MachineName: DahliaPC
Left: 1984
```

Depois que o dicionário de mapeamentos de chave é criado, ele contém os dados mostrados na tabela a seguir.

| Chave            | Valor                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Para demonstrar a troca de chaves usando o dicionário, execute o seguinte comando:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

As chaves de linha de comando são trocadas. A janela de console exibe os valores de configuração `Profile:MachineName` e `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>O arquivo Web. config

Um *Web. config* arquivo é necessário quando você hospeda o aplicativo no IIS ou IIS Express. *Web. config* ativa AspNetCoreModule no IIS para iniciar seu aplicativo. As configurações no *Web. config* habilitar AspNetCoreModule no IIS para iniciar seu aplicativo e definir outras configurações do IIS e módulos. Se você estiver usando o Visual Studio e excluir *Web. config*, Visual Studio irá criar um novo.

## <a name="additional-notes"></a>Observações adicionais

* Injeção de dependência (DI) não está definido até depois `ConfigureServices` é invocado.
* O sistema de configuração não está ciente de injeção de dependência.
* `IConfiguration`tem dois especializações:
  * `IConfigurationRoot`Usado para o nó raiz. Pode disparar um recarregamento.
  * `IConfigurationSection`Representa uma seção de valores de configuração. O `GetSection` e `GetChildren` métodos retornam um `IConfigurationSection`.

## <a name="additional-resources"></a>Recursos adicionais

* [Trabalhando com vários ambientes](environments.md)
* [Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento](../security/app-secrets.md)
* [Hospedagem no núcleo do ASP.NET](xref:fundamentals/hosting)
* [Injeção de dependência](dependency-injection.md)
* [Provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration)
