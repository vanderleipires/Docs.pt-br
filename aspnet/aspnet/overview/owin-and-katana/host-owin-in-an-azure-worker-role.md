---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hospedar OWIN em uma função de trabalho do Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como hospedar internamente o OWIN em uma função de trabalho do Microsoft Azure. Open Web Interface para .NET (OWIN) define uma abstração entre o servidor de web do .NET...
ms.author: aspnetcontent
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: f62b9299a4e369ae3a938c85e60dd6a79108548d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826475"
---
<a name="host-owin-in-an-azure-worker-role"></a>Hospedar OWIN em uma função de trabalho do Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar internamente o OWIN em uma função de trabalho do Microsoft Azure.
> 
> [Open Web Interface para .NET](http://owin.org/) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS – por exemplo, dentro de uma função de trabalho do Azure.
> 
> Neste tutorial, você aprenderá como hospedar internamente um aplicativos OWIN dentro de uma função de trabalho do Microsoft Azure. Para saber mais sobre as funções de trabalho, consulte [modelos de execução do Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [SDK do Azure para .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Criar um projeto do Microsoft Azure

Inicie o Visual Studio com privilégios de administrador. Privilégios de administrador são necessárias para depurar o aplicativo localmente, usando o emulador de computação do Azure.

No **arquivo** menu, clique em **New**, em seguida, clique em **projeto**. Partir **modelos instalados**, no Visual c#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**. Nomeie o projeto "AzureApp" e clique em **Okey**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

No **novo serviço de nuvem do Windows Azure** caixa de diálogo, clique duas vezes em **função de trabalho**. Deixe o nome padrão ("WorkerRole1"). Esta etapa adiciona uma função de trabalho à solução. Clique em **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

A solução do Visual Studio criado contém dois projetos:

- &quot;AzureApp&quot; define as funções e a configuração para o aplicativo do Azure.
- &quot;WorkerRole1&quot; contém o código da função de trabalho.

Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial usa uma única função.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Adicione os pacotes de auto-hospedagem de OWIN

Do **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca**, em seguida, clique em **Package Manager Console**.

Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Adicionar um ponto de extremidade HTTP

No Gerenciador de soluções, expanda o projeto AzureApp. Expanda o nó funções, WorkerRole1 com o botão direito e selecione **propriedades**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Clique em **pontos de extremidade**e, em seguida, clique em **Adicionar ponto de extremidade**.

No **protocolo** lista suspensa, selecione "http". Na **porta pública** e **porta privada**, digite 80. Esses números de porta podem ser diferentes. A porta pública é o que os clientes usam quando eles enviam uma solicitação para a função.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Criar a classe de inicialização OWIN

No Gerenciador de soluções, com o botão direito do mouse no projeto WorkerRole1 e selecione **Add** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

Substitua todo o código clichê com o seguinte:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

O `UseWelcomePage` método de extensão adiciona uma página HTML simples ao seu aplicativo, para verificar se o site está funcionando.

## <a name="start-the-owin-host"></a>Iniciar o Host OWIN

Abra o arquivo WorkerRole.cs. Essa classe define o código que é executado quando a função de trabalho é iniciada e parada.

Adicione a seguinte instrução using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Adicionar um **IDisposable** membro para o `WorkerRole` classe:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

No `OnStart` método, adicione o seguinte código para iniciar o host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

O **WebApp.Start** método inicia o host do OWIN. O nome da `Startup` classe é um parâmetro de tipo para o método. Por convenção, o host chamará o `Configure` método dessa classe.

Substituir a `OnStop` descartar o  *\_aplicativo* instância:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Aqui está o código completo para WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure. Dependendo de suas configurações de firewall, você precisa permitir que o emulador através do firewall.

O emulador de computação atribui um endereço IP local para o ponto de extremidade. Você pode encontrar o endereço IP, exibindo a interface de usuário do emulador de computação. Clique com botão direito no ícone do emulador na tarefa de área de notificação da barra e, em seguida, selecione **mostrar IU do emulador de computação**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Localize o endereço IP em implantações de serviços de implantação [id], detalhes do serviço. Abra um navegador da web e navegue até http://<em>endereço</em>, onde <em>endereço</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80`. Você verá a página de boas-vinda do OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Para esta etapa, você deve ter uma conta do Azure. Se você ainda não tiver uma, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

No Gerenciador de soluções, clique com botão direito no projeto AzureApp. Selecione **Publicar**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Se você não estiver conectado à sua conta do Azure, clique em **Sign In**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Depois que você está conectado, escolha uma assinatura e clique em **próxima**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Insira um nome para o serviço de nuvem e escolha uma região. Clique em **Criar**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Clique em **Publicar**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

A janela de Log de atividades do Azure mostra o progresso da implantação. Quando o aplicativo é implantado, navegue até `http://appname.cloudapp.net/`, onde *appname* é o nome do seu serviço de nuvem.

## <a name="additional-resources"></a>Recursos adicionais

- [Uma visão geral do projeto Katana](an-overview-of-project-katana.md)
- [Projeto Katana no GitHub](https://github.com/aspnet/AspNetKatana/)
