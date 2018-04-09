---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Conceitos básicos do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratório prático baseia-se no repositório de música MVC (Model View Controller), um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: a0dd32280321938aba84a2aed5273d80750ed774
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a>Conceitos básicos do ASP.NET MVC 4

Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](https://aka.ms/webcamps-training-kit)

Este laboratório prático baseia-se no repositório de música MVC (Model View Controller), um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio. Em todo o laboratório, você aprenderá a simplicidade, ainda potência de usar essas tecnologias em conjunto. Você começará com um aplicativo simples e criará a ele até que você tenha um aplicativo de Web do ASP.NET MVC 4 totalmente funcional.

Este laboratório funciona com o ASP.NET MVC 4.

Se você quiser explorar a versão do ASP.NET MVC 3 do tutorial aplicativo, você pode encontrar na [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).

Este laboratório prático pressupõe que o desenvolvedor tenha experiência em tecnologias de desenvolvimento na Web, como HTML e JavaScript.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [conceitos básicos do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>O aplicativo de repositório de música

O aplicativo web de repositório de música que será criado durante este laboratório consiste em três partes principais: compras, check-out e administração. Os visitantes poderão procurar álbuns por gênero, adicionar álbuns a seu carrinho, revise sua seleção e, por fim, vá para check-out ao fazer logon e concluir o pedido. Além disso, os administradores de armazenamento será capazes de gerenciar os álbuns disponíveis, bem como suas propriedades principais.

![Telas de repositório de música](aspnet-mvc-4-fundamentals/_static/image1.png "telas do repositório de música")

*Telas de repositório de música*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Aplicativo de repositório de música será criado usando **controlador MVC (Model View)**, um padrão de arquitetura que separa um aplicativo em três componentes principais:

- **Modelos de**: objetos de modelo são as partes do aplicativo que implementa a lógica do domínio. Geralmente, os objetos de modelo também recuperam e armazenam o estado de modelo em um banco de dados.
- **Modos de exibição:** exibições são os componentes que exibem a interface do usuário do aplicativo (IU). Normalmente, essa interface do usuário é criado a partir de dados de modelo. Um exemplo seria a exibição de edição de álbuns que exibe as caixas de texto e uma lista suspensa com base no estado atual de um objeto de álbum.
- **Controladores:** controladores são os componentes que lidar com a interação do usuário, manipulam o modelo e, por fim, selecionam um modo de exibição para renderizar a interface do usuário. Em um aplicativo MVC, a exibição mostra apenas informações; o controlador manipula e responde à entrada e à interação do usuário.

O padrão MVC ajuda você a criar aplicativos que separam os diferentes aspectos do aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), enquanto fornece um acoplamento fraco entre esses elementos. Essa separação ajuda a gerenciar a complexidade quando você cria um aplicativo, pois ele permite que você se concentre em um aspecto da implementação por vez. Além disso, o padrão MVC torna fácil de testar aplicativos, incentivando também o uso de desenvolvimento controlado por testes (TDD) para a criação de aplicativos.

O **ASP.NET MVC** framework fornece uma alternativa ao padrão Web Forms do ASP.NET para criar aplicativos Web baseados em ASP.NET MVC. O **ASP.NET MVC** framework é uma estrutura de apresentação leve e altamente testável que (como aplicativos baseados em formulários da Web) está integrado aos recursos ASP.NET existentes, como páginas mestras e baseada em associação autenticação para que você obtenha todos os recursos do .NET framework core. Isso é útil se você já estiver familiarizado com o ASP.NET Web Forms porque todas as bibliotecas que você já usa também estão disponíveis no ASP.NET MVC 4.

Além disso, o acoplamento flexível entre os três componentes principais de um aplicativo MVC também promove o desenvolvimento paralelo. Por exemplo, um desenvolvedor pode trabalhar no modo de exibição, um segundo desenvolvedor pode trabalhar na lógica do controlador e um terceiro desenvolvedor pode se concentrar na lógica de negócios no modelo.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Criar um aplicativo ASP.NET MVC do zero com base no tutorial do aplicativo de repositório de música
- Adicionar controladores para lidar com URLs para a Home page do site e para navegação sua funcionalidade principal
- Adicionar um modo de exibição para personalizar o conteúdo exibido junto com seu estilo
- Adicionar classes de modelo para conter e gerenciar dados e domínio lógica
- Usar padrão de modelo de exibição para passar informações de ações do controlador para exibir modelos
- Explore o novo modelo de ASP.NET MVC 4 para aplicativos da internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [O Visual Studio 2012 Express para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configuração

**Instalando os trechos de código**

Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático é composto pelos exercícios a seguir:

1. [Exercício 1: Criando o projeto de aplicativo Web do MusicStore ASP.NET MVC](#Exercise1)
2. [Exercício 2: Criando um controlador](#Exercise2)
3. [Exercício 3: Passar parâmetros para um controlador](#Exercise3)
4. [Exercício 4: Criar um modo de exibição](#Exercise4)
5. [Exercício 5: Criar um modelo de exibição](#Exercise5)
6. [Exercício 6: Usando parâmetros no modo de exibição](#Exercise6)
7. [Exercício 7: Visão geral do novo modelo do ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Exercício 1: Criando o projeto de aplicativo Web do MusicStore ASP.NET MVC

Neste exercício, você aprenderá como criar um aplicativo ASP.NET MVC no Visual Studio 2012 Express para Web, bem como sua organização pasta principal. Além disso, você aprenderá a adicionar um novo controlador e torná-lo a exibir uma cadeia de caracteres simple na página inicial do aplicativo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tarefa 1: Criando o projeto de aplicativo Web ASP.NET MVC

1. Nesta tarefa, você criará um projeto de aplicativo do ASP.NET MVC vazio usando o modelo MVC Visual Studio. Iniciar **VS Express para Web**.
2. No menu **Arquivo**, clique em **Novo Projeto**.
3. No **novo projeto** select da caixa de diálogo de **aplicativo Web do ASP.NET MVC 4** tipo, localizado no projeto **Visual c#,** **Web** modelo lista.
4. Alterar o **nome** para *MvcMusicStore*.
5. Defina o local da solução dentro de um novo **começar** na pasta de origem deste exercício, por exemplo **\Source\Ex01-CreatingMusicStoreProject\Begin [seu-HOL-PATH]**. Clique em **OK**.

    ![Criar caixa de diálogo Novo projeto](aspnet-mvc-4-fundamentals/_static/image2.png "criar caixa de diálogo Novo projeto")

    *Criar caixa de diálogo Novo projeto*
6. No **novo projeto ASP.NET MVC 4** select da caixa de diálogo de **básica** modelo e certifique-se de que o **mecanismo de exibição** selecionado é **Razor**. Clique em **OK**.

    ![Nova caixa de diálogo do projeto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nova caixa de diálogo do projeto ASP.NET MVC 4")

    *Nova caixa de diálogo do projeto ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tarefa 2: explorar a estrutura da solução

A estrutura ASP.NET MVC inclui um modelo de projeto do Visual Studio que ajuda você a criar aplicativos da Web que suporte o padrão MVC. Este modelo cria um novo aplicativo Web do ASP.NET MVC com as pastas necessárias, modelos de item e as entradas do arquivo de configuração.

Nesta tarefa, você examinará a estrutura da solução para compreender os elementos que estão envolvidos e suas relações. As pastas a seguir estão incluídas no aplicativo do ASP.NET MVC porque a estrutura ASP.NET MVC por padrão usa uma &quot;convenção sobre a configuração&quot; abordagem e faz algumas suposições padrão com base em nomes de pasta convenções.

1. Quando o projeto é criado, examine a estrutura de pasta que foi criada no Gerenciador de soluções, no lado direito:

    ![Estrutura de pasta do ASP.NET MVC no Gerenciador de soluções](aspnet-mvc-4-fundamentals/_static/image4.png "estrutura ASP.NET MVC pasta no Gerenciador de soluções")

    *Estrutura de pasta do ASP.NET MVC no Gerenciador de soluções*

   1. **Controladores**. Essa pasta contém as classes do controlador. Em um aplicativo MVC baseado, controladores são responsáveis por tratar a interação do usuário final, manipulando o modelo e, por fim, escolhendo um modo de exibição para renderizar a interface do usuário.

       > [!NOTE]
       > A estrutura do MVC requer que os nomes de todos os controladores para terminar com &quot;controlador&quot;-por exemplo, HomeController, LoginController ou ProductController.
   2. **Modelos de**. Essa pasta é fornecida para classes que representam o modelo de aplicativo para o aplicativo Web MVC. Isso geralmente inclui o código que define a lógica para interagir com o repositório de dados e objetos. Normalmente, os objetos de modelo real será em bibliotecas de classe separada. No entanto, quando você cria um novo aplicativo, você pode incluir classes e movê-los para as bibliotecas de classe separada posteriormente no ciclo de desenvolvimento.
   3. **Modos de exibição**. Essa pasta é o local recomendado para modos de exibição, os componentes responsáveis por exibir a interface do usuário do aplicativo. Modos de exibição usam arquivos. aspx,. ascx,. cshtml e. master, além de outros arquivos que estão relacionados aos modos de exibição de renderização. Pasta de modos de exibição contém uma pasta para cada controlador; a pasta é nomeada com o prefixo do nome do controlador. Por exemplo, se você tiver um controlador nomeado **HomeController**, modos de exibição contém uma pasta chamada início. Por padrão, quando a estrutura ASP.NET MVC carrega uma exibição, ele procura um arquivo. aspx com o nome de exibição solicitada na pasta Views\controllerName (**exibições [ControllerName] [ação]. aspx**) ou (**exibições [ControllerName] [Ação] cshtml**) para modos de exibição do Razor.

      > [!NOTE]
      > Além das pastas listadas anteriormente, um aplicativo MVC usa o **global. asax** padrões de arquivo para definir o roteamento de URL global e usa o **Web. config** arquivo para configurar o aplicativo.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tarefa 3: adicionando um HomeController

Em aplicativos ASP.NET que não usam a estrutura do MVC, a interação do usuário é organizada em torno de páginas e gerando e manipulando eventos a partir dessas páginas. Por outro lado, a interação do usuário com aplicativos ASP.NET MVC é organizada em torno de controladores e seus métodos de ação.

Por outro lado, estrutura ASP.NET MVC mapeia URLs para classes que são chamados de controladores. Controladores de processar solicitações de entrada, lidar com a entrada do usuário e interações, executar a lógica do aplicativo apropriada e determinar a resposta para enviar de volta para o cliente (exibição de HTML, baixar um arquivo, redirecionar para uma URL diferente, etc.). No caso de exibição de HTML, uma classe de controlador normalmente chama um componente de visualização separada para gerar a marcação HTML para a solicitação. Em um aplicativo MVC, a exibição mostra apenas informações; o controlador manipula e responde à entrada e à interação do usuário.

Nesta tarefa, você adicionará uma classe do controlador que tratará as URLs para a Home page do site do repositório de música.

1. Clique com botão direito **controladores** pasta no Gerenciador de soluções, selecione **adicionar** e **controlador** comando:

    ![Adicionar um comando de controlador](aspnet-mvc-4-fundamentals/_static/image5.png "adicionar um comando de controlador")

    *Adicionar um comando de controlador*
2. O **Adicionar controlador** caixa de diálogo é exibida. Nome do controlador *HomeController* e pressione **adicionar**.

    ![Controlador de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image6.png "Adicionar caixa de diálogo do controlador")

    *Adicionar caixa de diálogo do controlador*
3. O arquivo **HomeController** é criado no **controladores** pasta. Para ter o **HomeController** retornar uma cadeia de caracteres em sua ação de índice, substitua o **índice** método com o código a seguir:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex1 HomeController índice*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]
~~~

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - executando o aplicativo

Nesta tarefa, você experimentar o aplicativo em um navegador da web.

1. Pressione **F5** para executar o aplicativo. O projeto é compilado e inicia de servidor Web do IIS local. Local IIS Web Server automaticamente abrirá um navegador da web apontando para a URL do servidor Web.

    ![Aplicativo em execução em um navegador da web](aspnet-mvc-4-fundamentals/_static/image7.png "aplicativo executado em um navegador da web")

    *Aplicativo em execução em um navegador da web*

    > [!NOTE]
    > Servidor Web do IIS local executará o site em um número de porta livre aleatória. Na figura acima, o site está em execução no `http://localhost:50103/`, de modo que ele está usando a porta 50103. O número de porta pode variar.
2. Feche o navegador.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Exercício 2: Criando um controlador

Neste exercício, você aprenderá como atualizar o controlador para implementar a funcionalidade do aplicativo de repositório de música simple. Esse controlador definirá os métodos de ação para lidar com cada uma das seguintes solicitações específicas:

- Uma página de listagem de gêneros a música no repositório de música
- Uma página de pesquisa que lista todos os álbuns de música para um determinado gênero
- Uma página de detalhes que mostra informações sobre um álbum de música específico

Para o escopo deste exercício, essas ações simplesmente retornará uma cadeia de caracteres agora.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tarefa 1: adicionar uma nova classe StoreController

Nesta tarefa, você adicionará um novo controlador.

1. Se não estiver aberto, inicie **VS Express para Web 2012**.
2. No **arquivo** menu, escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, vá para **Source\Ex02 CreatingAController\Begin**, selecione **Begin.sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
3. Adicione o novo controlador. Para fazer isso, clique com botão direito do **controladores** pasta no Gerenciador de soluções, selecione **adicionar** e, em seguida, o **controlador** comando. Alterar o **nome do controlador** para *StoreController*e clique em **adicionar**.

    ![Controlador de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image8.png "Adicionar caixa de diálogo do controlador")

    *Adicionar caixa de diálogo do controlador*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tarefa 2: modificar ações do StoreController

Nesta tarefa, você modificará os métodos do controlador que são chamados **ações**. Ações são responsáveis por gerenciar solicitações de URL e determinar o conteúdo que deve ser enviado de volta para o navegador ou o usuário que invocou a URL.

1. O **StoreController** classe já tem um **índice** método. Você usará-lo posteriormente neste laboratório para implementar a página que lista todos os gêneros do repositório de música. Por enquanto, basta substituir a **índice** método com o código a seguir que retorna uma cadeia de caracteres &quot;Olá do Store.Index()&quot;:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - o Ex2 StoreController índice*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
~~~
2. Adicionar **procurar** e **detalhes** métodos. Para fazer isso, adicione o seguinte código para o **StoreController**:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - o Ex2 StoreController BrowseAndDetails*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executando o aplicativo

Nesta tarefa, você experimentar o aplicativo em um navegador da web.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado **início** página. Altere a URL para verificar a implementação de cada ação.

    1. **/Store**. Você verá  **&quot;Olá do Store.Index()&quot;**.
    2. **/Store/Browse**. Você verá  **&quot;Olá do Store.Browse()&quot;**.
    3. **/Store/Details**. Você verá  **&quot;Olá do Store.Details()&quot;**.

        ![Navegação StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de navegação")

        *Navegação /Store/Browse*
3. Feche o navegador.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Exercício 3: Passar parâmetros para um controlador

Até agora, você tem sido retornando cadeias de caracteres constantes dos controladores de. Neste exercício, você aprenderá como passar parâmetros para um controlador usando a URL e querystring e, em seguida, fazer com que as ações de método responder com texto para o navegador.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tarefa 1: Adicionar parâmetro gênero StoreController

Nesta tarefa, você usará o **querystring** para enviar parâmetros para o **procurar** método de ação de **StoreController**.

1. Se não estiver aberto, inicie **VS Express para Web**.
2. No **arquivo** menu, escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, vá para **Source\Ex03 PassingParametersToAController\Begin**, selecione **Begin.sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
3. Abra **StoreController** classe. Para fazer isso, no **Solution Explorer**, expanda o **controladores** pasta e clique duas vezes em **StoreController.cs**.
4. Alterar o **procurar** método, adicionando um parâmetro de cadeia de caracteres para solicitar um gênero específico. ASP.NET MVC será automaticamente passar qualquer querystring ou parâmetros nomeados de postagem de formulário **gênero** para este método de ação quando invocado. Para fazer isso, substitua o **procurar** método com o código a seguir:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2: executando o aplicativo

Nesta tarefa, você testar o aplicativo em um navegador da web e usará o **gênero** parâmetro.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado **início** página. Altere a URL para   */armazenar/procurar? Gênero = Disco* para verificar se a ação recebe o parâmetro de gênero.

    ![Navegação StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "navegação StoreBrowseGenre = Disco")

    *Navegação /Store/Browse? Gênero = Disco*
3. Feche o navegador.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tarefa 3: adicionando um parâmetro de Id inserido na URL

Nesta tarefa, você usará o **URL** para passar um **Id** parâmetro para o **detalhes** método de ação a **StoreController**. Padrão de ASP.NET MVC convenção de roteamento é tratar o segmento de URL após o nome do método de ação como um parâmetro denominado **Id**. Se seu método de ação tem um parâmetro chamado Id, em seguida, ASP.NET MVC automaticamente passará o segmento de URL para você como um parâmetro. Na URL **repositório/detalhes/5**, **Id** será interpretado como **5**.

1. Alterar o **detalhes** método o **StoreController**, adicionando um **int** parâmetro chamado **id**. Para fazer isso, substitua o **detalhes** método com o código a seguir:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - executando o aplicativo

Nesta tarefa, você testar o aplicativo em um navegador da web e usará o **Id** parâmetro.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado **início** página. Altere a URL para */Store/Details/5* para verificar se a ação recebe o parâmetro de id.

    ![Navegação StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de navegação")

    *Navegação /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Exercício 4: Criar um modo de exibição

Até você ter foi retornando cadeias de caracteres de ações do controlador. Embora o que é uma maneira útil entender como funcionam os controladores, não é como seus aplicativos Web reais são criados. Modos de exibição são os componentes que fornecem uma abordagem melhor para gerar HTML para o navegador com o uso de arquivos de modelo.

Neste exercício, você aprenderá como adicionar uma página de layout mestre para configurar um modelo de conteúdo HTML comum, uma folha de estilos para aprimorar a aparência do site e, finalmente, um modelo de exibição para habilitar o HomeController retornar o HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Tarefa 1: modificando o arquivo \_cshtml

O arquivo **~/Views/Shared/\_cshtml** permite que você configurar um modelo para HTML comuns para uso em todo o site. Nesta tarefa, você adicionará uma página de layout mestre com um cabeçalho comuns com links para a área página inicial e o repositório.

1. Se não estiver aberto, inicie **VS Express para Web**.
2. No **arquivo** menu, escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, vá para **Source\Ex04 CreatingAView\Begin**, selecione **Begin.sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
3. O arquivo  <strong>\_cshtml</strong> contém o layout de contêiner HTML para todas as páginas no site. Ele inclui o <strong>&lt;html&gt;</strong> elemento para a resposta HTML, bem como o <strong>&lt;head&gt;</strong> e <strong>&lt;corpo&gt;</strong> elementos. <strong>@RenderBody()</strong> em HTML corpo identificar regiões exibição modelos poderão ser preenchidos com conteúdo dinâmico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Adicione um cabeçalho comuns com links para a área página inicial e o armazenamento em todas as páginas no site. Para fazer isso, adicione o seguinte código abaixo &lt;corpo&gt; instrução.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Inclua um div para renderizar a seção de corpo de cada página. Substituir  <strong>@RenderBody()</strong> com o código a seguir higlighted: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Você sabia? O Visual Studio 2012 tem trechos de código que tornam mais fácil adicionar código de uso geral no HTML, arquivos de código e muito mais! Experimentar digitando **&lt;div&gt;** e pressionando **guia** duas vezes para inserir uma completa **div** marca.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tarefa 2 - adicionar folha de estilos CSS

O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas os estilos usados para exibir mensagens de validação e formas básicas. Você usará o CSS e imagens (potencialmente fornecidas por um designer) adicionais para melhorar a aparência do site.

Nesta tarefa, você adicionará uma folha de estilos CSS para definir os estilos do site.

1. O arquivo CSS e imagens são incluídas na **Source\Assets\Content** pasta deste laboratório. Para adicioná-las para o aplicativo, arraste o conteúdo de um **Windows Explorer** janela para o **Solution Explorer** no Visual Studio Express para Web, conforme mostrado abaixo:

    ![Arrastando o conteúdo de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "arrastando o conteúdo de estilo")

    *Arrastando o conteúdo de estilo*
2. Uma caixa de diálogo de aviso será exibida, solicitando a confirmação substituir **Site.css** de arquivo e algumas imagens existentes. Verificar **aplicar a todos os itens** e clique em **Sim**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tarefa 3: adicionando um modelo de exibição

Nesta tarefa, você adicionará um modelo de exibição para gerar a resposta HTML que usará a página mestra layout e CSS adicionados neste exercício.

1. Para usar um modelo de exibição durante a navegação da home page do site, primeiro será necessário indicar que em vez de retornar uma cadeia de caracteres, o **índice HomeController** método retornará um **exibição**. Abra **HomeController** classe e altere seu **índice** método para retornar um **ActionResult**, e ainda retornar **View()**.

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex4 HomeController índice*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
~~~
2. Agora, você precisa adicionar um modelo de exibição apropriado. Para fazer isso, **clique** dentro de **índice** método de ação e selecione **adicionar exibição**. Isso abrirá o **adicionar exibição** caixa de diálogo.

    ![Adicionando uma exibição de dentro do método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "adicionando uma exibição de dentro do método de índice")

    *Adicionando uma exibição de dentro do método de índice*
3. O **adicionar exibição** caixa de diálogo aparecerá gerar um arquivo de modelo de exibição. Por padrão, esta caixa de diálogo popula previamente o nome do modelo de exibição para que ele corresponda o método de ação que irá usá-la. Porque você usou o **adicionar modo de exibição** menu de contexto dentro de **índice** método de ação no HomeController, o **adicionar modo de exibição** caixa de diálogo possui um índice como o nome de exibição padrão. Clique em **Adicionar**.

    ![Exibição de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image14.png "exibição de caixa de diálogo Adicionar")

    *Adicionar caixa de diálogo de exibição*
4. O Visual Studio gera um **cshtml** exibir modelo dentro do **Views\Home** pasta e, em seguida, abre.

    ![Início de exibição do índice criado](aspnet-mvc-4-fundamentals/_static/image15.png "exibição Home índice criado")

    *Exibição de índice inicial criada*

    > [!NOTE]
    > nome e local do **cshtml** arquivo relevante e segue as convenções de nomenclatura padrão ASP.NET MVC.
    > 
    > A pasta \Views\**início** corresponde ao nome de controlador (**início** controlador). O nome do modelo de exibição (**índice**), coincide com o método de ação do controlador que deseja exibir o modo de exibição.
    > 
    > Dessa forma, o ASP.NET MVC evita a necessidade de especificar explicitamente o nome ou local de um modelo de exibição ao usar essa convenção de nomenclatura para retornar uma exibição.
5. O modelo de exibição gerado se baseia o  **\_cshtml** modelo definido anteriormente. Atualizar a propriedade ViewBag Title para **início**e alterar o conteúdo principal para **esta é a Home Page**, conforme mostrado no código a seguir:


~~~
[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
~~~
6. Selecione **MvcMusicStore** projeto no Gerenciador de soluções e pressione **F5** para executar o aplicativo.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tarefa 4: verificação

Para verificar que você executou corretamente todas as etapas no exercício anterior, faça o seguinte:

Com o aplicativo aberto em um navegador, você deve observar que:

1. Método de ação de índice do HomeController encontrado e exibidos a **\Views\Home\Index.cshtml** exibir modelo, mesmo que o código chamado **retornar View()**, pois o modelo de exibição seguido de convenção de nomenclatura padrão.
2. A Home Page exibe a mensagem de boas-vinda definida dentro de **\Views\Home\Index.cshtml** modelo de exibição.
3. A Home Page está usando o  **\_cshtml** modelo, e, portanto, a mensagem de boas-vinda está contida dentro do layout do site padrão HTML.

    ![Índice de exibição usando o LayoutPage definido e o estilo de início](aspnet-mvc-4-fundamentals/_static/image16.png "índice visualização inicial usando o LayoutPage e o estilo definido")

    *Exibição de índice inicial usando o LayoutPage e o estilo definido*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Exercício 5: Criar um modelo de exibição

Até agora, feitas seus modos de exibição exibem inserido no código HTML, mas para criar aplicativos web dinâmicos, o modelo de exibição deve receber informações do controlador. Uma técnica comum a ser usado para essa finalidade é a **ViewModel** padrão, que permite que um controlador empacotar todas as informações necessárias para gerar a resposta HTML apropriada.

Neste exercício, você aprenderá como criar uma classe ViewModel e adicionar as propriedades necessárias: o número de gêneros o armazenamento e uma lista desses gêneros. Você também atualizará o StoreController para usar o ViewModel criado e por fim, você criará um novo modelo de exibição que exibirá as propriedades mencionadas na página.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tarefa 1: Criando uma classe ViewModel

Nesta tarefa, você criará uma classe ViewModel implementa o cenário de listagem de gênero do repositório.

1. Se não estiver aberto, inicie **VS Express para Web**.
2. No **arquivo** menu, escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, vá para **Source\Ex05 CreatingAViewModel\Begin**, selecione **Begin.sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
3. Criar um **ViewModels** pasta para manter o ViewModel. Para fazer isso, clique com botão direito de nível superior **MvcMusicStore** projeto, selecione **adicionar** e **nova pasta**.

    ![Adicionando uma nova pasta](aspnet-mvc-4-fundamentals/_static/image17.png "adicionando uma nova pasta")

    *Adicionando uma nova pasta*
4. O nome da pasta *ViewModels*.

    ![Pasta ViewModels no Gerenciador de soluções](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels pasta no Gerenciador de soluções")

    *Pasta ViewModels no Gerenciador de soluções*
5. Criar um **ViewModel** classe. Para fazer isso, clique duas vezes no **ViewModels** pasta criada recentemente, selecione **adicionar** e **Novo Item**. Em **código**, escolha o **classe** item e nomeie o arquivo *StoreIndexViewModel.cs*, em seguida, clique em **adicionar**.

    ![Adicionando uma nova classe](aspnet-mvc-4-fundamentals/_static/image19.png "adicionando uma nova classe")

    *Adicionando uma nova classe*

    ![Criando classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel criação de classe")

    *Criando classe StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tarefa 2: Adicionando propriedades para a classe ViewModel

Há dois parâmetros a serem passados do StoreController para o modelo de exibição para gerar a resposta HTML esperada: o número de gêneros o armazenamento e uma lista desses gêneros.

Nesta tarefa, você adicionará essas 2 propriedades para o **StoreIndexViewModel** classe: **NumberOfGenres** (um inteiro) e **gêneros** (uma lista de cadeias de caracteres).

1. Adicionar **NumberOfGenres** e **gêneros** propriedades para o **StoreIndexViewModel** classe. Para fazer isso, adicione as seguintes 2 linhas à definição de classe:

    (Código de trecho - *ASP.NET MVC 4 conceitos básicos - propriedades de Ex5 StoreIndexViewModel*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature. It provides the benefits of a property without requiring us to declare a backing field.
~~~

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tarefa 3: atualização StoreController para usar o StoreIndexViewModel

O **StoreIndexViewModel** classe encapsula as informações necessárias para passar de **StoreController**do **índice** método para um modelo de exibição para gerar uma resposta .

Nesta tarefa, você atualizará o **StoreController** para usar o **StoreIndexViewModel**.

1. Abra **StoreController** classe.

    ![Abrir classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController de abertura de classe")

    *Classe de StoreController de abertura*
2. Para usar o **StoreIndexViewModel** classe o **StoreController**, adicione o seguinte namespace na parte superior do **StoreController** código:

    (Código de trecho - *ASP.NET MVC 4 conceitos básicos - StoreIndexViewModel Ex5 usando ViewModels*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
~~~
3. Alterar o **StoreController**do **índice** método de ação para que ele cria e preenche uma **StoreIndexViewModel** de objeto e passa em um modelo de exibição para gere uma resposta HTML com ele.

    > [!NOTE]
    > Laboratório &quot;acesso a dados e modelos do ASP.NET MVC&quot; você escreverá código que recupera a lista de gêneros de armazenamento de um banco de dados. O código a seguir, você criará uma **lista** de gêneros dados fictícios que preencherão o **StoreIndexViewModel**.
    > 
    > Depois de criar e configurar o **StoreIndexViewModel** do objeto, ela será transmitida como um argumento para o **exibição** método. Isso indica que o modelo de exibição usará esse objeto para gerar uma resposta HTML com ele.
4. Substitua o **índice** método com o código a seguir:

    (Código de trecho - *ASP.NET MVC 4 conceitos básicos - método Ex5 StoreController índice*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound. That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**. Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.
~~~

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tarefa 4 - criar um modelo de exibição que usa StoreIndexViewModel

Nesta tarefa, você criará um modelo de exibição que irá usar um objeto de StoreIndexViewModel passado do controlador para exibir uma lista de gêneros.

1. Antes de criar o novo modelo de exibição, vamos criar o projeto para que o **caixa de diálogo Adicionar modo de exibição** conhece o **StoreIndexViewModel** classe. Compile o projeto selecionando o **criar** item de menu e, em seguida, **MvcMusicStore criar**.

    ![Compilar o projeto](aspnet-mvc-4-fundamentals/_static/image22.png "compilar o projeto")

    *Criar o projeto*
2. Crie um novo modelo de exibição. Para fazer isso, clique dentro do **índice** método e selecione **adicionar exibição**.

    ![Adicionando uma exibição](aspnet-mvc-4-fundamentals/_static/image23.png "adicionando uma exibição")

    *Adicionando uma exibição*
3. Porque o **caixa de diálogo Adicionar modo de exibição** foi invocado do **StoreController**, ele adicionará o modelo de exibição por padrão em um **\Views\Store\Index.cshtml** arquivo. Verifique o **criar uma fortemente tipado-exibição** caixa de seleção e, em seguida, selecione **StoreIndexViewModel** como o **classe modelo**. Além disso, certifique-se de que o mecanismo de exibição selecionado é **Razor**. Clique em **Adicionar**.

    ![Exibição de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image24.png "exibição de caixa de diálogo Adicionar")

    *Adicionar caixa de diálogo de exibição*

    O **\Views\Store\Index.cshtml** exibir o arquivo de modelo é criado e aberto. Com base nas informações fornecidas para o **adicionar exibição** caixa de diálogo na última etapa, a exibição de modelo espera um **StoreIndexViewModel** instância como os dados a ser usado para gerar uma resposta HTML. Você observará que o modelo herda um `ViewPage<musicstore.viewmodels.storeindexviewmodel>` em c#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tarefa 5: atualizando o modelo de exibição

Nesta tarefa, você atualizará o modelo de exibição criado na última tarefa ao recuperar o número de gêneros e seus nomes dentro da página.

> [!NOTE]
> Você usará a sintaxe @ (conhecida como &quot;código novidades sobre&quot;) para executar o código dentro do modelo de exibição.


1. No **cshtml** arquivo, dentro de **repositório** pasta, substitua o código a seguir:


~~~
[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

> [!NOTE]
> As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
> 
> ![](aspnet-mvc-4-fundamentals/_static/image25.png)
> 
> *Getting Model properties and methods with Visual Studio's IntelliSense*
> 
> The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
> 
> You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
~~~
2. Loop em relação à lista de gênero em **StoreIndexViewModel** e criar um HTML **&lt;ul&gt;** lista usando um **foreach** loop.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Pressione **F5** para executar o aplicativo e procurar **/armazenamento**. Você verá a lista de gêneros passado a **StoreIndexViewModel** de objeto o **StoreController** para o modelo de exibição.

    ![Modo de exibição exibe uma lista de gêneros](aspnet-mvc-4-fundamentals/_static/image26.png "exibição exibindo uma lista de gêneros")

    *Exibindo uma lista de gêneros de exibição*
4. Feche o navegador.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Exercício 6: Usando parâmetros no modo de exibição

No Exercício 3, você aprendeu como passar parâmetros para o controlador. Neste exercício, você aprenderá como usar esses parâmetros no modelo de exibição. Para essa finalidade, você vai ser introduzido pela primeira vez para classes de modelo que ajudarão você a gerenciar sua lógica de dados e de domínio. Além disso, você aprenderá como criar links para páginas dentro do aplicativo ASP.NET MVC sem se preocupar de coisas como a codificação de caminhos de URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tarefa 1: Adicionar Classes de modelo

Diferentemente de ViewModels, que são criados apenas para passar informações do controlador para o modo de exibição, classes de modelo são criados para conter e gerenciar a lógica de dados e o domínio. Nesta tarefa, você adicionará duas classes de modelo para representar esses conceitos: **gênero** e **álbum**.

1. Se não estiver aberto, inicie **VS Express para Web**
2. No **arquivo** menu, escolha **Abrir projeto**. Na caixa de diálogo Abrir projeto, vá para **Source\Ex06 UsingParametersInView\Begin**, selecione **Begin.sln** e clique em **abrir**. Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
3. Adicionar um **gênero** classe de modelo. Para fazer isso, clique o **modelos** pasta o **Gerenciador de soluções**, selecione **adicionar** e, em seguida, o **Novo Item** opção. Em **código**, escolha o **classe** item e nomeie o arquivo *Genre.cs*, em seguida, clique em **adicionar**.

    ![Adicionando uma classe](aspnet-mvc-4-fundamentals/_static/image27.png "adicionando uma classe")

    *Adicionar um novo item*

    ![Adicionar classe de modelo de gênero](aspnet-mvc-4-fundamentals/_static/image28.png "Adicionar classe de modelo de gênero")

    *Adicionar classe de modelo de gênero*
4. Adicionar um **nome** propriedade para a classe de gênero. Para fazer isso, adicione o seguinte código:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - gênero Ex6*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
~~~
5. Siga o mesmo procedimento como antes, adicione um **álbum** classe. Para fazer isso, clique o **modelos** pasta o **Gerenciador de soluções**, selecione **adicionar** e, em seguida, o **Novo Item** opção. Em **código**, escolha o **classe** item e nomeie o arquivo *Album.cs*, em seguida, clique em **adicionar**.
6. Adicione duas propriedades à classe álbum: **gênero** e **título**. Para fazer isso, adicione o seguinte código:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - álbum Ex6*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]
~~~

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tarefa 2: adicionando um StoreBrowseViewModel

Um **StoreBrowseViewModel** será usado nessa tarefa para mostrar os álbuns que correspondem a um gênero selecionado. Nesta tarefa, você criará essa classe e, em seguida, adiciona duas propriedades para lidar com o **gênero** e sua **álbum**da lista.

1. Adicionar um **StoreBrowseViewModel** classe. Para fazer isso, clique o **ViewModels** pasta o **Solution Explorer**, selecione **adicionar** e, em seguida, o **Novo Item** opção. Em **código**, escolha o **classe** item e nomeie o arquivo *StoreBrowseViewModel.cs*, em seguida, clique em **adicionar**.
2. Adicione uma referência para os modelos no **StoreBrowseViewModel** classe. Para fazer isso, adicione o seguinte usando o namespace:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 UsingModel*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
~~~
3. Adicionar duas propriedades para **StoreBrowseViewModel** classe: **gênero** e **álbuns**. Para fazer isso, adicione o seguinte código:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 ModelProperties*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).
> 
> This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.
> 
> **List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace. One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.
~~~

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tarefa 3: usando o novo ViewModel no StoreController

Nesta tarefa, você modificará o **StoreController**do **procurar** e **detalhes** métodos de ação para usar o novo **StoreBrowseViewModel** .

1. Adicione uma referência para a pasta de modelos em **StoreController** classe. Para fazer isso, expanda o **controladores** pasta o **Solution Explorer** e abra o **StoreController** classe. Em seguida, adicione o seguinte código:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 UsingModelInController*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
~~~
2. Substitua o **procurar** método de ação para usar o **StoreViewBrowseController** classe. Você criará um gênero e dois novos objetos álbuns com dados fictícios (no próximo laboratório prático que consumirá dados reais de um banco de dados). Para fazer isso, substitua o **procurar** método com o código a seguir:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
~~~
3. Substitua o **detalhes** método de ação para usar o **StoreViewBrowseController** classe. Você criará um novo **álbum** objeto a ser retornado para o **exibição**. Para fazer isso, substitua o **detalhes** método com o código a seguir:

    (Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]
~~~

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tarefa 4: adicionando um modelo de exibição de navegação

Nesta tarefa, você adicionará um **procurar** exibição para mostrar os álbuns encontrados para um gênero específico.

1. Antes de criar o novo modelo de exibição, você deve compilar o projeto para que o **adicionar exibição** diálogo conhece o **ViewModel** classe a ser usada. Compile o projeto selecionando o **criar** item de menu e, em seguida, **MvcMusicStore criar**.
2. Adicionar um **procurar** exibição. Para fazer isso, clique no **procurar** método de ação de **StoreController** e clique em **adicionar modo de exibição**.
3. No **adicionar exibição** caixa de diálogo caixa, verifique se o nome de exibição **procurar**. Verifique o **criar uma exibição fortemente tipada** caixa de seleção e selecione **StoreBrowseViewModel** do **classe modelo** lista suspensa. Deixe os outros campos com seus valores padrão. Em seguida, clique em **adicionar**.

    ![Adicionando uma exibição de navegação](aspnet-mvc-4-fundamentals/_static/image29.png "adicionando uma exibição de navegação")

    *Adicionar um modo de procura*
4. Modificar o **Browse.cshtml** para exibir informações do gênero, acessando o **StoreBrowseViewModel** objeto que é passado para o modelo de exibição. Para fazer isso, substitua o conteúdo a seguir: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5: executando o aplicativo

Nesta tarefa, você testará que o **procurar** método recupera álbuns do **procurar** ação do método.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para   **/armazenar/procurar? Gênero = Disco** e verificar se a ação retorna dois álbuns.

    ![Navegação álbuns de Disco de armazenamento](aspnet-mvc-4-fundamentals/_static/image30.png "navegação álbuns de Disco de armazenamento")

    *Navegação álbuns de Disco de armazenamento*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tarefa 6 - exibindo informações sobre um álbum específico

Nesta tarefa, você implementará o **detalhes do repositório** para exibir informações sobre um álbum específico. Neste laboratório prático, tudo o que você irá exibir sobre o álbum já está contido no **exibição** modelo. Assim, em vez de criar um **StoreDetailsViewModel** classe, você usará o atual **StoreBrowseViewModel** modelo passando o álbum a ele.

1. Feche o navegador se necessário, para retornar à janela do Visual Studio. Adicionar um novo **detalhes** exibir para o **StoreController**do **detalhes** método de ação. Para fazer isso, clique com botão direito do **detalhes** método o **StoreController** classe e clique em **adicionar modo de exibição**.
2. No **adicionar exibição** caixa de diálogo, verifique o **nome de exibição** é **detalhes**. Verifique o **criar uma exibição fortemente tipada** caixa de seleção e selecione **álbum** do **classe modelo** lista suspensa. Deixe os outros campos com seus valores padrão. Em seguida, clique em **adicionar**. Isso irá criar e abrir um **\Views\Store\Details.cshtml** arquivo.

    ![Adicionando uma exibição de detalhes](aspnet-mvc-4-fundamentals/_static/image31.png "adicionando uma exibição de detalhes")

    *Adicionando uma exibição de detalhes*
3. Modificar o **Details.cshtml** arquivo para exibir informações do álbum, acessando o **álbum** objeto que é passado para o modelo de exibição. Para fazer isso, substitua o conteúdo a seguir: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarefa 7: executando o aplicativo

Nesta tarefa, você testará que o **detalhes** exibição recupera informações do álbum do **detalhes ação** método.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado **início** página. Altere a URL para **/Store/Details/5** para verificar as informações do álbum.

    ![Procurando detalhes álbuns](aspnet-mvc-4-fundamentals/_static/image32.png "navegação álbuns detalhes")

    *Detalhes do álbum de navegação*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tarefa 8 - adicionando Links entre páginas

Nesta tarefa, você adicionará um link na exibição de repositório para ter um link em cada nome de gênero ao apropriado **repositório/procurar** URL. Dessa forma, quando você clica em um gênero, por exemplo **Disco**, ele navegará para **repositório/procurar? gênero = Disco** URL.

1. Feche o navegador se necessário, para retornar à janela do Visual Studio. Atualização de **índice** página para adicionar um link para o **procurar** página. Para fazer isso, no **Gerenciador de soluções** expandir o **exibições** pasta, o **repositório** pasta e clique duas vezes o **cshtml** página.
2. Adicione um link para o modo de procura que indica o gênero selecionado. Para fazer isso, substitua o seguinte código dentro de **&lt;li&gt;** marcas: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > outra abordagem seria possível vincular diretamente para a página, com um código semelhante ao seguinte:
   > 
   > &lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Embora essa abordagem funciona, depende de uma cadeia de caracteres codificada. Se você renomear o controlador mais tarde, você precisará alterar essa instrução manualmente. Uma alternativa melhor é usar um **auxiliar HTML** método. O ASP.NET MVC inclui um método auxiliar HTML que está disponível para essas tarefas. O **Html.ActionLink()** método auxiliar torna mais fácil criar HTML **&lt;um&gt;** links, certificando-se os caminhos de URL corretamente são codificados de URL.
   > 
   > Htlm.ActionLink tem várias sobrecargas. Neste exercício você usará um que usa três parâmetros:
   > 
   > 1. Texto do link, que exibe o nome de gênero
   > 2. Nome de ação do controlador (**procurar**)
   > 3. Valores de parâmetro, especificando o nome de rota (**gênero**) e o valor (**gênero**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tarefa 9 - executando o aplicativo

Nesta tarefa, você testará que cada gênero é exibido com um link para as **repositório/procurar** URL.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/armazenamento** para verificar se cada gênero contém links para as **repositório/procurar** URL.

    ![Navegação gêneros com links para a página Procurar](aspnet-mvc-4-fundamentals/_static/image33.png "gêneros de navegação com links para a página de pesquisa")

    *Navegação gêneros com links para a página de pesquisa*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tarefa 10 - usando a coleção de ViewModel dinâmico para passar valores

Nesta tarefa, você aprenderá a um método simple e eficiente para transmitir valores entre o controlador e o modo de exibição sem fazer alterações no modelo. ASP.NET MVC 4 fornece a coleção &quot;ViewModel&quot;, que pode ser atribuído a qualquer valor dinâmico e acessado dentro de controladores e modos de exibição.

Agora você usará o conjunto dinâmico ViewBag para passar uma lista de &quot; **contêm gêneros** &quot; do controlador para o modo de exibição. O modo de exibição do índice de repositório acessará a **ViewModel** e exibir as informações.

1. Feche o navegador se necessário, para retornar à janela do Visual Studio. Abra **StoreController.cs** e modificar **índice** método para criar uma lista de contêm gêneros ViewModel coleção:


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

> [!NOTE]
> You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.
~~~
2. O ícone de estrela **&quot;starred.png&quot;** está incluído no **Source\Assets\Images** pasta deste laboratório. Para adicioná-lo para o aplicativo, arraste o conteúdo de um **Windows Explorer** janela para o **Solution Explorer** no Visual Web Developer Express, conforme mostrado abaixo:

    ![Imagem de estrela adicionando à solução](aspnet-mvc-4-fundamentals/_static/image34.png "adicionando imagem de estrela para a solução")

    *Adicionar imagem de estrela para a solução*
3. Abra a exibição **Store/Index.cshtml** e modificar o conteúdo. Você lerá o &quot;contêm&quot; propriedade o **ViewBag** coleção e pergunte se o nome de gênero atual está na lista. Nesse caso, você mostrará um ícone de estrela direita para o link de gênero.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tarefa 11 - executando o aplicativo

Nesta tarefa, você testará os gêneros com estrelas exibem um ícone de estrela.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado **início** página. Altere a URL para **/armazenamento** para verificar se cada gênero em destaque tem o rótulo respecting:

    ![Navegação gêneros com elementos com estrelas](aspnet-mvc-4-fundamentals/_static/image35.png "gêneros navegação com elementos com estrelas")

    *Navegação gêneros com elementos com estrelas*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Exercício 7: Visão geral do novo modelo do ASP.NET MVC 4

Neste exercício, você irá explorar os aprimoramentos nos modelos de projeto ASP.NET MVC 4, levando uma aparência recursos mais relevantes do novo modelo.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tarefa 1: Explorando o modelo de aplicativo de Internet do ASP.NET MVC 4

1. Se não estiver aberto, inicie **VS Express para Web**
2. Selecione o **arquivo | Novo | Projeto** comando de menu. No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha o **aplicativo Web do ASP.NET MVC 4**. **Nome** o projeto *MusicStore* e atualize o **nome da solução** para *começar*, em seguida, selecione um local (ou deixe o padrão) e clique em **Okey** .

    ![Criar um novo projeto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "criar um novo projeto ASP.NET MVC 4")

    *Criar um novo projeto ASP.NET MVC 4*
3. No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**. Observe que você pode selecionar Razor ou ASPX como o mecanismo de exibição.

    ![Criar um novo aplicativo ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "criar um novo aplicativo ASP.NET MVC 4 da Internet")

    *Criar um novo aplicativo ASP.NET MVC 4 da Internet*

    > [!NOTE]
    > A sintaxe do Razor foi introduzida no ASP.NET MVC 3. Seu objetivo é minimizar o número de caracteres e pressionamentos de tecla necessários em um arquivo, permitindo um rápido e fluido fluxo de trabalho de codificação. Aproveita o Razor existente C# /VB (ou outro) habilidades de linguagem e fornece uma sintaxe de marcação de modelo que permite que um fluxo de trabalho de construção awesome HTML.
4. Pressione **F5** para executar a solução e ver o modelo renovado. Você pode conferir os recursos a seguir:

    1. **Modelos de estilo moderno**

        Os modelos foram renovados, fornecendo mais estilos aparência moderna.

        ![Modelos do ASP.NET MVC 4 remodelado](aspnet-mvc-4-fundamentals/_static/image38.png "remodelado modelos do ASP.NET MVC 4")

        *Modelos do ASP.NET MVC 4 remodelado*
    2. **Renderização adaptável**

        Check-out de redimensionar a janela do navegador e observe como o layout de página adapta dinamicamente para o novo tamanho da janela. Esses modelos de usam a técnica de renderização adaptável para processar corretamente em plataformas móveis e desktop sem nenhuma personalização.

        ![Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador](aspnet-mvc-4-fundamentals/_static/image39.png "modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador")

        *Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador*
5. Feche o navegador para interromper o depurador e retornar ao Visual Studio.
6. Agora você já pode explorar a solução e check-out de alguns dos novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.

    ![ASP.NET MVC4-internet---modelo de projeto aplicativo](aspnet-mvc-4-fundamentals/_static/image40.png "o modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")

    *O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*

   1. **Marcação de HTML5**

       Procurar exibições de modelo para descobrir a marcação novo tema, por exemplo aberta **About.cshtml** exibir em **início** pasta.

       ![Novo modelo, usando marcação Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "novo modelo, usando marcação Razor e HTML5")

       *Novo modelo, usando marcação Razor e HTML5*
   2. **Bibliotecas JavaScript incluídas**

      1. **jQuery**: jQuery simplifica o desvio de documento HTML, manipulação de eventos, animação e interações de Ajax.
      2. **jQuery UI**: esta biblioteca fornece abstrações de interação de baixo nível avançado de animações, efeitos e widgets de um tema, criadas sobre a biblioteca JavaScript jQuery.

         > [!NOTE]
         > Você pode aprender sobre jQuery e jQuery UI em [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: modelo de padrão do ASP.NET MVC 4 agora inclui **KnockoutJS**, uma estrutura de JavaScript MVVM que permite que você crie aplicativos web altamente responsivo e avançado usando JavaScript e HTML. Como no ASP.NET MVC 3, jQuery e jQuery bibliotecas de interface do usuário também estão incluídas no ASP.NET MVC 4.

          > [!NOTE]
          > Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: esta biblioteca é executado automaticamente, tornando o seu site compatível com navegadores mais antigos com tecnologias de HTML5 e CSS3.

          > [!NOTE]
          > Você pode obter mais informações sobre a biblioteca Modernizr presentes neste link: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership incluído na solução**

       SimpleMembership foi projetado como uma substituição para o sistema de provedor de função ASP.NET e associação anterior. Ele tem muitos recursos novos que facilitam para o desenvolvedor de páginas da web seguras de forma mais flexível.

       O modelo de Internet já configurou algumas coisas a integrar SimpleMembership, por exemplo, o AccountController está preparado para usar OAuthWebSecurity (para o registro de conta OAuth, login, gerenciamento, etc.) e segurança da Web.

       ![SimpleMembership incluídos na solução](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluídos na solução")

       *SimpleMembership incluídos na solução*

       > [!NOTE]
       > Para obter mais informações sobre [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) no MSDN.

> [!NOTE]
> Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu os fundamentos do ASP.NET MVC:

- Os elementos principais de um aplicativo MVC e como eles interagem
- Como criar um aplicativo ASP.NET MVC
- Como adicionar e configurar controladores para lidar com os parâmetros passados a URL e querystring
- Como adicionar uma página de layout mestre para configurar um modelo de conteúdo HTML comum, uma folha de estilos para aprimorar a aparência e a um modelo de exibição para exibir o conteúdo HTML
- Como usar o padrão de ViewModel para passar propriedades para o modelo de exibição para exibir informações dinâmicas
- Como usar parâmetros passados para os controladores no modelo de exibição
- Como adicionar links para páginas dentro do aplicativo ASP.NET MVC
- Como adicionar e usar propriedades dinâmicas em uma exibição
- Os aprimoramentos nos modelos de projeto do ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express para o bloco de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice b: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1: criar um novo Site do Windows Azure Portal

1. Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce. Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal de gerenciamento do Azure do Windows*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo Site](aspnet-mvc-4-fundamentals/_static/image49.png "criar um novo Site")

    *Criando um novo Site*
3. Clique em **de computação** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site e clique em **Criar Site**.

    > [!NOTE]
    > Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](aspnet-mvc-4-fundamentals/_static/image50.png "criar um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando para o novo site](aspnet-mvc-4-fundamentals/_static/image51.png "navegando para o novo site")

    *Navegando para o novo site*

    ![Site da Web em execução](aspnet-mvc-4-fundamentals/_static/image52.png "site em execução")

    *Site da Web em execução*
6. Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-fundamentals/_static/image53.png "abrir páginas de gerenciamento do site")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.

    ![Baixando o site da web de perfil de publicação](aspnet-mvc-4-fundamentals/_static/image54.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação para um local conhecido. Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-fundamentals/_static/image55.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL. Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**. Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados, como ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image56.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) botão.

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Adicionar endereço IP do cliente*
3. Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.

    ![Publicar o aplicativo](aspnet-mvc-4-fundamentals/_static/image60.png "a publicação do aplicativo")

    *Publicar o site*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importar o perfil de publicação](aspnet-mvc-4-fundamentals/_static/image61.png "importar o perfil de publicação")

    *Importar o perfil de publicação*
3. Clique em **validar Conexão**. Quando a validação estiver concluída, clique em **próximo**.

    > [!NOTE]
    > A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.

    ![Validando a conexão](aspnet-mvc-4-fundamentals/_static/image62.png "validação de conexão")

    *Validação de conexão*
4. No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Em **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Em **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-fundamentals/_static/image64.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](aspnet-mvc-4-fundamentals/_static/image65.png "criar a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image66.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **visualização** , clique em **publicar**.

    ![Publicar o aplicativo web](aspnet-mvc-4-fundamentals/_static/image67.png "publicar o aplicativo web")

    *Publicar o aplicativo web*
9. Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.

    ![Aplicativo publicado no Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplicativo publicado no Windows Azure")

    *Aplicativos publicados no Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice c: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-fundamentals/_static/image69.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](aspnet-mvc-4-fundamentals/_static/image70.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-fundamentals/_static/image71.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-fundamentals/_static/image72.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-fundamentals/_static/image73.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-fundamentals/_static/image74.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
