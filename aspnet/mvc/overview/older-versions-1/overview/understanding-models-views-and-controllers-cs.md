---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Noções básicas sobre modelos, exibições e controladores (c#) | Microsoft Docs
author: StephenWalther
description: Confuso sobre modelos, exibições e controladores? Neste tutorial, Stephen Walther apresenta as diferentes partes de um aplicativo ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: c4c5247ac4b880c1be60f0419ebc9fc9b790c639
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823388"
---
<a name="understanding-models-views-and-controllers-c"></a>Noções básicas sobre modelos, exibições e controladores (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Confuso sobre modelos, exibições e controladores? Neste tutorial, Stephen Walther apresenta as diferentes partes de um aplicativo ASP.NET MVC.


Este tutorial fornece uma visão geral do ASP.NET MVC, modelos, exibições e controladores. Em outras palavras, ele explica o M', V' e C' no ASP.NET MVC.

Depois de ler este tutorial, você deve compreender como as diferentes partes de um aplicativo ASP.NET MVC funcionam juntos. Você também deve entender como a arquitetura de um aplicativo ASP.NET MVC difere de um aplicativo ASP.NET Web Forms ou um aplicativo de Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>O aplicativo ASP.NET MVC de exemplo

O modelo padrão do Visual Studio para a criação de aplicativos Web do ASP.NET MVC inclui um aplicativo de exemplo extremamente simples que pode ser usado para entender as diferentes partes de um aplicativo ASP.NET MVC. Podemos aproveitar esse aplicativo simples neste tutorial.

Você cria um novo aplicativo ASP.NET MVC com o modelo MVC iniciando o Visual Studio 2008 e selecionar a opção de menu Arquivo, novo projeto (consulte a Figura 1). Na caixa de diálogo Novo projeto, selecione sua linguagem de programação favorita em tipos de projeto (Visual Basic ou c#) e selecione **aplicativo Web ASP.NET MVC** em modelos. Clique no botão Okey.


[![Caixa de diálogo Novo projeto](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Figura 01**: caixa de diálogo Novo projeto ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image2.png))


Quando você cria um novo aplicativo ASP.NET MVC, o **criar o projeto de teste de unidade** caixa de diálogo aparece (veja a Figura 2). Essa caixa de diálogo permite que você crie um projeto separado em sua solução para testar seu aplicativo ASP.NET MVC. Selecione a opção **não, não crie um projeto de teste de unidade** e clique no **Okey** botão.


[![Criar caixa de diálogo de teste de unidade](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Figura 02**: Criar diálogo de teste de unidade ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image4.png))


Depois do ASP.NET MVC novo aplicativo é criado. Você verá várias pastas e arquivos na janela do Gerenciador de soluções. Em particular, você verá três pastas denominadas modelos, exibições e controladores. Como você pode imaginar a partir dos nomes de pasta, essas pastas contêm os arquivos para a implementação de modelos, exibições e controladores.

Se você expandir a pasta controladores, você deve ver um arquivo chamado AccountController.cs e um arquivo chamado HomeController.cs. Se você expandir a pasta de modos de exibição, você verá três subpastas nomeadas de conta, início e compartilhado. Se você expandir a pasta base, você verá dois arquivos adicionais chamados aspx e About (veja a Figura 3). Esses arquivos fazem parte do aplicativo de exemplo incluído com o modelo ASP.NET MVC padrão.


[![A janela do Gerenciador de soluções](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Figura 03**: A janela do Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image6.png))


Você pode executar o aplicativo de exemplo, selecionando a opção de menu **depurar, iniciar depuração**. Como alternativa, você pode pressionar a tecla F5.

Quando você executa um aplicativo ASP.NET, será exibida a caixa de diálogo na Figura 4 recomenda que você habilite o modo de depuração. Clique no botão Okey e o aplicativo será executado.


[![Caixa de diálogo depuração não habilitada](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Figura 04**: caixa de diálogo de depuração não habilitada ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image8.png))


Quando você executa um aplicativo ASP.NET MVC, o Visual Studio inicia o aplicativo no navegador da web. O aplicativo de exemplo consiste em apenas duas páginas: a página de índice e a página sobre. Quando o aplicativo é iniciado pela primeira vez, é exibida a página de índice (consulte a Figura 5). Você pode navegar para a página sobre clicando no link do menu na parte superior direita do aplicativo.


[![A página de índice](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Figura 05**: A página de índice ([clique para exibir a imagem em tamanho normal](understanding-models-views-and-controllers-cs/_static/image11.png))


Observe as URLs na barra de endereços do navegador. Por exemplo, quando você clica no link do menu sobre, a URL na barra de endereços do navegador é alterada para **/Home/About**.

Se você fechar a janela do navegador e retorne ao Visual Studio, você não conseguirá localizar um arquivo com o caminho inicial/sobre. Os arquivos não existem. Como é possível?

## <a name="a-url-does-not-equal-a-page"></a>Uma URL não é igual a uma página

Quando você compila um aplicativo Web Forms do ASP.NET tradicional ou um aplicativo de Active Server Pages, há uma correspondência direta entre a URL e uma página. Se você solicitar uma página chamada SomePage.aspx do servidor, em seguida, melhor que haja uma página em disco chamado SomePage.aspx. Se o arquivo SomePage.aspx não existir, você obterá um feio **404 - página não encontrada** erro.

Ao criar um aplicativo ASP.NET MVC, por outro lado, não há nenhuma correspondência entre a URL que você digitar na barra de endereços do navegador e os arquivos que você encontrar em seu aplicativo. Em um aplicativo ASP.NET MVC, uma URL corresponde a uma ação do controlador em vez de uma página no disco.

Em um aplicativo tradicional do ASP.NET ou ASP, solicitações de navegador são mapeadas para as páginas. Em um aplicativo ASP.NET MVC, por outro lado, as solicitações do navegador são mapeadas para ações do controlador. Um aplicativo ASP.NET Web Forms é centrado em conteúdo. Um aplicativo ASP.NET MVC, por outro lado, é voltada a lógica do aplicativo.

## <a name="understanding-aspnet-routing"></a>Noções básicas sobre roteamento do ASP.NET

Uma solicitação do navegador é mapeada para uma ação do controlador por meio de um recurso da estrutura do ASP.NET chamada *roteamento do ASP.NET*. O roteamento do ASP.NET é usado pela estrutura do ASP.NET MVC para *rota* solicitações de entrada para ações do controlador.

O roteamento do ASP.NET usa uma tabela de rotas para manipular solicitações de entrada. Essa tabela de rota é criada quando seu aplicativo web é iniciado pela primeira vez. A tabela de rotas é configurado no arquivo global asax. O arquivo global asax de MVC padrão está contido na listagem 1.

**Listagem 1 - global. asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Quando um aplicativo ASP.NET é iniciado, o aplicativo\_método Start () é chamado. Na listagem 1, este método chama o método RegisterRoutes() e o método RegisterRoutes() cria a tabela de rotas padrão.

A tabela de rotas padrão consiste em uma rota. Essa rota padrão interrompe todas as solicitações recebidas em três segmentos (um segmento de URL é qualquer coisa entre barras "/"). O primeiro segmento é mapeado para um nome de controlador, o segundo segmento é mapeado para um nome de ação e o segmento final é mapeado para um parâmetro passado para a ação chamada ID.

Por exemplo, considere a seguinte URL:

Produto/3/detalhes

Essa URL é analisada em três parâmetros como este:

Controlador = o produto

Ação = detalhes

ID = 3

A rota padrão definida no arquivo global. asax inclui valores padrão para todos os três parâmetros. O padrão controlador Home é, a ação padrão é o índice e o Id padrão é uma cadeia de caracteres vazia. Com esses padrões em mente, considere como a URL a seguir é analisada:

/ Funcionário

Essa URL é analisada em três parâmetros como este:

Controlador = funcionário

Ação = índice

ID =

Por fim, se você abrir um aplicativo ASP.NET MVC sem fornecer qualquer URL (por exemplo, `http://localhost`) e em seguida, a URL é analisada como este:

Controlador = início

Ação = índice

ID =

A solicitação é encaminhada para a ação Index () na classe HomeController.

## <a name="understanding-controllers"></a>Noções básicas sobre controladores

Um controlador é responsável por controlar a maneira que um usuário interage com um aplicativo MVC. Um controlador contém a lógica de controle de fluxo para um aplicativo ASP.NET MVC. Um controlador determina qual resposta para enviar de volta para um usuário quando um usuário faz uma solicitação do navegador.

Um controlador é apenas uma classe (por exemplo, uma classe Visual Basic ou c#). O exemplo de aplicativo ASP.NET MVC inclui um controlador chamado HomeController.cs localizado na pasta controladores. O conteúdo do arquivo HomeController.cs é reproduzido na listagem 2.

**Listagem 2 - HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Observe que o HomeController tem dois métodos chamados Index () e About (). Esses dois métodos correspondem a duas ações expostas pelo controlador. Índice/URL /Home invoca o método de HomeController.Index() e a URL /Home/sobre invoca o método HomeController.About().

Qualquer método público em um controlador é exposto como uma ação do controlador. Você precisa ter cuidado com isso. Isso significa que qualquer método público contido em um controlador pode ser chamado por qualquer pessoa com acesso à Internet, digitando a URL correta em um navegador.

## <a name="understanding-views"></a>Noções básicas sobre exibições

As ações do controlador de dois expostas pela classe HomeController, index () e About (), retornam um modo de exibição. Um modo de exibição contém a marcação HTML e o conteúdo que é enviado ao navegador. Um modo de exibição é o equivalente de uma página, ao trabalhar com um aplicativo ASP.NET MVC.

Você deve criar seus modos de exibição no local certo. A ação HomeController.Index() retorna uma exibição localizada no seguinte caminho:

\Views\Home\Index.aspx

A ação HomeController.About() retorna uma exibição localizada no seguinte caminho:

\Views\Home\About.aspx

Em geral, se você quiser retornar uma exibição para uma ação do controlador, em seguida, você precisa criar uma subpasta na pasta modos de exibição com o mesmo nome que seu controlador. Dentro da subpasta, você deve criar um arquivo. aspx com o mesmo nome que a ação do controlador.

O arquivo na listagem 3 contém a exibição About.

**Listagem 3 - About**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Se você ignorar a primeira linha na listagem 3, a maioria do resto do modo de exibição consiste em HTML padrão. Você pode modificar o conteúdo do modo de exibição, inserindo qualquer HTML que você deseja aqui.

Um modo de exibição é muito semelhante a uma página em páginas ASP ou ASP.NET Web Forms. Um modo de exibição pode conter scripts e o conteúdo HTML. Você pode escrever os scripts em seu .NET favorita (por exemplo, c# ou Visual Basic .NET) de linguagem de programação. Você pode usar scripts para exibir o conteúdo dinâmico como o banco de dados.

## <a name="understanding-models"></a>Noções básicas sobre modelos

Discutimos controladores e discutimos modos de exibição. O último tópico que precisamos para discutir é modelos. O que é um modelo do MVC?

Um modelo MVC contém todas a lógica do aplicativo que não está contida em uma exibição ou um controlador. O modelo deve conter todos os seus lógica de negócios do aplicativo, a lógica de validação e a lógica de acesso de banco de dados. Por exemplo, se você estiver usando o Entity Framework da Microsoft para acessar seu banco de dados, em seguida, você criaria suas classes de Entity Framework (arquivo. edmx) na pasta de modelos.

Um modo de exibição deve conter apenas a lógica relacionada à geração de interface do usuário. Um controlador deve conter apenas o mínimo necessário de lógica necessária para retornar a exibição à direita ou redirecionar o usuário para outra ação (controle de fluxo). Todo o resto deve estar contido no modelo.

Em geral, você deve se esforçar para controladores fino e modelos de fat. Os métodos do controlador devem conter apenas algumas linhas de código. Se uma ação do controlador obtém muito fat, você deve considerar passar a lógica para uma nova classe na pasta de modelos.

## <a name="summary"></a>Resumo

Este tutorial forneceu uma visão geral de alto nível das diferentes partes do ASP.NET MVC aplicativo da web. Você aprendeu como o roteamento do ASP.NET mapeia solicitações recebidas do navegador para ações do controlador específico. Você aprendeu como controladores de orquestrar como modos de exibição são retornados ao navegador. Por fim, você aprendeu como os modelos contêm lógica de acesso de banco de dados, validação e negócios de aplicativos.
