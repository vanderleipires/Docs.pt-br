---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detecção de classe de inicialização OWIN | Microsoft Docs
author: Praburaj
description: Este tutorial mostra como configurar a classe de inicialização que OWIN é carregado. Para obter mais informações sobre OWIN, consulte uma visão geral de projeto Katana. Este tutorial foi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 33d2745b24387419e5614c62c2d46948427b242a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="owin-startup-class-detection"></a>Detecção de classe de inicialização OWIN
====================
por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial mostra como configurar a classe de inicialização que OWIN é carregado. Para obter mais informações sobre OWIN, consulte [uma visão geral de projeto Katana](an-overview-of-project-katana.md). Este tutorial foi escrito de Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Pré-requisitos
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Detecção de classe de inicialização OWIN

 Cada aplicativo OWIN tem uma classe de inicialização em que você especificar componentes para o pipeline do aplicativo. Você pode se conectar a sua classe de inicialização com o tempo de execução de maneiras diferentes, dependendo do modelo de hospedagem que você escolher (OwinHost, IIS e o IIS Express). A classe de inicialização mostrada neste tutorial pode ser usada em todos os aplicativos de hospedagem. Você se conectar a classe de inicialização com o tempo de execução hospedagem usando uma dessas abordagens:  

1. **Convenção de nomenclatura**: Katana procura uma classe denominada `Startup` no namespace correspondentes ao nome de assembly ou o namespace global.
2. **Atributo OwinStartup**: essa é a abordagem a maioria dos desenvolvedores levará para especificar a classe de inicialização. O seguinte atributo definirá a classe de inicialização o `TestStartup` classe no `StartupDemo` namespace. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   O `OwinStartup` atributo substitui a convenção de nomenclatura. Você também pode especificar um nome amigável com esse atributo, no entanto, usar um nome amigável requer que você também use o `appSetting` elemento no arquivo de configuração.
3. **O elemento appSetting no arquivo de configuração**: O `appSetting` elemento substitui o `OwinStartup` atributo e a convenção de nomenclatura. Você pode ter várias classes de inicialização (cada usando um `OwinStartup` atributo) e configurar qual classe de inicialização será carregado em um arquivo de configuração usando marcação semelhante ao seguinte:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   A seguinte chave, que especifica explicitamente a classe de inicialização e o assembly também pode ser usada: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   O XML no arquivo de configuração a seguir especifica um nome de classe de inicialização amigável do `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   A marcação acima deve ser usada com o seguinte `OwinStartup` atributo que especifica um nome amigável e faz com que o `ProductionStartup2` classe a ser executada.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Para desabilitar a descoberta de inicialização OWIN, adicione o `appSetting owin:AutomaticAppStartup` com um valor de `"false"` no arquivo Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Criar um aplicativo Web do ASP.NET usando a inicialização de OWIN

1. Criar um aplicativo de web Asp.Net vazio e nomeie- **StartupDemo**. -Instalar `Microsoft.Owin.Host.SystemWeb` usando o NuGet package manager. Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**e, em seguida, **Package Manager Console**. Insira o seguinte comando:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Adicione uma classe de inicialização OWIN. No Visual Studio 2013 clique com botão direito no projeto e selecione **Adicionar classe**. - na **Adicionar Novo Item** caixa de diálogo, digite *OWIN* no campo de pesquisa e altere o nome para Startup.cs, e, em seguida, clique em **adicionar**.  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   Na próxima vez em que você deseja adicionar um *classe de inicialização Owin*, para que esteja disponível no **adicionar** menu.  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   Como alternativa, você pode clique com botão direito no projeto e selecione **adicionar**, em seguida, selecione **Novo Item**e, em seguida, selecione o **classe de inicialização Owin**.  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- Substitua o código gerado no *Startup.cs* arquivo com o seguinte:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  O `app.Use` expressão lambda é usada para registrar o componente de middleware especificado para o pipeline OWIN. Nesse caso estamos configurando registro em log de solicitações de entrada antes de responder à solicitação de entrada. O `next` parâmetro é o delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [tarefa](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) para o próximo componente no pipeline. O `app.Run` expressão lambda conecta o solicitações de entrada do pipeline e fornece o mecanismo de resposta.
     > [!NOTE]
     > No código acima é ter comentada o `OwinStartup` atributo e nós depende da convenção de executar a classe denominada `Startup` .-pressione ***F5*** para executar o aplicativo. Clique em atualizar algumas vezes.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  Observação: O número mostrado nas imagens neste tutorial não corresponderá o número exibido. A cadeia de caracteres de milissegundos é usada para mostrar uma nova resposta quando você atualizar a página.  
  Você pode ver as informações de rastreamento de **saída** janela.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Adicionar mais Classes de inicialização

Nesta seção, vamos adicionar de outra classe de inicialização. Você pode adicionar vários classe de inicialização OWIN para seu aplicativo. Por exemplo, você talvez queira criar classes de inicialização para o desenvolvimento, teste e produção.

1. Crie uma nova classe de inicialização OWIN e nomeie- `ProductionStartup`.
2. Substitua o código gerado pelo seguinte:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Pressione F5 de controle para executar o aplicativo. O `OwinStartup` atributo especifica a classe de inicialização de produção é executada.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Crie outra classe de inicialização OWIN e nomeie- `TestStartup`.
5. Substitua o código gerado pelo seguinte:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   O `OwinStartup` acima de sobrecarga de atributo especifica `TestingConfiguration` como o *amigável* nome da classe de inicialização.
6. Abra o *Web. config* de arquivos e adicionar a chave de inicialização do aplicativo OWIN que especifica o nome amigável da classe de inicialização:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Pressione F5 de controle para executar o aplicativo. O elemento de configurações do aplicativo usa precedência e o teste de configuração é executada.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Remover o *amigável* nome do `OwinStartup` atributo o `TestStartup` classe.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Substitua a chave de inicialização do aplicativo OWIN no *Web. config* arquivo com o seguinte:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Reverter o `OwinStartup` atributo em cada classe para o código do atributo padrão gerado pelo Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Cada uma das chaves de inicialização do aplicativo OWIN abaixo fará com que a classe de produção executar. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    A última chave de inicialização especifica o método de configuração de inicialização. A seguinte chave de inicialização do aplicativo OWIN permite que você altere o nome da classe de configuração para `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Usando Owinhost.exe

1. Substitua o arquivo Web. config com a seguinte marcação:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   A última chave wins, isso neste caso `TestStartup` for especificado.
2. Instale o Owinhost do PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navegue até a pasta do aplicativo (a pasta que contém o *Web. config* arquivo) e em um prompt de comando e digite: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Mostra a janela de comando: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Iniciar um navegador com a URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost cumprido as convenções de inicialização listadas acima.
5. Na janela de comando, pressione Enter para sair do OwinHost.
6. No `ProductionStartup` da classe, adicione o seguinte atributo OwinStartup que especifica um nome amigável do *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. No prompt de comando e tipo: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   A classe de inicialização de produção é carregada.  
    ![](owin-startup-class-detection/_static/image9.png)  
   Nosso aplicativo tem várias classes de inicialização e, neste exemplo, podemos adiaram qual classe de inicialização para carregar até o tempo de execução.
8. As seguintes opções de inicialização do tempo de execução de teste:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
