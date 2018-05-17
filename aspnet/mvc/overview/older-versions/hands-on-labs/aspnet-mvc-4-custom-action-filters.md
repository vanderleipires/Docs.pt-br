---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de ação personalizada do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fornece filtros de ação para a execução de lógica de filtragem antes ou depois de um método de ação é chamado. Filtros de ação são tha de atributos personalizados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtros de ação personalizada do ASP.NET MVC 4

Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC fornece filtros de ação para a execução de lógica de filtragem antes ou depois de um método de ação é chamado. Filtros de ação são os atributos personalizados que fornecem um meio declarativo para adicionar o comportamento de ação de pré e pós-ação para métodos de ação do controlador.

Este laboratório prático, você criará um atributo de filtro de ação personalizada na solução MvcMusicStore capturar solicitações do controlador e registra a atividade de um site em uma tabela de banco de dados. Você poderá adicionar o filtro de registro em log pela inclusão de qualquer controlador ou ação. Por fim, você verá o modo de exibição de log que mostra a lista de visitantes.

Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC**. Se você não usou **ASP.NET MVC** antes, é recomendável que você passe **conceitos básicos do ASP.NET MVC 4** laboratório prático.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível no site da [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [filtros de ação personalizada do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Crie um atributo de filtro de ação personalizada para estender os recursos de filtragem
- Aplicar um atributo de filtro personalizado por injeção de um nível específico
- Registrar um filtros de ação personalizada globalmente

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configuração

**Instalando os trechos de código**

Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático é composto pelos exercícios a seguir:

1. [Exercício 1: O log de ações](#Exercise1)
2. [Exercício 2: Gerenciar vários filtros de ação](#Exercise2)

Tempo estimado para concluir este laboratório: **30 minutos**.

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Exercício 1: O log de ações

Neste exercício, você aprenderá como criar um filtro de log de ação personalizada usando provedores de filtro do ASP.NET MVC 4. Para essa finalidade, você aplicará um filtro de registro em log para o site MusicStore que registra todas as atividades em controladores de selecionada.

O filtro estenderá **ActionFilterAttributeClass** e substituir **OnActionExecuting** método capturar cada solicitação e, em seguida, execute as ações de registro em log. As contexto informações sobre solicitações HTTP, executar métodos, resultados e parâmetros será fornecida pelo ASP.NET MVC **ActionExecutingContext** classe **.**

> [!NOTE]
> Além disso, o ASP.NET MVC 4 tem provedores de filtros de padrão, você pode usar sem criar um filtro personalizado. ASP.NET MVC 4 fornece os seguintes tipos de filtros:
> 
> - **Autorização** filtrar, que toma decisões de segurança sobre a possibilidade de executar um método de ação, como autenticação ou validação de propriedades da solicitação.
> - **Ação** filtro, que envolve a execução do método de ação. Esse filtro pode executar processamento adicional, como fornecer dados adicionais para o método de ação, inspecionando o valor de retorno ou cancelando a execução do método de ação
> - **Resultado** filtro, que envolve a execução do objeto ActionResult. Esse filtro pode executar processamento adicional dos resultados, como modificar a resposta HTTP.
> - **Exceção** filtro, que é executado se houver uma exceção sem tratamento em algum lugar lançada no método de ação, começando com os filtros de autorização e terminando com a execução do resultado. Filtros de exceção podem ser usados para tarefas como registrar em log ou exibir uma página de erro.
> 
> Para obter mais informações sobre filtros de provedores, visite este link do MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Sobre o recurso de log de aplicativo de repositório de música do MVC

Esta solução de repositório de música tem uma nova tabela de modelo de dados para o log do site, **ActionLog**, com os seguintes campos: nome do controlador que recebeu uma solicitação, a ação de chamado, o IP do cliente e o carimbo de data / hora.

![Modelo de dados. Tabela ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de dados. Tabela ActionLog.")

*Modelo de dados - ActionLog tabela*

A solução oferece um modo de exibição do ASP.NET MVC para o log de ações que pode ser encontrado em **MvcMusicStores/exibições/ActionLog**:

![Exibição de Log de ação](aspnet-mvc-4-custom-action-filters/_static/image2.png "exibição de Log de ações")

*Exibição de Log de ação*

Com isso considerando a estrutura, todo o trabalho se concentrarão na solicitação do controlador de interrupção e executar o registro em log usando o recurso de filtragem personalizada.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tarefa 1: criar um filtro personalizado para capturar a solicitação do controlador

Nesta tarefa, você criará uma classe de atributo de filtro personalizado que contém a lógica de registro em log. Para essa finalidade, você estenderá o ASP.NET MVC **ActionFilterAttribute** classe e implementar a interface **IActionFilter**.

> [!NOTE]
> O **ActionFilterAttribute** é a classe base para todos os filtros de atributo. Ele fornece os seguintes métodos para executar uma lógica específica depois e antes da execução da ação do controlador:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): antes da ação de método é chamado.
> - **OnActionExecuted**(ActionExecutedContext filterContext): depois que o método de ação é chamado e antes do resultado ser executado (antes da renderização de exibição).
> - **OnResultExecuting**(ResultExecutingContext filterContext): antes do resultado ser executado (antes da renderização de exibição).
> - **OnResultExecuted**(ResultExecutedContext filterContext): após o resultado é executado (depois que a exibição é renderizada).
> 
> Substituindo qualquer um desses métodos em uma classe derivada, você pode executar seu próprio código de filtragem.


1. Abra o **começar** solução localizado em **\Source\Ex01-LoggingActions\Begin** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
      > 
      > Para obter mais informações, consulte este artigo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Adicionar uma nova classe c# para o **filtros** pasta e nomeie-o *CustomActionFilter.cs*. Essa pasta armazena todos os filtros personalizados.
3. Abra **CustomActionFilter.cs** e adicione uma referência a **System.Web.Mvc** e **MvcMusicStore.Models** namespaces:

    (Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Herdar o **CustomActionFilter** classe **ActionFilterAttribute** e, em seguida, fazer **CustomActionFilter** implementar classe **IActionFilter** interface.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Verifique **CustomActionFilter** classe substituir o método **OnActionExecuting** e adicione a lógica necessária para registrar a execução do filtro. Para fazer isso, adicione o seguinte código dentro de **CustomActionFilter** classe.

    (Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** usando o método **do Entity Framework** para adicionar um novo registro de ActionLog. Ele cria e preenche uma nova instância de entidade com as informações de contexto do **filterContext**.
    > 
    > Você pode ler mais sobre **ControllerContext** classe em [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tarefa 2 - injetar um interceptador de código para a classe do controlador de armazenamento

Nesta tarefa, você irá adicionar o filtro personalizado injetando-lo a todas as classes do controlador e ações do controlador que serão registradas. Neste exercício, a classe do controlador de armazenamento terá um log.

O método **OnActionExecuting** de **ActionLogFilterAttribute** filtro personalizado é executado quando um elemento injetado é chamado.

Também é possível interceptar um método do controlador específico.

1. Abra o **StoreController** em **MvcMusicStore\Controllers** e adicione uma referência para o **filtros** namespace:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Inserir o filtro personalizado **CustomActionFilter** em **StoreController** classe adicionando **[CustomActionFilter]** atributo antes da declaração de classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Quando um filtro é injetado em uma classe de controlador, todas as suas ações também são injetadas. Se você deseja aplicar o filtro apenas para um conjunto de ações, você precisaria injetar **[CustomActionFilter]** para cada um deles:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executando o aplicativo

Nesta tarefa, você testará que o filtro de registro em log está funcionando. Inicie o aplicativo e visitar a loja e, em seguida, você verificará as atividades registradas.

1. Pressione **F5** para executar o aplicativo.
2. Navegue até **/ActionLog** para ver o estado inicial de exibição de log:

    ![Registrar o status do controlador antes da atividade de página](aspnet-mvc-4-custom-action-filters/_static/image3.png "registrar status do controlador antes da atividade de página")

    *Status do controlador do log antes da atividade de página*

   > [!NOTE]
   > Por padrão, ele sempre mostrará um item que é gerado quando recuperar os gêneros existentes para o menu.
   > 
   > Para fins de simplicidade, está limpando o **ActionLog** sempre que o aplicativo é executado para mostrará apenas os logs de verificação da cada tarefa específica tabela.
   > 
   > Você pode precisar remover o código a seguir do **sessão\_iniciar** método (no **global. asax** classe), para salvar um log de histórico para todas as ações executadas no repositório de Controlador.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.
4. Navegue até **/ActionLog** e se o log estiver vazio pressione **F5** para atualizar a página. Verifique se suas visitas foram rastreadas:

    ![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image4.png "log de ações com atividades registradas")

    *Log de ação com atividades registradas*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Exercício 2: Gerenciar vários filtros de ação

Neste exercício, você irá adicionar um segundo filtro de ação personalizada para a classe StoreController e definir da ordem específica no qual ambos os filtros serão executados. Em seguida, você atualizará o código para registrar o filtro global.

Há diferentes opções para levar em conta ao definir a ordem de execução de filtros. Por exemplo, a propriedade de ordem e o escopo de filtros:

Você pode definir um **escopo** para cada um dos filtros, por exemplo, você pode definir o escopo todos os filtros de ação para ser executado no **escopo controlador**e todos os filtros de autorização para executar em **escopo Global** . Os escopos terão a ordem de execução definido.

Além disso, cada filtro de ação tem uma propriedade de ordem que é usada para determinar a ordem de execução no escopo do filtro.

Para obter mais informações sobre a ordem de execução de filtros de ação personalizada, visite este artigo do MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tarefa 1: Criando um novo filtro de ação personalizada

Nesta tarefa, você criará um novo filtro de ação personalizada para injetar na classe StoreController, aprender como gerenciar a ordem de execução dos filtros.

1. Abra o **começar** solução localizado em **\Source\Ex02-ManagingMultipleActionFilters\Begin** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

        > [!NOTE]
        > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
        > 
        > Para obter mais informações, consulte este artigo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Adicionar uma nova classe c# para o **filtros** pasta e nomeie-o *MyNewCustomActionFilter.cs*
3. Abra **MyNewCustomActionFilter.cs** e adicione uma referência a **System.Web.Mvc** e **MvcMusicStore.Models** namespace:

    (Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - o Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Substitua a declaração de classe padrão com o código a seguir.

    (Código de trecho - *filtros de ação personalizada do ASP.NET MVC 4 - o Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Esse filtro de ação personalizada é quase o mesmo que você criou no exercício anterior. A principal diferença é que ela tem o *&quot;registradas por&quot;* atributo atualizado com de nome essa nova classe para identificar qual filtro registrado no log.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tarefa 2: Inserindo um novo interceptador de código na classe StoreController

Nesta tarefa, você irá adicionar um novo filtro personalizado na classe StoreController e executar a solução para verificar como ambos os filtros funcionam em conjunto.

1. Abra o **StoreController** classe localizado em **MvcMusicStore\Controllers** e inserir o novo filtro personalizado **MyNewCustomActionFilter** em  **StoreController** classe como é mostrado no código a seguir.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Agora, execute o aplicativo para ver como funcionam essas dois filtros de ação personalizada. Para fazer isso, pressione **F5** e aguarde até que o aplicativo for iniciado.
3. Navegue até **/ActionLog** para ver o estado inicial de exibição de log.

    ![Registrar o status do controlador antes da atividade de página](aspnet-mvc-4-custom-action-filters/_static/image5.png "registrar status do controlador antes da atividade de página")

    *Status do controlador do log antes da atividade de página*
4. Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.
5. Verifique se neste momento. suas visitas eram controladas duas vezes: depois de cada um dos filtros de ação personalizada que você adicionou no **StorageController** classe.

    ![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image6.png "log de ações com atividades registradas")

    *Log de ação com atividades registradas*
6. Feche o navegador.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tarefa 3: Gerenciando a ordem do filtro

Nesta tarefa, você aprenderá como gerenciar a ordem de execução de filtros usando a propriedade de ordem.

1. Abra o **StoreController** classe localizado em **MvcMusicStore\Controllers** e especifique o **ordem** propriedade em ambos os filtros, como mostrado abaixo.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Agora, verificar como os filtros são executados dependendo do valor da propriedade sua ordem. Você encontrará a que o filtro com o menor valor de ordem (**CustomActionFilter**) é o primeiro que é executado. Pressione **F5** e aguarde até que o aplicativo for iniciado.
3. Navegue até **/ActionLog** para ver o estado inicial de exibição de log.

    ![Registrar o status do controlador antes da atividade de página](aspnet-mvc-4-custom-action-filters/_static/image7.png "registrar status do controlador antes da atividade de página")

    *Status do controlador do log antes da atividade de página*
4. Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.
5. Verifique se esse tempo, suas visitas eram controladas ordenados pelo valor de ordem de filtros: **CustomActionFilter** logs' primeiro.

    ![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image8.png "log de ações com atividades registradas")

    *Log de ação com atividades registradas*
6. Agora, você atualizar o valor de ordem de filtros e verificar como a ordem de registro em log é alterado. No **StoreController** classe, atualize o valor de ordem de filtros como mostrado abaixo.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Execute o aplicativo novamente pressionando **F5**.
8. Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.
9. Verifique se esse tempo, os logs criadas por **MyNewCustomActionFilter** filtro aparece primeiro.

    ![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image9.png "log de ações com atividades registradas")

    *Log de ação com atividades registradas*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tarefa 4: Registrar filtros globalmente

Nesta tarefa, você atualizará a solução para registrar o novo filtro (**MyNewCustomActionFilter**) como um filtro global. Ao fazer isso, ele será disparado por todo a executada ações no aplicativo e não apenas em que o StoreController da tarefa anterior.

1. Em **StoreController** classe, remova **[MyNewCustomActionFilter]** atributo e a propriedade de ordem de **[CustomActionFilter]**. Ela deve parecer com o seguinte:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Abra **global. asax** de arquivo e localize o **aplicativo\_iniciar** método. Observe que cada vez que o aplicativo é iniciado ele está registrando os filtros globais chamando **RegisterGlobalFilters** método **FilterConfig** classe.

    ![Registro de filtros globais em global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registro de filtros globais em global. asax")

    *Registro de filtros globais em global. asax*
3. Abra **FilterConfig.cs** dentro do arquivo **aplicativo\_iniciar** pasta.
4. Adicione uma referência ao uso de System.Web.Mvc; usando MvcMusicStore.Filters; namespace.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Atualização **RegisterGlobalFilters** método adicionando seu filtro personalizado. Para fazer isso, adicione o código realçado:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Execute o aplicativo pressionando **F5**.
7. Clique em um do **gêneros** no menu e executar algumas ações, como navegar um álbum disponível.
8. Verificação de que agora **[MyNewCustomActionFilter]** está sendo injetado em HomeController e ActionLogController muito.

    ![Log de ação com atividades registradas](aspnet-mvc-4-custom-action-filters/_static/image11.png "log de ações com atividades registradas")

    *Log de ação com a atividade global conectada*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu como estender um filtro de ação para executar ações personalizadas. Você também aprendeu como injetar qualquer filtro com os controladores de página. Foram usados os seguintes conceitos:

- Como criar filtros de ação personalizada com a classe de ActionFilterAttribute do ASP.NET MVC
- Como inserir filtros em controladores de ASP.NET MVC
- Como gerenciar a ordenação de filtro usando a propriedade de ordem
- Como registrar filtros globalmente

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Faça logon no portal do Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal de gerenciamento do Azure do Windows*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "criar um novo Site")

    *Criando um novo Site*
3. Clique em **de computação** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site e clique em **Criar Site**.

    > [!NOTE]
    > Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "criar um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando para o novo site](aspnet-mvc-4-custom-action-filters/_static/image20.png "navegando para o novo site")

    *Navegando para o novo site*

    ![Site da Web em execução](aspnet-mvc-4-custom-action-filters/_static/image21.png "site em execução")

    *Site da Web em execução*
6. Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-custom-action-filters/_static/image22.png "abrir páginas de gerenciamento do site")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.

    ![Baixando o site da web de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image23.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação para um local conhecido. Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image24.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL. Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**. Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados, como ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) botão.

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Adicionar endereço IP do cliente*
3. Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.

    ![Publicar o aplicativo](aspnet-mvc-4-custom-action-filters/_static/image29.png "a publicação do aplicativo")

    *Publicar o site*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importar o perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image30.png "importar o perfil de publicação")

    *Importar o perfil de publicação*
3. Clique em **validar Conexão**. Quando a validação estiver concluída, clique em **próximo**.

    > [!NOTE]
    > A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.

    ![Validando a conexão](aspnet-mvc-4-custom-action-filters/_static/image31.png "validação de conexão")

    *Validação de conexão*
4. No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Em **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Em **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](aspnet-mvc-4-custom-action-filters/_static/image34.png "criar a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **visualização** , clique em **publicar**.

    ![Publicar o aplicativo web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publicar o aplicativo web")

    *Publicar o aplicativo web*
9. Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice c: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-custom-action-filters/_static/image37.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](aspnet-mvc-4-custom-action-filters/_static/image38.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-custom-action-filters/_static/image39.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-custom-action-filters/_static/image40.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-custom-action-filters/_static/image41.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-custom-action-filters/_static/image42.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
