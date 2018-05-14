---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratório prático: ASP.NET de um: integração de Web Forms do ASP.NET, MVC e API da Web | Microsoft Docs'
author: rick-anderson
description: O ASP.NET é uma estrutura para a criação de sites, aplicativos e serviços usando tecnologias especializadas, como MVC, API da Web e outros. Com a expansão h ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratório prático: ASP.NET de um: integração de Web Forms do ASP.NET, MVC e API da Web
====================
por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](http://aka.ms/webcamps-training-kit)

> O ASP.NET é uma estrutura para a criação de sites, aplicativos e serviços usando tecnologias especializadas, como MVC, API da Web e outros. Com a expansão do ASP.NET tem visto desde sua criação e a expressa precisa ter essas tecnologias integradas, houve esforços recentes no trabalho na direção **One ASP.NET**.
> 
> Visual Studio 2013 apresenta um novo sistema de projeto unificada que permite que você criar um aplicativo e usar todas as tecnologias do ASP.NET em um único projeto. Esse recurso elimina a necessidade de selecionar uma tecnologia no início de um projeto e o cartão com ele e, em vez disso, incentiva o uso de várias estruturas ASP.NET dentro de um projeto.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Visão Geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Criar um site com base no **One ASP.NET** tipo de projeto
- Use diferentes **ASP.NET** estruturas como **MVC** e **API da Web** no mesmo projeto
- Identificar os principais componentes de um **ASP.NET** aplicativo
- Aproveitar o **ASP.NET Scaffolding** framework para criar automaticamente os controladores e exibições para executar operações CRUD com base em suas classes de modelo
- Expõe o mesmo conjunto de informações em formatos máquina - e legível usando a ferramenta certa para cada trabalho

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O exemplo a seguir é necessário para concluir este laboratório prático:

- [O Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou maior
- [Visual Studio 2013 Atualização 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [Criar um novo projeto de formulários da Web](#Exercise1)
2. [Criando um controlador MVC usando o Scaffolding](#Exercise2)
3. [Criando um controlador de API da Web usando o Scaffolding](#Exercise3)

Tempo estimado para concluir este laboratório: **60 minutos**

> [!NOTE]
> Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida foi projetada para coincidir com um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma tarefa específica no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção. Se você escolher uma coleção de configurações diferentes para o seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Exercício 1: Criar um novo projeto de formulários da Web

Neste exercício, você criará um novo site de Web Forms no Visual Studio 2013 usando o **One ASP.NET** unified experiência de projeto, o que permitirá que você facilmente integrar os componentes Web Forms, MVC e API da Web no mesmo aplicativo. Você irá explorar a solução gerada e identificar suas partes, e por fim, você verá o site da Web em ação.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Tarefa 1 – criar um novo Site usando uma experiência de ASP.NET

Nesta tarefa, você iniciará criando um novo site da Web no Visual Studio com base no **One ASP.NET** tipo de projeto. **Um ASP.NET** unifica todas as tecnologias ASP.NET e oferece a opção para combinar e associá-las conforme desejado. Você, em seguida, reconhecerá os diferentes componentes de Web Forms, MVC e API da Web ao vivo lado a lado dentro de seu aplicativo.

1. Abra **Visual Studio Express 2013 para Web** e selecione **arquivo | Novo projeto...**  para iniciar uma nova solução.

    ![Criando um novo projeto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Criar um novo projeto*
2. No **novo projeto** caixa de diálogo, selecione **aplicativo Web ASP.NET** sob o **Visual c# | Web** guia e certifique-se de **.NET Framework 4.5** está selecionado. Nomeie o projeto *MyHybridSite*, escolha um **local** e clique em **Okey**.

    ![Novo projeto de aplicativo Web do ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Criar um novo projeto de aplicativo Web do ASP.NET*
3. No **novo projeto ASP.NET** caixa de diálogo, selecione o **Web Forms** modelo e selecione o **MVC** e **API da Web** opções. Além disso, certifique-se de que o **autenticação** opção é definida como **contas de usuário individuais**. Clique em **OK** para continuar.

    ![Criar um novo projeto com o modelo de Web Forms, incluindo componentes de API da Web e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Criar um novo projeto com o modelo de Web Forms, incluindo componentes de API da Web e MVC*
4. Agora você pode explorar a estrutura da solução gerada.

    ![Explorar a solução gerada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Explorar a solução gerada*

    1. **Conta:** esta pasta contém as páginas de formulário da Web para se registrar, faça logon no e gerenciar contas de usuário do aplicativo. Essa pasta é adicionada quando o **contas de usuário individuais** opção de autenticação é selecionada durante a configuração do modelo de projeto Web Forms.
    2. **Modelos:** essa pasta contém as classes que representam os dados do aplicativo.
    3. **Controladores** e **exibições**: essas pastas são necessárias para o **ASP.NET MVC** e **ASP.NET Web API** componentes. Você irá explorar as tecnologias MVC e a API da Web nos próximos exercícios.
    4. O **Default.aspx**, **Contact.aspx** e **About** arquivos são predefinidas páginas de formulário da Web que você pode usar como ponto de partida para criar as páginas específicas para seu aplicativo. A lógica de programação dos arquivos que reside em um arquivo separado, conhecido como o &quot;por trás do código&quot; arquivo, que tem um &quot;. aspx&quot; ou &quot;. aspx.cs&quot; extensão (dependendo do linguagem usada). A lógica por trás do código é executado no servidor e dinamicamente produz a saída HTML para a página.
    5. O **Site.Master** e **Site.Mobile.Master** páginas definem a aparência e o comportamento padrão de todas as páginas no aplicativo.
5. Clique duas vezes o **Default.aspx** arquivo para explorar o conteúdo da página.

    ![Explorando a página Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Explorando a página Default.aspx*

    > [!NOTE]
    > O **página** diretiva na parte superior do arquivo define os atributos de página de Web Forms. Por exemplo, o **MasterPageFile** atributo especifica o caminho para o mestre de página - nesse caso, o *Site.Master* página e o **Inherits** atributo define o classe code-behind para a página para herdar. Essa classe está localizada no arquivo determinado pelo **code-behind** atributo.
    > 
    > O **asp: Content** controle contém o conteúdo real da página (texto, marcação e controles) e é mapeado para um **asp: ContentPlaceHolder** controle na página mestra. Nesse caso, o conteúdo da página será renderizado dentro do *MainContent* controle definido no *Site.Master* página.
6. Expanda o **aplicativo\_iniciar** pasta e observe o **WebApiConfig.cs** arquivo. O Visual Studio incluídos nesse arquivo na solução gerada porque você incluiu API da Web ao configurar seu projeto com o modelo do ASP.NET de um.
7. Abra o **WebApiConfig.cs** arquivo. No *WebApiConfig* classe você encontrará a configuração associada a API da Web, que mapeia HTTP roteia para **controladores da API da Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Abra o **RouteConfig.cs** arquivo. Dentro de *RegisterRoutes* método você encontrará a configuração associada MVC, que mapeia as rotas HTTP para **controladores MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarefa 2 – em execução a solução

Nesta tarefa, você executar a solução gerada, explore o aplicativo e alguns de seus recursos, como a regravação de URL e a autenticação integrada.

1. Para executar a solução, pressione **F5** ou clique no **iniciar** botão localizado na barra de ferramentas. A home page do aplicativo deve abrir no navegador.

    ![Executar a solução](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Verifique se que as páginas de formulários da Web estão sendo invocadas. Para fazer isso, acrescente **/contact.aspx** para a URL na barra de endereços e pressione **Enter**.

    ![URLs amigáveis](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *URLs amigáveis*

    > [!NOTE]
    > Você pode ver, a URL será alterado para **ou contate**. A partir **ASP.NET 4**, recursos de roteamento de URL foram adicionados para Web Forms, portanto você pode escrever URLs como *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* em vez de  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Para obter mais informações, consulte [roteamento de URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Agora, você explorará o fluxo de autenticação integrado no aplicativo. Para fazer isso, clique em **registrar** no canto superior direito da página.

    ![Registrar um novo usuário](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrar um novo usuário*
4. No **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.

    ![Página de registro](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Página de registro*
5. O aplicativo registra a nova conta e o usuário é autenticado.

    ![Usuário autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Usuário autenticado*
6. Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Exercício 2: Criando um controlador MVC usando o Scaffolding

Neste exercício, você será tirar proveito da estrutura ASP.NET Scaffolding fornecido pelo Visual Studio para criar um controlador de ASP.NET MVC 5 com ações e modos de exibição do Razor para executar operações CRUD, sem escrever uma única linha de código. O processo de scaffolding usará Entity Framework Code First para gerar o contexto de dados e o esquema de banco de dados no banco de dados SQL.

**Sobre o Entity Framework Code primeiro**

Entity Framework (EF) é um mapeador relacional de objeto (ORM) que permite que você crie aplicativos de acesso de dados por programação com um modelo de aplicativo conceitual em vez de programar diretamente usando um esquema de armazenamento relacional.

O fluxo de trabalho de modelagem Entity Framework Code First permite que você use suas próprias classes de domínio para representar o modelo EF depende ao executar a consulta, funções de controle de alterações e atualização. Usando o fluxo de trabalho de desenvolvimento Code First, você precisa iniciar seu aplicativo criando um banco de dados ou especificando um esquema. Em vez disso, você pode escrever classes .NET padrão que definem os objetos de modelo de domínio mais apropriados para seu aplicativo e do Entity Framework criará o banco de dados para você.

> [!NOTE]
> Você pode aprender mais sobre o Entity Framework [aqui](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Tarefa 1 – criar um novo modelo

Agora você definirá um **pessoa** classe, que será o modelo usado pelo processo de scaffolding para criar o controlador MVC e os modos de exibição. Você começará criando uma **pessoa** classe de modelo e as operações CRUD no controlador serão criado automaticamente usando os recursos de scaffolding.

1. Abra **Visual Studio Express 2013 para Web** e **MyHybridSite.sln** solução localizada no **fonte/o Ex2-MvcScaffolding/início** pasta. Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. No **Gerenciador de soluções**, com o botão direito do **modelos** pasta do **MyHybridSite** do projeto e selecione **adicionar | Classe...** .

    ![Adicionando a classe de modelo de pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Adicionando a classe de modelo de pessoa*
3. No **Adicionar Novo Item** caixa de diálogo, nomeie o arquivo *Person.cs* e clique em **adicionar**.

    ![Criando a classe de modelo de pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Criando a classe de modelo de pessoa*
4. Substitua o conteúdo do **Person.cs** arquivo com o código a seguir. Pressione **CTRL + S** para salvar as alterações.

    (Código de trecho - *PersonClass BringingTogetherOneAspNet - o Ex2 -*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. Em **Solution Explorer**, com o botão direito do **MyHybridSite** do projeto e selecione **criar**, ou pressione **CTRL + SHIFT + B** para compilar o projeto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Tarefa 2 – criar um controlador MVC

Agora que o **pessoa** modelo é criado, você usará o scaffolding de ASP.NET MVC com o Entity Framework para criar as ações do controlador CRUD e modos de exibição para **pessoa**.

1. No **Gerenciador de soluções**, com o botão direito do **controladores** pasta do **MyHybridSite** do projeto e selecione **adicionar | Novo Item de scaffolding...** .

    ![Criando um novo controlador de scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Criando um novo controlador de Scaffold*
2. No **adicionar Scaffold** caixa de diálogo, selecione **controlador MVC 5 com modos de exibição usando o Entity Framework** e, em seguida, clique em **adicionar.**

    ![Selecionar controlador MVC 5 com modos de exibição e o Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Selecionar controlador MVC 5 com modos de exibição e o Entity Framework*
3. Definir *MvcPersonController* como o **nome do controlador**, selecione o **usar ações assíncronas do controlador** opção e selecione **pessoa (MyHybridSite.Models)**  como o **classe modelo**.

    ![Adicionando um controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Adicionando um controlador MVC com scaffolding*
4. Em **classe de contexto de dados**, clique em **novo contexto de dados...** .

    ![Criar um novo contexto de dados](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Criar um novo contexto de dados*
5. No **novo contexto de dados** caixa de diálogo, o novo contexto de dados de nome *PersonContext* e clique em **adicionar**.

    ![Criar o novo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Criar o novo tipo de PersonContext*
6. Clique em **adicionar** para criar o novo controlador de **pessoa** com scaffolding. O Visual Studio gera as ações do controlador, o contexto de dados de pessoa e os modos de exibição do Razor.

    ![Depois de criar o controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Depois de criar o controlador MVC com scaffolding*
7. Abra o **MvcPersonController.cs** de arquivos no **controladores** pasta. Observe que os métodos de ação CRUD foram gerados automaticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Selecionando o **usar ações assíncronas do controlador** opções de caixa de seleção o scaffolding nas etapas anteriores, o Visual Studio gera os métodos de ação assíncrono para todas as ações que envolvem acesso ao contexto de dados de pessoa. É recomendável que você use métodos de ação assíncrono de longa duração, CPU não associado a solicitações para evitar o bloqueio de servidor Web de execução de trabalho enquanto a solicitação está sendo processada.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarefa 3 – executar a solução

Nesta tarefa, você executará a solução novamente para verificar se os modos de exibição para **pessoa** estão funcionando como esperado. Você adicionará uma nova pessoa para verificar que ele é salvo com êxito no banco de dados.

1. Pressione **F5** para executar a solução.
2. Navegue até **/MvcPerson**. Exibição de scaffolding que mostra a lista de pessoas deve aparecer.
3. Clique em **criar novo** para adicionar uma nova pessoa.

    ![Navegando até as scaffolding exibições do MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navegando até as scaffolding exibições do MVC*
4. No **criar** exibir, forneça um **nome** e um **idade** para a pessoa e clique em **criar**.

    ![Adicionando uma nova pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Adicionando uma nova pessoa*
5. A nova pessoa é adicionada à lista. Na lista de elementos, clique em **detalhes** para exibir o modo de exibição de detalhes da pessoa. Em seguida, no **detalhes** exibir, clique em **voltar para a lista** para voltar à exibição de lista.

    ![Modo de exibição de detalhes da pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Modo de exibição de detalhes da pessoa*
6. Clique o **excluir** link para excluir a pessoa. No **excluir** exibir, clique em **excluir** para confirmar a operação.

    ![A exclusão de uma pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *A exclusão de uma pessoa*
7. Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Exercício 3: Criar um controlador de API da Web usando o Scaffolding

A estrutura da API Web faz parte da pilha do ASP.NET e projetado para facilitar a implementação de serviços de HTTP, geralmente enviando e recebendo dados formatados em XML ou JSON por meio de uma API RESTful.

Neste exercício, você usará ASP.NET Scaffolding novamente para gerar um controlador Web API. Você usará o mesmo **pessoa** e **PersonContext** classes do exercício anterior para fornecer os mesmos dados pessoa no formato JSON. Você verá como é possível expor os mesmos recursos de maneiras diferentes dentro do mesmo aplicativo ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Tarefa 1 – Criando um controlador de API da Web

Nesta tarefa, você criará um novo **controlador de API da Web** que irá expor os dados em um formato consumível máquina como JSON.

1. Se ainda não estiver aberto, abra **Visual Studio Express 2013 para Web** e abra o **MyHybridSite.sln** solução localizada no **fonte/Ex3-WebAPI/início** pasta. Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.

    > [!NOTE]
    > Se você iniciar com a solução de início de Exercício 3, pressione **CTRL + SHIFT + B** para criar a solução.
2. No **Gerenciador de soluções**, com o botão direito do **controladores** pasta do **MyHybridSite** do projeto e selecione **adicionar | Novo Item de scaffolding...** .

    ![Criando um novo controlador de scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Criando um novo controlador de scaffolding*
3. No **adicionar Scaffold** caixa de diálogo, selecione **API da Web** no painel esquerdo, em seguida, **Web 2 controlador API com ações, usando o Entity Framework** no painel central e, em seguida, clique em  **Adicione.**

    ![A seleção de controlador do Web API 2 com ações e o Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "selecionar controlador Web API 2 com ações e o Entity Framework")

    *A seleção de controlador do Web API 2 com ações e o Entity Framework*
4. Definir *ApiPersonController* como o **nome do controlador**, selecione o **usar ações assíncronas do controlador** opção e selecione **pessoa (MyHybridSite.Models)**  e **PersonContext (MyHybridSite.Models)** como o **modelo** e **contexto de dados** classes respectivamente. Em seguida, clique em **adicionar**.

    ![Adicionar um controlador de API da Web com estrutura](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "adicionando um controlador Web API com scaffolding")

    *Adicionando um controlador Web API com scaffolding*
5. O Visual Studio gera o **ApiPersonController** classe com as quatro ações CRUD para trabalhar com seus dados.

    ![Depois de criar o controlador de API da Web com estrutura](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "depois de criar o controlador de API da Web com estrutura")

    *Depois de criar o controlador de API da Web com estrutura*
6. Abra o **ApiPersonController.cs** de arquivo e inspecione o *GetPeople* método de ação. Este método consulta o campo de banco de dados de **PersonContext** tipo para as pessoas a obtenção de dados.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Agora, observe o comentário acima da definição de método. Ele fornece o URI que expõe esta ação que será usada na próxima tarefa.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Por padrão, a API da Web está configurada para capturar as consultas para o */api* caminho para evitar colisões com controladores MVC. Se você precisar alterar essa configuração, consulte [roteamento na API da Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarefa 2 – em execução a solução

Nesta tarefa, você usará o Internet Explorer **ferramentas de desenvolvedor F12** para inspecionar a resposta completa do controlador de API da Web. Você verá como você pode capturar o tráfego de rede para obter mais informações sobre os dados do aplicativo.

> [!NOTE]
> Verifique se **Internet Explorer** está selecionado no **iniciar** botão localizado na barra de ferramentas do Visual Studio.
> 
> ![Opção do Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> O **ferramentas de desenvolvedor F12** tem um amplo conjunto de funcionalidades que não é abordada neste laboratório prático. Se você quiser saber mais sobre ele, consulte [usando as ferramentas de desenvolvedor F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Pressione **F5** para executar a solução.

    > [!NOTE]
    > Para executar essa tarefa corretamente, seu aplicativo precisa ter dados. Se seu banco de dados estiver vazio, você pode voltar para a tarefa 3 no Exercício 2 e siga as etapas sobre como criar uma nova pessoa usando as exibições do MVC.
2. No navegador, pressione **F12** para abrir o **ferramentas de desenvolvedor** painel. Pressione **CTRL** + **4** ou clique no **rede** ícone e, em seguida, clique botão de seta verde para começar a capturar o tráfego de rede.

    ![Iniciar a captura de rede de API da Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "captura de rede iniciando API da Web")

    *Iniciar a captura de rede de API da Web*
3. Acrescentar **api/ApiPerson** para a URL na barra de endereços do navegador. Agora você irá inspecionar os detalhes da resposta do **ApiPersonController**.

    ![Recuperando dados da pessoa por meio da API da Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "recuperar dados da pessoa por meio da API da Web")

    *Recuperando dados da pessoa por meio da API da Web*

    > [!NOTE]
    > Quando o download termina, você será solicitado a fazer uma ação com o arquivo baixado. Deixe a caixa de diálogo aberta para que possa observar o conteúdo de resposta através da janela de ferramenta de desenvolvedores.
4. Agora você irá inspecionar o corpo da resposta. Para fazer isso, clique o **detalhes** guia e, em seguida, clique em **corpo da resposta**. Você pode verificar os dados baixados são uma lista de objetos com as propriedades **Id**, **nome** e **idade** que correspondam ao **pessoa** classe.

    ![Exibindo Web o corpo da resposta de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "exibição Web o corpo da resposta de API")

    *Corpo de resposta de API da Web exibindo*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Tarefa 3 – adicionar páginas de Ajuda da API da Web

Quando você cria uma API da Web, é útil criar uma página de ajuda para que outros desenvolvedores saibam como chamar sua API. Você pode criar e atualizar páginas de documentação manualmente, mas é melhor gerar automaticamente para evitar a necessidade de fazer o trabalho de manutenção. Nesta tarefa, você usará um pacote do Nuget para gerar automaticamente as páginas de Ajuda da API da Web para a solução.

1. Do **ferramentas** menu do Visual Studio, selecione **Gerenciador de biblioteca de pacote**e, em seguida, clique em **Package Manager Console**.
2. No **Package Manager Console** janela, execute o seguinte comando:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > O **Microsoft.AspNet.WebApi.HelpPage** pacote instala os assemblies necessários e adiciona as exibições do MVC para as páginas de Ajuda no **áreas/HelpPage** pasta.

    ![Área de HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage área")

    *Área de HelpPage*
3. Por padrão, a Ajuda páginas têm cadeias de caracteres de espaço reservado para documentação. Você pode usar comentários de documentação XML para criar a documentação. Para habilitar esse recurso, abra o **HelpPageConfig.cs** arquivo localizado no **HelpPage/áreas/aplicativo\_iniciar** pasta e remova os comentários da linha a seguir:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. Em **Solution Explorer**, clique com o botão direito **MyHybridSite**, selecione **propriedades** e clique no **criar** guia.

    ![Guia Build](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "criar seção")

    *Guia Build*
5. Em **saída**, selecione **arquivo de documentação XML**. Na caixa de edição, digite **aplicativo\_Data/XmlDocument.xml**.

    ![Seção do guia de criação de saída](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "seção do guia de criação de saída")

    *Seção de saída na guia de compilação*
6. Pressione **CTRL** + **S** para salvar as alterações.
7. Abra o **ApiPersonController.cs** arquivo o **controladores** pasta.
8. Insira uma nova linha entre o *GetPeople* assinatura do método e o */ / obter api/ApiPerson* comentar e, em seguida, digite três barras.

    > [!NOTE]
    > O Visual Studio automaticamente insere os elementos XML que definem a documentação do método.
9. Adicionar um texto de resumo e o valor de retorno para o *GetPeople* método. Ele deverá ter a seguinte aparência.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Pressione **F5** para executar a solução.
11. Acrescentar **/Help** para a URL na barra de endereços para navegar até a página de Ajuda.

    ![Página de Ajuda da API da Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "página de Ajuda da API da Web do ASP.NET")

    *Página de Ajuda da API da Web ASP.NET*

    > [!NOTE]
    > O conteúdo principal da página é uma tabela de APIs, agrupados pelo controlador. As entradas da tabela são geradas dinamicamente, usando o **IApiExplorer** interface. Se você adicionar ou atualizar um controlador de API, a tabela será automaticamente atualizada na próxima vez que você criar o aplicativo.
    > 
    > O **API** coluna lista o URI relativo e método HTTP. O **descrição** coluna contém informações que foi extraídas na documentação do método.
12. Observe que a descrição que você adicionou acima da definição de método é exibida na coluna Descrição.

    ![Descrição do método API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descrição do método de API")

    *Descrição do método de API*
13. Clique em um dos métodos da API para navegar até uma página com informações mais detalhadas, incluindo os corpos de resposta de exemplo.

    ![Página de informações de detalhe](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "página de informações de detalhes")

    *Página de informações detalhadas*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático você aprendeu como:

- Criar um novo aplicativo Web usando uma experiência de ASP.NET no Visual Studio 2013
- Integrar várias tecnologias do ASP.NET em um único projeto
- Gerar controladores MVC e modos de exibição de suas classes de modelo usando o Scaffolding de ASP.NET
- Gerar controladores da API da Web, que usam recursos como acesso a dados por meio do Entity Framework e programação assíncrona
- Gerar automaticamente as páginas de ajuda de API da Web para os controladores
