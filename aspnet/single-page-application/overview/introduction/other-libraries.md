---
uid: single-page-application/overview/introduction/other-libraries
title: Saber a uma biblioteca que não seja Knockout? | Microsoft Docs
author: madskristensen
description: Saber a uma biblioteca que não seja Knockout?
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 6ac260e88fd156bad4b414e93325d5a04c490c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872579"
---
<a name="know-a-library-other-than-knockout"></a>Saber a uma biblioteca que não seja Knockout?
====================
por [Mads Kristensen](https://github.com/madskristensen)

O [modelo de aplicativo de página única (SPA)](knockoutjs-template.md) é uma ótima maneira de começar a escrever aplicativos de única página. O modelo usa [KnockoutJS](http://knockoutjs.com/) para associar dados do aplicativo para elementos DOM.

Mas Knockout não é a biblioteca JavaScript somente para a criação de aplicativos de cliente avançado. Outras bibliotecas resolver desafios semelhantes de maneiras diferentes. Talvez você prefira uma biblioteca de um em detrimento de outro, para que fizemos vários modelos criados pela comunidade disponíveis para download. Cada um desses modelos usa uma combinação diferente de bibliotecas do cliente JavaScript.

Para instalar um modelo criados pela comunidade, visite o modelo páginas listados abaixo em clique no botão de Download. Os modelos são fornecidos como arquivos VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modelo do SPA backbone](../templates/backbonejs-template.md). Este modelo fornece um esqueleto inicial para o desenvolvimento de um [backbone](http://backbonejs.org/) aplicativo ASP.NET MVC. Pronto para uso, ele fornece funcionalidade de logon do usuário básica, incluindo redefinição de senha de inscrição, entrar, de usuário e a confirmação do usuário com modelos de email básico.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) é uma biblioteca de software livre para o gerenciamento de dados sofisticada em um cliente JavaScript. Muito fácil manipula consultando, cache, o controle de alterações, validação e muito mais. Dois modelos de recursos muito fácil:

- O [muito fácil/Knockout](../templates/breezeknockout-template.md) modelo estende o modelo Knockout SPA, mostrando como você pode criar facilmente um aplicativo de página única com muito fácil para gerenciamento de dados e KnockoutJS para associação de dados.
- O [muito fácil/Angular](../templates/breezeangular-template.md) modelo também estende o modelo de separação SPA com muito fácil, mas usando o [AngularJS](http://angularjs.org) biblioteca para associação de dados, injeção de dependência e gerenciamento de tela.

Além disso, o [modelo Hot SPA de toalhas](../templates/hottowel-template.md) usa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modelo do SPA EmberJS](../templates/emberjs-template.md). Este modelo usa [Ember](http://emberjs.com/), uma biblioteca JavaScript do MVC poderosa que resolve uma ampla gama de desafios para a criação de aplicativos cliente avançados.

O modelo Ember SPA é uma implementação nova do modelo Knockout SPA, usando EmberJS e guidões de modelagem.

## <a name="hot-towel"></a>Toalhas ativa

[Modelo de toalhas SPA hot](../templates/hottowel-template.md). Este modelo coloca em várias bibliotecas JavaScript, incluindo muito fácil, Knockout, RequireJS e inicialização do Twitter.

Em comparação com os outros modelos listados aqui, o Hot toalhas teample fornece um aplicativo mais completo do qual você pode criar seus próprios. Há mais conceitos a serem consideradas, mas depois de entendê-los, esse modelo pode ser o que você está procurando. Se você deseja criar um SPA mas não pode decidir onde iniciar, use um toalhas ativa e em segundos você terá um SPA e todas as ferramentas que você precisa construir sobre ele.

## <a name="feature-table"></a>Tabela de recursos

Aqui estão os recursos fornecidos por cada modelo SPA:


|                        | ASP.NET SPA | Estrutura | Breeze/Angular | Muito fácil/KO |  Ember   | Toalhas ativa |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Exemplo de tarefas       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Sem modelo      |             | &#10003; |                |           |          | &#10003;  |
| Navegação e histórico |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotecas        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Muito fácil         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

