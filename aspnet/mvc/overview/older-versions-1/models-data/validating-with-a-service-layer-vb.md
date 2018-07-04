---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Validação com uma camada de serviço (VB) | Microsoft Docs
author: StephenWalther
description: Aprenda a mover sua lógica de validação fora de suas ações do controlador e uma camada de serviço separado. Neste tutorial, Stephen Walther explica como você...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: a914a7351e0faf6babf144d80512994d513ed12f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393219"
---
<a name="validating-with-a-service-layer-vb"></a>Validação com uma camada de serviço (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Aprenda a mover sua lógica de validação fora de suas ações do controlador e uma camada de serviço separado. Neste tutorial, Stephen Walther explica como você pode manter uma nítida separação de preocupações, isolando sua camada de serviço de camada do seu controlador.


O objetivo deste tutorial é descrever um método de execução de validação em um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como mover sua lógica de validação para fora de seus controladores de e para uma camada de serviço separado.

## <a name="separating-concerns"></a>A separação de preocupações

Quando você compila um aplicativo ASP.NET MVC, você não deve colocar sua lógica de banco de dados dentro de suas ações de controlador. Misturar sua lógica de banco de dados e o controlador torna o seu aplicativo mais difícil manter ao longo do tempo. A recomendação é que você coloque toda sua lógica de banco de dados em uma camada de repositório separado.

Por exemplo, a listagem 1 contém um repositório simples chamado o ProductRepository. O repositório do produto contém todo o código de acesso de dados para o aplicativo. A listagem também inclui a interface IProductRepository que implementa o repositório de produto.

**Listagem 1 - Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

O controlador na listagem 2 usa a camada de repositório em ações de seu Index () e Create (). Observe que esse controlador não contêm nenhuma lógica de banco de dados. Criando uma camada de repositório permite que você mantenha uma separação limpa de preocupações. Controladores são responsáveis por lógica de controle de fluxo de aplicativo e o repositório é responsável pela lógica de acesso a dados.

**Listagem 2 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Criando uma camada de serviço

Assim, a lógica de controle de fluxo do aplicativo pertence a um controlador e lógica de acesso a dados pertence a um repositório. Nesse caso, onde colocar sua lógica de validação? Uma opção é colocar a lógica de validação em uma *camada de serviço*.

Uma camada de serviço é uma camada adicional em um aplicativo ASP.NET MVC que atua como mediador de comunicação entre um controlador e a camada de repositório. A camada de serviço contém a lógica de negócios. Em particular, ele contém a lógica de validação.

Por exemplo, a camada de serviço do produto na listagem 3 tem um método CreateProduct(). O método CreateProduct() chama o método de ValidateProduct() para validar um novo produto antes de passar o produto para o repositório de produto.

**Listagem 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

O controlador de produto foi atualizado na listagem 4 para usar a camada de serviço em vez de camada de repositório. A camada do controlador se comunica com a camada de serviço. A camada de serviço se comunica com a camada de repositório. Cada camada tem uma responsabilidade separada.

**Listagem 4 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Observe que o serviço de produto é criado no construtor do controlador de produto. Quando o serviço de produto é criado, o dicionário de estado de modelo é passado para o serviço. O serviço de produto usa estado de modelo para passar mensagens de erro de validação volta para o controlador.

## <a name="decoupling-the-service-layer"></a>Desacoplar a camada de serviço

Podemos ter falhado ao isolar o controlador e as camadas de serviço em um aspecto. O controlador e as camadas de serviço se comunicam por meio do estado do modelo. Em outras palavras, a camada de serviço tem uma dependência em um recurso específico da estrutura MVC do ASP.NET.

Gostaríamos de isolar a camada de serviço da nossa camada de controlador tanto quanto possível. Em teoria, poderemos usar a camada de serviço com qualquer tipo de aplicativo e não apenas um aplicativo ASP.NET MVC. Por exemplo, no futuro, queremos criar um front-end para nosso aplicativo do WPF. Podemos deve encontrar uma maneira de remover a dependência no ASP.NET MVC estado do modelo da nossa camada de serviço.

Na listagem 5, a camada de serviço foi atualizada para que ele não use o estado do modelo. Em vez disso, ele usa qualquer classe que implementa a interface IValidationDictionary.

**Listagem 5 - Models\ProductService.vb (separados)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

A interface IValidationDictionary é definida na listagem 6. Essa interface simple tem um único método e uma única propriedade.

**Listagem 6 - Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

A classe na listagem 7, nomeada da classe ModelStateWrapper, implementa a interface IValidationDictionary. Você pode instanciar a classe ModelStateWrapper, passando um dicionário de estado de modelo para o construtor.

**Listagem 7 - Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Por fim, o controlador atualizado na listagem 8 usa o ModelStateWrapper ao criar a camada de serviço em seu construtor.

**Listagem 8 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

Usando o IValidationDictionary interface e a classe ModelStateWrapper nos permite isolar completamente nossa camada de serviço de camada nosso controlador. A camada de serviço não é dependente de estado do modelo. Você pode passar qualquer classe que implementa a interface IValidationDictionary para a camada de serviço. Por exemplo, um aplicativo WPF pode implementar a interface IValidationDictionary com uma classe de coleção simples.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era discutir uma abordagem para executar a validação em um aplicativo ASP.NET MVC. Neste tutorial, você aprendeu como mover todos os de sua lógica de validação para fora de seus controladores de e para uma camada de serviço separado. Você também aprendeu como isolar sua camada de serviço de camada do seu controlador, criando uma classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Anterior](validating-with-the-idataerrorinfo-interface-vb.md)
> [Próximo](validation-with-the-data-annotation-validators-vb.md)
