---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Adicionando um controlador | Microsoft Docs
author: shanselman
description: "Uma versão atualizada se este tutorial está disponível aqui usando o Visual Studio 2013. O novo tutorial usa o ASP.NET MVC 5, que fornece muitas melhorias em t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 93a362cf83d39b29fcba3f2dee0c28257805a89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Adicionando um controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Uma versão atualizada se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa ASP.NET MVC 5, que fornece muitas melhorias sobre este tutorial.
> 
> 
> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Representa o MVC do modelo, o modo de exibição, o controlador. MVC é um padrão para o desenvolvimento de aplicativos, de modo que cada parte tem a responsabilidade diferente do outro.

- Modelo: Os dados do seu aplicativo
- Modos de exibição: Os arquivos de modelo seu aplicativo usará para gerar dinamicamente as respostas HTML.
- Controladores: Classes que lidam com solicitações de URL de entrada para o aplicativo, recuperar dados de modelo e, em seguida, especificar modelos de exibição que processam uma resposta ao cliente

Vamos ser abrangendo todos esses conceitos neste tutorial e mostram como usá-las para criar um aplicativo.

Vamos criar um novo controlador clicando duas vezes na pasta controllers no solution Explorer e selecionando Adicionar controlador.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nomeie seu novo controlador "HelloWorldController" e clique em Adicionar.

[![Adicionar caixa de diálogo do controlador](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Observe no Gerenciador de soluções, à direita que um novo arquivo foi criado para você chamou HelloWorldController.cs e se agora é aberto no **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Crie dois novos métodos assim dentro de sua nova classe pública HelloWorldController. Retornaremos uma cadeia de caracteres de HTML diretamente do nosso controlador como um exemplo.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

O controlador é denominado HelloWorldController e o novo método é chamado de índice. Execute o aplicativo novamente, como antes (clique no botão play ou pressione F5 para fazer isso). Depois que o navegador tenha sido iniciado, altere o caminho na barra de endereço para `http://localhost:xx/HelloWorld` onde xx é o número de seu computador tiver escolhido. Agora, seu navegador deve ser semelhante a captura de tela abaixo. Nosso método acima é retornado como uma cadeia de caracteres passada para um método chamado "Conteúdo". Dissemos que o sistema retorna apenas alguns HTML e fez isso!

ASP.NET MVC invoca diferentes classes de controlador (e diferentes métodos de ação dentro delas) dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como isso para controlar o que o código é executado:

/ [Controller] / [ActionName] / [parâmetros]

A primeira parte da URL determina a classe do controlador para executar. Portanto /HelloWorld mapeia para a classe HelloWorldController. A segunda parte da URL determina o método de ação na classe para executar. Portanto /HelloWorld/Index faria com que o método Index () da classe HelloWorldcontroller para executar. Observe que apenas precisamos visitar /HelloWorld acima e o método de que índice foi indicado. Isso ocorre porque um método denominado "Index" é o padrão que será chamado em um controlador, se ainda não for explicitamente especificado.

[![Este é o meu ação padrão](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Agora, vamos visite `http://localhost:xx/HelloWorld/Welcome.` agora nosso método bem-vindo é executada e retorna sua cadeia de caracteres HTML.

Novamente, / [Controller] / [ActionName] / [parâmetros] para controlador é HelloWorld e bem-vindo é o método nesse caso. Ainda não fizemos parâmetros.

[![Esse é o método de ação de boas-vindas](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Vamos modificar nosso exemplo um pouco para que podemos passar algumas informações da URL para o nosso controlador, por exemplo, como este: HelloWorld/boas-vindas? name = Scott&amp;numtimes = 4. Altere o método de boas-vindo para incluir os dois parâmetros e update-lo como abaixo. Observe que usamos o recurso de parâmetro opcional do c# para indicar que o parâmetro numTimes deve 1 como padrão se ele não for passado.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Execute o aplicativo e visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` alterando o valor de nome e numtimes como desejar. O sistema mapeadas automaticamente os parâmetros nomeados de sua cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.

Nos dois exemplos do controlador foi fazer todo o trabalho e tem sido HTML serão retornando diretamente. Em geral não queremos que nossa controladores retornando HTML diretamente - desde que acaba sendo muito difícil de código. Em vez disso, usaremos normalmente um arquivo de modelo separado do modo de exibição para ajudar a gerar a resposta HTML. Vamos ver como podemos fazer isso. Feche seu navegador e retornar para o IDE.

>[!div class="step-by-step"]
[Anterior](getting-started-with-mvc-part1.md)
[Próximo](getting-started-with-mvc-part3.md)
