---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hospedar internamente o ASP.NET Web API 1 (c#) | Microsoft Docs
author: MikeWasson
description: API Web ASP.NET não requer o IIS. Você pode hospedar internamente uma API da web em seu próprio processo de host. Este tutorial mostra como hospedar uma API da web dentro de um console applic...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830431"
---
<a name="self-host-aspnet-web-api-1-c"></a>Hospedar internamente o ASP.NET Web API 1 (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

> API Web ASP.NET não requer o IIS. Você pode hospedar internamente uma API da web em seu próprio processo de host. Este tutorial mostra como hospedar uma API da web dentro de um aplicativo de console.
> 
> **Novos aplicativos devem usar o OWIN para auto-hospedar a API da Web.** Ver [usar o OWIN para auto-hospedar a API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Criar o projeto de aplicativo de Console

Inicie o Visual Studio e selecione **Novo projeto** na página **Iniciar**. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Sob **Visual c#**, selecione **Windows**. Na lista de modelos de projeto, selecione **aplicativo de Console**. Nomeie o projeto &quot;SelfHost&quot; e clique em **Okey**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Defina a estrutura de destino (Visual Studio 2010)

Se você estiver usando o Visual Studio 2010, altere a estrutura de destino para o .NET Framework 4.0. (Por padrão, os destinos de modelo de projeto do [perfil do .net Framework Client](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

No Gerenciador de soluções, clique com botão direito no projeto e selecione **propriedades**. No **estrutura de destino** lista suspensa lista, altere a estrutura de destino para o .NET Framework 4.0. Quando solicitado para aplicar a alteração, clique em **Sim**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalar o Gerenciador de pacotes NuGet

O Gerenciador de pacotes do NuGet é a maneira mais fácil para adicionar os assemblies de API da Web a um projeto não seja ASP.NET.

Para verificar se o Gerenciador de pacotes do NuGet está instalado, clique o **ferramentas** menu do Visual Studio. Se você vir um item de menu chamado **Library Package Manager**, em seguida, você tem o Gerenciador de pacotes NuGet.

Para instalar o Gerenciador de pacotes NuGet:

1. Inicie o Visual Studio.
2. Dos **ferramentas** menu, selecione **extensões e atualizações**.
3. No **extensões e atualizações** caixa de diálogo, selecione **Online**.
4. Se você não vir "Gerenciador de pacotes do NuGet", digite "Gerenciador de pacotes do nuget" na caixa de pesquisa.
5. Selecione o Gerenciador de pacotes do NuGet e clique em **baixar**.
6. Após a conclusão do download, você será solicitado a instalar.
7. Depois que a instalação for concluída, você talvez seja solicitado para reiniciar o Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Adicione o pacote do NuGet da API Web

Depois de instalar o Gerenciador de pacotes NuGet, adicione o pacote de Self de API da Web ao seu projeto.

1. Dos **ferramentas** menu, selecione **Library Package Manager**. *Observação*: se você não vê esse menu item, certifique-se de que NuGet Package Manager instalada corretamente.
2. Selecione **gerenciar pacotes NuGet para solução...**
3. No **gerenciar pacotes NuGet** caixa de diálogo, selecione **Online**.
4. Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Selecione o pacote de Host de autoatendimento do ASP.NET Web API e clique em **instalar**.
6. Depois de instala o pacote, clique em **feche** para fechar a caixa de diálogo.

> [!NOTE]
> Certifique-se de instalar o pacote denominado Microsoft.AspNet.WebApi.SelfHost, não AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Criar o modelo e o controlador

Este tutorial usa as mesmas classes de modelo e controlador como o [guia de Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.

Adicione uma classe pública denominada `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Adicione uma classe pública denominada `ProductsController`. Derive essa classe de **ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Para obter mais informações sobre o código neste controlador, consulte o [guia de Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial. Esse controlador define três ações GET:

| URI | Descrição |
| --- | --- |
| produtos/api / | Obtenha uma lista de todos os produtos. |
| /API/produtos/*id* | Obter um produto por ID. |
| /api/products/?category=*category* | Obtenha uma lista de produtos por categoria. |

## <a name="host-the-web-api"></a>Hospedar a API da Web

Abra o arquivo Program.cs e adicione as seguintes instruções using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Adicione o seguinte código para o **programa** classe.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Opcional) Adicionar uma reserva de Namespace de URL HTTP

Esse aplicativo atende a `http://localhost:8080/`. Por padrão, escutando em um determinado endereço HTTP requer privilégios de administrador. Quando você executar o tutorial, portanto, você pode obter esse erro: "HTTP não foi possível registrar a URL http://+:8080/" há duas maneiras de evitar esse erro:

- Executar o Visual Studio com permissões de administrador com privilégios elevados, ou
- Use Netsh.exe para conceder as permissões de conta para reservar a URL.

Para usar Netsh.exe, abra um prompt de comando com privilégios de administrador e digite o seguinte comando: comando a seguir:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

em que *máquina \ nomedeusuário* é sua conta de usuário.

Quando tiver terminado de hospedagem interna, não se esqueça de excluir a reserva:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Chamar a API Web de um aplicativo cliente (c#)

Vamos escrever um aplicativo de console simples que chama a API da web.

Adicione um novo projeto de aplicativo de console à solução:

- No Gerenciador de soluções, a solução com o botão direito e selecione **adicionar novo projeto**.
- Criar um novo aplicativo de console chamado &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Use o Gerenciador de pacotes de NuGet para adicionar o pacote de bibliotecas de núcleo do ASP.NET Web API:

- No menu Ferramentas, selecione **Gerenciador de pacotes de biblioteca**.
- Selecione **gerenciar pacotes NuGet para solução...**
- No **gerenciar pacotes NuGet** caixa de diálogo, selecione **Online**.
- Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Selecione o pacote de bibliotecas de cliente do Microsoft ASP.NET Web API e clique em **instalar**.

Adicione uma referência no ClientApp ao projeto SelfHost:

- No Gerenciador de soluções, clique com botão direito no projeto ClientApp.
- Selecione **Adicionar Referência**.
- No **Gerenciador de referências** caixa de diálogo, em **solução**, selecione **projetos**.
- Selecione o projeto SelfHost.
- Clique em **OK**.

![](self-host-a-web-api/_static/image6.png)

Abra o arquivo Client/Program.cs. Adicione o seguinte **usando** instrução:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Adicionar um estático **HttpClient** instância:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Adicione os seguintes métodos para listar todos os produtos, um produto por ID de lista e listam os produtos por categoria.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Cada um desses métodos segue o mesmo padrão:

1. Chame **HttpClient.GetAsync** para enviar uma solicitação GET para o URI apropriado.
2. Chame **HttpResponseMessage.EnsureSuccessStatusCode**. Esse método gera uma exceção se o status da resposta HTTP é um código de erro.
3. Chame **ReadAsAsync&lt;T&gt;**  para desserializar um tipo CLR da resposta HTTP. Esse método é um método de extensão definido no **System.Net.Http.HttpContentExtensions**.

O **GetAsync** e **ReadAsAsync** métodos são assíncronas. Elas retornam **tarefa** objetos que representam a operação assíncrona. Introdução a **resultado** propriedade bloqueará o thread até que a operação seja concluída.

Para obter mais informações sobre como usar o HttpClient, incluindo como fazer chamadas sem bloqueio, consulte [chamar um Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Antes de chamar esses métodos, defina a propriedade BaseAddress na instância do HttpClient para "`http://localhost:8080`". Por exemplo:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Essa saída a seguir. (Lembre-se de executar o aplicativo SelfHost primeiro.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
