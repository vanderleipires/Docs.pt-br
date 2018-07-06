---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Adicionando uma coluna para o modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817195"
---
<a name="adding-a-column-to-the-model"></a>Adicionando uma coluna para o modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos examinar como podemos fazer alterações no esquema de nosso banco de dados e lidar com as alterações dentro do nosso aplicativo.

Vamos adicionar uma coluna "Classificação" a tabela Movie. Volte para o IDE e clique em Gerenciador de banco de dados. Tabela Movie clique com botão direito e selecione Abrir definição de tabela.

Adicione uma coluna de "Classificação", conforme mostrado abaixo. Como não há qualquer classificações agora, a coluna pode permitir nulos. Clique em Salvar.

[![Edição de tabela de filmes](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Em seguida, retorne ao Gerenciador de soluções e abra o arquivo Movies.edmx (que está na pasta \Models). Clique com o botão direito na superfície de design (na área branca) e selecione o modelo de atualização do banco de dados.

[![Filmes - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Isso iniciará o Assistente"atualização". Clique na guia de atualização dentro dele e clique em Concluir. Nossa classe de modelo de filme, em seguida, será atualizado com a nova coluna.

![Assistente de atualização (2)](getting-started-with-mvc-part8/_static/image5.png)

Depois de clicar em Concluir, você pode ver que a nova coluna de classificação foi adicionada à entidade em nosso modelo de filme.

[![Entidade de filme](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Adicionamos uma coluna no modelo de banco de dados, mas os modos de exibição não souber sobre ele.

## <a name="update-views-with-model-changes"></a>Modos de exibição de atualizações com as alterações do modelo

Há algumas maneiras de poderia atualizamos nossos modelos de exibição para refletir a nova coluna de classificação. Uma vez que criamos esses modos de exibição por meio da geração-los por meio da caixa de diálogo Adicionar modo de exibição, poderíamos excluí-los e recriá-los novamente. No entanto, normalmente as pessoas serão já tiver feito modificações em seus modelos de exibição da geração inicial gerado por scaffolding e vai querer adicionar ou excluir campos manualmente, exatamente como fizemos com o campo de ID para criar.

Abra o modelo \Views\Movies\Index.aspx e adicione uma &lt;ésimo&gt;Rating&lt;/th&gt; ao cabeçalho da tabela Movie. Eu adicionei meu após gênero. Em seguida, na mesma posição de coluna, mas mais baixo, adicione uma linha para nossa nova classificação de saída.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nosso modelo aspx final terá esta aparência:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Vamos, em seguida, abra o modelo \Views\Movies\Create.aspx e adicione um rótulo e uma caixa de texto para nossa nova propriedade de classificação:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nosso modelo aspx final se parecer com isso e vamos alterar o título e o secundário nosso navegador &lt;h2&gt; title para algo como "Criar um filme" enquanto estamos fazendo aqui!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Executar seu aplicativo e agora você tem um novo campo no banco de dados que foi adicionado para a página criar. Adicionar um novo filme – desta vez com uma classificação – e clique em criar.

[![Criar um filme - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Depois de clicar em criar, você é enviado para a página de índice onde você novo filme é listado com a nova coluna de classificação no banco de dados

[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Este tutorial básico te a você a começar a transformar controladores, associá-los de modos de exibição e à passagem de dados embutidos. Em seguida, criamos e criado um banco de dados e colocar alguns dados nele. Podemos recuperou os dados do banco de dados e exibida-lo em uma tabela HTML. Em seguida, adicionamos um formulário de criação que permitem ao usuário adicionar dados ao banco de dados em si de dentro do aplicativo Web. Estamos adicionado validação e fez com que a validação usar JavaScript do lado do cliente. Por fim, podemos alterado no banco de dados incluem uma nova coluna de dados e nossas duas páginas atualizadas para criar e exibir esses novos dados.

Agora recomendo que você para passar para nosso tutorial de nível intermediário "[Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", bem como os vários vídeos e recursos [ https://asp.net/mvc ](https://asp.net/mvc) para saber mais sobre o ASP.NET MVC!

Aproveite!

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) no Twitter.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part7.md)
