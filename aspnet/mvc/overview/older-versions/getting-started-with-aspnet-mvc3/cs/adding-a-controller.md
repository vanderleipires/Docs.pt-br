---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Adicionando um controlador (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express independentemente Pack 1, que i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: ba6cc715f8c8eaf624ab5314e3afbfd68da11485
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370886"
---
<a name="adding-a-controller-c"></a>Adicionando um controlador (c#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.
> 
> 
> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.


Representa o MVC *model-view-controller*. O MVC é um padrão para o desenvolvimento de aplicativos que são fáceis de manter e bem arquitetada. Aplicativos baseados no MVC contêm:

- Controladores: Classes que lidam com solicitações de entrada para o aplicativo recuperar dados de modelo e, em seguida, especificar modelos de exibição que retornam uma resposta ao cliente.
- Modelos: Classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócios para que os dados.
- Modos de exibição: Arquivos de modelo o aplicativo usa para gerar dinamicamente respostas HTML.

Vamos abordar todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.

Vamos começar criando uma classe de controlador. Na **Gerenciador de soluções**, clique com botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nomeie o novo controlador "HelloWorldController". Deixe o modelo padrão como **controlador vazio** e clique em **Add**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Observe na **Gerenciador de soluções** que um novo arquivo tiver sido criado com o nome *HelloWorldController.cs*. O arquivo está aberto no IDE.

![](adding-a-controller/_static/image5.png)

Dentro de `public class HelloWorldController` bloquear, crie dois métodos que se parecem com o código a seguir. O controlador retorna uma cadeia de caracteres de HTML como um exemplo.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

O controlador é denominado `HelloWorldController` e o primeiro método acima é denominado `Index`. Vamos chamá-la em um navegador. Execute o aplicativo (pressione F5 ou Ctrl + F5). No navegador, acrescente "HelloWorld" ao caminho na barra de endereços. (Por exemplo, na ilustração abaixo, é `http://localhost:43246/HelloWorld.`) a página no navegador se parecerá com a seguinte captura de tela. No método acima, o código retornado diretamente uma cadeia de caracteres. Você disse que o sistema para retornar apenas um HTML e fez isso!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC invoca as classes de controlador diferente (e diferentes métodos de ação dentro delas), dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código para invocar:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe de controlador para executar. Portanto, */HelloWorld* mapeia para o `HelloWorldController` classe. A segunda parte da URL determina o método de ação na classe para executar. Portanto, */HelloWorld/Index* faria com que o `Index` método da `HelloWorldController` classe para executar. Observe que precisamos navegar até */HelloWorld* e o `Index` método foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres “Este é o método de ação Boas-vindas...”. O mapeamento do padrão MVC é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image7.png)

Vamos modificar o exemplo um pouco para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? nome = Scott&amp;numtimes = 4*). Alterar sua `Welcome` método para incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o recurso de parâmetro opcional do c# para indicar que o `numTimes` parâmetro como padrão 1 se nenhum valor é passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Executar o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Você pode tentar valores diferentes para `name` e `numtimes` na URL. O sistema mapeia automaticamente os parâmetros nomeados da cadeia de consulta na barra de endereços para os parâmetros no método.

![](adding-a-controller/_static/image8.png)

Nos dois exemplos do controlador faz a parte "VC" do MVC — ou seja, o trabalho de exibição e controlador. O controlador retorna o HTML diretamente. Normalmente, você não quer controladores retornem HTML diretamente, já que isso é muito difícil para o código. Em vez disso, vamos normalmente usar um arquivo de modelo de exibição separada para ajudar a gerar a resposta HTML. Vamos dar uma olhada próxima em como é possível fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Próximo](adding-a-view.md)
