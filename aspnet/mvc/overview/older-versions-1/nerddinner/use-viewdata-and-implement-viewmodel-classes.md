---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usar ViewData e implementar Classes ViewModel | Microsoft Docs
author: microsoft
description: Etapa 6 mostra como habilitar o suporte para cenários, de edição de formulários mais rico e também aborda duas abordagens que podem ser usadas para passar dados de controladores para exibições:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 8df1ca30f8c0415b68d29eeaa0f7d83a606989ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831641"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Usar ViewData e implementar Classes ViewModel
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 6 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 6 mostra como habilitar o suporte para cenários, de edição de formulários mais rico e também aborda duas abordagens que podem ser usadas para passar dados de controladores para exibições: ViewData e ViewModel.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>Etapa 6 do NerdDinner: ViewData e ViewModel

Nós coberto vários cenários de postagem de formulário e descreveu como implementar criar, atualizar e excluir suporte (CRUD). Agora vamos executar nossa implementação DinnersController adicional e habilitar o suporte para cenários de edição de formulários mais avançados. Ao fazer isso, abordaremos duas abordagens que podem ser usadas para passar dados de controladores para exibições: ViewData e ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Transmitindo dados de controladores para modelos de exibição

Uma das características que definem o padrão MVC é estrita "separação de preocupações" ajuda a impor entre os diferentes componentes de um aplicativo. Modelos, controladores e modos de exibição de cada bem definido funções e responsabilidades e eles se comunicam entre si de maneiras bem definidas. Isso ajuda a promover a possibilidade de teste e a reutilização de código.

Quando uma classe de controlador decide renderizar uma resposta HTML para um cliente, ele é responsável por passar explicitamente para o modelo de exibição de todos os dados necessários para processar a resposta. Modelos de exibição nunca devem executar qualquer lógica de aplicativo ou de recuperação de dados – e em vez disso, devem limitar-se para ter apenas o código de renderização que é controlado por fora do modelo/dados passados para ele pelo controlador.

Agora os dados de modelo que está sendo passado por nossos DinnersController classe para os nossos modelos de exibição é simple e direto – uma lista de objetos de jantar no caso de Index () e um jantar único objeto no caso de Details(), Edit (), Create () e Delete (). Como podemos adicionar mais recursos de interface do usuário para o nosso aplicativo, geralmente vamos precisar passar mais do que apenas esses dados para processar respostas HTML dentro de nossos modelos de exibição. Por exemplo, queremos alterar o campo "País" dentro do nosso editar e criar modos de exibição seja uma caixa de texto HTML para uma dropdownlist. Em vez de embutir a lista suspensa de nomes de países em que o modelo de exibição, queremos gerá-lo em uma lista de países com suporte que podemos preencher dinamicamente. Precisaremos de uma maneira de passar o objeto jantar *e* a lista de países com suporte do nosso controlador para nossos modelos de exibição.

Vejamos duas maneiras de que nós pode fazer isso.

### <a name="using-the-viewdata-dictionary"></a>Usando o dicionário ViewData

A classe de base de controlador expõe uma propriedade de dicionário de "ViewData" que pode ser usada para passar os itens de dados adicionais de controladores para modos de exibição.

Por exemplo, para suportar o cenário em que desejamos alterar a caixa de texto "País" dentro de nosso modo de exibição Editar seja uma caixa de texto HTML para uma dropdownlist, podemos atualizar nosso método de ação Edit () para passar para um objeto de SelectList que pode ser usado como o m (além de um objeto de jantar) odelo de dropdownlist países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

O construtor do SelectList acima está aceitando uma lista de regiões para preencher o drop-downlist com, bem como o valor selecionado no momento.

Em seguida, podemos atualizar nosso modelo de exibição de Edit para usar o método auxiliar Html.DropDownList() em vez do método de auxiliares de Html.TextBox() que usamos anteriormente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

O método auxiliar de Html.DropDownList() acima usa dois parâmetros. A primeira é o nome do elemento de formulário HTML para a saída. O segundo é o modelo de "SelectList" que passamos por meio do dicionário ViewData. Estamos usando o c# "como" palavra-chave para converter o tipo de dicionário como um SelectList.

E agora quando executamos nosso aplicativo e o acesso a */Dinners/Edit/1* URL dentro de nosso navegador, veremos que nossa interface do usuário de edição foi atualizado para exibir uma dropdownlist de países, em vez de uma caixa de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Como podemos também tornar o modelo de exibição de edição do método HTTP POST Editar (em cenários quando ocorrem erros), queremos garantir que também podemos atualizar esse método para adicionar o SelectList ViewData quando o modelo de exibição é renderizado em cenários de erro:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

E agora nosso cenário de edição DinnersController é compatível com uma DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Usando um padrão de ViewModel

A abordagem do dicionário ViewData tem a vantagem de ser razoavelmente rápido e fácil de implementar. Alguns desenvolvedores não gostam de usar dicionários de cadeia de caracteres, no entanto, uma vez que os erros de digitação podem levar a erros que não serão capturados em tempo de compilação. O dicionário ViewData sem tipo também requer usando o operador "como" ou ao usar uma linguagem fortemente tipada, como o c# em um modelo de exibição de conversão.

Uma abordagem alternativa que poderíamos usar é um conhecido como o padrão "ViewModel". Ao usar esse padrão, podemos criar classes fortemente tipadas que são otimizados para nossos cenários de modo de exibição específico, e que expõem propriedades para o valores/conteúdo dinâmico necessários para os nossos modelos de exibição. Nossas classes de controlador podem, em seguida, preencher e passar essas classes de exibição otimizada para nosso modelo de exibição para usar. Isso permite que o tipo de segurança, a verificação de tempo de compilação e intellisense do editor dentro de modelos de exibição.

Por exemplo, para habilitar o formulário de jantar cenários de edição, podemos criar um "DinnerFormViewModel" de classe, como abaixo que expõe duas propriedades fortemente tipado: um objeto de jantar e o modelo de SelectList necessários para preencher a dropdownlist países:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Podemos atualizar nosso método de ação Edit () para criar o DinnerFormViewModel usando o objeto de jantar que recuperamos do nosso repositório e, em seguida, passá-lo para nosso modelo de exibição:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Vamos então nosso modelo de exibição para que ele espera que um "DinnerFormViewModel" em vez de "Jantar" do objeto, alterando o atributo "herda" na parte superior da página Edit de atualização da seguinte forma:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Depois de fazermos isso, o intellisense da propriedade "Modelo" dentro do nosso modelo de exibição será atualizado para refletir o modelo de objeto do tipo DinnerFormViewModel que estamos passando:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Em seguida, podemos atualizar nosso código de exibição funcione dele. Perceba abaixo como não estamos alterando os nomes dos elementos de entrada do que estamos criando (elementos de formulário serão ainda denominados "Title", "País") – mas estamos atualizando os métodos de auxiliares de HTML para recuperar os valores usando a classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Também atualizaremos nosso método de postagem de edição para usar a classe DinnerFormViewModel quando erros de renderização:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Pode também atualizamos nossos métodos de ação Create () para usar novamente exatamente iguais *DinnerFormViewModel* classe para habilitar os países DropDownList nelas também. Abaixo está a implementação de HTTP GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Abaixo está a implementação do método HTTP POST criar:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E agora nossas telas editar e criar suportam suspensas para escolher o país.

### <a name="custom-shaped-viewmodel-classes"></a>Classes de ViewModel em forma personalizada

No cenário acima, nossa classe DinnerFormViewModel expõe diretamente o objeto de modelo de jantar como uma propriedade, junto com uma propriedade de modelo SelectList suporte. Essa abordagem funciona bem para cenários em que a interface do usuário HTML que desejamos criar dentro do nosso modelo de exibição corresponde relativamente perto para nossos objetos de modelo de domínio.

Para cenários em que isso não é o caso, uma opção que você pode usar é criar uma classe ViewModel em forma de personalizar a cujo modelo de objeto é mais otimizado para consumo pelo modo de exibição – e que pode ser completamente diferente do objeto de modelo de domínio subjacente. Por exemplo, ele pode potencialmente expor os nomes de propriedade diferentes e/ou propriedades de agregação coletadas de vários objetos de modelo.

Classes de ViewModel em forma personalizada podem ser usada para passar dados de controladores para modos de exibição para renderizar, bem como para ajudar a lidar com dados de formulário postados volta para o método de ação de um controlador. Para este cenário posterior, você pode ter o método de ação de atualizar um objeto ViewModel com os dados de formulário postado e, em seguida, usar a instância de ViewModel para mapear ou recuperar um objeto de modelo de domínio real.

Classes de ViewModel em forma personalizada podem fornecer uma grande quantidade de flexibilidade e são algo para investigar a qualquer momento você encontrar o código de renderização dentro de seus modelos de exibição ou o código de lançamento de formulário dentro de seus métodos de ação começando a ficar muito complicado. Isso geralmente é um sinal de que os seus modelos de domínio não correspondem corretamente na interface do usuário que você está gerando e que pode ajudar a uma classe intermediária de ViewModel em forma personalizada.

### <a name="next-step"></a>Próxima etapa

Agora vejamos como podemos usar parciais e páginas mestras para reutilizar e compartilhar a interface do usuário em nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Próximo](re-use-ui-using-master-pages-and-partials.md)
