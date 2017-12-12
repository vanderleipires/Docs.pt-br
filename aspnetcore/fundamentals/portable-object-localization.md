---
title: "Configurar a localização do objeto portátil"
author: sebastienros
description: "Este artigo apresenta arquivos de objeto portátil e descreve as etapas para usá-los em um aplicativo ASP.NET Core com o framework Core Orchard."
keywords: "ASP.NET Core, localização, cultura, idioma, objeto portátil"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Configurar a localização do objeto portátil com Orchard Core

Por [Sébastien Ros](https://github.com/sebastienros) e [Scott Addie](https://twitter.com/Scott_Addie)

Este artigo explica as etapas para usar arquivos de objeto portátil (PO) em um aplicativo ASP.NET Core com o [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Observação:** Orchard principal não é um produto da Microsoft. Consequentemente, Microsoft não fornece suporte para esse recurso.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>O que é um arquivo de ordem de compra?

Arquivos de PO são distribuídos como arquivos de texto que contém as cadeias de caracteres traduzidas para um determinado idioma. Algumas vantagens de usar os arquivos de PO em vez disso, *. resx* arquivos incluem:
- Arquivos de PO oferecem suporte a pluralização; *. resx* arquivos não dão suporte a pluralização.
- PO arquivos não são compilados como *. resx* arquivos. Como tal, especializadas etapas de compilação e de ferramentas não são necessárias.
- Os arquivos de PO funcionam bem com ferramentas de colaboração para edição online.

### <a name="example"></a>Exemplo

Aqui está um arquivo de ordem de compra de exemplo que contém a tradução de duas cadeias de caracteres em francês, incluindo um com sua forma plural:

*FR.po*

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

Este exemplo usa a seguinte sintaxe:

- `#:`: Um comentário que indica o contexto da cadeia de caracteres a ser convertido. A mesma cadeia de caracteres pode ser convertida diferente dependendo de onde ele está sendo usado.
- `msgid`: A cadeia de caracteres não traduzida.
- `msgstr`: A cadeia de caracteres traduzida.

No caso de suporte a pluralização, mais entradas podem ser definidas.

- `msgid_plural`: A cadeia de caracteres no plural não traduzida.
- `msgstr[0]`: A cadeia de caracteres traduzida para o caso de 0.
- `msgstr[N]`: A cadeia de caracteres traduzida para o n maiusculas

A especificação de arquivo de ordem de compra pode ser encontrada [aqui](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Configurando o suporte a arquivo PO no núcleo do ASP.NET

Este exemplo é baseado em um aplicativo MVC do ASP.NET Core gerado a partir de um modelo de projeto do Visual Studio de 2017.

### <a name="referencing-the-package"></a>Referência do pacote

Adicione uma referência para o `OrchardCore.Localization.Core` pacote NuGet. Ele está disponível em [MyGet](https://www.myget.org/) na origem do pacote a seguir: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

O *. csproj* arquivo agora contém uma linha semelhante à seguinte (o número de versão pode variar):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Registrar o serviço

Adicionar os serviços necessários para o `ConfigureServices` método *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Adicionar o middleware necessário para o `Configure` método *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Adicione o seguinte código ao modo de exibição Razor de escolha. *About.cshtml* é usado neste exemplo.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

Um `IViewLocalizer` instância é injetada e usada para converter o texto "Olá, mundo!".

### <a name="creating-a-po-file"></a>Criando um arquivo de ordem de compra

Crie um arquivo chamado  *<culture code>. po* na pasta raiz do aplicativo. Neste exemplo, o nome do arquivo é *fr.po* porque o idioma francês é usado:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Esse arquivo armazena a cadeia de caracteres para converter e a cadeia de caracteres convertida em francês. Traduções reverter para a cultura pai, se necessário. Neste exemplo, o *fr.po* arquivo é usado se a cultura solicitada é `fr-FR` ou `fr-CA`.

### <a name="testing-the-application"></a>Testando o aplicativo

Execute o aplicativo e navegue até a URL `/Home/About`. O texto **Olá, mundo!** é exibida.

Navegue até a URL `/Home/About?culture=fr-FR`. O texto **Bon jour le monde!** é exibida.

## <a name="pluralization"></a>Pluralização

Arquivos de PO dão suporte a pluralização formas, que é útil quando a mesma cadeia de caracteres precisa ser traduzido variam de acordo com uma cardinalidade. Essa tarefa é feita complicada pelo fato de que cada linguagem define regras personalizadas para selecionar a cadeia de caracteres que para usar com base na cardinalidade.

O pacote de localização Orchard fornece uma API para invocar esses diferentes formas plurais automaticamente.

### <a name="creating-pluralization-po-files"></a>Criando a pluralização arquivos PO

Adicione o seguinte conteúdo para mencionadas anteriormente *fr.po* arquivo:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Consulte [o que é um arquivo de PO?](#what-is-a-po-file) para obter uma explicação do que representa cada entrada neste exemplo.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Adicionar um idioma usando formulários pluralização diferentes

Cadeias de caracteres em inglês e francês foram usadas no exemplo anterior. Inglês e francês tem apenas dois formulários de pluralização e compartilham as mesmas regras de formulário, que é que uma cardinalidade de um é mapeada para a primeira forma plural. Qualquer outra cardinalidade é mapeada para a segunda forma plural.

Nem todos os idiomas compartilham as mesmas regras. Isso é ilustrado com o idioma tcheco, que tem três formas no plural.

Criar o `cs.po` arquivos da seguinte maneira e observe como a pluralização precisa três traduções diferentes:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Para aceitar as localizações Tcheca, adicionar `"cs"` à lista de culturas com suporte a `ConfigureServices` método:

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

Editar o *Views/Home/About.cshtml* arquivo renderizar localizadas no plural cadeias de caracteres para várias cardinalidades:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Observação:** em um cenário do mundo real, uma variável deve ser usada para representar a contagem. Aqui, podemos Repita o mesmo código com três valores diferentes para expor um caso muito específico.

Ao alternar culturas, consulte o seguinte:

Para `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Para `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Para `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Observe que, para a cultura Tcheca, as três traduções são diferentes. Culturas francês e inglês compartilham a mesma construção para as cadeias de caracteres traduzidas últimas duas.

## <a name="advanced-tasks"></a>Tarefas avançadas

### <a name="contextualizing-strings"></a>Cadeias de caracteres de contexto

Os aplicativos geralmente contêm cadeias de caracteres a ser convertido em vários locais. A mesma cadeia de caracteres pode ter uma tradução diferente em determinados locais dentro de um aplicativo (exibições Razor ou arquivos de classe). Um arquivo de PO oferece suporte a noção de um contexto de arquivo, que pode ser usado para categorizar a cadeia de caracteres que está sendo representada. Usando um contexto de arquivo, uma cadeia de caracteres pode ser convertida de forma diferente, dependendo do contexto do arquivo (ou falta de um contexto de arquivo).

Os serviços de localização de PO usam o nome da classe completa ou o modo de exibição que é usado ao converter uma cadeia de caracteres. Isso é feito definindo o valor sobre o `msgctxt` entrada.

Considere uma adição secundária para a versão anterior *fr.po* exemplo. Um modo de exibição Razor localizado em *Views/Home/About.cshtml* pode ser definida como o contexto do arquivo definindo reservado `msgctxt` valor da entrada:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Com o `msgctxt` definido como tal, a conversão de texto ocorre ao navegar para `/Home/About?culture=fr-FR`. A conversão não ocorre ao navegar para `/Home/Contact?culture=fr-FR`.

Quando nenhuma entrada específica é correspondida com um contexto de arquivo fornecido, o mecanismo de fallback do núcleo Orchard procura um arquivo de PO apropriado sem contexto. Supondo que não há não é definido para nenhum contexto de arquivo específico *Views/Home/Contact.cshtml*, navegando para `/Home/Contact?culture=fr-FR` carrega um arquivo de ordem de compra, como:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Alterar o local dos arquivos de PO

O local padrão dos arquivos de ordem de compra pode ser alterado em `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

Neste exemplo, os arquivos de PO são carregados do *localização* pasta.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementar uma lógica personalizada para localizar arquivos de localização

Quando uma lógica mais complexa é necessária para localizar arquivos de ordem de compra, o `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface pode ser implementada e registrada como um serviço. Isso é útil quando PO arquivos podem ser armazenados em variáveis locais ou quando os arquivos precisam ser encontrada dentro de uma hierarquia de pastas.

### <a name="using-a-different-default-pluralized-language"></a>Usando um idioma padrão diferente pluralized

O pacote inclui um `Plural` método de extensão que é específico para duas formas no plural. Para idiomas que exigem mais formulários plural, crie um método de extensão. Com um método de extensão, você não precisa fornecer qualquer arquivo de localização para o idioma padrão &mdash; as cadeias de caracteres originais já estão disponíveis diretamente no código.

Você pode usar mais genérica `Plural(int count, string[] pluralForms, params object[] arguments)` sobrecarga que aceita uma matriz de cadeia de caracteres de traduções.