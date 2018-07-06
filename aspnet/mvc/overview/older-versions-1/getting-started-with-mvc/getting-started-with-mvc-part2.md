---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Adicionando um controlador | Microsoft Docs
author: shanselman
description: Uma versão atualizada, se este tutorial está disponível aqui usando o Visual Studio 2013. O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos em t...
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: afe1182bca0fe58e1df162c5027df35992eeef38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809904"
---
<a name="adding-a-controller"></a>Adicionando um controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Uma versão atualizada, se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos ao longo deste tutorial.
> 
> 
> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


MVC representa o modelo, o modo de exibição, o controlador. O MVC é um padrão para o desenvolvimento de aplicativos, de modo que cada parte tem uma responsabilidade que é diferente de outro.

- Modelo: Os dados do seu aplicativo
- Modos de exibição: Os arquivos de modelo seu aplicativo usará para gerar dinamicamente respostas HTML.
- Controladores: Classes que lidam com solicitações de URL de entrada para o aplicativo recuperar dados de modelo e, em seguida, especificar modelos de exibição que renderizam uma resposta ao cliente

Vamos abordar todos esses conceitos neste tutorial e mostrar como usá-los para criar um aplicativo.

Vamos criar um novo controlador clicando duas vezes na pasta controladores no solution Explorer e selecionando Adicionar controlador.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nomeie o novo controlador "HelloWorldController" e clique em Adicionar.

[![Adicionar caixa de diálogo do controlador](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Observe que no Gerenciador de soluções à direita que um novo arquivo foi criado para você chamou HelloWorldController.cs e agora, esse arquivo é aberto na **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Crie dois novos métodos que se parecem com isso dentro de sua nova classe pública HelloWorldController. Vamos retornar uma cadeia de caracteres de HTML diretamente do nosso controlador como um exemplo.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

O controlador é chamado HelloWorldController e o novo método é chamado de índice. Executar o aplicativo novamente, exatamente como antes (clique no botão play ou pressione F5 para fazer isso). Depois de iniciar seu navegador, altere o caminho na barra de endereços para `http://localhost:xx/HelloWorld` em que xx é o número de tudo o que seu computador tiver escolhido. Agora, seu navegador deve ser semelhante a captura de tela abaixo. Em nosso método acima é retornada uma cadeia de caracteres passada para um método chamado "Conteúdo". Nós dissemos que o sistema retorna apenas um HTML e fez isso!

ASP.NET MVC invoca as diferentes classes de controlador (e diferentes métodos de ação dentro delas), dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para controlar qual código é executado:

/ [Controller] / [nome da ação] / [parâmetros]

A primeira parte da URL determina a classe de controlador para executar. Portanto, /HelloWorld mapeia para a classe HelloWorldController. A segunda parte da URL determina o método de ação na classe para executar. Portanto, /HelloWorld/Index faria com que o método Index () da classe HelloWorldcontroller para executar. Observe que só tivemos que visite /HelloWorld acima e o método que index foi indicado. Isso ocorre porque um método chamado "Index" é o método padrão que será chamado em um controlador se nenhum for especificado explicitamente.

[![Essa é minha ação padrão](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Agora, vamos visite `http://localhost:xx/HelloWorld/Welcome.` agora bem-vindo ao nosso método foi executado e que retornou de sua cadeia de caracteres HTML.

Novamente, / [Controller] / [nome da ação] / [parâmetros] para que o controlador é HelloWorld e boas-vindas é o método nesse caso. Ainda não fizemos parâmetros.

[![Esse é o método de ação boas-vindas](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Vamos modificar nosso exemplo um pouco para que podemos passar algumas informações da URL para o nosso controlador, por exemplo, como este: / HelloWorld/Welcome? nome = Scott&amp;numtimes = 4. Altere o método de boas-vindo para incluir dois parâmetros e update-lo como abaixo. Observe que usamos o recurso de parâmetro opcional do c# para indicar que o parâmetro numTimes deve padrão como 1 se ele não for passado.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Executar o aplicativo e visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` alterando o valor de nome e numtimes como desejar. O sistema mapeadas automaticamente os parâmetros nomeados da sua cadeia de caracteres de consulta na barra de endereços para os parâmetros no método.

Nos dois exemplos do controlador tiver sido fazendo todo o trabalho e retornava HTML diretamente. Normalmente, não queremos que nossos controladores retornem HTML diretamente – desde que acaba sendo muito difíceis de código. Em vez disso, vamos normalmente usar um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML. Vamos examinar como é possível fazer isso. Feche seu navegador e retornar ao IDE.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part1.md)
> [Próximo](getting-started-with-mvc-part3.md)
