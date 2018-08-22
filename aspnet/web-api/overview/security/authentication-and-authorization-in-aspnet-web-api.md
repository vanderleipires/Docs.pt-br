---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticação e autorização na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Fornece uma visão geral de autenticação e autorização na API Web ASP.NET.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a78606a74b2149e68e3b01f4fe204f4a13edf4b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834062"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticação e autorização na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Você criou uma API da web, mas agora que você deseja controlar o acesso a ele. Nesta série de artigos, vamos examinar algumas opções para proteger uma API da web contra usuários não autorizados. Esta série abordarão a autenticação e autorização.

- *Autenticação* é saber a identidade do usuário. Por exemplo, Alice faz logon com seu nome de usuário e senha, e o servidor usa a senha para autenticar Alice.
- *Autorização* é decidir se um usuário tem permissão para executar uma ação. Por exemplo, Alice tem permissão para obter um recurso, mas não criar um recurso.

O primeiro artigo da série fornece uma visão geral de autenticação e autorização na API Web ASP.NET. Outros tópicos descrevem os cenários de autenticação comuns para a API da Web.

> [!NOTE]
> Graças a pessoas que revisado desta série e forneceram seus valiosos comentários: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Ge Hongmei, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Autenticação

API da Web supõe que a autenticação ocorre no host. Para hospedagem na web, o host é o IIS, que usa os módulos HTTP para autenticação. Você pode configurar seu projeto para usar qualquer um dos módulos de autenticação integrados do IIS ou do ASP.NET, ou escrever seu próprio módulo HTTP para realizar a autenticação personalizada.

Quando o host as autentica o usuário, ele cria um *principal*, que é um [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) objeto que representa o contexto de segurança sob a qual o código está em execução. O host anexa a entidade de segurança ao thread atual, definindo **thread. CurrentPrincipal**. A entidade contém um associado **identidade** objeto que contém informações sobre o usuário. Se o usuário for autenticado, o **Identity.IsAuthenticated** propriedade retorna **verdadeiro**. Para solicitações anônimas, **IsAuthenticated** retorna **falso**. Para obter mais informações sobre entidades de segurança, consulte [segurança baseada em função](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Manipuladores de mensagens de HTTP para autenticação

Em vez de usar o host para autenticação, você pode colocar a lógica de autenticação em um [manipulador de mensagens HTTP](../advanced/http-message-handlers.md). Nesse caso, o manipulador de mensagens examina a solicitação HTTP e define a entidade de segurança.

Quando você usar manipuladores de mensagens para autenticação? Aqui estão algumas vantagens e desvantagens:

- Um módulo HTTP vê todas as solicitações que passam pelo pipeline do ASP.NET. Um manipulador de mensagens vê somente as solicitações são roteadas para a API da Web.
- Você pode definir manipuladores de mensagens por rota, que permite aplicar um esquema de autenticação para uma rota específica.
- Módulos HTTP são específicos ao IIS. Manipuladores de mensagens são independentes de host, para que possam ser usados com hospedagem na web e hospedagem interna.
- Módulos HTTP participarem de log do IIS, auditoria e assim por diante.
- Os módulos HTTP executam antes no pipeline. Se você lidar com a autenticação em um manipulador de mensagens, a entidade de segurança não obter definida até que o manipulador é executado. Além disso, a entidade de segurança reverterá para a entidade de segurança anterior quando a resposta deixa o manipulador de mensagens.

Em geral, se você não precisar dar suporte à hospedagem interna, um módulo HTTP é uma opção melhor. Se você precisar dar suporte a hospedagem interna, considere um manipulador de mensagens.

### <a name="setting-the-principal"></a>Definir a entidade de segurança

Se seu aplicativo executa qualquer lógica de autenticação personalizada, você deve definir a entidade de segurança em dois locais:

- **Thread.CurrentPrincipal**. Esta propriedade é a maneira padrão para definir a entidade de segurança do thread no .NET.
- **HttpContext.Current.User**. Essa propriedade é específica para o ASP.NET.

O código a seguir mostra como definir a entidade de segurança:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Para hospedagem na web, você deve definir a entidade de segurança em ambos os locais; Caso contrário, o contexto de segurança pode se tornar inconsistente. Para auto-hospedagem, no entanto, **HttpContext. Current** é nulo. Para garantir que seu código é independente de host, portanto, verifique se há nulos antes de atribuí **HttpContext. Current**, conforme mostrado.

## <a name="authorization"></a>Autorização

Autorização ocorre posteriormente no pipeline, mais próximo ao controlador. Que lhe permite fazer escolhas mais granulares, quando você conceder acesso aos recursos.

- *Filtros de autorização* executar antes da ação do controlador. Se a solicitação não está autorizada, o filtro retorna uma resposta de erro e a ação não é invocada.
- Dentro de uma ação de controlador, você pode obter a entidade de segurança atual do **ApiController.User** propriedade. Por exemplo, você pode filtrar uma lista de recursos com base no nome de usuário, retornando apenas os recursos que pertencem a esse usuário.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Usando o [autorizar] atributo

API da Web fornece um filtro de autorização internos [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Esse filtro verifica se o usuário é autenticado. Caso contrário, ele retorna o código de status HTTP 401 (não autorizado), sem invocar a ação.

Você pode aplicar o filtro globalmente, no nível do controlador ou no nível de ações individuais.

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
> O **AuthorizeAttribute** filtro para controladores da API Web está localizado na **System** namespace. Há um filtro semelhante para controladores MVC na **System.Web.Mvc** namespace, que não é compatível com os controladores de API da Web.


### <a name="custom-authorization-filters"></a>Filtros de autorização personalizada

Para gravar um filtro de autorização personalizada, derivam de um destes tipos:

- **AuthorizeAttribute**. Estenda a classe para executar a lógica de autorização com base no usuário atual e as funções do usuário.
- **AuthorizationFilterAttribute**. Estenda a classe para executar a lógica de autorização síncrono que não necessariamente se baseia o usuário atual ou a função.
- **IAuthorizationFilter**. Implementar essa interface para executar a lógica de autorização assíncrona; Por exemplo, se sua lógica de autorização faz chamadas assíncronas de e/s ou de rede. (Se sua lógica de autorização é vinculado à CPU, é mais simples de derivam **AuthorizationFilterAttribute**, porque você precisa escrever um método assíncrono.)

O diagrama a seguir mostra a hierarquia de classe para o **AuthorizeAttribute** classe.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorização dentro de uma ação do controlador

Em alguns casos, você pode permitir que uma solicitação para continuar, mas alterar o comportamento com base no principal. Por exemplo, as informações que você retornar podem mudar dependendo da função do usuário. Dentro de um método de controlador, você pode obter a entidade de segurança atual do **ApiController.User** propriedade.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
