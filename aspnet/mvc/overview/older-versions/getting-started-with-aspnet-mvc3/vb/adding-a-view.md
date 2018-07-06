---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Adicionando uma exibição (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 140dc8a7302c52112c2d8873325b2567e3ba66a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829966"
---
<a name="adding-a-view-vb"></a>Adicionando uma exibição (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o c#, alterne para o [c# versão](../cs/adding-a-view.md) deste tutorial.


Nesta seção, vamos modificar o `HelloWorldController` encapsular a classe para usar um arquivo de modelo de exibição para corretamente o processo de geração de respostas HTML para um cliente.

Vamos começar usando um modelo de exibição com o `Index` método no `HelloWorldController` classe. Atualmente, o `Index` método retorna uma cadeia de caracteres com uma mensagem que é embutido no código dentro da classe controller. Alterar o `Index` método para retornar um `View` objeto, conforme mostrado a seguir:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Agora, vamos adicionar um modelo de exibição ao nosso projeto que podemos invocar com o `Index` método. Para fazer isso, clique com botão direito dentro do `Index` método e clique em **adicionar exibição**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

O **adicionar exibição** caixa de diálogo é exibida. Deixe as entradas padrão e clique no **adicionar** botão.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

O *MvcMovie\Views\HelloWorld* pasta e o *MvcMovie\Views\HelloWorld\Index.vbhtml* arquivos são criados. Você pode vê-los na **Gerenciador de soluções**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Adicionar um HTML sob o `<h2>` marca. Modificado *MvcMovie\Views\HelloWorld\Index.vbhtml* arquivo é mostrado abaixo.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Execute o aplicativo e navegue até a &quot;Olá, mundo&quot; controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método no seu controlador de não fazer muito trabalho; ele simplesmente executou a instrução `return View()`, que indicou que queremos usar um arquivo de modelo de exibição para renderizar uma resposta ao cliente. Porque não especificamos explicitamente o nome do arquivo de modelo de exibição para usar, ASP.NET MVC assumiu como padrão usando o *Index.vbhtml* arquivo de exibição dentro a *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres embutidos em código no modo de exibição.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Parece muito bom. No entanto, observe que a barra de título do navegador diz &quot;índice&quot; e diz que o título grande na página &quot;meu aplicativo MVC.&quot; Vamos alterá-los.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Primeiro, vamos alterar o texto &quot;meu aplicativo MVC.&quot; Esse texto é compartilhado e aparece em cada página. Na verdade, aparece em apenas um lugar em nosso projeto, mesmo que seja em cada página em nosso aplicativo. Vá para o */Views/Shared* pasta **Gerenciador de soluções** e abra o  *\_Layout.vbhtml* arquivo. Esse arquivo é chamado de uma página de layout e é compartilhado &quot;shell&quot; que usam todas as outras páginas.

Observação o `@RenderBody()` linha de código na parte inferior do arquivo. `RenderBody` é um espaço reservado em que todas as páginas que você cria aparecem &quot;encapsulado&quot; na página de layout. Alterar o `<h1>` título do **&quot;** meu aplicativo MVC&quot; para &quot;aplicativo de filme MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Execute o aplicativo e observe que agora diz &quot;aplicativo de filme MVC&quot;. Clique o **sobre** link e que a página mostra &quot;aplicativo de filme MVC&quot;também.

A conclusão  *\_Layout.vbhtml* arquivo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Agora, vamos alterar o título da página de índice (exibição).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Abra *MvcMovie\Views\HelloWorld\Index.vbhtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o `<h2>` elemento). Faremos-los um pouco diferente para que você possa ver qual parte do código altera qual parte do aplicativo.

Execute o aplicativo e navegue até`http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. É fácil fazer grandes alterações em seu aplicativo com pequenas alterações para um modo de exibição. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nosso pouco &quot;dados&quot; (nesse caso, o &quot;Olá, mundo!&quot; mensagem) é embutido no código, no entanto. Nosso aplicativo MVC tem V (exibições) e que temos C (controladores), mas ainda não há M (modelo). Em breve, vamos examinar como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre como passar informações do controlador para um modo de exibição. Queremos passar o que requer um modelo de exibição para renderizar uma resposta HTML para um cliente. Esses objetos são normalmente criados e passados por uma classe de controlador para um modelo de exibição, e eles devem conter apenas os dados que exige que o modelo de exibição — e não mais.

Anteriormente com o `HelloWorldController` classe, o `Welcome` método de ação levou um `name` e um `numTimes` parâmetro e saída, em seguida, os valores de parâmetro para o navegador. Em vez disso, que tem o controlador de continuar a renderize a resposta diretamente, vamos em vez disso, vamos colocar esses dados em um recipiente para o modo de exibição. Controladores e modos de exibição podem usar um `ViewBag` objeto para manter esses dados. Que será passado um modelo de exibição automaticamente e usado para processar a resposta HTML usando o conteúdo do recipiente de dados. Dessa forma, o controlador está preocupado com uma coisa e o modelo de exibição com outro — que nos permite manter limpa &quot;separação de preocupações&quot; dentro do aplicativo.

Como alternativa, podemos pode definir uma classe personalizada, em seguida, criar uma instância desse objeto em nosso próprio, preenchê-lo com dados e passá-lo para o modo de exibição. Que muitas vezes é chamado um ViewModel, porque ele é um modelo personalizado para o modo de exibição. Para pequenas quantidades de dados, no entanto, ViewBag funciona muito bem.

Volte para o *HelloWorldController.vb* alteração do arquivo a `Welcome` método dentro de controlador para colocar a mensagem e NumTimes em ViewBag. A ViewBag é um objeto dinâmico. Isso significa que você pode colocar tudo que quiser para ele. A ViewBag não tem nenhuma propriedade definida até que você insira algo dentro dele.

Completo `HelloWorldController.vb` com a nova classe no mesmo arquivo.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Agora nosso ViewBag contém dados que serão passados ao longo para o modo de exibição automaticamente. Novamente, como alternativa, podemos foi aprovado em nosso próprio objeto como este se gostávamos:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Agora temos um `WelcomeView` modelo! Execute o aplicativo para que o novo código é compilado. Feche o navegador, clique com botão direito dentro do `Welcome` método e depois clique em **adicionar exibição**.

Aqui está o que seu **adicionar exibição** caixa de diálogo é semelhante.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Adicione o seguinte código sob o `<h2>` elemento no novo <em>boas-vindas.</em> arquivo vbhtml. Vamos fazer um loop e dizer &quot;Hello&quot; quantas vezes o usuário disser que devemos!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora os dados são obtidos da URL e passados para o controlador automaticamente. O controlador empacota os dados em um `Model` objeto e passa esse objeto para o modo de exibição. O modo de exibição que exibe os dados como HTML para o usuário.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bem, isso foi uma espécie de uma &quot;M&quot; de modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
