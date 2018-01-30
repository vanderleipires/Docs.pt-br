---
title: "Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização"
description: "Saiba como criar um aplicativo de páginas Razor com dados de usuário protegidos por autorização. Inclui o SSL, autenticação, segurança, a identidade do ASP.NET Core."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 01/24/2018
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: ff5c97feca58318d5f4e5b6a6a930c92469602ba
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

Este tutorial mostra como criar um aplicativo web do ASP.NET Core com dados de usuário protegidos por autorização. Exibe uma lista de contatos (registrados) usuários autenticados criou. Há três grupos de segurança:

* **Usuários registrados** pode exibir todos os dados aprovados e pode editar/excluir seus próprios dados.
* **Gerenciadores de** pode aprovar ou rejeitar dados de contato. Apenas os contatos aprovados são visíveis aos usuários.
* **Os administradores** pode rejeitar/aprovar e editar/excluir todos os dados.

Na imagem a seguir, o usuário Rick (`rick@example.com`) está conectado. Rick só pode exibir contatos aprovados e **editar**/**excluir**/**criar novo** links para seus contatos. Somente o último registro criado pelo Rick, exibe **editar** e **excluir** links. Outros usuários não verão o último registro até que um gerente ou administrador altera o status como "Aprovada".

![imagem descrita anterior](secure-data/_static/rick.png)

Na imagem a seguir, `manager@contoso.com` está conectado e na função gerenciadores:

![imagem descrita anterior](secure-data/_static/manager1.png)

A imagem a seguir mostra os gerentes de modo de exibição de detalhes de um contato:

![imagem descrita anterior](secure-data/_static/manager.png)

O **aprovar** e **rejeitar** botões são exibidos somente para administradores e gerentes.

Na imagem a seguir, `admin@contoso.com` está conectado e na função de administradores:

![imagem descrita anterior](secure-data/_static/admin.png)

O administrador tem todos os privilégios. Ela pode ler/editar/excluir qualquer contato e alterar o status de contatos.

O aplicativo foi criado por [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) o seguinte `Contact` modelo:

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

O exemplo contém os seguintes manipuladores de autorização:

* `ContactIsOwnerAuthorizationHandler`: Garante que um usuário só pode editar seus dados.
* `ContactManagerAuthorizationHandler`: Permite que os gerentes aprovar ou rejeitar contatos.
* `ContactAdministratorsAuthorizationHandler`: Permite aos administradores para aprovar ou rejeitar contatos e editar/excluir contatos.

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial é avançado. Você deve estar familiarizado com:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticação](xref:security/authentication/index)
* [Confirmação de conta e recuperação de senha](xref:security/authentication/accconfirm)
* [Autorização](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

A versão 1.1 do ASP.NET Core deste tutorial está no [isso](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) pasta. O 1.1 exemplo do ASP.NET Core está no [exemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>O início e o aplicativo concluído

[Baixar](xref:tutorials/index#how-to-download-a-sample) o [concluída](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicativo. [Teste](#test-the-completed-app) o aplicativo concluído para que você se familiarize com seus recursos de segurança.

### <a name="the-starter-app"></a>O aplicativo de início

[Baixar](xref:tutorials/index#how-to-download-a-sample) o [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplicativo.

Executar o aplicativo, toque o **ContactManager** vincular e verifique se você pode criar, editar e excluir um contato.

## <a name="secure-user-data"></a>Proteger os dados de usuário

As seções a seguir têm todas as principais etapas para criar o aplicativo de dados de usuário segura. Talvez seja útil para fazer referência ao projeto concluído.

### <a name="tie-the-contact-data-to-the-user"></a>Vincular os dados de contato para o usuário

Usar o ASP.NET [identidade](xref:security/authentication/identity) ID de usuário para garantir que os usuários pode editar seus dados, mas não de outros dados de usuários. Adicionar `OwnerID` e `ContactStatus` para o `Contact` modelo:

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`é a ID do usuário da `AspNetUser` tabela o [identidade](xref:security/authentication/identity) banco de dados. O `Status` campo determina se um contato é exibido por usuários gerais.

Criar uma nova migração e atualizar o banco de dados:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a>Exigir SSL e os usuários autenticados

Adicionar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) para `Startup`:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

No `ConfigureServices` método o *Startup.cs* de arquivo, adicione o [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorização:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-)]

Se você estiver usando o Visual Studio, habilite o SSL.

Para redirecionar solicitações HTTP para HTTPS, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting). Se você estiver usando o código do Visual Studio ou testes em uma plataforma local que não incluem um certificado de teste para SSL:

  Definir `"LocalTest:skipSSL": true` no *appsettings. Developement.JSON* arquivo.

### <a name="require-authenticated-users"></a>Exigir que os usuários autenticados

Defina a política de autenticação padrão para exigir que os usuários sejam autenticados. Você pode recusar a autenticação no nível de método página Razor, controlador ou ação com o `[AllowAnonymous]` atributo. Configurar a política de autenticação padrão para exigir que os usuários sejam autenticados protege recém-adicionado páginas Razor e controladores. Com a autenticação exigida por padrão é mais segura do que contar com novos controladores e páginas Razor para incluir o `[Authorize]` atributo. Adicione o seguinte para o `ConfigureServices` método o *Startup.cs* arquivo:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-)]

Adicionar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ao índice, sobre e contatos páginas para usuários anônimos podem obter informações sobre o site antes de eles se registrar. 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Adicionar `[AllowAnonymous]` para o [LoginModel e RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Configurar a conta de teste

O `SeedData` classe cria duas contas: administrador e Gerenciador. Use o [ferramenta Gerenciador de segredo](xref:security/app-secrets) para definir uma senha para essas contas. Definir a senha do diretório do projeto (o diretório que contém *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Atualização `Main` para usar a senha de teste:

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Criar as contas de teste e atualizar os contatos

Atualização de `Initialize` método o `SeedData` classe para criar as contas de teste:

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Adicione a ID de usuário de administrador e `ContactStatus` para os contatos. Tornar um dos contatos "Enviada" e um "rejeitado". Adicione a ID de usuário e o status para todos os contatos. Somente um contato é exibido:

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Criar proprietário, Gerenciador e manipuladores de autorização do administrador

Criar um `ContactIsOwnerAuthorizationHandler` classe no *autorização* pasta. O `ContactIsOwnerAuthorizationHandler` verifica que o usuário atuando em um recurso possui o recurso.

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

O `ContactIsOwnerAuthorizationHandler` chamadas [contexto. Êxito](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se o usuário autenticado atual é o proprietário do contato. Manipuladores de autorização geralmente:

* Retornar `context.Succeed` quando os requisitos são atendidos.
* Retornar `Task.CompletedTask` quando os requisitos não atendidos. `Task.CompletedTask`não é êxito ou falha&mdash;permite que outros manipuladores de autorização executar.

Se você precisar explicitamente falhar, retornar [contexto. Falha](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

O aplicativo permite que proprietários de contato para editar/excluir/criar seus próprios dados. `ContactIsOwnerAuthorizationHandler`não precisa verificar a operação passada no parâmetro de requisito.

### <a name="create-a-manager-authorization-handler"></a>Criar um manipulador de autorização do Gerenciador

Criar um `ContactManagerAuthorizationHandler` classe no *autorização* pasta. O `ContactManagerAuthorizationHandler` verifica se o usuário atuando no recurso é um Gerenciador de. Somente os gerentes podem aprovar ou rejeitar alterações de conteúdo (nova ou alteradas).

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Criar um manipulador de autorização do administrador

Criar um `ContactAdministratorsAuthorizationHandler` classe no *autorização* pasta. O `ContactAdministratorsAuthorizationHandler` verifica se o usuário atuando no recurso é um administrador. Administrador pode fazer todas as operações.

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrar os manipuladores de autorização

Serviços usando o Entity Framework Core devem ser registrados para [injeção de dependência](xref:fundamentals/dependency-injection) usando [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). O `ContactIsOwnerAuthorizationHandler` usa o ASP.NET Core [identidade](xref:security/authentication/identity), que está incluído no Entity Framework Core. Registrar os manipuladores com a coleção de serviço para que eles estejam disponíveis para o `ContactsController` por meio de [injeção de dependência](xref:fundamentals/dependency-injection). Adicione o seguinte código ao final da `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-)]

`ContactAdministratorsAuthorizationHandler`e `ContactManagerAuthorizationHandler` são adicionados como singletons. Elas são singletons porque eles não usam o EF e todas as informações necessárias no `Context` parâmetro do `HandleRequirementAsync` método.

## <a name="support-authorization"></a>Suporte à autorização

Nesta seção, você atualize as páginas Razor e adicionar uma classe de requisitos de operações.

### <a name="review-the-contact-operations-requirements-class"></a>Examine a classe de requisitos de operações de contato

Examine o `ContactOperations` classe. Essa classe contém os requisitos de aplicativo suporta:

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Criar uma classe base para as páginas Razor

Crie uma classe base que contém os serviços usados em contatos páginas Razor. A classe base coloca esse código de inicialização em um local:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

O código anterior:

* Adiciona o `IAuthorizationService` serviço acesse os manipuladores de autorização.
* Adiciona a identidade `UserManager` serviço.
* Adicione a `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Atualizar o CreateModel

Atualizar o construtor de modelo de página criar para usar o `DI_BasePageModel` classe base:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Atualização de `CreateModel.OnPostAsync` método:

* Adicione a ID de usuário para o `Contact` modelo.
* Chama o manipulador de autorização para verificar se que o usuário tem permissão para criar contatos.

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Atualizar o IndexModel

Atualização de `OnGetAsync` método apenas os contatos aprovados são mostradas a usuários gerais:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Atualizar o EditModel

Adicione um manipulador de autorização para verificar se que o usuário possui o contato. Como a autorização de recursos está sendo validada, o `[Authorize]` atributo não é suficiente. O aplicativo não tiver acesso ao recurso quando atributos são avaliados. Autorização baseada em recursos deve ser obrigatória. Verificações de devem ser executadas depois que o aplicativo tenha acesso ao recurso, carregá-lo no modelo de página ou carregá-lo dentro do manipulador de si mesmo. Frequentemente, você acessar o recurso, passando a chave de recurso.

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Atualizar o DeleteModel

Atualize o modelo de página de exclusão para usar o manipulador de autorização para verificar se que o usuário tem permissão para excluir no contato.

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Injetar o serviço de autorização para os modos de exibição

No momento, a interface do usuário mostra editar e excluir links de dados que o usuário não pode modificar. A interface do usuário é fixo, aplicando o manipulador de autorização para os modos de exibição.

Injetar o serviço de autorização no *Views/_ViewImports.cshtml* arquivo para que ele esteja disponível para todos os modos de exibição:

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

A marcação anterior adiciona várias `using` instruções.

Atualização de **editar** e **excluir** links em *Pages/Contacts/Index.cshtml* para elas estão renderizadas somente para usuários com as permissões apropriadas:

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-)]

> [!WARNING]
> Ocultar links de usuários que não tem permissão para alterar os dados, o aplicativo não seguro. Ocultar links torna o aplicativo mais amigável exibindo links só é válidas. Os usuários podem hack as URLs geradas para chamar editar e excluir operações nos dados que não possuem. O Razor de página ou o controlador deve impor verificações de acesso para proteger os dados.

### <a name="update-details"></a>Detalhes da atualização

Atualize a exibição de detalhes para que os gerentes podem aprovar ou rejeitar contatos:

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-)]

Atualize o modelo de página de detalhes:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Testar o aplicativo concluído

Se você estiver usando o código do Visual Studio ou testes em uma plataforma local que não incluem um certificado de teste para SSL:

* Definir `"LocalTest:skipSSL": true` no *appsettings. Developement.JSON* arquivo para ignorar o requisito de SSL. Ignorar SSL somente em um computador de desenvolvimento.

Se o aplicativo tiver contatos:

* Excluir todos os registros de `Contact` tabela.
* Reinicie o aplicativo para propagar o banco de dados.

Registre um usuário para procurar os contatos.

Uma maneira fácil de testar o aplicativo concluído é iniciar três navegadores diferentes (ou versões de incógnita/InPrivate). Em um navegador, registrar um novo usuário (por exemplo, `test@example.com`). Entrar para cada localizador com um usuário diferente. Verifique se as seguintes operações:

* Os usuários registrados podem exibir todos os dados de contato aprovados.
* Os usuários registrados podem editar/excluir seus próprios dados.
* Os gerentes podem aprovar ou rejeitar dados de contato. O `Details` exibição mostra **aprovar** e **rejeitar** botões.
* Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.

| User| Opções |
| ------------ | ---------|
| test@example.com | Pode editar/excluir próprios dados |
| manager@contoso.com | Rejeitar/aprovar e editar/excluir podem ter dados |
| admin@contoso.com | Pode editar/excluir e Aprovar/rejeitar todos os dados|

Crie um contato no navegador do administrador. Copie a URL para excluir e editar o contato de administrador. Cole esses links no navegador do usuário de teste para verificar se que o usuário de teste não pode executar essas operações.

## <a name="create-the-starter-app"></a>Criar o aplicativo de início

* Criar um aplicativo de páginas Razor denominado "ContactManager"

  * Criar o aplicativo com **contas de usuário individuais**.
  * O nome "ContactManager" para o espaço para nome coincida com o namespace usado no exemplo.

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * `-uld`Especifica o LocalDB, em vez de SQLite

* Adicione o seguinte `Contact` modelo:

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Scaffold o `Contact` modelo:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Atualização de **ContactManager** fixar no *Pages/_Layout.cshtml* arquivo:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Scaffold a migração inicial e atualizar o banco de dados:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Testar o aplicativo de criação, edição e exclusão de um contato

### <a name="seed-the-database"></a>Propagar o banco de dados

Adicionar o `SeedData` de classe para o *dados* pasta. Se você baixou o exemplo, você pode copiar o *SeedData.cs* o arquivo para o *dados* pasta do projeto starter.

Chamar `SeedData.Initialize` de `Main`:

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

Teste o aplicativo propagado o banco de dados. Se houver linhas no banco de dados de contato, o método de propagação não é executado.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Recursos adicionais

* [Laboratório de autorização de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Este laboratório apresenta mais detalhes sobre os recursos de segurança introduzidos neste tutorial.
* [Autorização no ASP.NET Core: Simple, função, baseada em declarações e personalizada](xref:security/authorization/index)
* [Autorização baseada em política personalizada](xref:security/authorization/policies)