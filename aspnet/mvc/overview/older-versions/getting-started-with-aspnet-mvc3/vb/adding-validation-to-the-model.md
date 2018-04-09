---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Adicionando validação para o modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 86058530aa00ecbc00aeebc6ed7b5cf019fdad72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-to-the-model-vb"></a>Adicionando validação para o modelo (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com VB.NET código-fonte está disponível para acompanhar este tópico. [Baixe a versão VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir c#, alterne para o [versão c#](../cs/adding-validation-to-the-model.md) deste tutorial.


Nesta seção, você adicionará a lógica de validação de `Movie` modelo e você garantirá que as regras de validação são aplicadas a qualquer momento em que um usuário tenta criar ou editar um filme usando o aplicativo.

## <a name="keeping-things-dry"></a>Manter as coisas SECA

Um dos fundamentos de design do ASP.NET MVC é simulação ("não repetitivo"). ASP.NET MVC incentiva você a especificar funcionalidade ou comportamento somente uma vez e, em seguida, ter refletidas em qualquer lugar em um aplicativo. Isso reduz a quantidade de código que você precisa para escrever e faz com que o código que você escreve muito mais fácil para manter.

O suporte de validação fornecido pelo ASP.NET MVC e o Entity Framework Code First é um ótimo exemplo do princípio seco em ação. Você pode especificar declarativamente regras de validação em um lugar (a classe de modelo) e, em seguida, essas regras serão impostas em qualquer lugar no aplicativo.

Vamos ver como você pode tirar proveito desse suporte de validação no aplicativo de filme.

## <a name="adding-validation-rules-to-the-movie-model"></a>Adicionando regras de validação para o modelo de filme

Você começará com a adição de alguma lógica de validação para o `Movie` classe.

Abra o *Movie.vb* arquivo. Adicionar um `Imports` instrução na parte superior do arquivo que faz referência a [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

O namespace é parte do .NET Framework. Ele fornece um conjunto interno de atributos de validação que você pode aplicar declarativamente para qualquer classe ou propriedade.

Atualizar agora o `Movie` classe para aproveitar o interno [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validação . Use o código a seguir como um exemplo de onde aplicar os atributos.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados. O `Required` atributo indica que uma propriedade deve ter um valor; nesse exemplo, um filme deve ter valores para o `Title`, `ReleaseDate`, `Genre`, e `Price` propriedades para serem válidas. O atributo `Range` restringe um valor a um intervalo especificado. O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo.

Código primeiro assegura que as regras de validação que você especificar em uma classe de modelo são aplicadas antes que o aplicativo salva as alterações no banco de dados. Por exemplo, o código a seguir lançará uma exceção quando o `SaveChanges` método é chamado, porque vários necessário `Movie` estiverem faltando valores de propriedade e o preço é zero (que está fora do intervalo válido).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Com as regras de validação automaticamente impostas pelo .NET Framework ajuda a tornar o seu aplicativo mais robusto. Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.

Aqui está um listagem para a atualização de códigos completa *Movie.vb* arquivo:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erro de validação de interface do usuário no ASP.NET MVC

Execute novamente o aplicativo e navegue até o */Movies* URL.

Clique o **criar filme** link para adicionar um novo filme. Preencha o formulário com alguns valores inválidos e, em seguida, clique no **criar** botão.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Observe como o formulário automaticamente tem usado uma cor de plano de fundo para realçar as caixas de texto que contêm dados inválidos e emitida uma mensagem de erro de validação apropriadas ao lado de cada um. As mensagens de erro correspondem as cadeias de caracteres de erro especificado quando você anotado o `Movie` classe. Os erros são impostos no lado do cliente (usando JavaScript) e do lado do servidor (no caso de um usuário tiver JavaScript desabilitado).

Um benefício real é que você não precisa alterar uma única linha de código no `MoviesController` classe ou o *Create.vbhtml* exibição para permitir que essa validação da interface do usuário. O controlador e os modos de exibição que você criou anteriormente neste tutorial automaticamente escolhido até que a validação de regras que você especificou usando atributos no `Movie` classe de modelo.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Como a validação ocorre em criar exibir e cria o método de ação

Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições. A lista seguinte mostra o que o `Create` métodos de `MovieController` a aparência de classe. Eles são inalterados desde como são criados anteriormente neste tutorial.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

O primeiro método de ação exibe o formulário de criação inicial. O segundo manipula a postagem de formulário. A segunda `Create` chamadas de método `ModelState.IsValid` para verificar se o filme tem erros de validação. A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto. Se o objeto tem erros de validação, o `Create` método exibe novamente o formulário. Se não houver erros, o método salvará o novo filme no banco de dados.

Abaixo está o *Create.vbhtml* exibir modelo Scaffold anteriormente no tutorial. Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Observe como o código usa um `Html.EditorFor` auxiliar para gerar o `<input>` elemento para cada `Movie` propriedade. Ao lado deste auxiliar é uma chamada para o `Html.ValidationMessageFor` método auxiliar. Esses dois métodos auxiliares trabalham com o objeto de modelo que é passado pelo controlador para o modo de exibição (nesse caso, um `Movie` objeto). Ele procuram automaticamente para atributos de validação especificados nos modelo e exibir mensagens de erro conforme apropriado.

O que é realmente interessante sobre essa abordagem é que o controlador, nem o modelo de exibição criar sabe nada sobre as regras de validação real que está sendo impostas ou as mensagens de erro específico. As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`.

Se você quiser alterar a lógica de validação mais tarde, você pode fazer isso em exatamente um único local. Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares. Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver. Além disso, isso significa que você respeitará totalmente o princípio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Adicionando formatação para o modelo de filme

Abra o *Movie.vb* arquivo. O [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace fornece os atributos de formatação além do conjunto interno de atributos de validação. Você aplicará a [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo e um [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeração para a data de lançamento e os campos de preço. O código a seguir mostra o `ReleaseDate` e `Price` propriedades com apropriada [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Como alternativa, você pode definir explicitamente um [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valor. O código a seguir mostra a propriedade data de liberação com uma cadeia de caracteres de formato de data (isto é, "d"). Você usaria isso para especificar que você não deseja tempo como parte da data de lançamento.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

O código a seguir formatos de `Price` propriedade como moeda.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Completo `Movie` é mostrada abaixo.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Execute o aplicativo e navegue até o `Movies` controlador.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

A próxima parte da série, vamos examinar o aplicativo e fazer algumas melhorias no gerado automaticamente `Details` e `Delete` métodos.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Próximo](improving-the-details-and-delete-methods.md)
