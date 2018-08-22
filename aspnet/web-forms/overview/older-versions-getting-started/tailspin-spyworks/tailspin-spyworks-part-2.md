---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Camada de acesso a dados | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 2 mostra a adição de camada de acesso a dados.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5b5ae08b157054bf0f3231782127ee3110f36d00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824181"
---
<a name="part-2-data-access-layer"></a>Parte 2: Camada de acesso a dados
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 2 mostra a adição de camada de acesso a dados.


## <a id="_Toc260221668"></a>  Adicionar a camada de acesso de dados

Nosso aplicativo de comércio eletrônico dependerá de dois bancos de dados.

Para obter informações do cliente, usaremos o banco de dados de associação do ASP.NET padrão. Para nosso catálogo de produto e de carrinho de compras, implementaremos um banco de dados do SQL Express da seguinte maneira.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Depois de criar o banco de dados (Commerce.mdf) no aplicativo do aplicativo\_pasta de dados pode prosseguir para criar nossa camada de acesso de dados usando o Entity Framework do .NET.

Vamos criar uma pasta denominada "dados\_acesso" e eles clique com o botão direito na pasta e selecione "Adicionar Novo Item".

Em "Installed Templates" item e, em seguida, selecione "dados de modelo de entidade ADO.NET" Insira EDM\_Commerce.edmx como o nome e clique no botão "Adicionar".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Escolha "Gerar do banco de dados".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Salve e compile.

Agora estamos prontos para adicionar nosso primeiro recurso – um menu de categoria de produto.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-1.md)
> [Próximo](tailspin-spyworks-part-3.md)
