---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Visão geral e arquivo -> Novo projeto | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 1 abrange visão geral e arquivo -> Novo projeto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833699"
---
<a name="part-1-overview-and-file-new-project"></a>Parte 1: Visão geral e arquivo -> Novo projeto
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 1 aborda a visão geral e o arquivo -&gt;novo projeto.


## <a name="overview"></a>Visão geral

A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e Visual Web Developer para desenvolvimento da web. Podemos vai iniciar lentamente, portanto, a experiência de desenvolvimento para a web de nível iniciante é okey.

O aplicativo, vou desenvolver é uma loja de música simples. Há três partes principais para o aplicativo: compras, check-out e administração.

![](mvc-music-store-part-1/_static/image1.jpg)

Os visitantes podem procurar álbuns por gênero:

![](mvc-music-store-part-1/_static/image2.jpg)

Eles podem exibir um único álbum e adicioná-lo ao seu carrinho:

![](mvc-music-store-part-1/_static/image3.jpg)

Eles podem revisar seu carrinho, removendo todos os itens que não deseja mais querem:

![](mvc-music-store-part-1/_static/image4.jpg)

Continuar a fazer check-out lhe pedirá para fazer logon ou registre-se para uma conta de usuário.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Depois de criar uma conta, eles podem concluir o pedido, preenchendo as informações de envio e de pagamento. Para manter as coisas simples, estamos executando uma promoção incrível: tudo o que é gratuito se ele inserir o código de promoção "Gratuito"!

![](mvc-music-store-part-1/_static/image5.jpg)

Após a ordenação, eles veem uma tela de confirmação simples:

![](mvc-music-store-part-1/_static/image6.jpg)

Além das páginas do cliente faceing, também uma seção de administrador que mostra uma lista dos álbuns do qual os administradores podem criar, editar, criar e excluir álbuns:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Arquivo -&gt; novo projeto

### <a name="installing-the-software"></a>A instalação do software

Este tutorial começar criando um novo projeto ASP.NET MVC 3 usando o gratuito Visual Web Developer 2010 Express (que é gratuito) e, em seguida, vamos incrementalmente adicionar recursos para criar um aplicativo funcional completo. Ao longo do caminho, vamos abordar o acesso de banco de dados, cenários de postagem de formulário, validação de dados, usando as páginas mestras para layout de página consistente, usando o AJAX para atualizações de página e validação, logon de usuário e muito mais.

Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído no [MVC de música de Store](https://github.com/evilDave/MVC-Music-Store).

Você pode usar o Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (uma versão gratuita do Visual Studio 2010) para compilar o aplicativo. Usaremos o SQL Server Compact (também gratuito) para hospedar o banco de dados. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.


- [Pré-requisitos do visual Studio Web Developer Express SP1]
- [Atualização de ferramentas do ASP.NET MVC 3]
- [SQL Server Compact 4.0] - incluindo o suporte de tempo de execução e ferramentas


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Criar um novo projeto ASP.NET MVC 3

Vamos começar selecionando "Novo projeto" no menu arquivo no Visual Web Developer. Isso abre a caixa de diálogo Novo projeto.

![](mvc-music-store-part-1/_static/image5.png)

Vamos selecionar Visual c# -&gt; modelos da Web de grupo à esquerda, em seguida, escolha o modelo "Aplicativo de Web do ASP.NET MVC 3" na coluna central. Nomeie o projeto MvcMusicStore e pressione o botão Okey.

![](mvc-music-store-part-1/_static/image8.jpg)

Isso exibirá uma caixa de diálogo secundária, que nos permite fazer algumas configurações específicas do MVC para nosso projeto. Selecione o seguinte:

Modelo de projeto - selecione vazio

Mecanismo de exibição – selecionar Razor

Use marcação semântica HTML5 - check

Verifique se que suas configurações são mostradas conforme abaixo e, em seguida, pressione o botão Okey.

![](mvc-music-store-part-1/_static/image9.jpg)

Isso criará o nosso projeto. Vamos dar uma olhada em pastas que foram adicionados ao nosso aplicativo no Gerenciador de soluções, no lado direito.

![](mvc-music-store-part-1/_static/image10.jpg)

O modelo MVC 3 vazio não está completamente vazio – ele adiciona uma estrutura de pasta básica:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC faz uso de algumas convenções básicas de nomenclatura para nomes de pastas:

| **Pasta** | **Finalidade** |
| --- | --- |
| **/ Controladores** | Controladores de respondem à entrada do navegador, decidir o que fazer com ele e retornar a resposta para o usuário. |
| **/ Modos de exibição** | Modos de exibição manter nossos modelos de interface do usuário |
| **Ou os modelos** | Modelos de mantenham e manipulam dados |
| **/Content** | Essa pasta contém nossas imagens, CSS e qualquer outro conteúdo estático |
| **/ Scripts** | Essa pasta contém nossos arquivos de JavaScript |

Essas pastas são incluídas, mesmo em um aplicativo ASP.NET MVC vazio porque a estrutura ASP.NET MVC, por padrão, usa uma abordagem de "convention over configuration" e faz algumas suposições padrão com base nas convenções de nomenclatura de pasta. Por exemplo, controladores procurarem exibições na pasta modos de exibição por padrão sem a necessidade de especificar isso explicitamente em seu código. Adequar-se com as convenções padrão reduz a quantidade de código que você precisa para escrever, e pode também torna mais fácil para outros desenvolvedores a entender o seu projeto. Vamos explicar essas convenções mais durante a compilação de nosso aplicativo.

> [!div class="step-by-step"]
> [Avançar](mvc-music-store-part-2.md)
