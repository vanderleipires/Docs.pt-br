---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Atualizações Final para navegação e Design de Site, conclusão | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 10 abrange atualizações Final para navegação e S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: Atualizações Final para navegação e Design de Site, conclusão
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 10 abrange atualizações Final para navegação e Design de Site, a conclusão.


Concluímos toda a funcionalidade principal para o nosso site, mas ainda temos alguns recursos para adicionar a navegação do site, a página inicial e a página Procurar armazenamento.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Criando a exibição parcial resumo de carrinho compras

Queremos expor o número de itens no carrinho de compras do usuário em todo o site.

![](mvc-music-store-part-10/_static/image1.png)

Podemos facilmente implementar isso criando uma exibição parcial que é adicionada ao nosso Site.master.

Como mostrado anteriormente, o controlador ShoppingCart inclui um método de ação de CartSummary que retorna uma exibição parcial:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Para criar a exibição parcial CartSummary, com o botão direito na pasta exibições/ShoppingCart e selecione Adicionar modo de exibição. Nomeie a exibição CartSummary e marque a caixa de seleção "Criar uma exibição parcial", conforme mostrado abaixo.

![](mvc-music-store-part-10/_static/image2.png)

A exibição parcial CartSummary é realmente simple – é apenas um link para a exibição do índice ShoppingCart que mostra o número de itens no carrinho. O código completo para CartSummary.cshtml é o seguinte:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Podemos pode incluir uma exibição parcial em qualquer página no site, incluindo o mestre de Site, usando o método Html.RenderAction. RenderAction exige especificar o nome da ação ("CartSummary") e o nome do controlador ("ShoppingCart") como abaixo.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Antes de adicionar isso ao site de Layout, vamos também criar o Menu de gênero para que possamos fazer todos os nossos Site.master atualizações ao mesmo tempo.

## <a name="creating-the-genre-menu-partial-view"></a>Criando a exibição parcial do Menu de gênero

Podemos pode fazer muito mais fácil para os usuários navegar por meio da loja, adicionando um Menu de gênero que lista todos os gêneros disponíveis na loja.

![](mvc-music-store-part-10/_static/image3.png)

A seguir, o mesmo etapas também criam uma exibição parcial GenreMenu e, em seguida, podemos adicionar-os com o mestre de Site. Primeiro, adicione a seguinte ação de controlador GenreMenu para o StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Essa ação retorna uma lista de gêneros que será exibido quando a exibição parcial, vamos criar em seguida.

*Observação: Adicionamos o atributo [ChildActionOnly] para esta ação de controlador, o que indica que desejamos somente essa ação a ser usado em uma exibição parcial. Esse atributo impedirá que a ação de controlador que está sendo executado, navegando para /Store/GenreMenu. Isso não é necessário para exibições parciais, mas é uma boa prática, desde que você deseja certificar-se de que nossas ações do controlador são usadas como pretendemos. Estamos também retornando PartialView em vez de exibição, que permite que o mecanismo de exibição sabe que ele não deve usar o Layout para este modo de exibição, pois ele está sendo incluído em outros modos de exibição.*

Clique na ação do controlador GenreMenu e criar uma exibição parcial chamada GenreMenu que é fortemente tipado usando a classe de dados de exibição de gênero, conforme mostrado abaixo.

![](mvc-music-store-part-10/_static/image4.png)

Atualize o código de exibição para a exibição parcial GenreMenu exibir os itens usando uma lista não ordenada da seguinte maneira.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Atualizando o Layout do Site para exibir nossa exibições parciais

Podemos adicionar nossas exibições parciais para o Layout do Site (/exibições/compartilhado/\_cshtml) chamando Html.RenderAction(). Vamos adicioná-los no, bem como algumas marcações adicionais para exibi-los, conforme mostrado abaixo:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Agora quando o aplicativo é executado, veremos gênero na área de navegação à esquerda e o resumo do carrinho na parte superior.

## <a name="update-to-the-store-browse-page"></a>Atualizar para a página Procurar repositório

A página Procurar armazenamento está funcionando, mas não parece muito bom. Podemos atualizar a página para mostrar os álbuns em um layout melhor ao atualizar o código de exibição (encontrado no /Views/Store/Browse.cshtml) da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Aqui estamos fazendo uso de URL em vez de ActionLink para que podemos aplicar formatação especial para o link para incluir a arte do álbum.

*Observação: É estiver exibindo uma capa de álbum genérica para esses álbuns. Essas informações são armazenadas no banco de dados e pode ser editadas através do Gerenciador de armazenamento. Você está bem-vindo ao adicionar sua própria arte final.*

Agora quando podemos procurar um gênero, veremos os álbuns mostrados em uma grade com a arte do álbum.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Atualizando a página inicial para mostrar superior álbuns de venda

Queremos que nossa vendido álbuns na home page de aumentar as vendas de recursos. Faremos algumas atualizações nosso HomeController para lidar com isso e adicione alguns elementos de gráficos adicionais.

Primeiro, adicionaremos uma propriedade de navegação para nossa classe álbum para que EntityFramework sabe que estão associados. As últimas linhas do nosso **álbum** classe agora deve se parecer com:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Observação: Será necessário adicionar um usando a instrução para trazer o namespace de Generic.*

Primeiro, vamos adicionar um campo storeDB e o MvcMusicStore.Models usando as instruções, como em nossos outros controladores. Em seguida, adicionaremos o seguinte método ao qual consulta nosso banco de dados para localizar os principais álbuns de vendas de acordo com a OrderDetails HomeController.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Isso é um método privado, pois não queremos disponibilizá-lo como uma ação do controlador. Incluímos-lo no HomeController para manter a simplicidade, mas você é incentivado a mover sua lógica de negócios em classes de serviço separado conforme apropriado.

Com isso em vigor, podemos atualizar a ação de controlador de índice para consultar os 5 principais álbuns e retorná-los para o modo de exibição.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

O código completo para o HomeController atualizado é conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Por fim, precisaremos atualizar nossa visualização de índice de início para que ele pode exibir uma lista de álbuns atualizando o tipo de modelo e adicionar a lista de álbum na parte inferior. Nós o conduziremos essa oportunidade para também pode adicionar um título e uma seção de promoção para a página.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Agora quando o aplicativo é executado, veremos nossa página atualizada com principais álbuns de vendas e nossa mensagem promocional.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusão

Já vimos que o ASP.NET MVC torna mais fácil para criar um site sofisticado com acesso de banco de dados, associação, AJAX, etc. muito rapidamente. Esperamos que este tutorial lhe forneceu as ferramentas que você precisa para começar a criar seu próprio ASP.NET MVC aplicativos!


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-9.md)
