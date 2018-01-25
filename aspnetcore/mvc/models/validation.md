---
title: "Validação de modelo no ASP.NET MVC de núcleo"
author: rachelappel
description: "Saiba mais sobre a validação do modelo no ASP.NET MVC de núcleo."
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 56928c61ae47d313145afadf3e0fa93a078b681b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Introdução à validação do modelo no ASP.NET MVC de núcleo

Por [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introdução à validação do modelo

Antes de um aplicativo armazena dados em um banco de dados, o aplicativo deve validar os dados. Dados devem ser verificados quanto a possíveis ameaças de segurança, verificadas se está formatado corretamente por tipo e tamanho, e ele deve estar de acordo com as regras. Validação é necessária, embora ele possa ser tediosa implementar e redundantes. No MVC, a validação ocorre no cliente e no servidor.

Felizmente, o .NET tem abstraídos validação em atributos de validação. Esses atributos contêm o código de validação, reduzindo a quantidade de código que você deve gravar.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Atributos de validação

Atributos de validação são uma maneira de configurar a validação do modelo é semelhante conceitualmente a validação em campos em tabelas de banco de dados. Isso inclui as restrições, como a atribuição de tipos de dados ou os campos obrigatórios. Outros tipos de validação incluem a aplicação de padrões de dados para impor regras de negócio, como um cartão de crédito, número de telefone ou endereço de email. Atributos de validação Verifique impor esses requisitos muito mais simples e fácil de usar.

Abaixo está um anotado `Movie` modelo de um aplicativo que armazena informações sobre filmes e programas de TV. A maioria das propriedades é necessária e várias propriedades de cadeia de caracteres têm requisitos de comprimento. Além disso, há uma restrição de intervalo numérico em vigor para o `Price` propriedade de 0 a $999,99, juntamente com um atributo de validação personalizada.

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

Simplesmente lê por meio do modelo revela as regras sobre os dados para este aplicativo, facilitando a manutenção do código. Abaixo estão vários atributos de validação interna populares:

* `[CreditCard]`: Valida a propriedade tem um formato de cartão de crédito.

* `[Compare]`: Valida as duas propriedades em uma correspondência de modelo.

* `[EmailAddress]`: Valida a propriedade tem um formato de email.

* `[Phone]`: Valida a propriedade tem um formato de telefone.

* `[Range]`: Valida o valor da propriedade está dentro do intervalo especificado.

* `[RegularExpression]`: Valida os dados correspondem à expressão regular especificada.

* `[Required]`: Torna uma propriedade necessária.

* `[StringLength]`: Valida que uma propriedade de cadeia de caracteres no máximo tem o comprimento máximo especificado.

* `[Url]`: Valida a propriedade tem um formato de URL.

MVC oferece suporte a qualquer atributo que deriva de `ValidationAttribute` para fins de validação. Muitos atributos de validação útil podem ser encontrados no [DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.

Pode haver ocasiões em que você precisar de mais recursos de atributos internos fornecem. Para ocasiões, é possível criar atributos de validação personalizada derivando de `ValidationAttribute` ou alterar seu modelo para implementar `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Observações sobre o uso do atributo necessário

Os [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) que não permitem valores nulos (como `decimal`, `int`, `float` e `DateTime`) são inerentemente necessários e não precisam do atributo `Required`. O aplicativo não executa nenhuma verificação de validação do lado do servidor para tipos não anuláveis são marcados `Required`.

Associação de modelo MVC, que não está preocupada com atributos de validação e a validação, rejeita o envio de um campo de formulário que contém um valor ausente ou o espaço em branco para um tipo não anulável. Na ausência de um `BindRequired` atributo na propriedade de destino, a associação de modelo ignora dados ausentes para tipos não anuláveis, onde o campo de formulário está ausente nos dados de formulário de entrada.

O [BindRequired atributo](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (Consulte também [personalizar o comportamento de associação de modelo com atributos](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) é útil para garantir que os dados de formulário são concluídos. Quando aplicado a uma propriedade, o sistema de associação de modelo requer um valor para essa propriedade. Quando aplicado a um tipo, o sistema de associação de modelo requer valores para todas as propriedades desse tipo.

Quando você usa um [Nullable\<T > tipo](/dotnet/csharp/programming-guide/nullable-types/) (por exemplo, `decimal?` ou `System.Nullable<decimal>`) e marcá-la `Required`, é realizada uma verificação de validação do lado do servidor, como se a propriedade fosse um tipo anulável padrão (para exemplo, um `string`).

Validação do lado do cliente requer um valor para um campo de formulário que corresponde a uma propriedade de modelo que você marcou `Required` e para uma propriedade de tipo não anulável que ainda não marcado `Required`. `Required`pode ser usado para controlar a mensagem de erro de validação do lado do cliente.

## <a name="model-state"></a>Estado do modelo

Estado do modelo representa erros de validação em valores de formulário HTML enviados.

MVC continuará validando campos até atingir o número máximo de erros (200 por padrão). Você pode configurar esse número inserindo o seguinte código para o `ConfigureServices` método o *Startup.cs* arquivo:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Estado do modelo de tratamento de erros

Ocorre a validação do modelo antes de cada ação de controlador que está sendo invocada, e é responsabilidade do método de ação para inspecionar `ModelState.IsValid` e reagir de forma adequada. Em muitos casos, a reação apropriada é retornar uma resposta de erro, idealmente, detalhando o motivo por que falha na validação do modelo.

Alguns aplicativos escolherá a seguir uma convenção padrão para lidar com erros de validação de modelo, em que o caso de um filtro pode ser um local adequado para implementar essa política. Você deve testar o comportam de suas ações com os estados de modelo válidas e inválidas.

## <a name="manual-validation"></a>Validação manual

Após a conclusão validação e associação de modelo, talvez queira repetir partes dele. Por exemplo, um usuário pode ter inserido texto em um campo esperado um número inteiro, ou talvez seja necessário calcular um valor de propriedade do modelo.

Talvez seja necessário executar manualmente a validação. Para fazer isso, chame o `TryValidateModel` método, conforme mostrado aqui:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Validação personalizada

Atributos de validação funcionam para a maioria das necessidades de validação. No entanto, algumas regras de validação são específicas para o seu negócio. As regras não podem ser técnicas comuns de validação de dados, como garantir um campo é obrigatório ou que está de acordo com um intervalo de valores. Para esses cenários, os atributos de validação personalizada são uma excelente solução. É fácil criar seus próprios atributos de validação personalizada no MVC. Apenas herdam o `ValidationAttribute`e substituir o `IsValid` método. O `IsValid` método aceita dois parâmetros, a primeira é um objeto chamado *valor* e o segundo é um `ValidationContext` objeto chamado *validationContext*. *Valor* refere-se ao valor real do campo que o validador personalizado está sendo validada.

No exemplo a seguir, uma regra de negócio afirma que os usuários não podem definir o gênero para *clássico* de um filme liberado 1960. O `[ClassicMovie]` atributo verifica o gênero primeiro e, se for um clássico, ele verifica a data de lançamento para ver se ele é posterior à 1960. Se ele for liberado 1960, a validação falhará. O atributo aceita um parâmetro de número inteiro que representa o ano que você pode usar para validar dados. Você pode capturar o valor do parâmetro no construtor do atributo, conforme mostrado aqui:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

O `movie` variável acima representa um `Movie` objeto que contém os dados de envio do formulário para validar. Nesse caso, o código de validação verifica a data e gênero no `IsValid` método o `ClassicMovieAttribute` classe de acordo com as regras. Após a validação bem-sucedida `IsValid` retorna um `ValidationResult.Success` código, e quando a validação falhar, um `ValidationResult` com uma mensagem de erro. Quando um usuário modifica o `Genre` campo e envia o formulário, o `IsValid` método o `ClassicMovieAttribute` verificará se o filme é uma clássica. Como qualquer atributo interno, se aplicam a `ClassicMovieAttribute` para uma propriedade como `ReleaseDate` para garantir a validação ocorre, conforme mostrado no exemplo de código anterior. Como o exemplo funciona apenas com `Movie` tipos, a melhor opção é usar `IValidatableObject` conforme mostrado no seguinte parágrafo.

Como alternativa, esse mesmo código pode ser colocado no modelo Implementando o `Validate` método sobre o `IValidatableObject` interface. Enquanto os atributos de validação personalizada funcionam bem para validar as propriedades individuais, implementando `IValidatableObject` pode ser usado para implementar a validação de nível de classe como visto aqui.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Validação do lado do cliente

Validação do lado do cliente é uma ótima conveniência para usuários. Ele salva o tempo que outra forma seriam passam aguardando uma viagem de ida e para o servidor. Em termos de negócios, até mesmo algumas frações de segundos multiplicados centenas de vezes por dia adiciona até ser bastante tempo, despesas e frustração. Validação de imediata e simples permite que os usuários trabalhem com mais eficiência e produzir a melhor qualidade de entrada e saída.

Você deve ter uma exibição com as referências de script JavaScript corretas em vigor para a validação do lado do cliente a funcionar como você vê aqui.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

O [jQuery validação discreto](https://github.com/aspnet/jquery-validation-unobtrusive) script é uma biblioteca personalizada front-end do Microsoft que se baseia no conhecido [jQuery validar](https://jqueryvalidation.org/) plug-in. Sem jQuery validação discreto, você teria codificar a mesma lógica de validação em dois lugares: uma vez em que os atributos de validação do lado de servidor nas propriedades do modelo e, em seguida, novamente em scripts do lado do cliente (os exemplos para jQuery validar [ `validate()` ](https://jqueryvalidation.org/validate/) método mostra como complexas que isso poderia ser). Em vez disso, do MVC [auxiliares de marcação](xref:mvc/views/tag-helpers/intro) e [auxiliares HTML](xref:mvc/views/overview) podem usar os atributos de validação e digite os metadados das propriedades de modelo para renderizar HTML 5 [atributos de dados](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) em os elementos de formulário que precisam de validação. MVC gera o `data-` atributos para atributos internos e personalizados. jQuery validação discreto, em seguida, analisa thes `data-` atributos e passa a lógica para jQuery validar, "Copiar" efetivamente a lógica de validação do lado de servidor para o cliente. Você pode exibir erros de validação no cliente usando os auxiliares de marca relevantes, como mostrado aqui:

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Os auxiliares de marca acima renderizam HTML abaixo. Observe que o `data-` atributos no HTML de saída corresponde aos atributos de validação para o `ReleaseDate` propriedade. O `data-val-required` abaixo do atributo contém uma mensagem de erro a ser exibido se o usuário não preencher o campo de data de lançamento. jQuery discreto validação passa esse valor para o jQuery validar [ `required()` ](https://jqueryvalidation.org/required-method/) método, que, em seguida, exibe a mensagem em que o acompanha  **\<span >** elemento.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Validação do lado do cliente evita envio até que o formulário é válido. Botão Enviar executa JavaScript que envia o formulário ou exibe mensagens de erro.

MVC determina os valores de atributo de tipo com base no tipo de dados .NET de uma propriedade, possivelmente substituído usando `[DataType]` atributos. A base de `[DataType]` atributo não faz nenhuma validação do lado do servidor real. Navegadores escolha suas próprias mensagens de erro e exibem os erros, mas desejar, porém o pacote discreto de validação jQuery pode substituir as mensagens e exibi-los de forma consistente com outras pessoas. Isso acontece mais óbvio é quando os usuários aplicar `[DataType]` subclasses como `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Adicionar validação a formulários dinâmicos

Porque jQuery discreto validação passa parâmetros e lógica de validação para jQuery validar quando a página for carregada pela primeira vez, formulários gerados dinamicamente automaticamente não exibem a validação. Em vez disso, você deve informar jQuery discreto validação ao analisar o formulário dinâmico imediatamente depois de criá-lo. Por exemplo, o código a seguir mostra como você pode configurar a validação do lado do cliente em um formulário adicionado por meio de AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

O `$.validator.unobtrusive.parse()` método aceita um seletor de jQuery para um argumento. Esse método informa jQuery discreto validação ao analisar o `data-` atributos de formulários no seletor. Os valores desses atributos são passados para o plug-in do jQuery validar para que o formulário exibe as regras de validação do lado cliente desejado.

### <a name="add-validation-to-dynamic-controls"></a>Adicionar validação a controles dinâmicos

Você também pode atualizar as regras de validação em um formulário quando controles individuais, como `<input/>`s e `<select/>`s, são geradas dinamicamente. Você não pode passar seletores para esses elementos para o `parse()` diretamente o método porque o formulário ao redor já foi analisado e não são atualizados. Em vez disso, você primeiro remova os dados de validação existente e nova análise todo o formulário, conforme mostrado abaixo:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Você pode criar lógica do lado do cliente para o atributo personalizado, e [validação discreta](http://jqueryvalidation.org/documentation/) será executado no cliente para você automaticamente como parte da validação. A primeira etapa é controlar quais atributos de dados são adicionados, Implementando a `IClientModelValidator` interface conforme mostrado aqui:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Atributos que implementam esta interface podem adicionar atributos HTML para campos gerados. Examinando a saída para o `ReleaseDate` revela de elemento HTML que é semelhante ao exemplo anterior, exceto que agora há uma `data-val-classicmovie` atributo que foi definido no `AddValidation` método `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Validação discreta usa os dados a `data-` atributos para exibir mensagens de erro. No entanto, o jQuery não sabe sobre as regras ou mensagens até que você os adiciona do jQuery `validator` objeto. Isso é mostrado no exemplo a seguir que adiciona um método chamado `classicmovie` que contém o código de validação de cliente personalizadas para o jQuery `validator` objeto.

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

Agora o jQuery tem as informações para executar a validação personalizada do JavaScript, bem como a mensagem de erro a ser exibido se esse código de validação retornará false.

## <a name="remote-validation"></a>Validação remota

Validação remota é um ótimo recurso para usar quando você precisa validar os dados no cliente em relação aos dados no servidor. Por exemplo, seu aplicativo precisará verificar se um email ou nome de usuário já está em uso, e ele deve consultar uma grande quantidade de dados para fazer isso. Baixando grandes conjuntos de dados para validar um ou alguns campos consome muitos recursos. Ele também pode expor informações confidenciais. Uma alternativa é fazer uma solicitação de ida e volta para validar um campo.

Você pode implementar a validação remota em um processo de duas etapas. Primeiro, você deve anotar o modelo com o `[Remote]` atributo. O `[Remote]` atributo aceita várias sobrecargas que você pode usar para direcionar o JavaScript do lado do cliente para o código apropriado para chamar. O exemplo a seguir aponta para o `VerifyEmail` método de ação de `Users` controlador.

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

A segunda etapa é colocar o código de validação no método de ação correspondente, conforme definido no `[Remote]` atributo. Acordo com o jQuery validar [ `remote()` ](https://jqueryvalidation.org/remote-method/) documentação do método:

> A resposta de serverside deve ser uma cadeia de caracteres JSON deve ser `"true"` para elementos válidos e pode ser `"false"`, `undefined`, ou `null` para elementos inválidos, usando a mensagem de erro padrão. Se a resposta serverside é uma cadeia de caracteres, por exemplo. `"That name is already taken, try peter123 instead"`, essa cadeia de caracteres será exibida como uma mensagem de erro personalizada em vez do padrão.

A definição do `VerifyEmail()` método segue estas regras, conforme mostrado abaixo. Retorna um erro de validação de mensagem se o email foi obtido, ou `true` se o email é gratuito e encapsula o resultado em uma `JsonResult` objeto. O lado do cliente pode usar o valor retornado para continuar ou exibir o erro, se necessário.

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

Agora quando os usuários inserem um email, o JavaScript no modo de exibição faz uma chamada remota para ver se o email foi executada e, em caso afirmativo, exibe a mensagem de erro. Caso contrário, o usuário pode enviar o formulário como de costume.

O `AdditionalFields` propriedade o `[Remote]` atributo é útil para validação de combinações de campos em relação aos dados no servidor. Por exemplo, se o `User` modelo acima tinha duas propriedades adicionais chamadas `FirstName` e `LastName`, talvez você queira verificar que não há usuários existentes já tem esse par de nomes. Você define as novas propriedades, conforme mostrado no código a seguir:

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields`pode já foi definido explicitamente para as cadeias de caracteres `"FirstName"` e `"LastName"`, mas usando o [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operador como isso simplifica a refatoração mais tarde. O método de ação para executar a validação, em seguida, deve aceitar os dois argumentos, um para o valor de `FirstName` e outro para o valor de `LastName`.

[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

Agora quando os usuários inserir um nome e sobrenome, JavaScript:

* Faz uma chamada remota para ver se esse par de nomes foi usado.
* Se o par tiver sido tomado, uma mensagem de erro será exibida. 
* Se não ocorrer, o usuário pode enviar o formulário.

Se você precisa validar dois ou mais campos adicionais com o `[Remote]` atributo, você fornecê-los como uma lista delimitada por vírgulas. Por exemplo, para adicionar um `MiddleName` definida no modelo, o `[Remote]` atributo conforme mostrado no código a seguir:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, como todos os argumentos de atributo deve ser uma expressão constante. Portanto, você não deve usar um [interpolados cadeia de caracteres](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) ou chame [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) inicializar `AdditionalFields`. Para cada campo adicional que você adicionar ao `[Remote]` atributo, você deve adicionar outro argumento para o método de ação do controlador correspondente.
