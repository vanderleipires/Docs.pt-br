---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Auxiliares do ASP.NET MVC 4, formulários e validação | Microsoft Docs
author: rick-anderson
description: Em modelos do ASP.NET MVC 4 e laboratório prático do dados de acesso, você foi Carregando e exibindo dados do banco de dados. Neste laboratório prático, você adicionará ao...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: a84e35695fa08ac1bd4834d2803d2be76f863e5b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815915"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Validação, formulários e auxiliares do ASP.NET MVC 4

por [Web Camps equipe](https://twitter.com/webcamps)

[Baixe o Kit de treinamento do Web Camps](https://aka.ms/webcamps-training-kit)

Na **acesso a dados e modelos do ASP.NET MVC 4** laboratório prático, você foi o carregamento e a exibição de dados do banco de dados. Neste laboratório prático, você irá adicionar para o **Music Store** aplicativo a capacidade de editar esses dados.

Com esse objetivo em mente, primeiro você criará o controlador que dará suporte as ações criar, ler, atualizar e excluir (CRUD) de álbuns. Você irá gerar um modelo de exibição de índice, tirando proveito do recurso de scaffolding de ASP.NET MVC para exibir as propriedades dos álbuns em uma tabela HTML. Para aprimorar esse modo de exibição, você adicionará um auxiliar HTML personalizado que irá truncar descrições longas.

Depois disso, você adicionará o editar e criar exibições que permitem que você alteram os álbuns no banco de dados, com a Ajuda de elementos de formulário, como listas suspensas.

Por fim, você permitirá que os usuários excluir um álbum e também você impedirá de inserção de dados errados, validando suas entradas.

Este laboratório prático pressupõe que você tenha um conhecimento básico dos **ASP.NET MVC**. Se você não usou **ASP.NET MVC** antes, recomendamos que você passe **conceitos básicos do ASP.NET MVC** laboratório prático.

Este laboratório orienta os aprimoramentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo Web de exemplo fornecido na pasta de origem.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [versões Web/Microsoft-WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [auxiliares do ASP.NET MVC 4, formulários e validação](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Criar um controlador para dar suporte a operações CRUD
- Gerar uma exibição de índice para exibir as propriedades de entidade em uma tabela HTML
- Adicionar um auxiliar HTML personalizado
- Criar e personalizar um modo de exibição editar
- Diferenciar entre os métodos de ação que reagem a HTTP-GET ou chamadas de HTTP POST
- Adicionar e personalizar um modo de exibição criar
- Lidar com a exclusão de uma entidade
- Validar a entrada do usuário

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configuração

**Instalando os trechos de código**

Para sua conveniência, grande parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código executados **.\Source\Setup\CodeSnippets.vsi** arquivo.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [apêndice b: usando o Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Os seguintes exercícios compõem esse laboratório prático:

1. [Criando o controlador do Store Manager e sua exibição de índice](#Exercise1)
2. [Adicionando um auxiliar de HTML](#Exercise2)
3. [Criando a exibição de edição](#Exercise3)
4. [Adicionando uma exibição Create](#Exercise4)
5. [Tratamento de exclusão](#Exercise5)
6. [Adicionando uma Validação](#Exercise6)
7. [Usando o jQuery Unobtrusive no lado do cliente](#Exercise7)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Exercício 1: Criando o controlador do Store Manager e sua exibição de índice

Neste exercício, você aprenderá a criar um novo controlador para dar suporte a operações de CRUD, personalizar seu método de ação de índice para retornar uma lista dos álbuns do banco de dados e, finalmente, gerando um modelo de exibição de índice que aproveita o scaffolding de ASP.NET MVC recurso para exibir as propriedades dos álbuns em uma tabela HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tarefa 1: Criando o StoreManagerController

Nesta tarefa, você criará um novo controlador chamado **StoreManagerController** para dar suporte a operações CRUD.

1. Abra o **começar** solução localizado em **origem/Ex1-CreatingTheStoreManagerController/início/** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Adicione um novo controlador. Para fazer isso, clique com botão direito a **controladores** pasta dentro do Gerenciador de soluções, selecione **Add** e, em seguida, o **controlador** comando. Alterar o **controlador** **nome** para **StoreManagerController** e verifique se a opção **controlador MVC com ações de leitura/gravação vazias**está selecionado. Clique em **Adicionar**.

    ![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "caixa de diálogo Adicionar controlador")

    *Adicionar caixa de diálogo do controlador*

    Uma nova classe de controlador é gerada. Uma vez que você indicou para adicionar ações de leitura/gravação, métodos stub para aqueles, ações de CRUD comuns são criadas com comentários TODO preenchidos, solicitando que inclua a lógica específica do aplicativo.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tarefa 2 – Personalizando o índice StoreManager

Nesta tarefa, você personalizará o método de ação de índice StoreManager para retornar uma exibição com a lista de álbuns do banco de dados.

1. Na classe StoreManagerController, adicione o seguinte *usando* diretivas.

    (Código de trecho de código – *validação - Ex1, formulários e auxiliares do ASP.NET MVC 4 usando MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Adicione um campo para o **StoreManagerController** para armazenar uma instância de **MusicStoreEntities.**

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implemente a ação de índice StoreManagerController para retornar uma exibição com a lista dos álbuns.

    A lógica de ação do controlador será muito semelhante à ação de índice do StoreController gravada anteriormente. Use o LINQ para recuperar todos os álbuns, incluindo informações de gênero e artista para exibição.

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - Ex1 StoreManagerController índice*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tarefa 3 - criar o modo de exibição de índice

Nesta tarefa, você irá criar o modelo de exibição de índice para exibir a lista de álbuns retornado pela **StoreManager** controlador.

1. Antes de criar o novo modelo de exibição, você deve compilar o projeto para que o **caixa de diálogo Adicionar modo de exibição** sabe sobre o **álbum** classe a ser usada. Selecione **compilar | Compilar MvcMusicStore** para compilar o projeto.
2. Clique com botão direito dentro do **índice** método de ação e selecione **adicionar exibição**. Isso abrirá o **adicionar exibição** caixa de diálogo.

    ![Adicionar exibição](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "adicionar modo de exibição")

    *Adicionando uma exibição de dentro do método Index*
3. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição está **índice**. Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe de modelo** lista suspensa. Selecione **lista** da **modelo Scaffold** lista suspensa. Deixe o **mecanismo de exibição** à **Razor** e os outros campos com seus padrões de valor e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "adicionando uma exibição de índice")

    *Adicionando uma exibição de índice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tarefa 4 - personalizar o scaffold do modo de exibição de índice

Nesta tarefa, você irá ajustar o modelo de exibição simple criado com o recurso de scaffolding do ASP.NET MVC para que ele exiba os campos desejados.

> [!NOTE]
> O **scaffolding** suporte dentro do ASP.NET MVC gera um modelo de exibição simple que lista todos os campos no modelo de álbum. **Scaffolding** fornece uma maneira rápida de começar a usar em uma exibição fortemente tipada: em vez de precisar gravar o modelo de exibição manualmente, rapidamente o scaffolding gera um modelo padrão e, em seguida, você pode modificar o código gerado.


1. Examine o código criado. Lista de campos gerada farão parte das seguintes tabela HTML **Scaffolding** está usando para exibir dados tabulares.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Substitua os **&lt;tabela&gt;** código com o código a seguir para exibir apenas os **gênero**, **artista**, **título do álbum**, e **preço** campos. Isso exclui as **AlbumId** e **URL de arte do álbum** colunas. Além disso, ele altera GenreId e ArtistId colunas para exibir suas propriedades de classe vinculados do **Artist.Name** e **Genre.Name**e remove os **detalhes** link.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Altere as descrições a seguir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5 – executar o aplicativo

Nesta tarefa, você testará que a **StoreManager** **índice** modelo de exibição exibe uma lista dos álbuns de acordo com o design das etapas anteriores.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager** para verificar se uma lista dos álbuns é exibida, mostrando seus **título**, **artista** e **gênero**.

    ![Procurar a lista de álbuns](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "procurar a lista de álbuns")

    *Procurar a lista de álbuns*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Exercício 2: Adicionando um auxiliar de HTML

A página de índice StoreManager tem um problema potencial: propriedades do título e o nome do artista podem ambos ser longo o suficiente para livrar-se a formatação de tabela. Neste exercício, você aprenderá como adicionar um auxiliar HTML personalizado para truncar o texto.

Na figura a seguir, você pode ver como o formato é modificado por causa do comprimento do texto quando você usa um tamanho pequeno de navegador.

![Procurar a lista de álbuns com texto não truncado](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "procurar a lista de álbuns com texto não truncado")

*Procurar a lista de álbuns com texto não truncado*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tarefa 1 - estendendo o auxiliar HTML

Nesta tarefa, você adicionará um novo método **Truncate** para o **HTML** objeto exposto em exibições do ASP.NET MVC. Para fazer isso, você implementará uma **método de extensão** o interna **System.Web.Mvc.HtmlHelper** classe fornecida pelo ASP.NET MVC.

> [!NOTE]
> Para ler mais sobre **métodos de extensão**, visite este artigo do msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Abra o **começar** solução localizado em **origem/o Ex2-AddingAnHTMLHelper/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra a exibição de índice do StoreManager. Para fazer isso, no Gerenciador de soluções, expanda o **modos de exibição** pasta, em seguida, a **StoreManager** e abra o **index. cshtml** arquivo.
3. Adicione o código a seguir a <strong>@model</strong> diretiva para definir a <strong>Truncate</strong> método auxiliar.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tarefa 2 - truncar o texto da página

Nesta tarefa, você aprenderá a usar o **Truncate** método truncar o texto no modelo de exibição.

1. Abra a exibição de índice do StoreManager. Para fazer isso, no Gerenciador de soluções, expanda o **modos de exibição** pasta, em seguida, a **StoreManager** e abra o **index. cshtml** arquivo.
2. Substitua as linhas que mostram a **nome do artista** e do álbum **título**. Para fazer isso, substitua as linhas a seguir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executar o aplicativo

Nesta tarefa, você testará que a **StoreManager** **índice** exibir modelo trunca o título do álbum e o nome do artista.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager** para verificar se esse tempo textos na **título** e **artista** coluna são truncados.

    ![Truncado nomes de artistas e títulos](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "truncado nomes de artistas e títulos")

    *Truncado títulos e nomes dos artistas*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Exercício 3: Criar a exibição de edição

Neste exercício, você aprenderá como criar um formulário para permitir que os gerentes de loja editar um álbum. Ele irá procurar o **/StoreManager/Edit/id** URL (**id** sendo a id exclusiva do álbum para editar), tornando uma chamada HTTP GET para o servidor.

O método de ação Editar do controlador irá recuperar o álbum apropriado do banco de dados, crie uma **StoreManagerViewModel** objeto encapsulá-la (juntamente com uma lista dos artistas e gêneros) e, em seguida, transmiti-los para um modelo de exibição para renderize a página HTML para o usuário. Essa página conterá uma **&lt;formulário&gt;** elemento com caixas de texto e listas suspensas para editar as propriedades do álbum.

Depois que o usuário atualiza os valores de formulário do álbum e clica o **salve** botão, as alterações são enviadas por meio de um POST HTTP retorno de chamada para **/StoreManager/Edit/id**. Embora a URL permanecerá o mesmo a última chamada, ASP.NET MVC identifica que desta vez, ele é um HTTP-POST e, portanto, executa um método de ação Editar diferente (um decorada com **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tarefa 1 - Implementando o método de ação de edição de HTTP-GET

Nesta tarefa, você implementará a versão HTTP GET a edição do método de ação para recuperar o álbum apropriado do banco de dados, bem como uma lista de todos os gêneros e artistas. Ele irá empacotar esses dados de para o **StoreManagerViewModel** objeto definido na última etapa, em seguida, será passada para um modelo de exibição para renderizar a resposta com.

1. Abra o **começar** solução localizado em **origem/Ex3-CreatingTheEditView/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
3. Substitua os **Editar HTTP-GET** método de ação com o código a seguir para recuperar o apropriado **álbum** , bem como a **gêneros** e **artistas**lista.

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - Ex3 StoreManagerController HTTP-GET Editar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Você está usando **System.Web.Mvc** **SelectList** para artistas e gêneros em vez da **Collections** lista.
    > 
    > **SelectList** é uma maneira mais limpa para popular menus suspensos HTML e gerenciar itens como a seleção atual. Instanciando e posterior configurar esses objetos de ViewModel na ação de controlador fará o cenário de formulário de edição mais limpo.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tarefa 2: criar a exibição de edição

Nesta tarefa, você criará um modelo de exibição de edição que mais tarde exibirá as propriedades do álbum.

1. Crie a exibição de edição. Para fazer isso, clique com botão direito dentro do **edite** método de ação e selecione **adicionar exibição**.
2. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição está **editar**. Verifique a **criar uma exibição fortemente tipada** caixa de seleção e selecione **álbum (MvcMusicStore.Models)** do **exibir dados de classe** lista suspensa. Selecione **edite** da **modelo Scaffold** lista suspensa. Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "adicionando uma exibição de edição")

    *Adicionando uma exibição de edição*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executar o aplicativo

Nesta tarefa, você testará que a **StoreManager** **editar** página de exibição exibe os valores das propriedades para o álbum passado como parâmetro.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/Edit/1** para verificar se os valores das propriedades para o álbum passados são exibidos.

    ![Navegação na exibição de edição do álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "navegação na exibição de edição do álbum")

    *Exibição de edição do álbum de navegação*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tarefa 4 - implementando menus suspensos no modelo de Editor do álbum

Nesta tarefa, você irá adicionar menus suspensos para o modelo de exibição criado na última tarefa, para que o usuário pode selecionar em uma lista dos artistas e gêneros.

1. Substitua todo o **álbum** fieldset código com o seguinte:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Uma **Html.DropDownList** auxiliar foi adicionado para renderizar menus suspensos para a escolha de artistas e gêneros. Os parâmetros passados para **Html.DropDownList** são:
    > 
    > 1. O nome do campo de formulário (**&quot;ArtistId&quot;**).
    > 2. O **SelectList** de valores para a lista suspensa.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5 – executar o aplicativo

Nesta tarefa, você testará que a **StoreManager** **editar** página de exibição exibe os menus suspensos em vez de campos de texto do artista e a ID de gênero.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/Edit/1** para verificar se ele exibe listas suspensas em vez de campos de texto do artista e a ID de gênero.

    ![Navegação Editar modo de exibição do álbum com menus suspensos](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Editar modo de exibição do álbum navegação com menus suspensos")

    *Exibição de edição do álbum, desta vez com menus suspensos de navegação*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tarefa 6 - Implementando o método de ação Editar do HTTP POST

Agora que o modo de exibição Editar exibe conforme o esperado, você precisa implementar o método de ação Editar do HTTP POST para salvar as alterações feitas no álbum.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra **StoreManagerController** da **controladores** pasta.
2. Substitua **HTTP-POST editar** código do método de ação com o seguinte (Observe que o método que deve ser substituído é a versão sobrecarregada que recebe dois parâmetros):

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - Ex3 StoreManagerController HTTP-POST Editar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Esse método será executado quando o usuário clica o **salvar** botão do modo de exibição e executa um POST HTTP dos valores de formulário de volta para o servidor para mantê-los no banco de dados. O decorador **[HttpPost]** indica que o método deve ser usado para esses cenários de HTTP POST. O método utiliza um **álbum** objeto. ASP.NET MVC criará automaticamente o objeto de álbum do postados &lt;formulário&gt; valores.
    > 
    > O método irá executar essas etapas:
    > 
    > 1. Se o modelo é válido:
    > 
    >     1. Atualize a entrada de álbum no contexto para marcá-la como um objeto modificado.
    >     2. Salve as alterações e redirecionar para o modo de exibição de índice.
    > 2. Se o modelo não é válido, ele preencherá a ViewBag com o **GenreId** e **ArtistId**, ele retornará a exibição com o objeto de álbum recebido para permitir que o usuário execute qualquer atualização necessária.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarefa 7 – executar o aplicativo

Nesta tarefa, você testará que a **StoreManager editar** página de exibição, na verdade, salva os dados atualizados do álbum no banco de dados.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/Edit/1**. Alterar o título do álbum **Load** e clique em **salvar**. Verifique se que o título do álbum realmente foram alterados na lista de álbuns.

    ![Atualizando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "atualizando um álbum")

    *Atualizando um álbum*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Exercício 4: Adicionando uma criar exibição

Agora que o **StoreManagerController** dá suporte a **editar** capacidade, neste exercício você aprenderá a adicionar um modelo de Create View para permitem armazenar gerenciadores de adicionar novos álbuns ao aplicativo.

Como você fez com a funcionalidade de edição, você implementará o cenário de criar usando dois métodos separados dentro de **StoreManagerController** classe:

1. Um método de ação exibirá um formulário vazio quando os gerentes de loja primeiramente visite o **/StoreManager/criar** URL.
2. Um segundo método de ação manipulará o cenário em que o gerente da loja clica o **salve** botão dentro do formulário e envia os valores de volta para o **/StoreManager/criar** URL como um POST HTTP.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tarefa 1 - Implementando o método de ação Criar HTTP-GET

Nesta tarefa, você implementará a versão HTTP GET do método de ação Create para recuperar uma lista de todos os gêneros e artistas, empacote esses dados em um **StoreManagerViewModel** objeto, que, em seguida, será passado para um modelo de exibição.

1. Abra o **começar** solução localizado em **origem/Ex4-AddingACreateView/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
3. Substitua os **criar** código do método de ação com o seguinte:

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - Ex4 StoreManagerController HTTP-GET Criar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tarefa 2 - adicionando o modo de exibição criar

Nesta tarefa, você irá adicionar o modelo Criar modo de exibição que será exibido um novo formulário do álbum (vazio).

1. Clique com botão direito dentro de **Create** método de ação e selecione **adicionar exibição**. Isso abrirá a caixa de diálogo Adicionar modo de exibição.
2. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição está **criar**. Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe de modelo** lista suspensa e **criar** do **modelo de Scaffold** lista suspensa. Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adicionando-a-create-view.png")

    *Adicionar modo de exibição criar*
3. Atualizar o **GenreId** e **ArtistId** campos para usar uma lista suspensa, conforme mostrado abaixo:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executar o aplicativo

Nesta tarefa, você testará que a **StoreManager** **criar** página de exibição exibe um formulário vazio do álbum.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/criar**. Verifique se um formulário vazio é exibido para preencher as novas propriedades do álbum.

    ![Criar modo de exibição com um formulário vazio](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View com um formulário vazio")

    *Criar modo de exibição com um formulário vazio*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tarefa 4 - Implementando o HTTP-POST criar método de ação

Nesta tarefa, você implementará a versão do HTTP POST do método de ação Create que será invocado quando um usuário clica o **salvar** botão. O método deve salvar o novo álbum no banco de dados.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
2. Substitua **HTTP-POST criar** código do método de ação com o seguinte:

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - Ex4 StoreManagerController HTTP - POST Criar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > A ação de criação é muito parecida com o método de ação de edição anterior, mas em vez de definir o objeto como modificado, ele está sendo adicionado ao contexto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5 – executar o aplicativo

Nesta tarefa, você testará que a **StoreManager criar** página de exibição permite que você crie um novo álbum e, em seguida, redireciona para o modo de exibição de índice StoreManager.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/criar**. Preencha todos os campos de formulário com os dados para um novo álbum, como o mostrado na figura a seguir:

    ![Criando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "criando um álbum")

    *Criando um álbum*
3. Verifique se que será redirecionado para o modo de exibição de índice StoreManager que inclui o novo álbum que acabou de criar.

    ![Novo álbum criado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "novo álbum criado")

    *Novo álbum criado*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Exercício 5: Tratamento de exclusão

A capacidade de excluir álbuns ainda não está implementada. Isso é o que este exercício será aproximadamente. Como antes, você implementará o cenário de exclusão usando dois métodos separados dentro de **StoreManagerController** classe:

1. Um método de ação exibirá um formulário de confirmação
2. Um segundo método de ação irá manipular o envio do formulário

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tarefa 1 - Implementando o método de ação de exclusão de HTTP-GET

Nesta tarefa, você implementará a versão de HTTP GET do método de ação de exclusão para recuperar informações do álbum.

1. Abra o **começar** solução localizado em **origem/Ex5-HandlingDeletion/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
3. A ação de controlador de exclusão é exatamente o mesmo que a ação de controlador Store detalhes anterior: ele consulta os **álbum** objeto de banco de dados usando o **id** fornecidos na URL e retorna o apropriado **exibição**. Para fazer isso, substitua o HTTP-GET **excluir** código do método de ação com o seguinte:

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - HTTP-GET exclusão Ex5 tratamento excluir ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Clique com botão direito dentro do **excluir** método de ação e selecione **adicionar exibição**. Isso abrirá a caixa de diálogo Adicionar modo de exibição.
5. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição está **excluir**. Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe de modelo** lista suspensa. Selecione **exclua** da **modelo Scaffold** lista suspensa. Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "adicionando uma exibição Delete")

    *Adicionando uma exibição Delete*
6. O modelo de exclusão mostra todos os campos do modelo. Você mostrará apenas o título do álbum. Para fazer isso, substitua o conteúdo do modo de exibição com o código a seguir:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2 - executando o aplicativo

Nesta tarefa, você testará que a **StoreManager** **excluir** página de exibição exibe um formulário de exclusão de confirmação.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager**. Selecionar um álbum para excluir clicando **excluir** e verificar se o novo modo de exibição é carregado.

    ![Exclusão de um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "exclusão de um álbum")

    *Exclusão de um álbum*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tarefa 3-Implementando o método de ação de exclusão de HTTP POST

Nesta tarefa, você implementará a versão do HTTP POST do método de ação de exclusão que será invocado quando um usuário clica o **excluir** botão. O método deve excluir o álbum no banco de dados.

1. Feche o navegador, se necessário, para retornar à janela do Visual Studio. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
2. Substitua **HTTP-POST excluir** código do método de ação com o seguinte:

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - HTTP-POST exclusão Ex5 tratamento excluir ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - execução do aplicativo

Nesta tarefa, você testará que a **StoreManager Delete** página de exibição permite que você exclua um álbum e, em seguida, redireciona para o modo de exibição de índice StoreManager.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager**. Selecionar um álbum para excluir clicando **excluir.** Confirmar a exclusão clicando **excluir** botão:

    ![Exclusão de um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "exclusão de um álbum")

    *Exclusão de um álbum*
3. Verifique se o álbum foi excluído, pois ele não aparece na **índice** página.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Exercício 6: Adicionando validação

Atualmente, as formas de criar e editar, que você tem em vigor não executam qualquer tipo de validação. Se o usuário deixar um campo obrigatório em branco ou tipo letras no campo de preço, o primeiro erro que você obterá será do banco de dados.

Você pode adicionar validação para o aplicativo com a adição de anotações de dados à sua classe de modelo. Anotações de dados permitem que descreve as regras que você deseja que sejam aplicadas às suas propriedades de modelo e ASP.NET MVC se encarregará de imposição e exibindo a mensagem apropriada para os usuários.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tarefa 1: adicionar anotações de dados

Nesta tarefa, você irá adicionar anotações de dados para o modelo de álbum que tornarão a página criar e editar exibir mensagens de validação quando apropriado.

Para uma classe de modelo simple, adicionar uma anotação de dados é tratada apenas adicionando um **usando** instrução para **System.ComponentModel.DataAnnotation**, em seguida, colocar um **[obrigatório]** atributo sobre as propriedades adequadas. O exemplo a seguir faria o **nome** um campo obrigatório no modo de exibição da propriedade.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Isso é um pouco mais complexo em casos como esse aplicativo em que o modelo de dados de entidade é gerado. Se você adicionar anotações de dados diretamente para as classes de modelo, eles serão substituídos se você atualizar o modelo do banco de dados. Em vez disso, você pode fazer uso de classes parciais de metadados que existirão para manter as anotações e estão associadas com o modelo de classes usando o **[MetadataType]** atributo.

1. Abra o **começar** solução localizado em **origem/Ex6-AddingValidation/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **Album.cs** da **modelos** pasta.
3. Substitua **Album.cs** de conteúdo com o código realçado, para que ele é semelhante ao seguinte:

    > [!NOTE]
    > A linha **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica que as cadeias de caracteres vazias do modelo de não ser convertidas em nulo quando o campo de dados é atualizado na fonte de dados. Essa configuração irá evitar uma exceção quando o Entity Framework atribui valores nulos para o modelo antes de anotação de dados valida os campos.

    (Código de trecho de código – *auxiliares do ASP.NET MVC 4, formulários e validação - classe parcial de metadados de álbum Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Isso **álbum** classe parcial tem um **MetadataType** atributo que aponta para o **AlbumMetaData** classe para as anotações de dados. Estes são alguns dos atributos de anotação de dados que você está usando para anotar o modelo de álbum:
    > 
    > - Exigido - indica que a propriedade é um campo obrigatório
    > - DisplayName - define o texto a ser usado em campos de formulário e mensagens de validação
    > - DisplayFormat - Especifica como os campos de dados são exibidos e formatados.
    > - StringLength - define um comprimento máximo para um campo de cadeia de caracteres
    > - Intervalo - retorna um valor mínimo e máximo para um campo numérico
    > - ScaffoldColumn - permite ocultar campos de formulários do editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2 - executando o aplicativo

Nesta tarefa, você testará que as páginas criar e editar validam campos, usando os nomes de exibição escolhidos na última tarefa.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/criar**. Verifique se os nomes de exibição correspondem aqueles na classe parcial (como **URL de arte do álbum** em vez de **AlbumArtUrl**)
3. Clique em **criar**, sem preenchendo o formulário. Verifique se que você obtenha as mensagens de validação correspondentes.

    ![Validar campos na página criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validado campos na página criar")

    *Campos validados na página criar*
4. Você pode verificar se o mesmo ocorre com o **editar** página. Altere a URL para **/StoreManager/Edit/1** e verifique se os nomes de exibição correspondem aqueles na classe parcial (como **URL de arte do álbum** em vez de **AlbumArtUrl**). Vazio a **Title** e **preço** campos e clique em **salvar**. Verifique se que você obtenha as mensagens de validação correspondentes.

    ![Campos validados na página Editar](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Campos validados na página Editar*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Exercício 7: Usando o jQuery Unobtrusive no lado do cliente

Neste exercício, você aprenderá como habilitar a validação não invasiva MVC 4 do jQuery no lado do cliente.

> [!NOTE]
> O jQuery Unobtrusive usa o prefixo de dados ajax JavaScript para invocar métodos de ação no servidor em vez de scripts de cliente forma invasiva emitindo embutido.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tarefa 1: executar o aplicativo antes de habilitar discreto jQuery

Nesta tarefa, você executará o aplicativo antes de incluir jQuery para comparar os dois modelos de validação.

1. Abra o **começar** solução localizado em **origem/Ex7-UnobtrusivejQueryValidation/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. Pressione **F5** para executar o aplicativo.
3. O projeto é iniciado na Home page. Navegue **/StoreManager/Create** e clique em **criar** sem preenchendo o formulário para verificar que você obtenha as mensagens de validação:

    ![Validação de cliente desabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "desabilitada a validação do cliente")

    *Validação de cliente desabilitada*
4. No navegador, abra o código-fonte HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tarefa 2 - Habilitar validação de cliente discreto

Nesta tarefa, você habilitará o jQuery **validação do cliente não invasiva** de **Web. config** arquivo, que é definido como false em todos os novos projetos do ASP.NET MVC 4 por padrão. Além disso, você adicionará que o necessário scripts referências para tornar o trabalho de validação não invasiva do cliente do jQuery.

1. Abra **Web. config** do arquivo na raiz do projeto e certifique-se de que o **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** valores de chaves são definidos como **verdadeiro**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Você também pode habilitar a validação do cliente pelo código em Global.asax.cs para obter os mesmos resultados:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Além disso, você pode atribuir o atributo ClientValidationEnabled em qualquer controlador de ter um comportamento personalizado.
2. Abra **Create. cshtml** na **Views\StoreManager**.
3. Verifique se os seguintes arquivos de script, **jQuery. Validate** e **jquery.validate.unobtrusive**, são referenciados em através de modo de exibição de &quot; **~/bundles/jqueryval** &quot; pacote.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Todas essas bibliotecas de jQuery são incluídas em novos projetos de MVC 4. Você pode encontrar mais bibliotecas na **/Scripts** pasta do projeto.
    > 
    > Para fazer essa validação bibliotecas funcionam, você precisará adicionar uma referência à biblioteca jQuery do framework. Uma vez que essa referência já foi adicionada a  **\_layout. cshtml** arquivo, você não precisará adicioná-lo neste modo de exibição específico.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tarefa 3: executar a validação do jQuery Unobtrusive de aplicativo usando

Nesta tarefa, você testará que a **StoreManager** criar exibição de modelo executa a validação do lado do cliente usando bibliotecas de jQuery quando o usuário cria um novo álbum.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Navegue **/StoreManager/Create** e clique em **criar** sem preenchendo o formulário para verificar que você obtenha as mensagens de validação:

    ![Validação do cliente com o jQuery habilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validação do cliente com o jQuery habilitado")

    *Validação do cliente com o jQuery habilitado*
3. No navegador, abra o código-fonte para criar exibição:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Para cada regra de validação do cliente, o jQuery Unobtrusive adiciona um atributo com os dados-- val*rulename*=&quot;*mensagem*&quot;. Abaixo está uma lista de marcas que Unobtrusive jQuery insere no campo de entrada html para executar a validação do cliente:
   > 
   > - Dados val
   > - Número de dados val
   > - Intervalo de dados val
   > - Data-val-intervalo-min / dados val-intervalo máximo
   > - Dados val necessários
   > - Comprimento de dados val
   > - Data-val comprimento máx / dados val-comprimento mínimo
   > 
   > Todos os valores de dados são preenchidos com o modelo **anotação de dados**. Em seguida, toda a lógica que funciona no lado do servidor pode ser executada no lado do cliente. Por exemplo, o atributo de preço tem a anotação de dados a seguir no modelo:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Depois de usar jQuery discreto, o código gerado é:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu como habilitar usuários para alterar os dados armazenados no banco de dados com o uso das seguintes opções:

- Ações do controlador, como o índice, criar, editar, excluir
- Recurso de scaffolding de ASP.NET MVC para exibir as propriedades em uma tabela HTML
- Experiência de auxiliares HTML personalizados para melhorar o usuário
- Métodos de ação que reagem a HTTP-GET ou chamadas de HTTP POST
- Um modelo compartilhado editor de modelos de exibição semelhantes, como criar e editar
- Elementos de formulário, como listas suspensas
- Anotações de dados para a validação do modelo
- Validação do lado do cliente usando a biblioteca não invasiva do jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalação do Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalar o Visual Studio Express")

    *Instalar o Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação for concluído.

    ![Progresso da instalação](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalação concluída*
7. Clique em **Exit** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco da Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express para o bloco da Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice b: usar trechos de código

Com trechos de código, você tem todo o código que necessário ao seu alcance. O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usar trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "trechos de código usando o Visual Studio para inserir código em seu projeto")

*Usar trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (somente c#)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho de código (sem espaços ou hifens).
3. Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho de código serão expandido](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "pressione Tab novamente e o trecho de código serão expandido")

*Pressione Tab novamente e o trecho de código serão expandido*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito no qual você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")

*Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet*

![Escolher o trecho relevante na lista, clicando nele](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "escolher o trecho relevante na lista, clicando nele")

*Escolher o trecho relevante na lista, clicando nele*
