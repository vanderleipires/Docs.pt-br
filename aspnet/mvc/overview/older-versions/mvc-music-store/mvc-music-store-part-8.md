---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Shopping Cart com atualizações Ajax | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 8 abrange o carrinho de compras com atualizações Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871279"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: Carrinho de compras com atualizações Ajax
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 8 abrange o carrinho de compras com atualizações Ajax.


Vai permitir que os usuários a colocar álbuns no carrinho sem registrar, mas será necessário registrar como convidados completa check-out. O processo de compras e check-out será separado em dois controladores: um controlador ShoppingCart que permite anonimamente adicionar itens a um carrinho e um controlador de check-out, que gerencia o processo de check-out. Podemos vai começar com o carrinho de compras nesta seção, em seguida, crie o processo de check-out na seção a seguir.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Adicionando classes de modelo do carrinho, pedido e OrderDetail

Nossos processos de carrinho de compras e check-out fará com que o uso de algumas novas classes. Com o botão direito na pasta modelos e adicione uma classe de carrinho (Cart.cs) com o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Essa classe é muito semelhante a outras pessoas que usamos até agora, com a exceção do atributo [chave] para a propriedade RecordId. Os itens do carrinho terá um identificador de cadeia de caracteres chamado CartID para permitir compras anônima, mas a tabela inclui uma chave primária de inteiro nomeada RecordId. Por convenção, o Entity Framework Code-First espera que a chave primária para uma tabela chamada carrinho será CartId ou ID, mas podemos facilmente substituir que via anotações ou código se quisermos. Este é um exemplo de como podemos usar as convenções simples no Entity Framework Code-First quando eles se adequar nos, mas não está restrito por elas quando eles não.

Em seguida, adicione uma classe de ordem (Order.cs) com o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Esta classe controla informações de resumo e entrega de um pedido. **Ele não será compilado ainda**, pois ele tem uma propriedade de navegação OrderDetails que depende de uma classe que ainda não tenha criado. Vamos corrigir que agora com a adição de uma classe denominada OrderDetail.cs, adicionando o código a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Será feita uma última atualização à nossa classe MusicStoreEntities incluir DbSets que expõem as novas classes de modelo, incluindo também um DbSet&lt;artista&gt;. A classe MusicStoreEntities atualizada aparece como abaixo.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gerenciando a lógica de negócios do carrinho de compras

Em seguida, vamos criar a classe ShoppingCart na pasta de modelos. O modelo ShoppingCart lida com acesso a dados para a tabela de carrinho. Além disso, ele tratará a lógica de negócios para adicionar e remover itens do carrinho de compras.

Como não queremos que exija que os usuários se inscrever para uma conta apenas adicionar itens ao carrinho de compras, será atribuir usuários um identificador exclusivo temporário (usando um GUID ou o identificador global exclusivo) quando acessam o carrinho de compras. Armazenamos essa ID usando a classe de sessão do ASP.NET.

*Observação: A sessão do ASP.NET é um local conveniente para armazenar informações específicas do usuário que irá expirar depois que sair do site. Enquanto o uso indevido do estado da sessão pode ter implicações de desempenho em sites maiores, usamos leve funciona bem para fins de demonstração.*

A classe ShoppingCart expõe os métodos a seguir:

**AddToCart** leva um álbum como um parâmetro e o adiciona ao carrinho do usuário. Desde que a tabela de carrinho controla a quantidade de cada álbum, inclui a lógica para criar uma nova linha, se necessário, ou apenas incrementar a quantidade, se o usuário já tiver solicitado uma cópia do álbum.

**RemoveFromCart** usa uma ID de álbum e a remove do carrinho do usuário. Se o usuário tinha apenas uma cópia do álbum em seu carrinho, a linha será removida.

**EmptyCart** remove todos os itens do carrinho de compras do usuário.

**GetCartItems** recupera uma lista de CartItems para processamento ou exibição.

**GetCount** recupera um número total de álbuns de um usuário tem em seu carrinho de compras.

**GetTotal genérica** calcula o custo total de todos os itens no carrinho.

**CreateOrder** converte o carrinho de compras em uma ordem durante a fase de check-out.

**GetCart** é um método estático que permite que os controladores obter um objeto de carrinho. Ele usa o **GetCartId** método para tratar o CartId de leitura da sessão do usuário. O método GetCartId requer o HttpContextBase para que ele possa ler CartId do usuário da sessão do usuário.

Aqui está o completo **ShoppingCart classe**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Nosso controlador de carrinho de compras precisa se comunicar algumas informações complexas para seus modos de exibição que não mapeiam corretamente para os objetos de modelo. Não queremos modificar os modelos de acordo com os modos de exibição; Classes de modelo devem representar o domínio, não a interface do usuário. Uma solução seria passar as informações para os modos de exibição usando a classe ViewBag, fizemos com as informações de lista suspensa do Gerenciador de armazenamento, mas passar muitas informações via ViewBag obtém difíceis de gerenciar.

Uma solução para isso é usar o *ViewModel* padrão. Ao usar esse padrão, podemos criar classes fortemente tipada que são otimizados para nossos cenários de modo de exibição específico e que expõem propriedades para valores/conteúdo dinâmico necessária para os nossos modelos de exibição. Classes nosso controlador podem popular e passar essas classes otimizado para exibição para nosso modelo de exibição a ser usado. Isso permite que a segurança de tipo, a verificação de tempo de compilação e IntelliSense do editor nos modelos de exibição.

Vamos criar dois modelos de exibição para uso em nosso controlador do carrinho de compras: o ShoppingCartViewModel conterá o conteúdo do carrinho de compras do usuário e o ShoppingCartRemoveViewModel será usado para exibir informações de confirmação quando um usuário remove algo de seu carrinho.

Vamos criar uma nova pasta ViewModels na raiz do projeto para manter a organização. Clique com o botão direito, selecione Adicionar / nova pasta.

![](mvc-music-store-part-8/_static/image1.jpg)

O nome da pasta ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Em seguida, adicione a classe ShoppingCartViewModel na pasta ViewModels. Ele tem duas propriedades: uma lista de itens do carrinho e um valor decimal para manter o preço total para todos os itens no carrinho.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Agora adicione o ShoppingCartRemoveViewModel para a pasta ViewModels, com as seguintes propriedades de quatro.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>O controlador de carrinho de compras

O controlador de carrinho de compras tem três objetivos principais: adicionar itens a um carrinho, removendo itens do carrinho e exibir itens no carrinho de. Ela fará o uso das três classes acabou de criar: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart. Como no StoreController e StoreManagerController, vamos adicionar um campo para manter uma instância do MusicStoreEntities.

Adicione um novo controlador de carrinho de compras para o projeto usando o modelo de controlador vazio.

![](mvc-music-store-part-8/_static/image2.png)

Aqui está o controlador ShoppingCart concluída. As ações de índice e adicionar controlador devem ser bastante familiares. As ações do controlador remover e CartSummary lidar com dois casos especiais, que discutiremos na seção a seguir.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Atualizações de AJAX com jQuery

Em seguida, criaremos uma página de índice de carrinho de compras que terá rigidez de tipos para o ShoppingCartViewModel e usa o modelo de exibição de lista usando o mesmo método como antes.

![](mvc-music-store-part-8/_static/image3.png)

No entanto, em vez de usar um ActionLink para remover os itens do carrinho, vamos usar jQuery "conectar" evento de clique para todos os links neste modo de exibição que têm a classe HTML RemoveLink. Em vez de enviar o formulário, esse manipulador de eventos de clique apenas fará um retorno de chamada AJAX para nossa ação do controlador RemoveFromCart. O RemoveFromCart retorna um resultado JSON serializado, que nosso retorno de chamada do jQuery, em seguida, analisa e executa quatro atualizações rápidas para a página usando jQuery:

- 1. Remove o álbum excluído da lista
- 2. Atualiza a contagem do carrinho no cabeçalho
- 3. Exibe uma mensagem de atualização para o usuário
- 4. Atualiza o preço total do carrinho

Desde que o cenário de remoção está sendo tratado por um retorno de chamada Ajax na exibição de índice, não é necessário um modo de exibição adicional para a ação de RemoveFromCart. Aqui está o código completo para o modo de exibição de /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Para testar isso, precisamos adicionar itens ao nosso carrinho de compras. Vamos atualizar nossos **detalhes do repositório** exibição incluir um botão "Adicionar ao carrinho". Enquanto isso, podemos pode incluir algumas informações adicionais álbum que adicionamos desde que foi atualizado pela última este modo de exibição: gênero, artista, preço e arte. O código de exibição de detalhes do repositório atualizado é exibido conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Agora podemos clique por meio da loja e adicionando e removendo álbuns de nosso carrinho de compras e de teste. Execute o aplicativo e navegue até o índice de repositório.

![](mvc-music-store-part-8/_static/image4.png)

Em seguida, clique em um gênero para ver uma lista de álbuns.

![](mvc-music-store-part-8/_static/image5.png)

Clicando em um título de álbum agora mostra o modo de exibição nosso álbum detalhes atualizado, incluindo o botão "Adicionar ao carrinho".

![](mvc-music-store-part-8/_static/image6.png)

Clicando no botão "Adicionar ao carrinho" mostra a exibição nosso índice de carrinho de compras com a lista de resumo de carrinho compras.

![](mvc-music-store-part-8/_static/image7.png)

Depois de carregar o carrinho de compras, você pode clicar em Remover do link de carrinho para ver a atualização do Ajax ao carrinho de compras.

![](mvc-music-store-part-8/_static/image8.png)

Criamos um trabalho carrinho que permite que os usuários não registrados adicionar itens ao carrinho de compras. Na seção a seguir, será permitir que eles possam registrar e concluir o processo de check-out.


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-7.md)
> [Próximo](mvc-music-store-part-9.md)
