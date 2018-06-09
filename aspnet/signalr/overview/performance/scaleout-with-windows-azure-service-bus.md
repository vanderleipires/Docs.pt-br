---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Expansão do SignalR com o barramento de serviço do Azure | Microsoft Docs
author: MikeWasson
description: Versões de software usada nesta versão do Visual Studio 2013 .NET 4.5 SignalR tópico 2 versões anteriores desta versão do tópico para o SignalR 1. x desse tópico,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "32741534"
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Expansão do SignalR com o barramento de serviço do Azure
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Neste tutorial, você implantará um aplicativo SignalR para uma função do Windows Azure Web, usando o backplane de barramento de serviço para distribuir mensagens para cada instância de função. (Você também pode usar o backplane de barramento de serviço com [aplicativos no serviço de aplicativo do Azure web](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Pré-requisitos:

- Uma conta do Windows Azure.
- O [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 ou 2013.

O backplane de barramento de serviço também é compatível com [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versão 1.1. No entanto, não é compatível com a versão 1.0 do Service Bus for Windows Server.

## <a name="pricing"></a>Preços

O backplane de barramento de serviço usa tópicos para enviar mensagens. Para obter as informações mais recentes sobre preços, consulte [barramento de serviço](https://azure.microsoft.com/pricing/details/service-bus/). No momento da redação deste artigo, você pode enviar 1.000.000 mensagens por mês para menor que US $1. O backplane envia uma mensagem de barramento de serviço para cada invocação de um método de hub SignalR. Também há algumas mensagens de controle para conexões, desconexões, unindo ou deixar grupos e assim por diante. Na maioria dos aplicativos, a maioria do tráfego de mensagem será invocações de método do hub.

## <a name="overview"></a>Visão geral

Antes de entrar para o tutorial detalhado, aqui está uma visão geral das tarefas que você executará.

1. Use o portal do Windows Azure para criar um novo namespace de barramento de serviço.
2. Adicione esses pacotes do NuGet ao seu aplicativo: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) ou [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Crie um aplicativo do SignalR.
4. Adicione o seguinte código ao Startup.cs para configurar o backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Este código configura o backplane com os valores padrão para [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Para obter informações sobre como alterar esses valores, consulte [desempenho SignalR: métricas de expansão](signalr-performance.md#scaleout_metrics).

Para cada aplicativo, escolha um valor diferente de "Nomedoseuaplicativo". Não use o mesmo valor em vários aplicativos.

## <a name="create-the-azure-services"></a>Criar os serviços do Azure

Criar um serviço de nuvem, conforme descrito em [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Siga as etapas na seção "como: criar um serviço de nuvem usando a criação rápida". Para este tutorial, você não precisa carregar um certificado.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Criar um novo namespace de barramento de serviço, conforme descrito em [como barramento de serviço para usar tópicos/assinaturas](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Siga as etapas na seção "Criar um Namespace de serviço".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Certifique-se de selecionar a mesma região para o serviço de nuvem e o namespace do barramento de serviço.


## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Inicie o Visual Studio. Do **arquivo** menu, clique em **novo projeto**.

No **novo projeto** caixa de diálogo caixa, expanda **Visual C#**. Em **modelos instalados**, selecione **nuvem** e, em seguida, selecione **serviço de nuvem do Windows Azure**. Mantenha o padrão do .NET Framework 4.5. Nome do aplicativo ChatService e clique em **Okey**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

No **novo serviço de nuvem do Windows Azure** caixa de diálogo, selecione função Web do ASP.NET. Clique no botão de seta para a direita (**&gt;**) para adicionar a função à sua solução.

Passe o mouse sobre a nova função, então o ícone de lápis visível. Clique nesse ícone para renomear a função. Nome de função "SignalRChat" e clique em **Okey**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

No **novo projeto ASP.NET** caixa de diálogo, selecione **MVC**e clique em Okey.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

O Assistente de projeto cria dois projetos:

- ChatService: Este projeto é o aplicativo do Windows Azure. Ele define as funções do Azure e outras opções de configuração.
- SignalRChat: Este projeto é seu projeto ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Criar o aplicativo de Chat de SignalR

Para criar o aplicativo de chat, siga as etapas no tutorial [Introdução ao SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Use NuGet para instalar as bibliotecas necessárias. Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. No **Package Manager Console** janela, digite os seguintes comandos:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Use o `-ProjectName` opção para instalar os pacotes para o projeto ASP.NET MVC, em vez de projeto do Windows Azure.

## <a name="configure-the-backplane"></a>Configurar o Backplane

No arquivo de Startup.cs do seu aplicativo, adicione o seguinte código:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Agora você precisa obter sua cadeia de conexão do barramento de serviço. No portal do Azure, selecione o namespace de barramento de serviço que você criou e clique no ícone de chave de acesso.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Copie a cadeia de conexão para a área de transferência e, em seguida, cole-o no *connectionString* variável.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Implantar no Azure

No Gerenciador de soluções, expanda o **funções** pasta dentro do projeto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

A função SignalRChat e selecione **propriedades**. Selecione o **configuração** guia. Em **instâncias** selecione 2. Você também pode definir o tamanho da VM **Extrapequeno**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Salve as alterações.

No Gerenciador de soluções, clique com botão direito no projeto ChatService. Selecione **Publicar**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Se esta for sua primeira publicação de tempo para o Windows Azure, você deve baixar suas credenciais. No **publicar** assistente, clique em "Entrar para baixar credenciais". Isso solicitará que você entrar no portal do Windows Azure e baixar um arquivo de configurações de publicação.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Clique em **importação** e selecione o arquivo de configurações de publicação que você baixou.

Clique em **Avançar**. No **configurações de publicação** caixa de diálogo, em **serviço de nuvem**, selecione o serviço de nuvem que você criou anteriormente.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Clique em **Publicar**. Pode levar alguns minutos para implantar o aplicativo e iniciar as máquinas virtuais.

Agora quando você executa o aplicativo, as instâncias de função se comunicar por meio do barramento de serviço do Azure, usando um tópico do barramento de serviço. Um tópico é uma fila de mensagens que permite que vários assinantes.

O backplane cria automaticamente o tópico e as assinaturas. Para ver as assinaturas e a atividade de mensagens, abra o portal do Azure, selecione o namespace de barramento de serviço e clique em "Tópicos de".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Ele poderá levar alguns minutos para que a atividade de mensagem exibida no painel.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR gerencia o tempo de vida do tópico. Como o aplicativo é implantado, não tente excluir tópicos manualmente ou alterar as configurações no tópico.

## <a name="troubleshooting"></a>Solução de problemas

**System. InvalidOperationException "O somente IsolationLevel com suporte é 'IsolationLevel.Serializable'".**

Esse erro pode ocorrer se o nível de transação para uma operação é definido como algo diferente de `Serializable`. Verifique se que nenhuma operação está sendo executada com outros níveis de transação.
