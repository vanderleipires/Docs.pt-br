---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Exibições e ViewModels | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 3 aborda Views e ViewModels.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 0cfdc2864221a0efc81eb362571c72f20eb05af8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368729"
---
<a name="part-3-views-and-viewmodels"></a>Parte 3: Exibições e ViewModels
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 3 aborda Views e ViewModels.


Até agora podemos ter apenas foi retornar cadeias de caracteres de ações do controlador. Isso é uma excelente forma de obter uma ideia de como funcionam os controladores, mas é não como convém criar um aplicativo web real. Vamos querer uma maneira melhor de gerar HTML para navegadores visitar nosso site – uma em que podemos usar arquivos de modelo para personalizar mais facilmente o conteúdo HTML enviada de volta. Isso é exatamente o que fazem os modos de exibição.

## <a name="adding-a-view-template"></a>Adicionando um modelo de exibição

Para usar um modelo de exibição, vamos alterar o método Index do HomeController para retornar um ActionResult e que ele retorne View(), como abaixo:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

A alteração acima indica que, em vez de retornou uma cadeia de caracteres, em vez disso, queremos usar uma "exibição" para gerar um resultado de volta.

Agora vamos adicionar um modelo de exibição apropriado ao nosso projeto. Para fazer isso, podemos posicionar o cursor do texto dentro do método de ação de índice, e em seguida, clique com botão direito e selecione "Adicionar exibição". Isso abrirá a caixa de diálogo Adicionar modo de exibição:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

A caixa de diálogo "Adicionar exibição" nos permite rapidamente e facilmente gerar arquivos de modelo de exibição. Por padrão "Adicionar exibição de" caixa de diálogo preenche previamente o nome do modelo de exibição para criar para que ele corresponda ao método de ação que irá usá-lo. Como usamos o menu de contexto "Adicionar exibição" dentro do método de ação Index () de nossa HomeController, a caixa de diálogo "Adicionar exibição" acima tem "Index" como o nome de exibição previamente preenchido por padrão. Não precisamos alterar qualquer uma das opções na caixa de diálogo, então, clique no botão Adicionar.

Quando clicamos no botão Add, o Visual Web Developer criará um index. cshtml novo modelo de exibição para nós no diretório \Views\Home, criando a pasta se ainda não existe.

![](mvc-music-store-part-3/_static/image2.png)

O nome e uma pasta local do arquivo "Index. cshtml" é importante e segue as convenções de nomenclatura padrão ASP.NET MVC. O nome do diretório, \Views\Home, corresponde ao controlador - que é o nome HomeController. O nome de modelo de exibição, índice, coincide com o método de ação do controlador que deseja exibir o modo de exibição.

ASP.NET MVC permite evitar ter que especificar explicitamente o nome ou local de um modelo de exibição quando usamos essa convenção de nomenclatura para retornar uma exibição. Por padrão renderizará o modelo de exibição \Views\Home\Index.cshtml quando escrevemos código, como a seguir dentro do nosso HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

O Visual Web Developer criado e aberto o modelo de exibição "Index. cshtml", depois, clicamos no botão "Adicionar" na caixa de diálogo "Adicionar exibição". O conteúdo de index. cshtml é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Essa exibição está usando a sintaxe do Razor, que é mais concisa que o mecanismo de exibição de formulários da Web usado em Web Forms do ASP.NET e as versões anteriores do ASP.NET MVC. O mecanismo de exibição de Web Forms ainda está disponível no ASP.NET MVC 3, mas muitos desenvolvedores acham que o mecanismo de exibição do Razor se encaixa o desenvolvimento de ASP.NET MVC muito bem.

As três primeiras linhas definem o título da página usando ViewBag. Vamos examinar como isso funciona em mais detalhes em breve, mas primeiro vamos atualizar o texto de cabeçalho do texto e exibir a página. Atualizar o &lt;h2&gt; marca dizer "Esta é a Home Page", conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Executando a aplicativo mostra que nosso novo texto está visível na home page.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Usando um Layout para elementos comuns do site

A maioria dos sites têm conteúdo que é compartilhado entre várias páginas: navegação, rodapés de páginas, imagens de logotipo, referências de folha de estilos, etc. O mecanismo de exibição Razor torna isso fácil de gerenciar o uso de uma página chamada \_layout. cshtml, que automaticamente foi criado para nós dentro da pasta/Views/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Clique duas vezes nessa pasta para exibir o conteúdo, que são mostrados abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

O conteúdo do nosso exibições individuais será exibido pelo @RenderBodycomando () e qualquer conteúdo comum que desejamos aparecer fora que podem ser adicionados para o \_marcação de layout. cshtml. Vamos nosso Store de música do MVC para ter um cabeçalho comum com links para nossa área Home page e Store em todas as páginas no site, portanto, vamos adicionar isso ao modelo diretamente acima que @RenderBodyinstrução ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Atualizando a folha de estilos

O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas os estilos usados para exibir mensagens de validação. Nosso designer forneceu alguns CSS e imagens adicionais para definir a aparência do nosso site, portanto, vamos adicionar os agora.

O arquivo CSS e imagens atualizados estão incluídas no diretório de conteúdo de Assets.zip MvcMusicStore que está disponível em [MVC de música de Store](https://github.com/evilDave/MVC-Music-Store). Vamos selecionar ambos no Windows Explorer e soltá-los na pasta de conteúdo da nossa solução no Visual Web Developer, conforme mostrado abaixo:

![](mvc-music-store-part-3/_static/image5.png)

Você será solicitado a confirmar se deseja substituir o arquivo CSS existente. Clique em Sim.

![](mvc-music-store-part-3/_static/image6.png)

A pasta de conteúdo do seu aplicativo agora será exibida da seguinte maneira:

![](mvc-music-store-part-3/_static/image7.png)

Agora vamos executar o aplicativo e ver a aparecem de nossas alterações na Home page.

![](mvc-music-store-part-3/_static/image8.png)

- Vamos examinar o que mudou: método de ação de índice o HomeController encontrado e exibido modelo \Views\Home\Index.cshtmlView, mesmo que o nosso código chamado "retorno View()", pois nosso modelo de exibição seguido a convenção de nomenclatura padrão.
- A Home Page é exibindo uma mensagem de boas-vinda simples que é definida dentro do modelo de exibição \Views\Home\Index.cshtml.
- A Home Page é usando nossa \_modelo de layout. cshtml, e, portanto, a mensagem de boas-vinda está contida o layout do site padrão HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Usando um modelo para passar informações para nossa exibição

Um modelo de exibição que exibe apenas inserido no código HTML não vai deixar um site muito interessante. Para criar um site dinâmico, em vez disso, vamos passar informações de nossas ações do controller para nossos modelos de exibição.

O padrão Model-View-Controller, o termo A que modelo se refere objetos que representam os dados no aplicativo. Muitas vezes, objetos de modelo correspondem às tabelas no banco de dados, mas eles não precisem.

Os métodos de ação do controlador que retornar um ActionResult podem passar um objeto de modelo para o modo de exibição. Isso permite que um controlador empacotar corretamente todas as informações necessárias para gerar uma resposta e, em seguida, passar essas informações as a um modelo de exibição para usar para gerar a resposta apropriada do HTML. Isso é mais fácil de entender, conferindo-lo em ação, então vamos começar.

Primeiro, criaremos algumas classes de modelo para representar gêneros e álbuns em nosso armazenamento. Vamos começar criando uma classe de gênero. Clique com botão direito na pasta "Modelos" dentro de seu projeto, escolha a opção de ""adicionar Class e nomeie o arquivo "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Em seguida, adicione uma propriedade de nome da cadeia de caracteres pública à classe que foi criada:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Observação: no caso você esteja se perguntando, o {get; definir;} notação está fazendo uso do recurso de Propriedades autoimplementadas do #. Isso nos dá os benefícios de uma propriedade sem a necessidade de nós declarar um campo de suporte.*

Em seguida, siga as mesmas etapas para criar uma classe de álbum (chamada Album.cs) que tem um título e uma propriedade de Gênero:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Agora podemos modificar o StoreController para usar os modos de exibição que exibem informações dinâmicas do nosso modelo. Se - para fins de demonstração no momento - nomeamos o nosso álbuns com base na ID de solicitação, podemos pode exibir essas informações como no modo de exibição abaixo.

![](mvc-music-store-part-3/_static/image10.png)

Vamos começar alterando a ação de detalhes da Store para que ela mostre as informações para um único álbum. Adicione uma instrução "using" na parte superior do **StoreControllers** classe para incluir o namespace MvcMusicStore.Models, portanto, não precisamos digitar MvcMusicStore.Models.Album sempre que queremos usar a classe de álbum. A seção "usos" dessa classe agora deve aparecer conforme mostrado abaixo.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Em seguida, vamos atualizar a ação do controlador de detalhes para que ele retorne um ActionResult em vez de uma cadeia de caracteres, como fizemos com o método de índice do HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Agora podemos modificar a lógica para retornar um objeto de álbum para o modo de exibição. Posteriormente no tutorial Vamos recuperar os dados de um banco de dados – mas para agora, usaremos "fictício dados" para começar a usar.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Observação: Se você estiver familiarizado com c#, você pode supor que, usando var significa que nossa variável de álbum é a associação tardia. Não está correta – o compilador do c# é usando inferência de tipo com base no qual estamos atribuindo à variável para determinar o que álbum é do tipo álbum e compilando a variável local álbum como um tipo de álbum, portanto, obtemos a verificação de tempo de compilação e o editor de códigos do Visual Studio suporte.*

Agora, vamos criar um modelo de exibição que usa nosso álbum para gerar uma resposta HTML. Antes de fazer isso é necessário compilar o projeto para que a caixa de diálogo Adicionar modo de exibição sabe sobre nossa classe de álbum recém-criado. Você pode compilar o projeto, selecionando o Debug⇨Build MvcMusicStore item de menu (para créditos extras, você pode usar o atalho Ctrl-Shift-B para compilar o projeto).

![](mvc-music-store-part-3/_static/image11.png)

Agora que configuramos nosso classes de suporte, estamos prontos para criar nosso modelo de exibição. Clique com botão direito dentro do método detalhes e selecione "Adicionar modo de exibição..." no menu de contexto.

![](mvc-music-store-part-3/_static/image12.png)

Vamos criar um novo modelo de exibição como fizemos anteriormente com o HomeController. Como estamos criando do StoreController ele será por padrão gerado em um arquivo \Views\Store\Index.cshtml.

Ao contrário de antes, vamos verificar a caixa de seleção "Criar um fortemente tipados" modo de exibição. Em seguida, vamos selecionar nossa classe de "Álbum" dentro de "Modo de exibição de dados-class" drop-downlist. Isso fará com que a caixa de diálogo "Adicionar exibição" criar um modelo de exibição que espera que um álbum de objeto será passado a ele para usar.

![](mvc-music-store-part-3/_static/image13.png)

Quando clicamos no botão "Adicionar" nosso modelo de exibição \Views\Store\Details.cshtml será criado, que contém o código a seguir.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Observe que a primeira linha, que indica que essa exibição está fortemente tipada para nossa classe de álbum. O mecanismo de exibição Razor entende que ele foi passado um objeto de álbum, podemos facilmente acessar as propriedades do modelo e até mesmo ter a vantagem do IntelliSense no editor do Visual Web Developer.

Atualizar o &lt;h2&gt; Marque para que ele exibe a propriedade de título do álbum, modificando a linha a ser exibido da seguinte maneira.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Observe que o IntelliSense é disparado quando você insere o período após o @Model palavra-chave, mostrando as propriedades e métodos que oferece suporte a classe de álbum.

Vamos agora executar novamente o nosso projeto e acesse a URL de detalhes/Store/5. Vamos ver detalhes de um álbum, como abaixo.

![](mvc-music-store-part-3/_static/image14.png)

Agora vamos fazer uma atualização semelhante para o método de ação Store procurar. Atualize o método para que ela retorne um ActionResult e modificar a lógica do método, portanto, ele cria um novo objeto de gênero e retorna-o para o modo de exibição.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Clique com botão direito no método procurar e selecione "Adicionar modo de exibição..." no menu de contexto, em seguida, adicione uma exibição que é fortemente tipada adicionar fortemente tipados para a classe de gênero.

![](mvc-music-store-part-3/_static/image15.png)

Atualizar o &lt;h2&gt; elemento no modo de exibição de código (em /Views/Store/Browse.cshtml) para exibir as informações de gênero.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Agora vamos executar novamente o nosso projeto e navegue até/Store/procurar? Gênero = URL globo espelhado. Veremos a página Procurar exibida abaixo da seguinte maneira.

![](mvc-music-store-part-3/_static/image16.png)

Por fim, vamos fazer uma atualização de um pouco mais complexa o **Store índice** exibição para exibir uma lista de todos os gêneros em nosso repositório e o método de ação. Faremos isso usando uma lista de gêneros como nosso objeto de modelo, em vez de apenas um único gênero.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Clique com botão direito no método de ação de índice Store e selecione Adicionar exibição como antes, selecione o gênero como a classe de modelo e pressione o botão Adicionar.

![](mvc-music-store-part-3/_static/image17.png)

Primeiro vamos alterar o @model declaração para indicar que o modo de exibição estará esperando gênero vários objetos em vez de apenas um. Altere a primeira linha do /Store/Index.cshtml como segue:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Isso instrui o mecanismo de exibição do Razor que ele estará trabalhando com um objeto de modelo que pode conter vários objetos de gênero. Estamos usando um IEnumerable&lt;gênero&gt; em vez de uma lista&lt;gênero&gt; , pois ele é mais genérico, que nos permite alterar nosso tipo de modelo mais tarde para qualquer tipo de objeto que dá suporte a interface IEnumerable.

Em seguida, vamos fazer um loop por meio dos objetos de gênero no modelo, conforme mostrado no código de exibição completo abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Observe que temos suporte total ao IntelliSense como podemos inserir esse código, para que quando digitamos "@Model." podemos ver todos os métodos e propriedades com suporte por um IEnumerable de tipo de gênero.

![](mvc-music-store-part-3/_static/image18.png)

Dentro do nosso loop "foreach", o Visual Web Developer sabe que cada item é do tipo gênero, para que possamos ver IntelliSence para cada tipo de gênero.

![](mvc-music-store-part-3/_static/image19.png)

Em seguida, o recurso de scaffolding examinado o objeto de gênero e determinado a cada uma terá uma propriedade de nome, para que ele percorre e grava-os. Ele também gera links de edição, detalhes e exclusão para cada item individual. Podemos tirar vantagem que mais tarde no nosso gerente da loja, mas por enquanto gostaríamos de ter uma lista simples em vez disso.

Quando executar o aplicativo e navegue até /Store, podemos ver que a contagem e a lista de gêneros é exibida.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Adicionando Links entre as páginas

Nossa URL /Store que lista os gêneros atualmente lista os nomes de gênero simplesmente como texto sem formatação. Vamos alterar isso para que, em vez de texto sem formatação em vez disso, temos o link de nomes de gênero para a URL do Store/procura apropriada, para que o clique em um gênero de música, como "Disco" navegará até/Store/procurar? genre = URL do Disco. Podemos atualizar nosso modelo de exibição \Views\Store\Index.cshtml para esses links usando código como o abaixo de saída **(não digite isso em – vamos melhorá-lo)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Isso funciona, mas isso pode levar a problemas mais tarde, já que ele se baseia em uma cadeia de caracteres codificada. Por exemplo, se quiséssemos renomear o controlador, podemos precisaria pesquisar por meio de nosso código procurando por links que precisam ser atualizados.

É uma abordagem alternativa, que podemos usar tirar proveito de um método auxiliar HTML. O ASP.NET MVC inclui os métodos de auxiliares de HTML que estão disponíveis no nosso código de modelo de exibição para executar uma variedade de tarefas comuns, como isso. O método auxiliar Html.ActionLink() é particularmente útil e facilita a compilação HTML &lt;um&gt; vincula e cuida dos detalhes desagradáveis como certificando-se de caminhos de URL sejam corretamente codificados de URL.

Html.ActionLink() tem várias sobrecargas diferentes para permitir a especificação tanta informação quanto você precisa para os links. No caso mais simples, você irá fornecer apenas o texto do link e o método de ação para ir para quando o hiperlink é clicado no cliente. Por exemplo, podemos pode vincular a "/ Store /" método Index () na página de detalhes de Store com o texto do link de "Ir para a Store índice" usando a seguinte chamada:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Observação: nesse caso, não precisamos especificar o nome do controlador porque estamos apenas estiver vinculando a outra ação dentro do mesmo controlador que está renderizando o modo de exibição atual.*

Nosso links para a página Procurar precisará passar um parâmetro, no entanto, então, vamos usar outra sobrecarga do método ActionLink que usa três parâmetros:

- 1. Texto do link, que exibirá o nome de gênero
- 2. Nome da ação de controlador (Procurar)
- 3. Valores de parâmetro de rota, especificando o nome (gênero) e o valor (nome de gênero)

Colocando-se de que tudo isso, aqui está como vamos escrever esses links para a exibição de índice Store:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Agora quando podemos executar nosso projeto novamente e acesse a URL /Store/ veremos uma lista de gêneros. Cada gênero é um hiperlink – quando clicado ela nos levará à nossa/Store/procurar? genre =*[gênero]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

O HTML para a lista de gênero tem esta aparência:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-2.md)
> [Próximo](mvc-music-store-part-4.md)
