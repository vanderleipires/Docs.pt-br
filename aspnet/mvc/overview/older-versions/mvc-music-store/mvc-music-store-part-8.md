---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrinho com atualizações do Ajax de compras | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 8 aborda o carrinho de compras com atualizações do Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369790"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: Carrinho de compras com atualizações do Ajax
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 8 aborda o carrinho de compras com atualizações do Ajax.


Permitiremos que os usuários a colocar álbuns no carrinho sem registro, mas precisará se registrar como convidados fazer check-out completa. O processo de compras e check-out será separado em dois controladores: um controlador de ShoppingCart que permite anonimamente adicionar itens a um carrinho e um controlador de check-out que manipula o processo de check-out. Podemos vai começar com o carrinho de compras nesta seção, em seguida, crie o processo de check-out na seção a seguir.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Adicionando as classes de modelo carrinho, Order e OrderDetail

Nossos processos de carrinho de compras e check-out serão fazer uso de algumas novas classes. Clique com botão direito na pasta modelos e adicionar uma classe de carrinho (Cart.cs) com o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Essa classe é muito semelhante a outras pessoas que usamos até agora, com o atributo [chave] para a propriedade RecordId a exceção. Os itens do carrinho terá um identificador de cadeia de caracteres chamado CartID para permitir que compras anônimas, mas a tabela inclui uma chave primária de inteiro chamada RecordId. Por convenção, o Entity Framework Code-First espera que a chave primária para uma tabela chamada carrinho será CartId ou ID, mas podemos pode facilmente substituir isso por meio de anotações ou código se quisermos. Este é um exemplo de como podemos usar as convenções simples no Entity Framework Code-First quando eles se adequar nos, mas nós não estão restringidos pelas-los quando não necessitarem.

Em seguida, adicione uma classe de ordem ({1&gt;Order.CS&lt;1) com o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Esta classe controla informações de resumo e a entrega de um pedido. **Ele não será compilado ainda**, pois ele tem uma propriedade de navegação OrderDetails que depende de uma classe que ainda não criamos ainda. Vamos corrigir que agora com a adição de uma classe denominada OrderDetail.cs, adicionando o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Faremos uma última atualização à nossa classe MusicStoreEntities incluir DbSets que expõem essas novas classes de modelo, incluindo também um DbSet&lt;artista&gt;. A classe MusicStoreEntities atualizada é exibido como abaixo.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gerenciando a lógica de negócios do carrinho de compras

Em seguida, vamos criar a classe ShoppingCart na pasta de modelos. O modelo de ShoppingCart lida com acesso a dados na tabela do carrinho. Além disso, ele irá manipular a lógica de negócios para adicionar e remover itens do carrinho de compras.

Uma vez que não queremos exigir que os usuários para se inscrever para uma conta apenas adicionar itens ao carrinho de compras, atribuiremos usuários um identificador exclusivo temporário (usando um GUID ou o identificador global exclusivo) quando acessarem o carrinho de compras. Armazenaremos essa ID usando a classe de sessão do ASP.NET.

*Observação: A sessão do ASP.NET é um local conveniente para armazenar informações específicas do usuário que irá expirar depois que sair do site. Embora o uso indevido do estado da sessão pode ter implicações de desempenho em sites maiores, nosso uso de luz funcionará bem para fins de demonstração.*

A classe ShoppingCart expõe os métodos a seguir:

**AddToCart** leva um álbum como um parâmetro e o adiciona ao carrinho do usuário. Uma vez que a tabela de carrinho controla a quantidade para cada álbum, inclui a lógica para criar uma nova linha, se necessário, ou apenas incrementar a quantidade, se o usuário já tiver solicitada para uma cópia do álbum.

**RemoveFromCart** usa uma ID de álbum e a remove do carrinho do usuário. Se o usuário tiver apenas uma cópia do álbum em seu carrinho, a linha é removida.

**EmptyCart** remove todos os itens do carrinho de compras do usuário.

**GetCartItems** recupera uma lista de CartItems para exibição ou processamento.

**GetCount** recupera um número total de álbuns de um usuário tem em seu carrinho de compras.

**GetTotal genérica** calcula o custo total de todos os itens no carrinho.

**CreateOrder** converte o carrinho de compras em uma ordem durante a fase de check-out.

**GetCart** é um método estático que permite que os controladores obter um objeto de carrinho. Ele usa o **GetCartId** método para manipular o CartId lendo da sessão do usuário. O método GetCartId requer o HttpContextBase para que ele possa ler CartId do usuário da sessão do usuário.

Aqui está o completo **ShoppingCart classe**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Nosso controlador de carrinho de compras será preciso comunicar-se algumas informações complexas para seus modos de exibição que não é mapeado corretamente para nossos objetos de modelo. Não queremos modificar nossos modelos para atender às nossas opiniões; Classes de modelo devem representar o nosso domínio, não a interface do usuário. Uma solução seria passar as informações para nossos modos de exibição usando a classe ViewBag, como fizemos com as informações de lista suspensa do Store Manager, mas transmitir muitas informações por meio de ViewBag obtém difícil de gerenciar.

Uma solução para isso é usar o *ViewModel* padrão. Ao usar esse padrão, podemos criar classes fortemente tipadas que são otimizados para nossos cenários de modo de exibição específico, e que expõem propriedades para o valores/conteúdo dinâmico necessários para os nossos modelos de exibição. Nossas classes de controlador podem, em seguida, preencher e passar essas classes de exibição otimizada para nosso modelo de exibição para usar. Isso permite que o tipo de segurança, a verificação de tempo de compilação e IntelliSense do editor dentro de modelos de exibição.

Vamos criar dois modelos de exibição para uso no nosso controlador de carrinho de compras: o ShoppingCartViewModel conterá o conteúdo do carrinho de compras do usuário, e o ShoppingCartRemoveViewModel será usado para exibir informações de confirmação quando um usuário remove algo do seu carrinho.

Vamos criar uma nova pasta ViewModels na raiz do nosso projeto para manter as coisas organizadas. Clique com botão direito no projeto, selecione Adicionar / nova pasta.

![](mvc-music-store-part-8/_static/image1.jpg)

Nomeie a pasta ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Em seguida, adicione a classe ShoppingCartViewModel na pasta ViewModels. Ele tem duas propriedades: uma lista de itens do carrinho e um valor decimal para manter o preço total para todos os itens no carrinho.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Agora, adicione o ShoppingCartRemoveViewModel à pasta ViewModels, com as quatro propriedades a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>O controlador de carrinho de compras

O controlador de carrinho de compras tem três objetivos principais: adicionar itens a um carrinho, removendo itens do carrinho e exibir itens no carrinho de. Ele fará o uso das três classes, podemos acabou de criar: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart. Como no StoreController e StoreManagerController, vamos adicionar um campo para armazenar uma instância de MusicStoreEntities.

Adicione um novo controlador de carrinho de compras para o projeto usando o modelo de controlador vazia.

![](mvc-music-store-part-8/_static/image2.png)

Aqui está o controlador ShoppingCart completa. As ações de índice e adicionar controlador devem ser bastante familiares. As ações do controlador remover e CartSummary manipular dois casos especiais, que discutiremos na seção a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Atualizações do AJAX com o jQuery

Em seguida, criaremos uma página de índice de carrinho de compras que é fortemente tipada para o ShoppingCartViewModel e usa o modelo de exibição de lista usando o mesmo método que antes.

![](mvc-music-store-part-8/_static/image3.png)

No entanto, em vez de usar um HTML. ActionLink para remover itens do carrinho, vamos usar jQuery "conectar" para todos os links neste modo de exibição que têm a classe HTML RemoveLink o evento de clique. Em vez de lançar o formulário, esse manipulador de eventos click apenas fará um retorno de chamada do AJAX para nossa ação de controlador RemoveFromCart. O RemoveFromCart retorna um resultado serializado JSON, que nosso retorno de chamada do jQuery, em seguida, analisa e executa quatro atualizações rápidas para a página usando o jQuery:

- 1. Remove o álbum excluído da lista
- 2. Atualiza a contagem de carrinho no cabeçalho
- 3. Exibe uma mensagem de atualização para o usuário
- 4. Atualiza o preço total do carrinho

Uma vez que o cenário de remoção é manipulado por um retorno de chamada Ajax dentro da exibição de índice, não precisamos um modo de exibição adicional para a ação de RemoveFromCart. Aqui está o código completo para o modo de exibição /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Para testar isso, precisamos ser capazes de adicionar itens ao nosso carrinho de compras. Vamos atualizar nossos **Store detalhes** modo de exibição para incluir um botão "Adicionar ao carrinho". Enquanto estamos fazendo isso, podemos incluir algumas informações adicionais álbum que adicionamos, pois foi atualizado pela última vez nesta exibição: arte do álbum, artista, preço e gênero. O código de modo de exibição de detalhes de Store atualizado é exibido conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Agora podemos clique por meio da loja e adicionando e removendo álbuns para e do nosso carrinho de compras de teste. Execute o aplicativo e navegue até o índice de Store.

![](mvc-music-store-part-8/_static/image4.png)

Em seguida, clique em um gênero para exibir uma lista dos álbuns.

![](mvc-music-store-part-8/_static/image5.png)

Clicar em um título de álbum agora mostra nossa exibição de detalhes do álbum atualizada, incluindo o botão "Adicionar ao carrinho".

![](mvc-music-store-part-8/_static/image6.png)

Clicar no botão "Adicionar ao carrinho" mostra nossa exibição índice de carrinho de compras com a lista de resumo de carrinho compras.

![](mvc-music-store-part-8/_static/image7.png)

Após o carregamento de carrinho de compras, você pode clicar na remover do link de carrinho para ver a atualização do Ajax ao carrinho de compras.

![](mvc-music-store-part-8/_static/image8.png)

Criamos um carrinho que permite que os usuários não registrados adicionar itens ao carrinho de compras em funcionamento. Na seção a seguir, permitiremos que eles possam registrar e concluir o processo de check-out.


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-7.md)
> [Próximo](mvc-music-store-part-9.md)
