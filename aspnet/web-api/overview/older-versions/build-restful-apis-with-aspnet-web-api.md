---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Criar APIs RESTful com a API Web ASP.NET | Microsoft Docs
author: rick-anderson
description: Nos últimos anos, tornou-se claro que o HTTP não é apenas para servir as páginas HTML. Também é uma plataforma poderosa para a criação de APIs da Web, usando um punhado de s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363514"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Criar APIs RESTful com a API da Web ASP.NET
====================
por [Web Camps equipe](https://twitter.com/webcamps)

> Nos últimos anos, tornou-se claro que o HTTP não é apenas para servir as páginas HTML. Também é uma plataforma poderosa para a criação de APIs da Web, usando um punhado de verbos (GET, POST e assim por diante) além de alguns conceitos simples, como *URIs* e *cabeçalhos*. API Web ASP.NET é um conjunto de componentes que simplificam a programação HTTP. Porque ele se baseia no tempo de execução do ASP.NET MVC, API Web manipula automaticamente os detalhes de baixo nível de transporte de HTTP. Ao mesmo tempo, a API da Web naturalmente expõe o modelo de programação HTTP. Na verdade, é uma meta de API da Web *não* abstraem a realidade de HTTP. Como resultado, a API Web é flexível e fácil de estender. Neste laboratório prático, você usará a API da Web para criar uma API REST simples para um aplicativo do Gerenciador de contatos. Você também criará um cliente para consumir a API. O estilo de arquitetura REST provou para ser uma maneira eficiente de aproveitar HTTP – embora certamente isso não seja a abordagem só é válida para HTTP. O Gerenciador de contatos irá expor o RESTful para listagem, adicionando e removendo contatos, entre outros. Este laboratório requer um entendimento básico de HTTP, REST e pressupõe que você tenha um conhecimento prático básico de HTML, JavaScript e jQuery.
> 
> > [!NOTE]
> > O site da Web do ASP.NET tem uma área dedicada a estrutura de API Web ASP.NET no [ https://asp.net/web-api ](https://asp.net/web-api). Este site continuará fornecer informações mais recentes, exemplos e notícias relacionadas à API da Web, portanto, verifique-lo com frequência se você gostaria de nos aprofundar na arte de criação de APIs Web personalizadas disponíveis para praticamente qualquer estrutura de desenvolvimento ou dispositivo.
> > 
> > API Web ASP.NET, semelhante ao ASP.NET MVC 4, tem grande flexibilidade em termos de separar a camada de serviço dos controladores, permitindo que você usar várias das estruturas de injeção de dependência disponíveis razoavelmente fácil. Há um bom exemplo no MSDN que mostra como usar Ninject para injeção de dependência em um projeto de API Web ASP.NET que você pode baixá-lo partir [aqui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Implementar uma API Web RESTful
- Chamar a API de um cliente HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O exemplo a seguir é necessário para concluir este laboratório prático:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>
### <a name="setup"></a>Configuração

**Instalando os trechos de código**

Para sua conveniência, grande parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código executados **.\Source\Setup\CodeSnippets.vsi** arquivo.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [usando trechos de código de r: Apêndice](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui o exercício a seguir:

1. [Exercício 1: Criar uma API de Web somente leitura](#Exercise1)
2. [Exercício 2: Criar uma API da Web de leitura/gravação](#Exercise2)
3. [Exercício 3: Consumir a API da Web de um cliente HTML](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Exercício 1: Criar uma API de Web somente leitura

Neste exercício, você implementará os métodos GET somente leitura para o Gerenciador de contatos.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Tarefa 1 - criar o projeto de API

Nesta tarefa, você usará os novos modelos de projeto de web do ASP.NET para criar um aplicativo de web API da Web.

1. Execute **Visual Studio 2012 Express para Web**, para fazer isso, vá para **inicie** e digite **VS Express para Web** , em seguida, pressione **Enter**.
2. Dos **arquivo** menu, selecione **novo projeto**. Selecione o **Visual c# | Web** tipo de exibição de árvore de tipo de projeto do projeto, em seguida, selecione o **aplicativo Web do ASP.NET MVC 4** tipo de projeto. Defina o projeto **nome** para *ContactManager* e o **nome da solução** para *começar*, em seguida, clique em **Okey**.

    ![Criando um novo projeto de aplicativo do ASP.NET MVC 4.0 da Web](build-restful-apis-with-aspnet-web-api/_static/image1.png "criando um novo projeto de aplicativo do ASP.NET MVC 4.0 da Web")

    *Criando um novo projeto de aplicativo do ASP.NET MVC 4.0 da Web*
3. No diálogo de tipo de projeto do ASP.NET MVC 4, selecione a **API Web** tipo de projeto. Clique em **OK**.

    ![Especifica o tipo de projeto de API da Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "especificando o tipo de projeto de API da Web")

    *Especifica o tipo de projeto de API da Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Tarefa 2: criar os controladores de API do Gerenciador de contatos

Nesta tarefa, você criará as classes do controlador no qual os métodos de API residirá.

1. Exclua o arquivo chamado **ValuesController.cs** dentro **controladores** pasta do projeto.
2. Clique com botão direito do **controladores** pasta no projeto e selecione **adicionar | Controlador** no menu de contexto.

    ![Adicionando um novo controlador ao projeto](build-restful-apis-with-aspnet-web-api/_static/image3.png "adicionando um novo controlador ao projeto")

    *Adicionando um novo controlador ao projeto*
3. No **Adicionar controlador** caixa de diálogo que aparece, selecione **controlador API vazio** no menu modelo. Nomeie a classe do controlador **ContactController**. Em seguida, clique em **adicionar.**

    ![Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API da Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API da Web")

    *Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API da Web*
4. Adicione o seguinte código para o **ContactController**.

    (Código de trecho de código – *laboratório de API da Web - Ex01 - obter método API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Pressione **F5** para depurar o aplicativo. A home page padrão para um projeto de API da Web deve aparecer.

    ![A home page padrão de um aplicativo de API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "a home page padrão de um aplicativo de API Web ASP.NET")

    *A home page padrão de um aplicativo de API Web ASP.NET*
6. Na janela do Internet Explorer, pressione a **F12** tecla para abrir o **ferramentas de desenvolvedor** janela. Clique o **rede** guia e, em seguida, clique no **iniciar captura** botão para começar a capturar o tráfego de rede para a janela.

    ![Abrindo a guia rede e iniciar captura de rede](build-restful-apis-with-aspnet-web-api/_static/image6.png "abrindo a guia rede e iniciar captura de rede")

    *Abrindo a guia rede e iniciar a captura de rede*
7. Anexar a URL na barra de endereços do navegador com **/api/contact** e pressione enter. Os detalhes de transmissão aparecerá na janela de captura de rede. Observe que o tipo MIME da resposta **application/json**. Isso demonstra como o formato de saída padrão é JSON.

    ![Exibindo a saída da solicitação de API da Web no modo de exibição de rede](build-restful-apis-with-aspnet-web-api/_static/image7.png "exibindo a saída da solicitação de API da Web no modo de exibição de rede")

    *Exibindo a saída da solicitação de API da Web no modo de exibição de rede*

    > [!NOTE]
    > Comportamento de padrão do Internet Explorer 10 no momento será perguntar se o usuário gostaria de salvar ou abrir o fluxo resultante da chamada à API Web. A saída será um arquivo de texto que contém o resultado JSON da chamada de URL da API Web. Não cancele a caixa de diálogo para que seja possível assistir o conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.
8. Clique o **ir para a exibição detalhada** botão para ver mais detalhes sobre a resposta dessa chamada de API.

    ![Alternar para a exibição detalhada](build-restful-apis-with-aspnet-web-api/_static/image8.png "alternar para a exibição de detalhes")

    *Alternar para a exibição detalhada*
9. Clique o **corpo da resposta** guia para exibir o texto de resposta JSON real.

    ![Exibir o JSON de saída de texto no monitor de rede](build-restful-apis-with-aspnet-web-api/_static/image9.png "exibir o JSON de saída de texto no monitor de rede")

    *Exibindo o texto de saída JSON no monitor de rede*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Tarefa 3: Criando modelos de contato e aumente o controlador de contato

Nesta tarefa, você criará as classes do controlador no qual os métodos de API residirá.

1. Clique com botão direito do **modelos** pasta e selecione **adicionar | Classe...**  no menu de contexto.

    ![Adicionando um novo modelo para o aplicativo web](build-restful-apis-with-aspnet-web-api/_static/image10.png "adicionando um novo modelo para o aplicativo web")

    *Adicionando um novo modelo para o aplicativo web*
2. No **Adicionar Novo Item** caixa de diálogo, nomeie o novo arquivo **Contact.cs** e clique em **adicionar.**

    ![Criar o novo arquivo de classe de contato](build-restful-apis-with-aspnet-web-api/_static/image11.png "criar o novo arquivo de classe do contato")

    *Criar o novo arquivo de classe do contato*
3. Adicione o seguinte código realçado para o **entre em contato com** classe.

    (Código de trecho de código – *classe de contato do laboratório de API - Ex01 - Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. No **ContactController** classe, selecione a palavra **cadeia de caracteres** na definição de método da **obter** método e digite a palavra *entre em contato com*. Depois que o word é digitado, um indicador será exibido no início da palavra **entre em contato com**. O mantenha pressionada a **Ctrl** da chave e pressione a tecla de ponto (.) ou clique no ícone de usando o mouse para abrir a caixa de diálogo de Ajuda no editor de código para preencher automaticamente a **usando** para os modelos de diretiva namespace.

    ![Usando a assistência do Intellisense para declarações de namespace](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Usando a assistência do Intellisense para declarações de namespace*
5. Modificar o código para o **obter** , de modo que ele retorna uma matriz de instâncias de modelo de contato.

    (Código de trecho de código – *laboratório de API da Web - Ex01 - retornando uma lista de contatos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Pressione **F5** para depurar o aplicativo web no navegador. Para exibir as alterações feitas à saída de resposta da API, execute as seguintes etapas.

   1. Depois que o navegador é aberto, pressione **F12** se as ferramentas de desenvolvedor ainda não estiverem abertas.
   2. Clique o **rede** guia.
   3. Pressione a **iniciar captura** botão.
   4. Adicione o sufixo de URL **/api/contact** para a URL na barra de endereços e pressione a **Enter** chave.
   5. Pressione a **ir para a exibição detalhada** botão.
   6. Selecione o **corpo da resposta** guia. Você deve ver uma cadeia de caracteres JSON que representa o formato serializado de uma matriz de instâncias de contato.

      ![Saída de uma chamada de método de API da Web complexa serializado de JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "saída de uma chamada de método de API da Web complexa serializado de JSON")

      *Saída de uma chamada de método de API da Web complexa serializado de JSON*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Tarefa 4 - funcionalidade de extração em uma camada de serviço

Essa tarefa demonstrará como extrair a funcionalidade em uma camada de serviço para torná-lo mais fácil para os desenvolvedores a separar sua funcionalidade de serviço da camada de controlador, permitindo assim que a capacidade de reutilização dos serviços que realmente fazem o trabalho.

1. Crie uma nova pasta na raiz da solução e nomeie- **Services**. Para fazer isso, clique com botão direito **ContactManager** projeto, selecione **Add** | **nova pasta**, nomeie-o *serviços*.

    ![Criando pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image14.png "pasta criando serviços")

    *Criando a pasta de serviços*
2. Clique com botão direito do **Services** pasta e selecione **adicionar | Classe...**  no menu de contexto.

    ![Adicionando uma nova classe para a pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image15.png "adicionando uma nova classe para a pasta de serviços")

    *Adicionando uma nova classe para a pasta de serviços*
3. Quando o **Adicionar Novo Item** caixa de diálogo aparece, nomeie a nova classe **ContactRepository** e clique em **adicionar**.

    ![Criando um arquivo de classe para conter o código para a camada de serviço de repositório de contato](build-restful-apis-with-aspnet-web-api/_static/image16.png "criando um arquivo de classe para conter o código para a camada de serviço de repositório de contato")

    *Criando um arquivo de classe para conter o código para a camada de serviço de repositório de contato*
4. Adicionar uma diretiva para o **ContactRepository.cs** arquivo para incluir o namespace de modelos.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Adicione o seguinte código realçado para o **ContactRepository.cs** arquivo para implementar o método GetAllContacts.

    (Código de trecho de código – *Web repositório contato do laboratório de API - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Abra o **ContactController.cs** arquivo caso ele não ainda estiver aberto.
7. Adicione a seguinte instrução using à seção de declaração de namespace do arquivo.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Adicione o seguinte código realçado para o **ContactController.cs** classe para adicionar um campo particular para representar a instância do repositório, para que o restante da classe que os membros podem fazer usar da implementação do serviço.

    (Código de trecho de código – *controlador de contato do laboratório de API - Ex01 - Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Alterar o **obter** , de modo que ele faz ao uso do serviço de repositório de contato.

    (Código de trecho de código – *laboratório de API da Web - Ex01 - retornando uma lista de contatos por meio do repositório*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Colocar um ponto de interrupção a **ContactController**do **obter** definição de método.

   ![Adicionar pontos de interrupção ao controlador de contato](build-restful-apis-with-aspnet-web-api/_static/image17.png "adicionar pontos de interrupção ao controlador de contato")

   *Adicionar pontos de interrupção ao controlador de contato*
11. Pressione **F5** para executar o aplicativo.
12. Quando o navegador é aberto, pressione **F12** para abrir as ferramentas de desenvolvedor.
13. Clique o **rede** guia.
14. Clique o **iniciar captura** botão.
15. Anexar a URL na barra de endereços com o sufixo **/api/contact** e pressione **Enter** para carregar o controlador da API.
16. Visual Studio 2012 deve ser interrompido uma vez **obter** método começa a ser executada.

   ![Quebra no método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "quebra no método Get")

   *Quebra no método Get*
17. Pressione **F5** para continuar.
18. Volte para o Internet Explorer se ele não ainda estiver em foco. Observe a janela de captura de rede.

    ![Modo de exibição no Internet Explorer, mostrando os resultados da chamada à API da Web de rede](build-restful-apis-with-aspnet-web-api/_static/image19.png "exibição no Internet Explorer, mostrando os resultados da chamada à API da Web de rede")

    *Exibição de rede no Internet Explorer, mostrando os resultados da chamada à API da Web*
19. Clique o **ir para a exibição detalhada** botão.
20. Clique o **corpo da resposta** guia. Observe a saída JSON da chamada à API e como ele representa dois contatos recuperados pela camada de serviço.

    ![Exibindo a saída JSON da API Web na janela de ferramentas de desenvolvedor](build-restful-apis-with-aspnet-web-api/_static/image20.png "exibindo a saída JSON da API Web na janela de ferramentas de desenvolvedor")

    *Exibindo a saída JSON da API Web na janela de ferramentas de desenvolvedor*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Exercício 2: Criar uma API da Web de leitura/gravação

Neste exercício, você implementará o POST e PUT métodos para o Gerenciador de contato para habilitá-lo com os recursos de edição de dados.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Tarefa 1 - abrindo o projeto de API da Web

Nesta tarefa, você preparará aprimorar o projeto de API da Web criado no Exercício 1 para que ele pode aceitar a entrada do usuário.

1. Execute **Visual Studio 2012 Express para Web**, para fazer isso, vá para **inicie** e digite **VS Express para Web** , em seguida, pressione **Enter**.
2. Abra o **começar** solução localizado em **origem/Ex02-ReadWriteWebAPI/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
3. Abra o **Services/ContactRepository.cs** arquivo.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Tarefa 2 - adicionando recursos de persistência de dados para a implementação de repositório de contato

Nesta tarefa, você irá aumentar a classe ContactRepository do projeto de API da Web criado no Exercício 1 para que ele possa persistir e aceitar a entrada do usuário e as novas instâncias de contato.

1. Adicione a seguinte constante para o **ContactRepository** classe para representar o nome do web server cache item nome da chave mais tarde neste exercício.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Adicione um construtor para o **ContactRepository** que contém o código a seguir.

    (Código de trecho de código – *Web API laboratório - Ex02 - repositório contato construtor*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modificar o código para o **GetAllContacts** método conforme demonstrado a seguir.

    (Código de trecho de código – *laboratório de API da Web - Ex02 - obter todos os contatos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Este exemplo é para fins de demonstração e usará cache do servidor web como um meio de armazenamento, para que os valores serão estar disponível para vários clientes simultaneamente, em vez de usar um mecanismo de armazenamento de sessão ou um tempo de vida de armazenamento de solicitação. Um pode usar o Entity Framework, armazenamento XML ou qualquer outra variedade no lugar de cache do servidor web.
4. Implementar um novo método chamado **SaveContact** para o **ContactRepository** classe para fazer o trabalho de salvar um contato. O **SaveContact** método deve adotar uma única **contato** valor de parâmetro e retornar um booleano indicando o êxito ou falha.

    (Código de trecho de código – *Web API laboratório - Ex02 - Implementando o método SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Exercício 3: Consumir a API da Web de um cliente HTML

Neste exercício, você criará um cliente HTML para chamar a API da Web. Esse cliente facilitam a troca de dados com a API da Web usando JavaScript e exibirá os resultados em um navegador da web usando marcação HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Tarefa 1 - modificando a exibição de índice para fornecer uma GUI para exibir contatos

Nesta tarefa, você modificará a exibição de índice padrão do aplicativo web para dar suporte a necessidade de exibir a lista de contatos existentes em um navegador HTML.

1. Abra **Visual Studio 2012 Express para Web** se ele não ainda estiver aberto.
2. Abra o **começar** solução localizado em **origem/Ex03-ConsumingWebAPI/início/** pasta. Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
3. Abra o **index. cshtml** arquivo localizado em **exibições/inicial** pasta.
4. Substitua o código HTML dentro do elemento div com id **corpo** para que ele se parece com o código a seguir.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Adicione o seguinte código Javascript na parte inferior do arquivo para executar a solicitação HTTP para a API da Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Abra o **ContactController.cs** arquivo caso ele não ainda estiver aberto.
7. Colocar um ponto de interrupção na **Obtenha** método o **ContactController** classe.

    ![Colocar um ponto de interrupção no método Get do controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "colocando um ponto de interrupção no método Get do controlador de API")

    *Colocar um ponto de interrupção no método Get do controlador de API*
8. Pressione **F5** para executar o projeto. O navegador carregará o documento HTML.

    > [!NOTE]
    > Certifique-se de que você está navegando para a URL raiz do seu aplicativo.
9. Depois que a página for carregada e o JavaScript é executado, o ponto de interrupção será atingido e fará uma pausa a execução do código no controlador.

    ![Depuração nas chamadas de API da Web usando o VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "depuração nas chamadas de API da Web usando o VS Express para Web")

    *Depuração para a chamada de API da Web usando o Visual Studio 2012 Express para Web*
10. Remover o ponto de interrupção e pressione **F5** ou a barra de ferramentas depuração **continuar** botão para continuar carregando o modo de exibição no navegador. Após a conclusão da chamada à API Web você deve ver os contatos retornados da API Web chamar exibidos como itens de lista no navegador.

    ![Resultados da chamada à API exibida no navegador, como itens de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "resultados da chamada à API exibida no navegador, como itens de lista")

    *Resultados da chamada à API exibida no navegador, como itens de lista*
11. Pare a depuração.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Tarefa 2 – modificar a exibição de índice para fornecer uma interface gráfica para criação de contatos

Nesta tarefa, você continuará modificar a exibição de índice do aplicativo MVC. Um formulário será adicionado à página HTML que irão capturar a entrada do usuário e enviá-lo para a API da Web para criar um novo contato e um novo método de controlador de API da Web será criado para coletar a data da GUI.

1. Abra o **ContactController.cs** arquivo.
2. Adicione um novo método à classe do controlador nomeado **Post** conforme mostrado no código a seguir.

    (Código de trecho de código – *laboratório API - Ex03 - Post método Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Abra o **index. cshtml** arquivo no Visual Studio, caso ele não ainda estiver aberto.
4. Adicione o código HTML a seguir ao arquivo logo após a lista não ordenada que você adicionou na tarefa anterior.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Dentro do elemento de script na parte inferior do documento, adicione o seguinte código realçado para manipular eventos de clique de botão, que postará os dados para a API Web usando uma chamada HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. Na **ContactController.cs**, coloque um ponto de interrupção na **Post** método.
7. Pressione **F5** para executar o aplicativo no navegador.
8. Depois que a página é carregada no navegador, digite um novo nome de contato e Id e clique no **salvar** botão.

    ![O documento de cliente HTML carregado no navegador](build-restful-apis-with-aspnet-web-api/_static/image24.png "o documento de cliente HTML carregado no navegador")

    *O documento HTML do cliente carregada no navegador*
9. Quando a janela do depurador interrompe **Post** método, examine as propriedades da **entre em contato com** parâmetro. Os valores devem corresponder os dados inseridos no formulário.

    ![O objeto de contato que está sendo enviado para a API da Web do cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "objeto entre em contato com o que está sendo enviado para a API da Web do cliente")

    *O objeto de contato que está sendo enviado para a API da Web do cliente*
10. Etapa por meio do método no depurador até que o **resposta** variável foi criada. Após a inspeção na **Locals** janela no depurador, você verá que todas as propriedades foram definidas.

   ![A resposta após a criação no depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "a resposta após a criação no depurador")

   *A resposta após a criação no depurador*
11. Se você pressionar **F5** ou clique em **continuar** no depurador, a solicitação será concluída. Depois que você alternar de volta para o navegador, o novo contato foi adicionado à lista de contatos armazenados pelo **ContactRepository** implementação.

   ![O navegador reflete a criação bem-sucedida da nova instância de contato](build-restful-apis-with-aspnet-web-api/_static/image27.png "navegador reflete a criação bem-sucedida da nova instância de contato")

   *O navegador reflete a criação bem-sucedida da nova instância de contato*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo do Azure seguinte [apêndice c: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Este laboratório tem apresentou a você para a nova estrutura de API Web ASP.NET e a implementação de APIs da Web RESTful, usando a estrutura. A partir daqui, você pode criar um novo repositório que facilita a persistência de dados usando qualquer número de mecanismos e conectar esse serviço em vez do simples fornecido como um exemplo neste laboratório. API da Web dá suporte a um número de recursos adicionais, como habilitar a comunicação de clientes não-HTML escritos em qualquer linguagem que dá suporte a HTTP e JSON ou XML. A capacidade de hospedar uma API da Web fora de um aplicativo web típico também é possível, bem como é a capacidade de criar seus próprios formatos de serialização.

O site da Web do ASP.NET tem uma área dedicada a estrutura de API Web ASP.NET no [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Este site continuará fornecer informações mais recentes, exemplos e notícias relacionadas à API da Web, portanto, verifique-lo com frequência se você gostaria de nos aprofundar na arte de criação de APIs Web personalizadas disponíveis para praticamente qualquer estrutura de desenvolvimento ou dispositivo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apêndice a: usar trechos de código

Com trechos de código, você tem todo o código que necessário ao seu alcance. O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usar trechos de código do Visual Studio para inserir código em seu projeto](build-restful-apis-with-aspnet-web-api/_static/image28.png "trechos de código usando o Visual Studio para inserir código em seu projeto")

*Usar trechos de código do Visual Studio para inserir código em seu projeto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Para adicionar um trecho de código usando o teclado (somente c#)

1. Coloque o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho de código (sem espaços ou hifens).
3. Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

    ![Comece a digitar o nome do trecho](build-restful-apis-with-aspnet-web-api/_static/image29.png "comece a digitar o nome do trecho de código")

    *Comece a digitar o nome do trecho de código*

    ![Pressione Tab para selecionar o trecho de código realçado](build-restful-apis-with-aspnet-web-api/_static/image30.png "pressione Tab para selecionar o trecho de código realçado")

    *Pressione Tab para selecionar o trecho de código realçado*

    ![Pressione Tab novamente e o trecho de código serão expandido](build-restful-apis-with-aspnet-web-api/_static/image31.png "pressione Tab novamente e o trecho de código serão expandido")

    *Pressione Tab novamente e o trecho de código serão expandido*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)

1. Clique com botão direito no qual você deseja inserir o trecho de código.
2. Selecione **Inserir trecho** seguido **Meus trechos de código**.
3. Selecione o trecho relevante na lista, clicando nele.

    ![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")

    *Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet*

    ![Escolher o trecho relevante na lista, clicando nele](build-restful-apis-with-aspnet-web-api/_static/image33.png "escolher o trecho relevante na lista, clicando nele")

    *Escolher o trecho relevante na lista, clicando nele*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apêndice b: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instalar o Visual Studio Express")

    *Instalar o Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação for concluído.

    ![Progresso da instalação](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalação concluída*
7. Clique em **Exit** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco da Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express para o bloco da Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice c: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal do Azure e publicar o aplicativo que você obteve seguindo o ambiente de laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarefa 1 - criar um novo Site do Portal do Azure

1. Vá para o [Portal de gerenciamento](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego aumenta. Você pode se inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal*
2. Clique em **New** na barra de comandos.

    ![Criando um novo Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "criando um novo Site")

    *Criando um novo Site*
3. Clique em **Compute** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site da web e clique em **Criar Site**.

    > [!NOTE]
    > O Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído no Azure de fora do portal. Ele não inclui as etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "criando um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando até o novo site](build-restful-apis-with-aspnet-web-api/_static/image42.png "navegando até o novo site da web")

    *Navegando até o novo site da web*

    ![Site da Web em execução](build-restful-apis-with-aspnet-web-api/_static/image43.png "site da Web em execução")

    *Site da Web em execução*
6. Volte para o portal e clique no nome do site da web sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](build-restful-apis-with-aspnet-web-api/_static/image44.png "abrir as páginas de gerenciamento do site da web")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **Dashboard** página na **visão geral rápida** seção, clique no **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para conectar e autenticar em relação a cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web do Azure.

    ![Baixando o site da web de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image45.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Ainda mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web do Azure do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image46.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo faz uso do SQL Server você precisará criar um servidor de banco de dados SQL de bancos de dados. Se você quiser implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL na sua assinatura no portal de gerenciamento do Azure em **bancos de dados Sql** | **servidores** | **painel do servidor**. Se você não tiver um servidor criado, você pode criar uma usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados ainda, ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço de IP do cliente atual** e cole-o na **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique em de ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) botão.

    ![Adicionar endereço IP do cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Adicionar endereço IP do cliente*
3. Uma vez a **endereço IP do cliente** é adicionado para endereços IP, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confirmar alterações*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3 – publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Gerenciador de soluções**, clique com botão direito no projeto de site da web e selecione **publicar**.

    ![O aplicativo de publicação](build-restful-apis-with-aspnet-web-api/_static/image51.png "publicando o aplicativo")

    *Publicar o site da web*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importando o perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image52.png "importando o perfil de publicação")

    *Importando o perfil de publicação*
3. Clique em **validar Conexão**. Depois que a validação estiver concluída, clique em **próxima**.

    > [!NOTE]
    > A validação é concluída quando você vir uma marca de seleção verde aparecer ao lado do botão Validar Conexão.

    ![Validar conexão](build-restful-apis-with-aspnet-web-api/_static/image53.png "validar conexão")

    *Validando a conexão*
4. No **as configurações** página na **bancos de dados** seção, clique no botão ao lado da caixa de texto do sua conexão banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Na **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Na **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de caracteres de conexão de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](build-restful-apis-with-aspnet-web-api/_static/image56.png "criando a cadeia de caracteres de banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **versão prévia** , clique em **publicar**.

    ![Publicando o aplicativo web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publicando o aplicativo web")

    *Publicar o aplicativo da web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

    ![Aplicativo publicado no Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplicativo publicado no Windows Azure")

    *Aplicativo publicado no Azure*
