---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Adicionando uma exibição (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: c9675eb7776116ecbe910d5515abfe9b4391df22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-vb"></a>Adicionando uma exibição (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com VB.NET código-fonte está disponível para acompanhar este tópico. [Baixe a versão VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir c#, alterne para o [versão c#](../cs/adding-a-view.md) deste tutorial.


Nesta seção, vamos modificar o `HelloWorldController` classe para usar um arquivo de modelo de exibição para corretamente encapsulam o processo de geração de respostas HTML para um cliente.

Vamos começar usando um modelo de exibição com o `Index` método o `HelloWorldController` classe. Atualmente o `Index` método retorna uma cadeia de caracteres com uma mensagem que é embutido dentro da classe do controlador. Alterar o `Index` método para retornar um `View` do objeto, conforme mostrado a seguir:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Vamos adicionar agora um modelo de exibição ao nosso projeto que pode ser chamado com o `Index` método. Para fazer isso, clique dentro do `Index` método e clique em **adicionar exibição**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

O **adicionar exibição** caixa de diálogo é exibida. Deixe as entradas padrão e clique no **adicionar** botão.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

O *MvcMovie\Views\HelloWorld* pasta e o *MvcMovie\Views\HelloWorld\Index.vbhtml* arquivos são criados. Você pode vê-los em **Solution Explorer**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Adicionar um HTML sob o `<h2>` marca. A modificação *MvcMovie\Views\HelloWorld\Index.vbhtml* arquivo é mostrado abaixo.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Execute o aplicativo e navegue até o &quot;Olá, mundo&quot; controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método em seu controlador não fazer a quantidade de trabalho; ele simplesmente executou a instrução `return View()`, qual indicado que queremos usar um arquivo de modelo de exibição para renderizar uma resposta ao cliente. Porque estamos não especificar explicitamente o nome do arquivo do modelo de exibição a ser usado, o ASP.NET MVC padrão usando o *Index.vbhtml* Exibir arquivo dentro de *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres codificada no modo de exibição.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Parece muito bom. No entanto, observe que a barra de título do navegador diz &quot;índice&quot; e informa o título grande na página &quot;meu aplicativo MVC.&quot; Vamos alterar os.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Primeiro, vamos alterar o texto &quot;meu aplicativo MVC.&quot; Esse texto é compartilhado e aparece em cada página. Na verdade, aparece em apenas um local em nosso projeto, mesmo que seja em cada página em nosso aplicativo. Vá para o */exibições/compartilhado* pasta **Solution Explorer** e abra o  *\_Layout.vbhtml* arquivo. Esse arquivo é chamado de uma página de layout e é compartilhado &quot;shell&quot; que usam todas as outras páginas.

Observe o `@RenderBody()` linha de código na parte inferior do arquivo. `RenderBody` é um espaço reservado em que todas as páginas que você cria aparecem &quot;encapsulado&quot; na página de layout. Alterar o `<h1>` título do **&quot;** meu aplicativo MVC&quot; para &quot;aplicativo de filme MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Execute o aplicativo e Observe agora diz &quot;aplicativo de filme MVC&quot;. Clique o **sobre** link e que mostra a página &quot;aplicativo de filme MVC&quot;também.

Completo  *\_Layout.vbhtml* arquivo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Agora, vamos alterar o título da página de índice (exibição).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Abra *MvcMovie\Views\HelloWorld\Index.vbhtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho de secundário (o `<h2>` elemento). Verifique-os ligeiramente diferentes para ver qual parte do código altera qual parte do aplicativo.

Execute o aplicativo e navegue até`http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. É fácil fazer grandes alterações em seu aplicativo com pequenas alterações para um modo de exibição. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nosso pequeno &quot;dados&quot; (nesse caso o &quot;Olá, mundo!&quot; mensagem) é inserido no código, embora. Nosso aplicativo MVC tem V (views), e temos C (controladores), mas ainda não M (modelo). Em breve, examinaremos como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre passando informações do controlador para um modo de exibição. Queremos passar o que requer um modelo de exibição para renderizar uma resposta HTML para um cliente. Esses objetos são normalmente criados e passados por uma classe de controlador para um modelo de exibição, e eles devem conter apenas os dados que o modelo de exibição requer — e não mais.

Anteriormente com o `HelloWorldController` classe, o `Welcome` método de ação levou um `name` e um `numTimes` parâmetro e saída, em seguida, os valores de parâmetro para o navegador. Em vez de ter o controlador de continuar a processar essa resposta diretamente, vamos em vez disso, colocaremos esses dados em um recipiente para o modo de exibição. Controladores e exibições podem usar um `ViewBag` objeto para armazenar dados. Que será passado um modelo de exibição automaticamente e usada para processar a resposta HTML usando o conteúdo do conjunto de dados. Dessa forma, o controlador está preocupado com algo e o modelo de exibição com outro — que nos permite manter limpa &quot;separação de preocupações&quot; dentro do aplicativo.

Como alternativa, podemos pode definir uma classe personalizada, em seguida, criar uma instância do objeto em nossa própria, preenchê-lo com dados e passá-lo para o modo de exibição. Que é geralmente chamado um ViewModel, porque ele é um modelo personalizado para o modo de exibição. Para pequenas quantidades de dados, no entanto, a ViewBag funciona muito bem.

Volte para o *HelloWorldController.vb* alteração do arquivo de `Welcome` método dentro do controlador para colocar a mensagem e NumTimes em ViewBag. A ViewBag é um objeto dinâmico. Isso significa que você pode colocar tudo o que você quiser ele. A ViewBag tem propriedades não definidas até que você insira algo dentro dele.

Completo `HelloWorldController.vb` com a nova classe no mesmo arquivo.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Agora nosso ViewBag contém dados que serão passados para o modo de exibição automaticamente. Novamente, como alternativa, poderia passaram no nosso próprio objeto assim se podemos gostou:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Agora precisamos de um `WelcomeView` modelo! Execute o aplicativo para que o novo código é compilado. Feche o navegador, clique dentro de `Welcome` método e depois clique em **adicionar modo de exibição**.

Aqui está o que seu **adicionar exibição** aparência de caixa de diálogo.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Adicione o seguinte código sob o `<h2>` elemento no novo <em>bem-vindo.</em> arquivo vbhtml. Vamos fazer um loop e dizer &quot;Hello&quot; quantas vezes o usuário diz que deveríamos!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora dados é obtidos com a URL e passados para o controlador automaticamente. O controlador de pacotes de dados em um `Model` objeto e etapas de objeto para o modo de exibição. O modo de exibição que exibe os dados como HTML para o usuário.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bem, isso foi um tipo de um &quot;M&quot; para modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
