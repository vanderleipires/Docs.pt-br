---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Adicionando uma coluna para o modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-column-to-the-model"></a>Adicionando uma coluna para o modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos percorrer como podemos fazer alterações no esquema de nosso banco de dados e controlar as alterações em nosso aplicativo.

Vamos adicionar uma coluna "Classificação" à tabela de filme. Volte para o IDE e clique em Gerenciador de banco de dados. A tabela de filme clique com botão direito e selecione Abrir definição de tabela.

Adicione uma coluna de "classificação de" conforme mostrado abaixo. Desde que não temos as classificações agora, a coluna pode permitir valores nulos. Clique em Salvar.

[![Editando a tabela de filmes](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Em seguida, retorne ao Gerenciador de soluções e abra o arquivo Movies.edmx (que está na pasta \Models). Clique com o botão direito na superfície de design (na área branca) e selecione o modelo de atualização de banco de dados.

[![Filmes - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Isso iniciará o Assistente de atualização"". Clique na guia de atualização nele e clique em Concluir. Nossa classe de modelo do filme será atualizado com a nova coluna.

![Assistente de atualização (2)](getting-started-with-mvc-part8/_static/image5.png)

Depois de clicar em Concluir, você pode ver que a nova coluna de classificação foi adicionada à entidade filme em nosso modelo.

[![Entidade de filme](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Adicionamos uma coluna no modelo de banco de dados, mas os modos de exibição não souber sobre ele.

## <a name="update-views-with-model-changes"></a>Atualizar modos de exibição com as alterações do modelo

Há algumas maneiras pôde atualizamos nossos modelos de exibição para refletir a nova coluna de classificação. Desde que criamos esses modos de exibição por gerá-los por meio da caixa de diálogo Adicionar modo de exibição, poderíamos excluí-los e recriá-los novamente. No entanto, normalmente pessoas serão já feitas modificações a seus modelos de exibição de geração de scaffolding inicial e vai querer adicionar ou excluir campos manualmente, assim como foi feito com o campo ID de criar.

Abra o modelo de \Views\Movies\Index.aspx e adicione um &lt;th&gt;classificação&lt;/th&gt; ao cabeçalho da tabela de filme. Adicionei minhas após gênero. Em seguida, na mesma posição de coluna, mas mais baixo, adicione uma linha para nossa nova classificação de saída.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nosso modelo Index.aspx final será assim:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Vamos, em seguida, abra o modelo \Views\Movies\Create.aspx e adicionar um rótulo e a caixa de texto para nossa nova propriedade de classificação:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nosso modelo Create.aspx final ter esta aparência e vamos alterar o título e o secundário nosso navegador &lt;h2&gt; título para algo como "Criar um filme" enquanto estamos aqui!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Executar seu aplicativo e agora você tem um novo campo no banco de dados foi adicionado à página de criação. Adicione um novo filme - dessa vez com uma classificação - e clique em criar.

[![Criar um filme - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Depois que você clicar em criar, é enviadas para a página de índice onde você novo filme está listado com a nova coluna de classificação no banco de dados

[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Este tutorial básico temos começar a fazer controladores, associá-los a modos de exibição e à passagem de dados embutidos. Em seguida, é criado e criado um banco de dados e colocar alguns dados nele. Podemos recuperou os dados do banco de dados e exibição em uma tabela HTML. Em seguida, adicionamos um formulário de criação que permitem que o usuário adicionar dados ao banco de dados-se de dentro do aplicativo Web. Adicionada a validação, então feita a validação usar JavaScript do lado do cliente. Por fim, podemos alterou o banco de dados para incluir uma nova coluna de dados e atualizado nossas duas páginas para criar e exibir esses dados novos.

Agora recomendo que você para passar para nosso tutorial de nível intermediário "[repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", bem como os vários vídeos e recursos em [ https://asp.net/mvc ](https://asp.net/mvc) para saber mais sobre o ASP.NET MVC!

Aproveite!

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) no Twitter.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part7.md)
