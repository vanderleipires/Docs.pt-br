---
title: Configuração no ASP.NET Core
author: rick-anderson
description: Use a API de configuração para configurar um aplicativo do ASP.NET Core por vários métodos.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: afff36ffc232b00389c52d9e751ae398555c9656
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2018
---
# <a name="configuration-in-aspnet-core"></a>Configuração no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

A API de configuração fornece uma maneira de configurar um aplicativo Web do ASP.NET Core com base em uma lista de pares nome-valor. A configuração é lida em tempo de execução por meio de várias fontes. Os pares de nome-valor podem ser agrupados em uma hierarquia de vários níveis.

Há provedores de configuração para:

* Formatos de arquivo (INI, JSON e XML).
* Argumentos de linha de comando.
* Variáveis de ambiente.
* Objetos do .NET na memória.
* O armazenamento do [Secret Manager](xref:security/app-secrets) não criptografado.
* Um repositório de usuário criptografado, como o [Azure Key Vault](xref:security/key-vault-configuration).
* Provedores personalizados (instalados ou criados).

Cada valor de configuração é mapeado para uma chave de cadeia de caracteres. Há suporte interno de associação para desserializar configurações em um objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (uma classe simples de .NET com propriedades).

O padrão de opções usa classes de opções para representar grupos de configurações relacionadas. Para obter mais informações sobre como usar o padrão de opções, consulte o tópico [Opções](xref:fundamentals/configuration/options).

[Exibir ou baixar código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>Configuração de JSON

O aplicativo de console a seguir usa o provedor de configuração JSON:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

O aplicativo lê e exibe as seguintes definições de configuração:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

A configuração consiste em uma lista hierárquica de pares nome-valor na qual os nós são separados por dois pontos (`:`). Para recuperar um valor, acesse o indexador `Configuration` com a chave do item correspondente:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

Para trabalhar com matrizes em fontes de configuração formatadas em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois pontos. O exemplo a seguir obtém o nome do primeiro item na matriz `wizards` anterior:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Os pares nome-valor gravados nos provedores de [Configuração](/dotnet/api/microsoft.extensions.configuration) internos **não** são mantidos. No entanto, um provedor personalizado que salva valores pode ser criado. Consulte [provedor de configuração personalizado](xref:fundamentals/configuration/index#custom-config-providers).

O exemplo anterior usa o indexador de configuração para ler valores. Para acessar a configuração fora de `Startup`, use o *padrão de opções*. Para obter mais informações, consulte o tópico [Opções](xref:fundamentals/configuration/options).

## <a name="xml-configuration"></a>Configuração XML

Para trabalhar com matrizes em fontes de configuração formatadas em XML, forneça um índice `name` para cada elemento. Use o índice para acessar os valores:

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>Configuração por ambiente

É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção. O método de extensão `CreateDefaultBuilder` em um aplicativo do ASP.NET Core 2.x (ou usando `AddJsonFile` e `AddEnvironmentVariables` diretamente em um aplicativo do ASP.NET Core 1.x) adiciona provedores de configuração para ler arquivos JSON e fontes de configuração do sistema:

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* Variáveis de ambiente

Aplicativos do ASP.NET Core 1.x precisam chamar `AddJsonFile` e [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Consulte [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para uma explicação sobre os parâmetros. `reloadOnChange` só é compatível no ASP.NET Core 1.1 ou posterior.

As fontes de configuração são lidas na ordem em que são especificadas. No código anterior, as variáveis de ambiente são lidas por último. Os valores de configuração definidos por meio do ambiente substituem aqueles definidos nos dois provedores anteriores.

Considere o arquivo *appsettings.Staging.json* a seguir:

[!code-json[](index/sample/appsettings.Staging.json)]

Quando o ambiente está configurado como `Staging`, o seguinte método `Configure` lê o valor de `MyConfig`:

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

O ambiente normalmente é definido como `Development`, `Staging` ou `Production`. Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).

Considerações de configuração:

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) pode recarregar dados de configuração quando é alterado.
* Chaves de configuração **não** diferenciam maiúsculas de minúsculas.
* **Nunca** armazene senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação. Não use segredos de produção em ambientes de teste ou de desenvolvimento. Especifique segredos fora do projeto para que eles não sejam acidentalmente comprometidos com um repositório de código-fonte. Saiba mais sobre [como usar vários ambientes](xref:fundamentals/environments) e gerenciar [armazenamento seguro de segredos de aplicativo no desenvolvimento](xref:security/app-secrets).
* Para saber valores de configuração hierárquica especificados nas variáveis de ambiente, dois-pontos (`:`) pode não funcionar em todas as plataformas. Sublinhado duplo (`__`) é compatível com todas as plataformas.
* Ao interagir com a configuração de API, dois-pontos (`:`) funciona em todas as plataformas.

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Provedor na memória e associação a uma classe POCO

O exemplo a seguir mostra como usar o provedor na memória e associar a uma classe:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite a construção de objetos. A associação permite recuperar objetos POCO ou até mesmo gráficos de objetos inteiros.

### <a name="getvalue"></a>GetValue

O exemplo a seguir demonstra o método de extensão [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

O método `GetValue<T>` do ConfigurationBinder permite especificar um valor padrão (80 no exemplo). `GetValue<T>` é para cenários simples e não se associa a seções inteiras. `GetValue<T>` obtém valores escalares de `GetSection(key).Value` convertidos em um tipo específico.

## <a name="bind-to-an-object-graph"></a>Associar a um gráfico de objeto

Cada objeto em uma classe pode ser associado recursivamente. Considere a classe `AppSettings` a seguir:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

O exemplo a seguir associa à classe `AppSettings`:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

O **ASP.NET Core 1.1** e superior pode usar `Get<T>`, que trabalha com seções inteiras. O `Get<T>` pode ser mais conveniente do que usar `Bind`. O código a seguir mostra como usar o `Get<T>` com o exemplo acima:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Usando o seguinte arquivo *appsettings.json*:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

O programa exibe `Height 11`.

O código a seguir pode ser usado para o teste de unidade da configuração:

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Criar um provedor personalizado do Entity Framework

Nesta seção, é criado um provedor de configuração básico, que lê os pares nome-valor de um banco de dados, usando o EF.

Defina uma entidade `ConfigurationValue` para armazenar valores de configuração no banco de dados:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Adicione um `ConfigurationContext` para armazenar e acessar os valores configurados:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Crie uma classe que implementa [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Crie o provedor de configuração personalizado através da herança do [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). O provedor de configuração inicializa o banco de dados quando ele está vazio:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo é executado.

Um método de extensão `EFConfigSource` para adicionar a fonte de configuração pode ser usado:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

O código a seguir mostra como usar o `EFConfigProvider` personalizado:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Observe que o exemplo adiciona o `EFConfigProvider` personalizado depois do provedor JSON, de maneira que todas as configurações do banco de dados substituam as configurações do arquivo *appsettings.json*.

Usando o seguinte arquivo *appsettings.json*:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

É exibida a saída a seguir:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Provedor de configuração CommandLine

O [Provedor de configuração CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recebe pares chave-valor de argumento de linha de comando para a configuração em tempo de execução.

[Exibir ou baixar o exemplo de configuração do CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Configurar e usar o provedor de configuração CommandLine

# <a name="basic-configurationtabbasicconfiguration"></a>[Configuração básica](#tab/basicconfiguration/)

Para ativar a configuração de linha de comando, chame o método de extensão `AddCommandLine` em uma instância do [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Ao executar o código, a seguinte saída será exibida:

```console
MachineName: MairaPC
Left: 1980
```

Passar pares chave-valor de argumento na linha de comando altera os valores de `Profile:MachineName` e `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

A janela do console exibe:

```console
MachineName: BartPC
Left: 1979
```

Para substituir a configuração fornecida por outros provedores de configuração pela configuração de linha de comando, chame `AddCommandLine` por último em `ConfigurationBuilder`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Os aplicativos típicos de ASP.NET Core 2.x usam o método de conveniência estático `CreateDefaultBuilder` para criar o host:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

O `CreateDefaultBuilder` carrega a configuração opcional de *appsettings.json*, de *appsettings.{Environment}.json*, dos [segredos do usuário](xref:security/app-secrets) (no ambiente `Development`), das variáveis de ambiente e dos argumentos de linha de comando. O provedor CommandLine é chamado por último. Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.

Para arquivos *appsettings* em que:

* `reloadOnChange` está habilitado.
* Contém a mesma configuração nos argumentos de linha de comando e um arquivo *appsettings*.
* O arquivo *appsettings* que contém o argumento de linha de comando correspondente é alterado depois que o aplicativo for iniciado.

Se todas as condições acima forem verdadeiras, os argumentos de linha de comando serão substituídos.

O aplicativo do ASP.NET Core 2.x pode usar [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) em vez de `CreateDefaultBuilder`. Ao usar `WebHostBuilder`, defina manualmente a configuração com [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Consulte a guia do ASP.NET Core 1.x para obter mais informações.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Crie um [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chame o método `AddCommandLine` para usar o provedor de configuração CommandLine. Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente. Aplique a configuração ao [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) com o método `UseConfiguration`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Os argumentos passados na linha de comando devem estar em conformidade com um dos dois formatos mostrados na tabela a seguir:

| Formato de argumento                                                     | Exemplo        |
| ------------------------------------------------------------------- | :------------: |
| Argumento único: um par chave-valor separado por um sinal de igual (`=`) | `key1=value`   |
| Sequência de dois argumentos: um par chave-valor separado por um espaço    | `/key1 value1` |

**Argumento único**

O valor deve seguir um sinal de igual (`=`). O valor pode ser nulo (por exemplo, `mykey=`).

A chave pode ter um prefixo.

| Prefixo da chave               | Exemplo         |
| ------------------------ | :-------------: |
| Nenhum prefixo                | `key1=value1`   |
| Traço único (`-`)&#8224; | `-key2=value2`  |
| Dois traços (`--`)        | `--key3=value3` |
| Barra (`/`)      | `/key4=value4`  |

& #8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.

Comando de exemplo:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Observação: se `-key2` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.

**Sequência de dois argumentos**

O valor não pode ser nulo e deve seguir a chave separada por um espaço.

A chave deve ter um prefixo.

| Prefixo da chave               | Exemplo         |
| ------------------------ | :-------------: |
| Traço único (`-`)&#8224; | `-key1 value1`  |
| Dois traços (`--`)        | `--key2 value2` |
| Barra (`/`)      | `/key3 value3`  |

& #8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.

Comando de exemplo:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.

### <a name="duplicate-keys"></a>Chaves duplicadas

Se chaves duplicadas forem fornecidas, o último par chave-valor será usado.

### <a name="switch-mappings"></a>Mapeamentos de comutador

Ao criar manualmente a configuração com `ConfigurationBuilder`, um dicionário de mapeamentos de comutador pode ser adicionado ao método `AddCommandLine`. Os mapeamentos de comutador permitem fornecer a lógica de substituição do nome da chave.

Ao ser usado, o dicionário de mapeamentos de comutador é verificado para oferecer uma chave que corresponda à chave fornecida por um argumento de linha de comando. Se a chave de linha de comando for encontrada no dicionário, o valor do dicionário (a substituição da chave) será passado de volta para definir a configuração. Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixada com um traço único (`-`).

Regras de chave do dicionário de mapeamentos de comutador:

* Os comutadores devem começar com um traço (`-`) ou traço duplo (`--`).
* O dicionário de mapeamentos de comutador chave não deve conter chaves duplicadas.

No exemplo a seguir, o método `GetSwitchMappings` permite que os argumentos de linha de comando usem um prefixo de chave de traço único (`-`) e evitem prefixos de subchaves à esquerda.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Sem fornecer argumentos de linha de comando, o dicionário fornecido para `AddInMemoryCollection` definirá os valores de configuração. Execute o aplicativo com o seguinte comando:

```console
dotnet run
```

A janela do console exibe:

```console
MachineName: RickPC
Left: 1980
```

Use o seguinte para passar parâmetros de configuração:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

A janela do console exibe:

```console
MachineName: DahliaPC
Left: 1984
```

Depois que o dicionário de mapeamentos de comutador for criado, ele conterá os dados mostrados na tabela a seguir:

| Chave            | Valor                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Para demonstrar a troca de chaves usando o dicionário, execute o seguinte comando:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

As chaves da linha de comando são trocadas. A janela de console exibe os valores de configuração de `Profile:MachineName` e `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>Arquivo web.config

Um arquivo *web.config* é necessário quando você hospeda o aplicativo em IIS ou IIS Express. As configurações no *web.config* habilitam o [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para iniciar o aplicativo e definir outras configurações e módulos do IIS. Se o arquivo *web.config* não estiver presente e o arquivo de projeto inclui `<Project Sdk="Microsoft.NET.Sdk.Web">`, a publicação do projeto criará um arquivo *web.config* na saída publicada (a pasta *publish*). Para obter mais informações, consulte [Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="access-configuration-during-startup"></a>Acessar a configuração durante a inicialização

Para acessar a configuração em `ConfigureServices` ou `Configure` durante a inicialização, consulte os exemplos no tópico [Inicialização do aplicativo](xref:fundamentals/startup).

## <a name="adding-configuration-from-an-external-assembly"></a>Adicionar configuração de um assembly externo

Uma implementação [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite adicionar melhorias a um aplicativo durante a inicialização de um assembly externo fora da classe `Startup` do aplicativo. Para obter mais informações, veja [Aprimorar um aplicativo de um assembly externo](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Acessar a configuração em uma página do Razor ou exibição do MVC

Para acessar definições de configuração em uma página do Razor ou uma exibição do MVC, adicione [usando diretiva](xref:mvc/views/razor#using) ([referência de C#: usando diretiva](/dotnet/csharp/language-reference/keywords/using-directive)) para o [namespace Microsoft.Extensions.Configuration](/dotnet/api/microsoft.extensions.configuration) e injete [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) na página ou na exibição.

Em uma página do Razor:

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

Em uma exibição do MVC:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a>Observações adicionais

* A DI (injeção de dependência) não é configurada até que `ConfigureServices` seja invocado.
* O sistema de configuração não tem reconhecimento de DI.
* O `IConfiguration` tem duas especializações:
  * `IConfigurationRoot` Usado para o nó raiz. Pode disparar um recarregamento.
  * `IConfigurationSection` Representa uma seção de valores de configuração. O métodos `GetSection` e `GetChildren` retornam um `IConfigurationSection`.
  * Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) ao recarregar a configuração ou para ter acesso a cada provedor. Nenhuma dessas situações são comuns.

## <a name="additional-resources"></a>Recursos adicionais

* [Opções](xref:fundamentals/configuration/options)
* [Usar vários ambientes](xref:fundamentals/environments)
* [Armazenamento seguro dos segredos do aplicativo no desenvolvimento](xref:security/app-secrets)
* [Hospedagem no ASP.NET Core](xref:fundamentals/hosting)
* [Injeção de dependência](xref:fundamentals/dependency-injection)
* [Provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration)
