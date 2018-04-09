---
title: Impedir que os sites script (XSS) no núcleo do ASP.NET
author: rick-anderson
description: Saiba mais sobre a criação de scripts entre sites (XSS) e técnicas para lidar com essa vulnerabilidade em um aplicativo do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cross-site-scripting
ms.openlocfilehash: d9263a2c1bb6a376008b7d8a55864e4d15e77cee
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Impedir que os sites script (XSS) no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Execução de scripts entre sites (XSS) é uma vulnerabilidade de segurança que permite que um invasor inserir os scripts do lado do cliente (geralmente JavaScript) em páginas da web. Quando outros usuários carregar páginas afetadas, os invasores scripts serão executados, permitindo que o invasor roubar cookies e tokens de sessão, alterar o conteúdo da página da web por meio de manipulação de DOM ou redirecionar o navegador para outra página. Vulnerabilidades XSS geralmente ocorrem quando um aplicativo usa a entrada do usuário e passa em uma página sem validação, codificação ou escape-lo.

## <a name="protecting-your-application-against-xss"></a>Protegendo seu aplicativo contra XSS

AT um XSS de nível básico funciona enganar seu aplicativo para inserir um `<script>` marca na página renderizada, ou inserindo um `On*` evento em um elemento. Os desenvolvedores devem usar as seguintes etapas de prevenção para evitar introduzir XSS em seu aplicativo.

1. Nunca coloque dados não confiáveis em sua entrada HTML, a menos que você siga o restante das etapas a seguir. Dados não confiáveis são todos os dados que podem ser controlados por um invasor, entradas de formulário HTML, cadeias de caracteres de consulta, os cabeçalhos HTTP, até mesmo dados originados de um banco de dados como um invasor consiga violar seu banco de dados, mesmo se eles não podem violar seu aplicativo.

2. Antes de colocar os dados não confiáveis dentro de um elemento HTML garanta é codificada em HTML. Codificação HTML usa caracteres como &lt; e alterá-los em um formato seguro como &amp;lt;

3. Antes de colocar os dados não confiáveis em um atributo HTML garanta é atributo HTML codificado. Codificação do atributo HTML é um superconjunto de codificação HTML e codifica caracteres adicionais, como "e '.

4. Antes de colocar os dados não confiáveis em JavaScript coloca os dados em um elemento HTML cujo conteúdo você recuperar em tempo de execução. Se isso não é possível assegurar que os dados é codificado com o JavaScript. Codificação de JavaScript usa caracteres perigosas para JavaScript e substitui-los com seu hex, por exemplo &lt; poderia ser codificado como `\u003C`.

5. Antes de colocar os dados não confiáveis em uma cadeia de caracteres de consulta de URL, verifique se é codificada de URL.

## <a name="html-encoding-using-razor"></a>Codificação HTML usando o Razor

O mecanismo Razor usado no MVC automaticamente codifica todos saída originados de variáveis, a menos que você trabalhar arduamente para evitar que isso. Ele usa o atributo HTML regras de codificação, sempre que você usar o *@* diretiva. Como HTML a codificação do atributo é um superconjunto de codificação de HTML, que isso significa que você não precisa se preocupar com você deve usar a codificação HTML ou codificação do atributo HTML. Certifique-se de que você só usar em um contexto HTML, não ao tentar inserir não confiáveis de entrada diretamente em JavaScript. Auxiliares de marcação codificará também usam parâmetros de marca de entrada.

Executar a seguinte exibição Razor;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Essa exibição mostra o conteúdo do *untrustedInput* variável. Essa variável inclui alguns caracteres que são usadas em ataques XSS, ou seja, &lt;, "e &gt;. Examinando a fonte mostra a saída renderizada codificada como:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> Núcleo ASP.NET MVC fornece uma `HtmlString` classe que não é codificado automaticamente após a saída. Isso nunca deve ser usado em combinação com entradas não confiáveis, pois isso irá expor uma vulnerabilidade XSS.

## <a name="javascript-encoding-using-razor"></a>Usando o Razor de codificação JavaScript

Pode haver momentos que você deseja inserir um valor em JavaScript para processar no modo de exibição. Há duas formas de fazer isso. A maneira mais segura para inserir valores simples é colocar o valor em um atributo de dados de uma marca e recuperá-lo em seu JavaScript. Por exemplo:

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

Isso produzirá o HTML a seguir

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

Que, quando ele é executado, processará a seguir:

```none
<"123">
   <"123">
   ```

Você também pode chamar o codificador de JavaScript diretamente.

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

Isso será renderizado no navegador:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Não concatene a entrada não confiável em JavaScript para criar elementos de DOM. Você deve usar `createElement()` e atribuir valores de propriedade adequadamente como `node.TextContent=`, ou use `element.SetAttribute()` / `element[attribute]=` contrário você expor ao XSS baseado em DOM.

## <a name="accessing-encoders-in-code"></a>Acessando codificadores no código

Os codificadores HTML, JavaScript e URL estão disponíveis para o código de duas maneiras, você pode colocá-los por meio de [injeção de dependência](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) ou você pode usar os codificadores padrão contidos a `System.Text.Encodings.Web` namespace. Se você usar os codificadores padrão e qualquer aplicada a intervalos de caracteres a serem tratados como seguro não terão efeito - os codificadores padrão usem as regras de codificação mais seguras possível.

Para usar os codificadores configuráveis por meio de DI seus construtores devem levar uma *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* parâmetro conforme apropriado. Por exemplo,

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

## <a name="encoding-url-parameters"></a>Parâmetros de URL de codificação

Se você deseja criar uma cadeia de caracteres de consulta de URL com entradas não confiáveis, como um valor, use o `UrlEncoder` para codificar o valor. Por exemplo,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Após a codificação de encodedValue variável conterá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Espaços, aspas, pontuação e outros caracteres não seguros será porcentagem codificado para seu valor hexadecimal, por exemplo, um caractere de espaço será % 20.

>[!WARNING]
> Não use a entrada não confiável como parte de um caminho de URL. Sempre passe a entrada não confiável como um valor de cadeia de caracteres de consulta.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personalizando os codificadores

Por padrão, codificadores usam uma lista de segurança limitada para o intervalo Unicode Latim básico e codificar todos os caracteres fora do intervalo como seus equivalentes do código de caractere. Esse comportamento também afeta o processamento Razor TagHelper e HtmlHelper como ele usará os codificadores para suas cadeias de caracteres de saída.

O raciocínio por trás disso é proteger contra erros de navegador desconhecido ou futuro (bugs de navegador anterior tiveram ultrapassado a análise com base no processamento de caracteres estendidos). Se seu site faz uso intenso de caracteres não latinos, como o chinês, cirílico ou outras pessoas isso provavelmente não é o comportamento desejado.

Você pode personalizar as listas de codificador seguro para incluir Unicode intervalos adequados ao seu aplicativo durante a inicialização, em `ConfigureServices()`.

Por exemplo, usando a configuração padrão que você pode usar um HtmlHelper Razor assim;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Quando você exibir a origem da página da web, você verá que tenha sido processado como a seguir, com o texto chinês codificado;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Para ampliar os caracteres tratados como seguro pelo codificador você inseriria a linha a seguir para o `ConfigureServices()` método `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

Este exemplo amplia a lista segura para incluir o CjkUnifiedIdeographs de intervalo de Unicode. Agora se torna a saída renderizada

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Listar seguro intervalos são especificados como gráficos de código Unicode, não os idiomas. O [padrão Unicode](http://unicode.org/) tem uma lista de [código gráficos](http://www.unicode.org/charts/index.html) você pode usar para localizar o gráfico que contém os caracteres. Cada codificador, Html, JavaScript e a Url, deve ser configurado separadamente.

> [!NOTE]
> Personalização da lista de afeta somente os codificadores originados por meio de injeção de dependência. Se você acessar diretamente um codificador via `System.Text.Encodings.Web.*Encoder.Default` , em seguida, o padrão, Latim básico somente lista segura será usada.

## <a name="where-should-encoding-take-place"></a>Qual codificação take deve colocar?

Gerais aceito prática é que a codificação ocorre no ponto de saída e valores codificados nunca devem ser armazenados em um banco de dados. Codificação no ponto de saída permite que você altere o uso de dados, por exemplo, de HTML para um valor de cadeia de caracteres de consulta. Ele também permite que você pesquise facilmente os dados sem a necessidade de codificar valores antes de pesquisar e permite que você se beneficie de quaisquer alterações ou correções feitas codificadores.

## <a name="validation-as-an-xss-prevention-technique"></a>Validação como uma técnica de prevenção de XSS

A validação pode ser uma ferramenta útil limitar ataques XSS. Por exemplo, uma cadeia de caracteres numérica simple que contém somente os caracteres 0-9 não aciona um ataque XSS. A validação se torna mais complicada que você deseja aceitar HTML na entrada do usuário - análise de entrada HTML é difícil, se não impossível. Redução e outros formatos de texto seria uma opção mais segura para a entrada avançada. Você nunca deve depender somente de validação. Sempre codifica a entrada não confiável antes de saída, não importa o que você executou a validação.
