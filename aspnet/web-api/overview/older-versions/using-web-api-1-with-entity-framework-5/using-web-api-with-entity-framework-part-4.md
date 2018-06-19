---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Adicionando uma exibição de administração | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879524"
---
<a name="part-4-adding-an-admin-view"></a>Parte 4: Adicionando uma exibição de administração
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Adicionar um modo de exibição de administração

Agora vamos ativar ao lado do cliente e adicione uma página que pode consumir dados do controlador de administrador. A página permitirá aos usuários criar, editar ou excluir produtos, enviando solicitações do AJAX para o controlador.

No Gerenciador de soluções, expanda a pasta controladores e abra o arquivo chamado HomeController. Esse arquivo contém um controlador MVC. Adicionar um método chamado `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

O **HttpRouteUrl** método cria o URI para a API da web e armazenamos isso o multiconjunto da exibição para mais tarde.

Em seguida, posicione o cursor do texto dentro de `Admin` método de ação, em seguida, clique com botão direito e selecione **adicionar exibição**. Isso abrirá o **adicionar exibição** caixa de diálogo.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

No **adicionar exibição** caixa de diálogo, o nome de exibição "Admin". Selecione a caixa de seleção **criar uma exibição fortemente tipada**. Em **modelo de classe**, selecione "Product (ProductStore.Models)". Deixe todas as outras opções como seus valores padrão.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Clicando em **Add** adiciona um arquivo chamado Admin.cshtml em exibições/inicial. Abra o arquivo e adicionar o HTML a seguir. Este HTML define a estrutura da página, mas nenhuma funcionalidade está conectada ainda.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Criar um Link para a página de administração

No Gerenciador de soluções, expanda a pasta de modos de exibição e, em seguida, expanda a pasta compartilhada. Abra o arquivo chamado \_cshtml. Localize o **ul** elemento com id = "menu" e um link de ação para a exibição do administrador:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> O projeto de exemplo, fiz algumas outras alterações superficiais, como substituir a cadeia de caracteres "Seu logotipo aqui". Elas não afetam a funcionalidade do aplicativo. Você pode baixar o projeto e comparar os arquivos.


Execute o aplicativo e clique no link "Admin" que aparece na parte superior da home page. A página de administrador deve ser semelhante ao seguinte:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Direita agora, a página não faz nada. Na próxima seção, usaremos Knockout. js para criar uma interface do usuário dinâmica.

## <a name="add-authorization"></a>Adicione autorização

A página de administração é acessível no momento para qualquer pessoa que visite o site. Vamos alterar isso para restringir a permissão para administradores.

Comece adicionando uma função "Administrador" e um usuário administrador. No Gerenciador de soluções, expanda a pasta de filtros e abra o arquivo chamado InitializeSimpleMembershipAttribute.cs. Localize o `SimpleMembershipInitializer` construtor. Após a chamada a **WebSecurity.InitializeDatabaseConnection**, adicione o seguinte código:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Essa é uma maneira das para adicionar a função "Administrador" e criar um usuário para a função.

No Gerenciador de soluções, expanda a pasta controladores e abra o arquivo HomeController. Adicionar o **autorizar** de atributo para o `Admin` método.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Abra o arquivo AdminController.cs e adicione o **autorizar** a todo o atributo `AdminController` classe.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC e a API da Web definir **autorizar** atributos em namespaces diferentes. O MVC usa **System.Web.Mvc.AuthorizeAttribute**, enquanto a API da Web usa **System.Web.Http.AuthorizeAttribute**.


Agora apenas administradores podem exibir a página de administração. Além disso, se você enviar uma solicitação HTTP para o controlador de administração, a solicitação deve conter um cookie de autenticação. Caso contrário, o servidor envia uma resposta HTTP 401 (não autorizado). Você pode ver isso na Fiddler enviando uma solicitação GET para `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-3.md)
> [Próximo](using-web-api-with-entity-framework-part-5.md)
