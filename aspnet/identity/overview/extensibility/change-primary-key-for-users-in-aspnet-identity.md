---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "Alterar a chave primária para usuários no ASP.NET Identity | Microsoft Docs"
author: tfitzmac
description: "No Visual Studio 2013, o aplicativo web padrão usa um valor de cadeia de caracteres para a chave para as contas de usuário. Identidade do ASP.NET permite que você altere o tipo da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Alterar a chave primária para usuários no ASP.NET Identity
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> No Visual Studio 2013, o aplicativo web padrão usa um valor de cadeia de caracteres para a chave para as contas de usuário. Identidade do ASP.NET permite que você altere o tipo de chave para atender aos requisitos de dados. Por exemplo, você pode alterar o tipo da chave de uma cadeia de caracteres para um número inteiro.
> 
> Este tópico mostra como começar com o padrão aplicativo web e altere a chave de conta de usuário para um número inteiro. Você pode usar as mesmas modificações para implementar qualquer tipo de chave em seu projeto. Ele mostra como fazer essas alterações no aplicativo da web padrão, mas você pode aplicar modificações semelhantes a um aplicativo personalizado. Ele mostra as alterações necessárias ao trabalhar com MVC ou Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2013 Update 2 (ou posterior)
> - Identidade do ASP.NET 2.1 ou posterior


Para executar as etapas neste tutorial, você deve ter o Visual Studio 2013 atualização 2 (ou posterior) e um aplicativo da web criados usando o modelo de aplicativo Web ASP.NET. O modelo foi alterado na atualização 3. Este tópico mostra como alterar o modelo na atualização 2 e 3.

Esse tópico contém as seguintes seções:

- [Alterar o tipo de chave da classe de usuário de identidade](#userclass)
- [Adicionar classes de identidade personalizadas que usam o tipo de chave](#customclass)
- [Alterar o Gerenciador de usuário e de classe de contexto para usar o tipo de chave](#context)
- [Alterar configuração de inicialização para usar o tipo de chave](#startup)
- [Para MVC com atualização 2, altere o AccountController para passar o tipo de chave](#mvcupdate2)
- [Para MVC com atualização 3, altere o AccountController e ManageController para passar o tipo de chave](#mvcupdate3)
- [Para formulários da Web com atualização 2, alterar as páginas de conta para passar o tipo de chave](#webformsupdate2)
- [Para formulários da Web com atualização 3, alterar as páginas de conta para passar o tipo de chave](#webformsupdate3)
- [Executar o aplicativo](#run)
- [Outros recursos](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Alterar o tipo de chave da classe de usuário de identidade

No seu projeto criado usando o modelo de aplicativo Web ASP.NET, especifique que a classe ApplicationUser usa um número inteiro para a chave para as contas de usuário. Em IdentityModels.cs, altere a classe ApplicationUser para herdar de IdentityUser que tem um tipo de **int** para o parâmetro genérico TKey. Você também pode passar os nomes dos três classe personalizada que ainda não tiver implementado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Você alterou o tipo da chave, mas, por padrão, o restante do aplicativo ainda supõe que a chave é uma cadeia de caracteres. Você deve indicar explicitamente o tipo de chave no código que assume uma cadeia de caracteres.

No **ApplicationUser** classe, altere o **GenerateUserIdentityAsync** método incluir int, conforme mostrado no código realçado abaixo. Essa alteração não é necessária para projetos Web Forms com o modelo de atualização 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Adicionar classes de identidade personalizadas que usam o tipo de chave

As outras classes de identidade, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, ainda são configuradas para usar uma chave de cadeia de caracteres. Crie novas versões dessas classes que especificam um número inteiro para a chave. Você não precisa fornecer muito código de implementação nessas classes, principalmente apenas você está definindo int como a chave.

Adicione as seguintes classes no arquivo IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Alterar o Gerenciador de usuário e de classe de contexto para usar o tipo de chave

Na IdentityModels.cs, altere a definição do **ApplicationDbContext** classe para usar a nova personalizado classes e um **int** para a chave, conforme mostrado no código realçado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

O parâmetro ThrowIfV1Schema não é válido no construtor. Mudar o construtor, portanto, não passe um valor de ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Abra IdentityConfig.cs e altere o **ApplicationUserManger** classe para usar o novo usuário armazenar a classe para manter dados e um **int** para a chave.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

No modelo de atualização 3, você deve alterar a classe ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Alterar configuração de inicialização para usar o tipo de chave

No Startup.Auth.cs, substitua o código de OnValidateIdentity, como destacado abaixo. Observe que a definição de getUserIdCallback, analisa o valor de cadeia de caracteres em um número inteiro.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Se seu projeto não reconhece a implementação do **GetUserId** método, talvez seja necessário atualizar o pacote do NuGet do ASP.NET Identity à versão 2.1

Você fez muitas alterações para as classes de infraestrutura usadas pela identidade do ASP.NET. Se você tentar compilar o projeto, você observará vários erros. Felizmente, os erros restantes são semelhantes. A classe de identidade espera um número inteiro para a chave, mas o controlador (ou um formulário da Web) está passando um valor de cadeia de caracteres. Em cada caso, você precisa converter de uma cadeia de caracteres para e inteiro chamando **GetUserId&lt;int&gt;**. Você pode trabalhar com a lista de erros de compilação ou siga as alterações a seguir.

As alterações restantes dependem do tipo de projeto que você está criando e qual a atualização que você instalou no Visual Studio. Você pode ir diretamente para a seção através dos links a seguir

- [Para MVC com atualização 2, altere o AccountController para passar o tipo de chave](#mvcupdate2)
- [Para MVC com atualização 3, altere o AccountController e ManageController para passar o tipo de chave](#mvcupdate3)
- [Para formulários da Web com atualização 2, alterar as páginas de conta para passar o tipo de chave](#webformsupdate2)
- [Para formulários da Web com atualização 3, alterar as páginas de conta para passar o tipo de chave](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Para MVC com atualização 2, altere o AccountController para passar o tipo de chave

Abra o arquivo AccountController.cs. Você precisa alterar os métodos a seguir.

**ConfirmEmail** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Desassociar** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Para MVC com atualização 3, altere o AccountController e ManageController para passar o tipo de chave

Abra o arquivo AccountController.cs. Você precisa alterar o método a seguir.

**ConfirmEmail** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Abra o arquivo ManageController.cs. Você precisa alterar os métodos a seguir.

**Índice** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** métodos

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** métodos

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Para formulários da Web com atualização 2, alterar as páginas de conta para passar o tipo de chave

Para formulários da Web com atualização 2, você precisa alterar as páginas a seguir.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Para formulários da Web com atualização 3, alterar as páginas de conta para passar o tipo de chave

Para formulários da Web com atualização 3, você precisa alterar as páginas a seguir.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Executar o aplicativo

Você concluiu todas as alterações necessárias para o modelo de aplicativo da Web padrão. Execute o aplicativo e registrar um novo usuário. Depois de registrar o usuário, você observará que a tabela de AspNetUsers possui uma coluna de Id que é um número inteiro.

![nova chave primária](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Se você tiver criado anteriormente a identidade do ASP.NET tabelas com uma chave primária diferente, você precisa fazer algumas alterações adicionais. Se possível, exclua o banco de dados existente. O banco de dados será recriado com o design correto ao executar o aplicativo web e adicionar um novo usuário. Se a exclusão não é possível executar migrações do code first para alterar as tabelas. No entanto, a nova chave primária de inteiro será não ser configurada como uma propriedade de identidade de SQL no banco de dados. Você deve definir manualmente a coluna Id como uma identidade.

<a id="other"></a>
## <a name="other-resources"></a>Outros recursos

- [Visão geral dos provedores de armazenamento personalizado para a identidade do ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrando um site existente da associação SQL para a identidade do ASP.NET](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migração de dados Universal do provedor de associação e os perfis de usuário para a identidade do ASP.NET](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Aplicativo de exemplo](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) com a chave primária alterada
