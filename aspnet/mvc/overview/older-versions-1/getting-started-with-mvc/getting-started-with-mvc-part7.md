---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "Adicionando validação para o modelo | Microsoft Docs"
author: shanselman
description: "Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 5616c3c3bc77be0a770540d04cc2ae48ba9eedff
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
<a name="adding-validation-to-the-model"></a>Adicionando validação para o modelo
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos implementar o suporte necessário para habilitar a validação de entrada em nosso aplicativo. Irá garantir que nosso conteúdo de banco de dados sempre está correto e fornecer mensagens de erro úteis para os usuários finais quando eles tentem e inserem dados de filme que não é válidos. Vamos começar adicionando um pouco lógica de validação para a classe do filme.

Clique com o botão direito na pasta do modelo e selecione Adicionar classe. Nome de sua classe filme.

Quando criamos o modelo de entidade de filme anteriormente, o IDE criado uma classe do filme. Na verdade, parte da classe de filme pode estar em um arquivo e parte em outro. Isso é chamado de uma classe parcial. Vamos estender a classe de filme de outro arquivo.

Vamos criar uma classe parcial de filme que aponta para uma classe"amigos" com alguns atributos que fornecerá as dicas de validação para o sistema. Vamos marcar o título e o preço conforme necessário e também insistir que o preço seja em um determinado intervalo. Clique com botão direito na pasta modelos e selecione Adicionar classe. Nome de sua classe filme e clique no botão Okey. É aqui que nosso parcial filme classe parece.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Execute novamente o seu aplicativo e tentar inserir um filme com um preço acima de 100. Após ter enviado o formulário, você receberá um erro. O erro é detectado no lado do servidor e ocorre depois que o formulário é enviado. Observe como os auxiliares HTML internos de ASP.NET MVC foram inteligentes exibir a mensagem de erro e manter os valores para que possamos dentro de elementos de texto:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Isso funciona muito bem, mas seria interessante se poderia informamos ao usuário no lado do cliente, imediatamente, antes do servidor é envolvido.

Vamos habilitar alguma validação do lado do cliente com JavaScript.

## <a name="adding-client-side-validation"></a>Adicionando validação do lado do cliente

Como nossa classe filme já tem alguns atributos de validação, vamos apenas precisará adicionar alguns arquivos JavaScript para nosso modelo de exibição de Create.aspx e adicionar uma linha de código para habilitar a validação do lado do cliente ocorra.

No VWD acesse nossa pasta de modos de exibição/filme e abra Create.aspx.

Abra a pasta de Scripts no Gerenciador de soluções e arraste os seguintes três scripts para dentro do &lt;head&gt; marca.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Deseja que esses arquivos de script são exibidos nessa ordem.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Além disso, adicione esta linha única acima de Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Aqui está o código mostrado no IDE.

[![Filmes - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Executar seu aplicativo, visite /Movies/Create novamente e clique em criar sem inserir os dados. As mensagens de erro aparecem imediatamente sem a página flash que associadas ao enviar dados de volta para o servidor. Isso ocorre porque o ASP.NET MVC agora está validando a entrada no cliente (usando JavaScript) e no servidor.

[![Criar - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Isso está procurando válido! Agora vamos adicionar uma coluna adicional para o banco de dados.

>[!div class="step-by-step"]
[Anterior](getting-started-with-mvc-part6.md)
[Próximo](getting-started-with-mvc-part8.md)
