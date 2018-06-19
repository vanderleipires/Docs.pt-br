---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN em uma função de trabalho do Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como hospedar OWIN internamente em uma função de trabalho do Microsoft Azure. Interface da Web aberta para .NET (OWIN) define uma abstração entre o servidor da web .NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868419"
---
<a name="host-owin-in-an-azure-worker-role"></a>Host OWIN em uma função de trabalho do Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar OWIN internamente em uma função de trabalho do Microsoft Azure.
> 
> [Abra a Interface da Web para .NET](http://owin.org/) (OWIN) define uma abstração entre os servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna OWIN ideal para hospedagem interna de um aplicativo da web em seu próprio processo e fora de IIS – por exemplo, dentro de uma função de trabalho do Azure.
> 
> Neste tutorial, você aprenderá como hospedagem interna de um aplicativo OWIN dentro de uma função de trabalho do Microsoft Azure. Para saber mais sobre as funções de trabalho, consulte [modelos de execução do Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [SDK do Azure para .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Criar um projeto do Microsoft Azure

Inicie o Visual Studio com privilégios de administrador. Privilégios de administrador são necessárias para depurar o aplicativo localmente, usando o emulador de computação do Azure.

No **arquivo** menu, clique em **novo**, em seguida, clique em **projeto**. De **modelos instalados**, em Visual c#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**. Nomeie o projeto "AzureApp" e clique em **Okey**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

No **novo serviço de nuvem do Windows Azure** caixa de diálogo, clique duas vezes em **função de trabalho**. Deixe o nome padrão ("WorkerRole1"). Esta etapa adiciona uma função de trabalho para a solução. Clique em **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

A solução do Visual Studio que é criada contém dois projetos:

- &quot;AzureApp&quot; define as funções e a configuração do aplicativo do Azure.
- &quot;WorkerRole1&quot; contém o código da função de trabalho.

Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial usa uma única função.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Adicionar os pacotes de hospedagem interna de OWIN

Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote**, em seguida, clique em **Package Manager Console**.

Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Adicionar um ponto de extremidade de HTTP

No Gerenciador de soluções, expanda o projeto AzureApp. Expanda o nó de funções, clique com botão direito WorkerRole1 e selecione **propriedades**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Clique em **pontos de extremidade**e, em seguida, clique em **Adicionar ponto de extremidade**.

No **protocolo** lista suspensa, selecione "http". Em **porta pública** e **porta privada**, digite 80. Esses números de porta podem ser diferentes. A porta pública é que os clientes usam quando enviam uma solicitação para a função.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Criar a classe de inicialização OWIN

No Gerenciador de soluções, clique com botão direito no projeto WorkerRole1 e selecione **adicionar** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

Substitua todo o código clichê com o seguinte:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

O `UseWelcomePage` método de extensão adiciona uma página HTML simples ao seu aplicativo, verifique se o site está funcionando.

## <a name="start-the-owin-host"></a>Iniciar o Host OWIN

Abra o arquivo WorkerRole.cs. Essa classe define o código que é executado quando a função de trabalho é iniciada e parada.

Adicione a seguinte instrução using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Adicionar uma **IDisposable** membro para o `WorkerRole` classe:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

No `OnStart` método, adicione o seguinte código para iniciar o host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

O **WebApp** método inicia o host OWIN. O nome do `Startup` classe é um parâmetro de tipo para o método. Por convenção, o host chamará o `Configure` método dessa classe.

Substituir o `OnStop` de descartar o  *\_aplicativo* instância:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Aqui está o código completo para WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure. Dependendo de suas configurações de firewall, você precisará permitir que o emulador através do firewall.

O emulador de computação atribui um endereço IP local para o ponto de extremidade. Você pode encontrar o endereço IP ao exibir a interface de usuário do emulador de computação. Clique no ícone do emulador na tarefa a área de notificação da barra e selecione **Mostrar UI do emulador de computação**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Localize o endereço IP em implantações de serviços de implantação [id], detalhes do serviço. Abra um navegador da web e navegue até http://<em>endereço</em>, onde <em>endereço</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80`. Você deve ver a página de boas-vinda do OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Nesta etapa, você deve ter uma conta do Azure. Se você ainda não tiver um, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

No Gerenciador de soluções, clique com botão direito no projeto AzureApp. Selecione **Publicar**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Se você não tiver entrado sua conta do Azure, clique em **entrar**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Depois que você está conectado, escolha uma assinatura e clique em **próximo**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Insira um nome para o serviço de nuvem e escolha uma região. Clique em **Criar**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Clique em **Publicar**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

A janela de Log de atividades do Azure mostra o progresso da implantação. Quando o aplicativo é implantado, navegue até `http://appname.cloudapp.net/`, onde *appname* é o nome do seu serviço de nuvem.

## <a name="additional-resources"></a>Recursos adicionais

- [Uma visão geral do projeto Katana](an-overview-of-project-katana.md)
- [Projeto Katana no GitHub](https://github.com/aspnet/AspNetKatana/)
