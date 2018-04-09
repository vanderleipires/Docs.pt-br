---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.NET Web API 2 em uma função de trabalho do Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como hospedar API Web do ASP.NET em uma função de trabalho do Azure, usando OWIN para a estrutura da API Web de hospedagem interna. Abra a Interface da Web para de .NET (OWIN)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Host ASP.NET Web API 2 em uma função de trabalho do Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar API Web do ASP.NET em uma função de trabalho do Azure, usando OWIN para a estrutura da API Web de hospedagem interna.
> 
> [Abra a Interface da Web para .NET](http://owin.org/) (OWIN) define uma abstração entre os servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna OWIN ideal para hospedagem interna de um aplicativo da web em seu próprio processo e fora de IIS – por exemplo, dentro de uma função de trabalho do Azure.
> 
> Neste tutorial, você usará o pacote HttpListener, que fornece um servidor HTTP que sejam usados para hospedar os aplicativos OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - [SDK do Azure para .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Criar um projeto do Microsoft Azure

Inicie o Visual Studio com privilégios de administrador. Privilégios de administrador são necessárias para depurar o aplicativo localmente, usando o emulador de computação do Azure.

No **arquivo** menu, clique em **novo**, em seguida, clique em **projeto**. De **modelos instalados**, em Visual c#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**. Nomeie o projeto "AzureApp" e clique em **Okey**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

No **novo serviço de nuvem do Windows Azure** caixa de diálogo, clique duas vezes em **função de trabalho**. Deixe o nome padrão ("WorkerRole1"). Esta etapa adiciona uma função de trabalho para a solução. Clique em **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

A solução do Visual Studio que é criada contém dois projetos:

- &quot;AzureApp&quot; define as funções e a configuração do aplicativo do Azure.
- &quot;WorkerRole1&quot; contém o código da função de trabalho.

Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial usa uma única função.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicionar a API da Web e pacotes OWIN

Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote**, em seguida, clique em **Package Manager Console**.

Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Adicionar um ponto de extremidade de HTTP

No Gerenciador de soluções, expanda o projeto AzureApp. Expanda o nó de funções, clique com botão direito WorkerRole1 e selecione **propriedades**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Clique em **pontos de extremidade**e, em seguida, clique em **Adicionar ponto de extremidade**.

No **protocolo** lista suspensa, selecione "http". Em **porta pública** e **porta privada**, digite 80. Esses números de porta podem ser diferentes. A porta pública é que os clientes usam quando enviam uma solicitação para a função.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurar a API da Web para hospedar internamente

No Gerenciador de soluções, clique com botão direito no projeto WorkerRole1 e selecione **adicionar** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API da Web

Em seguida, adicione uma classe de controlador de API da Web. O projeto WorkerRole1 e selecione **adicionar** / **classe**. Nome da classe TestController. Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Para simplificar, este controlador apenas define dois métodos GET que retornam texto sem formatação.

## <a name="start-the-owin-host"></a>Iniciar o Host OWIN

Abra o arquivo WorkerRole.cs. Essa classe define o código que é executado quando a função de trabalho é iniciada e parada.

Adicione a seguinte instrução using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Adicionar uma **IDisposable** membro para o `WorkerRole` classe:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

No `OnStart` método, adicione o seguinte código para iniciar o host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

O **WebApp** método inicia o host OWIN. O nome do `Startup` classe é um parâmetro de tipo para o método. Por convenção, o host chamará o `Configure` método dessa classe.

Substituir o `OnStop` de descartar o  *\_aplicativo* instância:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Aqui está o código completo para WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure. Dependendo de suas configurações de firewall, você precisará permitir que o emulador através do firewall.

> [!NOTE]
> Se você obtiver uma exceção semelhante à seguinte, consulte [esta postagem de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para uma solução alternativa. "Não foi possível carregar arquivo ou assembly ' pt, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou uma de suas dependências. Definição do manifesto do assembly localizado não corresponde à referência de assembly. (Exceção de HRESULT: 0x80131040) "


O emulador de computação atribui um endereço IP local para o ponto de extremidade. Você pode encontrar o endereço IP ao exibir a interface de usuário do emulador de computação. Clique no ícone do emulador na tarefa a área de notificação da barra e selecione **Mostrar UI do emulador de computação**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Localize o endereço IP em implantações de serviços de implantação [id], detalhes do serviço. Abra um navegador da web e navegue até http://<em>endereço</em>/teste/1, onde <em>endereço</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80/test/1`. Você deve ver a resposta do controlador de API da Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Nesta etapa, você deve ter uma conta do Azure. Se você ainda não tiver um, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

No Gerenciador de soluções, clique com botão direito no projeto AzureApp. Selecione **Publicar**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Se você não tiver entrado sua conta do Azure, clique em **entrar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Depois que você está conectado, escolha uma assinatura e clique em **próximo**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Insira um nome para o serviço de nuvem e escolha uma região. Clique em **Criar**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Clique em **Publicar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

A janela de Log de atividades do Azure mostra o progresso da implantação. Quando o aplicativo é implantado, navegue até http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Recursos adicionais

- [Uma visão geral do projeto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Projeto Katana no GitHub](https://github.com/aspnet/AspNetKatana)
