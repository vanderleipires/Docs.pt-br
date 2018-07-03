---
uid: single-page-application/overview/introduction/other-libraries
title: Conhece uma biblioteca diferente do Knockout? | Microsoft Docs
author: madskristensen
description: Conhece uma biblioteca diferente do Knockout?
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 0424d209cbd24756d1a840788bb3dc5b48d905ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375234"
---
<a name="know-a-library-other-than-knockout"></a>Conhece uma biblioteca diferente do Knockout?
====================
por [Mads Kristensen](https://github.com/madskristensen)

O [modelo de aplicativo de página única (SPA)](knockoutjs-template.md) é uma ótima maneira de começar a escrever aplicativos de única página. O modelo usa [KnockoutJS](http://knockoutjs.com/) para associar dados de aplicativo para elementos DOM.

Mas o Knockout não é a biblioteca JavaScript somente para a criação de aplicativos de cliente avançado. Outras bibliotecas solucionar desafios semelhantes de maneiras diferentes. Talvez você prefira uma biblioteca de um em detrimento de outro, portanto, fizemos vários modelos criados pela comunidade disponíveis para download. Cada um desses modelos usa uma combinação diferente de bibliotecas de JavaScript do cliente.

Para instalar um modelo criados pela comunidade, visite um do modelo de páginas listados abaixo em clique no botão baixar. Os modelos são fornecidos como arquivos VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modelo SPA backbone. js](../templates/backbonejs-template.md). Esse modelo fornece um esqueleto inicial para o desenvolvimento de um [backbone. js](http://backbonejs.org/) aplicativo no ASP.NET MVC. Fora da caixa, ele fornece funcionalidade de logon de usuário básicos, incluindo redefinição de senha de inscrição, entrar, usuário e a confirmação do usuário com modelos de email básico.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) é uma biblioteca de software livre para gerenciamento de dados avançados em um cliente JavaScript. Breeze manipula consultando, cache, o controle de alterações, validação e muito mais. Dois modelos de recursos muito simples:

- O [Breeze/Knockout](../templates/breezeknockout-template.md) modelo estende o modelo de SPA do Knockout, mostrando como você pode criar facilmente um aplicativo de página única com a Breeze para KnockoutJS e gerenciamento de dados para vinculação de dados.
- O [Breeze/Angular](../templates/breezeangular-template.md) modelo também estende o modelo de SPA Knockout com a Breeze, mas usando o [AngularJS](http://angularjs.org) biblioteca para gerenciamento de tela, injeção de dependência e vinculação de dados.

Além disso, o [modelo de SPA de Hot Towel](../templates/hottowel-template.md) usa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modelo EmberJS SPA](../templates/emberjs-template.md). Este modelo usa [Ember](http://emberjs.com/), uma poderosa biblioteca JavaScript de MVC que resolve uma vasta gama de desafios para a criação de aplicativos cliente avançados.

O modelo de SPA Ember é uma nova implementação do modelo SPA do Knockout, usando modelagem EmberJS e Handlebars.

## <a name="hot-towel"></a>Hot Towel

[Modelo de hot Towel SPA](../templates/hottowel-template.md). Esse modelo traz várias bibliotecas JavaScript, incluindo a Breeze, o Knockout, o RequireJS e o Twitter Bootstrap.

Em comparação com os outros modelos listados aqui, o Hot Towel teample fornece um aplicativo mais completo do qual você pode criar seus próprios. Há mais conceitos a serem consideradas, mas depois de compreendê-las, esse modelo pode ser o que você está procurando. Se você deseja criar um SPA, mas não é possível decidir onde iniciar, use o Hot Towel e, em segundos você terá um SPA e todas as ferramentas que você precisará construir sobre ele.

## <a name="feature-table"></a>Tabela de recursos

Aqui estão os recursos fornecidos por cada modelo SPA:


|                        | ASP.NET SPA | Backbone | Breeze/Angular | Breeze/KO |  Ember   | Hot Towel |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Exemplo de tarefas pendentes       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Modelo vazio      |             | &#10003; |                |           |          | &#10003;  |
| Navegação e histórico |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotecas        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

