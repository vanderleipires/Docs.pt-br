---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Guia de Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 | Microsoft Docs"
author: Erikre
description: "Esta série de tutoriais passo a passo ensinam os fundamentos da compilação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ee3e244c4ed29384d11c7acc1440692d3f9b23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Guia de Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013
====================
Por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais passo a passo ensinam os fundamentos da compilação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. [Teste de formulários da Web do ASP.NET](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Testar seu conhecimento e reforçar os conceitos-chave executando o teste do ASP.NET Web Forms. Este teste foi projetado especificamente do conteúdo contido nesta série tutorial. Cada questão no teste fornece uma explicação junto com links para diretrizes adicionais.


## <a name="introduction"></a>Introdução

Esta série de tutoriais orienta você pelas etapas necessárias para criar um aplicativo de Web Forms do ASP.NET usando o Visual Studio Express 2013 para Web e ASP.NET 4.5.

O aplicativo que você criará é nomeado **Wingtip Toys**. É um exemplo simplificado de um site de repositório front que vende itens online. Esta série de tutoriais destaca os novos recursos disponíveis no ASP.NET 4.5.

Comentários são bem-vindos e faremos todos os esforços para atualizar esta série de tutoriais com base em suas sugestões.

### <a name="download-completed-project"></a>Projeto de download foi concluída

Você pode baixar um projeto c# que contém o tutorial concluído.

- [Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Revise o conteúdo fazendo o teste de Web Forms do ASP.NET relacionado

Depois de concluir este tutorial, testar seu conhecimento e reforçar os principais conceitos colocando o [ASP.NET Web Forms teste](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Este teste foi projetado especificamente do conteúdo contido nesta série tutorial. Cada questão no teste fornece uma explicação junto com links para diretrizes adicionais.

- [Teste de formulários da Web do ASP.NET](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Público-alvo

O público-alvo da série tutorial é desenvolvedores experientes que forem novos no Web Forms do ASP.NET. Um desenvolvedor interessado nesta série de tutoriais deve ter as seguintes capacidades:

- Familiarizados com um objeto orientado a linguagem de programação (OOP)
- Familiarizado com conceitos de desenvolvimento da Web (HTML, CSS, JavaScript)
- Conhecer os conceitos de banco de dados relacional
- Conhecer os conceitos de arquitetura de n camadas

Se você estiver interessado em examinar as áreas listadas acima, considere a possibilidade de revisar o conteúdo a seguir:

- [Guia de Introdução ao Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desenvolvimento na Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Banco de dados relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitetura de várias camadas](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Recursos de aplicativo

Os recursos de formulário da Web ASP.NET apresentados nesta série incluem:

- O projeto de aplicativo da Web (não o projeto de Site da Web)
- Web Forms
- Páginas mestras, configuração
- inicialização
- Entity Framework Code primeiro, o LocalDB
- Validação de solicitação
- Fortemente tipado a controles de dados, modelo de associação, as anotações de dados e provedores de valor
- SSL e OAuth
- Identidade do ASP.NET, configuração e autorização
- Validação discreta
- Roteamento
- Tratamento de erros do ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tarefas e cenários de aplicativos

Tarefas demonstradas nesta série:

- Criando, revisando e executando o novo projeto
- Criando a estrutura de banco de dados
- Inicializando e a propagação do banco de dados
- Personalizando a interface do usuário usando estilos, gráficos e uma página mestra
- Adição de páginas e navegação
- Exibindo detalhes do menu e os dados de produto
- Criando um carrinho de compras
- Suporte a SSL adicionando e OAuth
- Adicionando um método de pagamento
- Incluindo uma função de administrador e um usuário para o aplicativo
- Restringir o acesso a páginas específicas e pasta
- Carregando um arquivo para o aplicativo web
- Implementação de validação de entrada
- Registrando as rotas para o aplicativo web
- Implementando o tratamento de erros e log de erros

## <a name="overview"></a>Visão Geral

Se você for novo no Web Forms do ASP.NET mas tem familiaridade com conceitos de programação, você tem o direito tutorial. Se você já estiver familiarizado com o Web Forms do ASP.NET, você pode aproveitar essa série de tutoriais pelos novos recursos disponíveis no ASP.NET 4.5. Se você estiver familiarizado com conceitos de programação e Web Forms do ASP.NET, consulte os tutoriais adicionais fornecidos no Web Forms [Introdução](../../../index.md) seção no site da Web do ASP.NET.

Específico **mais recente** ASP.NET 4.5 recursos fornecido nesta Web Forms série de tutoriais incluem o seguinte:

- Uma interface do usuário simple para a criação de projetos que oferecem [suporte para várias estruturas ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC e API da Web).
- [Inicialização](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), uma estrutura de layout e temas que fornece recursos de design e temas responsivos.
- [Identidade do ASP.NET](../../../../identity/index.md), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), objetos de uma atualização para o Entity Framework que permite recuperar e manipular dados como fortemente tipados, acessar os dados de forma assíncrona, tratar falhas transitórias de conexão e instruções SQL de log.

Para obter uma lista completa dos recursos do ASP.NET 4.5, consulte [ASP.NET e Web Tools para notas de versão do Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>O aplicativo de exemplo do Wingtip Toys

As capturas de tela a seguir fornecem uma visão rápida do aplicativo de formulários da Web do ASP.NET que você criará nesta série tutorial. Quando você executa o aplicativo do Visual Studio Express 2013 para Web, você verá a página inicial.

![Wingtip Toys - página padrão](introduction-and-overview/_static/image1.png)

Você pode registrar como um novo usuário, ou faça logon como um usuário existente. Navegação é fornecida na parte superior de cada categoria de produto, recuperando os produtos disponíveis do banco de dados.

Selecionando o link de produtos, você poderá ver uma lista de todos os produtos disponíveis.

![Wingtip Toys - produtos](introduction-and-overview/_static/image2.png)

Você também pode ver detalhes do produto individual selecionando qualquer um dos produtos listados.

![Wingtip Toys - detalhes do produto](introduction-and-overview/_static/image3.png)

Como um usuário, você pode registrar e faça logon usando a funcionalidade padrão do modelo de Web Forms. Este tutorial também explica como fazer logon usando uma conta existente do Gmail. Além disso, você pode fazer logon como administrador para adicionar e remover produtos do banco de dados.

![Wingtip Toys - login](introduction-and-overview/_static/image4.png)

Depois de fazer logon como um usuário, você pode adicionar produtos ao carrinho de compras e check-out com o PayPal. Observe que este aplicativo de exemplo é projetado para funcionar com a área restrita do desenvolvedor do PayPal. Nenhuma transação de dinheiro real ocorrerá.

![Wingtip Toys - carrinho de compras](introduction-and-overview/_static/image5.png)

PayPal confirmar sua conta, a ordem e a informações de pagamento.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Após o retorno do PayPal, você pode revisar e concluir o pedido.

![Wingtip Toys - revisão de ordem](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web). O .NET Framework é instalado automaticamente.

Esta série de tutoriais usa o Microsoft Visual Studio Express 2013 para Web. Você pode usar o Microsoft Visual Studio Express 2013 para Web ou Microsoft Visual Studio 2013 para concluir este tutorial série.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecida como Visual Studio em toda a série de tutoriais.


Se você já tiver uma versão do Visual Studio instalada, o processo de instalação irá instalar Visual Studio 2013 ou o Microsoft Visual Studio Express 2013 para Web próximo à versão existente. Sites criados em versões anteriores podem ser abertos no Visual Studio 2013 e continuam a abrir em versões anteriores.

> [!NOTE] 
> 
> Este passo a passo pressupõe que você selecionou o *desenvolvimento Web* conjunto de configurações na primeira vez que você iniciar o Visual Studio. Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/en-us/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Baixe o aplicativo de exemplo

Depois de instalar os pré-requisitos, você está pronto para começar a criar o novo projeto da Web que é apresentado nesta série tutorial. Se você quiser **opcionalmente** executar o aplicativo de exemplo que cria esta série de tutoriais, você pode baixá-lo do site de exemplos do MSDN. Este download contém o seguinte:

- O aplicativo de exemplo no *WingtipToys* pasta.
- Os recursos usados para criar o aplicativo de exemplo no *WingtipToys ativos* pasta o *WingtipToys* pasta.

#### <a name="download-the-file-from-msdn-samples-site"></a>Baixe o arquivo do site de exemplos do MSDN:

[Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

O download é um *. zip* arquivo. Para ver o projeto completo que cria esta série de tutoriais, encontre e selecione o *c#*pasta o *. zip* arquivo. Salve o *c#* folderto a pasta que você pode usar para trabalhar com projetos do Visual Studio 2013. Por padrão, a pasta de projetos do Visual Studio 2013 é o seguinte:

**C:\Users\*****&lt;username&gt;* \Documents\Visual 2013\Projects Studio**

Renomear o ***c#*** pasta ***WingtipToys***.

> [!NOTE]
> Se você já tiver uma pasta chamada *WingtipToys* na sua pasta de projetos, renomeie temporariamente essa pasta existente antes de renomear o *c#* pasta *WingtipToys*.


Para executar o projeto concluído, abra o *WingtipToys* pasta e clique duas vezes o *WingtipToys.sln* arquivo. Visual Studio 2013 abrirá o projeto. Em seguida, clique com botão direito do *Default.aspx* arquivo na janela do Gerenciador de soluções e clique em Exibir no navegador, no menu de atalho.

### <a name="tutorial-support-and-comments"></a>Comentários e suporte de tutorial

Use a seção de p e r incluída com o [Introdução ao Web Forms do ASP.NET 4.5 e o Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) exemplo (c#) para qualquer pergunta ou comentário.

Comentários sobre esta série de tutoriais são bem-vindos e quando esta série de tutoriais é atualizado todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas nos comentários tutoriais.

Quando ocorrer um erro durante o desenvolvimento, ou se o site da Web não funcionar corretamente, as mensagens de erro podem fornecer pistas complexas para a origem do problema ou não podem explicar como corrigi-lo. Para ajudá-lo com alguns cenários comuns de problema, você também pode usar o [ASP.NET fóruns](https://forums.asp.net/) ou na seção de p e r incluídas com o [Introdução ao Web Forms do ASP.NET 4.5 e o Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) exemplo. Se você receber uma mensagem de erro ou algo não funciona ao percorrer os tutoriais, certifique-se de verificar os locais acima.

>[!div class="step-by-step"]
[Avançar](create-the-project.md)
