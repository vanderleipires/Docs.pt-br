---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Estrutura do ASP.NET MVC 4 Entity Framework e migrações | Microsoft Docs
author: rick-anderson
description: Se você estiver familiarizado com os métodos de controlador do ASP.NET MVC 4 ou concluiu a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve estar ciente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 548afe1926eed49841251832d54dc213da0cb753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>As migrações e estrutura do ASP.NET MVC 4 Entity Framework

Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](https://aka.ms/webcamps-training-kit)

Se você estiver familiarizado com os métodos de controlador do ASP.NET MVC 4 ou concluiu a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve estar ciente de que muitos a lógica para criar, atualizar, lista e remover qualquer entidade de dados é repetido entre o aplicativo. Para não mencionar que, se seu modelo tiver várias classes para manipular, será provável gastam um tempo considerável gravar os métodos de ação POST e GET para cada operação de entidade, bem como cada um dos modos de exibição.

Neste laboratório, você aprenderá como usar o ASP.NET MVC 4 de scaffolding para gerar automaticamente a linha de base de CRUD seu aplicativo (criar, ler, atualizar e excluir). A partir de uma classe de modelo simples e, sem escrever uma única linha de código, você criará um controlador que contém todas as operações CRUD, bem como as todas as necessárias exibições. Após criar e executar a solução mais simples, você terá o banco de dados de aplicativo gerado, juntamente com a lógica MVC e modos de exibição para a manipulação de dados.

Além disso, você aprenderá como é fácil usar o Entity Framework migrações para executar atualizações de modelo em todo o aplicativo. Migrações do Entity Framework permitem que você modificar o banco de dados depois que o modelo foi alterado com etapas simples. Com todos esses em mente, você poderá criar e manter os aplicativos web com mais eficiência, aproveitando os recursos mais recentes do ASP.NET MVC 4.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível no site da [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [migrações e ASP.NET MVC 4 Entity Framework Scaffolding](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Use ASP.NET scaffolding para operações CRUD de controladores.
- Altere o modelo de banco de dados usando o Entity Framework migrações.

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

O exercício a seguir compõem esse laboratório prático:

1. [Usando o ASP.NET MVC 4 de Scaffolding com migrações do Entity Framework](#Exercise1)

> [!NOTE]
> Este exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir o exercício. Você pode usar essa solução como um guia se você precisar de ajuda adicional, trabalhando no exercício.


Tempo estimado para concluir este laboratório: **30 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Exercício 1: Usando o ASP.NET MVC 4 de Scaffolding com migrações do Entity Framework

Estrutura do ASP.NET MVC fornece uma maneira rápida para gerar as operações CRUD de uma maneira padronizada, criando a lógica necessária que permite que seu aplicativo interaja com a camada de banco de dados.

Neste exercício, você aprenderá como usar scaffolding do ASP.NET MVC 4 com o código primeiro para criar os métodos CRUD. Em seguida, você aprenderá como atualizar seu modelo aplicando as alterações no banco de dados usando o Entity Framework migrações.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Tarefa 1-criar um novo do ASP.NET MVC 4 projeto usando o Scaffolding

1. Se não estiver aberto, inicie **Visual Studio 2012**.
2. Selecione **arquivo | Novo projeto**. No novo projeto em caixa de diálogo, o **Visual c# | Web** seção, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto para **MVC4andEFMigrations** e defina o local para **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** pasta deste laboratório. Definir o **nome da solução** para **começar** e certifique-se de **criar diretório para solução** é verificada. Clique em **OK**.

    ![Nova caixa de diálogo do projeto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nova caixa de diálogo do projeto ASP.NET MVC 4")

    *Nova caixa de diálogo do projeto ASP.NET MVC 4*
3. No **novo projeto ASP.NET MVC 4** select da caixa de diálogo de **aplicativo de Internet** modelo e certifique-se de que **Razor** é selecionado **domecanismodeexibição**. Clique em **OK** para criar o projeto.

    ![Novo aplicativo de Internet do ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "novo aplicativo de Internet do ASP.NET MVC 4")

    *Novo aplicativo de Internet do ASP.NET MVC 4*
4. No Gerenciador de soluções, clique com botão direito **modelos** e selecione **adicionar | Classe** para criar uma classe simples de pessoa (POCO). Nomeie- **pessoa** e clique em **Okey**.
5. Abra a classe de pessoa e insira as propriedades a seguir.

    (Código de trecho - *ASP.NET MVC 4 e o Entity Framework migrações - Ex1 pessoa propriedades*)


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
~~~
6. Clique em **compilar | Criar solução** para salvar as alterações e compilar o projeto.

    ![Criando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "criação do aplicativo")

    *Compilando o aplicativo*
7. No Gerenciador de soluções, clique na pasta controladores e selecione **adicionar | Controlador**.
8. Nome do controlador *PersonController* e conclua o **opções de Scaffolding** com os seguintes valores.

   1. No **modelo** lista suspensa, selecione o **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework** opção.
   2. No **classe modelo** lista suspensa, selecione o **pessoa** classe.
   3. No **classe de contexto de dados** lista, selecione  **&lt;novo contexto de dados... &gt;**. Escolher qualquer nome e clique em **Okey**.
   4. No **exibições** suspensa lista, certifique-se de que **Razor** está selecionado.

      ![Adição do controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "adição do controlador de pessoa com scaffolding")

      *Adição do controlador de pessoa com scaffolding*
9. Clique em **adicionar** para criar o novo controlador de pessoa com scaffolding. Você gerou as ações do controlador, bem como os modos de exibição.

    ![Depois de criar o controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "depois de criar o controlador de pessoa com scaffolding")

    *Depois de criar o controlador de pessoa com scaffolding*
10. Abra **PersonController** classe. Observe que os métodos de ação CRUD completos foi gerados automaticamente.

   ![No controlador de pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro a pessoa")

   *No controlador de pessoa*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tarefa 2-executar o aplicativo

Neste ponto, o banco de dados ainda não foi criado. Nesta tarefa, você irá executar o aplicativo pela primeira vez e testar as operações CRUD. O banco de dados será criado dinamicamente com o Code First.

1. Pressione **F5** para executar o aplicativo.
2. No navegador, adicionar **/Person** para a URL para abrir a página da pessoa.

    ![Primeira execução do aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "primeira execução do aplicativo")

    *O aplicativo: primeiro execute*
3. Agora você explorar as páginas de pessoa e testar as operações CRUD.

    1. Clique em **criar novo** para adicionar uma nova pessoa. Insira um nome e um sobrenome e clique em **criar**.

        ![Adicionando uma nova pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "adicionando uma nova pessoa")

        *Adicionando uma nova pessoa*
    2. Na lista da pessoa, você pode excluir, editar ou adicionar itens.

        ![lista de pessoas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de pessoas")

        *Lista de pessoas*
    3. Clique em **detalhes** para abrir os detalhes da pessoa.

        ![Detalhes da pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalhes da pessoa")

        *Detalhes da pessoa*
4. Feche o navegador e retorne ao Visual Studio. Observe que você criou o CRUD inteira para essa entidade em seu aplicativo - do modelo para os modos de exibição - sem a necessidade de escrever uma única linha de código!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tarefa 3-atualizando o banco de dados usando o Entity Framework migrações

Nesta tarefa, você atualizará o banco de dados usando o Entity Framework migrações. Você descobrirá como é fácil alterar o modelo e refletir as alterações nos bancos de dados usando o recurso de migrações do Entity Framework.

1. Abra o Console do Gerenciador de pacotes. Selecione **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console**.
2. No Console do Gerenciador de pacotes, digite o seguinte comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Habilitar migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migrações")

    *Habilitar migrações*

    O comando de permitir a migração cria o **migrações** pasta, que contém um script para inicializar o banco de dados.

    ![Pasta Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "pasta Migrations")

    *Pasta de migrações*
3. Abra o **Configuration.cs** arquivo na pasta de migrações. Localize o construtor da classe e altere o **AutomaticMigrationsEnabled** valor *true*.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
~~~
4. Abra a classe Person e adicione um atributo de nome do meio da pessoa. Com esse novo atributo, você está alterando o modelo.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
~~~
5. Selecione **compilar | Criar solução** no menu compilar o aplicativo.

    ![Criando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "criação do aplicativo")

    *Compilando o aplicativo*
6. No Console do Gerenciador de pacotes, digite o seguinte comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Este comando vai procurar alterações nos objetos de dados e, em seguida, ele adicionará os comandos necessários para modificar o banco de dados adequadamente.

    ![Adicionando um nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "adicionando um nome do meio")

    *Adicionando um nome do meio*
7. (Opcional) Você pode executar o seguinte comando para gerar um script SQL com a atualização diferencial. Isso permitirá que você atualizar o banco de dados manualmente (nesse caso não é necessário), ou aplicar as alterações em outros bancos de dados:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Gerar um script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "gerar um script SQL")

    *Gerar um script SQL*

    ![Atualização do Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "atualização do Script SQL")

    *Atualização do Script SQL*
8. No Console do Gerenciador de pacotes, digite o seguinte comando para atualizar o banco de dados:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Atualizando o banco de dados](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Atualizar banco de dados")

    *Atualizando o banco de dados*

    Isso adicionará o **MiddleName** coluna o **pessoas** tabela para coincidir com a definição atual do **pessoa** classe.
9. Depois que o banco de dados é atualizado, clique na pasta de controlador e selecione **adicionar | Controlador** para adicionar o controlador de pessoa novamente (completa com os mesmos valores). Isso atualizará os métodos existentes e as exibições para adicionar o novo atributo.

    ![Adicionando uma atualização do controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "adicionando uma atualização do controlador")

    *Atualizar o controlador*
10. Clique em **Adicionar**. Em seguida, selecione os valores **PersonController.cs substituir** e **substituir associados exibições** e clique em **Okey**.

   ![Adicionando uma substituição do controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Atualizar o controlador*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - executando o aplicativo

1. Pressione **F5** para executar o aplicativo.
2. Abra **/Person**. Observe que os dados foi preservados, enquanto a coluna de nome do meio foi adicionada.

    ![Nome do meio adicionado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "segundo nome adicionado")

    *Nome do meio adicionado*
3. Se você clicar em **editar**, você poderá adicionar um segundo nome do usuário atual.

    ![Nome do meio edição](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edição do nome do meio")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Este laboratório prático, você aprendeu etapas simples para criar operações CRUD com ASP.NET MVC 4 Scaffolding usando qualquer classe de modelo. Em seguida, você aprendeu como executar uma atualização de ponta a ponta em seu aplicativo - do banco de dados para os modos de exibição - usando o Entity Framework migrações.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express para o bloco de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice b: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
