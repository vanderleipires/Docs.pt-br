---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "Adicionando validação para o modelo | Microsoft Docs"
author: Rick-Anderson
description: "Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 93b4df5fcbde8d87866d00dffda8a241d0dd596b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model"></a>Adicionando validação para o modelo
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Isso nesta seção você adicionará a lógica de validação de `Movie` modelo e você garantirá que as regras de validação são aplicadas a qualquer momento em que um usuário tenta criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Manter as coisas SECA

Um dos fundamentos de design do ASP.NET MVC é simulação (&quot;não repetitivo&quot;). ASP.NET MVC incentiva você a especificar funcionalidade ou comportamento somente uma vez e, em seguida, ter refletidas em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa para escrever e faz com que o código que você escreve menos propenso a erro e mais fáceis de manter.

O suporte de validação fornecido pelo ASP.NET MVC e o Entity Framework Code First é um ótimo exemplo do princípio seco em ação. Você pode especificar declarativamente regras de validação em um lugar (a classe de modelo) e as regras são impostas em qualquer lugar no aplicativo.

Vamos ver como você pode tirar proveito desse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação para o modelo de filme

Você começará com a adição de alguma lógica de validação para o `Movie` classe.

Abra o arquivo *Movie.cs*. Adicionar um `using` instrução na parte superior do arquivo que faz referência a [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Observe que o namespace não contém `System.Web`. DataAnnotations fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente para qualquer classe ou propriedade.

Atualizar agora o `Movie` classe para aproveitar o interno [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validação . Use o código a seguir como um exemplo de onde aplicar os atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Execute o aplicativo e você receberá o seguinte erro de tempo de execução novamente:

***O modelo fazendo o contexto de 'MovieDBContext' foi alterado desde que o banco de dados foi criado. Considere o uso de migrações do Code First para atualizar o banco de dados ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Usaremos as migrações para atualizar o esquema. Compile a solução e, em seguida, abra o **Package Manager Console** janela e digite os seguintes comandos:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` classe derivada com o nome especificado (*AddDataAnnotationsMig*) e no `Up` método, você pode ver o código que atualiza as restrições de esquema. O `Title` e `Genre` campos não são anuláveis (ou seja, você deve inserir um valor) e o `Rating` campo tem um comprimento máximo de 5.

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. O `Required` atributo indica que uma propriedade deve ter um valor; nesse exemplo, um filme deve ter valores para o `Title`, `ReleaseDate`, `Genre`, e `Price` propriedades para serem válidas. O atributo `Range` restringe um valor a um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo. Tipos intrínsecos (como `decimal, int, float, DateTime`) são necessárias por padrão e não é necessário o `Required` atributo.

Código primeiro assegura que as regras de validação que você especificar em uma classe de modelo são aplicadas antes que o aplicativo salva as alterações no banco de dados. Por exemplo, o código a seguir lançará uma exceção quando o `SaveChanges` método é chamado, porque vários necessário `Movie` estiverem faltando valores de propriedade e o preço é zero (que está fora do intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Com as regras de validação automaticamente impostas pelo .NET Framework ajuda a tornar o seu aplicativo mais robusto. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

Aqui está um listagem para a atualização de códigos completa *Movie.cs* arquivo:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erro de validação de interface do usuário no ASP.NET MVC

Execute novamente o aplicativo e navegue até o */Movies* URL.

Clique o **criar novo** link para adicionar um novo filme. Preencha o formulário com alguns valores inválidos e, em seguida, clique no **criar** botão.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> para dar suporte a validação jQuery para idiomas diferentes do inglês que usam uma vírgula (&quot;,&quot;) para um ponto decimal, você deve incluir *globalize.js* e específicos *cultures/globalize.cultures.js* arquivo (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. O código a seguir mostra as modificações no arquivo Views\Movies\Edit.cshtml para trabalhar com o &quot;fr-FR&quot; cultura:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe como o formulário automaticamente tem usado uma cor de borda vermelha para realçar as caixas de texto que contêm dados inválidos e emitida uma mensagem de erro de validação apropriadas ao lado de cada um. Os erros são impostos no lado do cliente (usando o JavaScript e o jQuery) e no lado do servidor (caso um usuário tenha o JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código no `MoviesController` classe ou o *Create.cshtml* exibição para permitir que essa validação da interface do usuário. O controlador e as exibições criados anteriormente neste tutorial selecionaram automaticamente as regras de validação especificadas com atributos de validação nas propriedades da classe de modelo `Movie`.

Você deve ter notado para as propriedades `Title` e `Genre`, o atributo necessário não é imposto até que você enviar o formulário (atingido o **criar** botão), ou digite o texto no campo de entrada e removê-lo. Para um campo que é inicialmente vazio (como os campos no modo de criação) e que tem apenas o atributo necessário e nenhum outro atributo de validação, você pode fazer o seguinte para disparar a validação:

1. Guia para o campo.
2. Digite algum texto.
3. Guia saída.
4. Guia volta para o campo.
5. Remova o texto.
6. Guia saída.

A sequência acima disparará a validação necessária sem pressionar o botão de envio. Basta pressionar o botão de envio sem inserir qualquer um dos campos disparará a validação do lado do cliente. Os dados de formulário não são enviados no servidor até que não haja erros de validação do lado do cliente. Você pode testar isso colocando um ponto de interrupção no método HTTP Post ou usando o [ferramenta fiddler](http://fiddler2.com/fiddler2/) ou do IE 9 [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre em criar exibir e cria o método de ação

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A lista seguinte mostra o que o `Create` métodos de `MovieController` a aparência de classe. Eles são inalterados desde como são criados anteriormente neste tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

O primeiro método de ação (HTTP GET) `Create` exibe o formulário Criar inicial. A segunda versão (`[HttpPost]`) manipula a postagem de formulário. O segundo método `Create` (a versão `HttpPost`) chama `ModelState.IsValid` para verificar se o filme tem erros de validação. A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto. Se o objeto tiver erros de validação, o método `Create` exibirá o formulário novamente. Se não houver erros, o método salvará o novo filme no banco de dados. Em nosso exemplo de filme que estamos usando, **o formulário não é enviado ao servidor quando há erros de validação detectados no lado do cliente; o segundo** `Create` **nunca é chamado de método**. Se você desabilitar o JavaScript no navegador, a validação do cliente é desabilitada e o HTTP POST `Create` chamadas de método `ModelState.IsValid` para verificar se o filme tem erros de validação.

Defina um ponto de interrupção no método `HttpPost Create` e verifique se o método nunca é chamado; a validação do lado do cliente não enviará os dados de formulário quando forem detectados erros de validação. Se você desabilitar o JavaScript no navegador e, em seguida, enviar o formulário com erros, o ponto de interrupção será atingido. Você ainda pode obter uma validação completa sem o JavaScript. A imagem a seguir mostra como desabilitar JavaScript no Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

A imagem a seguir mostra como desabilitar o JavaScript no navegador FireFox.

![](adding-validation-to-the-model/_static/image5.png)

A imagem a seguir mostra como desabilitar JavaScript com o navegador Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Abaixo está o *Create.cshtml* exibir modelo Scaffold anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Observe como o código usa um `Html.EditorFor` auxiliar para gerar o `<input>` elemento para cada `Movie` propriedade. Ao lado deste auxiliar é uma chamada para o `Html.ValidationMessageFor` método auxiliar. Esses dois métodos auxiliares trabalham com o objeto de modelo que é passado pelo controlador para o modo de exibição (nesse caso, um `Movie` objeto). Ele procuram automaticamente para atributos de validação especificados nos modelo e exibir mensagens de erro conforme apropriado.

O que é realmente interessante sobre essa abordagem é que o controlador, nem o modelo de exibição criar sabe nada sobre as regras de validação real que está sendo impostas ou as mensagens de erro específico. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`. Essas mesmas regras de validação são aplicadas automaticamente para o modo de exibição de edição e outros modos de exibição modelos você pode criar que editar seu modelo.

Se você quiser alterar a lógica de validação mais tarde, você pode fazer isso em exatamente um local adicionando atributos de validação para o modelo (neste exemplo, a `movie` classe). Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. E significa que você estará totalmente para respeitar o princípio seco.

## <a name="adding-formatting-to-the-movie-model"></a>Adicionando formatação para o modelo de filme

Abra o arquivo *Movie.cs* e examine a classe `Movie`. O [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace fornece os atributos de formatação além do conjunto interno de atributos de validação. Nós já tiver aplicado uma [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeração para a data de lançamento e os campos de preço. O código a seguir mostra o `ReleaseDate` e `Price` propriedades com apropriada [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

O [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributos não são atributos de validação, eles são usados para informar o mecanismo de exibição como renderizar HTML. No exemplo acima, o `DataType.Date` atributo exibe as datas de filme como datas somente, sem tempo. Por exemplo, a seguinte [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributos não validam o formato dos dados:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Os atributos listados acima fornecem apenas dicas para o mecanismo de exibição formatar os dados (e fornecer atributos como &lt;um&gt; da URL e &lt;um href =&quot;mailto:EmailAddress.com&quot; &gt; para email. Você pode usar o [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo para validar o formato dos dados.

Uma abordagem alternativa para usar o `DataType` atributos, você pode definir explicitamente um [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valor. O código a seguir mostra a propriedade data de liberação com uma cadeia de caracteres de formato de data (ou seja, &quot;d&quot;). Você usaria isso para especificar que você não deseja tempo como parte da data de lançamento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Completo `Movie` é mostrada abaixo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Execute o aplicativo e navegue até o `Movies` controlador. A data de lançamento e preço bem são formatados. A imagem abaixo mostra a data de lançamento e preços usando &quot;fr-FR&quot; como a cultura.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

A imagem abaixo mostra os mesmos dados exibidos com a cultura padrão (em inglês-EUA).

![](adding-validation-to-the-model/_static/image8.png)

Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.

>[!div class="step-by-step"]
[Anterior](adding-a-new-field-to-the-movie-model-and-table.md)
[Próximo](examining-the-details-and-delete-methods.md)
