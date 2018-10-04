---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Adicionar validação ao modelo | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 5819d789f31b9452d40ae3aa7f821f101ae126ce
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578309"
---
<a name="adding-validation-to-the-model"></a>Adicionar validação ao modelo
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.


Nesta seção, você adicionará lógica de validação para o `Movie` modelo e você vai garantir que as regras de validação são impostas sempre que um usuário tenta criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Manter o processo DRY

Um dos princípios de design de núcleo do ASP.NET MVC é DRY (&quot;não Repeat Yourself&quot;). ASP.NET MVC incentiva você a especificar a funcionalidade ou comportamento somente uma vez e, em seguida, refleti-lo em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa escrever e torna o código escrito seja menos propenso a erro e mais fácil de manter.

O suporte de validação fornecido pelo ASP.NET MVC e ao Entity Framework Code First é um ótimo exemplo do princípio DRY em ação. Você pode especificar regras de validação declarativamente em um único lugar (na classe de modelo) e as regras são impostas em qualquer lugar no aplicativo.

Vamos dar uma olhada em como você pode tirar proveito desse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação ao modelo de filme

Você começará adicionando alguma lógica de validação para o `Movie` classe.

Abra o arquivo *Movie.cs*. Adicionar um `using` instrução na parte superior do arquivo que faz referência a [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Observe que o namespace não contém `System.Web`. DataAnnotations fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente a qualquer classe ou propriedade.

Atualize agora o `Movie` classe podem aproveitar o interno [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validação . Use o código a seguir como um exemplo de como aplicar os atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Execute o aplicativo e você obterá o seguinte erro de tempo de execução:

***O modelo fazendo o contexto de 'MovieDBContext' foi alterado desde que o banco de dados foi criado. Considere o uso de migrações do Code First para atualizar o banco de dados ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Usaremos as migrações para atualizar o esquema. Compile a solução e, em seguida, abra o **Package Manager Console** janela e digite os seguintes comandos:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Quando esse comando for concluído, o Visual Studio abre o arquivo de classe que define o novo `DbMIgration` classe derivada com o nome especificado (*AddDataAnnotationsMig*) e, no `Up` método, você pode ver o código que atualiza as restrições de esquema. O `Title` e `Genre` campos não são mais que permitem valor nulos (ou seja, você deve inserir um valor) e o `Rating` campo tem um comprimento máximo de 5.

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. O `Required` atributo indica que uma propriedade deve ter um valor; neste exemplo, um filme deve ter valores para o `Title`, `ReleaseDate`, `Genre`, e `Price` propriedades para que seja válido. O atributo `Range` restringe um valor a um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo. Tipos intrínsecos (como `decimal, int, float, DateTime`) são necessários por padrão e não é necessário o `Required` atributo.

Código primeiro garante que as regras de validação que você especificar em uma classe de modelo são aplicadas antes que o aplicativo salva as alterações no banco de dados. Por exemplo, o código a seguir lançará uma exceção quando o `SaveChanges` método é chamado, porque vários necessário `Movie` estiverem faltando valores de propriedade e o preço é zero (que está fora do intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Ter as regras de validação automaticamente impostas pelo .NET Framework o ajuda a tornar seu aplicativo de mais robusta. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

Aqui está um listagem para a atualização de código completa *Movie.cs* arquivo:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erro de validação de interface do usuário no ASP.NET MVC

Executar o aplicativo novamente e navegue até a */Movies* URL.

Clique o **criar novo** link para adicionar um novo filme. Preencha o formulário com alguns valores inválidos e, em seguida, clique no **criar** botão.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> para dar suporte a validação do jQuery para idiomas diferentes do inglês que usam uma vírgula (&quot;,&quot;) para um ponto decimal, você deve incluir *globalize.js* seu específicas e *cultures/globalize.cultures.js* arquivo (do [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. O código a seguir mostra as modificações no arquivo Views\Movies\Edit.cshtml para trabalhar com o &quot;fr-FR&quot; cultura:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe como o formulário tem usado automaticamente uma cor de borda vermelha para realçar as caixas de texto que contêm dados inválidos e emitida uma mensagem de erro de validação apropriado ao lado de cada um deles. Os erros são impostos no lado do cliente (usando o JavaScript e o jQuery) e no lado do servidor (caso um usuário tenha o JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código na `MoviesController` classe ou nos *Create. cshtml* exibição para permitir essa validação da interface do usuário. O controlador e as exibições criados anteriormente neste tutorial selecionaram automaticamente as regras de validação especificadas com atributos de validação nas propriedades da classe de modelo `Movie`.

Você deve ter notado para as propriedades `Title` e `Genre`, o atributo required não é imposto até que você envia o formulário (pressione a **criar** botão), ou digite o texto no campo de entrada e removê-lo. Para um campo que é inicialmente vazio (por exemplo, os campos no modo de exibição criar) e que tem apenas o atributo obrigatório e não há outros atributos de validação, você pode fazer o seguinte para disparar a validação:

1. Guia para o campo.
2. Digite algum texto.
3. Saída da guia.
4. Guia volta para o campo.
5. Remova o texto.
6. Saída da guia.

A sequência acima vai disparar a validação necessária sem pressionar o botão Enviar. Basta pressionar o botão de envio sem inserir qualquer um dos campos irá disparar a validação do lado do cliente. Os dados de formulário não são enviados no servidor até que não haja erros de validação do lado do cliente. Você pode testar isso colocando um ponto de interrupção no método HTTP Post ou usando o [ferramenta fiddler](http://fiddler2.com/fiddler2/) ou o IE 9 [ferramentas de desenvolvedor F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre a criar exibe e cria o método de ação

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A listagem de Avançar mostra o que o `Create` métodos no `MovieController` a aparência de classe. Eles são modificados em relação a como são criados neste tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

O primeiro método de ação (HTTP GET) `Create` exibe o formulário Criar inicial. A segunda versão (`[HttpPost]`) manipula a postagem de formulário. O segundo método `Create` (a versão `HttpPost`) chama `ModelState.IsValid` para verificar se o filme tem erros de validação. A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto. Se o objeto tiver erros de validação, o método `Create` exibirá o formulário novamente. Se não houver erros, o método salvará o novo filme no banco de dados. Em nosso exemplo de filme, estamos usando, **o formulário não é postado no servidor quando há erros de validação detectados no lado do cliente; a segunda** `Create` **método nunca é chamado**. Se você desabilitar o JavaScript no navegador, a validação do cliente está desabilitada e o HTTP POST `Create` chamadas de método `ModelState.IsValid` para verificar se o filme tem erros de validação.

Defina um ponto de interrupção no método `HttpPost Create` e verifique se o método nunca é chamado; a validação do lado do cliente não enviará os dados de formulário quando forem detectados erros de validação. Se você desabilitar o JavaScript no navegador e, em seguida, enviar o formulário com erros, o ponto de interrupção será atingido. Você ainda pode obter uma validação completa sem o JavaScript. A imagem a seguir mostra como desabilitar o JavaScript no Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

A imagem a seguir mostra como desabilitar o JavaScript no navegador FireFox.

![](adding-validation-to-the-model/_static/image5.png)

A imagem a seguir mostra como desabilitar o JavaScript com o navegador Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Abaixo está o *Create. cshtml* modelo de exibição gerada por scaffolding anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Observe como o código usa um `Html.EditorFor` auxiliar para a saída de `<input>` elemento para cada `Movie` propriedade. Ao lado deste auxiliar é uma chamada para o `Html.ValidationMessageFor` método auxiliar. Esses dois métodos auxiliares trabalham com o objeto de modelo que é passado pelo controlador para o modo de exibição (nesse caso, um `Movie` objeto). Eles parecem automaticamente para atributos de validação especificados em que as mensagens de erro de modelo e exibição conforme apropriado.

O que é realmente interessante nessa abordagem é que nem o controlador nem o modelo de exibição criar sabe nada sobre as regras de validação real que está sendo impostas ou sobre mensagens de erro específicas exibidas. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`. Essas mesmas regras de validação são aplicadas automaticamente para a exibição de edição e outros modelos de exibição você pode criar que editam o modelo.

Se você quiser alterar a lógica de validação mais tarde, você pode fazer isso em vigor exatamente uma adicionando atributos de validação para o modelo (neste exemplo, o `movie` classe). Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. Além disso, isso significa que você respeitará totalmente o princípio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Adicionando formatação ao modelo de filme

Abra o arquivo *Movie.cs* e examine a classe `Movie`. O [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace fornece os atributos de formatação além do conjunto interno de atributos de validação. Já aplicamos uma [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeração para a data de lançamento e para os campos de preço. O código a seguir mostra a `ReleaseDate` e `Price` propriedades com os devidos [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

O [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributos não são atributos de validação, eles são usados para informar o mecanismo de exibição como renderizar o HTML. No exemplo acima, o `DataType.Date` atributo exibe as datas de filme como datas somente, sem tempo. Por exemplo, a seguinte [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributos não validam o formato dos dados:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Os atributos listados acima fornecem apenas dicas para o mecanismo de exibição formate os dados (e fornece atributos como &lt;uma&gt; para as URLs e &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; para o email. Você pode usar o [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo para validar o formato dos dados.

Uma abordagem alternativa para usar o `DataType` atributos, você pode definir explicitamente uma [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valor. O código a seguir mostra a propriedade data de lançamento com uma cadeia de caracteres de formato de data (ou seja, &quot;1!d&quot;). Você o usaria para especificar que você não deseja tempo como parte da data de lançamento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Completo `Movie` classe é mostrado abaixo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Execute o aplicativo e navegue até o `Movies` controlador. O preço e data de lançamento são bem formatadas. A imagem abaixo mostra a data de lançamento e usando de preço &quot;fr-FR&quot; como a cultura.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

A imagem abaixo mostra os mesmos dados exibidos com a cultura padrão (em inglês-EUA).

![](adding-validation-to-the-model/_static/image8.png)

Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field-to-the-movie-model-and-table.md)
> [Próximo](examining-the-details-and-delete-methods.md)
