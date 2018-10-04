---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Adicionando um controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576217"
---
<a name="adding-a-controller-vb"></a>Adicionando um controlador (VB)
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o c#, alterne para o [c# versão](../cs/adding-a-controller.md) deste tutorial.


Representa o MVC *model-view-controller*. O MVC é um padrão para o desenvolvimento de aplicativos, de modo que cada parte tem uma responsabilidade separada:

- Modelo: Os dados para seu aplicativo.
- Modos de exibição: Os arquivos de modelo seu aplicativo usará para gerar dinamicamente respostas HTML.
- Controladores: Classes que lidam com solicitações de URL de entrada para o aplicativo recuperar dados de modelo e, em seguida, especificar modelos de exibição que renderizam uma resposta ao cliente.

Vamos abordar todos esses conceitos neste tutorial e mostrar como usá-los para criar um aplicativo.

Criar um novo controlador clicando com o *controladores* pasta **Gerenciador de soluções** e, em seguida, selecionando **Adicionar controlador**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nomeie o novo controlador &quot;HelloWorldController&quot; e clique em **Add**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Observe na **Gerenciador de soluções** à direita que um novo arquivo foi criado para você denominado *HelloWorldController.cs* e que o arquivo está aberto no IDE.

Dentro do novo `public class HelloWorldController` bloquear, crie dois novos métodos que se parecem com o código a seguir. Vamos retornar uma cadeia de caracteres de HTML diretamente no controlador como um exemplo.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

O controlador é denominado `HelloWorldController` e o novo método é chamado `Index`. Execute o aplicativo (pressione F5 ou Ctrl + F5). Depois de iniciar seu navegador, acrescente &quot;HelloWorld&quot; para o caminho na barra de endereços. (No meu computador, ele tem `http://localhost:43246/HelloWorld`) seu navegador será semelhante a captura de tela abaixo. No método acima, o código retornado diretamente uma cadeia de caracteres. Nós dissemos que o sistema para retornar apenas um HTML e fez isso!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca as classes de controlador diferente (e diferentes métodos de ação dentro delas), dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para controlar qual código é invocado:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe de controlador para executar. Portanto, */HelloWorld* mapeia para o `HelloWorldController` classe. A segunda parte da URL determina o método de ação na classe para executar. Portanto, */HelloWorld/Index* faria com que o `Index` método da `HelloWorldController` classe para executar. Observe que precisamos visitar */HelloWorld* acima e o `Index` método foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O `Welcome` método é executado e retorna a cadeia de caracteres &quot;esse é o método de ação boas-vindas... &quot;. O mapeamento do padrão MVC é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador está `HelloWorld` e `Welcome` é o método. Ainda não usamos o `[Parameters]` faz parte da URL.

![](adding-a-controller/_static/image6.png)

Vamos modificar o exemplo um pouco para que o que podemos passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? nome = Scott&amp;numtimes = 4*). Alterar sua `Welcome` método para incluir dois parâmetros, conforme mostrado abaixo. Observe que usamos o recurso de parâmetro opcional do VB para indicar que o `numTimes` parâmetro como padrão 1 se nenhum valor é passado para esse parâmetro.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Executar o aplicativo e navegue até `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Você pode tentar valores diferentes para `name` e `numtimes`. O sistema mapeia automaticamente os parâmetros nomeados da sua cadeia de caracteres de consulta na barra de endereços para os parâmetros no método.

![](adding-a-controller/_static/image7.png)

Nos dois exemplos do controlador faz a parte do VC do MVC — que é o trabalho de exibição e controlador. O controlador retorna o HTML diretamente. Normalmente, não queremos que os controladores retornem HTML diretamente, já que isso é muito difícil para o código. Em vez disso, vamos normalmente usar um arquivo de modelo de exibição separada para ajudar a gerar a resposta HTML. Vamos examinar como é possível fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Próximo](adding-a-view.md)
