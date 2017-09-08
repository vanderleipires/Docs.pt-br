---
title: "Auxiliares de marcação em formulários do ASP.NET Core"
author: rick-anderson
description: "Descreve interno de auxiliares de marcação usados com formulários."
keywords: "Formulários do ASP.NET Core, o auxiliar de marca, TagHelper, do formulário HTML,"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd69e008a81abc4f6785d93b89823c03e1a7df83
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Introdução ao uso de auxiliares de marcação em formulários do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), e [Jerrie Pelser](https://github.com/jerriep)

Este documento demonstra trabalhar com formulários e os elementos HTML comumente usados em um formulário. O HTML [formulário](https://www.w3.org/TR/html401/interact/forms.html) elemento fornece o uso de aplicativos web mecanismo principal para enviar dados para o servidor. A maioria deste documento descreve [auxiliares de marcação](tag-helpers/intro.md) e como eles podem ajudar você a criar produtiva robustos formulários HTML. É recomendável que você leia [Introdução ao auxiliares de marcação](tag-helpers/intro.md) antes de ler este documento.

Em muitos casos, auxiliares HTML fornecem uma abordagem alternativa para um auxiliar de marca específica, mas é importante reconhecer que auxiliares de marcação não substituem auxiliares HTML e não é um auxiliar de marca para cada auxiliar HTML. Quando existe uma alternativa de auxiliar HTML, ele é mencionado.

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a>O auxiliar de marca de formulário

O [formulário](https://www.w3.org/TR/html401/interact/forms.html) auxiliar de marca:

* Gera o HTML [ \<formulário >](https://www.w3.org/TR/html401/interact/forms.html) `action` valor de atributo para uma ação do controlador MVC ou uma rota nomeada

* Gera um oculta [solicitação de Token de verificação](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para evitar a falsificação de solicitação entre sites (quando usado com o `[ValidateAntiForgeryToken]` atributo no método de ação HTTP Post)

* Fornece o `asp-route-<Parameter Name>` atributo, onde `<Parameter Name>` é adicionado aos valores de rota. O `routeValues` parâmetros para `Html.BeginForm` e `Html.BeginRouteForm` fornecem funcionalidade semelhante.

* Tem uma alternativa de auxiliar HTML `Html.BeginForm` e`Html.BeginRouteForm`

Amostra:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

O auxiliar de marca de formulário acima gera o HTML a seguir:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

O tempo de execução do MVC gera o `action` valor do atributo dos atributos do auxiliar de marca de formulário `asp-controller` e `asp-action`. O auxiliar de marca de formulário também gera oculto [solicitação de Token de verificação](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para evitar a falsificação de solicitação entre sites (quando usado com o `[ValidateAntiForgeryToken]` atributo no método de ação HTTP Post). Proteger um formulário HTML puro contra falsificação de solicitação entre sites é difícil, o auxiliar de marca de formulário fornece este serviço para você.

### <a name="using-a-named-route"></a>Usando uma rota nomeada

O `asp-route` atributo do auxiliar de marca também pode gerar a marcação para o HTML `action` atributo. Um aplicativo com um [rota](../../fundamentals/routing.md) chamado `register` poderia usar a seguinte marcação para a página de registro:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Muitas das exibições no *modos de exibição/conta* pasta (gerado quando você cria um novo aplicativo web com *contas de usuário individuais*) contêm o [asp de rota de returnurl](http://docs.asp.net/en/latest/mvc/views/working-with-forms.html#the-form-tag-helper) atributo:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [2]}} -->

```none
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
   ```

>[!NOTE]
>Com os modelos internos, `returnUrl` só é preenchida automaticamente quando você tentar acessar um recurso autorizado, mas não é autenticado ou autorizado. Quando você tenta um acesso não autorizado, o middleware de segurança redireciona para a página de logon com o `returnUrl` definido.

## <a name="the-input-tag-helper"></a>O auxiliar de marca de entrada

O auxiliar de marca de entrada associa um HTML [ \<entrada >](https://www.w3.org/wiki/HTML/Elements/input) elemento para uma expressão de modelo no modo de exibição razor.

Sintaxe:

```HTML
<input asp-for="<Expression Name>" />
   ```

O auxiliar de marca de entrada:

* Gera o `id` e `name` atributos HTML para o nome da expressão especificada no `asp-for` atributo. `asp-for="Property1.Property2"` equivale a `m => m.Property1.Property2`. O nome da expressão é o que é usado para o `asp-for` valor do atributo. Consulte o [nomes de expressão](#expression-names) seção para obter informações adicionais.

* Define o HTML `type` com base no tipo de modelo de valor de atributo e [anotação de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) atributos aplicados para a propriedade de modelo

* Não substituirá o HTML `type` valor de atributo quando um for especificado

* Gera [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributos de validação de [anotação de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) atributos aplicados às propriedades de modelo

* Tem um recurso auxiliar HTML sobrepõe `Html.TextBoxFor` e `Html.EditorFor`. Consulte o **alternativas de auxiliar HTML para auxiliar de marca de entrada** seção para obter detalhes.

* Fornece alta segurança de tipos. Se o nome das alterações de propriedade e você não atualizar o auxiliar de marca você obterá um erro semelhante à seguinte:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

O `Input` auxiliar de marca define o HTML `type` atributo com base no tipo de .NET. A tabela a seguir lista alguns tipos comuns de .NET e o tipo HTML gerado (não todos os tipos de .NET é listado).

|Tipo .NET|Tipo de entrada|
|---|---|
|bool|tipo = "caixa de seleção"|
|Cadeia de caracteres|tipo = "text"|
|DateTime|tipo = "datetime"|
|Byte|tipo = "number"|
|int|tipo = "number"|
|Single e Double|tipo = "number"|


A tabela a seguir mostra algumas [as anotações de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) atributos que o auxiliar de marca de entrada serão mapeados para tipos específicos de entrada (não todos os atributos de validação é listado):


|Atributo|Tipo de entrada|
|---|---|
|[EmailAddress]|tipo = "email"|
|[Url]|tipo = "url"|
|[HiddenInput]|tipo = "oculto"|
|[Phone]|tipo = "telefone"|
|[DataType(DataType.Password)]| tipo = "senha"|
|[DataType(DataType.Date)]| tipo = "Data"|
|[DataType(DataType.Time)]| tipo = "Hora"|


Amostra:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

O código anterior gera o HTML a seguir:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

As anotações de dados aplicadas para o `Email` e `Password` propriedades geram metadados no modelo. O auxiliar de marca de entrada consome os metadados do modelo e produz [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributos (consulte [validação de modelo](../models/validation.md)). Esses atributos descrevem os validadores para anexar a campos de entrada. Isso fornece discreto HTML5 e [jQuery](https://jquery.com/) validação. Os atributos discretas tem o formato `data-val-rule="Error Message"`, em que a regra é o nome da regra de validação (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) Se uma mensagem de erro é fornecida no atributo, ele será exibido como o valor para o `data-val-rule` atributo. Também há atributos do formulário `data-val-ruleName-argumentName="argumentValue"` que fornecem detalhes adicionais sobre a regra, por exemplo, `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Alternativas de auxiliar HTML para auxiliar de marca de entrada

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` e `Html.EditorFor` ter sobreposição de recursos com o auxiliar de marca de entrada. O auxiliar de marca de entrada definirá automaticamente a `type` atributo; `Html.TextBox` e `Html.TextBoxFor` não. `Html.Editor`e `Html.EditorFor` manipular coleções, objetos complexos e modelos; não o auxiliar de marca de entrada. O auxiliar de marca de entrada, `Html.EditorFor` e `Html.TextBoxFor` são fortemente tipadas (eles usar expressões lambda); `Html.TextBox` e `Html.Editor` não são (usarem nomes de expressão).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`e `@Html.EditorFor()` usar especial `ViewDataDictionary` entrada denominada `htmlAttributes` durante a execução de seus modelos padrão. Esse comportamento é aumentado opcionalmente usando `additionalViewData` parâmetros. A chave "htmlAttributes" diferencia maiusculas de minúsculas. A chave "htmlAttributes" é tratada da mesma forma que o `htmlAttributes` objeto passado para entrada auxiliares como `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Nomes de expressão

O `asp-for` valor de atributo é um `ModelExpression` e o lado direito de uma expressão lambda. Portanto, `asp-for="Property1"` se torna `m => m.Property1` no código gerado que é por isso você não precisa com prefixo `Model`. Você pode usar o "@" caractere para iniciar uma expressão embutido e mover antes do `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Gera o seguinte:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Com as propriedades de coleção, `asp-for="CollectionProperty[23].Member"` gera o mesmo nome como `asp-for="CollectionProperty[i].Member"` quando `i` tem o valor `23`.

### <a name="navigating-child-properties"></a>Navegando propriedades filho

Você também pode navegar para propriedades filho usando o caminho da propriedade do modelo de exibição. Considere a possibilidade de uma classe de modelo mais complexa que contém um filho `Address` propriedade.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

No modo de exibição, podemos associar a `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

O HTML a seguir é gerado para `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
   ```

### <a name="expression-names-and-collections"></a>Nomes de expressão e coleções

Exemplo de um modelo que contém uma matriz de `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

O método de ação:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
   ```

O Razor a seguir mostra como você acessa um determinado `Color` elemento:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

O *Views/Shared/EditorTemplates/String.cshtml* modelo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Exemplo usando `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

O Razor a seguir mostra como iterar em uma coleção:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

O *Views/Shared/EditorTemplates/ToDoItem.cshtml* modelo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Sempre use `for` (e *não* `foreach`) para iterar sobre uma lista. Expressão avaliar um indexador em um LINQ pode ser caro e deve ser minimizada.

&nbsp;

>[!NOTE]
>O código de exemplo comentado acima mostra como você poderia substituir a expressão lambda com o `@` operador para acessar cada `ToDoItem` na lista.

## <a name="the-textarea-tag-helper"></a>O auxiliar de marca de área de texto

O `Textarea Tag Helper` auxiliar de marca é semelhante para o auxiliar de marca de entrada.

* Gera o `id` e `name` atributos e os atributos de validação de dados do modelo para um [ \<textarea >](http://www.w3.org/wiki/HTML/Elements/textarea) elemento.

* Fornece alta segurança de tipos.

* Alternativa de auxiliar HTML:`Html.TextAreaFor`

Amostra:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

O HTML a seguir é gerado:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6, 7, 8]}} -->

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>O auxiliar de marca de rótulo

* Gera a legenda do rótulo e `for` de atributo em uma [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elemento para um nome de expressão

* Alternativa de auxiliar HTML: `Html.LabelFor`.

O `Label Tag Helper` fornece os seguintes benefícios sobre um elemento de rótulo HTML puro:

* Você obtém automaticamente o valor de rótulo descritivo do `Display` atributo. O nome de exibição desejado pode mudar com o tempo e a combinação de `Display` auxiliar de marca de rótulo e de atributo se aplicará a `Display` em qualquer lugar, ele é usado.

* Menos marcação no código-fonte

* Forte digitando com a propriedade de modelo.

Amostra:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

O HTML a seguir é gerado para o `<label>` elemento:

```HTML
<label for="Email">Email Address</label>
   ```

O auxiliar de marca de rótulo gerado o `for` valor do atributo de "Email", que é a ID associada a `<input>` elemento. Os auxiliares de marca gerar consistente `id` e `for` elementos para que eles possam ser corretamente associados. A legenda neste exemplo é proveniente do `Display` atributo. Se o modelo não contém um `Display` atributo, a legenda deve ser o nome da propriedade da expressão.

## <a name="the-validation-tag-helpers"></a>Os auxiliares de marca de validação

Há dois auxiliares de marcação de validação. O `Validation Message Tag Helper` (que exibe uma mensagem de validação para uma única propriedade em seu modelo) e o `Validation Summary Tag Helper` (que exibe um resumo de erros de validação). O `Input Tag Helper` adiciona atributos de validação do HTML5 cliente lado para elementos com base em dados de atributos de anotação em suas classes de modelo de entrada. Além disso, a validação é executada no servidor. O auxiliar de marca de validação exibe essas mensagens de erro quando ocorre um erro de validação.

### <a name="the-validation-message-tag-helper"></a>O auxiliar de marca de mensagem de validação

* Adiciona o [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` de atributo para o [abrangem](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, que anexa as mensagens de erro de validação no campo de entrada da propriedade do modelo especificado.   Quando ocorre um erro de validação do lado cliente, [jQuery](https://jquery.com/) exibe a mensagem de erro no `<span>` elemento.

* Também, a validação é feita no servidor. Os clientes poderão ter JavaScript desabilitado e só pode ser feita alguma validação no lado do servidor.

* Alternativa de auxiliar HTML:`Html.ValidationMessageFor`

O `Validation Message Tag Helper` é usado com o `asp-validation-for` atributo em um HTML [abrangem](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.

```HTML
<span asp-validation-for="Email"></span>
   ```

O auxiliar de marca de mensagem de validação irá gerar o HTML a seguir:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Você geralmente usa o `Validation Message Tag Helper` após um `Input` auxiliar de marca para a mesma propriedade. Isso exibe as mensagens de erro de validação próximo a entrada que causou o erro.

> [!NOTE]
> Você deve ter uma exibição com o JavaScript correto e [jQuery](https://jquery.com/) script referências em vigor para a validação do lado do cliente. Consulte [validação de modelo](../models/validation.md) para obter mais informações.

Quando ocorre um erro de validação de lado de servidor (por exemplo quando você tem a validação do lado do servidor personalizado ou validação do lado do cliente é desabilitada), o MVC coloca essa mensagem de erro como o corpo do `<span>` elemento.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>O auxiliar de marca de resumo de validação

* Destinos `<div>` elementos com o `asp-validation-summary` atributo

* Alternativa de auxiliar HTML:`@Html.ValidationSummary`

O `Validation Summary Tag Helper` é usado para exibir um resumo das mensagens de validação. O `asp-validation-summary` valor de atributo pode ser qualquer um dos seguintes:

|Resumo de validação do ASP|Mensagens de validação|
|--- |--- |
|ValidationSummary.All|Nível de propriedade e o modelo|
|ValidationSummary.ModelOnly|Modelo|
|ValidationSummary.None|Nenhum|

### <a name="sample"></a>Amostra

No exemplo a seguir, o modelo de dados é decorado com `DataAnnotation` atributos, que gera mensagens de erro de validação no `<input>` elemento.  Quando ocorre um erro de validação, o auxiliar de marca de validação exibe a mensagem de erro:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

O código HTML gerado (quando o modelo é válido):

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 8, 9, 12, 13]}} -->

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>O auxiliar de marca de seleção

* Gera [selecione](https://www.w3.org/wiki/HTML/Elements/select) e respectivos [opção](https://www.w3.org/wiki/HTML/Elements/option) elementos para as propriedades do modelo.

* Tem uma alternativa de auxiliar HTML `Html.DropDownListFor` e`Html.ListBoxFor`

O `Select Tag Helper` `asp-for` Especifica o nome da propriedade de modelo para o [selecione](https://www.w3.org/wiki/HTML/Elements/select) elemento e `asp-items` Especifica o [opção](https://www.w3.org/wiki/HTML/Elements/option) elementos.  Por exemplo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Amostra:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

O `Index` método inicializa o `CountryViewModel`, define o país/região selecionado e o transmite para o `Index` exibição.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

O HTTP POST `Index` método exibe a seleção:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

O `Index` exibição:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Que gera o seguinte HTML (com "CA" selecionada):

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6]}} -->

```HTML
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

> [!NOTE]
> Não é recomendável usar `ViewBag` ou `ViewData` com o auxiliar de marca selecionar. Um modelo de exibição é mais robusto fornecer metadados MVC e geralmente menos problemáticos.

O `asp-for` valor de atributo é um caso especial e não requer um `Model` prefixo, os outros não de atributos do auxiliar de marcação (como `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Associação de enum

Geralmente é conveniente usar `<select>` com um `enum` propriedade e gerar o `SelectListItem` elementos do `enum` valores.

Amostra:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

O `GetEnumSelectList` método gera uma `SelectList` objeto para um enum.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Possível decorar o enumerador de lista com o `Display` atributo para obter uma interface de usuário mais rica:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

O HTML a seguir é gerado:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [4, 5]}} -->

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

### <a name="option-group"></a>Opção de grupo

O HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento é gerado quando o modelo de exibição contém um ou mais `SelectListGroup` objetos.

O `CountryViewModelGroup` grupos a `SelectListItem` elementos nos grupos "América do Norte" e "Europa":

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Os dois grupos são mostrados abaixo:

![exemplo de grupo de opção](working-with-forms/_static/grp.png)

O código HTML gerado:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]}} -->

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>Seleção múltipla

O auxiliar de marca selecione gerará automaticamente o [vários = "várias"](http://w3c.github.io/html-reference/select.html) atributo se a propriedade especificada a `asp-for` atributo é um `IEnumerable`. Por exemplo, considerando o seguinte modelo:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Com a seguinte exibição:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Gera o HTML a seguir:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3]}} -->

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>Nenhuma seleção

Se você estiver usando a opção "não especificada" em várias páginas, você pode criar um modelo para eliminar o HTML de repetição:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

O *Views/Shared/EditorTemplates/CountryViewModel.cshtml* modelo:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Adicionando HTML [ \<opção >](https://www.w3.org/wiki/HTML/Elements/option) elementos não está limitado ao *nenhuma seleção* caso. Por exemplo, o seguinte método de exibição e a ação irá gerar HTML com o código acima:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Corretas `<option>` elemento será selecionado (contêm o `selected="selected"` atributo) dependendo atual `Country` valor.

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [5]}} -->

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>Recursos adicionais

* [Auxiliares de marcação](tag-helpers/intro.md)

* [Elemento de formulário HTML](https://www.w3.org/TR/html401/interact/forms.html)

* [Solicitação de Token de verificação](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [Associação de modelo](../models/model-binding.md)

* [Validação de modelo](../models/validation.md)

* [anotações de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)

* [Trechos de código para este documento de código](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).
