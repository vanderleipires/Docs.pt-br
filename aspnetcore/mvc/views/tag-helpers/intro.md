---
title: "Auxiliares de marcação no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba quais são os auxiliares de marcação e como usá-los em ASP.NET Core."
keywords: "ASP.NET Core, auxiliares de marcação"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10471210075dc8a5366b7d5170d6594c2e66ce94
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>Introdução ao auxiliares de marcação no núcleo do ASP.NET 

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Quais são os auxiliares de marcação?

Auxiliares de marcação permitem que o código do lado do servidor participar de criação e renderização de elementos HTML em arquivos do Razor. Por exemplo, o interno `ImageTagHelper` pode acrescentar um número de versão para o nome da imagem. Sempre que a imagem é alterada, o servidor gera uma nova versão exclusiva para a imagem, para que os clientes têm garantia de obter a imagem atual (em vez de uma imagem em cache obsoleta). Há muitos auxiliares de marcação interna para tarefas comuns - como a criação de formulários, links, ativos de carregamento e pacotes mais - e ainda mais disponíveis em repositórios GitHub públicos e como NuGet. Auxiliares de marca são criados no c#, e eles se destinam a elementos HTML com base no nome do elemento, o nome do atributo ou marca pai. Por exemplo, o interno `LabelTagHelper` pode direcionar o HTML `<label>` elemento quando o `LabelTagHelper` atributos são aplicados. Se você estiver familiarizado com [auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), auxiliares de marcação reduzir as transições explícitas entre HTML e c# em modos de exibição do Razor. Em muitos casos, auxiliares HTML fornecem uma abordagem alternativa para um auxiliar de marca específica, mas é importante reconhecer que auxiliares de marcação não substituem auxiliares HTML e não é um auxiliar de marca para cada auxiliar HTML. [Em comparação comparados auxiliares HTML de auxiliares de marcação](#tag-helpers-compared-to-html-helpers) explica as diferenças em mais detalhes.

## <a name="what-tag-helpers-provide"></a>O que fornece os auxiliares de marcação

**Uma experiência de desenvolvimento compatível com HTML** para a maior parte, marcação Razor usando auxiliares de marcação parece HTML padrão. Designers de front-end familiarizadas com HTML/CSS/JavaScript podem editar Razor sem aprender a sintaxe do Razor c#.

**Um ambiente rico de IntelliSense para criar marcação HTML e Razor** está em contraste a auxiliares HTML, a abordagem anterior para a criação do servidor de marcação nos modos de exibição do Razor. [Em comparação comparados auxiliares HTML de auxiliares de marcação](#tag-helpers-compared-to-html-helpers) explica as diferenças em mais detalhes. [Suporte ao IntelliSense para os auxiliares de marcação](#intellisense-support-for-tag-helpers) explica o ambiente do IntelliSense. Até mesmo desenvolvedores experientes com sintaxe Razor c# são mais produtivos usando auxiliares de marcação que escrever marcação Razor c#.

**Uma maneira de fazer a você mais produtivos e pode produzir mais robusta e confiável e um código usando as informações disponíveis apenas no servidor** , por exemplo, historicamente mantra imagens a atualização foi alterar o nome da imagem quando você alterar a imagem. Imagens devem ter cache agressivamente por motivos de desempenho e, em seguida, a menos que você altere o nome de uma imagem, você corre o risco de clientes obtendo uma cópia obsoleta. Historicamente, depois que uma imagem foi editada, o nome foram alterados e cada referência à imagem no aplicativo web precisa ser atualizado. Não só é muito trabalho intensivas, também é propensa (você pode perder uma referência, acidentalmente, insira a cadeia de caracteres incorreta, etc.) O interno `ImageTagHelper` pode fazer isso para você automaticamente. O `ImageTagHelper` pode acrescentar uma versão de número para o nome da imagem, portanto, sempre que a imagem é alterada, o servidor gera automaticamente uma nova versão exclusiva para a imagem. Os clientes têm a garantia para obter a imagem atual. Esse aumento robustez e trabalho vem essencialmente livre usando o `ImageTagHelper`.

A maioria os auxiliares de marca internos elementos HTML existentes de destino e fornece os atributos do lado do servidor para o elemento. Por exemplo, o `<input>` elemento usado em muitas das exibições no *modos de exibição/conta* pasta contém a `asp-for` atributo, que extrai o nome da propriedade do modelo especificado no HTML renderizado. A seguinte marcação Razor:

```html
<label asp-for="Email"></label>
```

Gera o HTML a seguir:

```html
<label for="Email">Email</label>
```

O `asp-for` atributo é disponibilizado pelo `For` propriedade o `LabelTagHelper`. Consulte [criação auxiliares de marcação](authoring.md) para obter mais informações.

## <a name="managing-tag-helper-scope"></a>Gerenciamento de escopo do auxiliar de marca

Escopo de auxiliares de marca é controlado por uma combinação de `@addTagHelper`, `@removeTagHelper`e "!" recusar caractere.

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`disponibiliza auxiliares de marcação

Se você criar um novo aplicativo de web do ASP.NET Core denominado *AuthoringTagHelpers* (com nenhuma autenticação), o seguinte *Views/_ViewImports.cshtml* arquivo será adicionado ao seu projeto:

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

O `@addTagHelper` diretiva disponibiliza auxiliares de marcação para o modo de exibição. Nesse caso, o arquivo de exibição é *Views/_ViewImports.cshtml*, que por padrão é herdada por todos os arquivos de exibição no *exibições* pasta e seus subdiretórios; disponibilizar auxiliares de marcação. O código anterior usa a sintaxe de curinga ("\*") para especificar que todos os auxiliares de marcação no assembly especificado (*Microsoft.AspNetCore.Mvc.TagHelpers*) estarão disponíveis para todos os arquivos no modo de exibição de *exibições* diretório ou subdiretório. O primeiro parâmetro após `@addTagHelper` Especifica os auxiliares de marca para carregar (estamos usando "\*" para todos os auxiliares de marcação), e o segundo parâmetro "Microsoft.AspNetCore.Mvc.TagHelpers" Especifica o assembly que contém os auxiliares de marca. *Microsoft.AspNetCore.Mvc.TagHelpers* é o assembly para os auxiliares de marca de núcleo ASP.NET interno.

Para expor todos os auxiliares de marca neste projeto (que cria um assembly chamado *AuthoringTagHelpers*), você usaria o seguinte:

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Se o seu projeto contém um `EmailTagHelper` com o namespace padrão (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), você pode fornecer o nome totalmente qualificado (FQN) do auxiliar de marca:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Para adicionar um auxiliar de marca para uma exibição usando um FQN, primeiro adicione o FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e, em seguida, o nome do assembly (*AuthoringTagHelpers*). A maioria dos desenvolvedores preferem usar o "\*" sintaxe curingas. A sintaxe de caractere curinga permite que você insira o caractere curinga "\*" como o sufixo de um FQN. Por exemplo, qualquer um dos seguintes diretivas trará o `EmailTagHelper`:

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Conforme mencionado anteriormente, adicionando o `@addTagHelper` diretiva para o *Views/_ViewImports.cshtml* arquivo disponibiliza o auxiliar de marca para todos os arquivos de exibição no *exibições* diretório e subdiretórios. Você pode usar o `@addTagHelper` diretiva nos arquivos de modo de exibição específico se você deseja participar para expor o auxiliar de marca para apenas esses modos de exibição.

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`Remove os auxiliares de marcação

O `@removeTagHelper` tem os mesmos dois parâmetros `@addTagHelper`, e remove um auxiliar de marca foi adicionado anteriormente. Por exemplo, `@removeTagHelper` aplicado para remover um modo de exibição específico o auxiliar de marca especificado da exibição. Usando `@removeTagHelper` em uma *Views/Folder/_ViewImports.cshtml* arquivo remove o auxiliar de marca especificado de todas as exibições em *pasta*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Controlando o escopo do auxiliar de marca com o *viewimports. cshtml* arquivo

Você pode adicionar uma *viewimports. cshtml* para qualquer pasta de exibição e o modo de exibição mecanismo aplica as diretivas de ambas desse arquivo e o *Views/_ViewImports.cshtml* arquivo. Se você adicionou um vazio *Views/Home/_ViewImports.cshtml* de arquivos para o *início* exibições, não haveria nenhuma alteração porque o *viewimports. cshtml* arquivo é aditivas. Qualquer `@addTagHelper` diretivas que você adicionar à *Views/Home/_ViewImports.cshtml* arquivo (que não estão no padrão *Views/_ViewImports.cshtml* arquivo) poderia expor os auxiliares de marcação para modos de exibição somente no *Início* pasta.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Recusando elementos individuais

Você pode desabilitar um auxiliar de marca no nível do elemento com o caractere de recusar auxiliar de marca ("!"). Por exemplo, `Email` validação está desabilitada no `<span>` com o caractere de recusar auxiliar de marca:

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Você deve aplicar o caractere de recusar auxiliar de marca para a marca de abertura e fechamento. (O editor do Visual Studio adiciona automaticamente o caractere de saída para a marca de fechamento quando você adiciona um para a marca de abertura). Depois de adicionar o caractere de recusa, o elemento e os atributos do auxiliar de marca não são exibidos em uma fonte diferente.

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Usando `@tagHelperPrefix` para fazer uso do auxiliar de marca explícita

O `@tagHelperPrefix` diretiva permite que você especifique uma cadeia de caracteres de prefixo de marca para habilitar o suporte auxiliar de marca e fazer uso do auxiliar de marca explícita. Por exemplo, você pode adicionar a seguinte marcação para o *Views/_ViewImports.cshtml* arquivo:

```html
@tagHelperPrefix th:
```
A imagem do código abaixo, o prefixo de marca auxiliar é definido como `th:`, portanto, somente esses elementos usando o prefixo `th:` suporte auxiliares de marcação (elementos de auxiliar de marca tem uma fonte diferente). O `<label>` e `<input>` elementos têm o prefixo de marca auxiliar e são habilitados para auxiliar de marca, enquanto o `<span>` elemento não.

![imagem](intro/_static/thp.png)

As mesmas regras de hierarquia que se aplicam a `@addTagHelper` também se aplicam a `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Suporte ao IntelliSense para os auxiliares de marcação

Quando você cria um novo aplicativo web ASP.NET no Visual Studio, ele adiciona o pacote NuGet "Microsoft.AspNetCore.Razor.Tools". Este é o pacote que adiciona ferramentas de auxiliar de marca.

Considere a possibilidade de gravar uma marca HTML `<label>` elemento. Assim que você inserir `<l` no editor do Visual Studio, o IntelliSense exibe elementos correspondentes:

![imagem](intro/_static/label.png)

Não só você para obter ajuda em HTML, mas o ícone (o "@" símbolo com "<>" sob ele).

![imagem](intro/_static/tagSym.png)

identifica o elemento como alvo de auxiliares de marcação. Elementos HTML puros (como o `fieldset`) exibem o ícone de "<>".

Um HTML puro `<label>` marca exibe a marca HTML (com o tema de cores do Visual Studio padrão) em uma fonte marrom, os atributos em vermelho, e os valores de atributo em azul.

![imagem](intro/_static/LableHtmlTag.png)

Depois de inserir `<label`, IntelliSense listará os atributos HTML/CSS disponíveis e os atributos voltados para o auxiliar de marca:

![imagem](intro/_static/labelattr.png)

Preenchimento de instruções IntelliSense permite que você insira a tecla tab para completar a instrução com o valor selecionado:

![imagem](intro/_static/stmtcomplete.png)

Assim que um atributo do auxiliar de marca é inserido, alterar as fontes de marca e atributo. Usando o padrão Visual Studio "Blue" ou tema de cores "Light", a fonte está em negrito roxo. Se você estiver usando o tema "Escuro" a fonte está em negrito azul-petróleo. As imagens neste documento foram feitas usando o tema padrão.

![imagem](intro/_static/labelaspfor2.png)

Você pode inserir o Visual Studio *Palcompleta* atalho (Ctrl + Barra de espaço é o [padrão](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) dentro de aspas duplas (""), e você está agora no c#, exatamente como você deve estar em uma classe c#. O IntelliSense exibe todos os métodos e propriedades no modelo de página. Os métodos e propriedades estão disponíveis porque o tipo de propriedade é `ModelExpression`. Na imagem abaixo, eu estou editando o `Register` exibição, portanto, o `RegisterViewModel` está disponível.

![imagem](intro/_static/intellemail.png)

IntelliSense listará as propriedades e métodos disponíveis para o modelo na página. O ambiente de IntelliSense avançado ajuda a selecionar a classe CSS:

![imagem](intro/_static/iclass.png)

![imagem](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Auxiliares de marcação em comparação comparados auxiliares HTML

Anexar auxiliares de marcação para elementos HTML em exibições Razor, enquanto [auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) são invocados como métodos Intercalado com HTML nos modos de exibição do Razor. Considere a seguinte marcação Razor, que cria um rótulo HTML com a classe CSS "legenda":

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

O em (`@`) símbolo informa Razor, este é o início do código. Os dois parâmetros ("FirstName" e "nome:") são cadeias de caracteres, portanto [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) não pode ajudar. O último argumento:

```html
new {@class="caption"}
```

Um objeto anônimo é usado para representar os atributos. Porque **classe** é uma palavra reservada no c#, você deve usar o `@` símbolo forçar c# para interpretar "@class=" como um símbolo (nome da propriedade). Para um designer de front-end (alguém familiarizado com HTML/CSS/JavaScript e outras tecnologias de cliente, mas não está familiarizado com c# e Razor), a maior parte da linha é externa. Toda a linha deve ser criada com nenhuma ajuda do IntelliSense.

Usando o `LabelTagHelper`, a mesma marcação pode ser escrita como:

![imagem](intro/_static/label2.png)

Com a versão do auxiliar de marca, assim que você inserir `<l` no editor do Visual Studio, o IntelliSense exibe elementos correspondentes:

![imagem](intro/_static/label.png)

IntelliSense ajuda você a escrever a linha inteira. O `LabelTagHelper` também padrão é definir o conteúdo do `asp-for` ("FirstName") do valor do atributo "Primeiro nome"; Ele converte propriedades concatenados em uma frase composta do nome da propriedade com um espaço onde ocorre a cada nova letra maiuscula. Na marcação a seguir:

![imagem](intro/_static/label2.png)

gera:

```html
<label class="caption" for="FirstName">First Name</label>
```

O concatenados para conteúdo de maiusculas e minúsculas frase não será usado se você adicionar conteúdo para o `<label>`. Por exemplo:

![imagem](intro/_static/1stName.png)

gera:

```html
<label class="caption" for="FirstName">Name First</label>
```

A imagem a seguir mostra a parte do formato da *Views/Account/Register.cshtml* exibição Razor gerada a partir de herdado ASP.NET 4.5 MVC modelo incluído com o Visual Studio 2015.

![imagem](intro/_static/regCS.png)

Editor do Visual Studio exibe o código c# com um plano de fundo cinza. Por exemplo, o `AntiForgeryToken` auxiliar HTML:

```html
@Html.AntiForgeryToken()
```

é exibido com um plano de fundo cinza. A maioria da marcação na exibição de registro é c#. Compare isso com a abordagem equivalente usando auxiliares de marca:

![imagem](intro/_static/regTH.png)

A marcação é muito limpas e fáceis de ler, editar e manter-se que a abordagem de auxiliares HTML. O código c# é reduzido ao mínimo que o servidor precisa conhecer. Editor do Visual Studio exibe marcação direcionada por um auxiliar de marca em uma fonte diferente.

Considere o *Email* grupo:

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

Cada um dos atributos "asp-" tem um valor de "Email", mas "Email" não é uma cadeia de caracteres. Nesse contexto, "Email" é a c# expressão propriedade do modelo para o `RegisterViewModel`.

O editor do Visual Studio ajuda você a escrever **todas as** da marcação no método auxiliar de marca de formulário de registro, enquanto o Visual Studio não fornece nenhuma ajuda para a maioria do código na abordagem auxiliares HTML. [Suporte ao IntelliSense para os auxiliares de marcação](#intellisense-support-for-tag-helpers) apresenta detalhes sobre como trabalhar com auxiliares de marcação no editor do Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Auxiliares de marcação em comparação comparados controles de servidor Web

* Auxiliares de marcação não tem o elemento que estiverem associados; simplesmente participam no processamento do elemento e conteúdo. ASP.NET [controles de servidor Web](https://msdn.microsoft.com/library/7698y1f0.aspx) são declarados e invocado em uma página.

* [Controles de servidor Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) têm um ciclo de vida não trivial que pode dificultar o desenvolvimento e depuração.

* Controles de servidor Web permitem que você adicionar a funcionalidade para os elementos de modelo de objeto de documento (DOM) do cliente usando um controle de cliente. Auxiliares de marcação não tem nenhum DOM.

* Controles de servidor Web incluem a detecção automática do navegador. Auxiliares de marcação não têm nenhum conhecimento do navegador.

* Vários auxiliares de marcação pode agir no mesmo elemento (consulte [conflitos evitando auxiliar de marca](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) enquanto você normalmente não é possível compor controles de servidor Web.

* Auxiliares de marcação podem modificar a marca e o conteúdo de elementos HTML que eles estiverem no escopo, mas não modifique diretamente tudo em uma página. Controles de servidor Web tem um escopo de menos específico e podem executar ações que afetam outras partes da página; Habilitando efeitos colaterais não intencionais.

* Controles de servidor Web usam conversores de tipo para converter cadeias de caracteres em objetos. Com auxiliares de marca, você trabalha nativamente em c#, portanto você não precisa de conversão de tipo.

* Uso de controles de servidor Web [System. ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) para implementar o comportamento de tempo de execução e tempo de design de componentes e controles. `System.ComponentModel`inclui as classes e interfaces base para implementar atributos e conversores de tipo, associando a dados de fontes e licenciamento de componentes. Compare isso com a auxiliares de marca, que normalmente derivam `TagHelper`e o `TagHelper` classe base expõe apenas dois métodos, `Process` e `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Personalizando a fonte de elemento do auxiliar de marca

Você pode personalizar a fonte e a coloração de **ferramentas** > **opções** > **ambiente** > **fontes Cores e**:

![imagem](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Criando auxiliares de marcação](authoring.md)
* [Trabalhando com formulários](../working-with-forms.md)
* [TagHelperSamples no GitHub](https://github.com/dpaquette/TagHelperSamples) contém exemplos de auxiliar de marca para trabalhar com [inicialização](http://getbootstrap.com/).

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
