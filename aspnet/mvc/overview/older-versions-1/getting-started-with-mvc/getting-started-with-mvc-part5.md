---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acessando dados do seu modelo de um controlador | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834492"
---
<a name="accessing-your-models-data-from-a-controller"></a>Acessando dados do seu modelo de um controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos criar uma nova classe MoviesController e escrever um código que recupera os nossos dados de filme e exibe-o novamente para o navegador usando um modelo de exibição.

Clique com o botão direito na pasta Controllers e fazer uma nova MoviesController.

[![Adicionar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Isso criará um novo arquivo de "MoviesController.cs" sob a nossa pasta \Controllers dentro do nosso projeto. Vamos atualizar o MovieController para recuperar a lista de filmes do nosso banco de dados recentemente populado.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Estamos executando uma consulta LINQ para que podemos apenas recuperar filmes lançados após o segundo semestre de 1984. Precisaremos de um modelo de exibição para renderizar esta lista de filmes de volta, então, clique com botão direito no método e selecione Adicionar modo de exibição para criá-lo.

Na caixa de diálogo Adicionar modo de exibição indicaremos que estamos passando uma lista&lt;Movies.Models.Movie&gt; para nosso modelo de exibição. Ao contrário de tempos anteriores é usada a caixa de diálogo Adicionar modo de exibição e optar por criar um modelo de "Empty", neste momento que podemos irá indicar que queremos Visual Studio automaticamente "criar o scaffolding de" um modelo de exibição para nós com algum conteúdo padrão. Vamos fazer isso selecionando o item "List" em "Exibir conteúdo menu suspenso.

Lembre-se de que quando você criou uma nova classe, você precisará compilar seu aplicativo para que ele apareça na caixa de diálogo Adicionar modo de exibição.

![Adicionar modo de exibição](getting-started-with-mvc-part5/_static/image3.png)

Clique em Adicionar e o sistema gerará automaticamente o código para um modo de exibição para nós que exibe nossa lista de filmes. Isso é um bom momento para alterar o &lt;h2&gt; título para algo como "Minha lista de filmes" como fizemos anteriormente com a exibição de Hello World.

[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Executar o aplicativo e visite /Movies na barra de endereços. Agora nós dados recuperados do banco de dados usando uma consulta básica do controlador e os dados retornados a uma exibição que sabe sobre filmes. Esse modo de exibição gira através da lista de filmes e cria uma tabela de dados para nós.

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Nós não implementará funcionalidade de edição, detalhes e exclusão com este aplicativo - portanto, não precisamos de links padrão que o modelo de scaffold criado para nós. Abra o arquivo /Movies/Index.aspx e removê-los.

Aqui está o código-fonte para o nosso modelo de exibição atualizado aparência depois de fazermos a essas alterações:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Criação de links que não precisamos, portanto, nós os excluiremos para este exemplo. Vamos manter nosso criar novo link no entanto, como o que vem a seguir! Aqui está a aparência de nosso aplicativo com aquela coluna removida.

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Agora temos uma lista simples de nossos dados de filme. No entanto, se podemos clicar no link "Criar novo", obterá um erro pois ele não está associado! Vamos implementar um método de ação de criar e habilitar um usuário a inserir novos filmes em nosso banco de dados.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part4.md)
> [Próximo](getting-started-with-mvc-part6.md)
