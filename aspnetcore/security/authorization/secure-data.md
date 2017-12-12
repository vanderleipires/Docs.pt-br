---
title: "Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização"
author: rick-anderson
keywords: "Núcleo do ASP.NET, MVC, autorização, funções, segurança, administrador"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 8eeb5d71575fd819239da6dd63dd31e323fb0556
ms.sourcegitcommit: 96af03c9f44f7c206e68ae3ef8596068e6b4e5fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

Este tutorial mostra como criar um aplicativo web com dados de usuário protegidos por autorização. Exibe uma lista de contatos (registrados) usuários autenticados criou. Há três grupos de segurança:

* Os usuários registrados podem exibir todos os dados de contato aprovados.
* Os usuários registrados podem editar/excluir seus próprios dados. 
* Os gerentes podem aprovar ou rejeitar dados de contato. Apenas os contatos aprovados são visíveis aos usuários.
* Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.

Na imagem a seguir, o usuário Rick (`rick@example.com`) está conectado. Usuário Rick pode apenas exibir aprovado contatos e editar/excluir seus contatos. Somente o último registro criado pelo Rick, exibe editar e excluir links

![imagem descrita acima](secure-data/_static/rick.png)

Na imagem a seguir, `manager@contoso.com` está conectado e na função de gerentes. 

![imagem descrita acima](secure-data/_static/manager1.png)

A imagem a seguir mostra os gerentes de modo de exibição de detalhes de um contato.

![imagem descrita acima](secure-data/_static/manager.png)

Somente os gerentes e os administradores têm a aprovar e rejeitar botões.

Na imagem a seguir, `admin@contoso.com` está conectado e na função do administrador. 

![imagem descrita acima](secure-data/_static/admin.png)

O administrador tem todos os privilégios. Ela pode ler/editar/excluir qualquer contato e alterar o status de contatos.

O aplicativo foi criado por [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) o seguinte `Contact` modelo:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

Um `ContactIsOwnerAuthorizationHandler` manipulador de autorização garante que um usuário só pode editar seus dados. Um `ContactManagerAuthorizationHandler` manipulador de autorização permite que os gerentes aprovar ou rejeitar contatos.  Um `ContactAdministratorsAuthorizationHandler` manipulador de autorização permite que os administradores para aprovar ou rejeitar contatos e editar/excluir contatos. 

## <a name="prerequisites"></a>Pré-requisitos

Este não é um tutorial de início. Você deve estar familiarizado com:

* [Núcleo do ASP.NET MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>O início e o aplicativo concluído

[Baixar](xref:tutorials/index#how-to-download-a-sample) o [concluída](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplicativo. [Teste](#test-the-completed-app) o aplicativo concluído para que você se familiarize com seus recursos de segurança. 

### <a name="the-starter-app"></a>O aplicativo de início

É útil comparar seu código com o exemplo concluído.

[Baixar](xref:tutorials/index#how-to-download-a-sample) o [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplicativo. 

Consulte [criar o aplicativo de início](#create-the-starter-app) se deseja criá-lo a partir do zero.

Atualize o banco de dados:

```none
   dotnet ef database update
```

Executar o aplicativo, toque o **ContactManager** vincular e verifique se você pode criar, editar e excluir um contato.

Este tutorial tem todas as etapas principais para criar o aplicativo de dados de usuário segura. Talvez seja útil para fazer referência ao projeto concluído.

## <a name="modify-the-app-to-secure-user-data"></a>Modificar o aplicativo para proteger os dados de usuário

As seções a seguir têm todas as principais etapas para criar o aplicativo de dados de usuário segura. Talvez seja útil para fazer referência ao projeto concluído.

### <a name="tie-the-contact-data-to-the-user"></a>Vincular os dados de contato para o usuário

Usar o ASP.NET [identidade](xref:security/authentication/identity) ID de usuário para garantir que os usuários pode editar seus dados, mas não de outros dados de usuários. Adicionar `OwnerID` para o `Contact` modelo:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`é a ID do usuário da `AspNetUser` tabela o [identidade](xref:security/authentication/identity) banco de dados. O `Status` campo determina se um contato é exibido por usuários gerais. 

Scaffold uma nova migração e atualizar o banco de dados:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Exigir SSL e os usuários autenticados

No `ConfigureServices` método o *Startup.cs* de arquivo, adicione o [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorização:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Para redirecionar solicitações HTTP para HTTPS, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting). Se você estiver usando o código do Visual Studio ou teste na plataforma local que não inclui um certificado de teste para SSL:

- Definir `"LocalTest:skipSSL": true` no *appSettings. JSON* arquivo.

### <a name="require-authenticated-users"></a>Exigir que os usuários autenticados

Defina a política de autenticação padrão para exigir que os usuários sejam autenticados. Você pode recusar a autenticação, o método de ação ou controlador com o `[AllowAnonymous]` atributo. Com essa abordagem, quaisquer novos controladores adicionados automaticamente exigir autenticação, que é mais segura do que contar com novos controladores para incluir o `[Authorize]` atributo. Adicione o seguinte para o `ConfigureServices` método o *Startup.cs* arquivo:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Adicionar `[AllowAnonymous]` para o controlador principal para usuários anônimos podem obter informações sobre o site antes de eles se registrar.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Configurar a conta de teste

O `SeedData` classe cria duas contas de administrador e Gerenciador. Use o [ferramenta Gerenciador de segredo](xref:security/app-secrets) para definir uma senha para essas contas. Fazer isso no diretório do projeto (o diretório que contém *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Atualização `Configure` para usar a senha de teste:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Adicione a ID de usuário de administrador e `Status = ContactStatus.Approved` para os contatos. Somente um contato é exibido, adicione a ID de usuário para todos os contatos:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Criar proprietário, Gerenciador e manipuladores de autorização do administrador

Criar um `ContactIsOwnerAuthorizationHandler` classe no *autorização* pasta. O `ContactIsOwnerAuthorizationHandler` verificará se o usuário atuando no recurso possui o recurso.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

O `ContactIsOwnerAuthorizationHandler` chamadas `context.Succeed` se o usuário autenticado atual é o proprietário do contato. Manipuladores de autorização geralmente retornar `context.Succeed` quando os requisitos são atendidos. Elas retornam `Task.FromResult(0)` quando os requisitos não forem atendidos. `Task.FromResult(0)`não é êxito ou falha, ele permite que outro manipulador de autorização executar. Se você precisar explicitamente falhar, retorne `context.Fail()`.

Permitir que proprietários de contato editar/excluir seus próprios dados, portanto, não precisamos verificar a operação passada no parâmetro de requisito.

### <a name="create-a-manager-authorization-handler"></a>Criar um manipulador de autorização do Gerenciador

Criar um `ContactManagerAuthorizationHandler` classe no *autorização* pasta. O `ContactManagerAuthorizationHandler` verificará se o usuário atuando no recurso é um gerenciador. Somente os gerentes podem aprovar ou rejeitar alterações de conteúdo (nova ou alteradas).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Criar um manipulador de autorização do administrador

Criar um `ContactAdministratorsAuthorizationHandler` classe no *autorização* pasta. O `ContactAdministratorsAuthorizationHandler` verificará se o usuário atuando no recurso é um administrador. Administrador pode fazer todas as operações.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrar os manipuladores de autorização

Serviços usando o Entity Framework Core devem ser registrados para [injeção de dependência](xref:fundamentals/dependency-injection) usando [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). O `ContactIsOwnerAuthorizationHandler` usa o ASP.NET Core [identidade](xref:security/authentication/identity), que está incluído no Entity Framework Core. Registrar os manipuladores com a coleção de serviço para que estejam disponíveis para o `ContactsController` por meio de [injeção de dependência](xref:fundamentals/dependency-injection). Adicione o seguinte código ao final da `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`e `ContactManagerAuthorizationHandler` são adicionados como singletons. São singletons porque eles não usam o EF e todas as informações necessárias no `Context` parâmetro o `HandleRequirementAsync` método.

Completo `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Atualizar o código para oferecer suporte à autorização

Nesta seção, você atualizar o controlador e os modos de exibição e adicionar uma classe de requisitos de operações.

### <a name="update-the-contacts-controller"></a>Atualize o controlador de contatos

Atualização de `ContactsController` construtor:

* Adicionar o `IAuthorizationService` serviço acesse os manipuladores de autorização. 
* Adicionar o `Identity` `UserManager` serviço:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Adicionar uma classe de requisitos de operações de contato

Adicionar o `ContactOperations` de classe para o *autorização* pasta. Essa classe contém os requisitos de nosso aplicativo suporta:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Criar atualização

Atualização de `HTTP POST Create` método:

* Adicione a ID de usuário para o `Contact` modelo.
* Chama o manipulador de autorização para verificar se que o usuário possui o contato.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Atualização de edição

Atualizar `Edit` métodos para usar o manipulador de autorização para verificar se o usuário possui o contato. Como estamos executando a autorização de recursos não podemos usar o `[Authorize]` atributo. Não temos acesso ao recurso quando atributos são avaliados. Autorização de recursos com base deve ser obrigatória. Verificações de devem ser executadas depois que temos acesso ao recurso, carregando-o em nosso controlador ou carregá-lo dentro do manipulador de si mesmo. Frequentemente, você vai acessar o recurso, passando a chave de recurso.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Atualizar o método Delete

Atualizar `Delete` métodos para usar o manipulador de autorização para verificar se o usuário possui o contato.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Injetar o serviço de autorização para os modos de exibição

Atualmente a interface do usuário mostra editar e excluir links de dados que o usuário não pode modificar. Corrigiremos que aplicando o manipulador de autorização para os modos de exibição.

Injetar o serviço de autorização no *Views/_ViewImports.cshtml* arquivo para que fique disponível para todos os modos de exibição:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Atualização de *Views/Contacts/Index.cshtml* exibição do Razor para somente exibir editar e excluir links para os usuários que podem editar/excluir o contato.

Adicionar`@using ContactManager.Authorization;`

Atualização de `Edit` e `Delete` links para elas são renderizadas somente para usuários com permissão Editar e excluir o contato.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Aviso: Ocultar links de usuários que não têm permissão para editar ou excluir dados não protege o aplicativo. Ocultar links torna o aplicativo de usuário mais amigável exibindo links só é válidas. Os usuários podem hack as URLs geradas para chamar editar e excluir operações nos dados que não possuem.  O controlador deve repetir que verifica o acesso para ser protegido.

### <a name="update-the-details-view"></a>Atualizar a exibição de detalhes

Atualize a exibição de detalhes para que os gerentes podem aprovar ou rejeitar contatos:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Testar o aplicativo concluído

Se você estiver usando o código do Visual Studio ou teste na plataforma local que não inclui um certificado de teste para SSL:

- Definir `"LocalTest:skipSSL": true` no *appSettings. JSON* arquivo.

Se você executa o aplicativo e contatos, exclua todos os registros de `Contact` de tabela e reinicie o aplicativo para propagar o banco de dados. Se você estiver usando o Visual Studio, você precisa sair e reiniciar o IIS Express para propagar o banco de dados.

Registre um usuário para procurar os contatos.

Uma maneira fácil de testar o aplicativo concluído é iniciar três navegadores diferentes (ou versões de incógnita/InPrivate). Em um navegador, registrar um novo usuário, por exemplo, `test@example.com`. Entrar para cada localizador com um usuário diferente. Verifique o seguinte:

* Os usuários registrados podem exibir todos os dados de contato aprovados.
* Os usuários registrados podem editar/excluir seus próprios dados. 
* Os gerentes podem aprovar ou rejeitar dados de contato. O `Details` exibição mostra **aprovar** e **rejeitar** botões. 
* Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.

| User| Opções |
| ------------ | ---------|
| test@example.com | Pode editar/excluir próprios dados |
| manager@contoso.com | Rejeitar/aprovar e editar/excluir podem ter dados  |
| admin@contoso.com | Pode editar/excluir e Aprovar/rejeitar todos os dados|

Crie um contato no navegador de administradores. Copie a URL para excluir e editar o contato de administrador. Cole esses links no navegador do usuário de teste para verificar se que o usuário de teste não pode executar essas operações.

## <a name="create-the-starter-app"></a>Criar o aplicativo de início

Siga estas instruções para criar o aplicativo de início.

* Criar um **aplicativo Web do ASP.NET Core** usando [2017 do Visual Studio](https://www.visualstudio.com/) chamado "ContactManager"

  * Criar o aplicativo com **contas de usuário individuais**.
  * O nome "ContactManager" para que o namespace corresponderá o uso de namespace no exemplo.

* Adicione o seguinte `Contact` modelo:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Scaffold o `Contact` modelo usando o Entity Framework Core e o `ApplicationDbContext` contexto de dados. Aceite todos os padrões de scaffolding. Usando `ApplicationDbContext` para o contexto de dados classe coloca a tabela de contato na [identidade](xref:security/authentication/identity) banco de dados. Consulte [adicionando um modelo de](xref:tutorials/first-mvc-app/adding-model) para obter mais informações.

* Atualização o **ContactManager** fixar no *Views/Shared/_Layout.cshtml* arquivo `asp-controller="Home"` para `asp-controller="Contacts"` tocando assim o **ContactManager** link chamar o controlador de contatos. A marcação original:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

A marcação atualizada:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Scaffold a migração inicial e atualizar o banco de dados

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Testar o aplicativo, criar, editar e excluir um contato

### <a name="seed-the-database"></a>Propagar o banco de dados

Adicionar o `SeedData` de classe para o *dados* pasta. Se você baixou o exemplo, você pode copiar o *SeedData.cs* o arquivo para o *dados* pasta do projeto starter.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Adicione o código realçado até o final do `Configure` método o *Startup.cs* arquivo:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Teste o aplicativo propagado o banco de dados. O método de propagação não será executado se há quaisquer linhas no banco de dados de contato.

### <a name="create-a-class-used-in-the-tutorial"></a>Criar uma classe usada no tutorial

* Crie uma pasta chamada *autorização*.
* Copie o *Authorization\ContactOperations.cs* do arquivo de download do projeto concluído ou copie o seguinte código:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Recursos adicionais

* [Laboratório de autorização de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Este laboratório apresenta mais detalhes sobre os recursos de segurança introduzidos neste tutorial.
* [Autorização no ASP.NET Core: Simple, função, baseada em declarações e personalizada](index.md)
* [Autorização baseada em política personalizada](policies.md)
