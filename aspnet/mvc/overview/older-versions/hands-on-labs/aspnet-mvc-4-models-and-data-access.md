---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Acesso a dados e modelos do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Observação: Este laboratório prático supõe que você tenha um conhecimento básico do ASP.NET MVC. Se você não tiver usado o ASP.NET MVC antes, recomendamos que você falar sobre o ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Acesso a dados e modelos do ASP.NET MVC 4

Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](https://aka.ms/webcamps-training-kit)

Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC**. Se você não usou **ASP.NET MVC** antes, é recomendável que você passe **conceitos básicos do ASP.NET MVC 4** laboratório prático.

Este laboratório orienta os aperfeiçoamentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo da Web de exemplo fornecido na pasta de origem.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [acesso a dados e modelos do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

Em **conceitos básicos do ASP.NET MVC** laboratório prático, você tem foi passando dados embutida dos controladores de para os modelos de exibição. Mas, para criar um aplicativo Web real, você talvez queira usar um banco de dados real.

Este laboratório prático mostram como usar um mecanismo de banco de dados para armazenar e recuperar os dados necessários para o aplicativo de repositório de música. Para fazer isso, inicie com um banco de dados existente e criar o modelo de dados de entidade a partir dele. Durante este laboratório, você atenderá a **Database First** abordagem, bem como a **Code First** abordagem.

No entanto, você também pode usar o **Model First** abordagem, criar o mesmo modelo usando as ferramentas e, em seguida, gerar o banco de dados a partir dele.

![Vs primeiro do banco de dados. Modelo primeiro](aspnet-mvc-4-models-and-data-access/_static/image1.png "vs primeiro banco de dados. Modelo pela primeira vez")

*Vs primeiro do banco de dados. Modelo pela primeira vez*

Depois de gerar o modelo, você fará os ajustes adequados no StoreController para fornecer os modos de exibição do repositório com os dados obtidos do banco de dados, em vez de usar dados embutidos. Você não precisará fazer qualquer alteração nos modelos de exibição porque o StoreController será retornada mesmo ViewModels para os modelos de exibição, embora esse tempo os dados virão do banco de dados.

**A primeira abordagem de código**

A abordagem de Code First permite definir o modelo do código sem gerar classes que são geralmente juntamente com a estrutura.

No código em primeiro lugar, objetos de modelo são definidos com POCOs, &quot;Plain Old CLR Objects&quot;. POCOs são simples classes simples que não herança e não implementam as interfaces. Podemos gerar automaticamente o banco de dados deles, ou podemos usar um banco de dados e gera o mapeamento de classe a partir do código.

Os benefícios de usar essa abordagem é que o modelo permaneça independente da estrutura de persistência (nesse caso, Entity Framework), como as classes de POCOs não são juntamente com a estrutura de mapeamento.

> [!NOTE]
> Este laboratório é baseado no ASP.NET MVC 4 e uma versão do aplicativo de exemplo do repositório de música personalizada e minimizada para ajustar somente os recursos mostrados neste laboratório prático.
> 
> Se você quiser explorar todo o **repositório de música** aplicativo tutorial, você pode encontrar no [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).


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

1. [Exercício 1: Adicionar um banco de dados](#Exercise1)
2. [Exercício 2: Criar um banco de dados usando o Code First](#Exercise2)
3. [Exercício 3: Consultando o banco de dados com parâmetros](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **35 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Exercício 1: Adicionar um banco de dados

Neste exercício, você aprenderá como adicionar um banco de dados com as tabelas do aplicativo MusicStore à solução para consumir seus dados. Depois que o banco de dados é gerado com o modelo e adicionado à solução, você modificará a classe StoreController para fornecer o modelo de exibição com os dados obtidos do banco de dados, em vez de usar valores embutidos.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Tarefa 1: adicionar um banco de dados

Nesta tarefa, você irá adicionar um banco de dados já criado com as principais tabelas do aplicativo MusicStore à solução.

1. Abra o **começar** solução localizado em **fonte/Ex1-AddingADatabaseDBFirst/Begin/** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Adicionar **MvcMusicStore** arquivo de banco de dados. Neste laboratório prático, você usará um banco de dados já criado chamado **MvcMusicStore.mdf**. Para fazer isso, clique com botão direito **aplicativo\_dados** pasta, aponte para **adicionar** e, em seguida, clique em **Item existente**. Navegue até **\Source\Assets** e selecione o **MvcMusicStore.mdf** arquivo.

    ![Adicionar um Item existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "adicionar um Item existente")

    *Adicionar um Item existente*

    ![Arquivo de banco de dados de MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf arquivo de banco de dados")

    *Arquivo de banco de dados MvcMusicStore.mdf*

    O banco de dados foi adicionado ao projeto. Mesmo quando o banco de dados está localizado dentro da solução, você pode consultar e atualizá-lo como ele foi armazenado em um servidor de banco de dados diferente.

    ![Banco de dados MvcMusicStore no Gerenciador de soluções](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore banco de dados no Gerenciador de soluções")

    *Banco de dados MvcMusicStore no Gerenciador de soluções*
3. Verifique se a conexão ao banco de dados. Para fazer isso, clique duas vezes em **MvcMusicStore.mdf** para estabelecer uma conexão.

    ![Conectando-se a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "conectando-se a MvcMusicStore.mdf")

    *Conectando-se a MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Tarefa 2: criar um modelo de dados

Nesta tarefa, você criará um modelo de dados para interagir com o banco de dados adicionado na tarefa anterior.

1. Crie um modelo de dados que representa o banco de dados. Para fazer isso, no botão direito do mouse do Gerenciador de soluções de **modelos** pasta, aponte para **adicionar** e, em seguida, clique em **Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione o **dados** modelo e, em seguida, o **modelo de dados de entidade ADO.NET** item. Altere o nome do modelo de dados para **StoreDB.edmx** e clique em **adicionar**.

    ![Adicionar o modelo de dados de entidade ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "adicionando o modelo de dados de entidade ADO.NET StoreDB")

    *Adicionar o modelo de dados de entidade ADO.NET StoreDB*
2. O **Assistente de modelo de dados de entidade** será exibida. Este assistente o guiará através da criação da camada de modelo. Uma vez que o modelo deve ser criado com base no recentyl existentes do banco de dados adicionado, selecione **gerar do banco de dados** e clique em **próximo**.

    ![Escolhendo o conteúdo do modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "escolhendo o conteúdo do modelo")

    *Escolhendo o conteúdo do modelo*
3. Desde que você estiver gerando um modelo de banco de dados, você precisará especificar a conexão para usar. Clique em **nova Conexão**.
4. Selecione **o arquivo de banco de dados do Microsoft SQL Server** e clique em **continuar**.

    ![Escolher fonte de dados](aspnet-mvc-4-models-and-data-access/_static/image8.png "Escolher fonte de dados")

    *Escolha a caixa de diálogo de fonte de dados*
5. Clique em **procurar** e selecione o banco de dados **MvcMusicStore.mdf** localizado no **aplicativo\_dados** pasta e clique em **Okey**.

    ![Propriedades de Conexão](aspnet-mvc-4-models-and-data-access/_static/image9.png "propriedades de Conexão")

    *Propriedades de Conexão*
6. A classe gerada deve ter o mesmo nome que a cadeia de caracteres de conexão de entidade, portanto, altere seu nome para **MusicStoreEntities** e clique em **próximo**.

    ![Escolha a conexão de dados](aspnet-mvc-4-models-and-data-access/_static/image10.png "escolhendo a conexão de dados")

    *Escolha a conexão de dados*
7. Escolha os objetos de banco de dados para usar. Como o modelo de entidade usará apenas os tabelas do banco de dados, selecione o **tabelas** opção e certifique-se de que o **incluir colunas de chave estrangeira no modelo** e **Pluralizar ou singularizar gerado nomes de objeto** opções também são selecionadas. Alterar o Namespace de modelo para **MvcMusicStore.Model** e clique em **concluir**.

    ![Escolha os objetos de banco de dados](aspnet-mvc-4-models-and-data-access/_static/image11.png "escolhendo os objetos de banco de dados")

    *Escolha os objetos de banco de dados*

    > [!NOTE]
    > Se uma caixa de diálogo de aviso de segurança for exibida, clique em **Okey** para executar o modelo e gerar as classes para as entidades de modelo.
8. Um diagrama de entidade do banco de dados serão exibidos, enquanto uma classe separada que mapeia cada tabela no banco de dados será criada. Por exemplo, o **álbuns** tabela será representada por um **álbum** classe, onde cada coluna na tabela será mapeado para uma propriedade de classe. Isso permitirá que você consultar e trabalhar com objetos que representam linhas no banco de dados.

    ![Diagrama de entidade](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagrama de entidade")

    *Diagrama de entidade*

    > [!NOTE]
    > Os modelos T4. (TT) execute o código para gerar as classes de entidades e substituirão as classes existentes com o mesmo nome. Neste exemplo, as classes &quot;álbum&quot;, &quot;gênero&quot; e &quot;artista&quot; foram substituídos com o código gerado.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Tarefa 3: criar o aplicativo

Nesta tarefa, você verificará que, embora a geração de modelo removeu o **álbum**, **gênero** e **artista** classes de modelo, o projeto é compilado com êxito usando as novas classes de modelo de dados.

1. Compile o projeto selecionando o **criar** item de menu e, em seguida, **MvcMusicStore criar**.

    ![Compilar o projeto](aspnet-mvc-4-models-and-data-access/_static/image13.png "compilar o projeto")

    *Criar o projeto*
2. O projeto foi criado com êxito. Por que ele ainda funciona? Ele funciona porque as tabelas de banco de dados têm campos que incluem as propriedades que você estava usando nas classes removidas **álbum** e **gênero**.

    ![Compilações bem-sucedida](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilações foi bem-sucedida")

    *Compilações foi bem-sucedida*
3. Enquanto o designer exibe as entidades em um formato de diagrama, eles são realmente classes c#. Expanda o **StoreDB.edmx** nó no Gerenciador de soluções e, em seguida, **StoreDB.tt**, você verá as novas entidades geradas.

    ![Os arquivos gerados](aspnet-mvc-4-models-and-data-access/_static/image15.png "arquivos gerados")

    *Arquivos gerados*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarefa 4 - consulta o banco de dados

Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar dados codificados, ele consultará o banco de dados para recuperar as informações.

1. Abra **Controllers\StoreController.cs** e adicione o campo a seguir à classe para manter uma instância do **MusicStoreEntities** classe, denominada **storeDB**:

    (Código de trecho - *modelos e acesso a dados - Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. O **MusicStoreEntities** classe expõe uma propriedade de coleção para cada tabela no banco de dados. Atualização **procurar** método de ação para recuperar um gênero com todos os **álbuns**.

    (Código de trecho - *modelos e acesso a dados - Ex1 repositório procurar*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. Atualização **índice** método de ação para recuperar todos os gêneros.

    (Código de trecho - *modelos e acesso a dados - índice de repositório Ex1*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. Atualização **índice** método de ação para recuperar todos os gêneros e transformar a coleção para uma lista.

    (Código de trecho - *modelos e acesso a dados - Ex1 repositório GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5: executando o aplicativo

Nesta tarefa, você verificará que a página de índice de repositório agora exibirá os gêneros armazenados no banco de dados em vez do aqueles codificados. Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando as mesmas entidades como antes, embora esse tempo os dados virão do banco de dados.

1. Recompile a solução e pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Verificar se o menu de **gêneros** não é mais uma lista codificada, e os dados são recuperados diretamente do banco de dados.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Navegação gêneros do banco de dados*
3. Agora vá para qualquer gênero e verifique se que o álbuns são preenchidos a partir do banco de dados.

    ![Navegação álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image17.png "álbuns de navegação do banco de dados")

    *Álbuns de navegação do banco de dados*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Exercício 2: Criar um banco de dados usando o código

Neste exercício, você aprenderá como usar a abordagem de Code First para criar um banco de dados com as tabelas do aplicativo MusicStore e como acessar seus dados.

Depois que o modelo é gerado, você modificará o StoreController para fornecer o modelo de exibição com os dados obtidos do banco de dados, em vez de usar valores embutidos em código.

> [!NOTE]
> Se você concluiu o exercício 1 e já tiver trabalhado com a banco de dados primeira abordagem, agora você aprenderá como obter os mesmos resultados com um processo diferente. As tarefas que estão em comum com o exercício 1 foram marcadas para facilitar sua leitura. Se você não concluiu o exercício 1, mas gostaria de saber a abordagem de Code First, você pode iniciar este exercício e obter uma cobertura completa do tópico.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Tarefa 1 - popular os dados de exemplo

Nesta tarefa, você preencherá o banco de dados com dados de exemplo, quando ele é criado inicialmente usando Code First.

1. Abra o **começar** solução localizado em **fonte/o Ex2-CreatingADatabaseCodeFirst/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Adicionar o **SampleData.cs** o arquivo para o **modelos** pasta. Para fazer isso, clique com botão direito **modelos** pasta, aponte para **adicionar** e, em seguida, clique em **Item existente**. Navegue até **\Source\Assets** e selecione o **SampleData.cs** arquivo.

    ![Dados de exemplo popular código](aspnet-mvc-4-models-and-data-access/_static/image18.png "código de popular de dados de exemplo")

    *Código de popular de dados de exemplo*
3. Abra o **Global.asax.cs** de arquivo e adicione o seguinte *usando* instruções.

    (Código de trecho - *modelos e acesso a dados - o Ex2 Global Asax usos*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. No **aplicativo\_Start ()** método adicionar a linha a seguir para definir o inicializador de banco de dados.

    (Código de trecho - *modelos e acesso a dados - o Ex2 Global Asax SetInitializer*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Tarefa 2: configurar a conexão ao banco de dados

Agora que você já adicionou um banco de dados ao nosso projeto, você escreverá **Web. config** a cadeia de caracteres de conexão do arquivo.

1. Adicionar uma cadeia de caracteres de conexão em **Web. config**. Para fazer isso, abra **Web. config** na raiz do projeto e substituir a cadeia de caracteres de conexão chamado DefaultConnection com esta linha de **&lt;connectionStrings&gt;** seção:

    ![Local do arquivo Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "local do arquivo Web. config")

    *local do arquivo Web. config*


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Tarefa 3: Trabalhando com o modelo

Agora que você já tiver configurado a conexão ao banco de dados, você vinculará o modelo com as tabelas de banco de dados. Nesta tarefa, você criará uma classe que será vinculada ao banco de dados com o Code First. Lembre-se de que há uma classe de modelo existente POCO que deve ser modificada.

   > [!NOTE]
> Se você concluiu o exercício 1, você observará que esta etapa foi executada por um assistente. Fazendo Code First, você criará manualmente classes que serão vinculados entidades de dados.


1. Abra a classe de modelo POCO **gênero** de **modelos** pasta do projeto e incluir uma ID. Use uma int de propriedade com o nome **GenreId**.

    (Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro gênero*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. Agora, abra a classe de modelo POCO **álbum** de **modelos** pasta do projeto e inclui as chaves estrangeiras, crie propriedades com os nomes **GenreId** e  **ArtistId**. Essa classe já tiver o **GenreId** para a chave primária.

    (Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro álbum*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. Abra a classe de modelo POCO **artista** e incluir o **ArtistId** propriedade.

    (Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro artista*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. Clique com botão direito do **modelos** pasta do projeto e selecione **adicionar | Classe**. Nomeie o arquivo **MusicStoreEntities.cs**. Em seguida, clique em **adicionar.**

    ![Adicionando uma classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "adicionando uma classe")

    *Adicionar um novo item*

    ![Adicionando um class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "adicionando um class2")

    *Adicionando uma classe*
5. Abra a classe que você acabou de criar, **MusicStoreEntities.cs**e incluem os namespaces **System.Data.Entity** e **System.Data.Entity.Infrastructure**.


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. Substitua a declaração de classe para estender o **DbContext** classe: declarar um público **DBSet** e substituir **OnModelCreating** método. Após essa etapa, você obterá uma classe de domínio que será vinculado a seu modelo com o Entity Framework. Para fazer isso, substitua o código de classe com o seguinte:

    (Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro MusicStoreEntities*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarefa 4 - consulta o banco de dados

Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar dados codificados, ele irá recuperá-lo do banco de dados.

> [!NOTE]
> Esta tarefa está em comum com o exercício 1.
> 
> Se você concluiu o exercício 1, você observará que essas etapas são as mesmas em ambas as abordagens (banco de dados pela primeira vez ou código primeiro). São diferentes em como os dados estão vinculados com o modelo, mas o acesso a entidades de dados ainda é transparente do controlador.


1. Abra **Controllers\StoreController.cs** e adicione o campo a seguir à classe para manter uma instância do **MusicStoreEntities** classe, denominada **storeDB**:

    (Código de trecho - *modelos e acesso a dados - Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. O **MusicStoreEntities** classe expõe uma propriedade de coleção para cada tabela no banco de dados. Atualização **procurar** método de ação para recuperar um gênero com todos os **álbuns**.

    (Código de trecho - *modelos e acesso a dados - o Ex2 repositório procurar*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. Atualização **índice** método de ação para recuperar todos os gêneros.

    (Código de trecho - *modelos e acesso a dados - índice de repositório o Ex2*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. Atualização **índice** método de ação para recuperar todos os gêneros e transformar a coleção para uma lista.

    (Código de trecho - *modelos e acesso a dados - o Ex2 repositório GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarefa 5: executando o aplicativo

Nesta tarefa, você verificará que a página de índice de repositório agora exibirá os gêneros armazenados no banco de dados em vez do aqueles codificados. Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando o mesmo **StoreIndexViewModel** como antes, mas desta vez os dados virão do banco de dados.

1. Recompile a solução e pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Verificar se o menu de **gêneros** não é mais uma lista codificada, e os dados são recuperados diretamente do banco de dados.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Navegação gêneros do banco de dados*
3. Agora vá para qualquer gênero e verifique se que o álbuns são preenchidos a partir do banco de dados.

    ![Navegação álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image23.png "álbuns de navegação do banco de dados")

    *Álbuns de navegação do banco de dados*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Exercício 3: Consultando o banco de dados com parâmetros

Neste exercício, você aprenderá como o banco de dados usando parâmetros de consulta e como usar a formatação de resultados de consulta, um recurso que reduz o banco de dados de número acessa a recuperação de dados de forma mais eficiente.

> [!NOTE]
> Para obter mais informações sobre formatação de resultados de consulta, visite o [artigo do msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Tarefa 1 - StoreController modificação para recuperar álbuns de banco de dados

Nesta tarefa, você alterará a **StoreController** classe para acessar o banco de dados para recuperar álbuns de um gênero específico.

1. Abra o **começar** solução localizado no **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** pasta se você quiser usar a abordagem de código ou **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** pasta se você quiser usar a abordagem de banco de dados. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **StoreController** classe para alterar o **procurar** método de ação. Para fazer isso, no **Solution Explorer**, expanda o **controladores** pasta e clique duas vezes em **StoreController.cs**.
3. Alterar o **procurar** método de ação para recuperar álbuns de um gênero específico. Para fazer isso, substitua o código a seguir:

    (Código de trecho - *modelos e acesso a dados - Ex3 StoreController BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarefa 2: executando o aplicativo

Nesta tarefa, você executar o aplicativo e recuperará álbuns de um gênero específico do banco de dados.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **repositório/procurar? gênero = Pop** para verificar se os resultados são recuperados do banco de dados.

    ![Procurando por gênero](aspnet-mvc-4-models-and-data-access/_static/image24.png "navegação por gênero")

    *Pesquisa/repositório/procurar? gênero = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Tarefa 3: acessando álbuns por Id

Nesta tarefa, repita o procedimento anterior para obter álbuns por seu ID.

1. Feche o navegador se necessário, para retornar ao Visual Studio. Abra o **StoreController** classe para alterar o **detalhes** método de ação. Para fazer isso, no **Solution Explorer**, expanda o **controladores** pasta e clique duas vezes em **StoreController.cs**.
2. Alterar o **detalhes** método de ação ao recuperar detalhes de álbuns com base em seus **Id**. Para fazer isso, substitua o código a seguir:

    (Código de trecho - *modelos e acesso a dados - Ex3 StoreController DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - executando o aplicativo

Nesta tarefa, você executar o aplicativo em um navegador da web e obter detalhes sobre o álbum por seu ID.

1. Pressione **F5** para executar o aplicativo.
2. O projeto é iniciado na Home page. Altere a URL para **/Store/Details/51** ou procurar os gêneros e selecione um álbum para verificar se os resultados são recuperados do banco de dados.

    ![Procurando detalhes](aspnet-mvc-4-models-and-data-access/_static/image25.png "procurando detalhes")

    *Navegação /Store/Details/51*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Por concluir este laboratório prático você aprendeu os fundamentos do acesso a dados e modelos do ASP.NET MVC, usando o **Database First** abordagem, bem como a **Code First** abordagem:

- Como adicionar um banco de dados para a solução para consumir seus dados
- Como atualizar os controladores para fornecer exibir modelos com os dados obtidos do banco de dados em vez de um embutido
- Como consultar o banco de dados usando parâmetros
- Como usar a consulta resultado modelagem, um recurso que reduz o número de acessos de banco de dados, recuperação de dados de forma mais eficiente
- Como usar abordagens Database First e Code First no Entity Framework da Microsoft para vincular o banco de dados com o modelo

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Faça logon no portal do Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal de gerenciamento do Azure do Windows*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "criar um novo Site")

    *Criando um novo Site*
3. Clique em **de computação** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site e clique em **Criar Site**.

    > [!NOTE]
    > Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "criar um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando para o novo site](aspnet-mvc-4-models-and-data-access/_static/image34.png "navegando para o novo site")

    *Navegando para o novo site*

    ![Site da Web em execução](aspnet-mvc-4-models-and-data-access/_static/image35.png "site em execução")

    *Site da Web em execução*
6. Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-models-and-data-access/_static/image36.png "abrir páginas de gerenciamento do site")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.

    ![Baixando o site da web de perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image37.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação para um local conhecido. Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image38.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL. Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**. Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados, como ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) botão.

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Adicionar endereço IP do cliente*
3. Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confirmar alterações*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.

    ![Publicar o aplicativo](aspnet-mvc-4-models-and-data-access/_static/image43.png "a publicação do aplicativo")

    *Publicar o site*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importar o perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image44.png "importar o perfil de publicação")

    *Importar o perfil de publicação*
3. Clique em **validar Conexão**. Quando a validação estiver concluída, clique em **próximo**.

    > [!NOTE]
    > A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.

    ![Validando a conexão](aspnet-mvc-4-models-and-data-access/_static/image45.png "validação de conexão")

    *Validação de conexão*
4. No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Em **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Em **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados.

     ![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](aspnet-mvc-4-models-and-data-access/_static/image48.png "criar a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **visualização** , clique em **publicar**.

    ![Publicar o aplicativo web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publicar o aplicativo web")

    *Publicar o aplicativo web*
9. Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apêndice c: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-models-and-data-access/_static/image51.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](aspnet-mvc-4-models-and-data-access/_static/image52.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-models-and-data-access/_static/image53.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-models-and-data-access/_static/image54.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-models-and-data-access/_static/image55.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-models-and-data-access/_static/image56.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
