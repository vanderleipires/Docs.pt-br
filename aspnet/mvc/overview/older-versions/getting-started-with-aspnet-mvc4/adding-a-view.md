---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: "Adicionando uma exibição | Microsoft Docs"
author: Rick-Anderson
description: "Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 60374ddc6754f7e312ad08b420268308a9935bb4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-view"></a>Adicionando uma exibição
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Nesta seção você irá modificar o `HelloWorldController` classe para usar a exibição de arquivos de modelo para corretamente encapsulam o processo de geração de respostas HTML para um cliente.

Você criará um arquivo de modelo de exibição usando o [mecanismo de exibição Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduzido com o ASP.NET MVC 3. Modo de exibição baseado no Razor modelos têm um *. cshtml* extensão de arquivo e fornecem uma maneira elegante para criar HTML de saída usando c#. Razor minimiza o número de caracteres e pressionamentos de tecla necessários ao escrever um modelo de exibição e permite que um fluido rápido, fluxo de trabalho de codificação.

Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Alterar o `Index` método para retornar um `View` do objeto, como mostrado no código a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

O `Index` método acima usa um modelo de exibição para gerar uma resposta HTML para o navegador. Os métodos do controlador (também conhecido como [métodos de ação](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como o `Index` método acima, normalmente retornam um [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou uma classe derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), os tipos primitivos não como cadeia de caracteres.

No projeto, adicione um modelo de exibição que você pode usar com o `Index` método. Para fazer isso, clique dentro do `Index` método e clique em **adicionar exibição**.

![](adding-a-view/_static/image1.png)

O **adicionar exibição** caixa de diálogo é exibida. Deixe o padrão é a maneira como eles são e clique no **adicionar** botão:

![](adding-a-view/_static/image2.png)

O *MvcMovie\Views\HelloWorld* pasta e o *MvcMovie\Views\HelloWorld\Index.cshtml* arquivos são criados. Você pode vê-los em **Solution Explorer**:

![](adding-a-view/_static/image3.png)

A seguir mostra o *cshtml* arquivo que foi criado:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Adicionar o HTML a seguir sob o `<h2>` marca.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Completo *MvcMovie\Views\HelloWorld\Index.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Se você estiver usando o Visual Studio 2012, no Gerenciador de soluções, clique com botão direito do *cshtml* de arquivo e selecione **exibir em Inspetor de página**.

![PI](adding-a-view/_static/image5.png)

O [tutorial do Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) tem mais informações sobre essa nova ferramenta.

Como alternativa, execute o aplicativo e navegue até o `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). O `Index` método em seu controlador não fazer a quantidade de trabalho; ele simplesmente executou a instrução `return View()`, qual especificado que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Porque você não especificar explicitamente o nome do arquivo do modelo de exibição a ser usado, o ASP.NET MVC padrão usando o *cshtml* arquivo de exibição no *\Views\HelloWorld* pasta. A imagem abaixo mostra a cadeia de caracteres &quot;Olá do nosso modelo de exibição!&quot; embutidos em código no modo de exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador mostra &quot;índice meu ASP.NET A&quot; e informa que o link grande no topo da página &quot;seu logotipo aqui.&quot; Abaixo do &quot;seu logotipo aqui.&quot; link são registro e log nos links e abaixo com links para a página inicial, sobre e contato páginas. Vamos alterar algumas delas.

## <a name="changing-views-and-layout-pages"></a>Alterando modos de exibição e páginas de Layout

Primeiro, você deseja alterar o &quot;seu logotipo aqui.&quot; título na parte superior da página. Se o texto é comum a todas as páginas. Na verdade é implementado em um só lugar no projeto, mesmo que ela apareça em todas as páginas do aplicativo. Vá para o */exibições/compartilhado* pasta **Solution Explorer** e abra o  *\_cshtml* arquivo. Esse arquivo é chamado um *página de layout* e é compartilhado &quot;shell&quot; que usam todas as outras páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelos de layout permitem que você especifique o layout de contêiner HTML do seu site em um local e, em seguida, aplicá-lo em várias páginas em seu site. Localize a linha `@RenderBody()`. `RenderBody` é um espaço reservado em que todas as páginas específicas à exibição criadas são mostradas, &quot;encapsuladas&quot; na página de layout. Por exemplo, se você selecionar o link de sobre o *Views\Home\About.cshtml* exibição é renderizada dentro de `RenderBody` método.

Altere o título do título do site no modelo de layout de &quot;seu logotipo aqui&quot; para &quot;MVC filme&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Substitua o conteúdo do elemento title com a seguinte marcação:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Execute o aplicativo e observe que agora diz &quot;MVC filme &quot;. Clique o **sobre** link e você verá como essa página mostra &quot;MVC filme&quot;também. Fomos capazes de fazer a alteração de uma vez no modelo de layout e ter todas as páginas no site de refletir o novo título.

![](adding-a-view/_static/image8.png)

Agora, vamos alterar o título da exibição de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho de secundário (o `<h2>` elemento). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Para indicar o título HTML para exibir o código acima define um `Title` propriedade do `ViewBag` objeto (que está no *cshtml* modelo de exibição). Se você examinar novamente o código-fonte do modelo de layout, você observará que o modelo usa esse valor no `<title>` elemento como parte do `<head>` seção do HTML que modificamos anteriormente. Usando esse `ViewBag` abordagem, você pode facilmente passar outros parâmetros entre o modelo de exibição e o arquivo de layout.

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.) O título do navegador é criado com o `ViewBag.Title` definimos *cshtml* exibir modelo e adicional &quot;-aplicativo de filme&quot; adicionados ao arquivo de layout.

Observe também como o conteúdo a *cshtml* modelo de exibição foi mesclado com a  *\_cshtml* modelo de exibição e uma única resposta HTML foi enviada para o navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image9.png)

Nosso pequeno &quot;dados&quot; (nesse caso o &quot;Olá do nosso modelo de exibição de!&quot; mensagem) é inserido no código, embora. O aplicativo MVC tem um &quot;V&quot; (exibição) e você terá uma &quot;C&quot; (controlador), mas nenhum &quot;M&quot; (modelo) ainda. Em breve, examinaremos como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre passando informações do controlador para um modo de exibição. Classes do controlador é invocado em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que manipula o entrada navegador solicita, recupera dados de um banco de dados e, por fim, decide que tipo de resposta para enviar de volta para o navegador. Modelos de exibição, em seguida, podem ser usados em um controlador para gerar e formatar uma resposta HTML para o navegador.

Controladores serão responsáveis por fornecer quaisquer dados ou objetos necessários para um modelo de exibição renderizar uma resposta para o navegador. Uma prática recomendada: **um modelo de exibição nunca deve executar lógica de negócios ou interagir diretamente com um banco de dados**. Em vez disso, um modelo de exibição deve funcionar somente com os dados que são fornecidos pelo controlador. Manter isso &quot;separação de preocupações&quot; ajuda a manter seu código limpo, teste e mais fácil manutenção.

Atualmente, o `Welcome` método de ação de `HelloWorldController` classe leva um `name` e um `numTimes` parâmetro e, em seguida, os valores diretamente para o navegador de saídas. Em vez de fazer com que o controlador processar a resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição em vez disso. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador de colocar os dados dinâmicos (parâmetros) que o modelo de exibição precisa um `ViewBag` objeto que pode acessar o modelo de exibição.

Retorne ao *HelloWorldController.cs* de arquivo e altere o `Welcome` método para adicionar um `Message` e `NumTimes` o valor para o `ViewBag` objeto. `ViewBag`é um objeto dinâmico, o que significa que você pode colocar tudo o que você deseja o `ViewBag` objeto não tem nenhuma propriedade definida até que você insira algo dentro dele. O [sistema de associação do ASP.NET MVC modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados (`name` e `numTimes`) da cadeia de consulta na barra de endereços para parâmetros em seu método. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Agora o `ViewBag` objeto contém os dados que serão passados para o modo de exibição automaticamente.

Em seguida, você precisa de um modelo de exibição bem-vindo! No **criar** menu, selecione **MvcMovie Build** para certificar-se de que o projeto é compilado.

Em seguida, clique dentro do `Welcome` método e clique em **adicionar exibição**.

![](adding-a-view/_static/image10.png)

Aqui está o que o **adicionar exibição** caixa de diálogo aparência:

![](adding-a-view/_static/image11.png)

Clique em **adicionar**e, em seguida, adicione o seguinte código sob o `<h2>` elemento no novo *Welcome.cshtml* arquivo. Você criará um loop que diz &quot;Hello&quot; quantas vezes o usuário diz que deveria. Completo *Welcome.cshtml* arquivo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Execute o aplicativo e navegue até a URL a seguir:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora os dados é feitos da URL e passados para o controlador usando o [associador de modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). O controlador de pacotes de dados em um `ViewBag` objeto e etapas de objeto para o modo de exibição. O modo de exibição exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image12.png)

No exemplo acima, usamos um `ViewBag` objeto para passar dados do controlador para um modo de exibição. Último no tutorial, usaremos um modelo de exibição para passar dados de um controlador para um modo de exibição. A abordagem de modelo de exibição para passar dados é geralmente muito preferível a abordagem de recipiente de propriedades de exibição. Consulte a postagem do blog [V fortemente tipado exibições dinâmicas](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obter mais informações.

Bem, isso foi um tipo de um &quot;M&quot; para modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

>[!div class="step-by-step"]
[Anterior](adding-a-controller.md)
[Próximo](adding-a-model.md)
