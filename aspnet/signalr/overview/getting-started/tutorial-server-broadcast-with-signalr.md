---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: "Tutorial: Servidor de transmissão com SignalR 2 | Microsoft Docs"
author: tdykstra
description: "Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR 2 para fornecer a funcionalidade de difusão de servidor. Difusão de servidor significa que commun..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: cd800062e87c07a0ef1d8d3d32c910aaf3e683cc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Servidor de transmissão com SignalR 2
====================
por [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR 2 para fornecer a funcionalidade de difusão de servidor. Difusão de servidor significa que comunicações enviadas para os clientes são iniciadas pelo servidor. Esse cenário requer uma abordagem diferente de programação de cenários de ponto a ponto, como aplicativos de chat, em que as comunicações enviadas para os clientes são iniciadas por um ou mais dos clientes.
> 
> O aplicativo que você criará neste tutorial simula uma cotação de ações, um cenário típico para a funcionalidade de difusão de servidor.
> 
> Este tópico foi criado originalmente por Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Usando o Visual Studio 2012 com este tutorial
> 
> 
> Para usar o Visual Studio 2012 com este tutorial, faça o seguinte:
> 
> - Atualização de seu [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) para a versão mais recente.
> - Instalar o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - No Web Platform Installer, procurar e instalar **ASP.NET e Web 2013.1 de ferramentas para Visual Studio 2012**. Isso irá instalar os modelos do Visual Studio para SignalR classes como **Hub**.
> - Alguns modelos (como **classe de inicialização OWIN**) não estará disponível; para isso, use um arquivo de classe em vez disso.
> 
> 
> ## <a name="tutorial-versions"></a>Versões do tutoriais
> 
> Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Visão Geral

Neste tutorial, você vai criar um aplicativo de cotações da bolsa é representativo de aplicativos em tempo real no qual você deseja periodicamente "push" ou difusão, notificações do servidor para todos os clientes conectados. A primeira parte deste tutorial, você criará uma versão simplificada do aplicativo do zero. No restante do tutorial, você vai instalar um pacote do NuGet que contém recursos adicionais e examine o código para esses recursos.

O aplicativo que você criará na primeira parte deste tutorial exibe uma grade com dados de estoque.

![Versão inicial do StockTicker](tutorial-server-broadcast-with-signalr/_static/image1.png)

Periodicamente, o servidor atualiza os preços de estoque aleatoriamente e envia as atualizações para todos os clientes conectados. No navegador de números e símbolos no **alterar** e  **%**  colunas alteram dinamicamente em resposta a notificações do servidor. Se você abrir navegadores adicionais para a mesma URL, todas elas mostram os mesmos dados e as mesmas alterações nos dados simultaneamente.

Este tutorial contém as seções a seguir:

- [Pré-requisitos](#prerequisites)
- [Criar o projeto](#createproject)
- [Configure o código de servidor](#server)
- [Configure o código de cliente](#client)
- [Testar o aplicativo](#test)
- [Habilitar registro em log](#enablelogging)
- [Instalar e revise o exemplo StockTicker completo](#fullsample)
- [Próximas etapas](#nextsteps)

O [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pacote NuGet instala um aplicativo de cotações da bolsa simulado de exemplo em um projeto do Visual Studio.

> [!NOTE]
> Se você não quiser trabalhar através das etapas de criação do aplicativo, você pode instalar o pacote de SignalR.Sample em um novo projeto de aplicativo Web ASP.NET vazio. Se você instalar o pacote NuGet sem executar as etapas neste tutorial, **devem seguir as instruções no arquivo Leiame. txt**. Para executar o pacote, você precisa adicionar uma classe de inicialização OWIN que chama o método ConfigureSignalR no pacote instalado. Se você não adicionar a classe de inicialização OWIN, você receberá um erro.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que você tem o Visual Studio 2013 instalado no seu computador. Se você não tiver o Visual Studio, consulte [ASP.NET Downloads](https://www.asp.net/downloads) para obter o Visual Studio 2013 Express gratuito.

<a id="createproject"></a>

## <a name="create-the-project"></a>Criar o projeto

1. Do **arquivo** menu, clique em **novo projeto**.
2. No **novo projeto** caixa de diálogo caixa, expanda **c#** em **modelos** e selecione **Web**.
3. Selecione o **aplicativo Web vazio ASP.NET** modelo, nomeie o projeto *SignalR.StockTicker*e clique em **Okey**.

    ![Caixa de diálogo Novo Projeto](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. No **ASP.NET novo** janela do projeto, deixe **vazio** selecionado e clique em **criar projeto**.

    ![Caixa de diálogo Novo projeto do ASP](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configure o código de servidor

Nesta seção você configurar o código que é executado no servidor.

### <a name="create-the-stock-class"></a>Criar a classe de estoque

Comece criando a classe de modelo de estoque que você usará para armazenar e transmitir informações sobre uma ação.

1. Criar um novo arquivo de classe na pasta do projeto, nomeie-o *Stock.cs*e, em seguida, substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    As duas propriedades que você definirá quando você criar ações são o símbolo (por exemplo, MSFT da Microsoft) e o preço. As outras propriedades dependem de como e quando você define o preço. Na primeira vez que você definir preços, o valor é propagado para DayOpen. Outras vezes, quando você definir preços, a alteração e valores de propriedade PercentChange são calculados com base na diferença entre o preço e DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Criar as classes StockTicker e StockTickerHub

Você usará a API de Hub SignalR para tratar a interação com o servidor para cliente. Uma classe StockTickerHub que deriva da classe SignalR Hub tratará a receber chamadas de método e de conexões de clientes. Você também precisa manter dados de estoque e executar um objeto de Timer para disparar periodicamente atualizações de preço, independentemente das conexões de cliente. Você não pode colocar essas funções em uma classe de Hub, como instâncias de Hub são transitórias. Uma instância da classe Hub é criada para cada operação no hub, como conexões e chamadas do cliente para o servidor. Portanto, o mecanismo que mantém dados de estoque, atualizações de preços e transmite as atualizações de preços deve ser executado em uma classe separada, você nomeará StockTicker.

![Transmitindo de StockTicker](tutorial-server-broadcast-with-signalr/_static/image5.png)

Deseja apenas uma instância da classe StockTicker para executados no servidor, portanto, você precisará configurar uma referência de cada instância de StockTickerHub para a instância de StockTicker singleton. A classe StockTicker deve ser capaz de difusão para os clientes porque ela tem os dados de ações e gatilhos atualizações, mas StockTicker não é uma classe de Hub. Portanto, a classe StockTicker tem que obter uma referência para o objeto de contexto de conexão de SignalR Hub. Ele pode usar o objeto de contexto de conexão do SignalR para difusão para clientes.

1. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Classe de Hub SignalR (v2)**.
2. Nomeie o novo hub *StockTickerHub.cs*e, em seguida, clique em **adicionar**. Pacotes do SignalR NuGet serão adicionados ao seu projeto.
3. Substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    O [Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe é usada para definir métodos que os clientes podem chamar no servidor. Você está definindo um método: `GetAllStocks()`. Quando um cliente se conecta inicialmente ao servidor, ele chamará esse método para obter uma lista de todas as ações com os preços atuais. O método pode executar de forma síncrona e retornar `IEnumerable<Stock>` porque ele está retornando dados de memória. Se o método teve que obter os dados fazendo algo que envolve a espera, como uma pesquisa de banco de dados ou uma chamada de serviço web, você deve especificar `Task<IEnumerable<Stock>>` como o valor de retorno para habilitar o processamento assíncrono. Para obter mais informações, consulte [guia de API de Hubs do ASP.NET SignalR - Server - quando executadas de forma assíncrona](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    O atributo HubName Especifica como o Hub será referenciado no código JavaScript no cliente. O nome padrão do cliente se você não usar esse atributo é uma versão concatenados de nome de classe, que nesse caso, seria stockTickerHub.

    Como você verá posteriormente quando você criar a classe StockTicker, uma instância singleton da classe é criada em sua propriedade de instância estática. Que instância singleton do StockTicker permanece na memória, independentemente de quantos clientes conectar ou desconectar, e essa instância é o que usa o método GetAllStocks para retornar informações de estoque atuais.
4. Criar um novo arquivo de classe na pasta do projeto, nomeie-o *StockTicker.cs*e, em seguida, substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Já que vários threads executará a mesma instância de código StockTicker, a classe StockTicker deve ser threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Armazenar a instância singleton em um campo estático

    O código inicializa estático \_campo de instância que faz a propriedade de instância com uma instância da classe e isso é a única instância da classe que pode ser criada porque o construtor está marcado como privado. [Inicialização lenta](https://msdn.microsoft.com/en-us/library/dd997286.aspx) é usado para o \_campo de instância, não por motivos de desempenho mas para assegurar que a criação de instância é threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Cada vez que um cliente se conecta ao servidor, uma nova instância da classe StockTickerHub em execução em um thread separado obtém a instância singleton StockTicker da propriedade StockTicker.Instance estática, como você viu anteriormente na classe StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Armazenando dados de estoque em um ConcurrentDictionary

    O construtor inicializa o \_coleção de ações com alguns dados de estoque de exemplo e GetAllStocks retorna as ações. Como você viu anteriormente, esta coleção de ações por sua vez é retornada por StockTickerHub.GetAllStocks que é um método de servidor na classe Hub que os clientes podem chamar.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    A coleção de ações está definida como um [ConcurrentDictionary](https://msdn.microsoft.com/en-us/library/dd287191.aspx) tipo de acesso thread-safe. Como alternativa, você pode usar um [dicionário](https://msdn.microsoft.com/en-us/library/xfhwa508.aspx) do objeto e o dicionário de bloqueio explicitamente, quando você fizer alterações.

    Para este aplicativo de exemplo, Okey é armazenar dados de aplicativo na memória e os dados perdidos quando a instância de StockTicker é descartada. Em um aplicativo real, você trabalhará com um repositório de dados de back-end como um banco de dados.

    ### <a name="periodically-updating-stock-prices"></a>Atualizar periodicamente os preços de estoque

    Inicia o construtor de um objeto de Timer que chama os métodos que atualizam os preços de estoque de forma aleatória.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices é chamado pelo temporizador, que passa nulo no parâmetro state. Antes de atualizar os preços, um bloqueio é ativado a \_updateStockPricesLock objeto. O código verifica se outro thread já está atualizando os preços e, em seguida, ele chama TryUpdateStockPrice em cada ação na lista. O método TryUpdateStockPrice decide se alterar o preço de estoque e quanto para alterá-lo. Se o preço de estoque for alterado, BroadcastStockPrice é chamado para transmitir a alteração de preço de estoque para todos os clientes conectados.

    O \_updatingStockPrices sinalizador está marcado como [volátil](https://msdn.microsoft.com/en-us/library/x13ttww7.aspx) para garantir que o acesso a ele é threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    Em um aplicativo real, o método TryUpdateStockPrice poderia chamar um serviço web para pesquisar o preço. Nesse código, ele usa um gerador de número aleatório para fazer alterações aleatoriamente.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtendo o contexto de SignalR para que a classe StockTicker pode transmitir para clientes

    Como as alterações de preço são originadas aqui no objeto StockTicker, isso é o objeto que precisa chamar um método updateStockPrice em todos os clientes conectados. Em uma classe de Hub, você tem uma API para chamar métodos de cliente, mas StockTicker não derivam da classe de Hub e não tem uma referência a qualquer objeto de Hub. Portanto, para transmitir para clientes conectados, a classe StockTicker tem que obter a instância do contexto SignalR para a classe StockTickerHub e usá-lo para chamar métodos em clientes.

    O código obtém uma referência para o contexto de SignalR ao criar a instância da classe singleton, passa que fazem referência ao construtor, e o construtor coloca na propriedade de clientes.

    Há duas razões por que você deseja obter o contexto de apenas uma vez: obtendo o contexto é uma operação cara e Obtendo-o a vez garante que a ordem desejada de mensagens enviadas aos clientes é preservada.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Obtendo a propriedade de clientes do contexto e colocá-lo na propriedade StockTickerClient permite que você escreva código para chamar métodos de cliente que tem a mesma aparência como se estivesse em uma classe de Hub. Por exemplo, para transmitir a todos os clientes, você pode escrever Clients.All.updateStockPrice(stock).

    O método updateStockPrice da chamada em BroadcastStockPrice ainda; não existe você adicioná-lo mais tarde quando você escreve o código que é executado no cliente. Você pode consultar updateStockPrice aqui porque Clients.All é dinâmico, o que significa que a expressão será avaliada em tempo de execução. Quando esta chamada de método é executado, SignalR enviará o nome do método e o valor do parâmetro para o cliente e se o cliente tiver um método chamado updateStockPrice, esse método será chamado e o valor do parâmetro será passado para ele.

    Clients.All significa enviar a todos os clientes. O SignalR fornece outras opções para especificar quais clientes ou grupos de clientes para enviar para. Para obter mais informações, consulte [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrar a rota de SignalR

O servidor precisa saber qual URL para interceptar e direcionar o SignalR. Para fazer isso, você adicionará e classe de inicialização OWIN.

1. Em **Solution Explorer**, clique com o botão direito e, em seguida, clique em **adicionar | Classe de inicialização OWIN**. Nomeie a classe **Startup.cs**.
2. Substitua o código em **Startup.cs** com o seguinte.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Agora você concluiu o código do servidor de configuração. Na próxima seção, você irá configurar o cliente.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configure o código de cliente

1. Crie um novo arquivo HTML na pasta do projeto e nomeie- *StockTicker.html*.
2. Substitua o código de modelo com o código a seguir.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    O HTML cria uma tabela com 5 colunas, uma linha de cabeçalho e uma linha de dados com uma única célula que abrange todas as colunas de 5. A linha de dados exibe "Carregando..." e será exibida apenas momentaneamente quando o aplicativo for iniciado. O código JavaScript removerá essa linha e adicione em suas linhas de local com dados de estoque recuperados do servidor.

    As marcas de script especificam o arquivo de script do jQuery, o arquivo de script do SignalR core, o arquivo de script do SignalR proxies e um arquivo de script StockTicker que você criará mais tarde. O arquivo de script de proxies SignalR, que especifica a URL "hubs do signalr /", é gerado dinamicamente e define os métodos de proxy para os métodos na classe Hub, nesse caso para StockTickerHub.GetAllStocks. Se preferir, você pode gerar esse arquivo JavaScript manualmente usando [SignalR utilitários](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) e desativar a criação do arquivo dinâmico na chamada do método MapHubs.
3. > [!IMPORTANT]
 > Verifique se o arquivo JavaScript referencia em *StockTicker.html* estão corretas. Ou seja, certifique-se de que a versão jQuery em sua marca de script (1.10.2 no exemplo) é o mesmo que a versão jQuery em seu projeto *Scripts* pasta e certifique-se de que a versão de SignalR em sua marca de script é o mesmo que o SignalR versão em seu projeto *Scripts* pasta. Altere os nomes de arquivo em marcas de script, se necessário.
4. Em **Solution Explorer**, clique com botão direito *StockTicker.html*e, em seguida, clique em **definir como página inicial**.
5. Crie um novo arquivo JavaScript na pasta do projeto e nomeie- *StockTicker.js*...
6. Substitua o código de modelo com o código a seguir:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection se refere a proxies do SignalR. O código obtém uma referência ao proxy para a classe StockTickerHub e o coloca na variável de ação. O nome do proxy é o nome definido pelo atributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Depois que todas as variáveis e funções são definidas, a última linha do código no arquivo inicializa a conexão SignalR chamando a função de início do SignalR. Função inicial executa de forma assíncrona e retorna um [jQuery adiado objeto](http://api.jquery.com/category/deferred-object/), que significa que você pode chamar a função concluída para especificar a função ser chamada quando a operação assíncrona é concluída.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Função init chama a função getAllStocks no servidor e usa as informações que o servidor retorna para atualizar a tabela de estoque. Observe que, por padrão, você deve usar a concatenação com maiusculas e minúsculas no cliente Embora o nome do método é pascal de maiusculas e minúsculas no servidor. A regra de maiusculas e minúsculas ter só se aplica aos métodos, objetos não. Por exemplo, você se referir ao estoque. Símbolo e estoque. Preço, não stock.symbol ou stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Se você quiser usar maiusculas e minúsculas de pascal no cliente, ou se você quiser usar um nome de método completamente diferente, você poderia decorar o método de Hub com o atributo HubMethodName da mesma forma você decorado a própria classe de Hub com o atributo HubName.

    O método init, HTML para uma linha da tabela é criada para cada objeto de estoque recebido do servidor por chamada formatStock para propriedades de formato do objeto estoque e, em seguida, chamando suplantar (que é definida na parte superior do *StockTicker.js*) para substituir os espaços reservados na variável rowTemplate com os valores de propriedade do objeto de estoque. O HTML resultante, em seguida, é acrescentado à tabela de estoque.

    Chamar init, passá-la como uma função de retorno de chamada que é executado depois que a função de inicialização assíncrona é concluída. Se você chamou init como uma instrução JavaScript separada depois de chamar o início, a função falhará porque ele será executado imediatamente sem esperar que a função do início ao fim de estabelecer a conexão. Nesse caso, a função init tentar chamar a função getAllStocks antes que a conexão de servidor é estabelecida.

    Quando o servidor alterará o preço de estoque, ele chama o updateStockPrice em clientes conectados. A função é adicionada para a propriedade de cliente do proxy stockTicker para disponibilizá-lo para chamadas do servidor.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    A função updateStockPrice formata um objeto de estoque recebido do servidor em uma linha de tabela da mesma maneira que a função de inicialização. No entanto, em vez de acrescentar a linha na tabela, ele localiza a linha atual do estoque na tabela e substitui a linha com um novo.

<a id="test"></a>

## <a name="test-the-application"></a>Testar o aplicativo

1. Pressione F5 para executar o aplicativo no modo de depuração.

    A tabela de estoque inicialmente exibe a "Carregando..." linha, em seguida, após um pequeno intervalo que de dados de estoque inicias são exibidos e inicie os preços de estoque alterar.

    ![Carregando](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Tabela de estoque inicial](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Tabela de estoque receber alterações do servidor](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Copie a URL da barra de endereços do navegador e cole-o em uma ou mais novas janelas de navegador.

    A exibição de estoque inicial é o mesmo que o navegador primeiro e as alterações ocorrem simultaneamente.
3. Feche todos os navegadores e abra um novo navegador, vá para a mesma URL.

    O objeto de singleton StockTicker continua a executar no servidor, portanto, a exibição da tabela de estoque mostra que as ações continuam a alterar. (Você não verá a tabela inicial com zero alterar valores.)
4. Feche o navegador.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Habilite o registro em logs

O SignalR tem uma função de registro em log internos que podem ser habilitadas no cliente para ajudar na solução de problemas. Nesta seção você habilita o log e ver exemplos que mostram como logs de dizer qual dos seguintes métodos de transporte SignalR está usando:

- [O WebSocket](http://en.wikipedia.org/wiki/WebSocket), com suporte pelo IIS 8 e navegadores atuais.
- [Eventos enviados pelo servidor](http://en.wikipedia.org/wiki/Server-sent_events), com suporte por navegadores que não seja o Internet Explorer.
- [Para sempre quadro](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)com suporte pelo Internet Explorer.
- [AJAX sondagens longa](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), todos os navegadores com suporte.

Para qualquer conexão determinado, SignalR escolhe o melhor método de transporte que suportam o cliente e o servidor.

1. Abra *StockTicker.js* e adicione uma linha de código para habilitar o log imediatamente antes do código que inicializa a conexão ao final do arquivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Pressione F5 para executar o projeto.
3. Abrir janela de ferramentas de desenvolvedor do navegador e selecione o Console para ver os logs. Você talvez precise atualizar a página para ver os logs do Signalr negociar o método de transporte para uma nova conexão.

    Se você estiver executando o Internet Explorer 10 no Windows 8 (IIS 8), o método de transporte é WebSocket.

    ![Console do 10 IIS IE 8](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Se você estiver executando o Internet Explorer 10 no Windows 7 (IIS 7.5), o método de transporte é iframe.

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    No Firefox, instale o suplemento Firebug para obter uma janela do Console. Se você estiver executando o Firefox 19 no Windows 8 (IIS 8), o método de transporte é WebSocket.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Se você estiver executando o Firefox 19 no Windows 7 (IIS 7.5), o método de transporte é eventos enviados pelo servidor.

    ![Console do Firefox 19 IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalar e revise o exemplo StockTicker completo

O aplicativo StockTicker instalado, o [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pacote NuGet inclui mais recursos do que a versão simplificada que você acabou de criar do zero. Nesta seção do tutorial, instale o pacote NuGet e analise os novos recursos e o código que implementa-los. Se você instalar o pacote sem executar as etapas anteriores deste tutorial, você deve adicionar uma classe de inicialização OWIN ao seu projeto. Esta etapa é explicada no arquivo Leiame. txt para o pacote do NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Instale o pacote SignalR.Sample NuGet

1. Em **Solution Explorer**, clique com o botão direito e clique em **gerenciar pacotes NuGet**.
2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **Online**, digite *SignalR.Sample* no **Pesquisar Online** caixa e, em seguida, clique em  **Instalar** no **SignalR.Sample** pacote.

    ![Instalar o pacote de SignalR.Sample](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. Em **Solution Explorer**, expanda o *SignalR.Sample* pasta que foi criada pela instalação do pacote SignalR.Sample.
4. No *SignalR.Sample* pasta, clique com botão direito *StockTicker.html*e, em seguida, clique em **definir como página inicial**.

    > [!NOTE]
    > Instalar o SignalR.Sample NuGet o pacote pode alterar a versão do jQuery que você tem em seu *Scripts* pasta. O novo *StockTicker.html* que instala o pacote no arquivo de *SignalR.Sample* pasta serão sincronizada com a versão do jQuery que instala o pacote, mas se você deseja executar original *StockTicker.html* novamente, talvez seja necessário atualizar a referência de jQuery na marca de script primeiro.

### <a name="run-the-application"></a>Executar o aplicativo

1. Pressione F5 para executar o aplicativo.

    Além de grade que vimos anteriormente, o aplicativo de cotações da bolsa completo mostra uma janela de rolagem horizontal que exibe os mesmos dados de estoque. Quando você executa o aplicativo pela primeira vez, "market" é "closed" e você verá uma grade estática e uma janela de ticker não rolagem.

    ![Início de tela de StockTicker](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Quando você clica em **Open Market**, o **Live Stock Ticker** caixa começa a rolar horizontalmente, e o servidor começar a difusão periodicamente as alterações de preço de estoque de forma aleatória. Cada vez que um preço de estoque é alterado, ambos o **Live tabela de estoque** grade e o **Live cotações de ações** caixa são atualizadas. Quando a alteração de preço de estoque é positiva, o estoque é mostrado com um plano de fundo verde, e quando a alteração for negativa, o estoque com um plano de fundo vermelho.

    ![Abra o aplicativo StockTicker, mercado](tutorial-server-broadcast-with-signalr/_static/image15.png)

    O **mercado fechar** botão interrompe as alterações e não a rolagem ticker e o **redefinir** botão redefine todos os dados de estoque para o estado inicial antes de iniciar as alterações de preço. Se você abre mais janelas do navegador e vá para a mesma URL, você verá os mesmos dados são atualizados dinamicamente ao mesmo tempo em cada navegador. Quando você clica em um dos botões, todos os navegadores respondem da mesma forma, ao mesmo tempo.

### <a name="live-stock-ticker-display"></a>Exibição dinâmica de cotação de ações

O **Live Stock Ticker** exibição é uma lista não ordenada em um elemento div que é formatado em uma única linha por estilos CSS. A ação será inicializada e atualizada da mesma forma que a tabela:, substituindo espaços reservados em uma &lt;li&gt; cadeia de caracteres de modelo e adicionar dinamicamente a &lt;li&gt; elementos para o &lt;ul&gt; elemento. A rolagem é executada por meio da função animar jQuery para variar a margem à esquerda da lista não ordenada de dentro do div.

As cotações da bolsa HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

As cotações da bolsa CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

O código jQuery que torna role:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionais no servidor que o cliente pode chamar

A classe StockTickerHub define quatro métodos adicionais que o cliente pode chamar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

OpenMarket, CloseMarket e redefinição são chamados em resposta aos botões na parte superior da página. Eles demonstram o padrão de um cliente disparar uma alteração no estado imediatamente é propagado para todos os clientes. Cada um dos métodos a seguir chama um método na classe StockTicker que afetam o estado de mercado, alterar e, em seguida, transmite o novo estado.

Na classe StockTicker, o estado do mercado é mantido por uma propriedade MarketState que retorna um valor de enumeração MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada um dos métodos que alteram o estado de mercado fazê-lo dentro de um bloco de bloqueio porque a classe StockTicker deve ser threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para garantir que esse código é threadsafe, o \_marketState campo que faz a propriedade MarketState está marcado como volátil,

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Os métodos BroadcastMarketStateChange e BroadcastMarketReset são semelhantes ao método BroadcastStockPrice que você já viu, exceto que eles chamam de diferentes métodos definidos no cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funções adicionais no cliente que o servidor pode chamar

A função updateStockPrice agora manipula a grade e a exibição ticker e usa jQuery.Color para flash cores vermelho e verdes.

Novas funções no *SignalR.StockTicker.js* habilitar e desabilitar os botões com base no estado de mercado e parar ou iniciar a rolagem horizontal do ticker janela. Como várias funções que estão sendo adicionadas ao ticker.client, o [jQuery estender função](http://api.jquery.com/jQuery.extend/) é usada para adicioná-los.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Instalação de cliente adicionais depois de estabelecer a conexão

Depois que o cliente estabelece a conexão, ele tem algum trabalho adicional para fazer: saber se o mercado está aberta ou fechada para chamar a função marketClosed ou marketOpened apropriado e anexe as chamadas de método do servidor para os botões.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Os métodos de servidor não são conectados botões até depois que a conexão é estabelecida, para que o código não é possível tentar chamar os métodos de servidor antes que eles estejam disponíveis.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como programar um aplicativo de SignalR que transmite mensagens do servidor para todos os clientes conectados, em intervalos periódicos e em resposta a notificações de qualquer cliente. O padrão de usar uma instância singleton multithread para manter o estado do servidor também pode ser usado também em cenários de jogos online de vários jogadores. Para obter um exemplo, consulte [o jogo ShootR baseado em SignalR](https://github.com/NTaylorMullen/ShootR).

Para tutoriais que mostram os cenários de comunicação ponto a ponto, consulte [guia de Introdução ao SignalR](introduction-to-signalr.md) e [atualização em tempo real com SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Para saber mais avançados conceitos de desenvolvimento SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [SignalR do ASP.NET](../../index.md)
- [Projeto de SignalR](http://signalr.net/)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)

Para obter instruções sobre como implantar um aplicativo SignalR no Azure, consulte [usando SignalR com aplicativos Web no serviço de aplicativo do Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obter informações detalhadas sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).
