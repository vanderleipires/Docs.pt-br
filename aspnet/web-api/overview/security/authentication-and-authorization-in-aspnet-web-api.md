---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "Autenticação e autorização na API da Web ASP.NET | Microsoft Docs"
author: MikeWasson
description: "Fornece uma visão geral de autenticação e autorização na API da Web do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2a4b5ed8a712b061b4afdf5a3adc9378dd72b37f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticação e autorização na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Você criou uma API da web, mas agora deseja controlar o acesso a ele. Esta série de artigos, vamos examinar algumas opções para proteger uma API da web contra usuários não autorizados. Esta série aborda a autenticação e autorização.

- *Autenticação* é saber a identidade do usuário. Por exemplo, Alice se conectar com seu nome de usuário e senha, e o servidor usa a senha para autenticar Alice.
- *Autorização* é decidir se um usuário tem permissão para executar uma ação. Por exemplo, Alice tem permissão para obter um recurso, mas não criar um recurso.

O primeiro artigo na série fornece uma visão geral de autenticação e autorização na API da Web do ASP.NET. Outros tópicos descrevem cenários comuns de autenticação de API da Web.

> [!NOTE]
> Graças a pessoas que esta série de revisão e fornecidos comentários valiosos: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Ge Hongmei, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Autenticação

API da Web pressupõe que a autenticação ocorre no host. Para hospedagem na web, o host é o IIS, que usa módulos HTTP para autenticação. Você pode configurar seu projeto para usar qualquer um dos módulos de autenticação integrados do IIS ou o ASP.NET, ou escrever seu próprio módulo HTTP para realizar a autenticação personalizada.

Quando o host autentica o usuário, ele cria um *principal*, que é um [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) objeto que representa o contexto de segurança sob a qual o código está sendo executado. O host anexa o principal para o thread atual definindo **CurrentPrincipal**. A entidade contém um tipo de **identidade** objeto que contém informações sobre o usuário. Se o usuário é autenticado, o **Identity.IsAuthenticated** propriedade retorna **true**. Para solicitações anônimas, **IsAuthenticated** retorna **false**. Para obter mais informações sobre entidades de segurança, consulte [segurança baseada em função](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Manipuladores de mensagens de HTTP para autenticação

Em vez de usar o host para autenticação, você pode colocar a lógica de autenticação em um [manipulador de mensagens HTTP](../advanced/http-message-handlers.md). Nesse caso, o manipulador de mensagens examina a solicitação HTTP e define a entidade de segurança.

Quando você usar manipuladores de mensagens para autenticação? Aqui estão algumas vantagens e desvantagens:

- Um módulo HTTP vê todas as solicitações que passam pelo pipeline do ASP.NET. Um manipulador de mensagens vê somente as solicitações são roteadas para a API da Web.
- Você pode definir manipuladores de mensagens por rota, o que permite aplicar um esquema de autenticação para uma rota específica.
- Módulos HTTP são específicos ao IIS. Manipuladores de mensagens são independentes de host, para que possam ser usados com hospedagem na web e hospedagem interna.
- Módulos HTTP de participarem de log do IIS, auditoria e assim por diante.
- Módulos HTTP executados anteriormente no pipeline. Se você lidar com autenticação em um manipulador de mensagens, a entidade de segurança não obter definida até que o manipulador é executado. Além disso, a entidade de segurança voltarão a principal anterior, quando a resposta deixa o manipulador de mensagens.

Em geral, se você não precisa dar suporte a auto-hospedagem, um módulo HTTP é uma opção melhor. Se você precisar dar suporte a auto-hospedagem, considere um manipulador de mensagens.

### <a name="setting-the-principal"></a>Definir a entidade de segurança

Se o aplicativo executa qualquer lógica de autenticação personalizada, você deve definir a entidade de segurança em dois locais:

- **Thread.CurrentPrincipal**. Esta propriedade é o modo padrão para definir a entidade de segurança do thread no .NET.
- **HttpContext.Current.User**. Essa propriedade é específica para o ASP.NET.

O código a seguir mostra como definir a entidade de segurança:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Para hospedagem na web, você deve definir a entidade de segurança nos dois locais; Caso contrário, o contexto de segurança pode se tornar inconsistente. Para auto-hospedagem, no entanto, **HttpContext** é nulo. Para garantir que seu código é independente de host, portanto, verifique se há nulo antes de atribuir a **HttpContext**, conforme mostrado.

## <a name="authorization"></a>Autorização

Autorização ocorre posteriormente no pipeline, mais próximo do controlador. Que lhe permite fazer escolhas mais granulares, quando você concede acesso aos recursos.

- *Filtros de autorização* executados antes da ação de controlador. Se a solicitação não está autorizada, o filtro retorna uma resposta de erro e a ação não é invocada.
- Dentro de uma ação do controlador, você pode obter o objeto atual do **ApiController.User** propriedade. Por exemplo, você pode filtrar uma lista de recursos com base no nome de usuário, retornando somente os recursos que pertencem a esse usuário.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Usando a [autorizar] atributo

API da Web fornece um filtro de autorização interno, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Esse filtro verifica se o usuário é autenticado. Caso contrário, ele retorna o código de status HTTP 401 (não autorizado), sem chamar a ação.

Você pode aplicar o filtro globalmente, no nível do controlador ou o nível de inidivual ações.

**Globalmente**: para restringir o acesso para cada controlador de API da Web, adicione o **AuthorizeAttribute** filtro à lista de filtros globais:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controlador**: para restringir o acesso a um controlador específico, adicione o filtro como um atributo para o controlador:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Ação**: para restringir o acesso para ações específicas, adicione o atributo ao método de ação:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Como alternativa, você pode restringir o controlador e, em seguida, permitir acesso anônimo a ações específicas usando o `[AllowAnonymous]` atributo. No exemplo a seguir, o `Post` método é restrito, mas o `Get` método permite o acesso anônimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Nos exemplos anteriores, o filtro permite que qualquer usuário autenticado acessar os métodos restritos; somente os usuários anônimos são mantidos. Você também pode limitar o acesso a usuários específicos ou a usuários em funções específicas:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> O **AuthorizeAttribute** filtro para controladores de API da Web está localizado no **System.Web.Http** namespace. Há um filtro semelhante para controladores MVC no **System.Web.Mvc** namespace, que não é compatível com os controladores de API da Web.


### <a name="custom-authorization-filters"></a>Filtros de autorização personalizada

Para gravar um filtro de autorização personalizada, derivam de um destes tipos:

- **AuthorizeAttribute**. Estenda a classe para executar a lógica de autorização com base em usuário atual e as funções do usuário.
- **AuthorizationFilterAttribute**. Estenda a classe para executar lógica de autorização síncrono que não é necessariamente baseada em usuário atual ou função.
- **IAuthorizationFilter**. Implementar essa interface para executar a lógica de autorização assíncrona; Por exemplo, se sua lógica de autorização faz chamadas assíncronas de e/s ou de rede. (Se sua lógica de autorização está vinculado à CPU, é mais simples de derivar de **AuthorizationFilterAttribute**, porque você precisa escrever um método assíncrono.)

O diagrama a seguir mostra a hierarquia de classe para o **AuthorizeAttribute** classe.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorização dentro de uma ação do controlador

Em alguns casos, você pode permitir que uma solicitação para continuar, mas alterar o comportamento de acordo com a entidade de segurança. Por exemplo, as informações que você retornar podem mudar dependendo da função do usuário. Dentro de um método de controlador, você pode obter o princípio atual do **ApiController.User** propriedade.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
