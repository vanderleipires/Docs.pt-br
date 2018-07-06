---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hospedar a API Web ASP.NET 2 em uma função de trabalho do Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como hospedar a API Web do ASP.NET em uma função de trabalho do Azure, usar o OWIN para auto-hospedar a estrutura da API Web. Open Web Interface para de .NET (OWIN)...
ms.author: aspnetcontent
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: c53256b8a72a377f51b9fbac7944657cb6d4c6e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803844"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hospedar a API Web ASP.NET 2 em uma função de trabalho do Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar a API Web do ASP.NET em uma função de trabalho do Azure, usar o OWIN para auto-hospedar a estrutura da API Web.
> 
> [Open Web Interface para .NET](http://owin.org/) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS – por exemplo, dentro de uma função de trabalho do Azure.
> 
> Neste tutorial, você usará o pacote Microsoft.Owin.Host.HttpListener, que fornece um servidor HTTP que ser usado para hospedar internamente os aplicativos do OWIN.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - API Web 2
> - [SDK do Azure para .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Criar um projeto do Microsoft Azure

Inicie o Visual Studio com privilégios de administrador. Privilégios de administrador são necessárias para depurar o aplicativo localmente, usando o emulador de computação do Azure.

No **arquivo** menu, clique em **New**, em seguida, clique em **projeto**. Partir **modelos instalados**, no Visual c#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**. Nomeie o projeto "AzureApp" e clique em **Okey**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

No **novo serviço de nuvem do Windows Azure** caixa de diálogo, clique duas vezes em **função de trabalho**. Deixe o nome padrão ("WorkerRole1"). Esta etapa adiciona uma função de trabalho à solução. Clique em **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

A solução do Visual Studio criado contém dois projetos:

- &quot;AzureApp&quot; define as funções e a configuração para o aplicativo do Azure.
- &quot;WorkerRole1&quot; contém o código da função de trabalho.

Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial usa uma única função.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicionar a API da Web e pacotes do OWIN

Do **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca**, em seguida, clique em **Package Manager Console**.

Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Adicionar um ponto de extremidade HTTP

No Gerenciador de soluções, expanda o projeto AzureApp. Expanda o nó funções, WorkerRole1 com o botão direito e selecione **propriedades**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Clique em **pontos de extremidade**e, em seguida, clique em **Adicionar ponto de extremidade**.

No **protocolo** lista suspensa, selecione "http". Na **porta pública** e **porta privada**, digite 80. Esses números de porta podem ser diferentes. A porta pública é o que os clientes usam quando eles enviam uma solicitação para a função.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurar API da Web para hospedar internamente

No Gerenciador de soluções, com o botão direito do mouse no projeto WorkerRole1 e selecione **Add** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API da Web

Em seguida, adicione uma classe de controlador de API da Web. Clique com botão direito no projeto WorkerRole1 e selecione **Add** / **classe**. Nomeie a classe TestController. Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Para simplificar, este controlador apenas define dois métodos GET que retornam texto sem formatação.

## <a name="start-the-owin-host"></a>Iniciar o Host OWIN

Abra o arquivo WorkerRole.cs. Essa classe define o código que é executado quando a função de trabalho é iniciada e parada.

Adicione a seguinte instrução using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Adicionar um **IDisposable** membro para o `WorkerRole` classe:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

No `OnStart` método, adicione o seguinte código para iniciar o host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

O **WebApp.Start** método inicia o host do OWIN. O nome da `Startup` classe é um parâmetro de tipo para o método. Por convenção, o host chamará o `Configure` método dessa classe.

Substituir a `OnStop` descartar o  *\_aplicativo* instância:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Aqui está o código completo para WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure. Dependendo de suas configurações de firewall, você precisa permitir que o emulador através do firewall.

> [!NOTE]
> Se você receber uma exceção semelhante à seguinte, consulte [esta postagem de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para uma solução alternativa. "Não foi possível carregar arquivo ou assembly ' Microsoft. owin, versão = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou uma de suas dependências. Definição do manifesto do assembly localizado não coincide com a referência de assembly. (Exceção de HRESULT: 0x80131040) "


O emulador de computação atribui um endereço IP local para o ponto de extremidade. Você pode encontrar o endereço IP, exibindo a interface de usuário do emulador de computação. Clique com botão direito no ícone do emulador na tarefa de área de notificação da barra e, em seguida, selecione **mostrar IU do emulador de computação**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Localize o endereço IP em implantações de serviços de implantação [id], detalhes do serviço. Abra um navegador da web e navegue até http://<em>endereço</em>/teste/1, onde <em>endereço</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80/test/1`. Você deve ver a resposta do controlador de API da Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Para esta etapa, você deve ter uma conta do Azure. Se você ainda não tiver uma, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

No Gerenciador de soluções, clique com botão direito no projeto AzureApp. Selecione **Publicar**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Se você não estiver conectado à sua conta do Azure, clique em **Sign In**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Depois que você está conectado, escolha uma assinatura e clique em **próxima**.

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
