---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Habilitando a autenticação do Windows no Katana | Microsoft Docs
author: MikeWasson
description: 'Este artigo mostra como habilitar a autenticação do Windows no Katana. Ele aborda os dois cenários: usando o IIS para hospedar Katana e usando HttpListener para hospedar internamente Kat...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: e7fbe6af323ecdc09b4d79073f506c5ee056f30f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391606"
---
<a name="enabling-windows-authentication-in-katana"></a>Habilitando a autenticação do Windows no Katana
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este artigo mostra como habilitar a autenticação do Windows no Katana. Ele aborda os dois cenários: usando o IIS para hospedar Katana e usando HttpListener para hospedar internamente o Katana em um processo personalizado. Graças ao Barry Dorrans, David Matson e Chris Ross pela revisão deste artigo.


Katana é a implementação da Microsoft [OWIN](http://owin.org/), Open Web Interface para .NET. Você pode ler uma introdução ao OWIN e Katana [aqui](an-overview-of-project-katana.md). A arquitetura OWIN tem várias camadas:

- Host: Gerencia o processo no qual o pipeline do OWIN é executado.
- Servidor: Abre um soquete de rede e de escuta as solicitações.
- Middleware: Processa a solicitação e resposta HTTP.

Atualmente, o Katana fornece dois servidores, que dão suporte a autenticação integrada do Windows:

- **Microsoft.Owin.Host.SystemWeb**. Usa o IIS com o pipeline do ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Usa [HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Este servidor é atualmente a opção padrão quando a hospedagem interna Katana.

> [!NOTE]
> Katana atualmente não fornece o middleware OWIN para a autenticação do Windows, porque essa funcionalidade já está disponível nos servidores.


## <a name="windows-authentication-in-iis"></a>Autenticação do Windows no IIS

Usando systemweb, você pode simplesmente habilitar autenticação do Windows no IIS.

Vamos começar criando um novo aplicativo ASP.NET, usando o modelo de projeto "Aplicativo Web vazio ASP.NET".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Em seguida, adicione pacotes NuGet. Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Agora, adicione uma classe chamada `Startup` com o código a seguir:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Isso é tudo o que você precisa criar um aplicativo "Hello world" para OWIN, em execução no IIS. Pressione F5 para depurar o aplicativo. Você deve ver "Hello World!" Na janela do navegador.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Em seguida, podemos habilitar autenticação do Windows no IIS Express. Dos **modo de exibição** menu, selecione **propriedades**. Clique no nome do projeto no Gerenciador de soluções para exibir as propriedades do projeto.

No **propriedades** janela, defina **autenticação anônima** para **desabilitado** e defina **autenticação do Windows** para  **Habilitado**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Quando você executa o aplicativo do Visual Studio, o IIS Express exigirá as credenciais do usuário Windows. Você pode ver isso por meio [Fiddler](http://fiddler2.com/home) ou HTTP de outra ferramenta de depuração. Aqui está um exemplo de resposta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Os cabeçalhos de WWW-Authenticate nesta resposta indicam que o servidor oferece suporte a [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocolo, que usa o Kerberos ou NTLM.

Posteriormente, quando você implanta o aplicativo em um servidor, siga [essas etapas](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar a autenticação do Windows no IIS nesse servidor.

## <a name="windows-authentication-in-httplistener"></a>Autenticação do Windows no HttpListener

Se você estiver usando Microsoft.Owin.Host.HttpListener para hospedar internamente o Katana, você pode habilitar a autenticação do Windows diretamente na **HttpListener** instância.

Primeiro, crie um novo aplicativo de console. Em seguida, adicione pacotes NuGet. Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Agora, adicione uma classe chamada `Startup` com o código a seguir:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Essa classe implementa o mesmo exemplo "Hello world" de antes, mas ele também define a autenticação do Windows como o esquema de autenticação.

Dentro de `Main` funcionar, iniciar o pipeline do OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Você pode enviar uma solicitação no Fiddler para confirmar que o aplicativo está usando a autenticação do Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Tópicos relacionados

[Uma visão geral do projeto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Noções básicas sobre a autenticação de formulários do OWIN no MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
