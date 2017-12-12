---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "Próprio Host ASP.NET Web API 1 (c#) | Microsoft Docs"
author: MikeWasson
description: "API da Web do ASP.NET não requer o IIS. Automaticamente, você pode hospedar uma API da web em seu próprio processo de host. Este tutorial mostra como hospedar uma API da web dentro de um console applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a>Próprio Host ASP.NET Web API 1 (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

> API da Web do ASP.NET não requer o IIS. Automaticamente, você pode hospedar uma API da web em seu próprio processo de host. Este tutorial mostra como hospedar uma API da web dentro de um aplicativo de console.
> 
> **Novos aplicativos devem usar OWIN para hospedagem interna de API da Web.** Consulte [usar OWIN para hospedar o ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Criar o projeto de aplicativo de Console

Inicie o Visual Studio e selecione **novo projeto** do **iniciar** página. Ou, do **arquivo** menu, selecione **novo** e **projeto**.

No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó. Em **Visual C#**, selecione **Windows**. Na lista de modelos de projeto, selecione **aplicativo de Console**. Nomeie o projeto &quot;SelfHost&quot; e clique em **Okey**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Definir a estrutura de destino (Visual Studio 2010)

Se você estiver usando o Visual Studio 2010, altere a estrutura de destino para o .NET Framework 4.0. (Por padrão, os destinos de modelo de projeto de [perfil do .net Framework Client](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**. No **framework de destino** suspensa lista, altere a estrutura de destino para o .NET Framework 4.0. Quando solicitado para aplicar a alteração, clique em **Sim**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalar o NuGet Package Manager

O NuGet Package Manager é a maneira mais fácil para adicionar os assemblies de API da Web a um projeto de não-ASP.NET.

Para verificar se o NuGet Package Manager está instalado, clique o **ferramentas** menu do Visual Studio. Se você vir um item de menu chamado **Gerenciador de biblioteca de pacote**, em seguida, você tem o NuGet Package Manager.

Para instalar o NuGet Package Manager:

1. Inicie o Visual Studio.
2. Do **ferramentas** menu, selecione **extensões e atualizações**.
3. No **extensões e atualizações** caixa de diálogo, selecione **Online**.
4. Se você não vir "NuGet Package Manager", digite "nuget package manager" na caixa de pesquisa.
5. Selecione o NuGet Package Manager e clique em **baixar**.
6. Após a conclusão do download, você será solicitado a instalar.
7. Após a conclusão da instalação, você pode solicitado a reiniciar o Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Adicionar o pacote de NuGet de API da Web

Depois que o NuGet Package Manager está instalado, adicione o pacote de Self-Host de API da Web ao seu projeto.

1. Do **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**. *Observação*: se você não vir esse menu item, certifique-se de que NuGet Package Manager está instalado corretamente.
2. Selecione **gerenciar pacotes NuGet para solução...**
3. No **gerenciar preciosidade pacotes** caixa de diálogo, selecione **Online**.
4. Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Selecione o pacote ASP.NET Web API Self Host e clique em **instalar**.
6. Depois de instala o pacote, clique em **fechar** para fechar a caixa de diálogo.

> [!NOTE]
> Certifique-se de instalar o pacote denominado Microsoft.AspNet.WebApi.SelfHost, não AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Criar o modelo e o controlador

Este tutorial usa as mesmas classes de modelo e o controlador como o [Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.

Adicionar uma classe pública chamada `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Adicionar uma classe pública chamada `ProductsController`. Derive essa classe de **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Para obter mais informações sobre o código neste controlador, consulte o [Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial. Esse controlador define três ações GET:

| URI | Descrição |
| --- | --- |
| produtos/api / | Obter uma lista de todos os produtos. |
| /API/produtos/*id* | Obter um produto por ID. |
| /API/produtos /? categoria =*categoria* | Obter uma lista de produtos por categoria. |

## <a name="host-the-web-api"></a>Hospedar a API da Web

Abra o arquivo Program.cs e adicione o seguinte usando instruções:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Adicione o seguinte código para o **programa** classe.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Opcional) Adicionar uma reserva de Namespace de URL de HTTP

Este aplicativo ouve `http://localhost:8080/`. Por padrão, escutando em um determinado endereço HTTP requer privilégios de administrador. Quando você executar o tutorial, portanto, você pode obter esse erro: "HTTP não pôde registrar a URL http://+:8080/" há duas maneiras de evitar esse erro:

- Execute o Visual Studio com permissões de administrador com privilégios elevados, ou
- Use Netsh.exe para conceder as permissões de conta para reservar a URL.

Para usar Netsh.exe, abra um prompt de comando com privilégios de administrador e digite o seguinte comando: comando a seguir:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

onde *máquina \ nomedeusuário* é sua conta de usuário.

Quando você terminar de hospedagem interna, certifique-se de excluir a reserva:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Chame a API da Web de um aplicativo cliente (c#)

Vamos escrever um aplicativo de console simples que chama a API da web.

Adicione um novo projeto de aplicativo de console à solução:

- No Gerenciador de soluções, a solução e selecione **adicionar novo projeto**.
- Criar um novo aplicativo de console chamado &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Use o Gerenciador de pacote de NuGet para adicionar o pacote ASP.NET Web API Core Libraries:

- No menu Ferramentas, selecione **Gerenciador de pacotes de biblioteca**.
- Selecione **gerenciar pacotes NuGet para solução...**
- No **gerenciar pacotes NuGet** caixa de diálogo, selecione **Online**.
- Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Selecione o pacote de bibliotecas de cliente do Microsoft ASP.NET Web API e clique em **instalar**.

Adicione uma referência em ClientApp ao projeto SelfHost:

- No Gerenciador de soluções, clique com botão direito no projeto ClientApp.
- Selecione **adicionar referência**.
- No **Gerenciador de referências** caixa de diálogo, em **solução**, selecione **projetos**.
- Selecione o projeto SelfHost.
- Clique em **OK**.

![](self-host-a-web-api/_static/image6.png)

Abra o arquivo Client/Program.cs. Adicione o seguinte **usando** instrução:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Adicionar um estático **HttpClient** instância:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Adicione os seguintes métodos para listar todos os produtos, um produto por ID de lista e lista produtos por categoria.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Cada um desses métodos segue o mesmo padrão:

1. Chamar **HttpClient.GetAsync** para enviar uma solicitação GET para o URI apropriado.
2. Chamar **HttpResponseMessage.EnsureSuccessStatusCode**. Este método lança uma exceção se o status de resposta HTTP for um código de erro.
3. Chamar **ReadAsAsync&lt;T&gt;**  para desserializar um tipo CLR da resposta HTTP. Esse método é um método de extensão, definido em **System.Net.Http.HttpContentExtensions**.

O **GetAsync** e **ReadAsAsync** métodos são assíncronas. Elas retornam **tarefa** objetos que representam a operação assíncrona. Obtendo o **resultados** propriedade bloqueia o thread até que a operação seja concluída.

Para obter mais informações sobre como usar HttpClient, inclusive como fazer chamadas sem bloqueio, consulte [chamar uma Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Antes de chamar esses métodos, defina a propriedade BaseAddress na instância do HttpClient para "`http://localhost:8080`". Por exemplo:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Esta saída a seguir. (Lembre-se de executar o aplicativo SelfHost primeiro.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
