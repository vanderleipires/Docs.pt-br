---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e Menu de categoria | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 3 aborda a adição de layout e um menu de categoria.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 72642610c203127b431e03214b2f1bc85a85b5fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366641"
---
<a name="part-3-layout-and-category-menu"></a>Parte 3: Layout e Menu de categoria
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 3 aborda a adição de layout e um menu de categoria.


## <a id="_Toc260221669"></a>  Adicionando alguns Layout e um Menu de categoria

Em nossa página mestre do site, adicionaremos um div para a coluna do lado esquerdo que conterá nosso menu de categoria de produto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Observe que o alinhamento desejado e outra formatação serão fornecidos pela classe CSS que adicionamos ao nosso arquivo Style CSS.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

O menu de categoria de produto será ser criado dinamicamente em tempo de execução consultando o banco de dados do Commerce para categorias de produto e a criação dos itens de menu e existentes correspondente vincula.

Para fazer isso, que usaremos dois do ASP. Controles de dados avançada do NET. O controle de "Entidade de fonte de dados" e o controle de "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Vamos alternar para o "Modo de Design" e usar os auxiliares para configurar os controles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Vamos definir a propriedade ID EntityDataSource como EDS\_categoria\_Menu e clique em "Configurar a fonte de dados".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Selecione a Conexão CommerceEntities que foi criado para nós quando criamos o modelo de fonte de dados de entidade para nosso banco de dados de comércio e clique em "Avançar".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Selecione o nome do conjunto de entidade "Categorias" e deixe o restante das opções como padrão. Clique em "Concluir".

Agora vamos definir a propriedade de ID da instância do controle ListView que colocamos em nossa página de ListView\_ProductsMenu e ativar seu auxiliar.

![](tailspin-spyworks-part-3/_static/image5.jpg)

No entanto, poderíamos usar opções de controle para formatar a exibição de item de dados e formatação, a criação de nosso menu necessitarão apenas de marcação simples para que inserimos o código na exibição da fonte.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Observe que a instrução "Eval": &lt;# Eval("CategoryName") %&gt;

A sintaxe ASP.NET &lt;# %&gt; é uma convenção de taquigrafia que instrui o tempo de execução para executar tudo o que está contido e gerar os resultados "na linha".

A instrução Eval("CategoryName") instrui que, para a entrada atual na coleção associada de itens de dados, busca o valor dos nomes de item de modelo de entidade "CatagoryName". Isso é uma sintaxe concisa para um recurso muito poderoso.

Vamos executar o aplicativo agora.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Observe que nosso menu de categoria de produto agora é exibido e quando focalizamos em um dos itens de menu categoria, podemos ver os pontos de link de item de menu a uma página é ainda preciso implementar chamado ProductsList.aspx e que criamos um argumento de cadeia de caracteres de consulta dinâmica que contém o  id da categoria.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-2.md)
> [Próximo](tailspin-spyworks-part-4.md)
