---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Migrações e Scaffolding do Entity Framework do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve estar ciente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394197"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Migrações e Scaffolding do Entity Framework do ASP.NET MVC 4

por [Web Camps equipe](https://twitter.com/webcamps)

[Baixe o Kit de treinamento do Web Camps](https://aka.ms/webcamps-training-kit)

Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve conhecer muitos da lógica para criar, atualizar, listar e remover qualquer entidade de dados que ele é repetido entre o aplicativo. Para não mencionar que, se seu modelo tiver várias classes para manipular, será provável gastar um tempo considerável, escrever os métodos de ação POST e GET para cada operação de entidade, bem como cada um dos modos de exibição.

Neste laboratório, você aprenderá como usar o scaffolding do ASP.NET MVC 4 para gerar automaticamente a linha de base de CRUD seu aplicativo (criar, ler, atualizar e excluir). A partir de uma classe de modelo simples e, sem escrever uma única linha de código, você criará um controlador que irá conter todas as operações de CRUD, bem como as todas as necessárias exibições. Depois de criar e executar a solução mais simples, você terá o banco de dados do aplicativo gerado, juntamente com a lógica MVC e os modos de exibição para manipulação de dados.

Além disso, você aprenderá como é fácil usar migrações do Entity Framework para executar atualizações de modelo ao longo de todo o seu aplicativo. Migrações do Entity Framework permitem que você modifique seu banco de dados depois que o modelo foi alterado com etapas simples. Com todos eles em mente, você poderá criar e manter aplicativos da web com mais eficiência, aproveitando os recursos mais recentes do ASP.NET MVC 4.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível no site da [versões Web/Microsoft-WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [migrações e Scaffolding do ASP.NET MVC 4 Entity Framework](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Use o scaffolding do ASP.NET para operações CRUD em controladores.
- Altere o modelo de banco de dados usando migrações do Entity Framework.

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

O exercício a seguir fazem parte deste laboratório prático:

1. [Usando o Scaffolding do ASP.NET MVC 4 com migrações do Entity Framework](#Exercise1)

> [!NOTE]
> Este exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir o exercício. Você pode usar essa solução como um guia se você precisar de ajuda adicional, trabalhar com o exercício.


Tempo estimado para concluir este laboratório: **30 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Exercício 1: Usando o Scaffolding do ASP.NET MVC 4 com migrações do Entity Framework

O scaffolding do ASP.NET MVC fornece uma maneira rápida para gerar as operações CRUD de uma maneira padronizada, criando a lógica necessária que permite que seu aplicativo interaja com a camada de banco de dados.

Neste exercício, você aprenderá a usar scaffolding do ASP.NET MVC 4 com o código primeiro para criar os métodos CRUD. Em seguida, você aprenderá a atualizar o modelo de aplicação das alterações no banco de dados por meio de migrações do Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Projeto da tarefa 1-Criando um novo do ASP.NET MVC 4 usando o Scaffolding

1. Se não estiver aberto, inicie **Visual Studio 2012**.
2. Selecione **arquivo | Novo projeto**. No novo projeto na caixa de diálogo, o **Visual c# | Web** seção, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto para **MVC4andEFMigrations** e defina o local como **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** pasta deste laboratório. Definir a **nome da solução** para **começar** e verifique se **criar diretório para solução** é verificada. Clique em **OK**.

    ![Caixa de diálogo Novo projeto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "caixa de diálogo Novo projeto ASP.NET MVC 4")

    *Caixa de diálogo Novo projeto ASP.NET MVC 4*
3. No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo e certifique-se de que **Razor** está selecionado **domecanismodeexibição**. Clique em **OK** para criar o projeto.

    ![Novo aplicativo de Internet do ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "novo aplicativo de Internet do ASP.NET MVC 4")

    *Novo aplicativo de Internet do ASP.NET MVC 4*
4. No Gerenciador de soluções, clique com botão direito **modelos** e selecione **adicionar | Classe** para criar uma pessoa de classe simples (POCO). Denomine **pessoa** e clique em **Okey**.
5. Abra a classe Person e insira as propriedades a seguir.

    (Código de trecho de código – *ASP.NET MVC 4 e migrações do Entity Framework - propriedades de pessoas Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Clique em **compilar | Compilar solução** para salvar as alterações e compile o projeto.

    ![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilando o aplicativo")

    *Compilando o aplicativo*
7. No Gerenciador de soluções, clique com botão direito na pasta controladores e selecione **adicionar | Controlador**.
8. Nomeie o controlador *PersonController* e conclua a **opções de Scaffolding** com os seguintes valores.

   1. No **modelo** lista suspensa, selecione o **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework** opção.
   2. No **classe de modelo** lista suspensa, selecione o **pessoa** classe.
   3. No **classe de contexto de dados** lista, selecione  **&lt;novo contexto de dados... &gt;**. Escolha qualquer nome e clique em **Okey**.
   4. No **modos de exibição** suspensa lista, certifique-se de que **Razor** está selecionado.

      ![Adicionando o controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "adicionando o controlador de pessoa com scaffolding")

      *Adicionando o controlador de pessoa com scaffolding*
9. Clique em **adicionar** para criar o novo controlador de pessoa com scaffolding. Agora, você tiver gerado as ações do controlador, bem como os modos de exibição.

    ![Depois de criar o controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "depois de criar o controlador de pessoa com scaffolding")

    *Depois de criar o controlador de pessoa com scaffolding*
10. Abra **PersonController** classe. Observe que os métodos de ação CRUD completos foi gerados automaticamente.

   ![Dentro do controlador de pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro a pessoa")

   *Dentro do controlador de pessoa*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tarefa 2-executar o aplicativo

Neste ponto, o banco de dados ainda não foi criado. Nesta tarefa, você irá executar o aplicativo pela primeira vez e testar as operações CRUD. O banco de dados será criado em tempo real com o Code First.

1. Pressione **F5** para executar o aplicativo.
2. No navegador, adicione **/Person** para a URL para abrir a página de pessoa.

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
4. Feche o navegador e retorne ao Visual Studio. Observe que você tenha criado o CRUD todo entidade person em todo o aplicativo - do modelo para os modos de exibição - sem escrever uma única linha de código!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tarefa 3-atualizando o banco de dados usando migrações do Entity Framework

Nesta tarefa, você atualizará o banco de dados usando migrações do Entity Framework. Você descobrirá como é fácil alterar o modelo e refletir as alterações nos bancos de dados usando o recurso de migrações do Entity Framework.

1. Abra o Console do Gerenciador de pacotes. Selecione **ferramentas | Gerenciador de pacotes de biblioteca | Package Manager Console**.
2. No Console do Gerenciador de pacotes, digite o seguinte comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Habilitar migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migrações")

    *Habilitar migrações*

    O comando Enable-migração cria o **migrações** pasta que contém um script para inicializar o banco de dados.

    ![Pasta Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "pasta Migrations")

    *Pasta Migrations*
3. Abra o **Configuration.cs** arquivo à pasta Migrations. Localize o construtor de classe e altere o **AutomaticMigrationsEnabled** valor para *verdadeiro*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Abra a classe Person e adicione um atributo para o nome da pessoa intermediária. Com esse novo atributo, você está alterando o modelo.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Selecione **compilar | Compilar solução** no menu compilar o aplicativo.

    ![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilando o aplicativo")

    *Compilando o aplicativo*
6. No Console do Gerenciador de pacotes, digite o seguinte comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Esse comando irá procurar alterações nos objetos de dados e, em seguida, ele adicionará os comandos necessários para modificar o banco de dados de acordo.

    ![Adicionando um nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "adicionando um nome do meio")

    *Adicionando um nome do meio*
7. (Opcional) Você pode executar o seguinte comando para gerar um script SQL com a atualização de diferencial. Isso permitirá que você atualizar o banco de dados manualmente (nesse caso, não é necessário), ou aplicar as alterações em outros bancos de dados:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Gerar um script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "gerando um script SQL")

    *Gerar um script SQL*

    ![Atualização de Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "atualização de Script SQL")

    *Atualização de Script SQL*
8. No Console do Gerenciador de pacotes, digite o seguinte comando para atualizar o banco de dados:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Atualizando o banco de dados](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "atualizando o banco de dados")

    *Atualizando o banco de dados*

    Isso adicionará a **MiddleName** coluna o **pessoas** tabela para coincidir com a definição atual do **pessoa** classe.
9. Depois que o banco de dados for atualizado, clique com botão direito na pasta do controlador e selecione **adicionar | Controlador** para adicionar o controlador de pessoa novamente (completa com os mesmos valores). Isso atualizará os métodos existentes e exibições para adicionar o novo atributo.

    ![Adição de uma atualização do controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "adicionando uma atualização do controlador")

    *Atualizar o controlador*
10. Clique em **Adicionar**. Em seguida, selecione os valores **substituir PersonController.cs** e o **substituir associados a exibições** e clique em **Okey**.

   ![Adicionando uma substituição de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Atualizar o controlador*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - executando o aplicativo

1. Pressione **F5** para executar o aplicativo.
2. Abra **/Person**. Observe que os dados foi preservados, enquanto a coluna de nome do meio foi adicionada.

    ![Nome do meio adicionado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "sobrenome adicionado")

    *Nome do meio adicionado*
3. Se você clicar **editar**, você poderá adicionar um nome do meio para a pessoa atual.

    ![Edição do nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edição do nome do meio")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, você aprendeu as etapas simples para criar operações CRUD com Scaffolding do ASP.NET MVC 4 usando qualquer classe de modelo. Em seguida, você aprendeu a executar uma atualização de ponta a ponta em seu aplicativo - do banco de dados para os modos de exibição - por meio de migrações do Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalação do Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalar o Visual Studio Express")

    *Instalar o Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação for concluído.

    ![Progresso da instalação](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalação concluída*
7. Clique em **Exit** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco da Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express para o bloco da Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice b: usar trechos de código

Com trechos de código, você tem todo o código que necessário ao seu alcance. O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usar trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "trechos de código usando o Visual Studio para inserir código em seu projeto")

*Usar trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (somente c#)***

1. Coloque o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho de código (sem espaços ou hifens).
3. Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho de código serão expandido](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "pressione Tab novamente e o trecho de código serão expandido")

*Pressione Tab novamente e o trecho de código serão expandido*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito no qual você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")

*Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet*

![Escolher o trecho relevante na lista, clicando nele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "escolher o trecho relevante na lista, clicando nele")

*Escolher o trecho relevante na lista, clicando nele*
