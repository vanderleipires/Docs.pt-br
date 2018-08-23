---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Adicionando um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d5cc9db7b1eec139a37afb6427fd761342fcc1f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825002"
---
<a name="adding-a-controller"></a>Adicionando um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Representa o MVC *model-view-controller*. O MVC é um padrão para o desenvolvimento de aplicativos que são bem arquitetado, testável e fácil de manter. Aplicativos baseados no MVC contêm:

- **M** odels: Classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócios para que os dados.
- **V** iews: arquivos de modelo que seu aplicativo usa para gerar dinamicamente respostas HTML.
- **C** ontrollers: Classes que lidam com solicitações recebidas do navegador, recuperar dados de modelo e, em seguida, especificar modelos de exibição que retornam uma resposta ao navegador.

Vamos abordar todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.

> [!NOTE]
> Na etapa anterior, o padrão MVC modelo foi selecionado. Isso cria os controladores (controllers) Home, Account e Manage por padrão.

Vamos começar criando uma classe de controlador. No **Gerenciador de soluções**, com o botão direito a *controladores* pasta e clique **adicionar**, em seguida, **controlador**.


![](adding-a-controller/_static/image1.png)

No **adicionar Scaffold** caixa de diálogo, clique em **controlador MVC 5 - vazio**e, em seguida, clique em **adicionar**.

![](adding-a-controller/_static/image2.png)  
 

Nomeie o novo controlador "HelloWorldController" e clique em **adicionar**.

![Adicionar controlador](adding-a-controller/_static/image3.png)

Observe na **Gerenciador de soluções** que um novo arquivo tiver sido criado com o nome *HelloWorldController.cs* e uma nova pasta *Views\HelloWorld*. O controlador está aberto no IDE.

![](adding-a-controller/_static/image4.png)

Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Os métodos do controlador retornará uma cadeia de caracteres de HTML como um exemplo. O controlador é denominado `HelloWorldController` e o primeiro método é chamado `Index`. Vamos chamá-la em um navegador. Execute o aplicativo (pressione F5 ou Ctrl + F5). No navegador, acrescente &quot;HelloWorld&quot; para o caminho na barra de endereços. (Por exemplo, na ilustração abaixo, é `http://localhost:1234/HelloWorld.`) a página no navegador se parecerá com a seguinte captura de tela. No método acima, o código retornado diretamente uma cadeia de caracteres. Você disse que o sistema para retornar apenas um HTML e fez isso!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca as classes de controlador diferente (e diferentes métodos de ação dentro delas), dependendo da URL de entrada. A lógica de roteamento de URL do padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código para invocar:

`/[Controller]/[ActionName]/[Parameters]`

Você define o formato de roteamento na *App\_Start/RouteConfig.cs* arquivo.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Quando você executa o aplicativo e não fornece nenhum segmento de URL, o padrão é o controlador "Home" e o método de ação de "Index" especificado na seção de padrões de código acima.

A primeira parte da URL determina a classe de controlador para executar. Portanto, */HelloWorld* mapeia para o `HelloWorldController` classe. A segunda parte da URL determina o método de ação na classe para executar. Portanto, */HelloWorld/Index* faria com que o `Index` método da `HelloWorldController` classe para executar. Observe que precisamos navegar até */HelloWorld* e o `Index` método foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente. A terceira parte do segmento de URL (`Parameters`) refere-se aos dados de rota. Veremos rotear os dados mais tarde neste tutorial.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O `Welcome` método é executado e retorna a cadeia de caracteres &quot;esse é o método de ação boas-vindas... &quot;. O mapeamento do padrão MVC é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image6.png)

Vamos modificar o exemplo um pouco para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? nome = Scott&amp;numtimes = 4*). Alterar sua `Welcome` método para incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o recurso de parâmetro opcional do c# para indicar que o `numTimes` parâmetro como padrão 1 se nenhum valor é passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Observação de segurança: O código acima usa [HttpUtility](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger o aplicativo contra entrada mal-intencionada (ou seja, JavaScript). Para obter mais informações, consulte [como: proteger contra scripts maliciosos em um aplicativo Web aplicando codificação HTML em cadeias de caracteres](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Executar o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Você pode tentar valores diferentes para `name` e `numtimes` na URL. O [sistema de associação de modelos do ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados da cadeia de consulta na barra de endereços para os parâmetros no método.

![](adding-a-controller/_static/image7.png)

No exemplo acima, o segmento de URL ( `Parameters`) não for usado, o `name` e `numTimes` parâmetros são passados como [cadeias de caracteres de consulta](http://en.wikipedia.org/wiki/Query_string). O caractere curinga ? (ponto de interrogação) na URL anterior é um separador e siga as cadeias de caracteres de consulta. O caractere &amp; separa as cadeias de consulta.

Substitua o método de boas-vindo com o código a seguir:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Execute o aplicativo e insira a seguinte URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Neste momento, o terceiro segmento de URL correspondente o parâmetro de rota `ID.` as `Welcome` método de ação contém um parâmetro (`ID`) que correspondeu a especificação de URL no `RegisterRoutes` método.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Em aplicativos ASP.NET MVC, é mais comum para passar parâmetros como os dados de rota (como fizemos com a ID acima) que passá-los como cadeias de caracteres de consulta. Você também pode adicionar uma rota para passar a ambos os `name` e `numtimes` em parâmetros como dados de rota na URL. No *App\_Start\RouteConfig.cs* de arquivo, adicione a rota "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Execute o aplicativo e navegue até `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Para muitos aplicativos MVC, a rota padrão funciona bem. Você aprenderá posteriormente no tutorial para passar dados usando o associador de modelo, e você não precisará modificar a rota padrão para fazer isso.

Nesses exemplos de controlador faz o &quot;VC&quot; parte do MVC — ou seja, o trabalho de exibição e controlador. O controlador retorna o HTML diretamente. Normalmente, você não quer controladores retornem HTML diretamente, já que isso é muito difícil para o código. Em vez disso, vamos normalmente usar um arquivo de modelo de exibição separada para ajudar a gerar a resposta HTML. Vamos dar uma olhada próxima em como é possível fazer isso.

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Próximo](adding-a-view.md)
