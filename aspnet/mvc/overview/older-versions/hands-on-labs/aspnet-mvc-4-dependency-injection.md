---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Injeção de dependência do ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Observação: Este laboratório prático supõe que você tenha um conhecimento básico de filtros ASP.NET MVC e ASP.NET MVC 4. Se você não usou filtros ASP.NET MVC 4 antes, nós rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: c735bbdafe4b8f0423abb6bacd076f173a1be9d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Injeção de dependência do ASP.NET MVC 4

Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](https://aka.ms/webcamps-training-kit)

Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC** e **filtros ASP.NET MVC 4**. Se você não usou **filtros ASP.NET MVC 4** antes, é recomendável que você passe **filtros de ação do ASP.NET MVC personalizado** laboratório prático.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível no site da [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [injeção de dependência do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

Em **programação orientada a objeto** paradigma, objetos trabalham em conjunto em um modelo de colaboração em que há colaboradores e consumidores. Naturalmente, esse modelo de comunicação gera as dependências entre objetos e componentes, tornando-se difíceis de gerenciar ao aumenta a complexidade.

![Dependências de classe e a complexidade do modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "dependências de classe e a complexidade do modelo")

*Dependências de classe e a complexidade do modelo*

Você já deve ter ouvido sobre o **padrão de fábrica** e a separação entre a interface e a implementação usando os serviços, onde os objetos de cliente geralmente são responsáveis por local do serviço.

O padrão de injeção de dependência é uma implementação específica de inversão de controle. **Inversão de controle (IoC)** significa que os objetos não crie outros objetos que dependem para fazer seu trabalho. Em vez disso, eles obtêm os objetos que precisam de uma fonte externa (por exemplo, um arquivo de configuração xml).

**Injeção de dependência (DI)** significa que isso é feito sem a intervenção de objeto, normalmente por um componente de estrutura que passa parâmetros do construtor e definir propriedades.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>O padrão de Design de (DI) de injeção de dependência

Em um nível alto, o objetivo de injeção de dependência é que uma classe de cliente (por exemplo, *o jogador*) precisa de algo que atenda a uma interface (por exemplo, *IClub*). Ele não se importa o que é o tipo concreto (por exemplo, *WoodClub, IronClub, WedgeClub* ou *PutterClub*), que quiser que outra pessoa para lidar com isso (por exemplo, um bom *caddy*). O resolvedor de dependência no ASP.NET MVC podem permitir que você registrar sua lógica de dependência em outro lugar (por exemplo, um contêiner ou um *recipiente de paus*).

![Diagrama de injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustração de injeção de dependência")

*Injeção de dependência - analogia Golfe*

As vantagens de usar o padrão de injeção de dependência e inversão de controle são os seguintes:

- Reduz o acoplamento de classes
- Aumenta a reutilização de código
- Melhora a capacidade de manutenção de código
- Melhora o teste de aplicativos

> [!NOTE]
> Injeção de dependência, às vezes, é comparada com o padrão de Design de fábrica abstrata, mas há uma pequena diferença entre as duas abordagens. Injeção de dependência tem uma estrutura de trabalhar por trás de resolver dependências chamando as fábricas e os serviços registrados.


Agora que você entende o padrão de injeção de dependência, você aprenderá durante este laboratório para aplicá-la no ASP.NET MVC 4. Você irá iniciar usando a injeção de dependência no **controladores** para incluir um serviço de acesso de banco de dados. Em seguida, você aplicará a injeção de dependência de **exibições** consumir um serviço e exiba informações. Por fim, você estenderá a injeção de dependência para ASP.NET MVC 4 filtros, injeção de um filtro de ação personalizada na solução.

Neste laboratório prático, você aprenderá como:

- Integrar o ASP.NET MVC 4 com Unity para injeção de dependência usando pacotes do NuGet
- Use a injeção de dependência dentro de um controlador MVC do ASP.NET
- Use a injeção de dependência dentro de uma exibição ASP.NET MVC
- Use a injeção de dependência dentro de um filtro de ação do ASP.NET MVC

> [!NOTE]
> Este laboratório está usando o pacote do NuGet Unity.Mvc3 para resolução de dependência, mas é possível adaptar qualquer estrutura de injeção de dependência para trabalhar com o ASP.NET MVC 4.


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

Este laboratório prático é composto pelos exercícios a seguir:

1. [Exercício 1: Insira um controlador](#Exercise1)
2. [Exercício 2: Insira um modo de exibição](#Exercise2)
3. [Exercício 3: Injeção de filtros](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Exercício 1: Insira um controlador

Neste exercício, você aprenderá como usar injeção de dependência em controladores do ASP.NET MVC, integrando Unity usando um pacote do NuGet. Por esse motivo, você incluirá os serviços em seus controladores MvcMusicStore para separar a lógica de acesso a dados. Os serviços criará uma nova dependência no construtor de controlador, que será resolvido usando a injeção de dependência com a Ajuda de **Unity**.

Essa abordagem mostrará como gerar menos aplicativos acoplados, que são mais flexíveis e fáceis de manter e testar. Você também aprenderá como integrar o ASP.NET MVC Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Sobre o serviço de StoreManager

O repositório de música MVC fornecida na solução começar agora inclui um serviço que gerencia os dados do controlador de armazenamento denominados **StoreService**. Abaixo você encontrará a implementação do serviço de armazenamento. Observe que todos os métodos retornam entidades de modelo.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** do begin solução agora consome **StoreService**. Todas as referências de dados foram removidas do **StoreController**e agora é possível modificar o provedor de acesso de dados atual sem alterar qualquer método que consome **StoreService**.

Abaixo, você encontrará o **StoreController** implementação tem uma dependência com **StoreService** dentro do construtor de classe.

> [!NOTE]
> A dependência introduzida neste exercício está relacionada à **inversão de controle** (IoC).
> 
> O **StoreController** construtor da classe recebe um **IStoreService** parâmetro de tipo, que é essencial para realizar chamadas de serviço de dentro da classe. No entanto, **StoreController** não implementa o construtor padrão (sem nenhum parâmetro) que qualquer controlador deve ter para funcionar com o ASP.NET MVC.
> 
> Para resolver a dependência, o controlador deve ser criado por uma fábrica abstrata (uma classe que retorna qualquer objeto do tipo especificado).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Você obterá um erro quando a classe tenta criar a StoreController sem enviar o objeto de serviço, pois não há nenhum construtor sem parâmetros declarado.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tarefa 1: executar o aplicativo

Nesta tarefa, você executará o aplicativo de início, que inclui o serviço ao controlador de armazenamento que separa o acesso a dados da lógica do aplicativo.

Ao executar o aplicativo, você receberá uma exceção, como o serviço de controlador não for passado como um parâmetro por padrão:

1. Abra o **começar** solução localizada no **Controller\Begin injetando Source\Ex01**.

    1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Pressione **Ctrl + F5** para executar o aplicativo sem depuração. Você receberá a mensagem de erro &quot; **nenhum construtor sem parâmetros definido para este objeto**&quot;:

    ![Erro durante a execução do aplicativo do ASP.NET MVC começar](aspnet-mvc-4-dependency-injection/_static/image3.png "erro durante a execução do aplicativo do ASP.NET MVC começar")

    *Erro durante a execução do aplicativo do ASP.NET MVC começar*
3. Feche o navegador.

As etapas a seguir, você trabalhará na solução de armazenamento de música injetar a dependência que este controlador precisa.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tarefa 2 - incluindo Unity na solução MvcMusicStore

Nesta tarefa, você incluirá **Unity.Mvc3** pacote NuGet para a solução.

> [!NOTE]
> Unity.Mvc3 pacote foi projetado para ASP.NET MVC 3, mas ele é totalmente compatível com o ASP.NET MVC 4.
> 
> Unity é um contêiner de injeção de dependência leve e extensível com suporte opcional para a instância e digite a interceptação. É um contêiner de finalidade geral para uso em qualquer tipo de aplicativo .NET. Ele fornece todos os recursos comuns encontrados nos mecanismos de injeção de dependência incluindo: criação de objeto, a abstração de requisitos com a especificação de dependências em tempo de execução e a flexibilidade, adiando a configuração do componente para o contêiner.


1. Instalar **Unity.Mvc3** pacote NuGet no **MvcMusicStore** projeto. Para fazer isso, abra o **Package Manager Console** de **exibição** | **outras janelas**.
2. Execute o seguinte comando.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalando o pacote do NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instalando o pacote do NuGet Unity.Mvc3")

    *Instalando o pacote do NuGet Unity.Mvc3*
3. Uma vez o **Unity.Mvc3** pacote está instalado, explore os arquivos e pastas que adiciona automaticamente para simplificar a configuração de unidade.

    ![Pacote de Unity.Mvc3 instalado](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 pacote instalado")

    *Pacote de Unity.Mvc3 instalado*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Tarefa 3 - registrar Unity no aplicativo Global.asax.cs\_iniciar

Nesta tarefa, você atualizará o **aplicativo\_iniciar** método localizado em **Global.asax.cs** para chamar o inicializador de Bootstrapper Unity e, em seguida, atualize o registro de arquivo de Bootstrapper o serviço e o controlador usará para injeção de dependência.

1. Agora, você irá se conectar o Bootstrapper que é o arquivo que inicializa o contêiner do Unity e resolvedor de dependência. Para fazer isso, abra **Global.asax.cs** e adicione o seguinte código dentro de **aplicativo\_iniciar** método.

    (Código de trecho - *Unity inicializar o laboratório de injeção de dependência do ASP.NET - Ex01 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Abra **Bootstrapper.cs** arquivo.
3. Inclua os seguintes namespaces: **MvcMusicStore.Services** e **MusicStore.Controllers**.

    (Código de trecho - *injeção de dependência ASP.NET laboratório - Ex01 - Bootstrapper adicionando Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Substituir **BuildUnityContainer** do conteúdo com o código a seguir que registra o controlador de armazenamento e o serviço de armazenamento de método.

    (Código de trecho - *serviço e o controlador de armazenamento de registro do ASP.NET dependência injeção laboratório - Ex01 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - executando o aplicativo

Nesta tarefa, você executará o aplicativo para verificar que ele agora pode ser carregado depois incluindo Unity.

1. Pressione **F5** para executar o aplicativo, o aplicativo deve carregar agora sem mostrar qualquer mensagem de erro.

    ![Execução do aplicativo com a injeção de dependência](aspnet-mvc-4-dependency-injection/_static/image6.png "executando o aplicativo com a injeção de dependência")

    *Aplicativo em execução com a injeção de dependência*
2. Navegue até **/armazenamento**. Isso vai invocar **StoreController**, que é criado usando **Unity**.

    ![Repositório de música MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "repositório de música do MVC")

    *Repositório de música do MVC*
3. Feche o navegador.

Os exercícios a seguir, você aprenderá como estender o escopo de injeção de dependência para usá-lo em exibições do ASP.NET MVC e filtros de ação.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Exercício 2: Insira um modo de exibição

Neste exercício, você aprenderá como usar injeção de dependência em uma exibição com os novos recursos do ASP.NET MVC 4 para a integração do Unity. Para fazer isso, você chamará um serviço personalizado dentro da exibição de procurar o repositório, que mostrará uma mensagem e uma imagem abaixo.

Em seguida, você integrar o projeto com o Unity e criar um resolvedor de dependência personalizada para injetar as dependências.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tarefa 1: criar uma exibição que consome um serviço

Nesta tarefa, você criará uma exibição que executa uma chamada de serviço para gerar uma nova dependência. O serviço consiste em um serviço de mensagens simple incluído nesta solução.

1. Abra o **começar** solução localizada no **View\Begin injetando Source\Ex02** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
    > 
    > Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluir o **MessageService.cs** e o **IMessageService.cs** classes localizado no **\Assets de origem** pasta no **/serviços**. Para fazer isso, clique com botão direito **serviços** pasta e selecione **Add Existing Item**. Navegue até o local dos arquivos e incluí-los.

    ![Adicionar serviço de mensagens e Interface de serviço](aspnet-mvc-4-dependency-injection/_static/image8.png "Adicionar serviço de mensagens e Interface de serviço")

    *Adicionar serviço de mensagens e a Interface de serviço*

    > [!NOTE]
    > O **IMessageService** interface define duas propriedades implementadas pelo **MessageService** classe. Essas propriedades -**mensagem** e **ImageUrl**-armazenar a mensagem e a URL da imagem a ser exibida.
3. Crie a pasta **/páginas** do projeto raiz da pasta e, em seguida, adicione a classe existente **MyBasePage.cs** de **Source\Assets**. A página de base que herda de tem a seguinte estrutura.

    ![Pasta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "pasta páginas")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Abra **Browse.cshtml** exibir de **/exibições/repositório** pasta e torná-lo herdam **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. No **procurar** exibir, adicionar uma chamada para **MessageService** para exibir uma imagem e uma mensagem recuperada pelo serviço.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tarefa 2 - incluindo um resolvedor de dependência personalizada e um ativador de página de modo de exibição personalizado

Na tarefa anterior, é inserida uma nova dependência dentro de um modo de exibição para executar uma chamada de serviço dentro dele. Agora, você resolverá essa dependência ao implementar as interfaces de injeção de dependência do ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**. Você incluirá na solução de uma implementação de **IDependencyResolver** que lidará com a recuperação de serviço usando o Unity. Em seguida, você incluirá outra implementação personalizada de **IViewPageActivator** interface que resolverá a criação dos modos de exibição.

> [!NOTE]
> Desde o ASP.NET MVC 3, a implementação para injeção de dependência tinha simplificado as interfaces para registrar os serviços. **IDependencyResolver** e **IViewPageActivator** fazem parte dos recursos do ASP.NET MVC 3 para injeção de dependência.
> 
> **-IDependencyResolver** interface substitui o IMvcServiceLocator anterior. Os implementadores de IDependencyResolver devem retornar uma instância do serviço ou uma coleção de serviço.
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interface fornece um controle mais refinado sobre como exibir páginas são instanciadas por meio de injeção de dependência. As classes que implementam **IViewPageActivator** interface pode criar instâncias de exibição usando informações de contexto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Criar o /**fábricas** pasta na pasta raiz do projeto.
2. Incluir **CustomViewPageActivator.cs** à sua solução de **/fontes ativos/** para **fábricas** pasta. Para fazer isso, clique com botão direito do **/Factories** pasta, selecione **adicionar | Item existente** e, em seguida, selecione **CustomViewPageActivator.cs**. Essa classe implementa o **IViewPageActivator** interface para manter o contêiner do Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** é responsável por gerenciar a criação de um modo de exibição usando um contêiner de unidade.
3. Incluir **UnityDependencyResolver.cs** arquivo **fontes/ativos** para **/Factories** pasta. Para fazer isso, clique com botão direito do **/Factories** pasta, selecione **adicionar | Item existente** e, em seguida, selecione **UnityDependencyResolver.cs** arquivo.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** classe é um DependencyResolver personalizado para Unity. Quando um serviço não foi encontrado dentro do contêiner do Unity, o resolvedor de base é invocated.

Na tarefa a seguir serão registradas ambas as implementações para saber o local dos serviços e os modos de exibição do modelo.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tarefa 3 - registrar-se para injeção de dependência no contêiner do Unity

Nesta tarefa, você colocará todas as coisas anteriores para fazer a injeção de dependência de trabalho.

Até agora, sua solução tem os seguintes elementos:

- Um **procurar** exibição herda de **MyBaseClass** e consome **MessageService**.
- Uma classe intermediária -**MyBaseClass**-que foi declarada para a interface do serviço de injeção de dependência.
- Um serviço - **MessageService** - e sua interface **IMessageService**.
- Um resolvedor de dependência personalizada para Unity - **UnityDependencyResolver** -que lida com a recuperação de serviço.
- Um ativador de página de exibição - **CustomViewPageActivator** -que cria a página.

Inserir **procurar** modo de exibição, você registrar o resolvedor de dependência personalizada no contêiner do Unity.

1. Abra **Bootstrapper.cs** arquivo.
2. Registrar uma instância de **MessageService** no contêiner do Unity para inicializar o serviço:

    (Código de trecho - *serviço de mensagens de registro do ASP.NET dependência injeção laboratório - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Adicione uma referência a **MvcMusicStore.Factories** namespace.

    (Código de trecho - *ASP.NET dependência injeção laboratório - Ex02 - fábricas Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrar **CustomViewPageActivator** como um ativador de página de exibição no contêiner do Unity:

    (Código de trecho - *CustomViewPageActivator de registro do ASP.NET dependência injeção laboratório - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Substitua o resolvedor de dependência padrão ASP.NET MVC 4 com uma instância do **UnityDependencyResolver**. Para fazer isso, substitua **inicialização** método conteúdo com o código a seguir:

    (Código de trecho - *resolvedor de dependência de atualização do ASP.NET dependência injeção laboratório - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC fornece uma classe do resolvedor de dependência padrão. Para trabalhar com os resolvedores de dependência personalizada que você criou para unity, esse resolvedor deve ser substituído.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarefa 4 - executando o aplicativo

Nesta tarefa, você executará o aplicativo para verificar se o navegador do repositório consome o serviço e mostra a imagem e a mensagem recuperada:

1. Pressione **F5** para executar o aplicativo.
2. Clique em **Rock** dentro do Menu gêneros e veja como o **MessageService** foi inserida para o modo de exibição e carregar a mensagem de boas-vinda e a imagem. Neste exemplo, podemos inserir a &quot; **Rock**&quot;:

    ![Repositório de música MVC - exibição injeção](aspnet-mvc-4-dependency-injection/_static/image10.png "repositório de música MVC - injeção de exibição")

    *Repositório de música MVC - injeção de exibição*
3. Feche o navegador.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Exercício 3: Injetando filtros de ação

No laboratório prático anterior **filtros de ação personalizado** você trabalhou com injeção e personalização de filtros. Neste exercício, você aprenderá como inserir filtros com injeção de dependência usando o contêiner do Unity. Para fazer isso, você adicionará à solução de armazenamento de música um filtro de ação personalizada que será a atividade do site de rastreamento.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tarefa 1 – incluindo o filtro de rastreamento na solução

Nesta tarefa, você incluirá no repositório de música um filtro de ação personalizada para eventos de rastreamento. Como o filtro de ação personalizada conceitos já são tratados no laboratório anterior &quot;filtros de ação personalizado&quot;, inclua apenas a classe de filtro da pasta ativos deste laboratório e, em seguida, criar um provedor de filtro para Unity:

1. Abra o **começar** solução localizada no **Source\Ex03 - injetando ação Filter\Begin** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

    1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
    > 
    > Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluir **TraceActionFilter.cs** arquivo **fontes/ativos** para **/filtra** pasta.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Esse filtro de ação personalizada executa o rastreamento do ASP.NET. Você pode verificar &quot;local do ASP.NET MVC 4 e filtros de ação dinâmicos&quot; laboratório para mais de referência.
3. Adicione a classe vazia **FilterProvider.cs** para o projeto na pasta   **/filtros.**
4. Adicionar o **System.Web.Mvc** e **Microsoft.Practices.Unity** namespaces em **FilterProvider.cs**.

    (Código de trecho - *provedor ASP.NET dependência injeção laboratório - Ex03 - filtro adicionando Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Tornar a classe herda de **IFilterProvider** Interface.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Adicionar um **IUnityContainer** propriedade o **FilterProvider** classe e, em seguida, crie um construtor de classe para atribuir o contêiner.

    (Código de trecho - *construtor do provedor de filtro ASP.NET dependência injeção laboratório - Ex03 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > O construtor de classe de provedor de filtro não está criando um **novo** dentro do objeto. O contêiner é passado como um parâmetro e a dependência é resolvida Unity.
7. No **FilterProvider** classe, implemente o método **GetFilters** de **IFilterProvider** interface.

    (Código de trecho - *GetFilters de provedor de filtro do ASP.NET dependência injeção laboratório - Ex03 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tarefa 2: registrar e ativar o filtro

Nesta tarefa, você habilitará o controle do site. Para fazer isso, você registrará o filtro na **Bootstrapper.cs BuildUnityContainer** método para iniciar o rastreamento:

1. Abra **Web. config** localizado na raiz do projeto e habilitar o controle de rastreamento no grupo de System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Abra **Bootstrapper.cs** na raiz do projeto.
3. Adicione uma referência para o **MvcMusicStore.Filters** namespace.

    (Código de trecho - *injeção de dependência ASP.NET laboratório - Ex03 - Bootstrapper adicionando Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Selecione o **BuildUnityContainer** método e registrar o filtro no contêiner do Unity. Você precisará registrar o provedor de filtro, bem como o filtro de ação.

    (Código de trecho - *ASP.NET dependência injeção laboratório - Ex03 - Register FilterProvider e ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarefa 3: executando o aplicativo

Nesta tarefa, você executar o aplicativo e que o filtro de ação personalizado está rastreando a atividade de teste:

1. Pressione **F5** para executar o aplicativo.
2. Clique em **Rock** dentro do Menu gêneros. Você pode procurar mais gêneros se desejar.

    ![Repositório de música](aspnet-mvc-4-dependency-injection/_static/image11.png "repositório de música")

    *Loja de Música*
3. Navegue até **/Trace.axd** para ver o rastreamento de aplicativo de página e, em seguida, clique em **exibir detalhes**.

    ![Log de rastreamento de aplicativo](aspnet-mvc-4-dependency-injection/_static/image12.png "Log de rastreamento de aplicativo")

    *Log de rastreamento de aplicativo*

    ![Rastreamento de aplicativo - detalhes da solicitação](aspnet-mvc-4-dependency-injection/_static/image13.png "aplicativo de rastreamento - detalhes da solicitação")

    *Rastreamento de aplicativo - detalhes da solicitação*
4. Feche o navegador.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você aprendeu como usar injeção de dependência no ASP.NET MVC 4, integrando Unity usando um pacote do NuGet. Para fazer isso, você usou a injeção de dependência dentro de controladores, exibições e filtros de ação.

Os conceitos a seguir foram abordados:

- Recursos de injeção de dependência do ASP.NET MVC 4
- Integração do Unity usando o pacote do NuGet Unity.Mvc3
- Injeção de dependência em controladores
- Injeção de dependência em modos de exibição
- Injeção de dependência de filtros de ação

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express para o bloco de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice b: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-dependency-injection/_static/image19.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](aspnet-mvc-4-dependency-injection/_static/image20.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-dependency-injection/_static/image21.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-dependency-injection/_static/image22.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1. Clique com botão direito em que você deseja inserir o trecho de código.

1. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
2. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-dependency-injection/_static/image23.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-dependency-injection/_static/image24.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*
