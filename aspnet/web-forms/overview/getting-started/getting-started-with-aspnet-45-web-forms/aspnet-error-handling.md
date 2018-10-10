---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Tratamento de erros do ASP.NET | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: af5e5a9c8d211b07b57aa50238b02cabe249aef8
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911834"
---
<a name="aspnet-error-handling"></a>Tratamento de erros do ASP.NET
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para incluir o tratamento de erros e log de erros. Tratamento de erro permitirá que o aplicativo tratar erros normalmente e exibir mensagens de erro adequadamente. Log de erros permitirá que você localizar e corrigir erros que ocorreram. Este tutorial se baseia no tutorial anterior, "URL de roteamento" e faz parte da série de tutoriais de Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como adicionar a configuração do aplicativo de tratamento de erro global.
- Como adicionar tratamento no aplicativo, página e níveis de código de erro.
- Como registrar erros para revisão posterior.
- Como exibir mensagens de erro que não comprometam a segurança.
- Como implementar o log de erros de módulos de log de erros e manipuladores (ELMAH).

## <a name="overview"></a>Visão geral

Aplicativos ASP.NET devem ser capazes de lidar com erros que ocorrem durante a execução de uma maneira consistente. O ASP.NET usa o common language runtime (CLR), que fornece uma maneira de notificar os aplicativos de erros de maneira uniforme. Quando ocorre um erro, uma exceção é lançada. Uma exceção é qualquer erro, condição ou um comportamento inesperado que encontra um aplicativo.

No .NET Framework, uma exceção é um objeto que herda o `System.Exception` classe. Uma exceção é lançada de uma área do código em que ocorreu um problema. A exceção é passada na pilha de chamada para um local em que o aplicativo fornece código para tratar a exceção. Se o aplicativo não trata a exceção, o navegador é forçado para exibir os detalhes do erro.

Como prática recomendada, tratar erros no nível do código em `Try` / `Catch` / `Finally` blocos dentro de seu código. Tente colocar esses blocos de forma que o usuário pode corrigir problemas no contexto em que elas ocorrem. Se os blocos de tratamento de erros são muito distantes de onde ocorreu o erro, ele se torna mais difícil fornecer aos usuários as informações necessárias corrigir o problema.

### <a name="exception-class"></a>Classe de exceção

A classe de exceção é a classe base da qual as exceções herdam. A maioria dos objetos de exceção são instâncias de uma classe derivada da classe de exceção, tais como o `SystemException` classe, o `IndexOutOfRangeException` classe, ou o `ArgumentNullException` classe. A classe de exceção tem propriedades, como o `StackTrace` propriedade, o `InnerException` propriedade e o `Message` propriedade, que fornecem informações específicas sobre o erro que ocorreu.

### <a name="exception-inheritance-hierarchy"></a>Hierarquia de herança de exceção

O tempo de execução tem um conjunto base de exceções que derivam do `SystemException` classe que o tempo de execução gera quando uma exceção é encontrada. A maioria das classes que herdam da classe de exceção, tais como o `IndexOutOfRangeException` classe e o `ArgumentNullException` de classe, não implementam membros adicionais. Portanto, as informações mais importantes para uma exceção podem ser encontradas na hierarquia de exceções, o nome da exceção e as informações contidas na exceção.

### <a name="exception-handling-hierarchy"></a>Hierarquia de tratamento de exceções

Em um aplicativo ASP.NET Web Forms, exceções podem ser tratados com base em uma hierarquia de tratamento específica. Uma exceção pode ser tratada nos seguintes níveis:

- Nível de aplicativo
- Nível de página
- Nível de código

Quando um aplicativo manipula exceções, informações adicionais sobre a exceção que é herdada da classe de exceção geralmente podem recuperadas e exibidas ao usuário. Além de aplicativo, página e nível de código, você também pode tratar exceções no nível de módulo HTTP e usando um manipulador personalizado de IIS.

### <a name="application-level-error-handling"></a>Tratamento de erros de nível de aplicativo

Você pode tratar erros de padrão no nível do aplicativo modificando a configuração do aplicativo ou adicionando um `Application_Error` manipulador na *global. asax* arquivo do seu aplicativo.

Você pode manipular erros padrão e erros de HTTP, adicionando um `customErrors` seção para o *Web. config* arquivo. O `customErrors` seção permite que você especifique uma página padrão que os usuários serão redirecionados para quando ocorre um erro. Ele também permite que você especifique as páginas individuais para erros de código de status específico.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Infelizmente, quando você usa a configuração para redirecionar o usuário para uma página diferente, você não tem os detalhes do erro que ocorreu.

No entanto, você pode interceptar erros que ocorrem em qualquer lugar em seu aplicativo adicionando código para o `Application_Error` manipulador na *global. asax* arquivo.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Manipulação de eventos de erro de nível de página

Um manipulador de nível de página retorna o usuário para a página em que ocorreu o erro, mas porque as instâncias de controles não são mantidas, não haverá nada na página. Para fornecer os detalhes do erro para o usuário do aplicativo, você deve escrever especificamente os detalhes do erro para a página.

Normalmente você usaria um manipulador de erros de nível de página para registrar erros sem tratamento ou levar o usuário a uma página que pode exibir informações úteis.

Este exemplo de código mostra um manipulador para o evento de erro em uma página da Web do ASP.NET. Esse manipulador captura todas as exceções que ainda não foram tratadas em `try` / `catch` bloqueia na página.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Após você manipular um erro, você deve apagá-lo chamando o `ClearError` método do objeto de servidor (`HttpServerUtility` classe), caso contrário, você verá um erro que ocorreu anteriormente.

### <a name="code-level-error-handling"></a>Tratamento de erros de nível de código

A instrução try-catch consiste em um bloco try seguido por um ou mais cláusulas, que especificam os manipuladores para diferentes exceções de catch. Quando uma exceção é lançada, o common language runtime (CLR) procura a instrução catch que manipula essa exceção. Se o método em execução no momento, não contém um bloco catch, o CLR aborda o método que chamou o método atual e assim por diante, até a pilha de chamada. Se nenhum bloco catch for encontrado, o CLR exibirá uma mensagem de exceção sem tratamento para o usuário e interrompe a execução do programa.

O exemplo de código a seguir mostra uma maneira comum de usar `try` / `catch` / `finally` para lidar com erros.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

No código acima, o bloco try contém o código que precisa ser protegido contra uma possível exceção. O bloco é executado até que uma exceção é lançada ou o bloco for concluído com êxito. Se um `FileNotFoundException` exceção ou um `IOException` ocorrerá uma exceção, a execução é transferida para outra página. Em seguida, o código contido no bloco finally será executado, se ocorreu um erro ou não.

## <a name="adding-error-logging-support"></a>Adição de suporte de registro em log de erro

Antes de adicionar o tratamento de erros em que o aplicativo de exemplo Wingtip Toys, você adicionará o suporte de log de erros com a adição de um `ExceptionUtility` de classe para o *lógica* pasta. Fazendo isso, cada vez que o aplicativo manipula um erro, os detalhes do erro serão adicionados ao arquivo de log de erro.

1. Clique com botão direito do *lógica* pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual c#**  - &gt; **código** grupo de modelos à esquerda. Em seguida, selecione **classe**do meio lista e nomeie-o **ExceptionUtility.cs**.
3. Escolha **Adicionar**. O novo arquivo de classe é exibido.
4. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Quando ocorre uma exceção, a exceção pode ser gravada em um arquivo de log da exceção, chamando o `LogException` método. Esse método usa dois parâmetros, o objeto de exceção e uma cadeia de caracteres que contém detalhes sobre a origem da exceção. O log de exceções é gravado para o *ErrorLog.txt* arquivo na *App\_dados* pasta.

### <a name="adding-an-error-page"></a>Adicionando uma página de erro

No aplicativo de exemplo Wingtip Toys, uma página será usada para exibir os erros. A página de erro é projetada para mostrar uma mensagem de erro segura para usuários do site. No entanto, se o usuário for um desenvolvedor, fazendo com que uma solicitação HTTP que está sendo atendida localmente no computador em que o código reside, detalhes de erro adicionais serão exibidos na página de erro.

1. Clique com botão direito no nome do projeto (**Wingtip Toys**) em **Gerenciador de soluções** e selecione **Add**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual c#**  - &gt; **Web** grupo de modelos à esquerda. Na lista do meio, selecione **Web Form com página mestra**e nomeie-o **ErrorPage**.
3. Clique em **Adicionar**.
4. Selecione o *Master* file como a página mestra e, em seguida, escolha **Okey**.
5. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Substitua o código existente do code-behind (*ErrorPage.aspx.cs*) para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Quando a página de erro é exibida, o `Page_Load` manipulador de eventos é executado. No `Page_Load` manipulador, o local de onde o erro foi tratado pela primeira vez é determinado. Em seguida, o último erro ocorrido é determinado pela chamada a `GetLastError` método do objeto Server. Se a exceção não existir, uma exceção genérica é criada. Em seguida, se a solicitação HTTP tiver sido feita localmente, todos os detalhes do erro são mostrados. Nesse caso, apenas o computador local executando o aplicativo web verá esses detalhes do erro. Depois que as informações de erro foi exibidas, o erro é adicionado ao arquivo de log e o erro será apagado do servidor.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Exibindo mensagens de erro sem tratamento para o aplicativo

Adicionando um `customErrors` seção para o *Web. config* , você pode rapidamente identificador de arquivo simples erros que ocorrem em todo o aplicativo. Você também pode especificar como manipular erros com base em seu valor de código de status, como 404 - arquivo não encontrado.

#### <a name="update-the-configuration"></a>Atualizar a configuração

Atualizar a configuração com a adição de um `customErrors` seção para o *Web. config* arquivo.

1. Na **Gerenciador de soluções**, encontre e abra o *Web. config* arquivo na raiz do aplicativo de exemplo Wingtip Toys.
2. Adicione a `customErrors` seção para o *Web. config* dentro do arquivo a `<system.web>` nó da seguinte maneira:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Salvar a *Web. config* arquivo.

O `customErrors` seção especifica o modo, que é definido como "Ativado". Ela também especifica o `defaultRedirect`, que informa ao aplicativo qual página para navegar até quando ocorre um erro. Além disso, você adicionou um elemento de erro específico que especifica como lidar com um erro 404 quando uma página não for encontrada. Mais tarde neste tutorial, você irá adicionar tratamento adicional que irá capturar os detalhes de um erro no nível do aplicativo.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O navegador é aberta e mostra a *default. aspx* página.
2. Insira a seguinte URL no navegador (não se esqueça de usar **seu** número da porta):  
    `https://localhost:44300/NoPage.aspx`
3. Examine os *ErrorPage* exibida no navegador. 

    ![Tratamento de erros do ASP.NET - página não encontrada erro](aspnet-error-handling/_static/image1.png)

Quando você solicitar o *NoPage.aspx* página, que não existe, a página de erro mostrará a mensagem de erro simples e as informações de erro detalhadas se detalhes adicionais estão disponíveis. No entanto, se o usuário solicitou uma página inexistente de um local remoto, a página de erro somente mostraria a mensagem de erro em vermelho.

### <a name="including-an-exception-for-testing-purposes"></a>Incluindo uma exceção para fins de teste

Para verificar como seu aplicativo funciona quando um erro ocorre, você pode deliberadamente criar condições de erro no ASP.NET. O aplicativo de exemplo do Wingtip Toys, você gerará uma exceção de teste quando a página padrão é carregado para ver o que acontece.

1. Abra o code-behind do *default. aspx* página no Visual Studio.   
   O *Default.aspx.cs* página code-behind será exibida.
2. No `Page_Load` manipulador, adicione código para que o manipulador aparece da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

É possível criar vários tipos diferentes de exceções. No código acima, você está criando um `InvalidOperationException` quando o *default. aspx* página for carregada.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo para ver como o aplicativo trata a exceção.

1. Pressione **CTRL + F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O aplicativo gera a InvalidOperationException. 

    > [!NOTE] 
    > 
    > Você deve pressionar **CTRL + F5** para exibir a página sem quebrar o código para exibir a origem do erro no Visual Studio.
2. Examine os *ErrorPage* exibida no navegador. 

    ![Tratamento de erros do ASP.NET - página de erro](aspnet-error-handling/_static/image2.png)

Como você pode ver os detalhes do erro, a exceção foi interceptada pela `customError` seção o *Web. config* arquivo.

### <a name="adding-application-level-error-handling"></a>Adicionar tratamento de erros de nível de aplicativo

Em vez de interceptação a exceção usando o `customErrors` seção o *Web. config* de arquivo, em que você obtenha algumas informações sobre a exceção, você pode interceptar o erro no nível do aplicativo e recuperar os detalhes do erro.

1. Na **Gerenciador de soluções**, encontre e abra o *Global.asax.cs* arquivo.
2. Adicionar um **Application\_erro** manipulador de modo que ele apareça da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Quando ocorre um erro no aplicativo, o `Application_Error` manipulador é chamado. Nesse manipulador, a última exceção é recuperada e revisada. Se a exceção foi sem tratamento e a exceção contém detalhes da exceção interna (ou seja, `InnerException` não for nulo), o aplicativo transfere a execução para a página de erro em que os detalhes da exceção são exibidos.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo para ver os detalhes de erro adicionais fornecidos pelo tratamento da exceção no nível do aplicativo.

1. Pressione **CTRL + F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O aplicativo gera o `InvalidOperationException` .
2. Examine os *ErrorPage* exibida no navegador. 

    ![Tratamento de erros do ASP.NET - erro de nível de aplicativo](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Adicionar tratamento de erros de nível de página

Você pode adicionar o tratamento de erros de nível de página para uma página usando a adição de um `ErrorPage` de atributo para o `@Page` diretiva da página, ou adicionando um `Page_Error` manipulador de eventos para o code-behind de uma página. Nesta seção, você adicionará uma `Page_Error` manipulador de eventos que transferirá a execução para o *ErrorPage* página.

1. Na **Gerenciador de soluções**, encontre e abra o *Default.aspx.cs* arquivo.
2. Adicionar um `Page_Error` manipulador para que o code-behind é exibido como segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Quando ocorre um erro na página, o `Page_Error` manipulador de eventos é chamado. Nesse manipulador, a última exceção é recuperada e revisada. Se um `InvalidOperationException` ocorrer, o `Page_Error` manipulador de eventos transfere a execução para a página de erro em que os detalhes da exceção são exibidos.

#### <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **CTRL + F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O aplicativo gera o `InvalidOperationException` .
2. Examine os *ErrorPage* exibida no navegador. 

    ![Tratamento de erros do ASP.NET - erro de nível de página](aspnet-error-handling/_static/image4.png)
3. Feche a janela do navegador.

### <a name="removing-the-exception-used-for-testing"></a>Removendo a exceção usada para teste

Para permitir que o aplicativo de exemplo do Wingtip Toys para funcionar sem lançar a exceção que você adicionou anteriormente neste tutorial, remova a exceção.

1. Abra o code-behind do *default. aspx* página.
2. No `Page_Load` manipulador, remova o código que gera a exceção para que o manipulador aparece da seguinte maneira:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Adicionando o log de erros de nível de código

Conforme mencionado anteriormente neste tutorial, você pode adicionar instruções try/catch para tentar executar uma seção de código e lidar com o primeiro erro que ocorre. Neste exemplo, você irá escrever apenas os detalhes do erro para o arquivo de log de erros para que o erro pode ser examinado mais tarde.

1. Na **Gerenciador de soluções**, no *lógica* pasta, localize e abra o *PayPalFunctions.cs* arquivo.
2. Atualização de `HttpCall` método para que o código seja exibido como segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

As chamadas de código acima de `LogException` que está contido no método o `ExceptionUtility` classe. Você adicionou o *ExceptionUtility.cs* arquivo de classe para o *lógica* pasta neste tutorial. O método `LogException` utiliza dois parâmetros. O primeiro parâmetro é o objeto de exceção. O segundo parâmetro é uma cadeia de caracteres usada para reconhecer a origem do erro.

### <a name="inspecting-the-error-logging-information"></a>Inspecionar as informações de registro em log de erro

Conforme mencionado anteriormente, você pode usar o log de erros para determinar quais erros em seu aplicativo devem ser corrigidos primeiro. É claro que, somente os erros que foram capturados e gravados no log de erro serão registrados.

1. No **Gerenciador de soluções**, encontre e abra o *ErrorLog.txt* arquivo o *aplicativo\_dados* pasta.   
 Talvez você precise selecionar o "**Show All Files**" opção ou a "**atualizar**" opção do topo da **Gerenciador de soluções** para ver o *ErrorLog.txt*arquivo.
2. Examine o log de erros exibido no Visual Studio: 

    ![Tratamento de erros do ASP.NET - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Mensagens de erro seguras

Vale **importante observar** que quando seu aplicativo exibe mensagens de erro, ele não deve fornecer informações que um usuário mal-intencionado pode achar útil atacar seu aplicativo. Por exemplo, se seu aplicativo não puder ser tenta gravar um banco de dados, ele não deve exibir uma mensagem de erro que inclui o nome de usuário que está usando. Por esse motivo, uma mensagem de erro genérica em vermelho é exibida ao usuário. Todos os detalhes de erro adicionais são exibidos apenas para o desenvolvedor no computador local.

## <a name="using-elmah"></a>Usando o ELMAH

O ELMAH (módulos de log de erros e manipuladores) é um recurso de registro de erro que você conecte seu aplicativo ASP.NET como um pacote do NuGet. O ELMAH fornece os seguintes recursos:

- Registro em log de exceções sem tratamento.
- Uma página da web para exibir o log inteiro de exceções sem tratamento gravados.
- Uma página da web para exibir os detalhes completos de cada um conectado a exceção.
- Uma notificação por email de cada erro no momento em que ele ocorre.
- Um RSS feed dos últimos 15 erros do log.

Antes de poder trabalhar com o ELMAH, você deve instalá-lo. Isso é fácil usando o *NuGet* instalador do pacote. Como mencionado anteriormente nesta série de tutoriais, o NuGet é uma extensão do Visual Studio que facilita a instalação e atualização de bibliotecas de código-fonte aberto e ferramentas no Visual Studio.

1. No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para solução**. 

    ![Tratamento de erros do ASP.NET - gerenciar pacotes NuGet para solução](aspnet-error-handling/_static/image6.png)
2. O **gerenciar pacotes NuGet** caixa de diálogo é exibida dentro do Visual Studio.
3. No **gerenciar pacotes NuGet** diálogo caixa, expanda **Online** à esquerda e, em seguida, selecione **nuget.org**. Em seguida, localizar e instalar o **ELMAH** pacote da lista de pacotes disponíveis online. 

    ![Tratamento de erros do ASP.NET - pacote de NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Você precisará ter uma conexão de internet para baixar o pacote.
5. No **selecionar projetos** diálogo caixa, certifique-se a **WingtipToys** seleção está selecionada e, em seguida, clique em **Okey**. 

    ![Tratamento de erro do ASP.NET – selecione a caixa de diálogo projetos](aspnet-error-handling/_static/image8.png)
6. Clique em **feche** na **Manage NuGet Packages** caixa de diálogo se necessário.
7. Se o Visual Studio solicita que você recarregue os arquivos abertos, selecione "**Sim para tudo**".
8. O pacote ELMAH adiciona entradas para si mesmo na *Web. config* arquivo na raiz do seu projeto. Se o Visual Studio perguntará se você deseja recarregar o modificada *Web. config* arquivo, clique em **Sim**.

O ELMAH agora está pronto para armazenar os erros sem tratamento que ocorrem.

### <a name="viewing-the-elmah-log"></a>Exibindo o Log ELMAH

Exibindo o log ELMAH é fácil, mas primeiro você criará uma exceção sem tratamento que será registrada no log do ELMAH.

1. Pressione **CTRL + F5** para executar o aplicativo de exemplo Wingtip Toys.
2. Para gravar uma exceção sem tratamento no log ELMAH, navegue em seu navegador para a URL a seguir (usando seu número de porta):  
    `https://localhost:44300/NoPage.aspx` A página de erro será exibida.
3. Para exibir o log ELMAH, navegue em seu navegador para a URL a seguir (usando seu número de porta):  
    `https://localhost:44300/elmah.axd`

    ![Tratamento de erro do ASP.NET – Log de erros do ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu sobre tratamento de erros no nível do aplicativo, o nível de página e o nível de código. Você também aprendeu a registrar tratados e sem tratamento de erros para revisão posterior. Você adicionou o utilitário ELMAH para fornecer o registro de exceção e a notificação para seu aplicativo usando o NuGet. Além disso, você aprendeu sobre a importância das mensagens de erro de segurança.

## <a name="tutorial-series-conclusion"></a>Conclusão da série de tutoriais

*Obrigado por acompanhar. Espero que este conjunto de tutoriais ajudado você a se familiarizar com os Web Forms do ASP.NET. Se você precisar de mais informações sobre os recursos de Web Forms disponíveis no ASP.NET 4.5 e no Visual Studio 2013, consulte* [*ASP.NET e Web Tools para notas de versão do Visual Studio 2013*](../../../../visual-studio/overview/2013/release-notes.md)*. Além disso, certifique-se de examinar o tutorial mencionado no* ***próximas etapas****seção e defintely experimentar o* [*gratuita do Azure*](https://azure.microsoft.com/pricing/free-trial/)*.*

![Obrigado - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como implantar seu aplicativo web para o Microsoft Azure, consulte [implantar um Secure Web Forms aplicativo ASP.NET com associação, OAuth e banco de dados SQL em um Site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Avaliação gratuita

[Microsoft Azure – avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)  
 Publicar seu site no Microsoft Azure, você poupará tempo, manutenção e despesas. É um processo rápido para implantar seu aplicativo web no Azure. Quando você precisa manter e monitorar seu aplicativo web, o Azure oferece uma variedade de ferramentas e serviços. Gerencie dados, o tráfego, identidade, backups, mensagens, mídia e desempenho no Azure. E, então, tudo isso é fornecido em uma abordagem muito econômica.

## <a name="additional-resources"></a>Recursos adicionais

[Detalhes do erro de registro em log com o monitoramento de integridade do ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Confirmações

Eu gostaria de agradecer a pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Países Baixos](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, EUA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Eslovênia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasil](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [De Carlos Santos, Brasil](http://www.carloscds.net/)
- [Dave Campbell, EUA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [EUA, de Michael cerquilha](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Papa
- [Mitchel vendedores, EUA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contribuições da comunidade

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 relacionados ao código de exemplo no MSDN: [navegação Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 relacionados ao código de exemplo no MSDN: [série de Tutorial de formulários do ASP.NET 4.5 da Web no Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Colaborador de público-alvo técnico da Microsoft (twitter: @driazevedo)  
  Conversão do Visual Studio 2012: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Anterior](url-routing.md)
