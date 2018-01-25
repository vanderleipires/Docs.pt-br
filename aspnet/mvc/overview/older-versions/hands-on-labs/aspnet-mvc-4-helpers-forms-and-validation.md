---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "Auxiliares do ASP.NET MVC 4, formulários e validação | Microsoft Docs"
author: rick-anderson
description: "No ASP.NET MVC 4 modelos e dados de laboratório prático de acesso, você foi carregar e exibir dados do banco de dados. Neste laboratório prático, você adicionará à..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Validação, formulários e auxiliares do ASP.NET MVC 4
====================
por [Web Camps Team](https://twitter.com/webcamps)

> Em **acesso a dados e modelos do ASP.NET MVC 4** laboratório prático, você esteve carregar e exibir dados do banco de dados. Neste laboratório prático, você adicionará ao **repositório de música** aplicativo a capacidade de editar esses dados.
> 
> Com esse objetivo em mente, você criará primeiro o controlador que dará suporte as ações de criação, leitura, atualização e exclusão (CRUD) de álbuns. Você irá gerar um modelo de exibição de índice que aproveita o recurso de scaffolding de ASP.NET MVC para exibir as propriedades dos álbuns em uma tabela HTML. Para aprimorar a exibição, você adicionará um auxiliar HTML personalizado que truncará descrições longo.
> 
> Posteriormente, você adicionará a editar e criar exibições que permitem que você alteram álbuns no banco de dados, com a Ajuda de elementos de formulário, como listas suspensas.
> 
> Por fim, você permitirá que os usuários excluir um álbum e também você impedirá de inserção de dados errados, validando a entrada.
> 
> > [!NOTE]
> > Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC**. Se você não usou **ASP.NET MVC** antes, é recomendável que você passe **conceitos básicos do ASP.NET MVC** laboratório prático.
> 
> 
> Este laboratório orienta os aperfeiçoamentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo da Web de exemplo fornecido na pasta de origem.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Criar um controlador para dar suporte a operações CRUD
- Gerar uma exibição de índice para exibir as propriedades de entidade em uma tabela HTML
- Adicionar um auxiliar HTML personalizado
- Criar e personalizar uma exibição editar
- Diferenciar entre os métodos de ação reagem a HTTP GET ou chamadas HTTP POST
- Adicionar e personalizar uma criar modo de exibição
- Lidar com a exclusão de uma entidade
- Validar a entrada do usuário

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

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice b:](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Os exercícios a seguir compõem esse laboratório prático:

1. [Criando o controlador do Gerenciador de armazenamento e a exibição do índice](#Exercise1)
2. [Adicionando um auxiliar de HTML](#Exercise2)
3. [Criar a exibição de edição](#Exercise3)
4. [Adicionar um modo de criação](#Exercise4)
5. [Tratamento de exclusão](#Exercise5)
6. [Adicionando uma Validação](#Exercise6)
7. [Usando jQuery discreto no lado do cliente](#Exercise7)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Exercício 1: Criando o controlador do Gerenciador de armazenamento e a exibição do índice

Neste exercício, você aprenderá como criar um novo controlador para dar suporte a operações CRUD, personalizar seu método de ação de índice para retornar uma lista de álbuns de banco de dados e, finalmente, gerar um modelo de exibição de índice que aproveitam a estrutura de ASP.NET MVC recurso para exibir as propriedades dos álbuns em uma tabela HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tarefa 1: Criando o StoreManagerController

Nesta tarefa, você criará um novo controlador chamado **StoreManagerController** para dar suporte a operações CRUD.

1. Abra o **começar** solução localizado em **fonte/Ex1-CreatingTheStoreManagerController/Begin/** pasta.

    1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Adicione um novo controlador. Para fazer isso, clique com botão direito do **controladores** pasta no Gerenciador de soluções, selecione **adicionar** e, em seguida, o **controlador** comando. Alterar o **controlador** **nome** para **StoreManagerController** e verifique se a opção **controlador MVC com ações de leitura/gravação vazias**está selecionado. Clique em **Adicionar**.

    ![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "caixa de diálogo Adicionar controlador")

    *Adicionar caixa de diálogo do controlador*

    Uma nova classe de controlador é gerada. Como indicado para adicionar ações de leitura/gravação, métodos de stub para aqueles, ações CRUD comuns são criadas com comentários TODO preenchidos, solicitando para incluir a lógica específica do aplicativo.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tarefa 2: personalizar o índice StoreManager

Nesta tarefa, você personalizará o método de ação do índice StoreManager para retornar uma exibição com a lista de álbuns do banco de dados.

1. Na classe StoreManagerController, adicione o seguinte *usando* diretivas.

    (Código de trecho - *validação - Ex1, formulários e auxiliares do ASP.NET MVC 4 usando MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Adicione um campo para o **StoreManagerController** para manter uma instância do **MusicStoreEntities.**

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implemente a ação de índice StoreManagerController para retornar uma exibição com a lista de álbuns.

    A lógica de ação do controlador será muito semelhante à ação de índice do StoreController gravada anteriormente. Use o LINQ para recuperar todos os álbuns, inclusive informações de gênero e artista para exibição.

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex1 StoreManagerController índice*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tarefa 3: criar a exibição do índice

Nesta tarefa, você irá criar o modelo de exibição do índice para exibir a lista de álbuns retornado pelo **StoreManager** controlador.

1. Antes de criar o novo modelo de exibição, você deve compilar o projeto para que o **caixa de diálogo Adicionar modo de exibição** conhece o **álbum** classe a ser usada. Selecione **compilar | Criar MvcMusicStore** para compilar o projeto.
2. Clique dentro do **índice** método de ação e selecione **adicionar exibição**. Isso abrirá o **adicionar exibição** caixa de diálogo.

    ![Adicionar exibição](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "adicionar modo de exibição")

    *Adicionando uma exibição de dentro do método de índice*
3. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **índice**. Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe modelo** lista suspensa. Selecione **lista** do **modelo Scaffold** lista suspensa. Deixe o **mecanismo de exibição** para **Razor** e os outros campos com padrão de valor e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "adicionando uma exibição de índice")

    *Adicionando uma exibição de índice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tarefa 4 - personalizando scaffold da exibição de índice

Nesta tarefa, você ajustará o modelo de exibição simple criado com o recurso de estrutura ASP.NET MVC para que ele exiba os campos desejados.

> [!NOTE]
> O **scaffolding** suporte dentro de ASP.NET MVC gera um modelo de exibição simple que lista todos os campos no modelo álbum. **Estrutura** fornece uma maneira rápida para começar em uma exibição fortemente tipada: em vez de gravar o modelo de exibição manualmente, estrutura rapidamente gera um modelo padrão e, em seguida, você pode modificar o código gerado.


1. Examine o código criado. Lista de campos gerada fará parte dos seguintes HTML de tabela que **Scaffolding** está usando para exibir dados tabulares.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Substitua o  **&lt;tabela&gt;**  código com o código a seguir para exibir apenas o **gênero**, **artista**, **álbum**, e **preço** campos. Isso exclui o **AlbumId** e **álbum arte URL** colunas. Além disso, ele altera GenreId e ArtistId colunas para exibir suas propriedades de classe vinculados de **Artist.Name** e **Genre.Name**e remove o **detalhes** link.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Altere as descrições a seguir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager** **índice** modelo de exibição exibe uma lista de álbuns de acordo com o design das etapas anteriores.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager** para verificar se uma lista de álbuns é exibida, mostrando seus **título**, **artista** e **gênero**.

    ![Navegando pela lista de álbuns](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "navegando pela lista de álbuns")

    *Navegando pela lista de álbuns*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Exercício 2: Adicionando um auxiliar de HTML

A página de índice StoreManager tem um problema potencial: propriedades do título e nome de artista podem ambos ser grande o suficiente para livrar-se a formatação de tabela. Neste exercício, você aprenderá como adicionar um auxiliar HTML personalizado para truncar o texto.

Na figura a seguir, você pode ver como o formato é modificado devido ao tamanho do texto quando você usa um tamanho pequeno de navegador.

![Navegando pela lista de álbuns com texto truncado não](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "navegando pela lista de álbuns com texto não truncado")

*Navegando pela lista de álbuns com texto não truncado*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tarefa 1 - estendendo o auxiliar HTML

Nesta tarefa, você adicionará um novo método **Truncate** para o **HTML** objeto exposto em modos de exibição do ASP.NET MVC. Para fazer isso, você irá implementar um **método de extensão** o interna **System.Web.Mvc.HtmlHelper** classe fornecida pelo ASP.NET MVC.

> [!NOTE]
> Para saber mais sobre **métodos de extensão**, visite este artigo do msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Abra o **começar** solução localizado em **fonte/o Ex2-AddingAnHTMLHelper/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o modo de exibição do StoreManager índice. Para fazer isso, no Gerenciador de soluções, expanda o **exibições** pasta, em seguida, o **StoreManager** e abra o **cshtml** arquivo.
3. Adicione o seguinte código abaixo o  **@model**  diretiva para definir o **Truncate** método auxiliar.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tarefa 2 - truncando texto na página

Nesta tarefa, você usará o **Truncate** método truncar o texto no modelo de exibição.

1. Abra o modo de exibição do StoreManager índice. Para fazer isso, no Gerenciador de soluções, expanda o **exibições** pasta, em seguida, o **StoreManager** e abra o **cshtml** arquivo.
2. Substitua as linhas que mostram o **nome artista** e do álbum **título**. Para fazer isso, substitua as linhas a seguir.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager** **índice** exibir modelo trunca o título e o nome do artista do álbum.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager** para verificar essa longa textos no **título** e **artista** coluna são truncados.

    ![Truncado nomes títulos e artistas](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "truncado artistas e títulos de nomes")

    *Títulos truncados e nomes de artista*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Exercício 3: Criar a exibição de edição

Neste exercício, você aprenderá como criar um formulário para permitir que os gerentes de loja editar um álbum. Eles procurará o **/StoreManager/Edit/id** URL (**id** sendo a id exclusiva do álbum para editar), tornando uma chamada HTTP GET para o servidor.

O método de ação do controlador Editar irá recuperar o álbum apropriado do banco de dados, crie um **StoreManagerViewModel** objeto para encapsular o (junto com uma lista de artistas e gêneros) e passá-lo de um modelo de exibição para renderizar a página HTML para o usuário. Esta página conterá uma  **&lt;formulário&gt;**  elemento com caixas de texto e listas suspensas para editar as propriedades de álbum.

Depois que o usuário atualiza os valores de formulário álbum e clica o **salvar** botão, as alterações são enviadas por meio de um POST HTTP de retorno de chamada para **/StoreManager/Edit/id**. Embora a URL permanece o mesmo que a última chamada, ASP.NET MVC identifica que esse tempo é um HTTP POST e, portanto, executa um método de ação de edição diferente (uma decorado com **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tarefa 1 - Implementando o método de ação de edição de HTTP GET

Nesta tarefa, você implementar a versão de HTTP GET do método de ação de edição para recuperar o álbum apropriado do banco de dados, bem como uma lista de todos os gêneros e artistas. Ele será empacotar esses dados para o **StoreManagerViewModel** objeto definido na última etapa, que, em seguida, será passada para um modelo de exibição para renderizar a resposta com.

1. Abra o **começar** solução localizado em **fonte/Ex3-CreatingTheEditView/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
3. Substitua o **HTTP GET editar** método de ação com o código a seguir para recuperar as **álbum** , bem como a **gêneros** e **artistas**lista.

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex3 StoreManagerController HTTP GET Editar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Você está usando **System.Web.Mvc** **SelectList** para artistas e gêneros em vez do **Generic** lista.
    > 
    > **SelectList** é uma maneira de limpeza para popular menus suspensos HTML e gerenciar itens como a seleção atual. Criando e a configuração mais recente desses objetos ViewModel na ação do controlador fará o cenário de formulário Editar limpeza.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tarefa 2: criar o modo de exibição de edição

Nesta tarefa, você criará um modelo de exibição editar posteriormente exibirá as propriedades de álbum.

1. Crie a exibição de edição. Para fazer isso, clique dentro do **editar** método de ação e selecione **adicionar exibição**.
2. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **editar**. Verifique o **criar uma exibição fortemente tipada** caixa de seleção e selecione **álbum (MvcMusicStore.Models)** do **exibir dados de classe** lista suspensa. Selecione **editar** do **modelo Scaffold** lista suspensa. Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "adicionando uma exibição de edição")

    *Adicionando uma exibição de edição*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager** **editar** página de exibição exibe valores das propriedades para o álbum passado como parâmetro.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/Edit/1** para verificar se os valores das propriedades para o álbum passados são exibidos.

    ![Pesquisa no modo de exibição de edição do álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Editar exibição do álbum de navegação")

    *Exibição de edição do álbum de navegação*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tarefa 4 - implementar listas suspensas no modelo álbum Editor

Nesta tarefa, você irá adicionar listas suspensas para o modelo de exibição criado na última tarefa, para que o usuário pode selecionar em uma lista de artistas e gêneros.

1. Substituir todos os **álbum** fieldset código com o seguinte:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Um **Html.DropDownList** auxiliar foi adicionado ao renderizar listas suspensas para escolher artistas e gêneros. Os parâmetros passados para **Html.DropDownList** são:
    > 
    > 1. O nome do campo de formulário (**&quot;ArtistId&quot;**).
    > 2. O **SelectList** de valores na lista suspensa.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager** **editar** página de exibição exibe listas suspensas, em vez de campos de texto artista e ID de gênero.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/Edit/1** para verificar se ele exibe listas suspensas, em vez de campos de texto artista e ID de gênero.

    ![Navegação Editar exibição do álbum com menus suspensos](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Editar exibição da navegação álbum com menus suspensos")

    *Exibição de edição do álbum, dessa vez com menus suspensos de navegação*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tarefa 6: implementar o método de ação HTTP POST editar

Agora que a editar exibe conforme o esperado, você precisa implementar o método HTTP POST Editar ação para salvar as alterações feitas ao álbum.

1. Feche o navegador se necessário, para retornar à janela do Visual Studio. Abra **StoreManagerController** do **controladores** pasta.
2. Substituir **HTTP POST editar** código do método de ação com os seguintes (Observe que o método que deve ser substituído versão sobrecarregada que recebe dois parâmetros):

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex3 StoreManagerController HTTP POST Editar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Esse método será executado quando o usuário clica o **salvar** botão do modo de exibição e executa um POST HTTP dos valores de formulário para o servidor para mantê-las no banco de dados. O decorador **[HttpPost]** indica que o método deve ser usado para os cenários de HTTP POST. O método usa um **álbum** objeto. ASP.NET MVC criará automaticamente o objeto álbum do postados &lt;formulário&gt; valores.
    > 
    > O método executará estas etapas:
    > 
    > 1. Se o modelo é válido:
    > 
    >     1. Atualize a entrada de álbum no contexto para marcá-la como um objeto modificado.
    >     2. Salvar as alterações e redirecionar para o modo de exibição do índice.
    > 2. Se o modelo não é válido, ele preencherá ViewBag com o **GenreId** e **ArtistId**, ele retornará o modo de exibição com o objeto álbum recebido para permitir que o usuário execute qualquer atualização necessária.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarefa 7: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager editar** página de exibição de salvar os dados atualizados do álbum no banco de dados.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager/Edit/1**. Altere o título do álbum para **carga** e clique em **salvar**. Verifique se o título do álbum realmente alterado na lista de álbuns.

    ![Atualizando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "atualizando um álbum")

    *Atualizando um álbum*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Exercício 4: Adicionar um modo de criação

Agora que o **StoreManagerController** oferece suporte a **editar** capacidade neste exercício, você aprenderá a adicionar um modelo de Create View para permitem armazenar gerenciadores de adicionar novos álbuns ao aplicativo.

Como você fez com a funcionalidade de edição, implementará o cenário de criar usando dois métodos separados dentro de **StoreManagerController** classe:

1. Um método de ação exibirá um formulário vazio quando gerenciadores de armazenamento primeiro visite o **/StoreManager/criar** URL.
2. Um segundo método de ação tratará o cenário em que o Gerenciador de armazenamento clica o **salvar** botão no formulário e envia os valores de volta para o **/StoreManager/criar** URL como um POST HTTP.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tarefa 1 - Implementando o método de ação Criar HTTP GET

Nesta tarefa, você irá implementar a versão de HTTP GET do método de ação de criação para recuperar uma lista de todos os gêneros e artistas, empacote esses dados em um **StoreManagerViewModel** objeto, que, em seguida, passará para um modelo de exibição.

1. Abra o **começar** solução localizado em **fonte/Ex4-AddingACreateView/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
3. Substitua o **criar** código do método de ação com o seguinte:

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex4 StoreManagerController HTTP GET Criar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tarefa 2: adicionando Create View

Nesta tarefa, você adicionará o modelo Criar modo de exibição que exibirá um novo formulário álbum (vazio).

1. Clique dentro do **criar** método de ação e selecione **adicionar exibição**. Isso abrirá a caixa de diálogo Adicionar modo de exibição.
2. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **criar**. Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe modelo** suspenso e **criar** do **modelo Scaffold** lista suspensa. Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.

    ![Adicionar um modo de criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adicionando-a-criar-view.png")

    *Adicionando o Create View*
3. Atualização de **GenreId** e **ArtistId** campos para usar uma lista suspensa, conforme mostrado abaixo:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager** **criar** página de exibição exibe um formulário álbum vazio.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **StoreManager/criar**. Verifique se um formulário vazio é exibido para preencher as novas propriedades de álbum.

    ![Criar modo de exibição com um formulário vazio](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View com um formulário vazio")

    *Criar modo de exibição com um formulário vazio*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tarefa 4 - implementar o HTTP POST criar método de ação

Nesta tarefa, você irá implementar a versão HTTP POST do método de ação de criação que será chamado quando um usuário clica o **salvar** botão. O método deve salvar o novo álbum no banco de dados.

1. Feche o navegador se necessário, para retornar à janela do Visual Studio. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
2. Substituir **HTTP POST criar** código do método de ação com o seguinte:

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex4 StoreManagerController HTTP POST Criar ação*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Ação de criação é muito semelhante ao método de ação de edição anterior, mas em vez de configurar o objeto como modificado, ele está sendo adicionado ao contexto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager criar** página de exibição permite que você crie um novo álbum e, em seguida, redireciona para a exibição do índice StoreManager.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **StoreManager/criar**. Preencha todos os campos de formulário com dados para um novo álbum, como a mostrada na figura a seguir:

    ![Criando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "criando um álbum")

    *Criando um álbum*
3. Verifique se que você é redirecionado para a exibição do índice StoreManager que inclui o novo álbum que acabou de criar.

    ![Novo álbum criado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "novo álbum criado")

    *Novo álbum criado*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Exercício 5: Exclusão de tratamento

A capacidade de excluir álbuns ainda não foi implementada. Isso é o que este exercício sobre. Como antes, você implementar o cenário de exclusão usando dois métodos separados dentro de **StoreManagerController** classe:

1. Um método de ação exibirá um formulário de confirmação
2. Um segundo método de ação tratará o envio do formulário

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tarefa 1 - Implementando o método de ação de exclusão de HTTP GET

Nesta tarefa, você implementar a versão de HTTP GET do método de ação de exclusão para recuperar informações do álbum.

1. Abra o **começar** solução localizado em **fonte/Ex5-HandlingDeletion/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
3. A ação de controlador de exclusão é exatamente o mesmo que a ação de controlador de detalhes da loja anterior: ele consulta o **álbum** o objeto de banco de dados usando o **id** fornecidos na URL e retorna o apropriado **exibição**. Para fazer isso, substitua o HTTP GET **excluir** código do método de ação com o seguinte:

    (Código de trecho - *validação - ação HTTP GET exclusão Ex5 tratamento Delete, formulários e auxiliares do ASP.NET MVC 4*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Clique dentro do **excluir** método de ação e selecione **adicionar exibição**. Isso abrirá a caixa de diálogo Adicionar modo de exibição.
5. Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **excluir**. Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe modelo** lista suspensa. Selecione **excluir** do **modelo Scaffold** lista suspensa. Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.

    ![Adicionando uma exibição de exclusão](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "adicionando uma exibição de exclusão")

    *Adicionando uma exibição de exclusão*
6. O modelo de exclusão mostra todos os campos do modelo. Você mostrará apenas o título do álbum. Para fazer isso, substitua o conteúdo do modo de exibição com o código a seguir:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2: executando o aplicativo

Nesta tarefa, você testará que o **StoreManager** **excluir** página de exibição exibe um formulário de exclusão de confirmação.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager**. Selecione um álbum excluir clicando **excluir** e verifique se o novo modo de exibição é carregado.

    ![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "excluindo um álbum")

    *Excluir um álbum*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tarefa 3-implementar o método de ação de exclusão de HTTP POST

Nesta tarefa, você irá implementar a versão HTTP POST do método de ação de exclusão que será chamado quando um usuário clica o **excluir** botão. O método deve excluir o álbum no banco de dados.

1. Feche o navegador se necessário, para retornar à janela do Visual Studio. Abra **StoreManagerController** classe. Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.
2. Substituir **HTTP POST excluir** código do método de ação com o seguinte:

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - ação HTTP POST exclusão Ex5 tratamento excluir*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - executando o aplicativo

Nesta tarefa, você testará que o **StoreManager excluir** página de exibição permite que você exclua um álbum e, em seguida, redireciona para a exibição do índice StoreManager.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/StoreManager**. Selecione um álbum excluir clicando **excluir.** Confirme a exclusão clicando **excluir** botão:

    ![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "excluindo um álbum")

    *Excluir um álbum*
3. Verifique se o álbum foi excluído porque ele não aparece no **índice** página.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Exercício 6: Adicionando validação

Atualmente, as formas de criar e editar, que você tem em vigor não executar qualquer tipo de validação. Se o usuário deixar um campo obrigatório em branco ou letras de tipo no campo preço, o primeiro erro que será exibida será do banco de dados.

Você pode adicionar validação para o aplicativo com a adição de anotações de dados à sua classe de modelo. As anotações de dados permitem que descreve as regras a que serem aplicadas às suas propriedades de modelo e ASP.NET MVC cuidará de imposição e exibindo a mensagem apropriada a usuários.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tarefa 1: adicionar anotações de dados

Nesta tarefa, você irá adicionar anotações de dados para o modelo de álbum que fará com que a página criar e editar exibir mensagens de validação quando apropriado.

Para uma classe simple do modelo, adicionar uma anotação de dados é tratado apenas adicionando uma **usando** instrução para **System.ComponentModel.DataAnnotation**, em seguida, colocar um **[obrigatório]**atributo as propriedades adequadas. O exemplo a seguir faria o **nome** um campo obrigatório no modo de exibição da propriedade.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Isso é um pouco mais complexo em casos como esse aplicativo onde o modelo de dados de entidade é gerado. Se você adicionou anotações de dados diretamente para as classes de modelo, ele seriam substituídos se você atualizar o modelo do banco de dados. Em vez disso, você pode fazer uso de classes parciais de metadados que exista para manter as anotações e estão associadas com o modelo de classes usando o **[MetadataType]** atributo.

1. Abra o **começar** solução localizado em **fonte/Ex6-AddingValidation/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **Album.cs** do **modelos** pasta.
3. Substituir **Album.cs** de conteúdo com o código realçado, para que ele se parece com o seguinte:

    > [!NOTE]
    > A linha **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica que as cadeias de caracteres vazias do modelo não ser convertidas em nulo quando o campo de dados é atualizado na fonte de dados. Essa configuração evitará uma exceção quando o Entity Framework atribui valores nulos para o modelo antes de anotação de dados valida os campos.

    (Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - classe parcial de metadados de álbum Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Isso **álbum** classe parcial tem um **MetadataType** atributo que aponta para o **AlbumMetaData** classe para as anotações de dados. Estes são alguns dos atributos de anotação de dados que você está usando para anotar o modelo álbum:
    > 
    > - Exigido - indica que a propriedade é um campo obrigatório
    > - DisplayName - define o texto a ser usada em campos de formulário e mensagens de validação
    > - DisplayFormat - Especifica como os campos de dados são exibidos e formatados.
    > - StringLength - define um comprimento máximo de um campo de cadeia de caracteres
    > - Intervalo - retorna um valor mínimo e máximo para um campo numérico
    > - ScaffoldColumn - permite ocultar campos de formulários do editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2: executando o aplicativo

Nesta tarefa, você testará a criar e editar páginas validam campos, usando os nomes de exibição escolhidos na última tarefa.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **StoreManager/criar**. Verificar se os nomes de exibição coincidem com aqueles na classe parcial (como **álbum arte URL** em vez de **AlbumArtUrl**)
3. Clique em **criar**, sem preencher o formulário. Verifique se que você obtém as mensagens de validação correspondente.

    ![Validar os campos na página criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validado campos na página de criação")

    *Validado campos na página de criação*
4. Você pode verificar se o mesmo ocorre com o **editar** página. Altere a URL para **/StoreManager/Edit/1** e verificar se os nomes de exibição coincidem com aqueles na classe parcial (como **álbum arte URL** em vez de **AlbumArtUrl**). Vazio a **título** e **preço** campos e clique em **salvar**. Verifique se que você obtém as mensagens de validação correspondente.

    ![Validado campos na página de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Validado campos na página de edição*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Exercício 7: Usando jQuery discreto no lado do cliente

Neste exercício, você aprenderá como habilitar a validação jQuery MVC 4 discreto no lado do cliente.

> [!NOTE]
> O jQuery discreto usa prefixo dados ajax JavaScript para invocar métodos de ação no servidor em vez de scripts de cliente forma intrusiva emissão embutido.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tarefa 1: executar o aplicativo antes de habilitar discreto jQuery

Nesta tarefa, você executará o aplicativo antes de incluir jQuery para comparar os dois modelos de validação.

1. Abra o **começar** solução localizado em **fonte/Ex7-UnobtrusivejQueryValidation/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Pressione **F5** para executar o aplicativo.
3. O projeto é iniciado na Home page. Procurar **/StoreManager/criar** e clique em **criar** sem preencher o formulário para verificar se você receber mensagens de validação:

    ![Validação de cliente desabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "desabilitada a validação do cliente")

    *Validação de cliente desabilitada*
4. No navegador, abra o código-fonte HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tarefa 2 - Habilitar validação de cliente discreto

Nesta tarefa, você habilitará o jQuery **validação do cliente discreto** de **Web. config** arquivo, que é definido como false em todos os novos projetos do ASP.NET MVC 4 por padrão. Além disso, você irá adicionar que referências para tornar jQuery trabalho discreto validação do cliente de scripts necessários.

1. Abra **Web. config** arquivo na raiz do projeto e certifique-se de que o **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** valores de chaves são definidos como **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Você também pode habilitar a validação do cliente pelo código em Global.asax.cs para obter os mesmos resultados:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Além disso, você pode atribuir o atributo ClientValidationEnabled em qualquer controlador de ter um comportamento personalizado.
2. Abra **Create.cshtml** em **Views\StoreManager**.
3. Verifique se os seguintes arquivos de script, **jquery.validate** e **jquery.validate.unobtrusive**, são referenciados na através do modo de exibição de &quot; **~/bundles/jqueryval** &quot; pacote.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Todas essas bibliotecas jQuery são incluídas em novos projetos do MVC 4. Você pode encontrar mais bibliotecas no **/Scripts** pasta do projeto.
    > 
    > Para fazer essa validação bibliotecas de trabalho, você precisa adicionar uma referência à biblioteca do framework jQuery. Desde que esta referência já foi adicionada a  **\_cshtml** arquivo, não é necessário adicioná-lo neste modo de exibição específico.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tarefa 3: executando o aplicativo usando discreto jQuery validação

Nesta tarefa, você testará que o **StoreManager** criar exibição de modelo executa a validação do lado do cliente usando bibliotecas jQuery quando o usuário cria um novo álbum.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Procurar **/StoreManager/criar** e clique em **criar** sem preencher o formulário para verificar se você receber mensagens de validação:

    ![Validação do cliente com o jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validação do cliente com o jQuery habilitado")

    *Validação do cliente com o jQuery habilitado*
3. No navegador, abra o código-fonte para criar modo de exibição:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > Para cada regra de validação de cliente jQuery discreto adiciona um atributo com dados-val -*rulename*=&quot;*mensagem*&quot;. Abaixo está uma lista de marcas que Unobtrusive jQuery insere o campo de entrada html para executar a validação do cliente:
    > 
    > - Valor de dados
    > - Número de valor de dados
    > - Intervalo de valor de dados
    > - Data-val-intervalo-min / dados val-intervalo máximo
    > - Dados val necessários
    > - Data-val-length
    > - Data-val-tamanho-max / dados val-comprimento mínimo
    > 
    > Todos os valores de dados são preenchidos com modelo **anotação de dados**. Em seguida, toda a lógica que funciona no lado do servidor pode ser executada no lado do cliente. Por exemplo, o atributo preço tem a anotação de dados a seguir no modelo:
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

Ao concluir este laboratório prático, você aprendeu como habilitar usuários alterar os dados armazenados no banco de dados com o uso dos seguintes:

- Ações do controlador como índice, criar, editar, excluir
- Recurso de scaffolding de ASP.NET MVC para exibir as propriedades em uma tabela HTML
- Experiência de auxiliares HTML personalizados para melhorar o usuário
- Métodos de ação reagem a HTTP GET ou chamadas HTTP POST
- Um modelo do editor de compartilhado para modelos de exibição semelhantes de como criar e editar
- Elementos de formulário, como listas suspensas
- Anotações de dados para a validação de modelo
- Validação do lado do cliente usando a biblioteca discreto jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express para o bloco de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice b: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
