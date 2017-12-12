---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Adicionando um controlador (c#) | Microsoft Docs
author: Rick-Anderson
description: "Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express independentemente Pack 1, que..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77bfc8f3778dcf75453c216579e50a016b1ac971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller-c"></a>Adicionando um controlador (c#)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.
> 
> 
> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.


Representa o MVC *model-view-controller*. MVC é um padrão para o desenvolvimento de aplicativos que são bem projetado e fácil de manter. Aplicativos MVC contêm:

- Controladores: Classes que lidam com solicitações de entrada para o aplicativo, recuperar dados de modelo e, em seguida, especificar modelos de exibição que retornam uma resposta ao cliente.
- Modelos: Classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócio para os dados.
- Modos de exibição: Arquivos de modelo o aplicativo usa para gerar dinamicamente as respostas HTML.

Vamos ser abrangendo todos esses conceitos nesta série de tutoriais e mostram como usá-las para criar um aplicativo.

Vamos começar criando uma classe de controlador. Em **Solution Explorer**, com o botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nome do novo controlador de "HelloWorldController". Deixe o modelo padrão como **controlador vazio** e clique em **adicionar**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Observe na **Solution Explorer** que um novo arquivo tem foi criado com o nome *HelloWorldController.cs*. O arquivo é aberto no IDE.

![](adding-a-controller/_static/image5.png)

Dentro de `public class HelloWorldController` bloquear, crie dois métodos que se parecem com o código a seguir. O controlador retornará uma cadeia de caracteres de HTML como um exemplo.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

O controlador é nomeado `HelloWorldController` e o primeiro método acima é chamado `Index`. Vamos chamá-la em um navegador. Execute o aplicativo (pressione F5 ou Ctrl + F5). No navegador, acrescente "Olámundo" para o caminho na barra de endereços. (Por exemplo, na ilustração abaixo, ele `http://localhost:43246/HelloWorld.`) a página no navegador se parecerá com a captura de tela a seguir. No método, o código retornado diretamente uma cadeia de caracteres. Você disse que o sistema retornar apenas alguns HTML e fez isso!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC chama classes diferentes de controlador (e os métodos de ação diferente dentro delas) dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para determinar o que o código para chamar:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe do controlador para executar. Portanto */HelloWorld* mapeia para o `HelloWorldController` classe. A segunda parte da URL determina o método de ação na classe para executar. Portanto *HelloWorld/índice* causaria o `Index` método o `HelloWorldController` classe para executar. Observe que tivemos somente para navegar até */HelloWorld* e `Index` método foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador, se ainda não for explicitamente especificado.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres “Este é o método de ação Boas-vindas...”. O mapeamento de MVC padrão é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image7.png)

Vamos modificar o exemplo um pouco para que você pode passar algumas informações de parâmetro da URL para o controlador (por exemplo, *HelloWorld/boas-vindas? name = Scott&amp;numtimes = 4*). Alterar sua `Welcome` método incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o recurso de parâmetro opcional do c# para indicar que o `numTimes` parâmetro deve 1 como padrão se nenhum valor é passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Execute o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Você pode tentar valores diferentes para `name` e `numtimes` na URL. O sistema mapeia automaticamente os parâmetros nomeados da cadeia de consulta na barra de endereços para parâmetros em seu método.

![](adding-a-controller/_static/image8.png)

Nos dois exemplos do controlador está executando a parte "VC" do MVC — ou seja, o trabalho de exibição e controlador. O controlador está retornando HTML diretamente. Normalmente, você não deseja controladores retornando HTML diretamente, desde que se torna muito difícil de código. Em vez disso, usaremos normalmente um arquivo de modelo de exibição separada para ajudar a gerar a resposta HTML. Vamos Avançar como podemos fazer isso.

>[!div class="step-by-step"]
[Anterior](intro-to-aspnet-mvc-3.md)
[Próximo](adding-a-view.md)
