---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Criar um aplicativo de cliente do OData v4 (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: daa39fbbb4ff17d61f71bf2a642a9c2260b353e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-client-app-c"></a>Criar um aplicativo de cliente do OData v4 (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

No tutorial anterior, você criou um serviço OData básico que suporta operações CRUD. Agora vamos criar um cliente para o serviço.

Inicie uma nova instância do Visual Studio e crie um novo projeto de aplicativo de console. No **novo projeto** caixa de diálogo, selecione **instalado** &gt; **modelos** &gt; **Visual C#** &gt; **Windows Desktop**e selecione o **aplicativo de Console** modelo. Nomeie o projeto &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Você também pode adicionar o aplicativo de console para a mesma solução do Visual Studio que contém o serviço OData.


## <a name="install-the-odata-client-code-generator"></a>Instalar o gerador de código de cliente OData

Do **ferramentas** menu, selecione **extensões e atualizações**. Selecione **Online** &gt; **Galeria do Visual Studio**. Na caixa Pesquisar, procure &quot;gerador de código de cliente OData&quot;. Clique em **baixar** para instalar o VSIX. Você pode ser solicitado a reiniciar o Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Executar o serviço OData localmente

Execute o projeto ProductService do Visual Studio. Por padrão, o Visual Studio inicia um navegador para a raiz do aplicativo. Observe o URI. Isso será necessário na próxima etapa. Deixe o aplicativo em execução.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Se você colocar os dois projetos na mesma solução, certifique-se de executar o projeto ProductService sem depuração. Na próxima etapa, você precisará manter o serviço em execução enquanto você modificar o projeto de aplicativo de console.


## <a name="generate-the-service-proxy"></a>Gerar o Proxy de serviço

O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData. O proxy converte chamadas de método em solicitações HTTP. Você criará a classe proxy executando um [modelo T4](https://msdn.microsoft.com/en-us/library/bb126445.aspx).

Clique com botão direito no projeto. Selecione **adicionar** &gt; **Novo Item**.

![](create-an-odata-v4-client-app/_static/image5.png)

No **Adicionar Novo Item** caixa de diálogo, selecione **itens do Visual C#** &gt; **código** &gt; **cliente OData**. Nomear o modelo &quot;ProductClient.tt&quot;. Clique em **adicionar** e clique no aviso de segurança.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

Neste ponto, você obterá um erro, que pode ser ignorada. O Visual Studio executa automaticamente o modelo, mas o modelo precisa ser algumas configurações primeiro.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Abra o arquivo ProductClient.odata.config. No `Parameter` elemento, cole no URI do projeto ProductService (etapa anterior). Por exemplo:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Execute novamente o modelo. No Gerenciador de soluções, clique com botão direito do arquivo ProductClient.tt e selecione **executar personalizado ferramenta**.

O modelo cria um arquivo de código chamado ProductClient.cs que define o proxy. À medida que desenvolve seu aplicativo, se você alterar o ponto de extremidade OData, execute o modelo novamente para atualizar o proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Use o Proxy de serviço para chamar o serviço OData

Abra o arquivo Program.cs e substitua o código clichê pelo seguinte.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Substitua o valor de *serviceUri* com o URI do serviço anteriores.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Quando você executa o aplicativo, ele deve gerar o seguinte:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
