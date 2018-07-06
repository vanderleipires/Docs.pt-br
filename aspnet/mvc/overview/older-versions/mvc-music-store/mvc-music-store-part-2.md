---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 2 aborda controladores.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841089"
---
<a name="part-2-controllers"></a>Parte 2: controladores
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 2 aborda controladores.


Com estruturas da web tradicional, URLs de entrada são mapeadas normalmente a arquivos no disco. Por exemplo: uma solicitação para uma URL como "/ products. aspx" ou "/ products" pode ser processada por um arquivo de "Products. aspx" ou "Products".

Estruturas MVC baseado na Web mapeiam URLs para o código do servidor de forma ligeiramente diferente. Em vez de mapeamento de URLs de entrada aos arquivos, em vez disso, eles mapeiam URLs para métodos nas classes. Essas classes são chamadas de "Controladores" e eles são responsáveis pelo processamento de solicitações HTTP de entrada, manipulando a entrada do usuário, recuperar e salvar dados e determinar a resposta para enviar de volta para o cliente (exibição de HTML, baixar um arquivo, redirecionar para outra URL, etc.).

## <a name="adding-a-homecontroller"></a>Adicionando um HomeController

Vamos começar nosso aplicativo de Store de música do MVC, adicionando uma classe de controlador que tratará as URLs para a Home page do nosso site. Vamos seguir as convenções de nomenclatura padrão do ASP.NET MVC e chamá-lo HomeController.

Clique com botão direito na pasta "Controladores" no Gerenciador de soluções e selecione "Add" e, em seguida, o comando "Controlador...":

![](mvc-music-store-part-2/_static/image1.jpg)

Isso abrirá a caixa de diálogo "Adicionar controlador". Nomeie o controlador "HomeController" e pressione o botão Adicionar.

![](mvc-music-store-part-2/_static/image1.png)

Isso criará um novo arquivo, HomeController.cs, com o código a seguir:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Para começar a maneira mais simples possível, vamos substituir o método de índice com um método simples que retorna apenas uma cadeia de caracteres. Podemos fazer duas alterações:

- Alterar o método para retornar uma cadeia de caracteres em vez de um ActionResult
- Altere a instrução return para retornar "Olá, na página inicial"

Agora, o método deve ser assim:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Agora vamos executar o site. Podemos iniciar nosso servidor web e testar o site usando qualquer um dos seguintes:

- Escolha o item de menu de iniciar depuração ⇨ de depuração
- Clique no botão de seta verde na barra de ferramentas ![](mvc-music-store-part-2/_static/image2.jpg)
- Use o atalho de teclado, F5.

Usar qualquer uma das etapas acima compilar nosso projeto e, em seguida, fazer com que o ASP.NET Development Server que é integrado ao Visual Web Developer para iniciar. Uma notificação será exibida no canto inferior da tela para indicar que o ASP.NET Development Server foi iniciado e mostrará o número da porta que está em execução em.

![](mvc-music-store-part-2/_static/image2.png)

O Visual Web Developer, em seguida, será aberto automaticamente uma janela do navegador cuja URL aponta para o nosso servidor web. Isso nos permitirá testar rapidamente o nosso aplicativo web:

![](mvc-music-store-part-2/_static/image3.png)

Okey, que foi muito rápido – o que criamos um novo site, adicionado a uma função de linha de três, e temos o texto em um navegador. Não rocket ciência, mas é um começo.

*Observação: O Visual Web Developer inclui o ASP.NET Development Server, que executará o seu site em um número aleatório livre "porta". Na captura de tela acima, o site está em execução no `http://localhost:26641/`, de modo que ele está usando a porta 26641. O número da porta será diferente. Quando falamos sobre de /Store/Browse like as URLs neste tutorial, que irá após o número da porta. Supondo que um número de porta de 26641, navegando até/Store/procurar significa navegando até `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Adicionando um StoreController

Adicionamos um HomeController simple que implementa a Home Page do nosso site. Agora vamos adicionar outro controlador que usaremos para implementar a funcionalidade de navegação do nosso repositório de música. Nosso controlador de armazenamento dará suporte a três cenários:

- Uma página de listagem de gêneros música em nosso repositório de música
- Uma página de navegação que lista todos os álbuns de música em um gênero específico
- Uma página de detalhes que mostra informações sobre um álbum de música específico

Vamos começar adicionando uma nova classe StoreController... Se você ainda não fez isso, pare de executar o aplicativo fechar o navegador ou selecionando o item de menu de parar depuração ⇨ de depuração.

Agora, adicione um novo StoreController. Exatamente como fizemos com o HomeController, vamos fazer isso clicando duas vezes na pasta "Controladores de" dentro do Gerenciador de soluções e escolhendo Add -&gt;item de menu do controlador

![](mvc-music-store-part-2/_static/image4.png)

Nosso novo StoreController já tem um método "Index". Usaremos esse método "Index" implementar nossa página de listagem que lista todos os gêneros em nosso repositório de música. Também adicionaremos dois métodos adicionais para implementar os dois outros cenários queremos que nosso StoreController para lidar com: Procurar e detalhes.

Esses métodos (índice, procurar e detalhes) dentro do nosso controlador são chamados de "Ações do controlador" e como você já viu com o método de ação HomeController.Index (), seu trabalho é responder às solicitações de URL e (em termos gerais) determinar qual conteúdo deve ser enviada de volta para o navegador ou o usuário que invocou a URL.

Vamos começar nossa implementação StoreController alterando theIndex() método para retornar a cadeia de caracteres "Olá do Store.Index()" e vamos adicionar métodos semelhantes para Browse () e Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Execute o projeto novamente e procurar as URLs a seguir:

- / Store
- / Store/procura
- / Store/detalhes

Acessar essas URLs invocar os métodos de ação em nosso controlador e retornar respostas de cadeia de caracteres:

![](mvc-music-store-part-2/_static/image5.png)

Isso é ótimo, mas essas são cadeias de caracteres apenas constantes. Vamos torná-los dinâmicos, para que eles tenham informações da URL e exibem-la na saída de página.

Primeiro, vamos alterar o método de ação de navegação para recuperar um valor de cadeia de consulta da URL. Podemos fazer isso adicionando um parâmetro "gênero" ao nosso método de ação. Quando fazemos isso ASP.NET MVC passará automaticamente quaisquer parâmetros de postagem querystring ou formulário chamados "gênero" ao nosso método de ação quando ele é invocado.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Observação: Estamos usando o método de utilitário HttpUtility para limpar a entrada do usuário. Isso impede que os usuários de injeção de Javascript em nossa exibição com um link como /Store/Browse? Gênero =&lt;script&gt;Window 'http://hackersite.com'&lt;/script&gt;.*

Agora vamos navegar até/Store/procurar? Gênero = Disco

![](mvc-music-store-part-2/_static/image6.png)

Vamos em seguida, altere a ação de detalhes para ler e exibir um parâmetro de entrada chamado ID. Ao contrário de nosso método anterior, nós não incorporar o valor da ID como um parâmetro querystring. Em vez disso, inseriremos-lo diretamente na URL em si. Por exemplo: /Store/Details/5.

ASP.NET MVC nos permite fazer isso facilmente sem precisar configurar nada. Convenção de roteamento de ASP.NET MVC padrão é tratar o segmento de URL após o nome do método de ação como um parâmetro denominado "ID". Se seu método de ação tem um parâmetro chamado ID, em seguida, ASP.NET MVC automaticamente passará o segmento de URL para você como um parâmetro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Execute o aplicativo e navegue até /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Vamos recapitular o que fizemos até agora:

- Criamos um novo projeto ASP.NET MVC no Visual Web Developer
- Discutimos a estrutura de pasta básica de um aplicativo ASP.NET MVC
- Aprendemos como executar nosso site usando o ASP.NET Development Server
- Nós criamos duas classes de controlador: existe um HomeController e um StoreController
- Adicionamos os métodos de ação ao nosso controladores que respondem às solicitações de URL e retornam o texto para o navegador


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-1.md)
> [Próximo](mvc-music-store-part-3.md)
