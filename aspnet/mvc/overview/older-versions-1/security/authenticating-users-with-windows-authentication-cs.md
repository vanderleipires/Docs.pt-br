---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Autenticar usuários com a autenticação do Windows (c#) | Microsoft Docs
author: microsoft
description: Saiba como usar a autenticação do Windows no contexto de um aplicativo MVC. Você aprenderá a habilitar a autenticação do Windows dentro do co do seu aplicativo web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 0261f3b989d721e6e9aae8fa5766e02c8bca7b63
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397793"
---
<a name="authenticating-users-with-windows-authentication-c"></a>Autenticar usuários com a autenticação do Windows (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como usar a autenticação do Windows no contexto de um aplicativo MVC. Você aprenderá como habilitar a autenticação do Windows dentro do arquivo de configuração do seu aplicativo web e como configurar a autenticação com o IIS. Por fim, você aprenderá como usar o atributo [autorizar] para restringir o acesso a ações do controlador para determinados usuários do Windows ou grupos.


O objetivo deste tutorial é explicar como você pode tirar proveito da segurança recursos integrados do serviços de informações da Internet para senha protegem os modos de exibição em seus aplicativos MVC. Você aprenderá a permitir que as ações do controlador ser invocada apenas por usuários específicos do Windows ou os usuários que são membros de grupos específicos do Windows.

Usando a autenticação do Windows faz sentido quando você estiver criando um site de empresa interna (um site da intranet) e você deseja que os usuários sejam capazes de usar seus nomes de usuário padrão do Windows e senhas ao acessar o site. Se você estiver criando um para as extremidades voltados para o site (um site da Internet), considere usar autenticação de formulários.

#### <a name="enabling-windows-authentication"></a>Habilitando a autenticação do Windows

Quando você cria um novo aplicativo ASP.NET MVC, a autenticação do Windows não está habilitada por padrão. Autenticação de formulários é o tipo de autenticação padrão habilitado para aplicativos MVC. Você deve habilitar a autenticação do Windows, modificando o arquivo de configuração (Web. config) do seu aplicativo MVC da web. Localizar o &lt;autenticação&gt; seção e modifique-o para usar o Windows em vez da autenticação de formulários como este:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Quando você habilita a autenticação do Windows, seu servidor web se torna responsável por autenticar usuários. Normalmente, há dois tipos diferentes de servidores web que você usa ao criar e implantar um aplicativo ASP.NET MVC.

Primeiro, ao desenvolver um aplicativo MVC, você use Web do ASP.NET Development Server incluídos no Visual Studio. Por padrão, a Web do ASP.NET Development Server executa todas as páginas no contexto da conta do Windows atual (qualquer conta usada para fazer logon no Windows).

O servidor de Web de desenvolvimento do ASP.NET também dá suporte a autenticação NTLM. Você pode habilitar a autenticação NTLM clicando duas vezes o nome do seu projeto na janela do Gerenciador de soluções e selecionando Propriedades. Em seguida, selecione a guia Web e marque a caixa de seleção NTLM (veja a Figura 1).

**Figura 1 – habilitando a autenticação NTLM para o servidor de Web de desenvolvimento do ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Para um aplicativo web de produção, em mãos, você usa o IIS como seu servidor web. O IIS dá suporte a vários tipos de autenticação, incluindo:

- Autenticação básica – definido como parte do protocolo HTTP 1.0. Envia os nomes de usuário e senhas em texto não criptografado (codificado em Base64) pela Internet. -Autenticação Digest – envia um hash de uma senha, em vez da senha em si, com a internet. -Autenticação integrada do Windows (NTLM) – o melhor tipo de autenticação a ser usado em ambientes de intranet usando o windows. -Autenticação – permite a autenticação usando um certificado de cliente de certificado. O certificado é mapeado para uma conta de usuário do Windows.

> [!NOTE] 
> 
> Para obter uma visão mais detalhada desses tipos diferentes de autenticação, consulte [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Você pode usar o Gerenciador de serviços de informações da Internet para habilitar um determinado tipo de autenticação. Lembre-se de que todos os tipos de autenticação não estão disponíveis no caso de todos os sistemas operacionais. Além disso, se você estiver usando o IIS 7.0 com o Windows Vista, você precisará habilitar os diferentes tipos de autenticação do Windows antes de aparecerem no Gerenciador de serviços de informações da Internet. Abra **painel de controle, programas, programas e recursos, ou desativar recursos do Windows ativar**e expanda o nó Serviços de informações da Internet (veja a Figura 2).

**Figura 2 – recursos do IIS do Windows habilitando**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Usando os serviços de informações da Internet, você pode habilitar ou desabilitar diferentes tipos de autenticação. Por exemplo, a Figura 3 ilustra desabilitando a autenticação anônima e habilitar a autenticação integrada do Windows (NTLM) ao usar o IIS 7.0.

**Figura 3 – habilitar a autenticação integrada do Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Windows de autorizar usuários e grupos

Depois de habilitar a autenticação do Windows, você pode usar o atributo [autorizar] para controlar o acesso a controladores ou ações do controlador. Esse atributo pode ser aplicado a um controlador MVC inteiro ou uma ação do controlador específico.

Por exemplo, o controlador Home na listagem 1 expõe três ações chamadas Index (), CompanySecrets() e StephenSecrets(). Qualquer pessoa pode invocar a ação Index (). No entanto, somente os membros do grupo Windows local gerentes podem invocar a ação de CompanySecrets(). Por fim, somente o usuário de domínio do Windows chamado Stephen (no domínio Redmond) pode invocar a ação de StephenSecrets().

**Listagem 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Devido ao Windows User Account Control (UAC), ao trabalhar com o Windows Vista ou Windows Server 2008, o grupo de administradores local se comportará diferente dos outros grupos. O atributo [autorizar] não reconhece corretamente um membro do grupo Administradores local, a menos que você modifique as configurações de UAC do seu computador.


Exatamente o que acontece quando você tentar invocar uma ação de controlador sem ser as permissões corretas depende do tipo de autenticação habilitado. Por padrão, ao usar o ASP.NET Development Server, você simplesmente obtém uma página em branco. A página é atendida com um **401 não autorizado** Status de resposta HTTP.

Se, por outro lado, você estiver usando o IIS com autenticação anônima desabilitada e a autenticação básica habilitada, você continuar a receber uma solicitação de caixa de diálogo de logon de cada vez que você solicitar a página protegida (veja a Figura 4).

**Figura 4 – caixa de diálogo de logon de autenticação básica**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Resumo

Este tutorial explicou como você pode usar a autenticação do Windows no contexto de um aplicativo ASP.NET MVC. Você aprendeu como habilitar a autenticação do Windows dentro do arquivo de configuração do seu aplicativo web e como configurar a autenticação com o IIS. Por fim, você aprendeu a usar o atributo [autorizar] para restringir o acesso às ações do controlador para determinados usuários do Windows ou grupos.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-forms-authentication-cs.md)
> [Próximo](preventing-javascript-injection-attacks-cs.md)
