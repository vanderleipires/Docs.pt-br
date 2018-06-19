---
title: Provedores de arquivos no ASP.NET Core
author: ardalis
description: Saiba como o ASP.NET Core abstrai o acesso ao sistema de arquivos por meio do uso de provedores de arquivos.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29724565"
---
# <a name="file-providers-in-aspnet-core"></a>Provedores de arquivos no ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

O ASP.NET Core abstrai o acesso ao sistema de arquivos por meio do uso de provedores de arquivos.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Abstrações de provedor de arquivos

Provedores de arquivos são uma abstração dos sistemas de arquivos. A interface principal é `IFileProvider`. `IFileProvider` expõe métodos para obter informações sobre o arquivo (`IFileInfo`), informações sobre o diretório (`IDirectoryContents`) e para configurar notificações de alteração (usando um `IChangeToken`).

`IFileInfo` fornece métodos e propriedades sobre arquivos ou diretórios individuais. Ele tem duas propriedades boolianas, `Exists` e `IsDirectory`, bem como propriedades que descrevem o `Name` do arquivo, `Length` (em bytes) e a data de `LastModified`. É possível ler do arquivo usando seu método `CreateReadStream`.

## <a name="file-provider-implementations"></a>Implementações do provedor de arquivos

Três implementações de `IFileProvider` estão disponíveis: Físico, Inserido e Composto. O provedor físico é usado para acessar os arquivos do sistema de fato. O provedor inserido é usado para acessar arquivos inseridos em assemblies. O provedor composto é usado para fornecer acesso combinado a arquivos e diretórios de um ou mais provedores.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

O `PhysicalFileProvider` fornece acesso ao sistema de arquivos físico. Ele encapsula o tipo `System.IO.File` (para o provedor físico), tendo como escopo todos os caminhos para um diretório e seus filhos. Esse escopo limita o acesso a um determinado diretório e seus filhos, impedindo o acesso ao sistema de arquivos fora desse limite. Ao instanciar esse provedor, você deve fornecer a ele um caminho de diretório, que serve como o caminho base para todas as solicitações feitas a esse provedor (e que restringe o acesso de fora desse caminho). Em um aplicativo do ASP.NET Core, você pode instanciar um provedor `PhysicalFileProvider` diretamente, ou pode solicitar um `IFileProvider` em um Controlador ou o construtor do serviço por meio da [injeção de dependência](dependency-injection.md). A segunda abordagem normalmente produzirá uma solução mais flexível e passível de ser testada.

O exemplo a seguir mostra como criar um `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Você pode iterar no conteúdo do diretório ou obter as informações sobre o arquivo específico fornecendo um subcaminho.

Para solicitar um provedor de um controlador, especifique-o no construtor do controlador e atribua-o a um campo local. Use a instância local de seus métodos de ação:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Em seguida, crie o provedor na classe `Startup` do aplicativo:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

Na exibição *Index.cshtml*, itere no `IDirectoryContents` fornecido:

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

O resultado:

![Aplicativo de exemplo do provedor de arquivos listando arquivos e pastas físicas](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

O `EmbeddedFileProvider` é usado para acessar arquivos inseridos em assemblies. No .NET Core, você insere arquivos em um assembly com o elemento `<EmbeddedResource>` no arquivo *.csproj*:

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Você pode usar [padrões recurso de curinga](#globbing-patterns) ao especificar arquivos para serem inseridos no assembly. Esses padrões podem ser usados para corresponder a um ou mais arquivos.

> [!NOTE]
> É improvável que você de fato queira inserir todos os arquivos .js do projeto em seu assembly; o exemplo acima é apenas a fins de demonstração.

Ao criar um `EmbeddedFileProvider`, passe o assembly que ele lerá para o construtor.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

O trecho de código acima demonstra como criar um `EmbeddedFileProvider` com acesso ao assembly em execução no momento.

Atualizar o aplicativo de exemplo para usar um `EmbeddedFileProvider` resulta na seguinte saída:

![Aplicativo de exemplo do provedor de arquivos listando arquivos inseridos](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Recursos inseridos não expõem diretórios. Em vez disso, o caminho para o recurso (por meio de seu namespace) é inserido no nome de arquivo usando separadores `.`.

> [!TIP]
> O construtor `EmbeddedFileProvider` aceita um parâmetro `baseNamespace` opcional. Especificar esse parâmetro definirá o escopo de chamadas para `GetDirectoryContents` para esses recursos sob o namespace fornecido.

### <a name="compositefileprovider"></a>CompositeFileProvider

O `CompositeFileProvider` combina instâncias de `IFileProvider`, expondo uma interface única para trabalhar com arquivos de vários provedores. Ao criar o `CompositeFileProvider`, você passa uma ou mais instâncias de `IFileProvider` para o construtor:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Atualizar o aplicativo de exemplo para usar um `CompositeFileProvider` que inclui os provedores físico e inserido configurados anteriormente resulta na seguinte saída:

![Aplicativo de exemplo do provedor de arquivos listando arquivos e pastas físicas e arquivos inseridos](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Monitorando alterações

O método `IFileProvider` `Watch` proporciona uma maneira de monitorar um ou mais arquivos ou diretórios quanto a alterações. Esse método aceita uma cadeia de caracteres de caminho, que pode usar [padrões de recurso de curinga](#globbing-patterns) para especificar vários arquivos e retorna um `IChangeToken`. Esse token expõe uma propriedade `HasChanged` que pode ser inspecionada e um método `RegisterChangeCallback` que é chamado quando são detectadas alterações na cadeia de caracteres do caminho especificado. Observe que cada token de alteração chama apenas seu retorno de chamada associado em resposta a uma única alteração. Para habilitar o monitoramento constante, você pode usar um `TaskCompletionSource` conforme mostrado abaixo ou recriar instâncias de `IChangeToken` em resposta a alterações.

No exemplo deste artigo, um aplicativo de console é configurado para exibir uma mensagem sempre que um arquivo de texto é modificado:

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

O resultado, após salvar o arquivo várias vezes:

![A janela Comando após a execução do dotnet mostra o aplicativo monitorando o arquivo quotes.txt quanto a alterações e mostra que o arquivo foi alterado cinco vezes.](file-providers/_static/watch-console.png)

> [!NOTE]
> Alguns sistemas de arquivos, como contêineres do Docker e compartilhamentos de rede, podem não enviar notificações de alteração de forma confiável. Defina a variável de ambiente `DOTNET_USE_POLLINGFILEWATCHER` como `1` ou `true` para sondar o sistema de arquivos a cada quatro segundos em relação a alterações.

## <a name="globbing-patterns"></a>Padrões de recurso de curinga

Caminhos de sistema de arquivos usam padrões de curinga chamados *padrões de recurso de curinga*. Esses padrões simples podem ser usados para especificar grupos de arquivos. Os dois caracteres curinga são `*` e `**`.

**`*`**

   Corresponde a qualquer coisa no nível da pasta atual, ou qualquer nome de arquivo ou qualquer extensão de arquivo. As correspondências são terminadas pelos caracteres `/` e `.` no caminho do arquivo.

<strong><code>**</code></strong>

   Coincide a qualquer coisa em vários níveis de diretório. Pode ser usada para fazer a correspondência recursiva com vários arquivos em uma hierarquia de diretórios.

### <a name="globbing-pattern-examples"></a>Exemplos de padrão de recurso de curinga

**`directory/file.txt`**

   Corresponde a um arquivo específico em um diretório específico.

**<code>directory/*.txt</code>**

   Corresponde a todos os arquivos com a extensão `.txt` em um diretório específico.

**`directory/*/bower.json`**

   Corresponde a todos os arquivos `bower.json` em diretórios que estão exatamente um nível abaixo do diretório `directory`.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Corresponde a todos os arquivos com a extensão `.txt` encontrados em qualquer lugar abaixo do diretório `directory`.

## <a name="file-provider-usage-in-aspnet-core"></a>Uso de provedores de arquivos no ASP.NET Core

Várias partes do ASP.NET Core utilizam provedores de arquivos. `IHostingEnvironment` expõe o conteúdo raiz do aplicativo e a raiz da Web como tipos `IFileProvider`. O middleware de arquivos estáticos usa provedores de arquivos para localizar arquivos estáticos. O Razor usa `IFileProvider` intensamente para localizar exibições. A funcionalidade de publicação do dotnet usa padrões de recurso de curinga e provedores de arquivos para especificar quais arquivos devem ser publicados.

## <a name="recommendations-for-use-in-apps"></a>Recomendações para uso em aplicativos

Se seu aplicativo ASP.NET Core precisar de acesso ao sistema de arquivos, você pode solicitar uma instância de `IFileProvider` por meio da injeção de dependência e, em seguida, usar seus métodos para executar o acesso, conforme mostrado neste exemplo. Isso permite configurar o provedor uma vez, quando o aplicativo é iniciado, e reduz o número de tipos de implementação para os quais seu aplicativo cria uma instância.
