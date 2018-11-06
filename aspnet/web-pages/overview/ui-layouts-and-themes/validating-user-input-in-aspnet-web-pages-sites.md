---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validação de entrada do usuário na Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: Rick-Anderson
description: Este artigo discute como validar informações obtidas de usuários &mdash; ou seja, informações em HTML para certificar-se de que os usuários insiram válido formam em um....
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 8f049adce33e452896b5e2a444635ff30d18e480
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021515"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="b8902-103">Validando a entrada do usuário em Sites do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="b8902-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="b8902-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b8902-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b8902-105">Este artigo discute como validar informações obtidas de usuários &mdash; ou seja, para certificar-se de que os usuários insiram válido informações em HTML de formulários em um site de páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="b8902-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="b8902-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="b8902-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b8902-107">Como verificar se uma entrada do usuário corresponde a critérios de validação que você definir.</span><span class="sxs-lookup"><span data-stu-id="b8902-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="b8902-108">Como determinar se todos os testes de validação passaram.</span><span class="sxs-lookup"><span data-stu-id="b8902-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="b8902-109">Como exibir erros de validação (e como formatá-los).</span><span class="sxs-lookup"><span data-stu-id="b8902-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="b8902-110">Como validar dados que não vem diretamente de usuários.</span><span class="sxs-lookup"><span data-stu-id="b8902-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="b8902-111">Estes são os conceitos apresentados neste artigo de programação do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b8902-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="b8902-112">O `Validation` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b8902-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="b8902-113">O `Html.ValidationSummary` e `Html.ValidationMessage` métodos.</span><span class="sxs-lookup"><span data-stu-id="b8902-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b8902-114">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="b8902-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b8902-115">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b8902-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b8902-116">Este tutorial também funciona com ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="b8902-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="b8902-117">Este artigo contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="b8902-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="b8902-118">Visão geral da validação de entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="b8902-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="b8902-119">Validação de entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="b8902-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="b8902-120">Adicionando uma validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="b8902-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="b8902-121">Erros de validação de formatação</span><span class="sxs-lookup"><span data-stu-id="b8902-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="b8902-122">Validação de dados que não vêm diretamente de usuários</span><span class="sxs-lookup"><span data-stu-id="b8902-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="b8902-123">Visão geral da validação de entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="b8902-123">Overview of User Input Validation</span></span>

<span data-ttu-id="b8902-124">Se você pedir que os usuários insiram informações em uma página — por exemplo, em um formulário — é importante certificar-se de que os valores que eles digitam são válidos.</span><span class="sxs-lookup"><span data-stu-id="b8902-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="b8902-125">Por exemplo, você não quiser processar um formulário que está faltando informações críticas.</span><span class="sxs-lookup"><span data-stu-id="b8902-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="b8902-126">Quando os usuários inserem valores em um formulário HTML, os valores que eles digitam são cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="b8902-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="b8902-127">Em muitos casos, os valores que necessários são outros tipos de dados, como números inteiros ou datas.</span><span class="sxs-lookup"><span data-stu-id="b8902-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="b8902-128">Portanto, você também precisa certificar-se de que os valores que os usuários inserem podem ser convertidos corretamente para os tipos de dados apropriado.</span><span class="sxs-lookup"><span data-stu-id="b8902-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="b8902-129">Você também pode ter algumas restrições nos valores.</span><span class="sxs-lookup"><span data-stu-id="b8902-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="b8902-130">Mesmo que os usuários inserem corretamente um número inteiro, por exemplo, você talvez precise Certifique-se de que o valor está em um determinado intervalo.</span><span class="sxs-lookup"><span data-stu-id="b8902-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="b8902-132">**Importante** validar a entrada do usuário também é importante para a segurança.</span><span class="sxs-lookup"><span data-stu-id="b8902-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="b8902-133">Quando você restringe os valores que os usuários podem inserir em formulários, você pode reduzir a chance de que alguém pode inserir um valor que pode comprometer a segurança de seu site.</span><span class="sxs-lookup"><span data-stu-id="b8902-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="b8902-134">Validação de entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="b8902-134">Validating User Input</span></span>

<span data-ttu-id="b8902-135">No ASP.NET Web Pages 2, você pode usar o `Validator` auxiliares para testar a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="b8902-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="b8902-136">A abordagem básica é fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="b8902-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="b8902-137">Determine qual entrada elementos (campos) que deseja validar.</span><span class="sxs-lookup"><span data-stu-id="b8902-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="b8902-138">Normalmente você valide os valores em `<input>` elementos em um formulário.</span><span class="sxs-lookup"><span data-stu-id="b8902-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="b8902-139">No entanto, é uma boa prática validar todas as entradas, até mesmo de entrada que vem de um elemento restrito como um `<select>` lista.</span><span class="sxs-lookup"><span data-stu-id="b8902-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="b8902-140">Isso ajuda a garantir que os usuários não ignorar os controles em uma página e enviar um formulário.</span><span class="sxs-lookup"><span data-stu-id="b8902-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="b8902-141">Na página de código, adicionar verificações de validação individuais para cada elemento de entrada usando métodos do `Validation` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b8902-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="b8902-142">Para verificar os campos obrigatórios, use `Validation.RequireField(field, [error message])` (para um campo individual) ou `Validation.RequireFields(field1, field2, ...))` (para obter uma lista de campos).</span><span class="sxs-lookup"><span data-stu-id="b8902-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="b8902-143">Para outros tipos de validação, use `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="b8902-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="b8902-144">Para `ValidationType`, você pode usar essas opções:</span><span class="sxs-lookup"><span data-stu-id="b8902-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="b8902-145">Quando a página é enviada, verifique se a validação tiver passado, verificando `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="b8902-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="b8902-146">Se houver erros de validação, você pode ignorar o processamento de página normal.</span><span class="sxs-lookup"><span data-stu-id="b8902-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="b8902-147">Por exemplo, se a finalidade da página é atualizar um banco de dados, você não fazer isso até que todos os erros de validação foram corrigidos.</span><span class="sxs-lookup"><span data-stu-id="b8902-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="b8902-148">Se houver erros de validação, exibir mensagens de erro na marcação da página, usando `Html.ValidationSummary` ou `Html.ValidationMessage`, ou ambos.</span><span class="sxs-lookup"><span data-stu-id="b8902-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="b8902-149">O exemplo a seguir mostra uma página que ilustra essas etapas.</span><span class="sxs-lookup"><span data-stu-id="b8902-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="b8902-150">Para ver como funciona a validação, execute esta página e deliberadamente cometer erros.</span><span class="sxs-lookup"><span data-stu-id="b8902-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="b8902-151">Por exemplo, aqui está o que a página fique semelhante se você se esqueça de inserir um nome de curso, se você inserir um, e se você inserir uma data inválida:</span><span class="sxs-lookup"><span data-stu-id="b8902-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Erros de validação na página renderizada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="b8902-153">Adicionando uma validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="b8902-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="b8902-154">Por padrão, a entrada do usuário é validada depois que os usuários enviam a página — ou seja, a validação é executada no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="b8902-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="b8902-155">Uma desvantagem dessa abordagem é que os usuários não saibam que eles fizeram um erro até depois que eles enviam a página.</span><span class="sxs-lookup"><span data-stu-id="b8902-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="b8902-156">Se um formulário é longa ou complexa, pode ser inconveniente para o usuário relatando erros somente depois que a página é enviada.</span><span class="sxs-lookup"><span data-stu-id="b8902-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="b8902-157">Você pode adicionar suporte para executar a validação no script de cliente.</span><span class="sxs-lookup"><span data-stu-id="b8902-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="b8902-158">Nesse caso, a validação é executada conforme os usuários trabalham no navegador.</span><span class="sxs-lookup"><span data-stu-id="b8902-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="b8902-159">Por exemplo, suponha que você especificar que um valor deve ser um inteiro.</span><span class="sxs-lookup"><span data-stu-id="b8902-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="b8902-160">Se um usuário inserir um valor não inteiro, o erro é relatado, assim que o usuário deixa o campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="b8902-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="b8902-161">Os usuários obtenham um feedback imediato, o que é conveniente para eles.</span><span class="sxs-lookup"><span data-stu-id="b8902-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="b8902-162">Validação baseada em cliente também pode reduzir o número de vezes que o usuário tem que enviar o formulário para corrigir vários erros.</span><span class="sxs-lookup"><span data-stu-id="b8902-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="b8902-163">Mesmo se você usa a validação do lado do cliente, validação também sempre é executada no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="b8902-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="b8902-164">Executar a validação no código do servidor é uma medida de segurança, no caso de usuários ignorar a validação do cliente.</span><span class="sxs-lookup"><span data-stu-id="b8902-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="b8902-165">Registre as seguintes bibliotecas JavaScript na página:</span><span class="sxs-lookup"><span data-stu-id="b8902-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="b8902-166">Duas das bibliotecas são pode ser carregada a partir de uma rede de distribuição de conteúdo (CDN), portanto, você não precisa necessariamente tê-los em seu computador ou servidor.</span><span class="sxs-lookup"><span data-stu-id="b8902-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="b8902-167">No entanto, você deve ter uma cópia local do *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="b8902-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="b8902-168">Se você ainda não estiver trabalhando com um modelo do WebMatrix (como **Starter Site** ) que inclui a biblioteca, crie um site de páginas da Web que se baseia **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="b8902-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="b8902-169">Em seguida, copie o *. js* arquivo para seu site atual.</span><span class="sxs-lookup"><span data-stu-id="b8902-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="b8902-170">Na marcação, para cada elemento que você está validando, adicione uma chamada para `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="b8902-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="b8902-171">Esse método emite atributos que são usados pela validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b8902-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="b8902-172">(Em vez de emitir código real de JavaScript, o método emite atributos como `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="b8902-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="b8902-173">Esses atributos suporte à validação não invasiva do cliente que usa o jQuery para fazer o trabalho.)</span><span class="sxs-lookup"><span data-stu-id="b8902-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="b8902-174">A página a seguir mostra como adicionar recursos de validação de cliente para o exemplo mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b8902-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="b8902-175">Nem todas as verificações de validação executadas no cliente.</span><span class="sxs-lookup"><span data-stu-id="b8902-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="b8902-176">Em particular, validação de tipo de dados (inteiro, data e assim por diante) não são executados no cliente.</span><span class="sxs-lookup"><span data-stu-id="b8902-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="b8902-177">As seguintes verificações de trabalham no cliente e servidor:</span><span class="sxs-lookup"><span data-stu-id="b8902-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="b8902-178">Neste exemplo, o teste de uma data válida não funcionará no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="b8902-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="b8902-179">No entanto, o teste será executado no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="b8902-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="b8902-180">Erros de validação de formatação</span><span class="sxs-lookup"><span data-stu-id="b8902-180">Formatting Validation Errors</span></span>

<span data-ttu-id="b8902-181">Você pode controlar como os erros de validação são exibidos, definindo as classes CSS que têm os seguintes nomes reservados:</span><span class="sxs-lookup"><span data-stu-id="b8902-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="b8902-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="b8902-182">`field-validation-error`.</span></span> <span data-ttu-id="b8902-183">Define a saída do `Html.ValidationMessage` método quando ele está exibindo um erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="b8902-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="b8902-184">`field-validation-valid`.</span></span> <span data-ttu-id="b8902-185">Define a saída do `Html.ValidationMessage` método quando não há nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="b8902-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="b8902-186">`input-validation-error`.</span></span> <span data-ttu-id="b8902-187">Define como `<input>` elementos são renderizados quando ocorre um erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="b8902-188">(Por exemplo, você pode usar esta classe para definir a cor do plano de fundo de um &lt;entrada&gt; elemento para uma cor diferente se o seu valor é inválido.) Essa classe CSS é usado somente durante a validação do cliente (no ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="b8902-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="b8902-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="b8902-189">`input-validation-valid`.</span></span> <span data-ttu-id="b8902-190">Define a aparência dos `<input>` elementos quando não há nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="b8902-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="b8902-191">`validation-summary-errors`.</span></span> <span data-ttu-id="b8902-192">Define a saída do `Html.ValidationSummary` método, ele está exibindo uma lista de erros.</span><span class="sxs-lookup"><span data-stu-id="b8902-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="b8902-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="b8902-193">`validation-summary-valid`.</span></span> <span data-ttu-id="b8902-194">Define a saída do `Html.ValidationSummary` método quando não há nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="b8902-195">O seguinte `<style>` bloco mostra as regras para condições de erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="b8902-196">Se você incluir este bloco de estilo nas páginas de exemplo do anteriormente neste artigo, a exibição de erro será semelhante a ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="b8902-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="b8902-198">Se você não estiver usando a validação do cliente no ASP.NET Web Pages 2, o CSS classes para o `<input>` elementos (`input-validation-error` e `input-validation-valid` não têm qualquer efeito.</span><span class="sxs-lookup"><span data-stu-id="b8902-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="b8902-199">Exibição de erro estáticas e dinâmicas</span><span class="sxs-lookup"><span data-stu-id="b8902-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="b8902-200">As regras de CSS vêm em pares, como `validation-summary-errors` e `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="b8902-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="b8902-201">Esses pares permitem que você defina regras para as duas condições: uma condição de erro e uma condição (não de erro) "normal".</span><span class="sxs-lookup"><span data-stu-id="b8902-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="b8902-202">É importante entender que a marcação para a exibição de erro sempre é processada, mesmo se não houver nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="b8902-203">Por exemplo, se uma página tiver um `Html.ValidationSummary` método na marcação, a origem da página conterá a seguinte marcação, mesmo quando a página é solicitada pela primeira vez:</span><span class="sxs-lookup"><span data-stu-id="b8902-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="b8902-204">Em outras palavras, o `Html.ValidationSummary` método sempre renderiza um `<div>` elemento e uma lista, mesmo se a lista de erros está vazia.</span><span class="sxs-lookup"><span data-stu-id="b8902-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="b8902-205">Da mesma forma, o `Html.ValidationMessage` método sempre renderiza um `<span>` elemento como um espaço reservado para um erro de campo individual, mesmo se não houver nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="b8902-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="b8902-206">Em algumas situações, exibindo uma mensagem de erro pode fazer com que a página a retransmissão e pode fazer com que elementos na página para mover-se.</span><span class="sxs-lookup"><span data-stu-id="b8902-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="b8902-207">As regras de CSS que terminam em `-valid` permitem que você defina um layout que pode ajudar a evitar esse problema.</span><span class="sxs-lookup"><span data-stu-id="b8902-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="b8902-208">Por exemplo, você pode definir `field-validation-error` e `field-validation-valid` para ambos têm o mesmo tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="b8902-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="b8902-209">Dessa forma, a área de exibição para o campo é estática e não alterar o fluxo de página, se uma mensagem de erro é exibida.</span><span class="sxs-lookup"><span data-stu-id="b8902-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="b8902-210">Validação de dados que não vêm diretamente de usuários</span><span class="sxs-lookup"><span data-stu-id="b8902-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="b8902-211">Às vezes, você precisa validar as informações que não vêm diretamente de um formulário HTML.</span><span class="sxs-lookup"><span data-stu-id="b8902-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="b8902-212">Um exemplo típico é uma página em que um valor é passado em uma cadeia de caracteres de consulta, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b8902-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="b8902-213">Nesse caso, você deseja ter certeza de que o valor que é passado para a página (aqui, 1022 para o valor de `classid`) é válido.</span><span class="sxs-lookup"><span data-stu-id="b8902-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="b8902-214">Você não pode usar diretamente o `Validation` auxiliares para realizar essa validação.</span><span class="sxs-lookup"><span data-stu-id="b8902-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="b8902-215">No entanto, você pode usar outros recursos do sistema de validação, como a capacidade de exibir mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="b8902-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b8902-216">**Importante** sempre valide valores obtidos *qualquer* fonte, incluindo valores de campo de formulário, os valores de cadeia de caracteres de consulta e valores de cookie.</span><span class="sxs-lookup"><span data-stu-id="b8902-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="b8902-217">É fácil para as pessoas alterar esses valores (talvez para fins mal-intencionados).</span><span class="sxs-lookup"><span data-stu-id="b8902-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="b8902-218">Portanto, você deve verificar esses valores para proteger seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8902-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="b8902-219">O exemplo a seguir mostra como você pode validar um valor que é passado em uma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="b8902-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="b8902-220">O código testa se o valor não está vazio e que ele é um inteiro.</span><span class="sxs-lookup"><span data-stu-id="b8902-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="b8902-221">Observe que o teste é executado quando a solicitação não é um envio de formulário (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="b8902-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="b8902-222">Esse teste passaria na primeira vez que a página é solicitada, mas não quando a solicitação é o envio de um formulário.</span><span class="sxs-lookup"><span data-stu-id="b8902-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="b8902-223">Para exibir esse erro, você pode adicionar o erro à lista de erros de validação chamando `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="b8902-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="b8902-224">Se a página contém uma chamada para o `Html.ValidationSummary` método, o erro é exibido lá, assim como um erro de validação de entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="b8902-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b8902-225">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b8902-225">Additional Resources</span></span>

[<span data-ttu-id="b8902-226">Trabalhando com formulários HTML em Sites de páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b8902-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
