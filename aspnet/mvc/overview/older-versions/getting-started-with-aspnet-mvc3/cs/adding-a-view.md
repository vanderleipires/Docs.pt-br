---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Adicionando uma exibição (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873164"
---
<a name="adding-a-view-c"></a>Adicionando uma exibição (c#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

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


Nesta seção você irá modificar o `HelloWorldController` classe para usar a exibição de arquivos de modelo para corretamente encapsulam o processo de geração de respostas HTML para um cliente.

Você criará um arquivo de modelo de exibição usando o novo [mecanismo de exibição Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduzido com o ASP.NET MVC 3. Modo de exibição baseado no Razor modelos têm um *. cshtml* extensão de arquivo e fornecem uma maneira elegante para criar HTML de saída usando c#. Razor minimiza o número de caracteres e pressionamentos de tecla necessários ao escrever um modelo de exibição e permite que um fluido rápido, fluxo de trabalho de codificação.

Iniciar com um modelo de exibição com o `Index` método o `HelloWorldController` classe. Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Alterar o `Index` método para retornar um `View` do objeto, conforme mostrado a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Esse código usa um modelo de exibição para gerar uma resposta HTML para o navegador. No projeto, adicione um modelo de exibição que você pode usar com o `Index` método. Para fazer isso, clique dentro do `Index` método e clique em **adicionar exibição**.

![](adding-a-view/_static/image1.png)

O **adicionar exibição** caixa de diálogo é exibida. Deixe o padrão é a maneira como eles são e clique no **adicionar** botão:

![](adding-a-view/_static/image2.png)

O *MvcMovie\Views\HelloWorld* pasta e o *MvcMovie\Views\HelloWorld\Index.cshtml* arquivos são criados. Você pode vê-los em **Solution Explorer**:

![](adding-a-view/_static/image3.png)

A seguir mostra o *cshtml* arquivo que foi criado:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Adicionar um HTML sob o `<h2>` marca. A modificação *MvcMovie\Views\HelloWorld\Index.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Execute o aplicativo e navegue até o `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método em seu controlador não fazer a quantidade de trabalho; ele simplesmente executou a instrução `return View()`, qual especificado que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Porque você não especificar explicitamente o nome do arquivo do modelo de exibição a ser usado, o ASP.NET MVC padrão usando o *cshtml* arquivo de exibição no *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres codificada no modo de exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador diz "Index" e grande título na página diz "Meu aplicativo MVC". Vamos alterar os.

## <a name="changing-views-and-layout-pages"></a>Alterando modos de exibição e páginas de Layout

Primeiro, você deseja alterar o título "Meu aplicativo MVC" na parte superior da página. Se o texto é comum a todas as páginas. Na verdade, é implementado em apenas um lugar no projeto, mesmo que ela apareça em todas as páginas do aplicativo. Vá para o */exibições/compartilhado* pasta **Solution Explorer** e abra o  *\_cshtml* arquivo. Esse arquivo é chamado um *página de layout* e é o compartilhado "shell" que usam todas as outras páginas.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Modelos de layout permitem que você especifique o layout de contêiner HTML do seu site em um local e, em seguida, aplicá-lo em várias páginas em seu site. Observe o `@RenderBody()` linha na parte inferior do arquivo. `RenderBody` é um espaço reservado em que todas as páginas de específica que você cria aparecem "encapsuladas" na página de layout. Altere o título do título do modelo de layout de "My Application de MVC" para "Aplicativo de filme MVC".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Execute o aplicativo e observe que agora diz "Aplicativo de filme MVC". Clique o **sobre** link e você verá como essa página mostra o "Aplicativo de filme MVC," muito. Fomos capazes de fazer a alteração de uma vez no modelo de layout e ter todas as páginas no site de refletir o novo título.

![](adding-a-view/_static/image9.png)

Completo  *\_cshtml* arquivo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Agora, vamos alterar o título da página de índice (exibição).

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho de secundário (o `<h2>` elemento). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Para indicar o título HTML para exibir o código acima define um `Title` propriedade do `ViewBag` objeto (que está no *cshtml* modelo de exibição). Se você examinar novamente o código-fonte do modelo de layout, você observará que o modelo usa esse valor no `<title>` elemento como parte do `<head>` seção do HTML. Usando essa abordagem, você pode passar outros parâmetros facilmente entre o modelo de exibição e o arquivo de layout.

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.)

Observe também como o conteúdo a *cshtml* modelo de exibição foi mesclado com a  *\_cshtml* modelo de exibição e uma única resposta HTML foi enviada para o navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image10.png)

Apesar disso, nossos poucos “dados” (nesse caso, a mensagem “Olá de nosso modelo de exibição!”) são embutidos em código. O aplicativo MVC tem um “V” (exibição) e você tem um “C” (controlador), mas ainda nenhum “M” (modelo). Em breve, examinaremos como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre passando informações do controlador para um modo de exibição. Classes do controlador é invocado em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que lida com os parâmetros de entrada, recupera dados de um banco de dados e, finalmente, decide que tipo de resposta para enviar de volta para o navegador. Modelos de exibição, em seguida, podem ser usados em um controlador para gerar e formatar uma resposta HTML para o navegador.

Controladores serão responsáveis por fornecer quaisquer dados ou objetos necessários para um modelo de exibição renderizar uma resposta para o navegador. Um modelo de exibição nunca deve executar lógica de negócios ou interagir diretamente com um banco de dados. Em vez disso, ele deve funcionar somente com os dados que são fornecidos pelo controlador. Manter essa "separação de preocupações" ajuda a manter o código de limpeza e passível de manutenção.

Atualmente, o `Welcome` método de ação de `HelloWorldController` classe leva um `name` e um `numTimes` parâmetro e, em seguida, os valores diretamente para o navegador de saídas. Em vez de fazer com que o controlador processar a resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição em vez disso. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador de colocar os dados dinâmicos que o modelo de exibição precisa um `ViewBag` objeto que pode acessar o modelo de exibição.

Retorne ao *HelloWorldController.cs* de arquivo e altere o `Welcome` método para adicionar um `Message` e `NumTimes` o valor para o `ViewBag` objeto. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar tudo o que você deseja o `ViewBag` objeto não tem nenhuma propriedade definida até que você insira algo dentro dele. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Agora o `ViewBag` objeto contém os dados que serão passados para o modo de exibição automaticamente.

Em seguida, você precisa de um modelo de exibição bem-vindo! No **depurar** menu, selecione **MvcMovie Build** para certificar-se de que o projeto é compilado.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Em seguida, clique dentro do `Welcome` método e clique em **adicionar exibição**. Aqui está o que o **adicionar exibição** caixa de diálogo aparência:

![](adding-a-view/_static/image13.png)

Clique em **adicionar**e, em seguida, adicione o seguinte código sob o `<h2>` elemento no novo *Welcome.cshtml* arquivo. Você criará um loop que diz "Hello" quantas vezes o usuário diz que deveria. Completo *Welcome.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Execute o aplicativo e navegue até a URL a seguir:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora dados é obtidos com a URL e passados para o controlador automaticamente. O controlador de pacotes de dados em um `ViewBag` objeto e etapas de objeto para o modo de exibição. O modo de exibição exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image14.png)

Bem, isso foi um tipo de “M” de modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
