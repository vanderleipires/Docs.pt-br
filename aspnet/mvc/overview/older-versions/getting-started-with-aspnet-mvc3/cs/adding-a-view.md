---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Adicionando uma exibição (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576606"
---
<a name="adding-a-view-c"></a>Adicionando uma exibição (c#)
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

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


Nesta seção você irá modificar o `HelloWorldController` classe para usar a exibição de arquivos de modelo para corretamente encapsulam o processo de geração de respostas HTML para um cliente.

Você criará um arquivo de modelo de exibição usando o novo [mecanismo de exibição Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduzido com o ASP.NET MVC 3. Modelos de exibição baseado no Razor têm uma *. cshtml* extensão de arquivo e fornecem uma maneira elegante de criar o HTML de saída usando a linguagem c#. Razor minimiza o número de caracteres e pressionamentos de teclas necessários ao escrever um modelo de exibição e permite um rápido, fluido fluxo de trabalho de codificação.

Iniciar, usando um modelo de exibição com o `Index` método no `HelloWorldController` classe. Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Alterar o `Index` método para retornar um `View` objeto, conforme mostrado a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Esse código usa um modelo de exibição para gerar uma resposta HTML para o navegador. No projeto, adicione um modelo de exibição que você pode usar com o `Index` método. Para fazer isso, clique com botão direito dentro do `Index` método e clique em **adicionar exibição**.

![](adding-a-view/_static/image1.png)

O **adicionar exibição** caixa de diálogo é exibida. Mantenha os padrões a maneira como eles são e clique no **adicionar** botão:

![](adding-a-view/_static/image2.png)

O *MvcMovie\Views\HelloWorld* pasta e o *MvcMovie\Views\HelloWorld\Index.cshtml* arquivos são criados. Você pode vê-los na **Gerenciador de soluções**:

![](adding-a-view/_static/image3.png)

A seguir mostra a *index. cshtml* arquivo que foi criado:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Adicionar um HTML sob o `<h2>` marca. Modificado *MvcMovie\Views\HelloWorld\Index.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Execute o aplicativo e navegue até a `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método no seu controlador de não fazer muito trabalho; ele simplesmente executou a instrução `return View()`, qual especificado que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta ao navegador. Porque você não especificar explicitamente o nome do arquivo de modelo de exibição para usar, ASP.NET MVC assumiu como padrão usando o *index. cshtml* arquivo de exibição a *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres embutidos em código no modo de exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador diz "Index" e grande título na página diz "Meu aplicativo MVC." Vamos alterá-los.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de Layout

Primeiro, você deseja alterar o título de "Meu aplicativo MVC" na parte superior da página. Se o texto é comum a todas as páginas. Ele realmente é implementado em apenas um lugar no projeto, mesmo que ela apareça em todas as páginas no aplicativo. Vá para o */Views/Shared* pasta **Gerenciador de soluções** e abra o  *\_layout. cshtml* arquivo. Esse arquivo é chamado um *página de layout* e é o compartilhado "shell" que usam todas as outras páginas.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Modelos de layout permitem que você especifique o layout de contêiner HTML do seu site em um só lugar e, em seguida, aplicá-la em várias páginas em seu site. Observação o `@RenderBody()` linha na parte inferior do arquivo. `RenderBody` é um espaço reservado em que todas as páginas de específica que você cria são exibidos, "encapsuladas" na página de layout. Altere o cabeçalho de título no modelo de layout de "Meu aplicativo de MVC" para "Aplicativo de filme MVC".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Executar o aplicativo e observe que agora diz "Aplicativo de filme MVC". Clique o **sobre** link e você verá como essa página mostra o "Aplicativo de filme MVC", também. Fomos capazes de fazer a alteração uma vez no modelo de layout e ter todas as páginas no site de refletir o novo título.

![](adding-a-view/_static/image9.png)

A conclusão  *\_layout. cshtml* arquivo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Agora, vamos alterar o título da página de índice (exibição).

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o `<h2>` elemento). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Para indicar o título HTML para exibir o código acima define um `Title` propriedade do `ViewBag` objeto (que está na *index. cshtml* modelo de exibição). Se você olhar novamente o código-fonte do modelo de layout, você observará que o modelo usa esse valor na `<title>` elemento como parte do `<head>` seção do HTML. Usando essa abordagem, você pode passar outros parâmetros facilmente entre seu modelo de exibição e seu arquivo de layout.

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.)

Observe também como o conteúdo na *index. cshtml* modelo de exibição foi mesclado com a  *\_layout. cshtml* modelo de exibição e uma única resposta HTML foi enviada ao navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image10.png)

Apesar disso, nossos poucos “dados” (nesse caso, a mensagem “Olá de nosso modelo de exibição!”) são embutidos em código. O aplicativo MVC tem um “V” (exibição) e você tem um “C” (controlador), mas ainda nenhum “M” (modelo). Em breve, vamos examinar como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre como passar informações do controlador para um modo de exibição. As classes do controlador são invocadas em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que lida com os parâmetros de entrada, recupera dados de um banco de dados e, por fim, decide qual tipo de resposta será enviada ao navegador. Modelos de exibição, em seguida, podem ser usados em um controlador para gerar e formatar uma resposta HTML para o navegador.

Os controladores são responsáveis por fornecer quaisquer dados ou objetos são necessários para que um modelo de exibição renderizar uma resposta ao navegador. Um modelo de exibição nunca deve executar lógica de negócios ou interagir diretamente com um banco de dados. Em vez disso, ele deve funcionar somente com os dados que são fornecidos a ele pelo controlador. Manter essa "separação de preocupações" ajuda a manter seu código limpo e mais sustentável.

No momento, o `Welcome` método de ação de `HelloWorldController` classe usa um `name` e um `numTimes` parâmetro e, em seguida, gera os valores diretamente para o navegador. Em vez de fazer com que o controlador renderize a resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição em vez disso. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador coloque os dados dinâmicos que o modelo de exibição precisa em um `ViewBag` objeto que pode acessar o modelo de exibição.

Volte para o *HelloWorldController.cs* de arquivo e altere o `Welcome` para adicionar um `Message` e `NumTimes` valor para o `ViewBag` objeto. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar tudo que quiser a ele. o `ViewBag` objeto não tem nenhuma propriedade definida até que você insira algo dentro dele. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Agora o `ViewBag` objeto contém dados que serão passados para o modo de exibição automaticamente.

Em seguida, você precisa de um modelo de exibição bem-vindo! No **Debug** menu, selecione **MvcMovie Build** para garantir que o projeto é compilado.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Em seguida, clique duas vezes dentro de `Welcome` método e clique em **adicionar exibição**. Aqui está o que o **adicionar exibição** caixa de diálogo se parece com:

![](adding-a-view/_static/image13.png)

Clique em **Add**e, em seguida, adicione o seguinte código sob a `<h2>` elemento na nova *Welcome* arquivo. Você criará um loop que diz "Hello" quantas vezes o usuário disser que deveria. A conclusão *Welcome* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Execute o aplicativo e navegue até a URL a seguir:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora os dados são obtidos da URL e passados para o controlador automaticamente. O controlador empacota os dados em um `ViewBag` objeto e passa esse objeto para o modo de exibição. O modo de exibição, em seguida, exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image14.png)

Bem, isso foi um tipo de “M” de modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
