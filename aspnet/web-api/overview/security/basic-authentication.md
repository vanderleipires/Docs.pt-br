---
uid: web-api/overview/security/basic-authentication
title: Autenticação básica no ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Descreve como usar a autenticação básica no API Web do ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508125"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Autenticação básica na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticação básica é definida em [RFC 2617, autenticação HTTP: Basic e a autenticação Digest acesso](http://www.ietf.org/rfc/rfc2617.txt).

Desvantagens

- As credenciais do usuário são enviadas na solicitação.
- As credenciais são enviadas como texto sem formatação.
- As credenciais são enviadas com cada solicitação.
- Nenhuma maneira de fazer logoff, exceto por encerrar a sessão do navegador.
- Vulnerável a sites solicitação CSRF (falsificação); requer medidas anti-CSRF.

Vantagens

- Padrão da Internet.
- Suporte para todos os principais navegadores.
- Protocolo relativamente simple.

Autenticação básica funciona da seguinte maneira:

1. Se uma solicitação requer autenticação, o servidor retorna 401 (não autorizado). A resposta inclui um cabeçalho WWW-Authenticate, indicando que o servidor oferece suporte à autenticação básica.
2. O cliente envia outra solicitação, com as credenciais do cliente no cabeçalho de autorização. As credenciais são formatadas como a cadeia de caracteres "nome: senha", codificada em base64. As credenciais não são criptografadas.

Autenticação básica é executada dentro do contexto de um "Território". O servidor inclui o nome do território no cabeçalho WWW-Authenticate. As credenciais do usuário são válidas dentro desse realm. O escopo exato de um território é definido pelo servidor. Por exemplo, você pode definir vários territórios em ordem para a partição de recursos.

![](basic-authentication/_static/image1.png)

Como as credenciais são enviadas sem criptografia, autenticação básica é segura via HTTPS. Consulte [trabalhar com SSL na API da Web](working-with-ssl-in-web-api.md).

Autenticação básica também é vulnerável a ataques CSRF. Depois que o usuário insere as credenciais, o navegador envia automaticamente-los em solicitações subsequentes ao mesmo domínio, para a duração da sessão. Isso inclui solicitações do AJAX. Consulte [impedindo ataques CSRF (falsificação) de solicitação entre sites](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticação básica com o IIS

O IIS dá suporte à autenticação básica, mas há uma limitação: O usuário é autenticado com as credenciais do Windows. Isso significa que o usuário deve ter uma conta no domínio do servidor. Para um site voltado ao público, você geralmente deseja autenticar em um provedor de associação do ASP.NET.

Para habilitar a autenticação básica usando o IIS, defina o modo de autenticação para "Windows" no Web. config do projeto ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

Nesse modo, o IIS usa as credenciais do Windows para autenticar. Além disso, você deve habilitar a autenticação básica no IIS. No Gerenciador do IIS, vá para a exibição de recursos, selecione a autenticação e habilitar a autenticação básica.

![](basic-authentication/_static/image2.png)

No seu projeto de API da Web, adicione o `[Authorize]` atributo para as ações do controlador que precisam de autenticação.

Um cliente se autentica definindo o cabeçalho de autorização na solicitação. Clientes de navegador executam esta etapa automaticamente. Clientes de nonbrowser serão necessário definir o cabeçalho.

## <a name="basic-authentication-with-custom-membership"></a>Autenticação básica com associação personalizado

Conforme mencionado, a autenticação básica integradas no IIS usa as credenciais do Windows. Isso significa que você precisa criar contas para os usuários no servidor de hospedagem. Mas, para um aplicativo de internet, contas de usuário são normalmente armazenadas no banco de dados.

O código a seguir como um módulo HTTP que realiza a autenticação básica. Você pode conectar facilmente em um provedor de associação do ASP.NET, substituindo o `CheckPassword` método, que é um método fictício neste exemplo.

API Web 2, você deve considerar gravando um [filtro autenticação](authentication-filters.md) ou [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), em vez de um módulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Para habilitar o módulo HTTP, adicione o seguinte ao arquivo Web. config no **System. webServer** seção:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Substitua "YourAssemblyName" com o nome do assembly (não incluindo a extensão "dll").

Você deve desabilitar outros esquemas de autenticação, como autenticação de formulários ou Windows.
