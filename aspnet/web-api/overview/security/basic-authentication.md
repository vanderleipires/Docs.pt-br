---
uid: web-api/overview/security/basic-authentication
title: Autenticação básica na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Descreve como usar a autenticação básica na API Web ASP.NET.
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829581"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Autenticação básica na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticação básica é definida no [RFC 2617, autenticação HTTP: autenticação básica e Digest Access](http://www.ietf.org/rfc/rfc2617.txt).

Desvantagens

- As credenciais do usuário são enviadas na solicitação.
- As credenciais são enviadas como texto sem formatação.
- As credenciais são enviadas com cada solicitação.
- Nenhuma maneira de fazer logoff, exceto encerrando a sessão do navegador.
- Vulnerável a cruzada solicitação intersite forjada (CSRF); requer que medidas anti-CSRF.

Vantagens

- Padrão da Internet.
- Suporte para todos os principais navegadores.
- Protocolo relativamente simple.

Autenticação básica funciona da seguinte maneira:

1. Se uma solicitação requer autenticação, o servidor retorna 401 (não autorizado). A resposta inclui um cabeçalho WWW-Authenticate, que indica que o servidor dá suporte à autenticação básica.
2. O cliente envia outra solicitação, com as credenciais do cliente no cabeçalho de autorização. As credenciais são formatadas como a cadeia de caracteres "nome: senha", codificado na base64. As credenciais não são criptografadas.

Autenticação básica é executada dentro do contexto de um "Território". O servidor inclui o nome do território no cabeçalho WWW-Authenticate. As credenciais do usuário são válidas dentro desse realm. O escopo exato de um território é definido pelo servidor. Por exemplo, você pode definir vários territórios em ordem para a partição de recursos.

![](basic-authentication/_static/image1.png)

Como as credenciais são enviadas sem criptografia, autenticação básica é segura via HTTPS. Ver [trabalhando com SSL na API Web](working-with-ssl-in-web-api.md).

A autenticação básica também é vulnerável a ataques de CSRF. Depois que o usuário insere as credenciais, o navegador automaticamente envia-os em solicitações subsequentes ao mesmo domínio, para a duração da sessão. Isso inclui solicitações AJAX. Ver [impedindo solicitação intersite forjada (CSRF) ataques](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticação básica com o IIS

O IIS dá suporte à autenticação básica, mas há uma limitação: O usuário é autenticado em relação às suas credenciais do Windows. Isso significa que o usuário deve ter uma conta no domínio do servidor. Para um site voltado ao público, você geralmente deseja autenticar em relação a um provedor de associação do ASP.NET.

Para habilitar a autenticação básica usando o IIS, defina o modo de autenticação como "Windows" em Web. config do seu projeto do ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

Nesse modo, o IIS usa as credenciais do Windows para autenticar. Além disso, você deve habilitar a autenticação básica no IIS. No Gerenciador do IIS, vá para a exibição de recursos, selecione a autenticação e habilitar a autenticação básica.

![](basic-authentication/_static/image2.png)

Em seu projeto de API da Web, adicione o `[Authorize]` atributo para quaisquer ações do controlador que precisam de autenticação.

Um cliente se autentica, definindo o cabeçalho de autorização na solicitação. Clientes de navegador executam automaticamente essa etapa. Clientes nonbrowser serão necessário definir o cabeçalho.

## <a name="basic-authentication-with-custom-membership"></a>Autenticação básica com associação personalizado

Conforme mencionado, a autenticação básica integradas no IIS usa as credenciais do Windows. Isso significa que você precisa criar contas para os usuários no servidor de hospedagem. Mas, para um aplicativo de internet, as contas de usuário normalmente são armazenadas em um banco de dados externo.

O código a seguir como um módulo HTTP que realiza a autenticação básica. Você pode conectar facilmente em um provedor de associação do ASP.NET, substituindo o `CheckPassword` método, que é um método fictício neste exemplo.

Na API Web 2, você deve considerar escrever uma [filtro de autenticação](authentication-filters.md) ou [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), em vez de um módulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Para habilitar o módulo HTTP, adicione o seguinte ao arquivo Web. config na **System. webServer** seção:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Substitua "YourAssemblyName" com o nome do assembly (não incluindo a extensão "dll").

Você deve desabilitar a outras esquemas de autenticação, como autenticação de formulários ou Windows.
