---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Adicionando uma exibição de administrador | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 8d9d14e5228606a81482eb08086a114ad5145e44
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828806"
---
<a name="part-4-adding-an-admin-view"></a>Parte 4: Adicionando uma exibição de administrador
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Adicionar uma exibição de administrador

Agora vamos ativar ao lado do cliente e adicione uma página que pode consumir dados do controlador de administrador. A página permitirá que os usuários criar, editar ou excluir os produtos, enviando solicitações AJAX ao controlador.

No Gerenciador de soluções, expanda a pasta controladores e abra o arquivo chamado HomeController.cs. Esse arquivo contém um controlador MVC. Adicione um método chamado `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

O **HttpRouteUrl** método cria o URI para a API da web, e armazenamos tudo isso no recipiente de modo de exibição para uso posterior.

Em seguida, posicione o cursor de texto dentro de `Admin` método de ação, em seguida, clique com botão direito e selecione **adicionar exibição**. Isso abrirá o **adicionar exibição** caixa de diálogo.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

No **adicionar exibição** caixa de diálogo, nome de exibição de "Admin". Selecione a caixa de seleção rotulada **criar uma exibição fortemente tipada**. Sob **classe de modelo**, selecione "Product (ProductStore.Models)". Deixe todas as outras opções como seus valores padrão.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Clicando em **Add** adiciona um arquivo chamado Admin.cshtml em exibições/inicial. Abra esse arquivo e adicione o seguinte HTML. Este HTML define a estrutura da página, mas nenhuma funcionalidade está conectada ainda.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Criar um Link para a página de administração

No Gerenciador de soluções, expanda a pasta de modos de exibição e, em seguida, expanda a pasta compartilhada. Abra o arquivo chamado \_layout. cshtml. Localize o **ul** elemento com id = "menu" e um link de ação para o modo de exibição do administrador:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> No projeto de exemplo, fiz algumas outras alterações superficiais, como substituir a cadeia de caracteres "Seu logotipo aqui". Elas não afetam a funcionalidade do aplicativo. Você pode baixar o projeto e compare os arquivos.


Execute o aplicativo e clique no link "Admin" que aparece na parte superior da home page. A página de administração deve ser semelhante ao seguinte:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Direita agora, a página não faz nada. Na próxima seção, vamos usar Knockout. js para criar uma interface do usuário dinâmica.

## <a name="add-authorization"></a>Adicionar autorização

A página de administração é acessível para qualquer pessoa que visitar o site no momento. Vamos alterar isso para restringir a permissão para administradores.

Comece adicionando uma função de "Administrador" e um usuário administrador. No Gerenciador de soluções, expanda a pasta de filtros e abra o arquivo chamado InitializeSimpleMembershipAttribute.cs. Localize o `SimpleMembershipInitializer` construtor. Após a chamada para **WebSecurity.InitializeDatabaseConnection**, adicione o seguinte código:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Essa é uma maneira rápida e simples para adicionar a função "Administrador" e criar um usuário para a função.

No Gerenciador de soluções, expanda a pasta controladores e abra o arquivo HomeController.cs. Adicione a **Authorize** atributo para o `Admin` método.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Abra o arquivo AdminController.cs e adicione a **Authorize** do atributo a todo o `AdminController` classe.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC e Web API ambos definem **autorizar** atributos em namespaces diferentes. O MVC usa **System.Web.Mvc.AuthorizeAttribute**, enquanto a API Web usa **authorizeattribute**.


Agora somente os administradores podem exibir a página de administração. Além disso, se você enviar uma solicitação HTTP para o controlador de administrador, a solicitação deve conter um cookie de autenticação. Caso contrário, o servidor envia uma resposta HTTP 401 (não autorizado). Você pode ver isso no Fiddler, enviando uma solicitação GET para `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-3.md)
> [Próximo](using-web-api-with-entity-framework-part-5.md)
