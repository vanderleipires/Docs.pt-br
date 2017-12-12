---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 2 abrange controladores."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a>Parte 2: controladores
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 2 abrange controladores.


Estruturas da web tradicional, URLs de entrada geralmente são mapeados para arquivos no disco. Por exemplo: uma solicitação para uma URL, como "/ products. aspx" ou "/ Products.php" pode ser processado por um arquivo de "Products. aspx" ou "Products.php".

Estruturas MVC baseado na Web mapeiam URLs para o código de servidor de forma ligeiramente diferente. Em vez de mapeamento de URLs de entrada aos arquivos, em vez disso, eles são mapeados URLs para métodos nas classes. Essas classes são chamadas de "Controladores" e eles são responsáveis pelo processamento de solicitações HTTP de entrada, manipulação de entrada do usuário, recuperar e salvar dados e determinar a resposta para enviar de volta ao cliente (exibição de HTML, baixar um arquivo, Redirecione para outro URL, etc.).

## <a name="adding-a-homecontroller"></a>Adicionando um HomeController

Vamos começar nosso aplicativo de repositório de música MVC adicionando uma classe de controlador que tratará as URLs para a Home page do nosso site. Vamos seguem as convenções de nomenclatura padrão do ASP.NET MVC e chamá-lo HomeController.

Clique na pasta "Controladores" no Gerenciador de soluções e selecione "Adicionar" e "Controlador …" comando:

![](mvc-music-store-part-2/_static/image1.jpg)

Isso abrirá a caixa de diálogo "Adicionar controlador". Nome do controlador "HomeController" e pressione o botão Adicionar.

![](mvc-music-store-part-2/_static/image1.png)

Isso criará um novo arquivo, HomeController, com o código a seguir:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Para iniciar da maneira mais simples possível, vamos substituir o método de índice com um método simples que retorna apenas uma cadeia de caracteres. Verifique duas alterações:

- Alterar o método para retornar uma cadeia de caracteres em vez de um ActionResult
- Alterar a instrução return para retornar "Hello de início"

Agora, o método deve ser assim:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Agora vamos executar o site. Podemos começar nosso servidor web e experimentar o site usando qualquer um dos seguintes:

- Escolha o item de menu Iniciar depuração de depuração ⇨
- Clique no botão de seta verde na barra de ferramentas![](mvc-music-store-part-2/_static/image2.jpg)
- Use o atalho de teclado F5.

Usar qualquer uma das etapas acima compilar nosso projeto e, em seguida, fazer com que o ASP.NET Development Server é integrado ao Visual Web Developer para iniciar. Uma notificação será exibida no canto inferior da tela para indicar que o servidor de desenvolvimento do ASP.NET foi iniciado e mostrará o número da porta que está sendo executado em.

![](mvc-music-store-part-2/_static/image2.png)

O Visual Web Developer abrirá automaticamente uma janela do navegador cuja URL aponta para o nosso servidor web. Isso nos permitirá testar rapidamente o nosso aplicativo web:

![](mvc-music-store-part-2/_static/image3.png)

Okey, que era muito rápido – criamos um novo site, adicionado a uma função de três linha, e temos o texto em um navegador. Não rocket ciência, mas é um começo.

*Observação: O Visual Web Developer inclui o ASP.NET Development Server, que irá executar o seu site em um número aleatório livre "porta". Na captura de tela acima, o site está em execução no `http://localhost:26641/`, de modo que ele está usando a porta 26641. O número da porta será diferente. Quando falamos sobre /Store/Browse like da URL neste tutorial, que irá após o número da porta. Supondo que um número de porta de 26641, navegando para armazenamento/procurar significa navegando para `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Adicionando um StoreController

Adicionamos um HomeController simple que implementa a Home Page do nosso site. Agora vamos adicionar outro controlador que usaremos para implementar a funcionalidade de navegação do nosso repositório de música. Controlador nosso repositório oferecerá suporte a três cenários:

- Uma página de listagem de gêneros de música em nosso repositório de música
- Uma página de pesquisa que lista todos os álbuns de música em um gênero específico
- Uma página de detalhes que mostra informações sobre um álbum de música específico

Vamos começar adicionando uma nova classe StoreController... Se você ainda não fez isso, param o aplicativo fechar o navegador ou selecionando o item de menu de parar depuração ⇨ de depuração.

Agora, adicione um novo StoreController. Como com HomeController, faremos isso clicando duas vezes na pasta "Controladores" no Gerenciador de soluções e escolhendo Adicionar -&gt;item de menu do controlador

![](mvc-music-store-part-2/_static/image4.png)

Nosso novo StoreController já tem um método "Index". Vamos usar esse método "Index" implementar nossa página de listagem que lista todos os gêneros em nosso repositório de música. Nós adicionaremos dois métodos adicionais para implementar os dois outros cenários queremos que nossa StoreController tratar: Procurar e detalhes.

Esses métodos (índice, procurar e detalhes) em nosso controlador são chamados de "Ações do controlador", e como você já viu com o método de ação HomeController.Index (), seu trabalho é responder às solicitações de URL e (geral) determinar qual conteúdo deve ser enviada de volta para o navegador ou o usuário que invocou a URL.

Vamos começar nosso implementação StoreController alterando theIndex() método para retornar a cadeia de caracteres "Hello de Store.Index()" e vamos adicionar métodos semelhantes para Browse () e Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Execute o projeto novamente e procurar as seguintes URLs:

- / Store
- / Repositório/procurar
- / / Detalhes do repositório

Acessar essas URLs invocar os métodos de ação em nosso controlador e retorna respostas de cadeia de caracteres:

![](mvc-music-store-part-2/_static/image5.png)

Isso é ótimo, mas são cadeias de caracteres constantes apenas. Vamos torná-los dinâmicos, para que eles levar informações da URL e exibem-lo na saída da página.

Primeiro, vamos alterar o método de ação de navegação para recuperar um valor de querystring da URL. Podemos fazer isso adicionando um parâmetro "gênero" para nosso método de ação. Quando fazemos isso ASP.NET MVC passará automaticamente qualquer parâmetro de postagem querystring ou formulário denominado "gênero" para nosso método de ação quando ele é invocado.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Observação: Estamos usando o método de utilitário HttpUtility para limpar a entrada do usuário. Isso impede que os usuários de injeção de Javascript em nosso exibição com um link como /Store/Browse? Gênero =&lt;script&gt;window.location= 'http://hackersite.com'&lt;/script&gt;.*

Agora vamos procure/repositório/procurar? Gênero = Disco

![](mvc-music-store-part-2/_static/image6.png)

Vamos Avançar alterar a ação de detalhes para ler e exibir um parâmetro de entrada chamado ID. Ao contrário do nosso método anterior, nós não inserindo o valor da ID como um parâmetro querystring. Em vez disso, podemos será incorporá-lo diretamente na URL de si mesmo. Por exemplo: /Store/Details/5.

ASP.NET MVC permite fazer isso facilmente sem a necessidade de configurar nada. Convenção de roteamento de ASP.NET MVC padrão é tratar o segmento de URL após o nome do método de ação como um parâmetro denominado "ID". Se seu método de ação tem um parâmetro chamado ID, em seguida, o ASP.NET MVC automaticamente passará o segmento de URL para você como um parâmetro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Execute o aplicativo e navegue até /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Vamos recapitular o que fizemos até agora:

- Criamos um novo projeto ASP.NET MVC no Visual Web Developer
- Discutimos a estrutura de pasta básica de um aplicativo ASP.NET MVC
- Aprendemos como executar nosso site usando o ASP.NET Development Server
- Nós criamos duas classes do controlador: um HomeController e um StoreController
- Adicionamos os métodos de ação ao nosso controladores que respondem às solicitações de URL e retornam o texto para o navegador


>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-1.md)
[Próximo](mvc-music-store-part-3.md)
