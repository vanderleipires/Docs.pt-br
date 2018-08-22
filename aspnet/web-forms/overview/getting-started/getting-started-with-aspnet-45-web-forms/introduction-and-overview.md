---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Esta série de tutoriais passo a passo ensina as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: ad5e97cd596e146f742c4c5e882d3938005070d1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830408"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais passo a passo ensina os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. [ASP.NET Web Forms do teste](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Teste seu conhecimento e reforçar os conceitos-chave executando o teste do ASP.NET Web Forms. Este teste foi desenvolvido especificamente desde o conteúdo contido nessa série de tutoriais. Cada pergunta em teste fornece uma explicação juntamente com links para orientações adicionais.


## <a name="introduction"></a>Introdução

Esta série de tutoriais orienta você pelas etapas necessárias para criar um aplicativo Web Forms do ASP.NET usando o Visual Studio Express 2013 para Web e ASP.NET 4.5.

O aplicativo, você cria é denominado **Wingtip Toys**. É um exemplo simplificado de um site de armazenamento de front-que vende itens online. Esta série de tutoriais destaca os novos recursos disponíveis no ASP.NET 4.5.

Comentários são bem-vindos e podemos fazer todos os esforços para atualizar esta série de tutoriais com base em suas sugestões.

### <a name="download-completed-project"></a>Projeto de download concluído

Você pode baixar um projeto c# que contém o tutorial concluído.

- [Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Revise o conteúdo, fazendo o teste de Web Forms do ASP.NET relacionado

Depois de concluir este tutorial, teste seu conhecimento e reforçar os conceitos-chave por meio de [teste do ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Este teste foi desenvolvido especificamente desde o conteúdo contido nessa série de tutoriais. Cada pergunta em teste fornece uma explicação juntamente com links para orientações adicionais.

- [ASP.NET Web Forms do teste](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Público-alvo

O público-alvo desta série de tutoriais é os desenvolvedores experientes novatos no Web Forms do ASP.NET. Um desenvolvedor interessado nessa série de tutoriais deve ter as seguintes capacidades:

- Esteja familiarizado com um objeto orientado a linguagem de programação (OOP)
- Esteja familiarizado com conceitos de desenvolvimento da Web (HTML, CSS e JavaScript)
- Conheça os conceitos de banco de dados relacional
- Conheça os conceitos de arquitetura de n camadas

Se você estiver interessado em analisar as áreas listadas acima, considere a possibilidade de revisar o conteúdo a seguir:

- [Introdução ao Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desenvolvimento na Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Banco de dados relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitetura de várias camadas](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Recursos do aplicativo

Os recursos do Web Form do ASP.NET apresentados nesta série incluem:

- O projeto de aplicativo Web (não o projeto de Site da Web)
- Web Forms
- Páginas mestras, configuração
- Bootstrap
- Entity Framework Code First, LocalDB
- Validação de solicitação
- Fortemente tipado a controles de dados, da associação, anotações de dados de modelo e provedores de valor
- SSL e OAuth
- O ASP.NET Identity, configuração e autorização
- Validação não invasiva
- Roteamento
- Tratamento de erros do ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tarefas e cenários de aplicativo

Demonstrado nesta série de tarefas incluem:

- Criação, a revisão e executar o novo projeto
- Criando a estrutura de banco de dados
- Inicializando e a propagação do banco de dados
- Personalizando a interface do usuário usando estilos, gráficos e uma página mestra
- Adicionar páginas e navegação
- Exibindo detalhes de menu e os dados de produto
- Criando um carrinho de compras
- Suporte a adição de SSL e OAuth
- Adicionando um método de pagamento
- Incluindo uma função de administrador e um usuário para o aplicativo
- Restringir o acesso a páginas específicas e pasta
- Carregar um arquivo para o aplicativo web
- Implementar validação de entrada
- Registrando as rotas para o aplicativo web
- Implementando o tratamento de erros e log de erros

## <a name="overview"></a>Visão geral

Se você for novo no Web Forms do ASP.NET, mas têm familiaridade com conceitos de programação, você tem o direito tutorial. Se você já estiver familiarizado com o ASP.NET Web Forms, você pode aproveitar esta série de tutoriais, os novos recursos disponíveis no ASP.NET 4.5. Se você estiver familiarizado com conceitos de programação e Web Forms do ASP.NET, consulte os tutoriais adicionais fornecidos no Web Forms [guia de Introdução](../../../index.md) seção no site da Web do ASP.NET.

O específico **mais recente** ASP.NET 4.5 recursos fornecidos no Web Forms do série de tutoriais incluem o seguinte:

- Uma interface do usuário simple para a criação de projetos que oferecem [dar suporte a várias estruturas de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC e API da Web).
- [O Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), uma estrutura de layout e temas que fornece recursos dinâmicos de design e temas.
- [O ASP.NET Identity](../../../../identity/index.md), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), uma atualização para o Entity Framework que permite recuperar e manipular dados tão fortemente tipada de objetos, acessar os dados de forma assíncrona, tratar falhas transitórias de conexão e as instruções SQL de log.

Para obter uma lista completa dos recursos do ASP.NET 4.5, consulte [ASP.NET e Web Tools para notas de versão do Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>O aplicativo de exemplo do Wingtip Toys

As capturas de tela a seguir fornecem uma visão rápida do aplicativo ASP.NET Web forms que serão criados nessa série de tutoriais. Quando você executa o aplicativo do Visual Studio Express 2013 para Web, você verá a seguinte página inicial da web.

![A Wingtip Toys - página padrão](introduction-and-overview/_static/image1.png)

Você pode registrar como um novo usuário, ou faça logon como um usuário existente. Na parte superior, a navegação é fornecida para cada categoria de produto, recuperando os produtos disponíveis do banco de dados.

Selecionando o link de produtos, você poderá ver uma lista de todos os produtos disponíveis.

![A Wingtip Toys - produtos](introduction-and-overview/_static/image2.png)

Você também pode ver os detalhes do produto individuais selecionando qualquer um dos produtos listados.

![A Wingtip Toys - detalhes do produto](introduction-and-overview/_static/image3.png)

Como um usuário, você pode registrar e faça logon usando a funcionalidade padrão do modelo de formulários da Web. Este tutorial também explica como fazer logon usando uma conta existente do Gmail. Além disso, você pode fazer logon como administrador para adicionar e remover os produtos de banco de dados.

![A Wingtip Toys - faça logon no](introduction-and-overview/_static/image4.png)

Depois de fazer logon como um usuário, você pode adicionar produtos ao carrinho de compras e check-out com o PayPal. Observe que este aplicativo de exemplo é projetado para funcionar com a área restrita do desenvolvedor do PayPal. Nenhuma transação de dinheiro real ocorrerá.

![A Wingtip Toys - carrinho de compras](introduction-and-overview/_static/image5.png)

PayPal confirmará a sua conta, ordem e informações de pagamento.

![A Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Depois de retornar do PayPal, você pode revisar e concluir o pedido.

![A Wingtip Toys - revisão de ordem](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente.

Esta série de tutoriais usa o Microsoft Visual Studio Express 2013 para Web. Você pode usar o Microsoft Visual Studio Express 2013 para Web ou Microsoft Visual Studio 2013 para concluir esta série de tutoriais.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecido como Visual Studio em toda esta série de tutoriais.


Se você já tiver uma versão do Visual Studio instalada, o processo de instalação instalará o Visual Studio 2013 ou Microsoft Visual Studio Express 2013 para Web ao lado da versão existente. Sites criados em versões anteriores podem ser abertos no Visual Studio 2013 e continuam a abrir nas versões anteriores.

> [!NOTE] 
> 
> Este passo a passo pressupõe que você selecionou a *desenvolvimento Web* coleção de configurações na primeira vez que você iniciou o Visual Studio. Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Baixe o aplicativo de exemplo

Depois de instalar os pré-requisitos, você está pronto para começar a criar o novo projeto Web que é apresentado nessa série de tutoriais. Se você quiser **opcionalmente** executar o aplicativo de exemplo que cria esta série de tutoriais, você pode baixá-lo do site de exemplos do MSDN. Este download contém o seguinte:

- O aplicativo de exemplo na *WingtipToys* pasta.
- Os recursos usados para criar o aplicativo de exemplo na *ativos WingtipToys* pasta na *WingtipToys* pasta.

#### <a name="download-the-file-from-msdn-samples-site"></a>Baixe o arquivo do site de exemplos do MSDN:

[Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

O download é um <em>. zip</em> arquivo. Para ver o projeto completo que cria esta série de tutoriais, encontre e selecione os <em>c#</em>pasta o <em>. zip</em> arquivo. Salvar a <em>c#</em> folderto a pasta que você pode usar para trabalhar com projetos do Visual Studio 2013. Por padrão, a pasta de projetos do Visual Studio 2013 é o seguinte:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Renomeie o ***c#*** pasta a ser ***WingtipToys***.

> [!NOTE]
> Se você já tiver uma pasta chamada *WingtipToys* na sua pasta de projetos, renomeie temporariamente essa pasta existente antes de renomear o *c#* pasta a ser *WingtipToys*.


Para executar o projeto concluído, abra o *WingtipToys* pasta e clique duas vezes o *WingtipToys.sln* arquivo. Visual Studio 2013 abrirá o projeto. Em seguida, clique com botão direito do *default. aspx* do arquivo na janela do Gerenciador de soluções e clique em Exibir no navegador do menu de atalho.

### <a name="tutorial-support-and-comments"></a>Comentários e suporte de tutorial

Use a seção de p e r incluída com o [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) exemplo tiver perguntas ou comentários.

Comentários sobre esta série de tutoriais são boas-vindos e quando esta série de tutoriais é atualizada todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas nos comentários tutoriais.

Quando ocorrer um erro durante o desenvolvimento, ou se o site da Web não é executado corretamente, as mensagens de erro podem fornecer pistas complexas para a origem do problema ou não podem explicar como corrigi-lo. Para ajudar você com alguns cenários comuns de problema, você também pode usar o [fóruns do ASP.NET](https://forums.asp.net/) ou na seção de p e r incluídas com o [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) exemplo. Se você receber uma mensagem de erro ou se algo não funciona ao percorrer os tutoriais, certifique-se de verificar os locais acima.

> [!div class="step-by-step"]
> [Avançar](create-the-project.md)
