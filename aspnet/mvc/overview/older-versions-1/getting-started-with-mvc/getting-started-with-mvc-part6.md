---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Adicionando um método de criação e criar modo de exibição | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: cf0d721b551c38e8c38e35f82b73ee1b14cd068f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840953"
---
<a name="adding-a-create-method-and-create-view"></a>Adicionando um método de criação e criar modo de exibição
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos implementar o suporte necessário para permitir que os usuários criem novos filmes em nosso banco de dados. Faremos isso Implementando a ação de URL Movies/Create.

Implementando a URL de filmes/Criar é um processo de duas etapas. Quando um usuário pela primeira vez visita a URL de filmes/Criar queremos mostrá-los em um formulário HTML que pode preencher a inserir um novo filme. Em seguida, quando o usuário envia o formulário e os dados de volta para o servidor de postagens, queremos recuperar o conteúdo postado e salvá-lo em nosso banco de dados.

Implementaremos essas duas etapas dentro de dois métodos Create () dentro de nossa classe MoviesController. Um método mostrará a &lt;formulário&gt; que o usuário deve preencher para criar um novo filme. O segundo método tratará o processamento de dados postados quando o usuário envia o &lt;formulário&gt; de volta para o servidor e salvar um novo filme dentro do nosso banco de dados.

Abaixo está o código, adicionaremos à nossa classe MoviesController implementar isso:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

O código acima contém todo o código que vamos precisar dentro do nosso controlador.

Agora, vamos implementar o modelo de Create View que usaremos para exibir um formulário para o usuário. Vamos clique com botão direito no primeiro método de criar e selecione "Adicionar exibição" para criar o modelo de exibição para nosso formulário de filme.

Vamos selecionar que estamos indo para passar o modelo de exibição "Filme" como sua classe de dados de exibição e indicar que desejamos criar o "scaffolding" de um modelo de "Criar".

[![Adicionar modo de exibição](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Depois de clicar no botão Add, o modelo de exibição \Movies\Create.aspx será criado para você. Porque nós selecionamos "Criar" na lista suspensa "modo de exibição de conteúdo", a caixa de diálogo Adicionar modo de exibição automaticamente "geradas por scaffolding" algum conteúdo padrão para nós. O scaffolding criou um HTML &lt;formulário&gt;, mensagens de um local para o erro de validação para ir e uma vez que o scaffolding sabe sobre filmes, ele criou o rótulo e campos para cada propriedade de nossa classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Como nosso banco de dados fornece automaticamente um filme uma ID, vamos remover esses campos desse modelo de referência. ID de nosso Create View. Remover as 7 linhas depois &lt;legenda&gt;campos&lt;/legend&gt; conforme eles mostram o campo de ID que não queremos.

Vamos agora criar um novo filme e adicioná-lo ao banco de dados. Vamos fazer isso executando o aplicativo novamente e visite a "/ filmes" URL e clique em "Criar" link para adicionar um novo filme.

[![Criar – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Quando clicamos no botão Criar, publicaremos novamente (por meio de HTTP POST) os dados neste formulário para o método /Movies/Create que acabamos de criar. Assim como quando o sistema automaticamente o parâmetro "numTimes" e "nome" da URL e mapeados-los para parâmetros em um método anteriormente, o sistema automaticamente tirar os campos de formulário de uma POSTAGEM e mapeá-los para um objeto. Nesse caso, os valores dos campos em HTML, como "ReleaseDate" e "Title" serão automaticamente colocados nas propriedades corretas de uma nova instância de um filme.

Vamos examinar o segundo método de criação de nosso MoviesController novamente. Observe como ele utiliza um objeto de "Filme" como um argumento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Esse objeto de filme, em seguida, foi passado para a versão de [HttpPost] do nosso método de ação de criar e, ele será salvo no banco de dados e redirecionado, em seguida, o usuário para o método de ação Index () que mostra o resultado salvo na lista de filmes:

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Nós não estão verificando se o nosso filmes estão corretos, no entanto, e o banco de dados não nos permite salvar um filme sem título. Seria bom se poderíamos dar ao usuário que gerou um erro antes do banco de dados. Faremos isso em seguida, adicionando suporte de validação ao nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part5.md)
> [Próximo](getting-started-with-mvc-part7.md)
