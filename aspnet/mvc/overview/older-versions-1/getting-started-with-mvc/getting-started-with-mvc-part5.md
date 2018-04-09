---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acessando dados do modelo de um controlador | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Acessando dados do modelo de um controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos criar uma nova classe MoviesController e escrever um código que recupera os dados do filme e exibe-o novamente para o navegador usando um modelo de exibição.

Clique com o botão direito na pasta controladores e fazer uma nova MoviesController.

[![Adicionar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Isso criará um novo arquivo de "MoviesController.cs" sob nossa pasta \Controllers em nosso projeto. Vamos atualizar MovieController para recuperar a lista de filmes de nosso banco de dados recentemente preenchido.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Estamos executando uma consulta LINQ para que recuperamos somente filmes lançados após o segundo semestre de 1984. Precisaremos de um modelo de exibição para renderizar a esta lista de filmes novamente, então com o botão direito no método e selecione Adicionar modo de exibição para criá-lo.

A caixa de diálogo Adicionar modo de exibição será indicamos que podemos estiver passando uma lista&lt;Movies.Models.Movie&gt; nosso modelo de exibição. Ao contrário das vezes anteriores é usada a caixa de diálogo Adicionar modo de exibição e optar por criar um modelo de "Empty", neste momento, poderá indicar que queremos Visual Studio automaticamente "Scaffold" um modelo de exibição para nós com algum conteúdo padrão. Faremos isso selecionando o item "List" no menu"conteúdo suspenso de exibição.

Lembre-se de que quando você criou uma nova classe que você precisará compilar seu aplicativo para que ela seja exibida na caixa de diálogo Adicionar modo de exibição.

![Adicionar modo de exibição](getting-started-with-mvc-part5/_static/image3.png)

Clique em Adicionar e o sistema irá gerar automaticamente o código para um modo de exibição para nós que exibe a lista de filmes. Esse é um bom momento para alterar o &lt;h2&gt; título para algo como "Minha lista de filme" como fizemos anteriormente com a exibição de Hello World.

[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Execute o aplicativo e visite /Movies na barra de endereços. Agora você recuperou dados do banco de dados usando uma consulta básica dentro do controlador e retornou os dados para uma exibição que conhece filmes. Esse modo de exibição gira a lista de filmes e cria uma tabela de dados para nós.

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Nós não implementar funcionalidade de edição, detalhes e Delete com este aplicativo - para os links padrão que o modelo de scaffold criado para nós não é necessário. Abra o arquivo /Movies/Index.aspx e removê-los.

Aqui está o código-fonte para o nosso modelo de exibição atualizado deve ter aparência depois que podemos fazer essas alterações:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Criar links que não precisamos, portanto iremos exclui-los para este exemplo. Manteremos nosso criar novo link, pois esse é o próximo! Aqui está a aparência de nosso aplicativo com a coluna removida.

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Agora temos uma lista simples de nossos dados do filme. No entanto, se clicarmos no link "Criar novo", podemos obterá um erro como ele está conectado não! Vamos implementar um método de ação de criar e habilitar um usuário para inserir novos filmes em nosso banco de dados.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part4.md)
> [Próximo](getting-started-with-mvc-part6.md)
