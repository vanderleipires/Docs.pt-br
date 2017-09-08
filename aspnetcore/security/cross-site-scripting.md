---
title: "Impedindo a execução de scripts entre sites"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: 1816977837efd82f374a03d9f776db21358e2850
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="a18b2-103">Impedindo a execução de scripts entre sites</span><span class="sxs-lookup"><span data-stu-id="a18b2-103">Preventing Cross-Site Scripting</span></span>

<a name=security-cross-site-scripting></a>

<span data-ttu-id="a18b2-104">Execução de scripts entre sites (XSS) é uma vulnerabilidade de segurança que permite que um invasor inserir os scripts do lado do cliente (geralmente JavaScript) em páginas da web.</span><span class="sxs-lookup"><span data-stu-id="a18b2-104">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="a18b2-105">Quando outros usuários carregar páginas afetadas, os invasores scripts serão executados, permitindo que o invasor roubar cookies e tokens de sessão, alterar o conteúdo da página da web por meio de manipulação de DOM ou redirecionar o navegador para outra página.</span><span class="sxs-lookup"><span data-stu-id="a18b2-105">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="a18b2-106">Vulnerabilidades XSS geralmente ocorrem quando um aplicativo usa a entrada do usuário e passa em uma página sem validação, codificação ou escape-lo.</span><span class="sxs-lookup"><span data-stu-id="a18b2-106">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="a18b2-107">Protegendo seu aplicativo contra XSS</span><span class="sxs-lookup"><span data-stu-id="a18b2-107">Protecting your application against XSS</span></span>

<span data-ttu-id="a18b2-108">AT um XSS de nível básico funciona enganar seu aplicativo para inserir um `<script>` marca na página renderizada, ou inserindo um `On*` evento em um elemento.</span><span class="sxs-lookup"><span data-stu-id="a18b2-108">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="a18b2-109">Os desenvolvedores devem usar as seguintes etapas de prevenção para evitar introduzir XSS em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a18b2-109">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="a18b2-110">Nunca coloque dados não confiáveis em sua entrada HTML, a menos que você siga o restante das etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="a18b2-110">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="a18b2-111">Dados não confiáveis são todos os dados que podem ser controlados por um invasor, entradas de formulário HTML, cadeias de caracteres de consulta, os cabeçalhos HTTP, até mesmo dados originados de um banco de dados como um invasor consiga violar seu banco de dados, mesmo se eles não podem violar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a18b2-111">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="a18b2-112">Antes de colocar os dados não confiáveis dentro de um elemento HTML garanta é codificada em HTML.</span><span class="sxs-lookup"><span data-stu-id="a18b2-112">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="a18b2-113">Codificação HTML usa caracteres como &lt; e alterá-los em um formato seguro como &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="a18b2-113">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="a18b2-114">Antes de colocar os dados não confiáveis em um atributo HTML garanta é atributo HTML codificado.</span><span class="sxs-lookup"><span data-stu-id="a18b2-114">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="a18b2-115">Codificação do atributo HTML é um superconjunto de codificação HTML e codifica caracteres adicionais, como "e '.</span><span class="sxs-lookup"><span data-stu-id="a18b2-115">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="a18b2-116">Antes de colocar os dados não confiáveis em JavaScript coloca os dados em um elemento HTML cujo conteúdo você recuperar em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="a18b2-116">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="a18b2-117">Se isso não é possível assegurar que os dados é codificado com o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a18b2-117">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="a18b2-118">Codificação de JavaScript usa caracteres perigosas para JavaScript e substitui-los com seu hex, por exemplo &lt; poderia ser codificado como `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="a18b2-118">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="a18b2-119">Antes de colocar os dados não confiáveis em uma cadeia de caracteres de consulta de URL, verifique se é codificada de URL.</span><span class="sxs-lookup"><span data-stu-id="a18b2-119">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="a18b2-120">Codificação HTML usando o Razor</span><span class="sxs-lookup"><span data-stu-id="a18b2-120">HTML Encoding using Razor</span></span>

<span data-ttu-id="a18b2-121">O mecanismo Razor usado no MVC automaticamente codifica todos saída originados de variáveis, a menos que você trabalhar arduamente para evitar que isso.</span><span class="sxs-lookup"><span data-stu-id="a18b2-121">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="a18b2-122">Ele usa o atributo HTML regras de codificação, sempre que você usar o  *@*  diretiva.</span><span class="sxs-lookup"><span data-stu-id="a18b2-122">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="a18b2-123">Como HTML a codificação do atributo é um superconjunto de codificação de HTML, que isso significa que você não precisa se preocupar com você deve usar a codificação HTML ou codificação do atributo HTML.</span><span class="sxs-lookup"><span data-stu-id="a18b2-123">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="a18b2-124">Certifique-se de que você só usar em um contexto HTML, não ao tentar inserir não confiáveis de entrada diretamente em JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a18b2-124">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="a18b2-125">Auxiliares de marcação codificará também usam parâmetros de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="a18b2-125">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="a18b2-126">Executar a seguinte exibição Razor;</span><span class="sxs-lookup"><span data-stu-id="a18b2-126">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="a18b2-127">Essa exibição mostra o conteúdo do *untrustedInput* variável.</span><span class="sxs-lookup"><span data-stu-id="a18b2-127">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="a18b2-128">Essa variável inclui alguns caracteres que são usadas em ataques XSS, ou seja, &lt;, "e &gt;.</span><span class="sxs-lookup"><span data-stu-id="a18b2-128">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="a18b2-129">Examinando a fonte mostra a saída renderizada codificada como:</span><span class="sxs-lookup"><span data-stu-id="a18b2-129">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="a18b2-130">Núcleo ASP.NET MVC fornece uma `HtmlString` classe que não é codificado automaticamente após a saída.</span><span class="sxs-lookup"><span data-stu-id="a18b2-130">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="a18b2-131">Isso nunca deve ser usado em combinação com entradas não confiáveis, pois isso irá expor uma vulnerabilidade XSS.</span><span class="sxs-lookup"><span data-stu-id="a18b2-131">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="a18b2-132">Usando o Razor de codificação JavaScript</span><span class="sxs-lookup"><span data-stu-id="a18b2-132">Javascript Encoding using Razor</span></span>

<span data-ttu-id="a18b2-133">Pode haver momentos que você deseja inserir um valor em JavaScript para processar no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a18b2-133">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="a18b2-134">Há duas formas de fazer isso.</span><span class="sxs-lookup"><span data-stu-id="a18b2-134">There are two ways to do this.</span></span> <span data-ttu-id="a18b2-135">A maneira mais segura para inserir valores simples é colocar o valor em um atributo de dados de uma marca e recuperá-lo em seu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a18b2-135">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="a18b2-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a18b2-136">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="a18b2-137">Isso produzirá o HTML a seguir</span><span class="sxs-lookup"><span data-stu-id="a18b2-137">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="a18b2-138">Que, quando ele é executado, processará a seguir:</span><span class="sxs-lookup"><span data-stu-id="a18b2-138">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="a18b2-139">Você também pode chamar o codificador de JavaScript diretamente.</span><span class="sxs-lookup"><span data-stu-id="a18b2-139">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="a18b2-140">Isso será renderizado no navegador:</span><span class="sxs-lookup"><span data-stu-id="a18b2-140">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="a18b2-141">Não concatene a entrada não confiável em JavaScript para criar elementos de DOM.</span><span class="sxs-lookup"><span data-stu-id="a18b2-141">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="a18b2-142">Você deve usar `createElement()` e atribuir valores de propriedade adequadamente como `node.TextContent=`, ou use `element.SetAttribute()` / `element[attribute]=` contrário você expor ao XSS baseado em DOM.</span><span class="sxs-lookup"><span data-stu-id="a18b2-142">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="a18b2-143">Acessando codificadores no código</span><span class="sxs-lookup"><span data-stu-id="a18b2-143">Accessing encoders in code</span></span>

<span data-ttu-id="a18b2-144">Os codificadores HTML, JavaScript e URL estão disponíveis para o código de duas maneiras, você pode colocá-los por meio de [injeção de dependência](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) ou você pode usar os codificadores padrão contidos a `System.Text.Encodings.Web` namespace.</span><span class="sxs-lookup"><span data-stu-id="a18b2-144">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="a18b2-145">Se você usar os codificadores padrão e qualquer aplicada a intervalos de caractere para serem tratados como seguro não entrarão em vigor - os codificadores padrão usem as regras de codificação mais seguras possível.</span><span class="sxs-lookup"><span data-stu-id="a18b2-145">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="a18b2-146">Para usar os codificadores configuráveis por meio de DI seus construtores devem levar uma *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* parâmetro conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="a18b2-146">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="a18b2-147">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="a18b2-147">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="a18b2-148">Parâmetros de URL de codificação</span><span class="sxs-lookup"><span data-stu-id="a18b2-148">Encoding URL Parameters</span></span>

<span data-ttu-id="a18b2-149">Se você deseja criar uma cadeia de caracteres de consulta de URL com entradas não confiáveis, como um valor, use o `UrlEncoder` para codificar o valor.</span><span class="sxs-lookup"><span data-stu-id="a18b2-149">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="a18b2-150">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="a18b2-150">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="a18b2-151">Após a codificação de encodedValue variável conterá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="a18b2-151">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="a18b2-152">Espaços, aspas, pontuação e outros caracteres não seguros será porcentagem codificado para seu valor hexadecimal, por exemplo, um caractere de espaço será % 20.</span><span class="sxs-lookup"><span data-stu-id="a18b2-152">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="a18b2-153">Não use a entrada não confiável como parte de um caminho de URL.</span><span class="sxs-lookup"><span data-stu-id="a18b2-153">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="a18b2-154">Sempre passe a entrada não confiável como um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="a18b2-154">Always pass untrusted input as a query string value.</span></span>

<a name=security-cross-site-scripting-customization></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="a18b2-155">Personalizando os codificadores</span><span class="sxs-lookup"><span data-stu-id="a18b2-155">Customizing the Encoders</span></span>

<span data-ttu-id="a18b2-156">Por padrão, codificadores usam uma lista de segurança limitada para o intervalo Unicode Latim básico e codificar todos os caracteres fora do intervalo como seus equivalentes do código de caractere.</span><span class="sxs-lookup"><span data-stu-id="a18b2-156">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="a18b2-157">Esse comportamento também afeta o processamento Razor TagHelper e HtmlHelper como ele usará os codificadores para suas cadeias de caracteres de saída.</span><span class="sxs-lookup"><span data-stu-id="a18b2-157">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="a18b2-158">O raciocínio por trás disso é proteger contra erros de navegador desconhecido ou futuro (bugs de navegador anterior tiveram ultrapassado a análise com base no processamento de caracteres estendidos).</span><span class="sxs-lookup"><span data-stu-id="a18b2-158">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="a18b2-159">Se seu site faz uso intenso de caracteres não latinos, como o chinês, cirílico ou outras pessoas isso provavelmente não é o comportamento desejado.</span><span class="sxs-lookup"><span data-stu-id="a18b2-159">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="a18b2-160">Você pode personalizar as listas de codificador seguro para incluir Unicode intervalos adequados ao seu aplicativo durante a inicialização, em `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="a18b2-160">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="a18b2-161">Por exemplo, usando a configuração padrão que você pode usar um HtmlHelper Razor assim;</span><span class="sxs-lookup"><span data-stu-id="a18b2-161">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="a18b2-162">Quando você exibir a origem da página da web, você verá que tenha sido processado como a seguir, com o texto chinês codificado;</span><span class="sxs-lookup"><span data-stu-id="a18b2-162">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="a18b2-163">Para ampliar os caracteres tratados como seguro pelo codificador você inseriria a linha a seguir para o `ConfigureServices()` método `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="a18b2-163">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="a18b2-164">Este exemplo amplia a lista segura para incluir o CjkUnifiedIdeographs de intervalo de Unicode.</span><span class="sxs-lookup"><span data-stu-id="a18b2-164">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="a18b2-165">Agora se torna a saída renderizada</span><span class="sxs-lookup"><span data-stu-id="a18b2-165">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="a18b2-166">Listar seguro intervalos são especificados como gráficos de código Unicode, não os idiomas.</span><span class="sxs-lookup"><span data-stu-id="a18b2-166">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="a18b2-167">O [padrão Unicode](http://unicode.org/) tem uma lista de [código gráficos](http://www.unicode.org/charts/index.html) você pode usar para localizar o gráfico que contém os caracteres.</span><span class="sxs-lookup"><span data-stu-id="a18b2-167">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="a18b2-168">Cada codificador, Html, JavaScript e a Url, deve ser configurado separadamente.</span><span class="sxs-lookup"><span data-stu-id="a18b2-168">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="a18b2-169">Personalização da lista de afeta somente os codificadores originados por meio de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="a18b2-169">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="a18b2-170">Se você acessar diretamente um codificador via `System.Text.Encodings.Web.*Encoder.Default` , em seguida, o padrão, Latim básico somente lista segura será usada.</span><span class="sxs-lookup"><span data-stu-id="a18b2-170">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="a18b2-171">Qual codificação take deve colocar?</span><span class="sxs-lookup"><span data-stu-id="a18b2-171">Where should encoding take place?</span></span>

<span data-ttu-id="a18b2-172">Gerais aceito prática é que a codificação ocorre no ponto de saída e valores codificados nunca devem ser armazenados em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18b2-172">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="a18b2-173">Codificação no ponto de saída permite que você altere o uso de dados, por exemplo, de HTML para um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="a18b2-173">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="a18b2-174">Ele também permite que você pesquise facilmente os dados sem a necessidade de codificar valores antes de pesquisar e permite que você se beneficie de quaisquer alterações ou correções feitas codificadores.</span><span class="sxs-lookup"><span data-stu-id="a18b2-174">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="a18b2-175">Validação como uma técnica de prevenção de XSS</span><span class="sxs-lookup"><span data-stu-id="a18b2-175">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="a18b2-176">A validação pode ser uma ferramenta útil limitar ataques XSS.</span><span class="sxs-lookup"><span data-stu-id="a18b2-176">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="a18b2-177">Por exemplo, uma cadeia de caracteres numérica simple que contém somente os caracteres 0-9 não vai disparar um ataque XSS.</span><span class="sxs-lookup"><span data-stu-id="a18b2-177">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="a18b2-178">A validação se torna mais complicada que você deseja aceitar HTML na entrada do usuário - análise de entrada HTML é difícil, se não impossível.</span><span class="sxs-lookup"><span data-stu-id="a18b2-178">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="a18b2-179">Redução e outros formatos de texto seria uma opção mais segura para a entrada avançada.</span><span class="sxs-lookup"><span data-stu-id="a18b2-179">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="a18b2-180">Você nunca deve depender somente de validação.</span><span class="sxs-lookup"><span data-stu-id="a18b2-180">You should never rely on validation alone.</span></span> <span data-ttu-id="a18b2-181">Sempre codifica a entrada não confiável antes de saída, não importa o que você executou a validação.</span><span class="sxs-lookup"><span data-stu-id="a18b2-181">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
