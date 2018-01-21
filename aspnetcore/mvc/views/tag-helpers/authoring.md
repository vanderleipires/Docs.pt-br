---
title: "Criação de auxiliares de marcação no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como criar auxiliares de marcação no núcleo do ASP.NET."
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9aaf40377e07e53fd0b7ebb177bcbb2df52b7553
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Auxiliares de marcação de autor no núcleo do ASP.NET, um passo a passo com exemplos

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Introdução ao auxiliares de marcação

Este tutorial fornece uma introdução à programação auxiliares de marcação. [Introdução ao auxiliares de marcação](intro.md) descreve os benefícios que fornecem auxiliares de marcação.

Um auxiliar de marca é qualquer classe que implementa o `ITagHelper` interface. No entanto, quando você cria um auxiliar de marca, você geralmente deriva `TagHelper`, fornece acesso ao `Process` método.

1. Criar um novo projeto ASP.NET Core chamado **AuthoringTagHelpers**. Você não precisa de autenticação para este projeto.

2. Criar uma pasta para armazenar os auxiliares de marca chamado *TagHelpers*. O *TagHelpers* pasta é *não* necessário, mas é uma convenção razoável. Agora vamos começar a escrever alguns auxiliares de marca simples.

## <a name="a-minimal-tag-helper"></a>Um auxiliar de marca mínima

Nesta seção, você escreve um auxiliar de marca que atualiza uma marca de email. Por exemplo:

```html
<email>Support</email>
   ```

O servidor usará nosso auxiliar de marca de email para converter essa marcação como a seguir:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Ou seja, uma marca de âncora que torna isso um link de email. Você talvez queira fazer isso se você estiver escrevendo um mecanismo de blog e precisa enviar email de marketing, suporte e outros contatos, todos ao mesmo domínio.

1.  Adicione o seguinte `EmailTagHelper` de classe para o *TagHelpers* pasta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Observações:**
    
    * Auxiliares de marcação usam uma convenção de nomenclatura que tem como alvo os elementos do nome da classe raiz (menos o *TagHelper* parte do nome de classe). Neste exemplo, o nome da raiz **Email**TagHelper é *email*, portanto, o `<email>` marca será direcionada. Essa convenção de nomenclatura deve funcionar para a maioria dos auxiliares de marcação, posteriormente, mostrarei como substituí-la.
    
    * O `EmailTagHelper` classe deriva de `TagHelper`. O `TagHelper` classe fornece métodos e propriedades para gravar auxiliares de marcação.
    
    * Substituído `Process` método controla o que faz o auxiliar de marca quando executado. O `TagHelper` classe também fornece uma versão assíncrona (`ProcessAsync`) com os mesmos parâmetros.
    
    * O parâmetro de contexto para `Process` (e `ProcessAsync`) contém informações associadas com a execução da marca HTML atual.
    
    * O parâmetro de saída para `Process` (e `ProcessAsync`) contém um elemento HTML com monitoração de estado representa a fonte original usada para gerar uma marca HTML e o conteúdo.
    
    * Nome da nossa classe possui um sufixo de **TagHelper**, que é *não* necessário, mas é considerada uma convenção de prática recomendada. Você pode declarar a classe como:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Para fazer o `EmailTagHelper` classe disponíveis a todos os nossos modos de exibição do Razor, adicione o `addTagHelper` diretiva para o *Views/_ViewImports.cshtml* arquivo:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    O código anterior usa a sintaxe de curinga para especificar que todos os auxiliares de marca no nosso assembly estarão disponíveis. A primeira cadeia de caracteres após `@addTagHelper` Especifica o auxiliar de marca para carregar (Use "*" para todos os auxiliares de marcação), e a segunda cadeia de caracteres "AuthoringTagHelpers" Especifica o assembly de auxiliar de marca está em. Além disso, observe que a segunda linha coloca nos auxiliares de marca do MVC do ASP.NET Core usando a sintaxe de curinga (os auxiliares são discutidos em [Introdução ao auxiliares de marcação](intro.md).) É o `@addTagHelper` diretiva que disponibiliza o auxiliar de marca para o modo de exibição do Razor. Como alternativa, você pode fornecer o nome totalmente qualificado (FQN) de um auxiliar de marca, conforme mostrado abaixo:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Para adicionar um auxiliar de marca para uma exibição usando um FQN, primeiro adicione o FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e, em seguida, o nome do assembly (*AuthoringTagHelpers*). A maioria dos desenvolvedores vão preferir usar a sintaxe de curinga. [Introdução ao auxiliares de marcação](intro.md) apresenta detalhes sobre a sintaxe de adição, remoção, hierarquia e curinga do auxiliar de marca.
    
3.  Atualizar a marcação no *Views/Home/Contact.cshtml* arquivos com essas alterações:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Execute o aplicativo e usar seu navegador favorito para exibir o código-fonte HTML para verificar se as marcas de email são substituídas com marcação de âncora (por exemplo, `<a>Support</a>`). *Suporte* e *Marketing* são renderizados como links, mas não têm um `href` atributo para torná-los funcional. Corrigiremos que na próxima seção.

Observação: Como marcas e atributos, marcas, nomes de classes e atributos HTML no Razor e c# não são diferencia maiusculas de minúsculas.

## <a name="setattribute-and-setcontent"></a>SetAttribute e SetContent

Nesta seção, vamos atualizar o `EmailTagHelper` para que ele cria uma marca de âncora válido para email. Vamos atualizar para obter informações de um modo de exibição do Razor (na forma de um `mail-to` atributo) e usá-lo a gerar a âncora.

Atualização de `EmailTagHelper` classe com o seguinte:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Observações:**

* Nomes de classe e a propriedade de maiusculas e minúsculas de Pascal para os auxiliares de marca são convertidos em seus [inferior caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Portanto, para usar o `MailTo` atributo, você usará `<email mail-to="value"/>` equivalente.

* A última linha define o conteúdo concluído para nosso auxiliar de marca minimamente funcional.

* A linha realçada mostra a sintaxe para adicionar atributos:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Essa abordagem funciona para o atributo "href" como no momento, ele não existe na coleção de atributos. Você também pode usar o `output.Attributes.Add` para adicionar um atributo do auxiliar de marca ao final da coleção de atributos de marca.

1.  Atualizar a marcação no *Views/Home/Contact.cshtml* arquivos com essas alterações:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Execute o aplicativo e verificar que ele gera os links corretos.
    
    > [!NOTE]
    >Se você escrever o email marca de fechamento automático (`<email mail-to="Rick" />`), a saída final também seriam fechamento automático. Para habilitar a capacidade de gravar a marca com uma marca de início (`<email mail-to="Rick">`) deve decorar a classe com o seguinte:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    Com um fechamento automático auxiliar de marca de email, a saída será `<a href="mailto:Rick@contoso.com" />`. Marcas de âncora de fechamento automático não são HTML válido, portanto, você não quiser criar um, mas talvez você queira criar um auxiliar de marca é fechamento automático. Auxiliares de marcação definir o tipo do `TagMode` propriedade depois de ler uma marca.
    
### <a name="processasync"></a>ProcessAsync

Nesta seção, vamos escrever um auxiliar de email assíncrona.

1.  Substitua o `EmailTagHelper` classe com o código a seguir:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Observações:**

    * Essa versão usa o assíncrona `ProcessAsync` método. O assíncrona `GetChildContentAsync` retorna um `Task` que contém o `TagHelperContent`.

    * Use o `output` para obter o conteúdo do elemento HTML.

2.  Faça a seguinte alteração para o *Views/Home/Contact.cshtml* de arquivo para o auxiliar de marca possa obter o email de destino.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Execute o aplicativo e verificar que ele gera links de email válido.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent

1.  Adicione o seguinte `BoldTagHelper` de classe para o *TagHelpers* pasta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Observações:**
    
    * O `[HtmlTargetElement]` atributo passa um parâmetro de atributo que especifica que qualquer elemento HTML que contém um atributo HTML denominado "bold" fará a correspondência, e o `Process` substituição do método na classe será executado. Em nosso exemplo, o `Process` método Remove o atributo "bold" e envolve a marcação que contém com `<strong></strong>`.
    
    * Porque você não deseja substituir a conteúdo de marca existente, você deve escrever a abertura `<strong>` marca com o `PreContent.SetHtmlContent` método e o fechamento `</strong>` marca com o `PostContent.SetHtmlContent` método.
    
2.  Modificar o *About.cshtml* exibição para conter um `bold` valor do atributo. O código completo é mostrado abaixo.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Execute o aplicativo. Você pode usar o seu navegador favorito para inspecionar a origem e verifique se a marcação.

    O `[HtmlTargetElement]` atributo acima se destina somente a marcação HTML que fornece um nome de atributo de "bold". O `<bold>` elemento não foi modificado pelo auxiliar de marca.

4. Comente o `[HtmlTargetElement]` linha de atributo e o padrão serão direcionamento `<bold>` marcas, ou seja, a marcação HTML do formulário `<bold>`. Lembre-se de que a convenção de nomenclatura padrão corresponderá ao nome da classe **negrito**TagHelper para `<bold>` marcas.

5. Execute o aplicativo e verificar se o `<bold>` a marca é processada pelo auxiliar de marca.

Decoração de uma classe com vários `[HtmlTargetElement]` resulta em um OR lógico de destinos de atributos. Por exemplo, usando o código a seguir, uma marca em negrito ou um atributo em negrito fará a correspondência.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Quando vários atributos são adicionados à mesma instrução, o tempo de execução trata como uma lógica AND. Por exemplo, no código a seguir, um elemento HTML deve ser nomeado "bold" com um atributo chamado "bold" (`<bold bold />`) para fazer a correspondência.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Você também pode usar o `[HtmlTargetElement]` para alterar o nome do elemento de destino. Por exemplo, se você quisesse o `BoldTagHelper` destino `<MyBold>` marcas, você usaria o seguinte atributo:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Passar um modelo para um auxiliar de marca

1.  Adicionar um *modelos* pasta.

2.  Adicione a seguinte classe `WebsiteContext` à pasta *Models*:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Adicione o seguinte `WebsiteInformationTagHelper` de classe para o *TagHelpers* pasta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Observações:**
    
    * Conforme mencionado anteriormente, auxiliares de marcação converte maiusculas e minúsculas de Pascal c# nomes de classes e propriedades para os auxiliares de marca em [inferior caso kebab](http://wiki.c2.com/?KebabCase). Portanto, para usar o `WebsiteInformationTagHelper` Razor, você escreverá `<website-information />`.
    
    * Você não explicitamente identificando o elemento de destino com o `[HtmlTargetElement]` de atributo, isso o padrão de `website-information` será direcionada. Se você aplicou o seguinte atributo (Observação não for o caso de kebab, mas corresponde ao nome de classe):
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    A marca de caso kebab inferior `<website-information />` não serão compatíveis. Se você deseja usar o `[HtmlTargetElement]` atributo, você usaria caso kebab conforme mostrado abaixo:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Elementos de fechamento automático não têm nenhum conteúdo. Neste exemplo, a marcação Razor usará uma marca de fechamento automático, mas o auxiliar de marca criará um [seção](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (que não é fechamento automático e você estiver escrevendo o conteúdo dentro do `section` elemento). Portanto, você precisa definir `TagMode` para `StartTagAndEndTag` para gravar a saída. Como alternativa, você pode comentar a linha que define `TagMode` e escrita de marcação com uma marca de fechamento. (Marcação de exemplo é fornecida posteriormente neste tutorial.)
    
    * O `$` (cifrão) na linha a seguir usa uma [interpolados cadeia de caracteres](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Adicione a seguinte marcação para o *About.cshtml* exibição. A marcação realçada exibe as informações do site da web.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > Na marcação Razor mostrada abaixo:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >O Razor Saiba o `info` atributo é uma classe, não uma cadeia de caracteres, e você quiser escrever código c#. Qualquer atributo do auxiliar de marca de cadeia de caracteres não deve ser escrito sem a `@` caracteres.
    
5.  Executar o aplicativo e navegue até o modo de exibição sobre para ver as informações do site da web.

    >[!NOTE]
    >Você pode usar a seguinte marcação com uma marca de fechamento e remova a linha com `TagMode.StartTagAndEndTag` no auxiliar de marca:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Auxiliar de marca de condição

O auxiliar de marca de condição renderiza a saída quando passado um valor true.

1.  Adicione o seguinte `ConditionTagHelper` de classe para o *TagHelpers* pasta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Substitua o conteúdo do *Views/Home/Index.cshtml* arquivo com a seguinte marcação:

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Substitua o `Index` método o `Home` controlador com o código a seguir:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Executar o aplicativo e navegue até a página inicial. A marcação na condicional `div` não será renderizado. Acrescente a cadeia de caracteres de consulta `?approved=true` para a URL (por exemplo, `http://localhost:1235/Home/Index?approved=true`). `approved`é definido como true e a condicional marcação será exibida.

>[!NOTE]
>Use o [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador para especificar o atributo de destino em vez de especificar uma cadeia de caracteres como você fez com o auxiliar de marca em negrito:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>O [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador protegerá o código deve ele nunca ser refatorado (queremos alterar o nome para `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Evitar conflitos de auxiliar de marca

Nesta seção, você escreve um par de auxiliares de marcação de vinculação automática. O primeiro substituirá a marcação que contém uma URL iniciada por HTTP para um HTML âncora marca que contém a mesma URL (e, portanto, resultando em um link de URL). O segundo fará o mesmo para uma URL começando com WWW.

Como esses dois auxiliares estão intimamente relacionados e pode refatorá-los no futuro, vamos mantê-los no mesmo arquivo.

1.  Adicione o seguinte `AutoLinkerHttpTagHelper` de classe para o *TagHelpers* pasta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >O `AutoLinkerHttpTagHelper` classe destinos `p` elementos e usa [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para criar a âncora.

2.  Adicione a seguinte marcação para o fim do *Views/Home/Contact.cshtml* arquivo:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Execute o aplicativo e verificar se o auxiliar de marca renderizado corretamente a âncora.

4.  Atualização de `AutoLinker` classe para incluir o `AutoLinkerWwwTagHelper` que converterá www texto em uma marca de âncora que contém o texto original da Web. O código atualizado é realçado abaixo:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Execute o aplicativo. Observe o texto www é renderizado como um link, mas o texto HTTP não está. Se você colocar um ponto de interrupção em ambas as classes, você pode ver que a classe do auxiliar de marca HTTP é executado primeira. O problema é que a saída do auxiliar de marca é armazenado em cache e quando o auxiliar de marca da Web é executado, ele substitui a saída do cache do auxiliar de marca HTTP. Posteriormente no tutorial, veremos como controlar a ordem de auxiliares de marcação executados no. Corrigiremos o código com o seguinte:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >Na primeira edição os auxiliares de marca vinculação automática, você obteve o conteúdo do destino com o código a seguir:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >Ou seja, você chama `GetChildContentAsync` usando o `TagHelperOutput` passado para o `ProcessAsync` método. Como mencionado anteriormente, porque a saída é armazenada em cache, a última marca auxiliar para executar o wins. Corrigido o problema com o código a seguir:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >O código acima verifica para ver se o conteúdo tiver sido modificado e, em caso afirmativo, ele obtém o conteúdo do buffer de saída.

6.  Execute o aplicativo e verifique se os dois links funcionam como esperado. Enquanto pode aparecer que nosso auxiliar de marca de vinculador automática está correto e completa, ela tem um problema sutil. Se o auxiliar de marca da Web é executado primeiro, os links da Web não estarão corretos. Atualize o código adicionando o `Order` sobrecarga para controlar a ordem em que a marca é executado no. O `Order` propriedade determina a ordem de execução em relação a outros auxiliares de marcação direcionando o mesmo elemento. O valor de ordem padrão é zero e instâncias com valores mais baixos são executadas primeiro.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    O código acima, será possível garantir que o auxiliar de marca HTTP é executada antes do auxiliar de marca da Web. Alterar `Order` para `MaxValue` e verifique se a marcação gerada para a marca WWW está incorretova.

## <a name="inspect-and-retrieve-child-content"></a>Inspecionar e recuperar o conteúdo filho

Os auxiliares de marca fornecem várias propriedades para recuperar o conteúdo.

-  O resultado de `GetChildContentAsync` pode ser anexada à `output.Content`.
-  Você pode inspecionar o resultado de `GetChildContentAsync` com `GetContent`.
-  Se você modificar `output.Content`, o corpo da TagHelper não será executado ou renderizado a menos que você chame `GetChildContentAsync` como em nosso exemplo de vinculador automática:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Diversas chamadas para `GetChildContentAsync` retornará o mesmo valor e não será executado novamente o `TagHelper` corpo, a menos que você passar um parâmetro false indicando que não use o resultado em cache.
