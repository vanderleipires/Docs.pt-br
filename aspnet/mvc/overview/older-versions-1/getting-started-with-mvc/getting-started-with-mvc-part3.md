---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Adicionando uma exibição | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: f55e558dd056e86bdd2310894959aef02a9d8de2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823488"
---
<a name="adding-a-view"></a>Adicionando uma exibição
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos examinar como podemos ter nossa classe HelloWorldController usar um arquivo de modelo de exibição para encapsular corretamente gerando respostas HTML para um cliente.

Vamos começar usando um modelo de exibição com nosso método de índice. Nosso método é chamado de índice e ele está no HelloWorldController. No momento, nosso método Index () retorna uma cadeia de caracteres com uma mensagem que é codificado dentro da classe Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Agora vamos alterar o método de índice em vez disso, ter esta aparência:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Agora vamos adicionar um modelo de exibição ao nosso projeto que usaremos para nosso método Index (). Para fazer isso, clique com o mouse em algum lugar no meio do método de índice e clique em Adicionar modo de exibição...

![imagem](getting-started-with-mvc-part3/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar exibição", que nos fornece algumas opções para como queremos criar um modelo de exibição pode ser usado pelo nosso método de índice. Por enquanto, não altera nada e simplesmente clique no botão Adicionar.

[![Adicionar caixa de diálogo de exibição](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Depois de clicar em Adicionar, uma nova pasta e um novo arquivo aparecerá na pasta da solução, como visto aqui. Agora tenho uma pasta de HelloWorld em modos de exibição e um arquivo aspx dentro dessa pasta.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

O novo arquivo de índice também já está aberto e pronto para edição. Adicione algum texto sob o primeiro &lt;h2&gt;índice&lt;/h2&gt; , como "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Executar o aplicativo e visite [ `http://localhost:xx/HelloWorld` ](http://localhostxx) novamente no seu navegador. O método Index em nosso controlador neste exemplo não realiza nenhum trabalho, mas chamou de "retorno View()" que indicado que queremos usar um arquivo de modelo de exibição para renderizar uma resposta de volta ao cliente. Porque não especificamos explicitamente o nome do arquivo de modelo de exibição para usar, ASP.NET MVC assumiu como padrão usando o arquivo de exibição aspx dentro da pasta \Views\HelloWorld. Agora, vemos que a cadeia de caracteres que é embutido em código na nossa exibição.

[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Parece muito bom. No entanto, observe que o título do navegador diz "Index" e grande título na página diz "Meu aplicativo MVC." Vamos alterá-los.

### <a name="changing-views-and-master-pages"></a>Alterando exibições e páginas mestras

Primeiro, vamos alterar o texto "Meu aplicativo MVC." Esse texto é compartilhado e aparece em cada página. Na verdade, aparece em apenas um lugar no nosso código, mesmo que seja em cada página em nosso aplicativo. Vá para a pasta /Views/Shared no Gerenciador de soluções e abra o arquivo site. Esse arquivo é chamado de uma página mestra e é o compartilhado "shell" que usam todas as nossas páginas.

Observe algum texto que diz ContentPlaceholder "MainContent" nesse arquivo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Esse espaço reservado é onde todas as páginas que você cria serão exibidas, "encapsuladas" na página mestra. Tente alterar o título, em seguida, execute o aplicativo e visite várias páginas. Você observará que a uma alteração aparece em várias páginas.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Agora, todas as páginas terão o cabeçalho primário - que é H1 - de "Meu aplicativo de filme do MVC." Que manipula o texto em branco na parte superior lá o que é compartilhado entre todas as páginas.

Aqui está o site em sua totalidade com nosso título alterado:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Agora, vamos alterar o título da página de índice.

Abra /HelloWorld/Index.aspx. Há dois lugares para alterar. Primeiro, o título que aparece no título do navegador, em seguida, o cabeçalho secundário - que também é H2. Vou salvá-las, cada um pouco diferente para que você possa ver qual parte do código altera qual parte do aplicativo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Executar seu aplicativo e visite /Movies. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. É fácil fazer grandes alterações em seu aplicativo com pequenas alterações ao modo de exibição.

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nosso pequeno bit de "dados" (no caso de "Hello World!" mensagem) foi codificado no entanto. Temos V (exibições) e temos C (controladores), mas ainda não há M (modelo). Em breve vamos examinar como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-a-viewmodel"></a>Passando um ViewModel

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre "ViewModels". Esses são objetos que representam o que requer um modelo de exibição para renderizar uma resposta HTML para um cliente. Eles normalmente são criados e passado por uma classe de controlador para um modelo de exibição e devem conter apenas os dados que exige que o modelo de exibição – e nada mais.

Anteriormente com o nosso exemplo de HelloWorld, nosso método de ação Welcome() levou um nome e um parâmetro numTimes e enviá-lo para o navegador. Em vez de fazer com que o controlador de continuar a renderize a resposta diretamente, em vez disso, faremos uma pequena classe para manter esses dados e, em seguida, passá-lo ao longo de um modelo de exibição para renderizar a resposta HTML usá-lo de volta. Dessa forma, o controlador está preocupado com o modelo de exibição e de uma coisa outro – que nos permite manter "separação limpa de preocupações" dentro do nosso aplicativo.

Retornar para o arquivo HelloWorldController.cs e adicione uma nova classe "WelcomeViewModel" e altere o método de boas-vindo dentro do controlador. Aqui está o HelloWorldController.cs completa com a nova classe no mesmo arquivo.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Mesmo que seja em várias linhas, bem-vindo ao nosso método é realmente apenas duas instruções de código. A primeira instrução empacota nossos dois parâmetros em um objeto ViewModel e a segunda passa o objeto resultante para o modo de exibição.

Agora precisamos de um modelo de exibição bem-vindo! Clique com o botão direito no método de boas-vindo e selecione Adicionar modo de exibição. Desta vez, vamos verificar "Criar uma exibição fortemente tipada" e selecionar nossa classe WelcomeViewModel na lista suspensa. Esse novo modo de exibição só saberá sobre WelcomeViewModels e não há outros tipos de objetos.

> *Observação: Você precisará ter compilado uma vez depois de adicionar seu WelcomeViewModel para aparecer na lista suspensa.*


Aqui está o que deve ser a aparência de sua caixa de diálogo Adicionar modo de exibição. Clique no botão Adicionar. ![Adicionar que modo de exibição dentro de um círculo](getting-started-with-mvc-part3/_static/image10.png)

Adicione este código sob o &lt;h2&gt; em seu novo Welcome.aspx. Vamos fazer um loop e diga Olá quantas vezes o usuário disser que devemos!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Além disso, observe enquanto você estiver digitando que porque nós dissemos isso exibição sobre o WelcomeViewModel (são casados, lembre-se?) que obtemos o Intellisense útil sempre que fazemos referência nosso objeto de modelo, como visto na captura de tela abaixo:

[![Código-fonte NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Executar o aplicativo e visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` novamente. Agora estamos levando dados da URL, ele é passado para o nosso controlador automaticamente, nosso controlador empacota os dados em um ViewModel e passa esse objeto para nosso modo de exibição. O modo de exibição que exibe os dados como HTML para o usuário.

[![Bem-vindo - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Bem, isso foi uma espécie de "M" para o modelo, mas não o tipo de banco de dados. Vejamos o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part2.md)
> [Próximo](getting-started-with-mvc-part4.md)
