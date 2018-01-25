---
title: "Provedores de arquivo no núcleo do ASP.NET"
author: ardalis
description: Saiba como o ASP.NET Core abstrai o acesso de sistema de arquivos com o uso de provedores de arquivo.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a>Provedores de arquivo no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core abstrai o acesso de sistema de arquivos com o uso de provedores de arquivo.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Abstrações de provedor de arquivo

Provedores de arquivo são uma abstração sobre os sistemas de arquivo. É a principal interface `IFileProvider`. `IFileProvider`expõe métodos para obter informações sobre o arquivo (`IFileInfo`), informações de diretório (`IDirectoryContents`) e para configurar as notificações de alteração (usando um `IChangeToken`).

`IFileInfo`fornece métodos e propriedades sobre arquivos individuais ou diretórios. Ele tem duas propriedades booleanas, `Exists` e `IsDirectory`, bem como as propriedades que descrevem o arquivo `Name`, `Length` (em bytes), e `LastModified` Data. É possível ler o arquivo usando seu `CreateReadStream` método.

## <a name="file-provider-implementations"></a>Implementações do provedor de arquivo

Três implementações de `IFileProvider` estão disponíveis: física, inserida e composto. O provedor físico é usado para acessar os arquivos do sistema atual. O provedor inserido é usado para acessar arquivos inseridos em assemblies. O provedor composto é usado para fornecer acesso combinado a arquivos e diretórios de um ou mais provedores.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

O `PhysicalFileProvider` fornece acesso ao sistema de arquivos físico. Ele encapsula o `System.IO.File` tipo (para o provedor físico), todos os caminhos para um diretório e seus filhos de escopo. Esse escopo limita o acesso a um determinado diretório e seus filhos, impedindo o acesso ao sistema de arquivos fora esse limite. Ao instanciar esse provedor, você deve fornecê-lo com um caminho de diretório, que serve como o caminho base para todas as solicitações feitas a esse provedor (e que restringe o acesso de fora deste caminho). Em um aplicativo do ASP.NET Core, você pode instanciar uma `PhysicalFileProvider` provedor diretamente, ou você pode solicitar uma `IFileProvider` em um controlador ou o construtor do serviço por meio de [injeção de dependência](dependency-injection.md). A segunda abordagem normalmente produzirá uma solução mais flexível e pode ser testada.

O exemplo a seguir mostra como criar um `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Você pode iterar por meio de seu conteúdo do diretório ou obter informações do arquivo específico, fornecendo um subcaminho.

Para solicitar um provedor de um controlador, especificá-lo no construtor do controlador e atribuí-la a um campo local. Use a instância local de métodos de ação:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Em seguida, criar o provedor no aplicativo do `Startup` classe:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

No *cshtml* exibir, percorrer o `IDirectoryContents` fornecido:

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

O resultado:

![Pastas e arquivos físicos do provedor exemplo aplicativo listagem de arquivos](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

O `EmbeddedFileProvider` é usada para acessar arquivos incorporados em assemblies. No núcleo do .NET, você deve inserir arquivos em um assembly com o `<EmbeddedResource>` elemento o *. csproj* arquivo:

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Você pode usar [padrões de globalização](#globbing-patterns) ao especificar arquivos para inserir no assembly. Esses padrões podem ser usados para corresponder a um ou mais arquivos.

> [!NOTE]
> É improvável que você nunca deseja inserir, na verdade, todos os arquivos. js em seu projeto em seu assembly; o exemplo acima é apenas a fins de demonstração.

Ao criar um `EmbeddedFileProvider`, passar o assembly será exibido para o construtor.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

O trecho de código acima demonstra como criar um `EmbeddedFileProvider` com acesso ao assembly em execução no momento.

Atualizando o aplicativo de exemplo para usar um `EmbeddedFileProvider` resulta na seguinte saída:

![Aplicativo de exemplo de provedor de arquivo listando arquivos incorporados](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Recursos incorporados não expõem diretórios. Em vez disso, o caminho para o recurso (por meio de seu namespace) é inserido em seu nome de arquivo usando `.` separadores.

> [!TIP]
> O `EmbeddedFileProvider` construtor aceita um opcional `baseNamespace` parâmetro. Especificar isso será escopo chamadas para `GetDirectoryContents` a esses recursos sob o namespace fornecido.

### <a name="compositefileprovider"></a>CompositeFileProvider

O `CompositeFileProvider` combina `IFileProvider` instâncias, expor uma interface única para trabalhar com arquivos de vários provedores. Ao criar o `CompositeFileProvider`, passar um ou mais `IFileProvider` instâncias para o construtor:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Atualizando o aplicativo de exemplo para usar um `CompositeFileProvider` que inclui ambos os físicos e incorporados provedores configurados anteriormente, o que resulta na seguinte saída:

![Aplicativo de exemplo de provedor de arquivo listando arquivos físicos e pastas e arquivos incorporados](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Monitorar alterações

O `IFileProvider` `Watch` método fornece uma maneira de observar um ou mais arquivos ou pastas para que as alterações. Esse método aceita uma cadeia de caracteres de caminho, o que pode usar [padrões de globalização](#globbing-patterns) para especificar vários arquivos e retorna um `IChangeToken`. Esse token expõe um `HasChanged` propriedade que pode ser inspecionada, e um `RegisterChangeCallback` método é chamado quando forem detectadas alterações para a cadeia de caracteres do caminho especificado. Observe que cada token de alteração apenas chama o retorno de chamada associado em resposta a uma única alteração. Para habilitar o monitoramento constante, você pode usar um `TaskCompletionSource` conforme mostrado abaixo, ou recrie `IChangeToken` instâncias em resposta a alterações.

No exemplo deste artigo, um aplicativo de console é configurado para exibir uma mensagem sempre que um arquivo de texto é modificado:

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

O resultado, após salvar o arquivo várias vezes:

![Janela de comando após a execução dotnet executar mostra o arquivo quotes.txt para alterações de monitoramento de aplicativos e que o arquivo foi alterado cinco vezes.](file-providers/_static/watch-console.png)

> [!NOTE]
> Alguns sistemas de arquivos, como contêineres do Docker e compartilhamentos de rede, podem não enviar notificações de alteração de forma confiável. Definir o `DOTNET_USE_POLLINGFILEWATCHER` variável de ambiente `1` ou `true` para sondar o sistema de arquivos para que as alterações a cada quatro segundos.

## <a name="globbing-patterns"></a>Padrões de globalização

Caminhos de sistema de arquivos usam padrões de curinga chamados *padrões de globalização*. Esses padrões simples podem ser usados para especificar grupos de arquivos. Os dois caracteres curinga são `*` e `**`.

**`*`**

   Corresponde qualquer coisa no nível da pasta atual, ou qualquer nome de arquivo ou qualquer extensão de arquivo. Correspondências são finalizadas pelo `/` e `.` caracteres no caminho do arquivo.

<strong><code>**</code></strong>

   Coincide com qualquer coisa entre vários níveis de diretório. Pode ser usada para recursivamente corresponder muitos arquivos dentro de uma hierarquia de diretórios.

### <a name="globbing-pattern-examples"></a>Exemplos de padrão de globalização

**`directory/file.txt`**

   Corresponde a um arquivo específico em um diretório específico.

**<code>directory/*.txt</code>**

   Corresponde a todos os arquivos com `.txt` extensão em um diretório específico.

**`directory/*/bower.json`**

   Corresponde a todos os `bower.json` arquivos em diretórios exatamente um nível abaixo de `directory` directory.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Corresponde a todos os arquivos com `.txt` extensão encontrada em qualquer lugar no `directory` directory.

## <a name="file-provider-usage-in-aspnet-core"></a>Uso do provedor de arquivo no núcleo do ASP.NET

Várias partes do ASP.NET Core utilizam provedores de arquivo. `IHostingEnvironment`expõe o aplicativo raiz do conteúdo e a raiz da web como `IFileProvider` tipos. O middleware de arquivos estáticos usa provedores de arquivo para localizar arquivos estáticos. Razor faz uso intenso de `IFileProvider` localizar os modos de exibição. Dotnet publica provedores de arquivo usa a funcionalidade e padrões de globalização para especificar quais arquivos devem ser publicados.

## <a name="recommendations-for-use-in-apps"></a>Recomendações para uso em aplicativos

Se seu aplicativo ASP.NET Core requer acesso de sistema de arquivos, você pode solicitar uma instância de `IFileProvider` injeção de dependência e, em seguida, usar seus métodos para executar o acesso, conforme mostrado neste exemplo. Isso permite que você configure o provedor de uma vez, quando o aplicativo é iniciado e reduz o número de tipos de implementação que cria uma instância do seu aplicativo.
