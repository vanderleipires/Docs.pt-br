---
title: Suporte a norma de proteção de dados geral (GDPR) no núcleo do ASP.NET
author: rick-anderson
description: Saiba como acessar os pontos de extensão GDPR em um aplicativo web do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: c986eeca572eecb43e76d56dbc5cb872a9dff6b2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277632"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Suporte de regulamentação de proteção de dados geral (GDPR) da UE no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core fornece APIs e modelos para ajudar a atender alguns do [regulamentação de proteção de dados geral (GDPR) da UE](https://www.eugdpr.org/) requisitos:

* Os modelos de projeto incluem pontos de extensão e marcação fragmentada, que você pode substituir por sua privacidade e a política de uso de cookies.
* Um recurso de consentimento do cookie permite solicitar (e acompanhar) consentimento dos usuários para o armazenamento de informações pessoais. Se um usuário não aceitou a coleta de dados e o aplicativo é definido com [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) para `true`, cookies não-essenciais não serão enviados para o navegador.
* Cookies podem ser marcados como essenciais. Cookies essenciais são enviados para o navegador, mesmo quando o usuário não aceitou e rastreamento é desabilitado.
* [Cookies de sessão e TempData](#tempdata) não são funcionais quando o controle está desabilitado.
* O [gerenciar identidade](#pd) página fornece um link para baixar e excluir dados de usuário.

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) permite que você teste a maioria dos pontos de extensão GDPR e APIs adicionado aos modelos ASP.NET Core 2.1. Consulte o [Leiame](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) arquivo para testar as instruções.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Suporte do ASP.NET Core GDPR no código de modelo gerado

Páginas Razor e MVC projetos criados com os modelos de projeto incluem o seguinte GDPR:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) são definidos no `Startup`.
* O *_CookieConsentPartial.cshtml* [exibição parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* O *Pages/Privacy.cshtml* ou *Home/Privacy.cshtml* exibição fornece uma página detalhada da política de privacidade do seu site. O *_CookieConsentPartial.cshtml* arquivo gera um link para a página de privacidade.
* Para aplicativos criados com contas de usuário individuais, a página de gerenciamento fornece links para baixar e excluir [dados pessoais do usuário](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions e UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) são inicializadas no `Startup` classe `ConfigureServices` método:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) é chamado `Startup` classe `Configure` método:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Exibição parcial _CookieConsentPartial.cshtml

O *_CookieConsentPartial.cshtml* exibição parcial:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Essa parcial:

* Obtém o estado do controle do usuário. Se o aplicativo está configurado para exigir o consentimento do usuário deve consentir antes de cookies que podem ser controlados. Se o consentimento for necessário, o chrome de consentimento do cookie é fixa na parte superior da barra de navegação criada no *Pages/Shared/_Layout.cshtml* arquivo.
* Fornece um HTML `<p>` elemento para resumir sua privacidade e cookies usar a política.
* Fornece um link para *Pages/Privacy.cshtml* onde você pode detalhar a política de privacidade do seu site.

## <a name="essential-cookies"></a>Cookies essenciais

Se o consentimento não foi fornecido, somente os cookies marcados essenciais são enviados para o navegador. O código a seguir faz com que um cookie essenciais:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Cookies de estado de sessão e de provedor TempData não são essenciais

O [provedor Tempdata](xref:fundamentals/app-state#tempdata) cookie não é essencial. Se o controle está desabilitado, o provedor de Tempdata não está funcionando. Para habilitar o provedor Tempdata quando o controle está desabilitado, marcar o cookie de TempData como essenciais em `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Estado de sessão](xref:fundamentals/app-state) cookies não são essenciais. Estado da sessão não está funcionando, quando o controle está desabilitado.

<a name="pd"></a>

## <a name="personal-data"></a>Dados pessoais

Aplicativos do ASP.NET Core criados com contas de usuário individuais incluem código para baixar e excluir dados pessoais.

Selecione o nome de usuário e, em seguida, selecione **dados pessoais**:

![Gerenciar a página de dados pessoais](gdpr/_static/pd.png)

Notas:

* Para gerar o `Account/Manage` de código, consulte [Scaffold identidade](xref:security/authentication/scaffold-identity).
* Excluir e baixar apenas impacto sobre os dados de identidade padrão. Os dados de usuário personalizadas de criação de aplicativos devem ser estendidos para delete/baixar os dados de usuário personalizada. O problema do GitHub [como adicionar/excluir dados de usuário personalizada para identidade](https://github.com/aspnet/Docs/issues/6226) acompanha um artigo proposto na criação personalizada/excluir/download de dados de usuário personalizada. Se você quiser ver esse tópico priorizado, deixe um polegar para cima reação no problema.
* Salvo tokens para o usuário que são armazenados na tabela de banco de dados de identidade `AspNetUserTokens` são excluídos quando o usuário é excluído por meio do comportamento de exclusão em cascata devido ao [chave estrangeira](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Criptografia em repouso

Alguns bancos de dados e mecanismos de armazenamento permitem criptografia em repouso. Criptografia em repouso:

* Criptografa dados armazenados automaticamente.
* Criptografa sem configuração, programação ou outro trabalho para o software que acessa os dados.
* É a opção mais fácil e segura.
* Permite que o banco de dados gerenciar chaves e criptografia.

Por exemplo:

* Microsoft SQL e SQL Azure fornecem [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [O SQL Azure criptografa o banco de dados por padrão](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Blobs do Azure, arquivos, tabela e fila de armazenamento são criptografadas por padrão](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Você poderá usar a criptografia de disco para fornecer a mesma proteção para bancos de dados que não fornecem criptografia interna em repouso. Por exemplo:

* [BitLocker para Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Recursos adicionais

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
