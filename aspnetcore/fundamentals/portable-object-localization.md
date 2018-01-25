---
title: "Configurar a localização do objeto portátil"
author: sebastienros
description: "Este artigo apresenta arquivos de objeto portátil e descreve as etapas para usá-los em um aplicativo ASP.NET Core com o framework Core Orchard."
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="8e750-103">Configurar a localização do objeto portátil com Orchard Core</span><span class="sxs-lookup"><span data-stu-id="8e750-103">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="8e750-104">Por [Sébastien Ros](https://github.com/sebastienros) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8e750-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8e750-105">Este artigo explica as etapas para usar arquivos de objeto portátil (PO) em um aplicativo ASP.NET Core com o [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="8e750-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="8e750-106">**Observação:** Orchard principal não é um produto da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8e750-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="8e750-107">Consequentemente, Microsoft não fornece suporte para esse recurso.</span><span class="sxs-lookup"><span data-stu-id="8e750-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="8e750-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e750-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="8e750-109">O que é um arquivo de ordem de compra?</span><span class="sxs-lookup"><span data-stu-id="8e750-109">What is a PO file?</span></span>

<span data-ttu-id="8e750-110">Arquivos de PO são distribuídos como arquivos de texto que contém as cadeias de caracteres traduzidas para um determinado idioma.</span><span class="sxs-lookup"><span data-stu-id="8e750-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="8e750-111">Algumas vantagens de usar os arquivos de PO em vez disso, *. resx* arquivos incluem:</span><span class="sxs-lookup"><span data-stu-id="8e750-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="8e750-112">Arquivos de PO oferecem suporte a pluralização; *. resx* arquivos não dão suporte a pluralização.</span><span class="sxs-lookup"><span data-stu-id="8e750-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="8e750-113">PO arquivos não são compilados como *. resx* arquivos.</span><span class="sxs-lookup"><span data-stu-id="8e750-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="8e750-114">Como tal, especializadas etapas de compilação e de ferramentas não são necessárias.</span><span class="sxs-lookup"><span data-stu-id="8e750-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="8e750-115">Os arquivos de PO funcionam bem com ferramentas de colaboração para edição online.</span><span class="sxs-lookup"><span data-stu-id="8e750-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="8e750-116">Exemplo</span><span class="sxs-lookup"><span data-stu-id="8e750-116">Example</span></span>

<span data-ttu-id="8e750-117">Aqui está um arquivo de ordem de compra de exemplo que contém a tradução de duas cadeias de caracteres em francês, incluindo um com sua forma plural:</span><span class="sxs-lookup"><span data-stu-id="8e750-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="8e750-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="8e750-118">*fr.po*</span></span>

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

<span data-ttu-id="8e750-119">Este exemplo usa a seguinte sintaxe:</span><span class="sxs-lookup"><span data-stu-id="8e750-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="8e750-120">`#:`: Um comentário que indica o contexto da cadeia de caracteres a ser convertido.</span><span class="sxs-lookup"><span data-stu-id="8e750-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="8e750-121">A mesma cadeia de caracteres pode ser convertida diferente dependendo de onde ele está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="8e750-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="8e750-122">`msgid`: A cadeia de caracteres não traduzida.</span><span class="sxs-lookup"><span data-stu-id="8e750-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="8e750-123">`msgstr`: A cadeia de caracteres traduzida.</span><span class="sxs-lookup"><span data-stu-id="8e750-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="8e750-124">No caso de suporte a pluralização, mais entradas podem ser definidas.</span><span class="sxs-lookup"><span data-stu-id="8e750-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="8e750-125">`msgid_plural`: A cadeia de caracteres no plural não traduzida.</span><span class="sxs-lookup"><span data-stu-id="8e750-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="8e750-126">`msgstr[0]`: A cadeia de caracteres traduzida para o caso de 0.</span><span class="sxs-lookup"><span data-stu-id="8e750-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="8e750-127">`msgstr[N]`: A cadeia de caracteres traduzida para o n maiusculas</span><span class="sxs-lookup"><span data-stu-id="8e750-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="8e750-128">A especificação de arquivo de ordem de compra pode ser encontrada [aqui](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="8e750-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="8e750-129">Configurando o suporte a arquivo PO no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8e750-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="8e750-130">Este exemplo é baseado em um aplicativo MVC do ASP.NET Core gerado a partir de um modelo de projeto do Visual Studio de 2017.</span><span class="sxs-lookup"><span data-stu-id="8e750-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="8e750-131">Referência do pacote</span><span class="sxs-lookup"><span data-stu-id="8e750-131">Referencing the package</span></span>

<span data-ttu-id="8e750-132">Adicione uma referência para o `OrchardCore.Localization.Core` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="8e750-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="8e750-133">Ele está disponível em [MyGet](https://www.myget.org/) na origem do pacote a seguir: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="8e750-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="8e750-134">O *. csproj* arquivo agora contém uma linha semelhante à seguinte (o número de versão pode variar):</span><span class="sxs-lookup"><span data-stu-id="8e750-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="8e750-135">Registrar o serviço</span><span class="sxs-lookup"><span data-stu-id="8e750-135">Registering the service</span></span>

<span data-ttu-id="8e750-136">Adicionar os serviços necessários para o `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e750-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="8e750-137">Adicionar o middleware necessário para o `Configure` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e750-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="8e750-138">Adicione o seguinte código ao modo de exibição Razor de escolha.</span><span class="sxs-lookup"><span data-stu-id="8e750-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="8e750-139">*About.cshtml* é usado neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="8e750-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="8e750-140">Um `IViewLocalizer` instância é injetada e usada para converter o texto "Olá, mundo!".</span><span class="sxs-lookup"><span data-stu-id="8e750-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="8e750-141">Criando um arquivo de ordem de compra</span><span class="sxs-lookup"><span data-stu-id="8e750-141">Creating a PO file</span></span>

<span data-ttu-id="8e750-142">Crie um arquivo chamado  *<culture code>. po* na pasta raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e750-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="8e750-143">Neste exemplo, o nome do arquivo é *fr.po* porque o idioma francês é usado:</span><span class="sxs-lookup"><span data-stu-id="8e750-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="8e750-144">Esse arquivo armazena a cadeia de caracteres para converter e a cadeia de caracteres convertida em francês.</span><span class="sxs-lookup"><span data-stu-id="8e750-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="8e750-145">Traduções reverter para a cultura pai, se necessário.</span><span class="sxs-lookup"><span data-stu-id="8e750-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="8e750-146">Neste exemplo, o *fr.po* arquivo é usado se a cultura solicitada é `fr-FR` ou `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="8e750-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="8e750-147">Testando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="8e750-147">Testing the application</span></span>

<span data-ttu-id="8e750-148">Execute o aplicativo e navegue até a URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="8e750-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="8e750-149">O texto **Olá, mundo!**</span><span class="sxs-lookup"><span data-stu-id="8e750-149">The text **Hello world!**</span></span> <span data-ttu-id="8e750-150">é exibida.</span><span class="sxs-lookup"><span data-stu-id="8e750-150">is displayed.</span></span>

<span data-ttu-id="8e750-151">Navegue até a URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="8e750-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="8e750-152">O texto **Bon jour le monde!**</span><span class="sxs-lookup"><span data-stu-id="8e750-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="8e750-153">é exibida.</span><span class="sxs-lookup"><span data-stu-id="8e750-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="8e750-154">Pluralização</span><span class="sxs-lookup"><span data-stu-id="8e750-154">Pluralization</span></span>

<span data-ttu-id="8e750-155">Arquivos de PO dão suporte a pluralização formas, que é útil quando a mesma cadeia de caracteres precisa ser traduzido variam de acordo com uma cardinalidade.</span><span class="sxs-lookup"><span data-stu-id="8e750-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="8e750-156">Essa tarefa é feita complicada pelo fato de que cada linguagem define regras personalizadas para selecionar a cadeia de caracteres que para usar com base na cardinalidade.</span><span class="sxs-lookup"><span data-stu-id="8e750-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="8e750-157">O pacote de localização Orchard fornece uma API para invocar esses diferentes formas plurais automaticamente.</span><span class="sxs-lookup"><span data-stu-id="8e750-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="8e750-158">Criando a pluralização arquivos PO</span><span class="sxs-lookup"><span data-stu-id="8e750-158">Creating pluralization PO files</span></span>

<span data-ttu-id="8e750-159">Adicione o seguinte conteúdo para mencionadas anteriormente *fr.po* arquivo:</span><span class="sxs-lookup"><span data-stu-id="8e750-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="8e750-160">Consulte [o que é um arquivo de PO?](#what-is-a-po-file) para obter uma explicação do que representa cada entrada neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="8e750-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="8e750-161">Adicionar um idioma usando formulários pluralização diferentes</span><span class="sxs-lookup"><span data-stu-id="8e750-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="8e750-162">Cadeias de caracteres em inglês e francês foram usadas no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="8e750-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="8e750-163">Inglês e francês tem apenas dois formulários de pluralização e compartilham as mesmas regras de formulário, que é que uma cardinalidade de um é mapeada para a primeira forma plural.</span><span class="sxs-lookup"><span data-stu-id="8e750-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="8e750-164">Qualquer outra cardinalidade é mapeada para a segunda forma plural.</span><span class="sxs-lookup"><span data-stu-id="8e750-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="8e750-165">Nem todos os idiomas compartilham as mesmas regras.</span><span class="sxs-lookup"><span data-stu-id="8e750-165">Not all languages share the same rules.</span></span> <span data-ttu-id="8e750-166">Isso é ilustrado com o idioma tcheco, que tem três formas no plural.</span><span class="sxs-lookup"><span data-stu-id="8e750-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="8e750-167">Criar o `cs.po` arquivos da seguinte maneira e observe como a pluralização precisa três traduções diferentes:</span><span class="sxs-lookup"><span data-stu-id="8e750-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="8e750-168">Para aceitar as localizações Tcheca, adicionar `"cs"` à lista de culturas com suporte a `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="8e750-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

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

<span data-ttu-id="8e750-169">Editar o *Views/Home/About.cshtml* arquivo renderizar localizadas no plural cadeias de caracteres para várias cardinalidades:</span><span class="sxs-lookup"><span data-stu-id="8e750-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="8e750-170">**Observação:** em um cenário do mundo real, uma variável deve ser usada para representar a contagem.</span><span class="sxs-lookup"><span data-stu-id="8e750-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="8e750-171">Aqui, podemos Repita o mesmo código com três valores diferentes para expor um caso muito específico.</span><span class="sxs-lookup"><span data-stu-id="8e750-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="8e750-172">Ao alternar culturas, consulte o seguinte:</span><span class="sxs-lookup"><span data-stu-id="8e750-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="8e750-173">Para `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="8e750-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="8e750-174">Para `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="8e750-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="8e750-175">Para `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="8e750-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="8e750-176">Observe que, para a cultura Tcheca, as três traduções são diferentes.</span><span class="sxs-lookup"><span data-stu-id="8e750-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="8e750-177">Culturas francês e inglês compartilham a mesma construção para as cadeias de caracteres traduzidas últimas duas.</span><span class="sxs-lookup"><span data-stu-id="8e750-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="8e750-178">Tarefas avançadas</span><span class="sxs-lookup"><span data-stu-id="8e750-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="8e750-179">Cadeias de caracteres de contexto</span><span class="sxs-lookup"><span data-stu-id="8e750-179">Contextualizing strings</span></span>

<span data-ttu-id="8e750-180">Os aplicativos geralmente contêm cadeias de caracteres a ser convertido em vários locais.</span><span class="sxs-lookup"><span data-stu-id="8e750-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="8e750-181">A mesma cadeia de caracteres pode ter uma tradução diferente em determinados locais dentro de um aplicativo (exibições Razor ou arquivos de classe).</span><span class="sxs-lookup"><span data-stu-id="8e750-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="8e750-182">Um arquivo de PO oferece suporte a noção de um contexto de arquivo, que pode ser usado para categorizar a cadeia de caracteres que está sendo representada.</span><span class="sxs-lookup"><span data-stu-id="8e750-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="8e750-183">Usando um contexto de arquivo, uma cadeia de caracteres pode ser convertida de forma diferente, dependendo do contexto do arquivo (ou falta de um contexto de arquivo).</span><span class="sxs-lookup"><span data-stu-id="8e750-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="8e750-184">Os serviços de localização de PO usam o nome da classe completa ou o modo de exibição que é usado ao converter uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="8e750-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="8e750-185">Isso é feito definindo o valor sobre o `msgctxt` entrada.</span><span class="sxs-lookup"><span data-stu-id="8e750-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="8e750-186">Considere uma adição secundária para a versão anterior *fr.po* exemplo.</span><span class="sxs-lookup"><span data-stu-id="8e750-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="8e750-187">Um modo de exibição Razor localizado em *Views/Home/About.cshtml* pode ser definida como o contexto do arquivo definindo reservado `msgctxt` valor da entrada:</span><span class="sxs-lookup"><span data-stu-id="8e750-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="8e750-188">Com o `msgctxt` definido como tal, a conversão de texto ocorre ao navegar para `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="8e750-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="8e750-189">A conversão não ocorre ao navegar para `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="8e750-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="8e750-190">Quando nenhuma entrada específica é correspondida com um contexto de arquivo fornecido, o mecanismo de fallback do núcleo Orchard procura um arquivo de PO apropriado sem contexto.</span><span class="sxs-lookup"><span data-stu-id="8e750-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="8e750-191">Supondo que não há não é definido para nenhum contexto de arquivo específico *Views/Home/Contact.cshtml*, navegando para `/Home/Contact?culture=fr-FR` carrega um arquivo de ordem de compra, como:</span><span class="sxs-lookup"><span data-stu-id="8e750-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="8e750-192">Alterar o local dos arquivos de PO</span><span class="sxs-lookup"><span data-stu-id="8e750-192">Changing the location of PO files</span></span>

<span data-ttu-id="8e750-193">O local padrão dos arquivos de ordem de compra pode ser alterado em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8e750-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="8e750-194">Neste exemplo, os arquivos de PO são carregados do *localização* pasta.</span><span class="sxs-lookup"><span data-stu-id="8e750-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="8e750-195">Implementar uma lógica personalizada para localizar arquivos de localização</span><span class="sxs-lookup"><span data-stu-id="8e750-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="8e750-196">Quando uma lógica mais complexa é necessária para localizar arquivos de ordem de compra, o `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface pode ser implementada e registrada como um serviço.</span><span class="sxs-lookup"><span data-stu-id="8e750-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="8e750-197">Isso é útil quando PO arquivos podem ser armazenados em variáveis locais ou quando os arquivos precisam ser encontrada dentro de uma hierarquia de pastas.</span><span class="sxs-lookup"><span data-stu-id="8e750-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="8e750-198">Usando um idioma padrão diferente pluralized</span><span class="sxs-lookup"><span data-stu-id="8e750-198">Using a different default pluralized language</span></span>

<span data-ttu-id="8e750-199">O pacote inclui um `Plural` método de extensão que é específico para duas formas no plural.</span><span class="sxs-lookup"><span data-stu-id="8e750-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="8e750-200">Para idiomas que exigem mais formulários plural, crie um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="8e750-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="8e750-201">Com um método de extensão, você não precisa fornecer qualquer arquivo de localização para o idioma padrão &mdash; as cadeias de caracteres originais já estão disponíveis diretamente no código.</span><span class="sxs-lookup"><span data-stu-id="8e750-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="8e750-202">Você pode usar mais genérica `Plural(int count, string[] pluralForms, params object[] arguments)` sobrecarga que aceita uma matriz de cadeia de caracteres de traduções.</span><span class="sxs-lookup"><span data-stu-id="8e750-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
