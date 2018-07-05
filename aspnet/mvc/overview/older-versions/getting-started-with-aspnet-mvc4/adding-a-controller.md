---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Adicionando um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 100f5623bafa33548f9c979e98765c92327eb369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373283"
---
<a name="adding-a-controller"></a>Adicionando um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.


Representa o MVC *model-view-controller*. O MVC é um padrão para o desenvolvimento de aplicativos que são bem arquitetado, testável e fácil de manter. Aplicativos baseados no MVC contêm:

- **M** odels: Classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócios para que os dados.
- **V** iews: arquivos de modelo que seu aplicativo usa para gerar dinamicamente respostas HTML.
- **C** ontrollers: Classes que lidam com solicitações recebidas do navegador, recuperar dados de modelo e, em seguida, especificar modelos de exibição que retornam uma resposta ao navegador.

Vamos abordar todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.

Vamos começar criando uma classe de controlador. Na **Gerenciador de soluções**, clique com botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**.

![](adding-a-controller/_static/image1.png)

Nomeie o novo controlador &quot;HelloWorldController&quot;. Deixe o modelo padrão como **controlador MVC vazio** e clique em **Add**.

![Adicionar controlador](adding-a-controller/_static/image2.png)

Observe na **Gerenciador de soluções** que um novo arquivo tiver sido criado com o nome *HelloWorldController.cs*. O arquivo está aberto no IDE.

![](adding-a-controller/_static/image3.png)

Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Os métodos do controlador retornará uma cadeia de caracteres de HTML como um exemplo. O controlador é denominado `HelloWorldController` e o primeiro método acima é denominado `Index`. Vamos chamá-la em um navegador. Execute o aplicativo (pressione F5 ou Ctrl + F5). No navegador, acrescente &quot;HelloWorld&quot; para o caminho na barra de endereços. (Por exemplo, na ilustração abaixo, é `http://localhost:1234/HelloWorld.`) a página no navegador se parecerá com a seguinte captura de tela. No método acima, o código retornado diretamente uma cadeia de caracteres. Você disse que o sistema para retornar apenas um HTML e fez isso!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC invoca as classes de controlador diferente (e diferentes métodos de ação dentro delas), dependendo da URL de entrada. A lógica de roteamento de URL do padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código para invocar:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe de controlador para executar. Portanto, */HelloWorld* mapeia para o `HelloWorldController` classe. A segunda parte da URL determina o método de ação na classe para executar. Portanto, */HelloWorld/Index* faria com que o `Index` método da `HelloWorldController` classe para executar. Observe que precisamos navegar até */HelloWorld* e o `Index` método foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O `Welcome` método é executado e retorna a cadeia de caracteres &quot;esse é o método de ação boas-vindas... &quot;. O mapeamento do padrão MVC é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image5.png)

Vamos modificar o exemplo um pouco para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? nome = Scott&amp;numtimes = 4*). Alterar sua `Welcome` método para incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o recurso de parâmetro opcional do c# para indicar que o `numTimes` parâmetro como padrão 1 se nenhum valor é passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Executar o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Você pode tentar valores diferentes para `name` e `numtimes` na URL. O [sistema de associação de modelos do ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados da cadeia de consulta na barra de endereços para os parâmetros no método.

![](adding-a-controller/_static/image6.png)

Nos dois exemplos do controlador faz o &quot;VC&quot; parte do MVC — ou seja, o trabalho de exibição e controlador. O controlador retorna o HTML diretamente. Normalmente, você não quer controladores retornem HTML diretamente, já que isso é muito difícil para o código. Em vez disso, vamos normalmente usar um arquivo de modelo de exibição separada para ajudar a gerar a resposta HTML. Vamos dar uma olhada próxima em como é possível fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-4.md)
> [Próximo](adding-a-view.md)
