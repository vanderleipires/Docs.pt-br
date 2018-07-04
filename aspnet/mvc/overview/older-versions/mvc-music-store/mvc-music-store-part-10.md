---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Atualizações Final para navegação e Design de Site, conclusão | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 10 aborda atualizações finais à navegação e S....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: a5b1d02d268530d6288ed2bc502679336d06d403
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366314"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: Atualizações Final para navegação e Design de Site, conclusão
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 10 abrange atualizações finais à navegação e Design de Site, conclusão.


Concluímos toda a funcionalidade principal para o nosso site, mas ainda temos alguns recursos para adicionar a navegação do site, a home page e a página Procurar Store.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Criando a exibição parcial resumo de carrinho compras

Queremos expor o número de itens no carrinho de compras do usuário em todo o site.

![](mvc-music-store-part-10/_static/image1.png)

Podemos facilmente implementar isso criando uma exibição parcial que é adicionada ao nosso site.

Conforme mostrado anteriormente, o controlador ShoppingCart inclui um método de ação de CartSummary que retorna uma exibição parcial:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Para criar a exibição parcial de CartSummary, clique com botão direito na pasta exibições/ShoppingCart e selecione Adicionar modo de exibição. Nomeie a exibição CartSummary e marque a caixa de seleção "Criar uma exibição parcial", conforme mostrado abaixo.

![](mvc-music-store-part-10/_static/image2.png)

O modo de exibição parcial CartSummary é muito simple - é apenas um link para o modo de exibição de índice ShoppingCart que mostra o número de itens no carrinho. O código completo para CartSummary.cshtml é da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Podemos incluir uma exibição parcial em qualquer página no site, incluindo o mestre do Site, usando o método Html.RenderAction. RenderAction exige especificar o nome da ação ("CartSummary") e o nome do controlador ("ShoppingCart") como abaixo.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Antes de adicionar isso para o site de Layout, podemos também criará o Menu de gênero para que possamos fazer todas as nossas atualizações do site ao mesmo tempo.

## <a name="creating-the-genre-menu-partial-view"></a>Criando a exibição parcial do Menu de gênero

Fazemos isso muito mais fácil para nossos usuários navegar por meio da loja, adicionando um Menu de gênero que lista todos os gêneros disponíveis em nosso repositório.

![](mvc-music-store-part-10/_static/image3.png)

Vamos seguir o mesmo etapas também criam uma exibição parcial de GenreMenu e, em seguida, podemos adicionar ambos ao mestre do Site. Primeiro, adicione a seguinte ação do controlador GenreMenu para o StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Essa ação retorna uma lista de gêneros que serão exibidos pela exibição parcial, que criaremos em seguida.

*Observação: Adicionamos o atributo [ChildActionOnly] para esta ação de controlador, que indica que apenas queremos que essa ação a ser usado em uma exibição parcial. Esse atributo impedirá que a ação do controlador que está sendo executado, navegando até /Store/GenreMenu. Isso não é necessário para exibições parciais, mas é uma boa prática, uma vez que queremos nos assegurar-se de que nossas ações do controlador são usadas como pretendemos. Estamos retornando também PartialView em vez de exibição, que permite que o mecanismo de exibição sabe que ele não deve usar o Layout para esta exibição, pois ele está sendo incluído em outros modos de exibição.*

Clique duas vezes em que a ação do controlador GenreMenu e criar uma exibição parcial chamada GenreMenu que é fortemente tipado usando a classe de dados de exibição do gênero, conforme mostrado abaixo.

![](mvc-music-store-part-10/_static/image4.png)

Atualize o código de exibição para o modo de exibição parcial GenreMenu exibir os itens usando uma lista não ordenada da seguinte maneira.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Atualizando o Layout do Site para exibir nosso exibições parciais

Podemos adicionar nossa exibições parciais para o Layout do Site (/exibições/Shared/\_layout. cshtml) chamando Html.RenderAction(). Vamos adicioná-los no, bem como algumas marcações adicionais para exibi-los, conforme mostrado abaixo:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Agora quando executamos o aplicativo, veremos o gênero na área de navegação à esquerda e o resumo de carrinho na parte superior.

## <a name="update-to-the-store-browse-page"></a>Atualizar para a página Procurar Store

A página Procurar Store está funcionando, mas não parece muito bom. Podemos atualizar a página para mostrar os álbuns em um layout melhor atualizando o código de exibição (encontrado no /Views/Store/Browse.cshtml) da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Aqui estamos fazendo uso de Action em vez de HTML. ActionLink para que podemos aplicar formatação especial para o link para incluir a arte do álbum.

*Observação: Estamos estiver exibindo uma capa do álbum genérico para esses álbuns. Essas informações são armazenadas no banco de dados e pode ser editadas por meio do Gerenciador de Store. Você está bem-vindo ao adicionar sua própria arte final.*

Agora quando podemos navegar para um gênero, veremos os álbuns mostrados em uma grade com a arte do álbum.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Atualizando a Home Page para mostrar superior álbuns de venda

Queremos que nossos mais vendidos álbuns na home page para aumentar as vendas de recursos. Faremos algumas atualizações para nossa HomeController para lidar com isso e adicionar alguns elementos de gráficos adicionais.

Primeiro, vamos adicionar uma propriedade de navegação à nossa classe de álbum para que o EntityFramework sabe que estão associadas. As últimas linhas do nosso **álbum** classe agora deve ser assim:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Observação: Será necessário adicionar um usando a instrução para colocar no namespace Collections.*

Primeiro, vamos adicionar um campo de storeDB e o MvcMusicStore.Models usando instruções, como em nossos outros controladores. Em seguida, adicionaremos o seguinte método para o HomeController que consulta o nosso banco de dados para localizar as principais álbuns de vendas de acordo com a OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Esse é um método privado, já que não queremos disponibilizá-lo como uma ação do controlador. Incluímos-lo no HomeController para manter a simplicidade, mas você é incentivado a mover sua lógica de negócios em classes de serviço separado conforme apropriado.

Com isso funcionando, podemos atualizar a ação do controlador de índice para consultar os 5 principais álbuns e retorná-los para o modo de exibição.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

O código completo para o HomeController atualizado é conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Por fim, precisaremos atualizar exibição nosso índice de início para que ele possa exibir uma lista dos álbuns atualizando o tipo de modelo e adicionar a lista de álbum na parte inferior. Faremos essa oportunidade para também adicionar um título e uma seção de promoção para a página.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Agora quando executamos o aplicativo, veremos nosso home page atualizada com principais álbuns de vendas e nossa mensagem promocional.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusão

Já vimos que ASP.NET MVC torna mais fácil para criar um site da Web sofisticado com acesso de banco de dados, associação, AJAX, etc. muito rapidamente. Espero que este tutorial tenha fornecido as ferramentas que você precisa para começar a criação de aplicativos ASP.NET MVC!


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-9.md)
