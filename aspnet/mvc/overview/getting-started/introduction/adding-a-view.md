---
title: Adicionar uma exibição em um aplicativo MVC
author: Rick-Anderson
description: Adicionar uma exibição em um aplicativo MVC
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 21db97e635b5db580df31f46ca7f8b60a80d6f94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view"></a>Adicionando uma exibição
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Nesta seção você irá modificar o `HelloWorldController` classe para usar a exibição de arquivos de modelo para corretamente encapsulam o processo de geração de respostas HTML para um cliente. 

Você criará um arquivo de modelo de exibição usando o [mecanismo de exibição Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Modo de exibição baseado no Razor modelos têm um *. cshtml* extensão de arquivo e fornecem uma maneira elegante para criar HTML de saída usando c#. Razor minimiza o número de caracteres e pressionamentos de tecla necessários ao escrever um modelo de exibição e permite que um fluido rápido, fluxo de trabalho de codificação.

Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Alterar o `Index` método para retornar um `View` do objeto, como mostrado no código a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

O `Index` método acima usa um modelo de exibição para gerar uma resposta HTML para o navegador. Os métodos do controlador (também conhecido como [métodos de ação](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como o `Index` método acima, normalmente retornam um [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou uma classe derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), os tipos primitivos não como cadeia de caracteres.

Clique com botão direito do *Views\HelloWorld* pasta e clique em **adicionar**, em seguida, clique em **página de exibição MVC 5 com Layout (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
No **especificar nome para o Item** caixa de diálogo, digite *índice*e, em seguida, clique em **Okey**.  
  
![](adding-a-view/_static/image2.png)  
  
No **selecionar uma página de Layout** caixa de diálogo, aceite o padrão  **\_cshtml** e clique em **Okey**.  
  
![](adding-a-view/_static/image3.png)  
  
Na caixa de diálogo acima, o *exibições \ compartilhadas* pasta está selecionada no painel esquerdo. Se você tiver um arquivo de layout personalizado em outra pasta, você pode selecionar a ele. Falaremos sobre o arquivo de layout no tutorial posteriormente

O *MvcMovie\Views\HelloWorld\Index.cshtml* arquivo é criado.

![](adding-a-view/_static/image4.png)

Adicione a seguinte marcação realçada.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Clique com botão direito do *cshtml* de arquivo e selecione **exibir no navegador**.

![PI](adding-a-view/_static/image5.png)

Você pode também clique com botão direito do *cshtml* de arquivo e selecione **exibir em Inspetor de página.** Consulte o [tutorial do Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) para obter mais informações.

Como alternativa, execute o aplicativo e navegue até o `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método em seu controlador não fazer a quantidade de trabalho; ele simplesmente executou a instrução `return View()`, qual especificado que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Porque você não especificar explicitamente o nome do arquivo do modelo de exibição a ser usado, o ASP.NET MVC padrão usando o *cshtml* arquivo de exibição no *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres &quot;Olá do nosso modelo de exibição!&quot; embutidos em código no modo de exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador mostra &quot;índice - meu aplicativo do ASP.NET "e o link grande no topo da página diz"Nome do aplicativo". Dependendo de como as pequenas que a janela do navegador, talvez seja necessário clicar em três barras no canto superior direito para ver o para o **início**, **sobre**, **entre em contato com**, **Registrar** e **login** links.

## <a name="changing-views-and-layout-pages"></a>Alterando modos de exibição e páginas de Layout

Primeiro, você deseja alterar o &quot;nome do aplicativo&quot; link na parte superior da página. Se o texto é comum a todas as páginas. Na verdade é implementado em um só lugar no projeto, mesmo que ela apareça em todas as páginas do aplicativo. Vá para o */exibições/compartilhado* pasta **Solution Explorer** e abra o  *\_cshtml* arquivo. Esse arquivo é chamado um *página de layout* e ele está na pasta compartilhada que usam todas as outras páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelos de layout permitem que você especifique o layout de contêiner HTML do seu site em um local e, em seguida, aplicá-lo em várias páginas em seu site. Localize a linha `@RenderBody()`. `RenderBody` é um espaço reservado em que todas as páginas específicas à exibição criadas são mostradas, &quot;encapsuladas&quot; na página de layout. Por exemplo, se você selecionar o **sobre** link, o *Views\Home\About.cshtml* exibição é renderizada dentro de `RenderBody` método.

Altere o conteúdo do elemento de título. Alterar o [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) no modelo de layout de &quot;nome do aplicativo&quot; para &quot;MVC filme&quot; e do controlador do `Home` para `Movies`. O arquivo de layout concluída é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Execute o aplicativo e observe que agora diz &quot;MVC filme &quot;. Clique o **sobre** link e você verá como essa página mostra &quot;MVC filme&quot;também. Fomos capazes de fazer a alteração de uma vez no modelo de layout e ter todas as páginas no site de refletir o novo título.

![](adding-a-view/_static/image8.png)

Quando é criado pela primeira vez o *Views\HelloWorld\Index.cshtml* arquivo continha o código a seguir:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

O código do Razor acima é definir explicitamente a página de layout. Examine o *exibições\\viewstart* arquivo, ele contém a mesma marcação Razor exata. O *[exibições\\viewstart](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* arquivo define o layout comuns que todas as exibições usarão, portanto você pode comentar ou remova esse código da *Views\HelloWorld\ Cshtml* arquivo.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Use a propriedade `Layout` para definir outra exibição de layout ou defina-a como `null` para que nenhum arquivo de layout seja usado.

Agora, vamos alterar o título da exibição de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho de secundário (o `<h2>` elemento). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Para indicar o título HTML para exibir o código acima define um `Title` propriedade do `ViewBag` objeto (que está no *cshtml* modelo de exibição). Observe que o modelo de layout ( *exibições \ compartilhadas\\cshtml* ) usa esse valor no `<title>` elemento como parte do `<head>` seção do HTML que modificamos anteriormente.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Usando esse `ViewBag` abordagem, você pode facilmente passar outros parâmetros entre o modelo de exibição e o arquivo de layout.

Execute o aplicativo. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.) O título do navegador é criado com o `ViewBag.Title` definimos *cshtml* exibir modelo e adicional &quot;-aplicativo de filme&quot; adicionados ao arquivo de layout.

Observe também como o conteúdo a *cshtml* modelo de exibição foi mesclado com a  *\_cshtml* modelo de exibição e uma única resposta HTML foi enviada para o navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image9.png)

Nosso pequeno &quot;dados&quot; (nesse caso o &quot;Olá do nosso modelo de exibição de!&quot; mensagem) é inserido no código, embora. O aplicativo MVC tem um &quot;V&quot; (exibição) e você terá uma &quot;C&quot; (controlador), mas nenhum &quot;M&quot; (modelo) ainda. Em breve, examinaremos como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre passando informações do controlador para um modo de exibição. Classes do controlador é invocado em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que manipula o entrada navegador solicita, recupera dados de um banco de dados e, por fim, decide que tipo de resposta para enviar de volta para o navegador. Modelos de exibição, em seguida, podem ser usados em um controlador para gerar e formatar uma resposta HTML para o navegador.

Controladores serão responsáveis por fornecer quaisquer dados ou objetos necessários para um modelo de exibição renderizar uma resposta para o navegador. Uma prática recomendada: **um modelo de exibição nunca deve executar lógica de negócios ou interagir diretamente com um banco de dados**. Em vez disso, um modelo de exibição deve funcionar somente com os dados que são fornecidos pelo controlador. Manter isso &quot;separação de preocupações&quot; ajuda a manter seu código limpo, teste e mais fácil manutenção.

Atualmente, o `Welcome` método de ação de `HelloWorldController` classe leva um `name` e um `numTimes` parâmetro e, em seguida, os valores diretamente para o navegador de saídas. Em vez de fazer com que o controlador processar a resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição em vez disso. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador de colocar os dados dinâmicos (parâmetros) que o modelo de exibição precisa um `ViewBag` objeto que pode acessar o modelo de exibição.

Retorne ao *HelloWorldController.cs* de arquivo e altere o `Welcome` método para adicionar um `Message` e `NumTimes` o valor para o `ViewBag` objeto. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar tudo o que você deseja o `ViewBag` objeto não tem nenhuma propriedade definida até que você insira algo dentro dele. O [sistema de associação do ASP.NET MVC modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados (`name` e `numTimes`) da cadeia de consulta na barra de endereços para parâmetros em seu método. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Agora o `ViewBag` objeto contém os dados que serão passados para o modo de exibição automaticamente. Em seguida, você precisa de um modelo de exibição bem-vindo! No **criar** menu, selecione **compilar solução** (ou Ctrl + Shift + B) para certificar-se de que o projeto é compilado. Clique com botão direito do *Views\HelloWorld* pasta e clique em **adicionar**, em seguida, clique em **página de exibição MVC 5 com Layout (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
No **especificar nome para o Item** caixa de diálogo, digite *bem-vindo*e, em seguida, clique em **Okey**.   
  
No **selecionar uma página de Layout** caixa de diálogo, aceite o padrão  **\_cshtml** e clique em **Okey**.  
  
![](adding-a-view/_static/image11.png)   

O *MvcMovie\Views\HelloWorld\Welcome.cshtml* arquivo é criado.

Substitua a marcação no *Welcome.cshtml* arquivo. Você criará um loop que diz &quot;Hello&quot; quantas vezes o usuário diz que deveria. Completo *Welcome.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Execute o aplicativo e navegue até a URL a seguir:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora os dados é feitos da URL e passados para o controlador usando o [associador de modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). O controlador de pacotes de dados em um `ViewBag` objeto e etapas de objeto para o modo de exibição. O modo de exibição exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image12.png)

No exemplo acima, usamos um `ViewBag` objeto para passar dados do controlador para um modo de exibição. Mais adiante no tutorial, usaremos um modelo de exibição para passar dados de um controlador para uma exibição. A abordagem de modelo de exibição para passar dados é geralmente muito preferível a abordagem de recipiente de propriedades de exibição. Consulte a postagem do blog [V fortemente tipado exibições dinâmicas](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obter mais informações. 

Bem, isso foi um tipo de um &quot;M&quot; para modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
