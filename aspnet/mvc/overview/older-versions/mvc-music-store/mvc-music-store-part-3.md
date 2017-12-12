---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "Parte 3: Exibições e ViewModels | Microsoft Docs"
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 3 abrange ViewModels e exibições."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a>Parte 3: Exibições e ViewModels
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 3 abrange ViewModels e exibições.


Até agora é tiver apenas foi retornar cadeias de caracteres de ações do controlador. Que é uma boa forma de obter uma ideia de como funcionam os controladores, mas não como você seria deseja criar um aplicativo web real. Vamos deseja uma maneira melhor de gerar HTML para navegadores visitar nosso site – um em que podemos usar arquivos de modelo para personalizar mais facilmente o conteúdo HTML enviada de volta. É exatamente o que fazem modos de exibição.

## <a name="adding-a-view-template"></a>Adicionando um modelo de exibição

Para usar um modelo de exibição, vamos alterar o método HomeController índice para retornar um ActionResult e ainda retornar View(), como abaixo:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

A alteração acima indica que, em vez de retornou uma cadeia de caracteres, em vez disso, queremos usar uma "exibição" para gerar um resultado de volta.

Agora vamos adicionar um modelo de exibição apropriado ao nosso projeto. Para fazer isso, vamos posicionar o cursor de texto dentro do método de ação do índice, e em seguida, clique com botão direito e selecione "Adicionar modo de exibição". Isso abrirá a caixa de diálogo Adicionar modo de exibição:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

A caixa de diálogo "Adicionar modo de exibição" permite a rápida e facilmente gerar arquivos de modelo de exibição. Por padrão "Adicionar modo de exibição" caixa de diálogo popula previamente o nome do modelo de exibição para criar exatamente como o método de ação que irá usá-la. Porque usamos o menu de contexto "Adicionar modo de exibição" dentro do método de ação Index () do nosso HomeController, a caixa de diálogo "Adicionar modo de exibição" acima tem "Index" como o nome de exibição preenchido por padrão. Não precisamos alterar qualquer uma das opções na caixa de diálogo, portanto, clique no botão Adicionar.

Depois, clique no botão Adicionar, Visual Web Developer criará um cshtml novo modelo de exibição para nós no diretório \Views\Home, criando a pasta se já não existe.

![](mvc-music-store-part-3/_static/image2.png)

O nome e uma pasta local do arquivo "Cshtml" é importante e segue as convenções de nomenclatura padrão ASP.NET MVC. O nome do diretório, \Views\Home, coincide com o controlador - que é chamado HomeController. Nome do modelo da exibição, índice, faz a correspondência de método de ação do controlador que deseja exibir o modo de exibição.

ASP.NET MVC permite evitar ter que especificar explicitamente o nome ou local de um modelo de exibição quando usamos essa convenção de nomenclatura para retornar uma exibição. Ele por padrão processará o modelo de exibição \Views\Home\Index.cshtml quando escrevemos código como abaixo em nosso HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

O Visual Web Developer criado e aberto o modelo de exibição "Cshtml" depois de clicar no botão "Adicionar" na caixa de diálogo "Adicionar modo de exibição". O conteúdo de cshtml é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Essa exibição está usando a sintaxe do Razor, que é mais concisa que o mecanismo de exibição de Web Forms usado em Web Forms do ASP.NET e versões anteriores do ASP.NET MVC. O mecanismo de exibição de Web Forms ainda está disponível no ASP.NET MVC 3, mas muitos desenvolvedores consideram que o mecanismo de exibição Razor adequada o desenvolvimento do ASP.NET MVC muito bem.

As três primeiras linhas definem o título da página usando ViewBag. Vamos examinar como isso funciona em mais detalhes em breve, mas primeiro vamos atualizar o texto de cabeçalho de texto e exibir a página. Atualização de &lt;h2&gt; marca dizer "Esta é a Home Page", conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Executando a aplicativo mostra que o nosso novo texto está visível na página inicial.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Usando um Layout para elementos comuns do site

A maioria dos sites têm conteúdo que é compartilhado entre várias páginas: navegação rodapés, imagens de logotipo, referências de folha de estilos, etc. O mecanismo de exibição Razor torna isso fácil de gerenciar usando uma página chamada \_cshtml que automaticamente foi criado para nós dentro da pasta/exibições/compartilhado.

![](mvc-music-store-part-3/_static/image4.png)

Clique duas vezes nessa pasta para exibir o conteúdo, que são mostrados abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

O conteúdo do nosso exibições individuais será exibido pelo @RenderBodycomando () e qualquer conteúdo comum que queremos aparecer fora do que podem ser adicionados para o \_cshtml marcação. Queremos será nosso repositório de música MVC para tiver um cabeçalho de comuns com links para a área página inicial e o armazenamento em todas as páginas do site, portanto, adicionaremos que o modelo diretamente acima que @RenderBodyinstrução ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Atualizar a folha de estilos

O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas os estilos usados para exibir mensagens de validação. Nosso designer fornece alguns CSS e imagens adicionais para definir a aparência do nosso site, portanto, adicionaremos os agora.

O arquivo CSS atualizado e as imagens são incluídas no diretório de conteúdo de Assets.zip MvcMusicStore que está disponível em [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store). Vamos selecionar ambos no Windows Explorer e soltá-los na pasta de conteúdo da nossa solução no Visual Web Developer, conforme mostrado abaixo:

![](mvc-music-store-part-3/_static/image5.png)

Você será solicitado a confirmar se deseja substituir o arquivo de site existente. Clique em Sim.

![](mvc-music-store-part-3/_static/image6.png)

A pasta de conteúdo do seu aplicativo agora será exibida da seguinte maneira:

![](mvc-music-store-part-3/_static/image7.png)

Agora vamos executar o aplicativo e ver a aparecem de nossas alterações na Home page.

![](mvc-music-store-part-3/_static/image8.png)

- Vamos analisar o que mudou: método de ação do HomeController o índice encontrado e exibidos modelo \Views\Home\Index.cshtmlView, embora nosso código chamado "retorno View()", pois nosso modelo de exibição seguido a convenção de nomenclatura padrão.
- A Home Page está exibindo uma mensagem de boas-vinda simples que é definida dentro do modelo de exibição \Views\Home\Index.cshtml.
- A Home Page está usando nosso \_cshtml modelo, e, portanto, a mensagem de boas-vinda está contida dentro do layout do site padrão HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Usando um modelo para passar informações para nossa visão

Um modelo de exibição que exibe apenas inserido no código HTML não vai fazer um site da web muito interessantes. Para criar um site da web dinâmico, em vez disso, podemos desejará passar informações de nosso ações do controlador para os nossos modelos de exibição.

O padrão Model-View-Controller, o termo A que modelo refere-se objetos que representam os dados no aplicativo. Geralmente, objetos de modelo correspondem às tabelas no banco de dados, mas não é necessário.

Métodos de ação do controlador que retornam um ActionResult podem passar um objeto de modelo para o modo de exibição. Isso permite que um controlador pacote corretamente todas as informações necessárias para gerar uma resposta e, em seguida, passar essa informação para um modelo de exibição a ser usado para gerar a resposta HTML apropriada. Isso é mais fácil de entender, vê-lo em ação, vamos começar.

Primeiro, vamos criar algumas classes de modelo para representar gêneros e álbuns em nosso repositório. Vamos começar criando uma classe de gênero. Clique na pasta "Modelos" dentro de seu projeto, escolha a opção "Adicionar classe" e nomeie o arquivo "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Em seguida, adicione uma propriedade de nome da cadeia de caracteres pública à classe que foi criada:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Observação: no caso você esteja se perguntando, o {get; definir;} notação está fazendo uso do recurso de Propriedades autoimplementadas do #. Isso nos fornece os benefícios de uma propriedade sem a necessidade de nós declarar um campo existente.*

Em seguida, siga as mesmas etapas para criar uma classe álbum (chamada Album.cs) que tem um título e uma propriedade de Gênero:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Agora podemos modificar StoreController para usar os modos de exibição que exibem informações dinâmicas do nosso modelo. Se - - agora para fins de demonstração é denominado nossos álbuns com base na ID da solicitação, podemos pode exibir essas informações como o modo de exibição abaixo.

![](mvc-music-store-part-3/_static/image10.png)

Vamos começar alterando a ação de detalhes do repositório para que ele mostra as informações para um único álbum. Adicione uma instrução "using" na parte superior do **StoreControllers** classe para incluir o namespace MvcMusicStore.Models, portanto, não precisamos digite MvcMusicStore.Models.Album toda vez que você deseja usar a classe álbum. A seção "usos" da classe agora deve aparecer abaixo.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Em seguida, vamos atualizar a ação de controlador de detalhes para que ela retorne um ActionResult em vez de uma cadeia de caracteres, como foi feito com o método de índice do HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Agora podemos modificar a lógica para retornar um objeto álbum para o modo de exibição. Posteriormente neste tutorial Vamos recuperar os dados de um banco de dados – mas para agora usaremos "fictício dados" para começar.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Observação: Se você estiver familiarizado com c#, você pode presumir que usar var significa que nossa variável álbum associação tardia. Não é correto – o compilador c# é usando inferência de tipo com base no qual estamos atribuir à variável para determinar o que álbum é do tipo álbum e compilar a variável local álbum como um tipo de álbum, portanto obtemos a verificação de tempo de compilação e o editor de códigos do Visual Studio suporte.*

Agora vamos criar um modelo de exibição que usa o nosso álbum para gerar uma resposta HTML. Antes de fazermos isso, precisamos criar o projeto para que a caixa de diálogo Adicionar modo de exibição sabe sobre nossa classe álbum recém-criado. Você pode compilar o projeto selecionando o Debug⇨Build MvcMusicStore item de menu (para crédito extra, você pode usar o atalho Ctrl-Shift-B para compilar o projeto).

![](mvc-music-store-part-3/_static/image11.png)

Agora que criamos nossas classes de suporte, estamos prontos para compilar nosso modelo de exibição. Clique no método de detalhes e selecione "Adicionar modo de exibição..." no menu de contexto.

![](mvc-music-store-part-3/_static/image12.png)

Vamos criar um novo modelo de exibição como antes com HomeController. Porque estamos criando do StoreController será por padrão gerado em um arquivo \Views\Store\Index.cshtml.

Ao contrário, vamos para verificar se a caixa de seleção "Criar fortemente tipado" modo de exibição. Em seguida, vamos selecionar nossa classe de "Álbum" dentro de "Classe de dados de exibição" drop-downlist. Isso fará com que a caixa de diálogo "Adicionar modo de exibição" criar um modelo de exibição que espera que um álbum objeto será passado para ele usar.

![](mvc-music-store-part-3/_static/image13.png)

Quando, clique no botão "Adicionar" nosso modelo de exibição \Views\Store\Details.cshtml será criado, que contém o código a seguir.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Observe que a primeira linha, o que indica que essa exibição está fortemente tipado para nossa classe álbum. O mecanismo de exibição Razor compreende que ele foi passado um objeto álbum, podemos acessar facilmente as propriedades de modelo e ter o benefício do IntelliSense no editor do Visual Web Developer.

Atualização de &lt;h2&gt; marcar exibe propriedades do título do álbum, modificando a linha da seguinte maneira.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Observe que o IntelliSense é acionado quando você insere o período após o @Model palavra-chave, mostrando as propriedades e métodos que oferece suporte a classe álbum.

Vamos agora execute novamente o nosso projeto e visita a URL de repositório/detalhes/5. Veremos detalhes de um álbum como abaixo.

![](mvc-music-store-part-3/_static/image14.png)

Agora será feita uma atualização semelhante para o método de ação procurar armazenamento. O método de atualização para que ela retorne um ActionResult e modificar a lógica do método para que ele cria um novo objeto de gênero e retorna ao modo de exibição.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Clique com botão direito no método procurar e selecione "Adicionar modo de exibição..." no menu de contexto, em seguida, adicione uma exibição fortemente tipada adicionar fortemente tipada para a classe de gênero.

![](mvc-music-store-part-3/_static/image15.png)

Atualização de &lt;h2&gt; elemento no modo de exibição de código (em /Views/Store/Browse.cshtml) para exibir as informações de gênero.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Agora vamos executar novamente o nosso projeto e navegue até o repositório/procurar? Gênero = URL globo. Veremos a página Procurar exibida como abaixo.

![](mvc-music-store-part-3/_static/image16.png)

Por fim, vamos fazer uma atualização de um pouco mais complexa para o **índice armazenar** método de ação e o modo de exibição para exibir uma lista de todos os gêneros na loja. Faremos isso usando uma lista de gêneros como nosso objeto de modelo, em vez de apenas um único gênero.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Clique no método de ação de índice de repositório e selecione Adicionar modo de exibição como antes, selecione gênero, como a classe de modelo e pressione o botão Adicionar.

![](mvc-music-store-part-3/_static/image17.png)

Primeiro vamos alterar o @model declaração para indicar que o modo de exibição estará esperando gênero vários objetos em vez de apenas um. Altere a primeira linha do /Store/Index.cshtml da seguinte forma:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Isso informa ao mecanismo de exibição do Razor que estará trabalhando com um objeto de modelo que pode conter vários objetos de gênero. Estamos usando um IEnumerable&lt;gênero&gt; em vez de uma lista&lt;gênero&gt; como ele é mais genérico, permitindo-nos alterar os tipo de modelo mais tarde para qualquer tipo de objeto que oferece suporte a interface IEnumerable.

Em seguida, vamos será loop pelos objetos no modelo, conforme mostrado no código abaixo concluídas gênero.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Observe que há total suporte do IntelliSense como podemos inserir esse código, para que quando digitamos "@Model." podemos ver todos os métodos e propriedades com suporte por um IEnumerable de tipo gênero.

![](mvc-music-store-part-3/_static/image18.png)

Em nosso loop "foreach", Visual Web Developer sabe que cada item é do tipo gênero, para vemos IntelliSence para cada tipo de gênero.

![](mvc-music-store-part-3/_static/image19.png)

Em seguida, o recurso de scaffolding examinou o objeto de gênero e determinado que cada uma terá uma propriedade de nome, para que ele percorre e grava-los. Ela também gera links de edição, detalhes e exclusão para cada item individual. Vamos vantagem de que mais tarde em nossa gerente da loja, mas agora queremos ter uma lista simples em vez disso.

Quando executar o aplicativo e navegue até /Store, podemos ver que a contagem e a lista de gêneros é exibido.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Adicionando Links entre páginas

Nossa URL /Store que lista gêneros atualmente lista os nomes de gênero simplesmente como texto sem formatação. Vamos alterar isso para que, em vez de texto sem formatação em vez disso, temos o link de nomes de gênero para a URL de repositório/procurar apropriada, para que clicar em um gênero de música, como "Disco" navegará para a loja/procurar? gênero = URL do Disco. Foi atualizamos nosso modelo de exibição de \Views\Store\Index.cshtml à saída desses links usando código como abaixo **(não digite o texto, vamos melhorá-lo)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Isso funciona, mas isso pode levar a problemas mais tarde, desde que ele se baseia em uma cadeia de caracteres codificada. Por exemplo, se quiséssemos renomear o controlador, podemos precisa pesquisar por meio de nosso código procurando links que precisam ser atualizados.

É uma abordagem alternativa que podemos usar tirar proveito de um método auxiliar HTML. O ASP.NET MVC inclui os métodos auxiliares HTML que estão disponíveis em nosso código de modelo de exibição para executar uma variedade de tarefas comuns como isso. O método auxiliar Html.ActionLink() é particularmente útil e torna mais fácil criar HTML &lt;um&gt; links e cuida dos detalhes irritantes, como verificar se os caminhos de URL corretamente são codificados de URL.

Html.ActionLink() tem várias sobrecargas diferentes para permitir a especificação de informações conforme necessário para os links. No caso mais simples, você irá fornecer apenas o texto do link e o método de ação para ir para quando o hiperlink é clicado no cliente. Por exemplo, podemos pode vincular a "Repositório /" método Index () na página de detalhes do repositório com o texto do link "Ir para o índice de repositório" usando a chamada a seguir:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Observação: nesse caso, não precisamos especificar o nome do controlador porque estamos apenas está vinculando outra ação no mesmo controlador que está processando o modo de exibição atual.*

Os links para a página Procurar precisará passar um parâmetro, porém, então, vamos usar outra sobrecarga do método ActionLink que usa três parâmetros:

- 1. Texto do link, que exibe o nome de gênero
- 2. Nome de ação do controlador (Procurar)
- 3. Valores de parâmetro de rota, especificando o nome (gênero) e o valor (nome de gênero)

Colocando-se de que todos os aspectos juntos, aqui está como vamos gravar esses links para a exibição do índice de repositório:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Agora quando executar o nosso projeto novamente e acessar a URL /Store/ veremos uma lista de gêneros. Cada gênero é um hiperlink – quando clicado levará conosco para nosso repositório/procurar? gênero =*[gênero]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

O HTML para a lista de gênero tem esta aparência:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-2.md)
[Próximo](mvc-music-store-part-4.md)
