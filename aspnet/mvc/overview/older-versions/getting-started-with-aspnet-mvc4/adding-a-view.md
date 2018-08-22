---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Adicionando uma exibição | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 6244bfd96c547c5ccbcaed7ba17214df49d2886c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824591"
---
<a name="adding-a-view"></a>Adicionando uma exibição
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.


Nesta seção você irá modificar o `HelloWorldController` classe para usar a exibição de arquivos de modelo para corretamente encapsulam o processo de geração de respostas HTML para um cliente.

Você criará um arquivo de modelo de exibição usando o [mecanismo de exibição Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduzido com o ASP.NET MVC 3. Modelos de exibição baseado no Razor têm uma *. cshtml* extensão de arquivo e fornecem uma maneira elegante de criar o HTML de saída usando a linguagem c#. Razor minimiza o número de caracteres e pressionamentos de teclas necessários ao escrever um modelo de exibição e permite um rápido, fluido fluxo de trabalho de codificação.

Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Alterar o `Index` método para retornar um `View` do objeto, conforme mostrado no código a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

O `Index` método acima usa um modelo de exibição para gerar uma resposta HTML para o navegador. Métodos do controlador (também conhecido como [métodos de ação](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como o `Index` método acima, geralmente retornam um [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou uma classe derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), não os tipos primitivos como cadeia de caracteres.

No projeto, adicione um modelo de exibição que você pode usar com o `Index` método. Para fazer isso, clique com botão direito dentro do `Index` método e clique em **adicionar exibição**.

![](adding-a-view/_static/image1.png)

O **adicionar exibição** caixa de diálogo é exibida. Mantenha os padrões a maneira como eles são e clique no **adicionar** botão:

![](adding-a-view/_static/image2.png)

O *MvcMovie\Views\HelloWorld* pasta e o *MvcMovie\Views\HelloWorld\Index.cshtml* arquivos são criados. Você pode vê-los na **Gerenciador de soluções**:

![](adding-a-view/_static/image3.png)

A seguir mostra a *index. cshtml* arquivo que foi criado:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Adicione o seguinte HTML sob o `<h2>` marca.

[!code-html[Main](adding-a-view/samples/sample2.html)]

A conclusão *MvcMovie\Views\HelloWorld\Index.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Se você estiver usando o Visual Studio 2012, no Gerenciador de soluções, clique com botão direito do *index. cshtml* do arquivo e selecione **exibir no Page Inspector**.

![PI](adding-a-view/_static/image5.png)

O [tutorial do Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) possui mais informações sobre essa nova ferramenta.

Como alternativa, execute o aplicativo e navegue até a `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método no seu controlador de não fazer muito trabalho; ele simplesmente executou a instrução `return View()`, qual especificado que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta ao navegador. Porque você não especificar explicitamente o nome do arquivo de modelo de exibição para usar, ASP.NET MVC assumiu como padrão usando o *index. cshtml* arquivo de exibição a *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres &quot;Olá de nosso modelo de exibição!&quot; embutido em código no modo de exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador mostra &quot;meu ASP.NET A do índice&quot; e o link grande na parte superior da página informa &quot;seu logotipo aqui.&quot; Abaixo de &quot;seu logotipo aqui.&quot; link registro e log nos links e abaixo que se vincula à página inicial, sobre e contato páginas. Vamos alterar algumas dessas.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de Layout

Primeiro, você deseja alterar o &quot;seu logotipo aqui.&quot; título na parte superior da página. Se o texto é comum a todas as páginas. Na verdade, ele é implementado em apenas um lugar no projeto, mesmo que ela apareça em todas as páginas no aplicativo. Vá para o */Views/Shared* pasta **Gerenciador de soluções** e abra o  *\_layout. cshtml* arquivo. Esse arquivo é chamado um *página de layout* e é compartilhado &quot;shell&quot; que usam todas as outras páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelos de layout permitem que você especifique o layout de contêiner HTML do seu site em um só lugar e, em seguida, aplicá-la em várias páginas em seu site. Localize a linha `@RenderBody()`. `RenderBody` é um espaço reservado em que todas as páginas específicas à exibição criadas são mostradas, &quot;encapsuladas&quot; na página de layout. Por exemplo, se você selecionar o link de sobre o *Views\Home\About.cshtml* exibição é renderizada dentro de `RenderBody` método.

Altere o cabeçalho de título do site no modelo de layout &quot;seu logotipo aqui&quot; à &quot;filme MVC&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Substitua o conteúdo do elemento title com a seguinte marcação:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Execute o aplicativo e observe que agora você verá &quot;filme MVC &quot;. Clique o **sobre** link e você verá como essa página mostra &quot;filme MVC&quot;também. Fomos capazes de fazer a alteração uma vez no modelo de layout e ter todas as páginas no site de refletir o novo título.

![](adding-a-view/_static/image8.png)

Agora, vamos alterar o título da exibição índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o `<h2>` elemento). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Para indicar o título HTML para exibir o código acima define um `Title` propriedade do `ViewBag` objeto (que está na *index. cshtml* modelo de exibição). Se você olhar novamente o código-fonte do modelo de layout, você observará que o modelo usa esse valor na `<title>` elemento como parte do `<head>` seção do HTML que modificamos anteriormente. Usando este `ViewBag` abordagem, você pode facilmente passar outros parâmetros entre seu modelo de exibição e seu arquivo de layout.

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.) O título do navegador é criado com o `ViewBag.Title` definimos na *index. cshtml* exibir modelo e adicional &quot;-aplicativo de filme&quot; adicionados no arquivo de layout.

Observe também como o conteúdo na *index. cshtml* modelo de exibição foi mesclado com a  *\_layout. cshtml* modelo de exibição e uma única resposta HTML foi enviada ao navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image9.png)

Nosso pouco &quot;dados&quot; (nesse caso, o &quot;Olá de nosso modelo de exibição!&quot; mensagem) é embutido no código, no entanto. O aplicativo MVC tem um &quot;V&quot; (exibição) e você tem um &quot;C&quot; (controlador), mas nenhum &quot;M&quot; (modelo) ainda. Em breve, vamos examinar como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre como passar informações do controlador para um modo de exibição. As classes do controlador são invocadas em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que manipula o entrada navegador solicita, recupera dados de um banco de dados e, por fim, decide qual tipo de resposta será enviada ao navegador. Modelos de exibição, em seguida, podem ser usados em um controlador para gerar e formatar uma resposta HTML para o navegador.

Os controladores são responsáveis por fornecer quaisquer dados ou objetos são necessários para que um modelo de exibição renderizar uma resposta ao navegador. Uma prática recomendada: **um modelo de exibição nunca deve executar lógica de negócios ou interagir diretamente com um banco de dados**. Em vez disso, um modelo de exibição deve trabalhar somente com os dados que são fornecidos a ela pelo controlador. Manter isso &quot;separação de preocupações&quot; ajuda a manter seu código limpo, testável e mais sustentável.

No momento, o `Welcome` método de ação de `HelloWorldController` classe usa um `name` e um `numTimes` parâmetro e, em seguida, gera os valores diretamente para o navegador. Em vez de fazer com que o controlador renderize a resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição em vez disso. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador coloque os dados dinâmicos (parâmetros) que o modelo de exibição precisa em um `ViewBag` objeto que pode acessar o modelo de exibição.

Volte para o *HelloWorldController.cs* de arquivo e altere o `Welcome` para adicionar um `Message` e `NumTimes` valor para o `ViewBag` objeto. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar tudo que quiser a ele. o `ViewBag` objeto não tem nenhuma propriedade definida até que você insira algo dentro dele. O [sistema de associação de modelos do ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados (`name` e `numTimes`) da cadeia de consulta na barra de endereços para os parâmetros no método. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Agora o `ViewBag` objeto contém dados que serão passados para o modo de exibição automaticamente.

Em seguida, você precisa de um modelo de exibição bem-vindo! No **construir** menu, selecione **MvcMovie Build** para garantir que o projeto é compilado.

Em seguida, clique duas vezes dentro de `Welcome` método e clique em **adicionar exibição**.

![](adding-a-view/_static/image10.png)

Aqui está o que o **adicionar exibição** caixa de diálogo se parece com:

![](adding-a-view/_static/image11.png)

Clique em **Add**e, em seguida, adicione o seguinte código sob a `<h2>` elemento na nova *Welcome* arquivo. Você criará um loop que diz &quot;Hello&quot; quantas vezes o usuário disser que deveria. A conclusão *Welcome* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Execute o aplicativo e navegue até a URL a seguir:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora os dados são obtidos da URL e passados para o controlador usando o [associador de modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). O controlador empacota os dados em um `ViewBag` objeto e passa esse objeto para o modo de exibição. O modo de exibição, em seguida, exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image12.png)

No exemplo acima, usamos um `ViewBag` objeto para passar dados do controlador para um modo de exibição. Último no tutorial, usaremos um modelo de exibição para passar dados de um controlador para um modo de exibição. A abordagem de modelo de exibição para passar dados é geralmente a preferida em relação à abordagem de recipiente de propriedades de exibição. Consulte a entrada de blog [V fortemente tipados exibições dinâmicas](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obter mais informações.

Bem, isso foi uma espécie de uma &quot;M&quot; de modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
