---
title: Adicionar validação a uma Página Razor do ASP.NET Core
author: rick-anderson
description: Saiba como adicionar validação a uma Página Razor no ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: d4cc0ab9de314c0c5a1a9016efd1e566ff1c47d2
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505772"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="6781c-103">Adicionar validação a uma Página Razor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6781c-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="6781c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6781c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6781c-105">Nesta seção, a lógica de validação é adicionada para o modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6781c-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="6781c-106">As regras de validação são impostas sempre que um usuário cria ou edita um filme.</span><span class="sxs-lookup"><span data-stu-id="6781c-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="6781c-107">Validação</span><span class="sxs-lookup"><span data-stu-id="6781c-107">Validation</span></span>

<span data-ttu-id="6781c-108">Um princípio-chave do desenvolvimento de software é chamado [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) (“**D**on't **R**epeat **Y**ourself”).</span><span class="sxs-lookup"><span data-stu-id="6781c-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="6781c-109">As Páginas Razor incentivam o desenvolvimento quando a funcionalidade é especificada uma vez e ela é refletida em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6781c-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="6781c-110">O DRY pode ajudar a reduzir a quantidade de código em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6781c-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="6781c-111">O DRY faz com que o código seja menos propenso a erros e mais fácil de testar e manter.</span><span class="sxs-lookup"><span data-stu-id="6781c-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="6781c-112">O suporte de validação fornecido pelas Páginas Razor e pelo Entity Framework é um bom exemplo do princípio DRY.</span><span class="sxs-lookup"><span data-stu-id="6781c-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="6781c-113">As regras de validação são especificadas de forma declarativa em um único lugar (na classe de modelo) e as regras são impostas em qualquer lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6781c-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="6781c-114">Adicionando regras de validação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="6781c-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="6781c-115">Abra o arquivo *Models/Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6781c-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="6781c-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) fornece um conjunto interno de atributos de validação que são aplicados de forma declarativa a uma classe ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="6781c-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="6781c-117">DataAnnotations também contém atributos de formatação como `DataType`, que ajudam com a formatação e não fornecem validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="6781c-118">Atualize a classe `Movie` para aproveitar os atributos de validação `Required`, `StringLength`, `RegularExpression` e `Range`.</span><span class="sxs-lookup"><span data-stu-id="6781c-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6781c-119">Os atributos de validação especificam o comportamento que é imposto nas propriedades do modelo:</span><span class="sxs-lookup"><span data-stu-id="6781c-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="6781c-120">Os atributos `Required` e `MinimumLength` indicam que uma propriedade deve ter um valor.</span><span class="sxs-lookup"><span data-stu-id="6781c-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="6781c-121">No entanto, nada impede que um usuário digite espaços em branco para atender à restrição de validação para um tipo de permite valor nulo.</span><span class="sxs-lookup"><span data-stu-id="6781c-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="6781c-122">Os [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) que não permitem valores nulos (como `decimal`, `int`, `float` e `DateTime`) são inerentemente necessários e não precisam do atributo `Required`.</span><span class="sxs-lookup"><span data-stu-id="6781c-122">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="6781c-123">O atributo `RegularExpression` limita os caracteres que o usuário pode inserir.</span><span class="sxs-lookup"><span data-stu-id="6781c-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="6781c-124">No código anterior, `Genre` precisa começar com uma ou mais letras maiúsculas e seguir com zero ou mais letras, aspas simples ou duplas, caracteres de espaço em branco ou traço.</span><span class="sxs-lookup"><span data-stu-id="6781c-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="6781c-125">`Rating` precisa começar com uma ou mais letras maiúsculas e seguir com zero ou mais letras, números, aspas simples ou duplas, caracteres de espaço em branco ou traço.</span><span class="sxs-lookup"><span data-stu-id="6781c-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="6781c-126">O atributo `Range` restringe um valor a um intervalo especificado.</span><span class="sxs-lookup"><span data-stu-id="6781c-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="6781c-127">O atributo `StringLength` define o tamanho máximo de uma cadeia de caracteres e, opcionalmente, o tamanho mínimo.</span><span class="sxs-lookup"><span data-stu-id="6781c-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="6781c-128">Ter as regras de validação automaticamente impostas pelo ASP.NET Core ajuda a tornar um aplicativo mais robusto.</span><span class="sxs-lookup"><span data-stu-id="6781c-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="6781c-129">A validação automática em modelos ajuda a proteger o aplicativo porque você não precisa se lembrar de aplicá-los quando um novo código é adicionado.</span><span class="sxs-lookup"><span data-stu-id="6781c-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="6781c-130">Interface do usuário do erro de validação nas Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="6781c-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="6781c-131">Execute o aplicativo e navegue para Pages/Movies.</span><span class="sxs-lookup"><span data-stu-id="6781c-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="6781c-132">Selecione o link **Criar Novo**.</span><span class="sxs-lookup"><span data-stu-id="6781c-132">Select the **Create New** link.</span></span> <span data-ttu-id="6781c-133">Preencha o formulário com alguns valores inválidos.</span><span class="sxs-lookup"><span data-stu-id="6781c-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="6781c-134">Quando a validação do lado do cliente do jQuery detecta o erro, ela exibe uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="6781c-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Formulário da exibição de filmes com vários erros de validação do lado do cliente do jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="6781c-136">Observe como o formulário renderizou automaticamente uma mensagem de erro de validação em cada campo que contém um valor inválido.</span><span class="sxs-lookup"><span data-stu-id="6781c-136">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="6781c-137">Os erros são impostos no lado do cliente (usando o JavaScript e o jQuery) e no lado do servidor (quando um usuário tem o JavaScript desabilitado).</span><span class="sxs-lookup"><span data-stu-id="6781c-137">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="6781c-138">Uma vantagem significativa é que **nenhuma** alteração de código foi necessária nas páginas Criar ou Editar.</span><span class="sxs-lookup"><span data-stu-id="6781c-138">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="6781c-139">Depois que DataAnnotations foi aplicado ao modelo, a interface do usuário de validação foi habilitada.</span><span class="sxs-lookup"><span data-stu-id="6781c-139">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="6781c-140">As Páginas Razor criadas neste tutorial selecionaram automaticamente as regras de validação (usando os atributos de validação nas propriedades da classe do modelo `Movie`).</span><span class="sxs-lookup"><span data-stu-id="6781c-140">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="6781c-141">Validação do teste usando a página Editar: a mesma validação é aplicada.</span><span class="sxs-lookup"><span data-stu-id="6781c-141">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="6781c-142">Os dados de formulário não serão postados no servidor enquanto houver erros de validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="6781c-142">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="6781c-143">Verifique se os dados de formulário não são postados por uma ou mais das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="6781c-143">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="6781c-144">Coloque um ponto de interrupção no método `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="6781c-144">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="6781c-145">Envie o formulário (selecione **Criar** ou **Salvar**).</span><span class="sxs-lookup"><span data-stu-id="6781c-145">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="6781c-146">O ponto de interrupção nunca é atingido.</span><span class="sxs-lookup"><span data-stu-id="6781c-146">The break point is never hit.</span></span>
* <span data-ttu-id="6781c-147">Use a [ferramenta Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="6781c-147">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="6781c-148">Use as ferramentas do desenvolvedor do navegador para monitorar o tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="6781c-148">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="6781c-149">Validação do servidor</span><span class="sxs-lookup"><span data-stu-id="6781c-149">Server-side validation</span></span>

<span data-ttu-id="6781c-150">Quando o JavaScript está desabilitado no navegador, o envio do formulário com erros será postado no servidor.</span><span class="sxs-lookup"><span data-stu-id="6781c-150">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="6781c-151">(Opcional) Teste a validação do servidor:</span><span class="sxs-lookup"><span data-stu-id="6781c-151">Optional, test server-side validation:</span></span>

* <span data-ttu-id="6781c-152">Desabilite o JavaScript no navegador.</span><span class="sxs-lookup"><span data-stu-id="6781c-152">Disable JavaScript in the browser.</span></span> <span data-ttu-id="6781c-153">É possível fazer isso usando as ferramentas para desenvolvedores do navegador.</span><span class="sxs-lookup"><span data-stu-id="6781c-153">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="6781c-154">Se você não conseguir desabilitar o JavaScript no navegador, tente outro navegador.</span><span class="sxs-lookup"><span data-stu-id="6781c-154">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="6781c-155">Defina um ponto de interrupção no método `OnPostAsync` da página Criar ou Editar.</span><span class="sxs-lookup"><span data-stu-id="6781c-155">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="6781c-156">Envie um formulário com erros de validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-156">Submit a form with validation errors.</span></span>
* <span data-ttu-id="6781c-157">Verifique se o estado do modelo é inválido:</span><span class="sxs-lookup"><span data-stu-id="6781c-157">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="6781c-158">O código a seguir mostra uma parte da página *Create.cshtml* gerada por scaffolding anteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="6781c-158">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="6781c-159">Ele é usado pelas páginas Criar e Editar para exibir o formulário inicial e exibir o formulário novamente, em caso de erro.</span><span class="sxs-lookup"><span data-stu-id="6781c-159">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="6781c-160">O [Auxiliar de Marcação de Entrada](xref:mvc/views/working-with-forms) usa os atributos de [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produz os atributos HTML necessários para a Validação do jQuery no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="6781c-160">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="6781c-161">O [Auxiliar de Marcação de Validação](xref:mvc/views/working-with-forms#the-validation-tag-helpers) exibe erros de validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-161">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="6781c-162">Consulte [Validação](xref:mvc/models/validation) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="6781c-162">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="6781c-163">As páginas Criar e Editar não têm nenhuma regra de validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-163">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="6781c-164">As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6781c-164">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="6781c-165">Essas regras de validação são aplicadas automaticamente às Páginas Razor que editam o modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6781c-165">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="6781c-166">Quando a lógica de validação precisa ser alterada, ela é feita apenas no modelo.</span><span class="sxs-lookup"><span data-stu-id="6781c-166">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="6781c-167">A validação é aplicada de forma consistente em todo o aplicativo (a lógica de validação é definida em um único lugar).</span><span class="sxs-lookup"><span data-stu-id="6781c-167">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="6781c-168">A validação em um único lugar ajuda a manter o código limpo e facilita sua manutenção e atualização.</span><span class="sxs-lookup"><span data-stu-id="6781c-168">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="6781c-169">Usando atributos DataType</span><span class="sxs-lookup"><span data-stu-id="6781c-169">Using DataType Attributes</span></span>

<span data-ttu-id="6781c-170">Examine a classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6781c-170">Examine the `Movie` class.</span></span> <span data-ttu-id="6781c-171">O namespace `System.ComponentModel.DataAnnotations` fornece atributos de formatação, além do conjunto interno de atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-171">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="6781c-172">O atributo `DataType` é aplicado às propriedades `ReleaseDate` e `Price`.</span><span class="sxs-lookup"><span data-stu-id="6781c-172">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="6781c-173">Os atributos `DataType` fornecem apenas dicas para que o mecanismo de exibição formate os dados (e fornece atributos como `<a>` para as URLs e `<a href="mailto:EmailAddress.com">` para o email).</span><span class="sxs-lookup"><span data-stu-id="6781c-173">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="6781c-174">Use o atributo `RegularExpression` para validar o formato dos dados.</span><span class="sxs-lookup"><span data-stu-id="6781c-174">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="6781c-175">O atributo `DataType` é usado para especificar um tipo de dados mais específico do que o tipo intrínseco de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6781c-175">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="6781c-176">Os atributos `DataType` não são atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-176">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="6781c-177">No aplicativo de exemplo, apenas a data é exibida, sem a hora.</span><span class="sxs-lookup"><span data-stu-id="6781c-177">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="6781c-178">A Enumeração `DataType` fornece muitos tipos de dados, como Date, Time, PhoneNumber, Currency, EmailAddress e muito mais.</span><span class="sxs-lookup"><span data-stu-id="6781c-178">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="6781c-179">O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo.</span><span class="sxs-lookup"><span data-stu-id="6781c-179">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="6781c-180">Por exemplo, um link `mailto:` pode ser criado para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="6781c-180">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="6781c-181">Um seletor de data pode ser fornecido para `DataType.Date` em navegadores que dão suporte a HTML5.</span><span class="sxs-lookup"><span data-stu-id="6781c-181">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="6781c-182">Os atributos `DataType` emitem atributos `data-` HTML 5 (pronunciados “data dash”) que são consumidos pelos navegadores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="6781c-182">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="6781c-183">Os atributos `DataType` **não** fornecem nenhuma validação.</span><span class="sxs-lookup"><span data-stu-id="6781c-183">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="6781c-184">`DataType.Date` não especifica o formato da data exibida.</span><span class="sxs-lookup"><span data-stu-id="6781c-184">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="6781c-185">Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base nas `CultureInfo` do servidor.</span><span class="sxs-lookup"><span data-stu-id="6781c-185">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6781c-186">A anotação de dados `[Column(TypeName = "decimal(18, 2)")]` é necessária para que o Entity Framework Core possa mapear corretamente o `Price` para a moeda no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6781c-186">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="6781c-187">Para obter mais informações, veja [Tipos de Dados](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="6781c-187">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="6781c-188">O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:</span><span class="sxs-lookup"><span data-stu-id="6781c-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="6781c-189">A configuração `ApplyFormatInEditMode` especifica que a formatação deve ser aplicada quando o valor é exibido para edição.</span><span class="sxs-lookup"><span data-stu-id="6781c-189">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="6781c-190">Não é recomendável ter esse comportamento em alguns campos.</span><span class="sxs-lookup"><span data-stu-id="6781c-190">You might not want that behavior for some fields.</span></span> <span data-ttu-id="6781c-191">Por exemplo, em valores de moeda, você provavelmente não deseja que o símbolo de moeda seja exibido na interface do usuário de edição.</span><span class="sxs-lookup"><span data-stu-id="6781c-191">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="6781c-192">O atributo `DisplayFormat` pode ser usado por si só, mas geralmente é uma boa ideia usar o atributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="6781c-192">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="6781c-193">O atributo `DataType` transmite a semântica dos dados, ao invés de apresentar como renderizá-lo em uma tela e oferece os seguintes benefícios que você não obtém com DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="6781c-193">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="6781c-194">O navegador pode habilitar os recursos do HTML5 (por exemplo, mostrar um controle de calendário, o símbolo de moeda apropriado à localidade, links de email, etc.)</span><span class="sxs-lookup"><span data-stu-id="6781c-194">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="6781c-195">Por padrão, o navegador renderizará os dados usando o formato correto de acordo com a localidade.</span><span class="sxs-lookup"><span data-stu-id="6781c-195">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="6781c-196">O atributo `DataType` pode permitir que a estrutura ASP.NET Core escolha o modelo de campo correto para renderizar os dados.</span><span class="sxs-lookup"><span data-stu-id="6781c-196">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="6781c-197">O `DisplayFormat`, se usado por si só, usa o modelo de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="6781c-197">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="6781c-198">Observação: a validação do jQuery não funciona com os atributos `Range` e `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="6781c-198">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="6781c-199">Por exemplo, o seguinte código sempre exibirá um erro de validação do lado do cliente, mesmo quando a data estiver no intervalo especificado:</span><span class="sxs-lookup"><span data-stu-id="6781c-199">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="6781c-200">Geralmente, não é uma boa prática compilar datas rígidas nos modelos e, portanto, o uso do atributo `Range` e de `DateTime` não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="6781c-200">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="6781c-201">O seguinte código mostra como combinar atributos em uma linha:</span><span class="sxs-lookup"><span data-stu-id="6781c-201">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6781c-202">[Comece com o Razor Pages e o EF Core](xref:data/ef-rp/intro) mostra operações mais avançadas do EF Core com o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6781c-202">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="6781c-203">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="6781c-203">Publish to Azure</span></span>

<span data-ttu-id="6781c-204">Para obter informações sobre como implantar no Azure, consulte [Tutorial: compilar um aplicativo ASP.NET no Azure com o Banco de Dados SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="6781c-204">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="6781c-205">Essas instruções são para um aplicativo ASP.NET, não um aplicativo ASP.NET Core, mas as etapas são as mesmas.</span><span class="sxs-lookup"><span data-stu-id="6781c-205">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="6781c-206">Obrigado por concluir esta introdução às Páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="6781c-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="6781c-207">Agradecemos os comentários.</span><span class="sxs-lookup"><span data-stu-id="6781c-207">We appreciate feedback.</span></span> <span data-ttu-id="6781c-208">A [Introdução ao Razor Pages e ao EF Core](xref:data/ef-rp/intro) é um excelente acompanhamento para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="6781c-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6781c-209">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6781c-209">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [<span data-ttu-id="6781c-210">Anterior: Adicionando um novo campo</span><span class="sxs-lookup"><span data-stu-id="6781c-210">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
