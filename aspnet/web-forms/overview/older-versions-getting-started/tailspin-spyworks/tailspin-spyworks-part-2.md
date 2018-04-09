---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Camada de acesso de dados | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 2 aborda a adição de camada de acesso a dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-2-data-access-layer"></a>Parte 2: Camada de acesso a dados
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 2 aborda a adição de camada de acesso a dados.


## <a id="_Toc260221668"></a>  Adicionando a camada de acesso a dados

Nosso aplicativo de comércio eletrônico depende de dois bancos de dados.

Para obter informações do cliente, usaremos o banco de dados de associação do ASP.NET padrão. Para nosso catálogo de produto e de carrinho de compras implementaremos um banco de dados do SQL Express da seguinte maneira.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Tendo criado o banco de dados (Commerce.mdf) no aplicativo do aplicativo\_pasta de dados pode continuar para criar nossa camada de acesso a dados usando o Entity Framework .NET.

Vamos criar uma pasta denominada "dados\_acesso" e clique com o botão direito na pasta e selecione "Adicionar Novo Item".

Em "Modelos instalados" item e, em seguida, selecione "ADO.NET Entity Data Model" Insira EDM\_Commerce.edmx como o nome e clique no botão "Adicionar".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Escolha "Gerar do banco de dados".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Salve e compile.

Agora você está pronto para adicionar o nosso primeiro recurso – um menu de categoria de produto.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-1.md)
> [Próximo](tailspin-spyworks-part-3.md)
