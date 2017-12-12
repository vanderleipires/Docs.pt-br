---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Use ViewData e implementar ViewModel Classes | Microsoft Docs
author: microsoft
description: "Etapa 6 mostra como habilita o suporte para edição cenários, de forma mais avançada e também descreve dois métodos que podem ser usados para passar dados de controladores para modos de exibição:..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Use ViewData e implementar ViewModel Classes
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 6 do livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 6 mostra como habilita o suporte para edição cenários, de forma mais avançada e também descreve dois métodos que podem ser usados para passar dados de controladores para modos de exibição: ViewData e ViewModel.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner etapa 6: ViewData e ViewModel

Nós coberto vários cenários de postagem de formulário e, discutido a implementação de criar, atualizar e excluir suporte (CRUD). Agora vamos tomar nossa implementação DinnersController novas e habilitar o suporte para o formulário mais rico editando cenários. Ao fazer isso, discutiremos duas abordagens que podem ser usadas para passar dados de controladores para modos de exibição: ViewData e ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passando dados controladores para exibir modelos

Uma das características de definição do padrão MVC é estrita "separação de preocupações" ajudam a impor entre os diferentes componentes de um aplicativo. Modelos, controladores e exibições cada bem definido de funções e responsabilidades e se comunicam entre si de maneiras bem definidas. Isso ajuda a promover a capacidade de teste e reutilização de código.

Quando uma classe de controlador decide renderizar uma resposta HTML para um cliente, ele é responsável por explicitamente passando para o modelo de exibição de todos os dados necessários para processar a resposta. Exibir modelos nunca devem executar qualquer lógica de aplicativo ou de recuperação de dados – e em vez disso, devem limitar-se para que apenas o código de processamento que é controlado de modelo/dados passados a ele pelo controlador.

Agora os dados do modelo que está sendo passado pelo nosso DinnersController classe para os nossos modelos de exibição é simple e direta – uma lista de objetos de uma refeição no caso de Index () e uma refeição uma único objeto no caso de Details(), Edit(), Create () e Delete (). Como podemos adicionar mais recursos de interface do usuário para nosso aplicativo, geralmente vamos precisar passar mais do que apenas esses dados para processar respostas HTML em nossos modelos de exibição. Por exemplo, podemos talvez queira alterar o campo de "País" em nosso editar e criar exibições de ser uma caixa de texto HTML para dropdownlist. Em vez de embutir em código lista suspensa de nomes de país no modelo de exibição, desejamos gerá-lo em uma lista de países com suporte é popular dinamicamente. Precisaremos de uma maneira de passar o objeto de uma refeição *e* a lista de países com suporte do nosso controlador para os nossos modelos de exibição.

Vamos examinar o que pode fazer isso de duas maneiras.

### <a name="using-the-viewdata-dictionary"></a>Usando o dicionário ViewData

A classe base do controlador expõe uma propriedade de dicionário "ViewData" que pode ser usada para passar os itens de dados adicionais de controladores para modos de exibição.

Por exemplo, para dar suporte a situação onde desejamos passam a caixa de texto de "País" em nosso modo de exibição de edição de uma caixa de texto HTML para dropdownlist, podemos atualizar nosso método de ação Edit() para passar para um objeto de SelectList que pode ser usado como o m (além de um objeto de uma refeição) odelo de dropdownlist países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

O construtor do SelectList acima está aceitando uma lista de regiões para preencher o drop-downlist com, bem como o valor selecionado no momento.

Em seguida, podemos atualizar nosso modelo de exibição Edit.aspx para usar o método auxiliar Html.DropDownList() em vez do método de auxiliar Html.TextBox() que usamos anteriormente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

O método auxiliar de Html.DropDownList() acima usa dois parâmetros. A primeira é o nome do elemento de formulário HTML para a saída. O segundo é o modelo de "SelectList" são passados por meio do dicionário ViewData. Estamos usando o c# "como" palavra-chave para converter o tipo de dentro do dicionário como um SelectList.

E agora quando executamos nosso aplicativo e o acesso a */Dinners/Edit/1* URL em nosso navegador, veremos que nossa interface do usuário de edição foi atualizada para exibir dropdownlist de países, em vez de uma caixa de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Como podemos processar o modelo de exibição de edição do método HTTP POST Editar (em cenários quando ocorrem erros), podemos desejará Certifique-se de que podemos também atualizar este método para adicionar o SelectList ViewData quando o modelo de exibição é renderizado em cenários de erro:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

E agora oferece suporte a nosso cenário de edição DinnersController DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Usando um padrão de ViewModel

A abordagem de dicionário ViewData tem a vantagem de ser bastante rápida e fácil de implementar. Alguns desenvolvedores não gostam usando dicionários baseada em cadeia de caracteres, no entanto, como erros de digitação podem levar a erros que não serão detectados em tempo de compilação. O dicionário ViewData sem tipo também requer usando o operador "como" ou a conversão ao usar uma linguagem fortemente tipada como c# em um modelo de exibição.

Uma abordagem alternativa que podemos usar é uma conhecida como o padrão "ViewModel". Ao usar esse padrão, podemos criar classes fortemente tipada que são otimizados para nossos cenários de modo de exibição específico e que expõem propriedades para valores/conteúdo dinâmico necessária para os nossos modelos de exibição. Classes nosso controlador podem popular e passar essas classes otimizado para exibição para nosso modelo de exibição a ser usado. Isso permite que a segurança de tipo, a verificação de tempo de compilação e intellisense do editor nos modelos de exibição.

Por exemplo, para habilitar o formulário refeição cenários edição, podemos criar um "DinnerFormViewModel" classe como abaixo que expõe duas propriedades fortemente tipado: um objeto de uma refeição e o modelo de SelectList necessários para preencher dropdownlist países:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Podemos atualizar nosso método de ação Edit() para criar o DinnerFormViewModel usando o objeto de uma refeição que recuperamos de nosso repositório e, em seguida, passá-lo ao nosso modelo de exibição:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Vamos enviar uma mensagem e a atualização do objeto alterando o atributo "inherits" na parte superior da página edit.aspx nosso modelo de exibição de forma que ele espera um "DinnerFormViewModel" em vez de "Refeição" da seguinte forma:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Depois que fizer isso, o intellisense da propriedade "Modelo" em nosso modelo de exibição será atualizado para refletir o modelo de objeto do tipo DinnerFormViewModel, ele passa:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Em seguida, podemos atualizar nosso código do modo de exibição para trabalham com ele. Observe abaixo como não estamos alterando os nomes dos elementos de entrada, estamos criando (os elementos de formulário serão ainda denominados "Title", "País") – mas estamos atualizando os métodos auxiliares HTML para recuperar os valores usando a classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Também vamos atualizar nossos método post de edição para usar a classe DinnerFormViewModel quando erros de renderização:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Podemos também atualizar nossos métodos de ação Create () para usar novamente o exata mesmo *DinnerFormViewModel* classe para habilitar os países DropDownList em esses também. Abaixo está a implementação de HTTP GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Abaixo está a implementação do método HTTP POST criar:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E agora nossas telas editar e criar suportam suspensas para escolher o país.

### <a name="custom-shaped-viewmodel-classes"></a>Classes de ViewModel em formato personalizado

No cenário acima, nossa classe DinnerFormViewModel expõe diretamente o objeto de modelo de uma refeição como uma propriedade, junto com uma propriedade de modelo SelectList suporte. Essa abordagem funciona bem para cenários em que a interface do usuário HTML quer criar em nosso modelo de exibição corresponde relativamente atentamente a nossos objetos de modelo de domínio.

Para cenários em que isso não é o caso, uma opção que você pode usar é criar uma classe de ViewModel em formato personalizado cujo modelo de objeto mais é otimizado para consumo pelo modo de exibição – e que pode ser completamente diferente do objeto de modelo de domínio subjacente. Por exemplo, ela pode expor potencialmente nomes de propriedade diferentes e/ou propriedades de agregação coletadas de vários objetos de modelo.

Classes de ViewModel em forma personalizada podem ser usada para passar dados de controladores para modos de exibição para renderizar, bem como para ajudar a manipular os dados do formulário enviados de volta para o método de ação do controlador. Para este cenário posteriormente, você pode ter o método de ação atualizar um objeto ViewModel com os dados de formulário postado e, em seguida, usar a instância de ViewModel para mapear ou recuperar um objeto de modelo de domínio real.

Classes de ViewModel em forma personalizada podem fornecer uma grande quantidade de flexibilidade e são algo para investigar a qualquer momento você encontrar o código de renderização em seus modelos de exibição ou o código de postagem de formulário dentro de métodos de ação começando a ficar muito complicado. Isso geralmente é um sinal de que seus modelos de domínio corretamente não correspondem à interface do usuário que você está gerando e que pode ajudar a uma classe intermediária de ViewModel em formato personalizado.

### <a name="next-step"></a>Próxima etapa

Agora, vamos ver como podemos pode usar existe meio-termo e páginas mestras para reutilizar e compartilhar a interface do usuário em nosso aplicativo.

>[!div class="step-by-step"]
[Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Próximo](re-use-ui-using-master-pages-and-partials.md)
