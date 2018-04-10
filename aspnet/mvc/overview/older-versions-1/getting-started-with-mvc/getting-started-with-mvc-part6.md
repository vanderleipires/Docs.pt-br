---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Adicionando um método de criação e criar exibição | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-create-method-and-create-view"></a>Adicionando um método de criação e criar modo de exibição
====================
by [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos implementar o suporte necessário para permitir que os usuários criem novos filmes em nosso banco de dados. Faremos isso Implementando a ação de URL de filmes/Criar.

Implementando a URL de filmes/cria é um processo de duas etapas. Quando um usuário acessa primeiro a URL de filmes/Criar queremos mostrá-los em um formulário HTML que pode ser preenchida para inserir um novo filme. Em seguida, quando o usuário envia o formulário e os dados de volta para o servidor de mensagens, queremos recuperar o conteúdo publicado e salvá-lo em nosso banco de dados.

Implementaremos essas duas etapas dentro de dois métodos de Create () dentro de nossa classe MoviesController. Um método mostrará o &lt;formulário&gt; que o usuário deve preencher para criar um novo filme. O segundo método tratará o processamento de dados de postagem quando o usuário envia o &lt;formulário&gt; de volta para o servidor e salvar um novo filme em nosso banco de dados.

Abaixo está o código vamos adicionar à nossa classe MoviesController implementar isso:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

O código acima contém todo o código que vamos precisar em nosso controlador.

Agora, vamos implementar o modelo Criar modo de exibição que será usado para exibir um formulário para o usuário. Vamos clique com o botão direito no primeiro método de criação e selecione "Adicionar modo de exibição" para criar o modelo de exibição de nosso formulário de filme.

Selecionaremos que vão para passar o modelo de exibição "Filme" como sua classe de dados de exibição e indicar que queremos "scaffold" em um modelo de "Criar".

[![Adicionar modo de exibição](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Depois de clicar no botão Adicionar, modelo de exibição de \Movies\Create.aspx será criado para você. Como podemos selecionou "Criar" no menu suspenso "Exibir conteúdo", a caixa de diálogo Adicionar modo de exibição automaticamente "Scaffold" algum conteúdo padrão para nós. O scaffolding criado um HTML &lt;formulário&gt;, mensagens de um local para o erro de validação para ir e desde scaffolding conhece filmes, ele criou rótulo e campos para cada propriedade de nossa classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Como o nosso banco de dados fornece automaticamente um filme uma ID, vamos remover esses campos desse modelo de referência. ID da visão de criar. Remover as 7 linhas depois &lt;legenda&gt;campos&lt;/legend&gt; conforme elas mostram o campo de ID que não queremos.

Vamos agora criar um novo filme e adicioná-lo ao banco de dados. Vamos fazer isso executando o aplicativo novamente e visite o "/ filmes" URL e clique em "Criar" link para adicionar um novo filme.

[![Criar - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Quando é clicar no botão Criar, publicaremos novamente (por meio de HTTP POST) os dados neste formulário para o método /Movies/Create que acabou de criar. Assim como quando o sistema automaticamente o parâmetro "numTimes" e "nome" da URL e mapeados-los para parâmetros em um método anteriormente, o sistema automaticamente levar os campos de formulário de um POST e mapeá-los para um objeto. Nesse caso, os valores dos campos em HTML como "ReleaseDate" e "Title" serão automaticamente colocados nas propriedades corretas de uma nova instância de um filme.

Vamos analisar o segundo método de criação do nosso MoviesController novamente. Observe como ele usa um objeto de "Filme" como um argumento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Esse objeto de filme, em seguida, passado para a versão do [HttpPost] do nosso método de ação de criar, e é salvo no banco de dados e redirecionado, em seguida, o usuário para o método de ação Index () que mostrará o resultado salvo na lista de filmes:

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Nós não verificando se o nosso filmes estão corretos, embora, e o banco de dados não permite a salvar um filme sem título. Seria interessante se poderia informamos ao usuário que gerou um erro antes do banco de dados. Faremos isso em seguida, adicionando suporte de validação para o nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part5.md)
> [Próximo](getting-started-with-mvc-part7.md)
