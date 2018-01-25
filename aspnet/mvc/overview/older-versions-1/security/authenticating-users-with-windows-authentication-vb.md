---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: "Autenticar usuários com a autenticação do Windows (VB) | Microsoft Docs"
author: microsoft
description: "Saiba como usar a autenticação do Windows no contexto de um aplicativo MVC. Você aprenderá como habilitar a autenticação do Windows em co do seu aplicativo da web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 63b1266e03041c4261e71fd25e988c63932b503e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Autenticar usuários com a autenticação do Windows (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como usar a autenticação do Windows no contexto de um aplicativo MVC. Você aprenderá como habilitar a autenticação do Windows no arquivo de configuração do aplicativo web e como configurar a autenticação com o IIS. Por fim, você aprenderá a usar o atributo [autorizar] para restringir o acesso a ações do controlador para grupos ou usuários específicos do Windows.


O objetivo deste tutorial é explicar como você pode tirar proveito da segurança de recursos internos do serviços de informações da Internet para senha protegem os modos de exibição em seus aplicativos MVC. Você aprenderá a permitir ações do controlador a ser invocado apenas por usuários específicos do Windows ou os usuários que são membros de grupos específicos do Windows.

Usando a autenticação do Windows faz sentido quando você estiver criando um site da empresa interno (um site de intranet) e desejar que os usuários sejam capazes de usar seus nomes de usuário padrão do Windows e senhas ao acessar o site. Se você estiver criando um outwards voltada para o site da Web (um site da Internet), considere usar autenticação de formulários.

#### <a name="enabling-windows-authentication"></a>Habilitar a autenticação do Windows

Quando você cria um novo aplicativo ASP.NET MVC, a autenticação do Windows não está habilitada por padrão. Autenticação de formulários é o tipo de autenticação padrão habilitado para aplicativos MVC. Você deve habilitar a autenticação do Windows, modificando o arquivo de configuração (Web. config) da web do seu aplicativo MVC. Localizar o &lt;autenticação&gt; seção e modifique-o para usar o Windows em vez da autenticação de formulários como este:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Quando você habilitar a autenticação do Windows, seu servidor web se torna responsável pela autenticação de usuários. Normalmente, há dois tipos diferentes de servidores web que você usar ao criar e implantar um aplicativo ASP.NET MVC.

Primeiro, ao desenvolver um aplicativo MVC, use o servidor de Web de desenvolvimento ASP.NET incluídos com o Visual Studio. Por padrão, o servidor de Web de desenvolvimento ASP.NET executa todas as páginas no contexto da conta do Windows atual (qualquer conta usada para fazer logon no Windows).

O servidor de Web de desenvolvimento ASP.NET também oferece suporte a autenticação NTLM. Você pode habilitar a autenticação NTLM, clicando duas vezes no nome de seu projeto na janela do Gerenciador de soluções e selecione Propriedades. Em seguida, selecione a guia Web e marque a caixa de seleção NTLM (consulte a Figura 1).

**Figura 1 – habilitando a autenticação NTLM para o servidor de Web de desenvolvimento do ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Para um aplicativo web de produção, lado, você usa o IIS como seu servidor web. O IIS dá suporte a vários tipos de autenticação, incluindo:

- Autenticação básica – definidos como parte do protocolo HTTP 1.0. Envia os nomes de usuário e senhas em texto não criptografado (codificação Base64) pela Internet. -A autenticação Digest – envia um hash de uma senha, em vez da própria senha, através da internet. -Autenticação integrada do Windows (NTLM) – o melhor tipo de autenticação a ser usado em ambientes de intranet usando o windows. -Certificados de autenticação – permite a autenticação usando um certificado de cliente. O certificado é mapeado para uma conta de usuário do Windows.

> [!NOTE] 
> 
> Para obter uma visão mais detalhada desses tipos diferentes de autenticação, consulte [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Você pode usar o Gerenciador de serviços de informações da Internet para habilitar um determinado tipo de autenticação. Lembre-se de que todos os tipos de autenticação não estão disponíveis no caso de cada sistema operacional. Além disso, se você estiver usando o IIS 7.0 com o Windows Vista, você precisará habilitar os diferentes tipos de autenticação do Windows antes de aparecerem no Gerenciador de serviços de informações da Internet. Abra **painel de controle, programas, programas e recursos, ativar recursos do Windows ou desativar**e expanda o nó Serviços de informações da Internet (consulte a Figura 2).

**Figura 2 – os recursos do IIS do Windows habilitando**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Usando serviços de informações da Internet, você pode habilitar ou desabilitar diferentes tipos de autenticação. Por exemplo, a Figura 3 ilustra desabilitar a autenticação anônima e habilitar autenticação do Windows integrada (NTLM) ao usar o IIS 7.0.

**Figura 3 – habilitando a autenticação integrada do Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Windows de autorizar usuários e grupos

Depois de habilitar a autenticação do Windows, você pode usar o &lt;autorizar&gt; atributo para controlar o acesso aos controladores ou ações do controlador. Esse atributo pode ser aplicado a um controlador MVC inteiro ou uma ação do controlador específico.

Por exemplo, o controlador Home na listagem 1 expõe três ações denominadas Index (), CompanySecrets() e StephenSecrets(). Qualquer pessoa pode chamar a ação Index (). No entanto, somente os membros do grupo de gerentes de local do Windows podem chamar a ação de CompanySecrets(). Por fim, somente o usuário de domínio do Windows chamado Stephen (no domínio Redmond) pode chamar a ação de StephenSecrets().

**Listando 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Devido ao Windows User Account Control (UAC), ao trabalhar com o Windows Vista ou Windows Server 2008, o grupo de administradores local se comportará Diferentemente de outros grupos. O &lt;autorizar&gt; atributo corretamente não reconhecerão um membro do grupo Administradores local, a menos que você modificar as configurações de UAC do seu computador.


Exatamente o que acontece quando você tentar chamar uma ação do controlador sem as permissões corretas de sendo depende do tipo de autenticação habilitado. Por padrão, ao usar o servidor de desenvolvimento ASP.NET, você obtém uma página em branco. A página é atendida com um **401 não autorizado** Status de resposta HTTP.

Se, por outro lado, você estiver usando o IIS com autenticação anônima desabilitada e a autenticação básica habilitada, você estiver recebendo um prompt de logon da caixa de diálogo quando você solicitar a página protegida (consulte a Figura 4).

**Figura 4 – caixa de diálogo de logon de autenticação básica**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Resumo

Este tutorial explica como você pode usar a autenticação do Windows no contexto de um aplicativo ASP.NET MVC. Você aprendeu como habilitar a autenticação do Windows no arquivo de configuração do aplicativo web e configurar a autenticação com o IIS. Por fim, você aprendeu a usar o &lt;autorizar&gt; atributo para restringir o acesso a ações do controlador para grupos ou usuários específicos do Windows.

>[!div class="step-by-step"]
[Anterior](authenticating-users-with-forms-authentication-vb.md)
[Próximo](preventing-javascript-injection-attacks-vb.md)
