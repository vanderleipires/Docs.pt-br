---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Laboratório prático: Criar um aplicativo de página única (SPA) com a API da Web do ASP.NET e Angular.js | Microsoft Docs'
author: rick-anderson
description: Em aplicativos web tradicionais, o cliente (navegador) inicia a comunicação com o servidor, solicitando uma página. O servidor processa a solicitação...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratório prático: Criar um aplicativo de página única (SPA) com a API da Web do ASP.NET e Angular.js
====================
por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](http://aka.ms/webcamps-training-kit)

> Em aplicativos web tradicionais, o cliente (navegador) inicia a comunicação com o servidor, solicitando uma página. O servidor processa a solicitação e envia o HTML da página para o cliente. Interações subsequentes com a página – por exemplo, o usuário navega para um link ou envia um formulário com dados – uma nova solicitação é enviada para o servidor e do início do fluxo novamente: o servidor processa a solicitação e envia uma nova página para o navegador em resposta à solicitação de ação de novo ED pelo cliente.
> 
> Em aplicativos de página única (SPAs) toda a página é carregada no navegador após a solicitação inicial, mas as interações subsequentes ocorrem por meio de solicitações do Ajax. Isso significa que o navegador precisa atualizar somente a parte da página que foi alterado; não é necessário para recarregar a página inteira. A abordagem SPA reduz o tempo gasto pelo aplicativo para responder a ações do usuário, resultando em uma experiência mais fluida.
> 
> A arquitetura de um SPA envolve determinados desafios que não estão presentes em aplicativos web tradicionais. No entanto, novas tecnologias, como a API da Web ASP.NET, estruturas de JavaScript como AngularJS e novos recursos de estilo fornecidos pelo CSS3 facilitam realmente projetar e criar SPAs.
> 
> Neste laboratório disponível na rede, você será tirar proveito dessas tecnologias para implementar nerd do teste, um site trívia baseado no conceito SPA. Você primeiro implementar a camada de serviço com a API da Web do ASP.NET para expor os pontos de extremidade necessários para recuperar as perguntas de teste e armazenar as respostas. Em seguida, você criará um avançados e capacidade de resposta da interface do usuário usando os efeitos de transformação AngularJS e CSS3.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Visão Geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Crie um serviço de API da Web ASP.NET para enviar e receber dados JSON
- Criar uma interface do usuário responsiva usando AngularJS
- Aprimorar a experiência de interface do usuário com transformações CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O exemplo a seguir é necessário para concluir este laboratório prático:

- [O Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou maior

<a id="Setup"></a>
### <a name="setup"></a>Configuração

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro.

1. Abra o Windows Explorer e navegue para o laboratório **fonte** pasta.
2. Clique duas vezes em **Setup.cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instale os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.

> [!NOTE]
> Verifique se que você selecionou todas as dependências para este laboratório antes de executar a instalação.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento de laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar a necessidade para adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada no **começar** pasta do exercício que permite que você execute cada exercício independentemente de outras pessoas. Esteja ciente de que os trechos de código que são adicionados durante um exercício estão ausentes dos seguintes iniciando soluções e podem não funcionar até que você concluiu o exercício. Dentro do código-fonte para um exercício, você também encontrará um **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente. Você pode usar essas soluções como orientação se você precisar de ajuda adicional usando este laboratório prático.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Criando uma API da Web](#Exercise1)
2. [Criando uma Interface do SPA](#Exercise2)

Tempo estimado para concluir este laboratório: **60 minutos**

> [!NOTE]
> Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida foi projetada para coincidir com um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma tarefa específica no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção. Se você escolher uma coleção de configurações diferentes para o seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Exercício 1: Criar uma API da Web

Uma das principais partes de um SPA é a camada de serviço. Ele é responsável pelo processamento de chamadas Ajax enviadas pela interface do usuário e os dados retornados em resposta a essa chamada. Os dados recuperados devem ser apresentados em um formato legível por máquina para ser analisado e consumido pelo cliente.

A estrutura da API Web faz parte da pilha do ASP.NET e foi projetada para tornar mais fácil de implementar serviços HTTP, geralmente enviando e recebendo dados formatados em XML ou JSON por meio de uma API RESTful. Neste exercício, você criará o site para hospedar o aplicativo de teste nerd e implementar o serviço de back-end para expor e manter os dados de teste usando a API da Web do ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tarefa 1 – Criando o projeto inicial para especialista de teste

Nesta tarefa, você irá iniciar criando um novo projeto ASP.NET MVC com suporte para a API da Web ASP.NET com base no **One ASP.NET** projeto tipo que vem com o Visual Studio. **Um ASP.NET** unifica todas as tecnologias ASP.NET e oferece a opção para combinar e associá-las conforme desejado. Em seguida, você irá adicionar classes de modelo do Entity Framework e o initializator de banco de dados para inserir as perguntas de teste.

1. Abra **Visual Studio Express 2013 para Web** e selecione **arquivo | Novo projeto...**  para iniciar uma nova solução.

    ![Criar um novo projeto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "criar um novo projeto")

    *Criar um novo projeto*
2. No **novo projeto** caixa de diálogo, selecione **aplicativo Web ASP.NET** sob o **Visual c# | Web** guia. Certifique-se de **.NET Framework 4.5** é selecionada, nomeie- *GeekQuiz*, escolha um **local** e clique em **Okey**.

    ![Criar um novo projeto de aplicativo Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "criar um novo projeto de aplicativo Web do ASP.NET")

    *Criar um novo projeto de aplicativo Web do ASP.NET*
3. No **novo projeto ASP.NET** caixa de diálogo, selecione o **MVC** modelo e selecione o **API da Web** opção. Além disso, certifique-se de que o **autenticação** opção é definida como **contas de usuário individuais**. Clique em **OK** para continuar.

    ![Criar um novo projeto com o modelo MVC, incluindo componentes de API da Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Criar um novo projeto com o modelo MVC, incluindo componentes de API da Web*
4. No **Gerenciador de soluções**, com o botão direito do **modelos** pasta do **GeekQuiz** do projeto e selecione **adicionar | Item existente...** .

    ![Adicionar um item existente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "adicionar um item existente")

    *Adicionar um item existente*
5. No **Add Existing Item** caixa de diálogo, navegue até o **modelos/de ativos de origem** pasta e selecione todos os arquivos. Clique em **Adicionar**.

    ![Adicionando os ativos de modelo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "adicionando os ativos de modelo")

    *Adicionando os ativos de modelo*

    > [!NOTE]
    > Ao adicionar esses arquivos, você adiciona o modelo de dados, o contexto do banco de dados do Entity Framework e o inicializador de banco de dados para o aplicativo de teste nerd.
    > 
    > **Entity Framework (EF)** é um mapeador relacional de objeto (ORM) que permite que você crie aplicativos de acesso de dados por programação com um modelo de aplicativo conceitual em vez de programar diretamente usando um esquema de armazenamento relacional. Você pode aprender mais sobre o Entity Framework [aqui](../../../entity-framework.md).
    > 
    > A seguir está uma descrição das classes que você acabou de adicionar:
    > 
    > - **TriviaOption:** representa uma única opção associada a uma pergunta de teste
    > - **TriviaQuestion:** representa uma pergunta de teste e exibe as opções associadas por meio de **opções** propriedade
    > - **TriviaAnswer:** representa a opção selecionada pelo usuário em resposta a uma pergunta de teste
    > - **TriviaContext:** representa o contexto de banco de dados do Entity Framework do aplicativo nerd do teste. Essa classe é derivada de **DContext** e expõe **DbSet** propriedades que representam coleções de entidades descritas acima.
    > - **TriviaDatabaseInitializer:** a implementação do inicializador de Entity Framework para o **TriviaContext** classe que herda de **CreateDatabaseIfNotExists**. O comportamento padrão dessa classe é criar o banco de dados somente se ele não existir, inserir as entidades especificadas no **semente** método.
6. Abra o **Global.asax.cs** de arquivo e adicione o seguinte usando a instrução.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Adicione o seguinte código no início de **aplicativo\_iniciar** método para definir o **TriviaDatabaseInitializer** como o inicializador de banco de dados.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modificar o **início** controlador para restringir o acesso a usuários autenticados. Para fazer isso, abra o **HomeController** dentro do arquivo a **controladores** pasta e adicione o **autorizar** de atributo para o **HomeController**definição da classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > O **autorizar** filtrar verifica se o usuário é autenticado. Se o usuário não é autenticado, ele retorna o código de status HTTP 401 (não autorizado) sem chamar a ação. Você pode aplicar o filtro globalmente, no nível do controlador ou o nível de ações individuais.
9. Agora você deseja personalizar o layout das páginas da web e a identidade visual. Para fazer isso, abra o  **\_cshtml** dentro do arquivo de **exibições | Compartilhado** pasta e atualizar o conteúdo do **&lt;título&gt;** elemento substituindo *meu aplicativo ASP.NET* com *nerd do teste* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. No mesmo arquivo, atualizar a barra de navegação, removendo o *sobre* e *contato* links e renomear o *início* vincular a *reproduzir*. Além disso, renomeie o *nome do aplicativo* vincular a *nerd teste*. O HTML para a barra de navegação deve parecer com o código a seguir.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Atualizar o rodapé da página de layout, substituindo *meu aplicativo ASP.NET* com *nerd teste*. Para fazer isso, substitua o conteúdo do **&lt;rodapé&gt;** elemento com o seguinte código realçado.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tarefa 2 – criar a API da Web TriviaController

Na tarefa anterior, você criou a estrutura inicial do aplicativo da web nerd do teste. Agora, você criará um serviço de API da Web simples que interage com o modelo de dados de teste e expõe as seguintes ações:

- **GET/api/trívia**: recupera a próxima questão da lista de teste para ser respondida pelo usuário autenticado.
- **POST/api/trívia**: armazena a resposta de teste especificada pelo usuário autenticado.

Você usará as ferramentas ASP.NET Scaffolding fornecidas pelo Visual Studio para criar a linha de base para a classe do controlador API da Web.

1. Abra o **WebApiConfig.cs** dentro do arquivo de **aplicativo\_iniciar** pasta. Esse arquivo define a configuração do serviço de API da Web, como como rotas são mapeadas para ações do controlador de API da Web.
2. Adicione o seguinte usando a instrução no início do arquivo.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Adicione o seguinte código realçado para o **registrar** método para configurar globalmente o formatador para os dados JSON recuperados pelos métodos de ação de API da Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > O **CamelCasePropertyNamesContractResolver** converte automaticamente os nomes de propriedade *ter* caso, que é a convenção geral para nomes de propriedade em JavaScript.
4. No **Gerenciador de soluções**, com o botão direito do **controladores** pasta do **GeekQuiz** do projeto e selecione **adicionar | Novo Item de scaffolding...** .

    ![Criar um novo item de scaffolding](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "criar um novo item de scaffolding")

    *Criar um novo item de scaffolding*
5. No **adicionar Scaffold** caixa de diálogo caixa, certifique-se de que o **comuns** nó é selecionado no painel esquerdo. Em seguida, selecione o **controlador de 2 de API da Web - vazio** modelo no painel central e clique **adicionar**.

    ![Selecione o modelo vazio de controlador Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "selecionando o modelo vazio de controlador Web API 2")

    *Selecione o modelo vazio de controlador Web API 2*

    > [!NOTE]
    > **ASP.NET Scaffolding** é uma estrutura de geração de código para aplicativos Web ASP.NET. Visual Studio 2013 inclui geradores de código instalado previamente para projetos MVC e a API da Web. Você deve usar o scaffolding em seu projeto quando você deseja adicionar rapidamente o código que interage com modelos de dados para reduzir a quantidade de tempo necessário para desenvolver as operações de dados padrão.
    > 
    > O processo de scaffolding também garante que todas as dependências necessárias são instaladas no projeto. Por exemplo, se você iniciar com um projeto ASP.NET vazio e, em seguida, use o scaffolding para adicionar um controlador de API da Web, os pacotes do NuGet do API Web necessários e as referências são adicionadas ao seu projeto automaticamente.
6. No **Adicionar controlador** caixa de diálogo, digite *TriviaController* no **nome do controlador** caixa de texto e clique em **adicionar**.

    ![Adição do controlador Trívia](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "adição do controlador Trívia")

    *Adição do controlador Trívia*
7. O **TriviaController.cs** , em seguida, é adicionado ao **controladores** pasta do **GeekQuiz** projeto que contém um vazio **TriviaController** classe. Adicione o seguinte usando instruções no início do arquivo.

    (Código de trecho - *TriviaControllerUsings AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Adicione o seguinte código no início do **TriviaController** classe para definir, inicializar e descartar o **TriviaContext** instância no controlador.

    (Código de trecho - *TriviaControllerContext AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > O **Dispose** método **TriviaController** invoca o **Dispose** método do **TriviaContext** instância, o que garante que todos os os recursos usados pelo objeto de contexto são liberados quando a **TriviaContext** instância é descartada ou coletados como lixo. Isso inclui fechar todas as conexões de banco de dados abertas pelo Entity Framework.
9. Adicione o método auxiliar no final de **TriviaController** classe. Esse método recupera as seguintes perguntas de teste do banco de dados a serem respondidas pelo usuário especificado.

    (Código de trecho - *TriviaControllerNextQuestion AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Adicione o seguinte **obter** método de ação para o **TriviaController** classe. Este método de ação chama o **NextQuestionAsync** método auxiliar definido na etapa anterior para recuperar a próxima questão para o usuário autenticado.

    (Código de trecho - *TriviaControllerGetAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Adicione o método auxiliar no final de **TriviaController** classe. Esse método armazena a resposta especificada no banco de dados e retorna um valor booliano que indica se a resposta está correta ou não.

    (Código de trecho - *TriviaControllerStoreAsync AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Adicione o seguinte **Post** método de ação para o **TriviaController** classe. Este método de ação associa a resposta para o usuário autenticado e chama o **StoreAsync** método auxiliar. Em seguida, ele envia uma resposta com o valor booliano retornado pelo método auxiliar.

    (Código de trecho - *TriviaControllerPostAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modificar o controlador de API da Web para restringir o acesso aos usuários autenticados, adicionando o **autorizar** de atributo para o **TriviaController** definição da classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarefa 3 – executar a solução

Nesta tarefa, você verificará que o serviço de API da Web que você criou na tarefa anterior está funcionando conforme o esperado. Você usará o Internet Explorer **ferramentas de desenvolvedor F12** para capturar o tráfego de rede e inspecionar a resposta completa do serviço de API da Web.

> [!NOTE]
> Verifique se **Internet Explorer** está selecionado no **iniciar** botão localizado na barra de ferramentas do Visual Studio.
> 
> ![Opção do Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Pressione **F5** para executar a solução. O **login** página deve ser exibida no navegador.

    > [!NOTE]
    > Quando o aplicativo é iniciado, a rota MVC padrão é disparada, o que, por padrão é mapeado para o **índice** ação o **HomeController** classe. Como **HomeController** é restrito aos usuários autenticados (Lembre-se de que você decorado classe com o **autorizar** atributo no Exercício 1) e não há nenhum usuário autenticado ao mesmo tempo, o aplicativo Redireciona a solicitação original para a página de logon.

    ![Executar a solução](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "executando a solução")

    *Executar a solução*
2. Clique em **registrar** para criar um novo usuário.

    ![Registrar um novo usuário](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "registrar um novo usuário")

    *Registrar um novo usuário*
3. No **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.

    ![Página registrar](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "página de registro")

    *Página de registro*
4. O aplicativo registra a nova conta e o usuário é autenticado e redirecionado de volta para a home page.

    ![Usuário é autenticado](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "usuário autenticado")

    *Autenticação do usuário*
5. No navegador, pressione **F12** para abrir o **ferramentas de desenvolvedor** painel. Pressione **CTRL + 4** ou clique no **rede** ícone e, em seguida, clique botão de seta verde para começar a capturar o tráfego de rede.

    ![Iniciar a captura de rede de API da Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "captura de rede iniciando API da Web")

    *Iniciar a captura de rede de API da Web*
6. Acrescentar **api/trívia** para a URL na barra de endereços do navegador. Agora você irá inspecionar os detalhes da resposta do **obter** método de ação **TriviaController**.

    ![Recuperar os dados da próximos pergunta por meio da API da Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "recuperar os dados da próximos pergunta por meio da API da Web")

    *Recuperar os dados da próximos pergunta por meio da API da Web*

    > [!NOTE]
    > Quando o download termina, você será solicitado a fazer uma ação com o arquivo baixado. Deixe a caixa de diálogo aberta para que possa observar o conteúdo de resposta através da janela de ferramenta de desenvolvedores.
7. Agora você irá inspecionar o corpo da resposta. Para fazer isso, clique o **detalhes** guia e, em seguida, clique em **corpo da resposta**. Você pode verificar se os dados baixados são um objeto com as propriedades **opções** (que é uma lista de **TriviaOption** objetos), **id** e **título** que correspondam ao **TriviaQuestion** classe.

    ![Exibir o corpo da resposta de API da Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "exibir o corpo da resposta de API da Web")

    *Corpo de resposta de API da Web exibindo*
8. Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Exercício 2: Criar a Interface do SPA

Neste exercício primeiro você criará a parte de front-end da web de teste nerd, concentrando-se no aplicativo de página única interação usando **AngularJS**. Em seguida, você aumentará a experiência do usuário com CSS3 para executar animações avançadas e fornecer um efeito visual ao realizar a transição de uma pergunta para a próxima de alternância de contexto.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tarefa 1 – Criando a Interface do SPA usando AngularJS

Essa tarefa, você usará **AngularJS** para implementar o lado do cliente do aplicativo nerd do teste. **AngularJS** é uma estrutura de JavaScript do código-fonte aberto que aumenta a aplicativos baseados em navegador com *Model-View-Controller* recurso (MVC), facilitando a ambos os desenvolvimento e teste.

Você irá iniciar instalação AngularJS a partir Package Manager Console do Visual Studio. Em seguida, você criará o controlador para o comportamento do aplicativo de teste nerd e o modo de exibição para renderizar o teste e respostas usando o mecanismo de modelo AngularJS.

> [!NOTE]
> Para obter mais informações sobre AngularJS, consulte [ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).


1. Abra **Visual Studio Express 2013 para Web** e abra o **GeekQuiz.sln** solução localizada no **fonte/o Ex2-CreatingASPAInterface/início** pasta. Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. Abra o **Package Manager Console** de **ferramentas** | **Gerenciador de pacotes de biblioteca**. Digite o seguinte comando para instalar o **AngularJS.Core** pacote NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. Em **Gerenciador de soluções**, com o botão direito do **Scripts** pasta do **GeekQuiz** do projeto e selecione **adicionar | Nova pasta**. O nome da pasta **aplicativo** e pressione **Enter**.
4. Clique com botão direito do **aplicativo** pasta recém-criado e selecione **adicionar | Arquivo JavaScript**.

    ![Criar um novo arquivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Criar um novo arquivo JavaScript*
5. No **especificar nome para o Item** caixa de diálogo, digite *controlador de teste* no **nome do Item** caixa de texto e clique em **Okey**.

    ![Nomear o novo arquivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nomear o novo arquivo JavaScript*
6. No **teste controller.js** de arquivo, adicione o seguinte código para declarar e inicializar o AngularJS **QuizCtrl** controlador.

    (Código de trecho - *AngularQuizController AspNetWebApiSpa - o Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Função do construtor de **QuizCtrl** controlador espera um parâmetro injectable chamado **$scope**. O estado inicial do escopo deve ser configurado na função de construtor com a anexação de propriedades para o **$scope** objeto. As propriedades contêm o **modelo de exibição**e poderá ser acessado no modelo quando o controlador está registrado.
    > 
    > O **QuizCtrl** controlador é definido dentro de um módulo chamado **QuizApp**. Os módulos são unidades de trabalho que lhe permitem dividir seu aplicativo em componentes separados. As principais vantagens de usar módulos é que o código é mais fácil de entender e facilita o teste de unidade, reutilização e facilidade de manutenção.
7. Agora você irá adicionar o comportamento para o escopo para reagir a eventos disparados da exibição. Adicione o seguinte código no final o **QuizCtrl** controlador para definir o **nextQuestion** funcionar no **$scope** objeto.

    (Código de trecho - *AngularQuizControllerNextQuestion AspNetWebApiSpa - o Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Essa função recupera a próxima questão do **Trívia** API da Web criado no exercício anterior e anexa os dados de pergunta para a **$scope** objeto.
8. Insira o seguinte código no final do **QuizCtrl** controlador para definir o **sendAnswer** funcionar no **$scope** objeto.

    (Código de trecho - *AngularQuizControllerSendAnswer AspNetWebApiSpa - o Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Essa função envia a resposta selecionada pelo usuário para o **Trívia** API da Web e armazena o resultado – ou seja, se a resposta está correta ou não – o **$scope** objeto.
    > 
    > O **nextQuestion** e **sendAnswer** funções acima usam o AngularJS **$http** objeto abstrair a comunicação com a API da Web por meio de XMLHttpRequest Objeto de JavaScript do navegador. AngularJS oferece suporte a outro serviço que oferece um nível mais alto de abstração para executar operações CRUD em um recurso por meio de APIs RESTful. O AngularJS **$resource** objeto tem métodos de ação que fornecem comportamentos de alto nível sem a necessidade de interagir com o **$http** objeto. Considere o uso de **$resource** objeto em cenários que exija o modelo CRUD (fore informações, consulte o [$resource documentação](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. A próxima etapa é criar o modelo de AngularJS que define a exibição do teste. Para fazer isso, abra o **cshtml** dentro do arquivo de **exibições | Início** pasta e substitua o conteúdo com o código a seguir.

    (Código de trecho - *GeekQuizView AspNetWebApiSpa - o Ex2 -*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > O modelo de AngularJS é uma especificação declarativa que usa as informações do modelo e o controlador de transformar marcação estática em exibição dinâmica do que o usuário vê no navegador. A seguir é exemplos de AngularJS elementos e atributos do elemento que podem ser usados em um modelo:
    > 
    > - O **ng-app** diretiva informa AngularJS o elemento DOM que representa o elemento raiz do aplicativo.
    > - O **ng controlador** diretiva anexa um controlador para o DOM no ponto em que a diretiva é declarada.
    > - A notação de chave **{{}}** denota associações para as propriedades de escopo definidas no controlador.
    > - O **ng-click** diretiva é usada para chamar funções definidas no escopo em resposta aos cliques do usuário.
10. Abra o **Site.css** dentro do arquivo de **conteúdo** pasta e adicione os seguintes estilos realçados no final do arquivo para fornecer uma aparência para o modo de exibição de teste.

    (Código de trecho - *GeekQuizStyles AspNetWebApiSpa - o Ex2 -*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarefa 2 – em execução a solução

Nesta tarefa, você executará a solução usando o novo usuário interface é construído com AngularJS para responder a algumas das perguntas de teste.

1. Pressione **F5** para executar a solução.
2. Registre uma nova conta de usuário. Para fazer isso, siga as etapas de Registro descritas no Exercício 1, tarefa 3.

    > [!NOTE]
    > Se você estiver usando a solução de exercício anterior, você pode fazer logon com a conta de usuário que você criou antes.
3. O **início** página deve ser exibida, mostrando a primeira pergunta de teste. Responda a pergunta, clicando em uma das opções. Isso vai disparar a **sendAnswer** função definida anteriormente, que envia a opção selecionada para o **Trívia** API da Web.

    ![Responder a uma pergunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "responder a uma pergunta")

    *Responder a uma pergunta*
4. Depois de clicar em um dos botões, a resposta deve aparecer. Clique em **próxima pergunta** para mostrar a pergunta a seguir. Isso vai disparar a **nextQuestion** função definida no controlador.

    ![Solicitando a próxima pergunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "solicitando a próxima pergunta")

    *Solicitando a próxima pergunta*
5. A próxima pergunta deve aparecer. Continue respondendo perguntas quantas vezes desejar. Depois de concluir todas as perguntas, você deve retornar à primeira pergunta.

    ![Outra pergunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "outra pergunta")

    *Próxima pergunta*
6. Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tarefa 3 – Criando uma animação invertida usando CSS3

Nesta tarefa, você usará propriedades CSS3 para executar animações avançadas ao adicionar um efeito invertido quando uma pergunta foi respondida e quando a próxima pergunta é recuperada.

1. Em **Gerenciador de soluções**, com o botão direito do **conteúdo** pasta do **GeekQuiz** do projeto e selecione **adicionar | Item existente...** .

    ![Adicionar um item existente para a pasta de conteúdo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "adicionar um item existente para a pasta de conteúdo")

    *Adicionar um item existente para a pasta de conteúdo*
2. No **Add Existing Item** caixa de diálogo, navegue até o **fonte/ativos** pasta e selecione **Flip.css**. Clique em **Adicionar**.

    ![Adicionar o arquivo de Flip.css ativos](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "adicionar o arquivo de Flip.css de ativos")

    *Adicionando o arquivo Flip.css ativos*
3. Abra o **Flip.css** arquivo que você acabou de adicionar e inspecione o seu conteúdo.
4. Localize o **inverter transformação** comentário. Os estilos abaixo comentário usarem CSS **perspectiva** e **rotateY** transformações para gerar um &quot;cartão invertido&quot; efeito.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Localize o **ocultar parte posterior do painel durante o flip** comentário. O estilo abaixo comentário oculta lado de trás das faces quando elas são opostas o visualizador, definindo o **backface visibilidade** propriedade CSS para *oculto*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Abra o **BundleConfig.cs** dentro do arquivo a **aplicativo\_iniciar** pasta e adicionar a referência ao **Flip.css** do arquivo no **&quot;~/Content/css&quot;** pacote de estilo

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Pressione **F5** para executar a solução e faça logon com suas credenciais.
8. Responda a uma pergunta, clicando em uma das opções. Observe o flip efeito ao realizar a transição entre os modos de exibição.

    ![Responder a uma pergunta com o flip efeito](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "responder a uma pergunta com o efeito invertido")

    *Responder a uma pergunta com o efeito invertido*
9. Clique em **próxima pergunta** para recuperar a pergunta a seguir. O efeito invertido deve aparecer novamente.

    ![Recuperando a pergunta a seguir com o flip efeito](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "recuperar a seguinte pergunta com o efeito invertido")

    *Recuperando a pergunta a seguir com o efeito invertido*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático você aprendeu como:

- Criar um controlador API Web do ASP.NET usando a estrutura do ASP.NET
- Implementar uma ação de Get de API da Web para recuperar a próxima questão de teste
- Implementar uma ação de postagem de API da Web para armazenar as respostas de teste
- Instalar o AngularJS do Console do Gerenciador de pacote do Visual Studio
- Implementar AngularJS modelos e controladores
- Use as transições do CSS3 para executar os efeitos de animação
