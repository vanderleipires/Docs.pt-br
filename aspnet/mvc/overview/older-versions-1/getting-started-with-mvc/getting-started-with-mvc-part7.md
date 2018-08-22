---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Adicionar validação ao modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824676"
---
<a name="adding-validation-to-the-model"></a>Adicionar validação ao modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos implementar o suporte necessário para habilitar a validação de entrada dentro do nosso aplicativo. Nós garantiremos que nosso conteúdo de banco de dados sempre está correto e fornecer mensagens de erro úteis para os usuários finais quando eles tente e insere dados de filme que não não válidos. Vamos começar adicionando um pouco lógica de validação para a classe de filme.

Clique com o botão direito na pasta do modelo e selecione Add Class. Nome de sua classe de filme.

Quando criamos o modelo de entidade do filme anteriormente, o IDE criado uma classe de filme. Na verdade, parte da classe de filme pode ser em um arquivo e parte em outro. Isso é chamado de uma classe parcial. Vamos estender a classe de filme de outro arquivo.

Vamos criar uma classe parcial do filme que aponta para uma amigo "class" com alguns atributos que fornecerá dicas de validação para o sistema. Vamos marcar o título e o preço, conforme necessário e também insistir que o preço estar em um determinado intervalo. Clique com botão direito na pasta modelos e selecione Add Class. Nomeie sua classe de filme e clique no botão Okey. Aqui está a aparência de nossa pesquisa de classe parcial do filme.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Execute novamente o seu aplicativo e tentar inserir um filme com um preço acima de 100. Você obterá um erro depois de enviar o formulário. O erro é capturado no lado do servidor e ocorre depois que o formulário é postado. Observe como os auxiliares HTML internos de ASP.NET MVC foram inteligentes o suficiente para exibir a mensagem de erro e mantenha os valores para que possamos dentro dos elementos da caixa de texto:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Isso funciona muito bem, mas seria interessante se poderíamos dar ao usuário no lado do cliente, imediatamente, antes que o servidor é envolvido.

Vamos habilitar alguma validação do lado do cliente com o JavaScript.

## <a name="adding-client-side-validation"></a>Adicionando uma validação do lado do cliente

Como nossa classe Movie já tem alguns atributos de validação, estamos apenas será necessário adicionar alguns arquivos de JavaScript para nosso modelo de exibição de aspx e adicionar uma linha de código para habilitar a validação do lado do cliente entrar em vigor.

De dentro do VWD, acesse nossa pasta de modos de exibição/filme e abra aspx.

Abra a pasta de Scripts no Gerenciador de soluções e arraste os seguintes três scripts para dentro do &lt;head&gt; marca.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Você deseja que esses arquivos de script são exibidos nessa ordem.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Além disso, adicione essa única linha acima a Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Aqui está o código mostrado no IDE.

[![Filmes - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Executar seu aplicativo, visite /Movies/Create novamente e clique em criar sem inserir nenhum dado. As mensagens de erro são exibidas imediatamente sem a página flash que associamos ao enviar dados de volta para o servidor. Isso é porque o ASP.NET MVC agora está validando a entrada em ambos o cliente (usando JavaScript) e no servidor.

[![Criar – Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Isso está funcionando bem! Vamos agora adicionar uma coluna adicional para o banco de dados.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part6.md)
> [Próximo](getting-started-with-mvc-part8.md)
