---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Visão geral e arquivo -> Novo projeto | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 1 abrange visão geral e o arquivo -> Novo projeto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a>Parte 1: Visão geral e arquivo -> Novo projeto
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 1 abrange visão geral e o arquivo -&gt;novo projeto.


## <a name="overview"></a>Visão geral

O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Web Developer para desenvolvimento na web. Nós começarão lenta, para que a experiência de desenvolvimento de web de nível para iniciantes é okey.

O aplicativo que é criarei é um repositório de música simples. Há três partes principais para o aplicativo: compras, check-out e administração.

![](mvc-music-store-part-1/_static/image1.jpg)

Os visitantes podem procurar álbuns por gênero:

![](mvc-music-store-part-1/_static/image2.jpg)

Eles podem exibir um único álbum e adicioná-lo ao seu carrinho:

![](mvc-music-store-part-1/_static/image3.jpg)

Eles podem revisar seu carrinho, removendo todos os itens que não desejam mais:

![](mvc-music-store-part-1/_static/image4.jpg)

Continuar o check-out solicitará-los para fazer logon ou registre-se para uma conta de usuário.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Depois de criar uma conta, eles podem concluir a ordem de preencher as informações de pagamento e envio. Para manter as coisas simples, estamos executando uma promoção incrível: tudo é grátis que insira o código de promoção "Livre"!

![](mvc-music-store-part-1/_static/image5.jpg)

Após a ordenação, eles veem uma tela de confirmação simples:

![](mvc-music-store-part-1/_static/image6.jpg)

Além das páginas de faceing de cliente, vamos também criar uma seção de administrador que mostra uma lista de álbuns do qual os administradores podem criar, editar e excluir álbuns:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Arquivo -&gt; novo projeto

### <a name="installing-the-software"></a>Instalar o software

Este tutorial começar criando um novo projeto ASP.NET MVC 3 usando o livre Visual Web Developer 2010 Express (que é gratuito) e, em seguida, vamos incrementalmente adicionar recursos para criar um aplicativo funcional completo. Ao longo do caminho, vamos abordar o acesso de banco de dados, cenários de postagem de formulário, validação de dados, usando páginas mestras para layout de página consistente usando AJAX para atualizações de página e validação, logon de usuário e muito mais.

Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído de [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).

Você pode usar o Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (uma versão gratuita do Visual Studio 2010) para compilar o aplicativo. Usaremos o SQL Server Compact (também gratuito) para hospedar o banco de dados. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.


- [Pré-requisitos do visual Studio Web Developer Express SP1]
- [Atualização de ferramentas do ASP.NET MVC 3]
- [SQL Server Compact 4.0] - incluindo o tempo de execução e ferramentas de suporte


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Criar um novo projeto ASP.NET MVC 3

Vamos começar selecionando "Novo projeto" no menu arquivo no Visual Web Developer. Isso abre a caixa de diálogo Novo projeto.

![](mvc-music-store-part-1/_static/image5.png)

Selecionaremos Visual c# -&gt; modelos da Web grupo à esquerda, em seguida, escolha o modelo de "Aplicativo de Web do ASP.NET MVC 3" no centro da coluna. Nomeie o projeto MvcMusicStore e pressione o botão Okey.

![](mvc-music-store-part-1/_static/image8.jpg)

Isso exibirá uma caixa de diálogo secundária, que permite fazer algumas configurações específicas do MVC para nosso projeto. Selecione o seguinte:

Modelo de projeto - selecione vazio

Mecanismo de exibição - selecione Razor

Usar marcação semântica do HTML5 - marcada

Verifique se as configurações são mostradas conforme abaixo e pressione o botão Okey.

![](mvc-music-store-part-1/_static/image9.jpg)

Isso criará o nosso projeto. Vamos analisar as pastas que foram adicionados ao nosso aplicativo no Gerenciador de soluções, no lado direito.

![](mvc-music-store-part-1/_static/image10.jpg)

O modelo MVC 3 vazio não está totalmente vazio – adiciona uma estrutura de pasta básica:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC usa algumas convenções básicas de nomenclatura para nomes de pasta:

| **Pasta** | **Finalidade** |
| --- | --- |
| **/Controllers** | Controladores de respondem à entrada do navegador, decidir o que fazer com ele e retornar a resposta para o usuário. |
| **/Views** | Exibições de manter os nossos modelos de interface do usuário |
| **/Models** | Modelos de manter e manipulam dados |
| **/Content** | Esta pasta contém nosso imagens, CSS e qualquer outro conteúdo estático |
| **/Scripts** | Esta pasta contém os arquivos do JavaScript |

Essas pastas estão incluídas mesmo em um aplicativo ASP.NET MVC vazio porque a estrutura ASP.NET MVC, por padrão, usa uma abordagem de "convenção sobre a configuração" e faz algumas suposições padrão com base em convenções de nomenclatura de pasta. Por exemplo, controladores procurarem exibições na pasta exibições por padrão sem precisar especificar isso explicitamente em seu código. Adequar-se com as convenções padrão reduz a quantidade de código que você precisa gravar, e também podem tornar mais fácil para outros desenvolvedores entender seu projeto. Vamos explicar essas convenções mais durante a compilação de nosso aplicativo.

> [!div class="step-by-step"]
> [Avançar](mvc-music-store-part-2.md)
