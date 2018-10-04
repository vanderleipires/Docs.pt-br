---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Adicionar validação ao modelo (c#) | Microsoft Docs
author: Rick-Anderson
description: Criando um controlador
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: dd126ca070b81285f902e164f9e5f82397974096
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578049"
---
<a name="adding-validation-to-the-model-c"></a>Adicionar validação ao modelo (c#)
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.
> 
> 
> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.


Nesta seção, você adicionará lógica de validação para o `Movie` modelo e você vai garantir que as regras de validação são impostas sempre que um usuário tenta criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Manter o processo DRY

Um dos princípios de design de núcleo do ASP.NET MVC é DRY ("não Repeat Yourself"). ASP.NET MVC incentiva você a especificar a funcionalidade ou comportamento somente uma vez e, em seguida, refleti-lo em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa escrever e torna o código que você escreve muito mais fácil de manter.

O suporte de validação fornecido pelo ASP.NET MVC e ao Entity Framework Code First é um ótimo exemplo do princípio DRY em ação. Você pode especificar regras de validação declarativamente em um único lugar (na classe de modelo) e, em seguida, essas regras são impostas em qualquer lugar no aplicativo.

Vamos dar uma olhada em como você pode tirar proveito desse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação ao modelo de filme

Você começará adicionando alguma lógica de validação para o `Movie` classe.

Abra o arquivo *Movie.cs*. Adicionar um `using` instrução na parte superior do arquivo que faz referência a [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

O namespace é parte do .NET Framework. Ele fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente a qualquer classe ou propriedade.

Atualize agora o `Movie` classe podem aproveitar o interno [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validação . Use o código a seguir como um exemplo de como aplicar os atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. O `Required` atributo indica que uma propriedade deve ter um valor; neste exemplo, um filme deve ter valores para o `Title`, `ReleaseDate`, `Genre`, e `Price` propriedades para que seja válido. O atributo `Range` restringe um valor a um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo.

Código primeiro garante que as regras de validação que você especificar em uma classe de modelo são aplicadas antes que o aplicativo salva as alterações no banco de dados. Por exemplo, o código a seguir lançará uma exceção quando o `SaveChanges` método é chamado, porque vários necessário `Movie` estiverem faltando valores de propriedade e o preço é zero (que está fora do intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Ter as regras de validação automaticamente impostas pelo .NET Framework o ajuda a tornar seu aplicativo de mais robusta. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

Aqui está um listagem para a atualização de código completa *Movie.cs* arquivo:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erro de validação de interface do usuário no ASP.NET MVC

Executar o aplicativo novamente e navegue até a */Movies* URL.

Clique o **filme criar** link para adicionar um novo filme. Preencha o formulário com alguns valores inválidos e, em seguida, clique no **criar** botão.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Observe como o formulário tem usado automaticamente uma cor de fundo para realçar as caixas de texto que contêm dados inválidos e emitida uma mensagem de erro de validação apropriado ao lado de cada um deles. As mensagens de erro corresponderem as cadeias de caracteres de erro que você especificou quando você anotado o `Movie` classe. Os erros são impostos no lado do cliente (usando JavaScript) e o lado do servidor (no caso de um usuário tem o JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código na `MoviesController` classe ou nos *Create. cshtml* exibição para permitir essa validação da interface do usuário. O controlador e exibições criadas automaticamente neste tutorial selecionaram as regras de validação que você especificou usando atributos no `Movie` classe de modelo.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre a criar exibe e cria o método de ação

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A listagem de Avançar mostra o que o `Create` métodos no `MovieController` a aparência de classe. Eles são modificados em relação a como são criados neste tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

O primeiro método de ação exibe o formulário Criar inicial. O segundo manipula a postagem de formulário. A segunda `Create` chamadas de método `ModelState.IsValid` para verificar se o filme tem erros de validação. A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto. Se o objeto tiver erros de validação, o `Create` método exibe novamente o formulário. Se não houver erros, o método salvará o novo filme no banco de dados.

Abaixo está o *Create. cshtml* modelo de exibição gerada por scaffolding anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe como o código usa um `Html.EditorFor` auxiliar para a saída de `<input>` elemento para cada `Movie` propriedade. Ao lado deste auxiliar é uma chamada para o `Html.ValidationMessageFor` método auxiliar. Esses dois métodos auxiliares trabalham com o objeto de modelo que é passado pelo controlador para o modo de exibição (nesse caso, um `Movie` objeto). Eles parecem automaticamente para atributos de validação especificados em que as mensagens de erro de modelo e exibição conforme apropriado.

O que é realmente interessante nessa abordagem é que nem o controlador nem o modelo de exibição criar sabe nada sobre as regras de validação real que está sendo impostas ou sobre mensagens de erro específicas exibidas. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`.

Se você quiser alterar a lógica de validação mais tarde, você pode fazer isso em exatamente um só lugar. Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. Além disso, isso significa que você respeitará totalmente o princípio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Adicionando formatação ao modelo de filme

Abra o arquivo *Movie.cs*. O [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace fornece os atributos de formatação além do conjunto interno de atributos de validação. Você aplicará a [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo e uma [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeração para a data de lançamento e para os campos de preço. O código a seguir mostra a `ReleaseDate` e `Price` propriedades com os devidos [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Como alternativa, você pode definir explicitamente uma [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valor. O código a seguir mostra a propriedade data de lançamento com uma cadeia de caracteres de formato de data (ou seja, "d"). Você o usaria para especificar que você não deseja tempo como parte da data de lançamento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

O código a seguir formatos o `Price` a propriedade como moeda.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Completo `Movie` classe é mostrado abaixo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Execute o aplicativo e navegue até o `Movies` controlador.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Próximo](improving-the-details-and-delete-methods.md)
