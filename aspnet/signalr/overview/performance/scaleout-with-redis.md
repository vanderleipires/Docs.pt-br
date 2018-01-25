---
uid: signalr/overview/performance/scaleout-with-redis
title: "Expansão do SignalR com Redis | Microsoft Docs"
author: MikeWasson
description: "Versões de software usados neste tópico Visual Studio 2013 .NET 4.5 SignalR versões anteriores de versão 2 deste tópico para obter informações sobre versões anteriores do..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-redis"></a>Expansão do SignalR com Redis
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


Neste tutorial, você usará [Redis](http://redis.io/) para distribuir mensagens através de um aplicativo de SignalR é implantado em duas instâncias separadas do IIS.

Redis é um repositório de chave-valor na memória. Ele também dá suporte a um sistema de mensagens com um modelo de publicação/assinatura. O backplane SignalR Redis usa o recurso de publicação/assinatura para encaminhar mensagens para outros servidores.

![](scaleout-with-redis/_static/image1.png)

Para este tutorial, você usará os três servidores:

- Dois servidores que executam o Windows, que você usará para implantar um aplicativo do SignalR.
- Um servidor executando o Linux, o que você usará para executar o Redis. Para as capturas de tela neste tutorial, usei Ubuntu 12.04 TLS.

Se você não tem três servidores físicos para usar, você pode criar VMs no Hyper-V. Outra opção é criar VMs no Azure.

Embora este tutorial usa a implementação de Redis oficial, há também uma [Windows porta do Redis](https://github.com/MSOpenTech/redis) de MSOpenTech. Instalação e configuração são diferentes, mas, caso contrário, as etapas são as mesmas.

> [!NOTE] 
> 
> Expansão do SignalR com Redis não oferece suporte a clusters do Redis.


## <a name="overview"></a>Visão geral

Antes de entrar para o tutorial detalhado, aqui está uma visão geral das tarefas que você executará.

1. Instale o Redis e iniciar o servidor do Redis.
2. Adicione esses pacotes do NuGet ao seu aplicativo: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Crie um aplicativo do SignalR.
4. Adicione o seguinte código ao Startup.cs para configurar o backplane: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu no Hyper-V

Usando o Windows Hyper-V, você pode criar facilmente uma VM Ubuntu no Windows Server.

Baixe o ISO Ubuntu da [http://www.ubuntu.com](http://www.ubuntu.com/).

No Hyper-V, adicione uma nova VM. No **conectar disco rígido Virtual** etapa, selecione **criar um disco rígido virtual**.

![](scaleout-with-redis/_static/image2.png)

No **opções de instalação** etapa, selecione **o arquivo de imagem (. ISO)**, clique em **procurar**e navegue para a instalação do Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Instalar o Redis

Siga as etapas em [http://redis.io/download](http://redis.io/download) para baixar e criar Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Isso cria os binários do Redis a `src` directory.

Por padrão, o Redis não requer uma senha. Para definir uma senha, edite o `redis.conf` arquivo, que está localizado no diretório raiz do código-fonte. (Fazer uma cópia de backup do arquivo antes de editá-lo!) Adicione a seguinte diretiva para `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Agora, inicie o servidor do Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Abra a porta 6379, que é a porta padrão que Redis escuta. (Você pode alterar o número da porta no arquivo de configuração).

## <a name="create-the-signalr-application"></a>Criar o aplicativo de SignalR

Crie um aplicativo de SignalR seguindo um destes tutoriais:

- [Guia de Introdução ao SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introdução ao SignalR 2.0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Em seguida, modificaremos o aplicativo de chat para dar suporte a expansão com Redis. Primeiro, adicione o pacote SignalR.Redis NuGet ao seu projeto. No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Em seguida, abra o arquivo Startup.cs. Adicione o seguinte código para o **configuração** método:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "servidor" é o nome do servidor que está executando o Redis.
- *porta* é o número da porta
- "password" é a senha que você definiu no arquivo conf.
- "AppName" é qualquer cadeia de caracteres. SignalR cria um canal de pub/sub Redis com esse nome.

Por exemplo:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Implantar e executar o aplicativo

Prepare suas instâncias de servidor do Windows para implantar o aplicativo do SignalR.

Adicione a função do IIS. Inclua recursos de "Desenvolvimento de aplicativos", incluindo o protocolo WebSocket.

![](scaleout-with-redis/_static/image5.png)

Também inclui o serviço de gerenciamento (listadas em "Ferramentas de gerenciamento").

![](scaleout-with-redis/_static/image6.png)

**Instalar Web implantação 3.0.** Quando você executar o Gerenciador do IIS, ele solicitará que você instale o Microsoft Web Platform, ou você pode [baixar o intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). No instalador de plataforma, pesquisar para implantação da Web e instalar o Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Verifique se o serviço de gerenciamento da Web está em execução. Caso contrário, inicie o serviço. (Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando você adicionou a função do IIS.)

Por padrão, o serviço de gerenciamento da Web escuta na porta TCP 8172. No Firewall do Windows, crie uma nova regra de entrada para permitir o tráfego TCP na porta 8172. Para obter mais informações, consulte [Configurando regras de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Se você estiver hospedando as VMs no Azure, você pode fazer isso diretamente no portal do Azure. Consulte [como configurar pontos de extremidade para uma máquina Virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Agora você está pronto para implantar o projeto do Visual Studio em seu computador de desenvolvimento para o servidor. No Gerenciador de soluções, clique com botão direito a solução e clique em **publicar**.

Para obter mais documentação sobre a implantação da web, consulte [mapa de conteúdo de implantação da Web do Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se você implantar o aplicativo em dois servidores, você pode abrir cada instância em uma janela separada do navegador e ver a que cada um receba mensagens do SignalR da outra. (É claro que, em um ambiente de produção, os dois servidores estaria atrás de um balanceador de carga.)

![](scaleout-with-redis/_static/image8.png)

Se você estiver curioso para ver as mensagens que são enviadas para Redis, você pode usar o **redis-cli** cliente, que é instalado com o Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
