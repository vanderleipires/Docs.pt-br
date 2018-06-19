---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e Menu categoria | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 3 aborda a adição de layout e um menu de categoria.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882082"
---
<a name="part-3-layout-and-category-menu"></a>Parte 3: Layout e Menu de categoria
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 3 aborda a adição de layout e um menu de categoria.


## <a id="_Toc260221669"></a>  Adicionando alguns Layout e um Menu de categoria

Em nossa página mestra vamos adicionar um div para a coluna do lado esquerdo que conterá o nosso menu de categoria de produto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Observe que o alinhamento desejado e outros tipos de formatação serão fornecidos pela classe CSS que foi adicionada ao nosso arquivo Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

O menu de categoria de produto será dinamicamente criado em tempo de execução consultando o banco de dados de comércio de links existentes categorias de produtos e criar os itens de menu e correspondente.

Para fazer isso, que vamos usar dois ASP. Controles de dados avançados do NET. O controle de "Entidade de fonte de dados" e o "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Vamos alternar para o "Modo de Design" e use os auxiliares para configurar os controles.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Vamos definir a propriedade ID EntityDataSource EDS\_categoria\_Menu e clique em "Configurar a fonte de dados".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Selecione a Conexão CommerceEntities que foi criado para nós quando criamos o modelo de fonte de dados de entidade para nosso banco de dados de comércio e clique em "Avançar".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Selecione o nome do conjunto de entidade "Categorias" e deixe o resto das opções como padrão. Clique em "Concluir".

Agora vamos definir a propriedade de ID da instância do controle ListView são colocados em nossa página de ListView\_ProductsMenu e ativar seu auxiliar.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Embora podemos usar opções de controle para formatar a exibição de item de dados e formatação, nosso criação somente exigirá marcação simples para entramos o código na exibição da fonte.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Observe que a instrução "Eval": &lt;% # % Eval("CategoryName")&gt;

A sintaxe ASP.NET &lt;% # %&gt; é uma convenção de abreviação que instrui o tempo de execução para executar tudo o que está contido dentro e gerar os resultados "na linha".

A instrução Eval("CategoryName") instrui que, para a entrada atual na coleção de itens de dados associada busca o valor dos nomes de item de modelo de entidade "CatagoryName". Esta é a sintaxe concisa para um recurso muito poderoso.

Permite executar o aplicativo agora.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Observe que nosso menu de categoria de produto agora é exibido e quando, passe o mouse sobre um dos itens de menu categoria, podemos ver os pontos de link de item de menu para uma página que temos para implementar denominado ProductsList.aspx e que criamos um argumento de cadeia de caracteres de consulta dinâmica que contém o  id da categoria.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-2.md)
> [Próximo](tailspin-spyworks-part-4.md)
