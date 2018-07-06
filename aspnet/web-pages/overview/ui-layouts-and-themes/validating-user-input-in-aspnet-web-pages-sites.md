---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validação de entrada do usuário na Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo discute como validar informações obtidas de usuários &mdash; ou seja, informações em HTML para certificar-se de que os usuários insiram válido formam em um....
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d412f3fa4ca144a8a9107c971279f7bf2663cfe5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819254"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validando a entrada do usuário em Sites do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo discute como validar informações obtidas de usuários &mdash; ou seja, para certificar-se de que os usuários insiram válido informações em HTML de formulários em um site de páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como verificar se uma entrada do usuário corresponde a critérios de validação que você definir.
> - Como determinar se todos os testes de validação passaram.
> - Como exibir erros de validação (e como formatá-los).
> - Como validar dados que não vem diretamente de usuários.
> 
> Estes são os conceitos apresentados neste artigo de programação do ASP.NET:
> 
> - O `Validation` auxiliar.
> - O `Html.ValidationSummary` e `Html.ValidationMessage` métodos.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


Este artigo contém as seguintes seções:

- [Visão geral da validação de entrada do usuário](#Overview_of_User_Input_Validation)
- [Validação de entrada do usuário](#Validating_User_Input)
- [Adicionando uma validação do lado do cliente](#Adding_Client-Side_Validation)
- [Erros de validação de formatação](#Formatting_Validation_Errors)
- [Validação de dados que não vêm diretamente de usuários](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Visão geral da validação de entrada do usuário

Se você pedir que os usuários insiram informações em uma página — por exemplo, em um formulário — é importante certificar-se de que os valores que eles digitam são válidos. Por exemplo, você não quiser processar um formulário que está faltando informações críticas.

Quando os usuários inserem valores em um formulário HTML, os valores que eles digitam são cadeias de caracteres. Em muitos casos, os valores que necessários são outros tipos de dados, como números inteiros ou datas. Portanto, você também precisa certificar-se de que os valores que os usuários inserem podem ser convertidos corretamente para os tipos de dados apropriado.

Você também pode ter algumas restrições nos valores. Mesmo que os usuários inserem corretamente um número inteiro, por exemplo, você talvez precise Certifique-se de que o valor está em um determinado intervalo.

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** validar a entrada do usuário também é importante para a segurança. Quando você restringe os valores que os usuários podem inserir em formulários, você pode reduzir a chance de que alguém pode inserir um valor que pode comprometer a segurança de seu site.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validação de entrada do usuário

No ASP.NET Web Pages 2, você pode usar o `Validator` auxiliares para testar a entrada do usuário. A abordagem básica é fazer o seguinte:

1. Determine qual entrada elementos (campos) que deseja validar.

    Normalmente você valide os valores em `<input>` elementos em um formulário. No entanto, é uma boa prática validar todas as entradas, até mesmo de entrada que vem de um elemento restrito como um `<select>` lista. Isso ajuda a garantir que os usuários não ignorar os controles em uma página e enviar um formulário.
2. Na página de código, adicionar verificações de validação individuais para cada elemento de entrada usando métodos do `Validation` auxiliar.

    Para verificar os campos obrigatórios, use `Validation.RequireField(field, [error message])` (para um campo individual) ou `Validation.RequireFields(field1, field2, ...))` (para obter uma lista de campos). Para outros tipos de validação, use `Validation.Add(field, ValidationType)`. Para `ValidationType`, você pode usar essas opções:

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
3. Quando a página é enviada, verifique se a validação tiver passado, verificando `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Se houver erros de validação, você pode ignorar o processamento de página normal. Por exemplo, se a finalidade da página é atualizar um banco de dados, você não fazer isso até que todos os erros de validação foram corrigidos.
4. Se houver erros de validação, exibir mensagens de erro na marcação da página, usando `Html.ValidationSummary` ou `Html.ValidationMessage`, ou ambos.

O exemplo a seguir mostra uma página que ilustra essas etapas.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Para ver como funciona a validação, execute esta página e deliberadamente cometer erros. Por exemplo, aqui está o que a página fique semelhante se você se esqueça de inserir um nome de curso, se você inserir um, e se você inserir uma data inválida:

![Erros de validação na página renderizada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Adicionando uma validação do lado do cliente

Por padrão, a entrada do usuário é validada depois que os usuários enviam a página — ou seja, a validação é executada no código do servidor. Uma desvantagem dessa abordagem é que os usuários não saibam que eles fizeram um erro até depois que eles enviam a página. Se um formulário é longa ou complexa, pode ser inconveniente para o usuário relatando erros somente depois que a página é enviada.

Você pode adicionar suporte para executar a validação no script de cliente. Nesse caso, a validação é executada conforme os usuários trabalham no navegador. Por exemplo, suponha que você especificar que um valor deve ser um inteiro. Se um usuário inserir um valor não inteiro, o erro é relatado, assim que o usuário deixa o campo de entrada. Os usuários obtenham um feedback imediato, o que é conveniente para eles. Validação baseada em cliente também pode reduzir o número de vezes que o usuário tem que enviar o formulário para corrigir vários erros.

> [!NOTE]
> Mesmo se você usa a validação do lado do cliente, validação também sempre é executada no código do servidor. Executar a validação no código do servidor é uma medida de segurança, no caso de usuários ignorar a validação do cliente.


1. Registre as seguintes bibliotecas JavaScript na página:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Duas das bibliotecas são pode ser carregada a partir de uma rede de distribuição de conteúdo (CDN), portanto, você não precisa necessariamente tê-los em seu computador ou servidor. No entanto, você deve ter uma cópia local do *jquery.validate.unobtrusive.js*. Se você ainda não estiver trabalhando com um modelo do WebMatrix (como **Starter Site** ) que inclui a biblioteca, crie um site de páginas da Web que se baseia **Starter Site**. Em seguida, copie o *. js* arquivo para seu site atual.
2. Na marcação, para cada elemento que você está validando, adicione uma chamada para `Validation.For(field)`. Esse método emite atributos que são usados pela validação do lado do cliente. (Em vez de emitir código real de JavaScript, o método emite atributos como `data-val-...`. Esses atributos suporte à validação não invasiva do cliente que usa o jQuery para fazer o trabalho.)

A página a seguir mostra como adicionar recursos de validação de cliente para o exemplo mostrado anteriormente.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nem todas as verificações de validação executadas no cliente. Em particular, validação de tipo de dados (inteiro, data e assim por diante) não são executados no cliente. As seguintes verificações de trabalham no cliente e servidor:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

Neste exemplo, o teste de uma data válida não funcionará no código do cliente. No entanto, o teste será executado no código do servidor.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Erros de validação de formatação

Você pode controlar como os erros de validação são exibidos, definindo as classes CSS que têm os seguintes nomes reservados:

- `field-validation-error`. Define a saída do `Html.ValidationMessage` método quando ele está exibindo um erro.
- `field-validation-valid`. Define a saída do `Html.ValidationMessage` método quando não há nenhum erro.
- `input-validation-error`. Define como `<input>` elementos são renderizados quando ocorre um erro. (Por exemplo, você pode usar esta classe para definir a cor do plano de fundo de um &lt;entrada&gt; elemento para uma cor diferente se o seu valor é inválido.) Essa classe CSS é usado somente durante a validação do cliente (no ASP.NET Web Pages 2).
- `input-validation-valid`. Define a aparência dos `<input>` elementos quando não há nenhum erro.
- `validation-summary-errors`. Define a saída do `Html.ValidationSummary` método, ele está exibindo uma lista de erros.
- `validation-summary-valid`. Define a saída do `Html.ValidationSummary` método quando não há nenhum erro.

O seguinte `<style>` bloco mostra as regras para condições de erro.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Se você incluir este bloco de estilo nas páginas de exemplo do anteriormente neste artigo, a exibição de erro será semelhante a ilustração a seguir:

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Se você não estiver usando a validação do cliente no ASP.NET Web Pages 2, o CSS classes para o `<input>` elementos (`input-validation-error` e `input-validation-valid` não têm qualquer efeito.


### <a name="static-and-dynamic-error-display"></a>Exibição de erro estáticas e dinâmicas

As regras de CSS vêm em pares, como `validation-summary-errors` e `validation-summary-valid`. Esses pares permitem que você defina regras para as duas condições: uma condição de erro e uma condição (não de erro) "normal". É importante entender que a marcação para a exibição de erro sempre é processada, mesmo se não houver nenhum erro. Por exemplo, se uma página tiver um `Html.ValidationSummary` método na marcação, a origem da página conterá a seguinte marcação, mesmo quando a página é solicitada pela primeira vez:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Em outras palavras, o `Html.ValidationSummary` método sempre renderiza um `<div>` elemento e uma lista, mesmo se a lista de erros está vazia. Da mesma forma, o `Html.ValidationMessage` método sempre renderiza um `<span>` elemento como um espaço reservado para um erro de campo individual, mesmo se não houver nenhum erro.

Em algumas situações, exibindo uma mensagem de erro pode fazer com que a página a retransmissão e pode fazer com que elementos na página para mover-se. As regras de CSS que terminam em `-valid` permitem que você defina um layout que pode ajudar a evitar esse problema. Por exemplo, você pode definir `field-validation-error` e `field-validation-valid` para ambos têm o mesmo tamanho fixo. Dessa forma, a área de exibição para o campo é estática e não alterar o fluxo de página, se uma mensagem de erro é exibida.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validação de dados que não vêm diretamente de usuários

Às vezes, você precisa validar as informações que não vêm diretamente de um formulário HTML. Um exemplo típico é uma página em que um valor é passado em uma cadeia de caracteres de consulta, como no exemplo a seguir:

`http://server/myapp/EditClassInformation?classid=1022`

Nesse caso, você deseja ter certeza de que o valor que é passado para a página (aqui, 1022 para o valor de `classid`) é válido. Você não pode usar diretamente o `Validation` auxiliares para realizar essa validação. No entanto, você pode usar outros recursos do sistema de validação, como a capacidade de exibir mensagens de erro de validação.

> [!NOTE] 
> 
> **Importante** sempre valide valores obtidos *qualquer* fonte, incluindo valores de campo de formulário, os valores de cadeia de caracteres de consulta e valores de cookie. É fácil para as pessoas alterar esses valores (talvez para fins mal-intencionados). Portanto, você deve verificar esses valores para proteger seu aplicativo.


O exemplo a seguir mostra como você pode validar um valor que é passado em uma cadeia de caracteres de consulta. O código testa se o valor não está vazio e que ele é um inteiro.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Observe que o teste é executado quando a solicitação não é um envio de formulário (`if(!IsPost)`). Esse teste passaria na primeira vez que a página é solicitada, mas não quando a solicitação é o envio de um formulário.

Para exibir esse erro, você pode adicionar o erro à lista de erros de validação chamando `Validation.AddFormError("message")`. Se a página contém uma chamada para o `Html.ValidationSummary` método, o erro é exibido lá, assim como um erro de validação de entrada do usuário.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Trabalhando com formulários HTML em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
