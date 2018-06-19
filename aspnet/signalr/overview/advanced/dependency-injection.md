---
uid: signalr/overview/advanced/dependency-injection
title: Injeção de dependência no SignalR | Microsoft Docs
author: MikeWasson
description: Versões de software usados neste tópico Visual Studio 2013 .NET 4.5 SignalR versões anteriores de versão 2 deste tópico para obter informações sobre versões anteriores do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504175"
---
<a name="dependency-injection-in-signalr"></a>Injeção de dependência no SignalR
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
> 
> Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


Injeção de dependência é uma maneira de remover embutido dependências entre objetos, tornando mais fácil para substituir as dependências de um objeto, para testar (usando objetos fictícios) ou para alterar o comportamento de tempo de execução. Este tutorial mostra como realizar injeção de dependência em hubs de SignalR. Ele também mostra como usar contêineres IoC com SignalR. Um contêiner IoC é uma estrutura geral para a injeção de dependência.

## <a name="what-is-dependency-injection"></a>O que é a injeção de dependência?

Se você já estiver familiarizado com a injeção de dependência, ignore esta seção.

*Injeção de dependência* (DI) é um padrão onde os objetos não são responsáveis por criar suas próprias dependências. Aqui está um exemplo simples motivar DI. Suponha que você tem um objeto que precisa para as mensagens de log. Você pode definir uma interface de log:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Em seu objeto, você pode criar um `ILogger` para as mensagens de log:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Isso funciona, mas não é o melhor design. Se você quiser substituir `FileLogger` com outra `ILogger` implementação, você precisará modificar `SomeComponent`. Suponhamos que muitos outros objetos `FileLogger`, você precisará alterar todos eles. Ou se você decidir fazer `FileLogger` um singleton, você também precisará fazer alterações em todo o aplicativo.

Uma abordagem melhor é "inserir" um `ILogger` no objeto — por exemplo, usando um argumento de construtor:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Agora que o objeto não é responsável por selecionar qual `ILogger` para usar. Você pode alternar `ILogger` implementações sem alterar os objetos que dependem dele.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Esse padrão é chamado [injeção de construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Outro padrão é a inclusão de setter, em que você definir a dependência por meio de uma propriedade ou método de setter.

## <a name="simple-dependency-injection-in-signalr"></a>Injeção de dependência simples no SignalR

Considere a aplicação de Chat do tutorial [guia de Introdução ao SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Aqui está a classe de hub do aplicativo:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Suponha que você deseja armazenar mensagens de chat no servidor antes de enviá-los. Você pode definir uma interface que abstrai essa funcionalidade e usar a DI injetar a interface para o `ChatHub` classe.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

O único problema é que um aplicativo SignalR não cria diretamente hubs; SignalR criará para você. Por padrão, o SignalR espera uma classe de hub para ter um construtor sem parâmetros. No entanto, você pode facilmente registrar uma função para criar instâncias de hub e usar essa função para executar a injeção de dependência. Registre a função chamando **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Agora o SignalR chamará essa função anônima sempre que precisar criar um `ChatHub` instância.

## <a name="ioc-containers"></a>Contêineres IoC

O código anterior é bom para casos simples. Mas ainda era necessário escrever isso:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

Em um aplicativo complexo com várias dependências, você precisará escrever muito do código "fiação". Esse código pode ser difícil de manter, especialmente se as dependências são aninhadas. Também é difícil de teste de unidade.

Uma solução é usar um contêiner IoC. Um contêiner IoC é um componente de software que é responsável por gerenciar as dependências. Você registrar tipos com o contêiner e, em seguida, usa o contêiner para criar objetos. O contêiner automaticamente descobre as relações de dependência. Vários contêineres IoC também permitem que você controle como escopo e tempo de vida do objeto.

> [!NOTE]
> "IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo. Um contêiner IoC constrói os objetos para você, o que "inverte" o fluxo normal de controle.


## <a name="using-ioc-containers-in-signalr"></a>Usando contêineres IoC no SignalR

O aplicativo de Chat provavelmente é muito simple para se beneficiar de um contêiner IoC. Em vez disso, vamos analisar o [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemplo.

O exemplo StockTicker define duas classes principais:

- `StockTickerHub`: A classe de hub, que gerencia as conexões de cliente.
- `StockTicker`: Um singleton que contém os preços de estoque e os atualiza periodicamente.

`StockTickerHub`contém uma referência para o `StockTicker` singleton, enquanto `StockTicker` contém uma referência para o **IHubConnectionContext** para o `StockTickerHub`. Ele usa essa interface para se comunicar com `StockTickerHub` instâncias. (Para obter mais informações, consulte [de difusão de servidor com ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Podemos usar um contêiner IoC para simplificar os essas dependências um pouco. Primeiro, vamos simplificar o `StockTickerHub` e `StockTicker` classes. No código a seguir, eu já comentadas as partes que não é necessário.

Remover o construtor sem parâmetros de `StockTickerHub`. Em vez disso, nós usaremos sempre DI para criar o hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Para StockTicker, remova a instância singleton. Posteriormente, usaremos o contêiner IoC para controlar o tempo de vida StockTicker. Além disso, verifique o construtor público.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Em seguida, é possível refatorar o código, criando uma interface para `StockTicker`. Vamos usar essa interface para desacoplar a `StockTickerHub` do `StockTicker` classe.

O Visual Studio faz esse tipo de refatoração fácil. Abra o arquivo StockTicker.cs, clique duas vezes no `StockTicker` declaração de classe e, em seguida, selecione **refatorar** ... **Extrair Interface**.

![](dependency-injection/_static/image1.png)

No **extrair Interface** caixa de diálogo, clique em **Selecionar tudo**. Deixe os outros padrões. Clique em **OK**.

![](dependency-injection/_static/image2.png)

O Visual Studio cria uma nova interface chamada `IStockTicker`e também o altera `StockTicker` derivar `IStockTicker`.

Abra o arquivo IStockTicker.cs e alterar a interface para **público**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

No `StockTickerHub` classe, altere as duas instâncias de `StockTicker` para `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Criando um `IStockTicker` interface não é estritamente necessária, mas gostaria de mostrar como a injeção de dependência pode ajudar a reduzir o acoplamento flexível entre os componentes de seu aplicativo.

## <a name="add-the-ninject-library"></a>Adicionar a biblioteca de Ninject

Há vários contêineres IoC de código-fonte aberto para .NET. Para este tutorial, usarei [Ninject](http://www.ninject.org/). (Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)

Use o Gerenciador de pacote de NuGet para instalar o [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10). No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** | **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Substitua o resolvedor de dependência de SignalR

Para usar Ninject no SignalR, crie uma classe que deriva de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Esta classe substitui o **GetService** e **GetServices** métodos de **DefaultDependencyResolver**. SignalR chama esses métodos para criar vários objetos em tempo de execução, incluindo instâncias do hub, bem como vários serviços usados internamente pelo SignalR.

- O **GetService** método cria uma única instância de um tipo. Substitua este método para chamar o kernel de Ninject **TryGet** método. Se esse método retornará nulo, voltar para o resolvedor padrão.
- O **GetServices** método cria uma coleção de objetos de um tipo especificado. Substitua este método para concatenar os resultados de Ninject com os resultados do resolvedor padrão.

## <a name="configure-ninject-bindings"></a>Configurar as associações de Ninject

Agora vamos usar Ninject para declarar associações de tipo.

Abra a classe de Startup.cs do aplicativo (que é criada manualmente de acordo com as instruções do pacote de `readme.txt`, ou que foi criado pela adição de autenticação ao seu projeto). No `Startup.Configuration` método, criar o contêiner Ninject que Ninject chama o *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Crie uma instância do nosso resolvedor de dependência personalizada:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Crie uma associação para `IStockTicker` da seguinte maneira:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Esse código está dizendo duas coisas. Primeiro, sempre que o aplicativo precisa de um `IStockTicker`, o kernel deve criar uma instância de `StockTicker`. Segundo, a `StockTicker` classe deve ser criado como um objeto de singleton. Ninject criará uma instância do objeto e retornar a mesma instância para cada solicitação.

Crie uma associação para **IHubConnectionContext** da seguinte maneira:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Esse código creatres uma função anônima que retorna um **IHubConnection**. O **WhenInjectedInto** Ninject para usar essa função somente durante a criação do método informa `IStockTicker` instâncias. O motivo é que o SignalR cria **IHubConnectionContext** instâncias internamente, e não queremos substituir como SignalR criá-los. Essa função só se aplica ao nosso `StockTicker` classe.

Passe o resolvedor de dependência para o **MapSignalR** método adicionando uma configuração de hub:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Atualize o método Startup.ConfigureSignalR na classe de inicialização do exemplo com o novo parâmetro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Agora o SignalR usará o resolvedor especificado em **MapSignalR**, em vez do resolvedor padrão.

Aqui está o código completo lista para `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Para executar o aplicativo StockTicker no Visual Studio, pressione F5. Na janela do navegador, navegue até `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

O aplicativo tem a mesma funcionalidade. (Para obter uma descrição, consulte [de difusão de servidor com ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Nós não alterou o comportamento; acabou de criar o código mais fácil de testar, manter e evoluir.
