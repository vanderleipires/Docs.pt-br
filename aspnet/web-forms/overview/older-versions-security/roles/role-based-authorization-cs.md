---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autorização baseada em função (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial começa com uma olhada em como o framework de funções associa uma função do usuário com o contexto de segurança. Depois, ele examina a aplicação de URL com base em função...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 0a494e697eba44fcbf373c979e119572a8e37565
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="role-based-authorization-c"></a>Autorização baseada em função (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Este tutorial começa com uma olhada em como o framework de funções associa uma função do usuário com o contexto de segurança. Em seguida, ele examina como aplicar regras de autorização de URL com base em função. A seguir que, examinaremos usando meios programáticos e declarativos para alterar os dados exibidos e a funcionalidade oferecida por uma página ASP.NET.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial vimos como usar autorização de URL para especificar que os usuários podem visitar um determinado conjunto de páginas. Com apenas um pouco de marcação em `Web.config`, podemos pode instruir o ASP.NET para permitir que somente usuários autenticados visita uma página. Ou podemos foi ditam que somente usuários Tito e Bob eram permitidos ou indicar que todos os usuários autenticados, exceto o Sam foram permitidos.

Além de autorização de URL, também vimos programáticas e declarativas técnicas para controlar os dados exibidos e a funcionalidade oferecida por uma página com base no usuário visitar. Em particular, criamos uma página que lista o conteúdo do diretório atual. Qualquer pessoa pode visite essa página, mas somente os usuários autenticados podem exibir os conteúdos dos arquivos e Tito só foi possível excluir os arquivos.

Aplicar regras de autorização em uma base por usuário pode se tornar um pesadelo de contabilidade. Uma abordagem mais passível de manutenção é usar a autorização baseada em função. A boa notícia é que as ferramentas à nossa disposição para aplicar regras de autorização funcionam igualmente bem com funções que têm contas de usuário. Regras de autorização de URL podem especificar funções em vez de usuários. O controle LoginView, que renderiza a saída diferentes para usuários anônimos e autenticados, pode ser configurado para exibir conteúdo diferente com base nas funções do usuário conectado. E a API de funções inclui métodos para determinar as funções do usuário conectado.

Este tutorial começa com uma olhada em como o framework de funções associa uma função do usuário com o contexto de segurança. Em seguida, ele examina como aplicar regras de autorização de URL com base em função. A seguir que, examinaremos usando meios programáticos e declarativos para alterar os dados exibidos e a funcionalidade oferecida por uma página ASP.NET. Vamos começar!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Compreendendo como funções associadas ao contexto de segurança do usuário

Sempre que a solicitação insere o pipeline do ASP.NET está associado um contexto de segurança, que inclui informações que identificam o solicitante. Ao usar a autenticação de formulários, um tíquete de autenticação é usado como um token de identidade. Conforme abordado no <a id="_msoanchor_2"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) e <a id="_msoanchor_3"> </a> [ *formulários Configuração de autenticação e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutoriais, o `FormsAuthenticationModule` é responsável por determinar a identidade do solicitante, o que ocorre durante o [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Se for encontrado um tíquete de autenticação válido, não expirou, o `FormsAuthenticationModule` decodifica para verificar a identidade do solicitante. Cria um novo `GenericPrincipal` do objeto e o atribui como a `HttpContext.User` objeto. A finalidade de uma entidade, como `GenericPrincipal`, é identificar o nome do usuário autenticado e quais funções que ela pertence ao. Essa finalidade é evidente pelo fato de que todos os objetos principais têm um `Identity` propriedade e um `IsInRole(roleName)` método. O `FormsAuthenticationModule`, no entanto, não está interessado no registro de informações de função e o `GenericPrincipal` cria o objeto não especificar a todas as funções.

Se a estrutura de funções estiver habilitada, o [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) módulo HTTP etapas após a `FormsAuthenticationModule` e identifica as funções do usuário autenticado durante o [ `PostAuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que é acionado depois que o `AuthenticateRequest` evento. Se a solicitação for de um usuário autenticado, o `RoleManagerModule` substitui o `GenericPrincipal` objeto criado pelo `FormsAuthenticationModule` e substitui-lo com um [ `RolePrincipal` objeto](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). O `RolePrincipal` classe usa a API de funções para determinar as funções que o usuário pertence.

A Figura 1 mostra o fluxo de trabalho de pipeline do ASP.NET ao usar autenticação de formulários e a estrutura de funções. O `FormsAuthenticationModule` executa primeiro, identifica o usuário por meio de seu tíquete de autenticação e cria um novo `GenericPrincipal` objeto. Em seguida, o `RoleManagerModule` etapas e substitui o `GenericPrincipal` do objeto com um `RolePrincipal` objeto.

Se um usuário anônimo visitar o site, não o `FormsAuthenticationModule` nem o `RoleManagerModule` cria um objeto de entidade.


[![Os eventos de Pipeline do ASP.NET para um usuário autenticado ao usar autenticação de formulários e a estrutura de funções](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Figura 1**: A eventos de Pipeline do ASP.NET para um autenticado usuário ao usar autenticação de formulários e a estrutura de funções ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Informações de função em um Cookie do cache

O `RolePrincipal` do objeto `IsInRole(roleName)` chamadas de método `Roles.GetRolesForUser` para obter as funções para o usuário para determinar se o usuário é membro do *roleName*. Ao usar o `SqlRoleProvider`, isso resulta em uma consulta para o banco de dados do repositório de função. Ao usar regras de autorização de URL com base em função do `RolePrincipal`do `IsInRole` método será chamado em cada solicitação para uma página que é protegida por regras de autorização de URL com base em função. Em vez de ter que pesquisar as informações de função no banco de dados em cada solicitação, a estrutura de funções inclui uma opção para armazenar em cache as funções do usuário em um cookie.

Se a estrutura de funções estiver configurada para armazenar em cache as funções do usuário em um cookie, o `RoleManagerModule` cria o cookie durante o pipeline do ASP.NET [ `EndRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Esse cookie é usado em solicitações subsequentes no `PostAuthenticateRequest`, que é quando o `RolePrincipal` objeto é criado. Se o cookie é válido e não expirou, os dados no cookie são analisados e usados para preencher as funções do usuário, economizando a `RolePrincipal` de ter que fazer uma chamada para o `Roles` classe para determinar as funções do usuário. Figura 2 mostra o fluxo de trabalho.


[![Informações de função do usuário podem ser armazenadas em um Cookie para melhorar o desempenho](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Figura 2**: função podem ser armazenadas do usuário em um Cookie para melhorar o desempenho ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image6.png))


Por padrão, o mecanismo de cookie do cache de função está desabilitado. Ele pode ser habilitado por meio de `<roleManager>` marcação de configuração em `Web.config`. Discutimos usando o [ `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx) para especificar provedores de função no <a id="_msoanchor_4"> </a> [ *criando e gerenciando funções* ](creating-and-managing-roles-cs.md) tutorial, Portanto, você já deve ter esse elemento em seu aplicativo `Web.config` arquivo. As configurações de cookie do cache de função são especificadas como atributos do `<roleManager>` elemento e são resumidos na tabela 1.

> [!NOTE]
> As definições de configuração listadas na tabela 1 especificam as propriedades do cookie de cache de função resultante. Para obter mais informações sobre cookies, como elas funcionam e as várias propriedades, leia [este tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descrição</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Um valor booliano que indica se o cache de cookie é usado. Assume o padrão de `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     O nome do cookie de cache de função. O valor padrão é ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                O caminho para o cookie de nome de funções. O atributo de caminho permite que um desenvolvedor limitar o escopo de um cookie para uma hierarquia de diretórios específica. O valor padrão é a barra "/", que informa ao navegador para enviar o cookie de tíquete de autenticação a qualquer solicitação feita no domínio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica as técnicas usadas para proteger o cookie do cache de função. Os valores permitidos são: `All` (padrão). `Encryption`; `None`; e `Validation`. Voltar para a etapa 3 do <a id="_anchor_5"> </a> [ *configuração de autenticação de formulários e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutorial para obter mais informações sobre os níveis de proteção.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Um valor booliano que indica se uma conexão SSL é necessária para transmitir o cookie de autenticação. O valor padrão é `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Um valor booliano que indica que se o tempo de limite do cookie é redefinido cada vez que o usuário acessa o site durante uma única sessão. O valor padrão é `false`. Este valor só é pertinente quando `createPersistentCookie` é definido como `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Especifica o tempo, em minutos, após o qual o cookie de tíquete de autenticação expira. O valor padrão é `30`. Este valor só é pertinente quando `createPersistentCookie` é definido como `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Um valor booleano que especifica se o cookie do cache de função é um cookie de sessão ou um cookie persistente. Se `false` (padrão), um cookie de sessão é usado, qual é excluído quando o navegador for fechado. Se `true`, um cookie persistente é usado; expirar `cookieTimeout` número de minutos depois de ele ter sido criado ou após a visita anterior, dependendo do valor de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Especifica o valor de domínio do cookie. O valor padrão é uma cadeia de caracteres vazia, o que faz com que o navegador para usar o domínio do qual ele foi emitido (como www.yourdomain.com). Nesse caso, o cookie será <strong>não</strong> ser enviada ao fazer solicitações para sub-domínios, como admin.yourdomain.com. Se você quiser que o cookie a ser passado para todos os subdomínios que você precisa personalizar o `domain` atributo, definindo-a como "SeuDomínio".                                                                                                                                                 |
|    `maxCachedResults`     | Especifica o número máximo de nomes de função são armazenados em cache no cookie. O padrão é 25. O `RoleManagerModule` não cria um cookie para usuários que pertencem a mais de `maxCachedResults` funções. Consequentemente, o `RolePrincipal` do objeto `IsInRole` método usará a `Roles` classe para determinar as funções do usuário. O motivo `maxCachedResults` existe é porque muitos agentes de usuário não permite mais de 4.096 bytes de cookies. Portanto esse limite destina-se para reduzir a probabilidade de superar essa limitação de tamanho. Se você tiver nomes de função muito longa, você talvez queira considerar especificando um menor `maxCachedResults` valor; contrariwise, se você tiver nomes de função muito curto, você provavelmente pode aumentar esse valor. |

**Tabela 1:** as opções de configuração de Cookie de Cache de função

Vamos configurar o nosso aplicativo para usar cookies de cache de função não persistente. Para fazer isso, atualize o `<roleManager>` elemento `Web.config` para incluir os seguintes atributos de cookie:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

Atualizei o `<roleManager>` elemento adicionando três atributos: `cacheRolesInCookie`, `createPersistentCookie`, e `cookieProtection`. Definindo `cacheRolesInCookie` para `true`, o `RoleManagerModule` serão agora automaticamente armazenados em cache as funções do usuário em um cookie em vez de precisar pesquisar informações de função do usuário em cada solicitação. Posso definir explicitamente o `createPersistentCookie` e `cookieProtection` atributos para `false` e `All`, respectivamente. Tecnicamente, não preciso especificar valores para esses atributos desde que atribuí-los apenas para os valores padrão, mas eu colocá-los aqui para torná-lo explicitamente limpar que não estou usando cookies persistentes e que o cookie é criptografado e validado.

Isso é tudo que é necessário para que ele! Daqui em diante, o framework de funções armazenará em cache as funções de usuários em cookies. Se o navegador do usuário não dá suporte a cookies, ou se os cookies são excluídos ou perdidos, de alguma forma, é não seria difícil – o `RolePrincipal` objeto simplesmente usará o `Roles` classe no caso de nenhum cookie (ou um inválida ou expirado) está disponível.

> [!NOTE]
> Padrões da Microsoft &amp; grupo práticas desencoraja o uso de cookies de cache de função persistente. Desde que a posse do cookie de cache de função é suficiente para comprovar a associação de função, se um hacker alguma forma pode obter acesso ao cookie de um usuário válido ele pode representar o usuário. Aumenta a probabilidade de que isso ocorra se o cookie é mantido no navegador do usuário. Para obter mais informações sobre esta recomendação de segurança, bem como outras questões de segurança, consulte o [lista de pergunta de segurança para o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Etapa 1: Definir regras de autorização de URL com base em função

Como discutido o <a id="_msoanchor_6"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial, a autorização de URL oferece um meio de restringir o acesso a um conjunto de páginas em um usuário por usuário ou função por função base. As regras de autorização de URL são detalhadas em `Web.config` usando o [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) com `<allow>` e `<deny>` elementos filho. Além das regras de autorização relacionadas ao usuário discutidas nos tutoriais anteriores, cada `<allow>` e `<deny>` também pode incluir o elemento filho:

- Uma função específica
- Uma lista delimitada por vírgulas de funções

Por exemplo, as regras de autorização de URL concedem acesso aos usuários nas funções de administradores e supervisores, mas negam acesso a todos os outros:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

O `<allow>` elemento na marcação acima informa que as funções de administradores e supervisores são permitidas; o `<deny>` elemento instrui que *todos os* usuários serão negados.

Vamos configurar o nosso aplicativo para que o `ManageRoles.aspx`, `UsersAndRoles.aspx`, e `CreateUserWizardWithRoles.aspx` páginas só são acessíveis aos usuários na função de administradores, enquanto o `RoleBasedAuthorization.aspx` página permanece acessível a todos os visitantes.

Para fazer isso, comece adicionando um `Web.config` o arquivo para o `Roles` pasta.


[![Adicionar um arquivo Web. config para o diretório de funções](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Figura 3**: adicionar um `Web.config` o arquivo para o `Roles` diretório ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image9.png))


Em seguida, adicione a seguinte marcação de configuração para `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

O `<authorization>` elemento o `<system.web>` seção indica que apenas os usuários da função de administradores podem acessar os recursos do ASP.NET no `Roles` directory. O `<location>` elemento define um conjunto alternativo de regras de autorização de URL para o `RoleBasedAuthorization.aspx` página, permitindo que todos os usuários visitar a página.

Depois de salvar suas alterações para `Web.config`, faça logon como um usuário que não está na função de administradores e, em seguida, tente visite uma das páginas protegidas. O `UrlAuthorizationModule` detectará que você não tem permissão para visitar o recurso solicitado; Consequentemente, o `FormsAuthenticationModule` redirecionará para a página de logon. A página de logon, em seguida, você será encaminhado para o `UnauthorizedAccess.aspx` página (consulte a Figura 4). Esse redirecionamento final da página de logon para `UnauthorizedAccess.aspx` ocorre devido ao código que adicionamos à página de logon na etapa 2 do <a id="_msoanchor_7"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial. Em particular, a página de logon redireciona automaticamente qualquer usuário autenticado `UnauthorizedAccess.aspx` se querystring contém um `ReturnUrl` parâmetro, como esse parâmetro indica que o usuário acessou a página de logon depois de tentar exibir uma página não foi autorização para exibir.


[![Somente os usuários na função de administradores podem exibir as páginas protegidas](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Figura 4**: somente os usuários na função de administradores podem exibir páginas protegido ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image12.png))


Faça logoff e faça logon como um usuário que está na função de administradores. Agora você deve ser capaz de exibir três páginas protegidas.


[![Tito pode visitar que o UsersAndRoles.aspx página porque ele está na função de administradores](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Figura 5**: Tito pode visitar o `UsersAndRoles.aspx` página porque ele está na função de administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> Ao especificar regras de autorização de URL – para funções ou usuários – é importante ter em mente que as regras são analisados um por vez, de cima para baixo. Assim que uma correspondência for encontrada, o usuário é concedido ou negado o acesso, dependendo se a correspondência foi encontrada em um `<allow>` ou `<deny>` elemento. **Se nenhuma correspondência for encontrada, é concedido acesso ao usuário.** Consequentemente, se você quiser restringir o acesso a uma ou mais contas de usuário, é essencial que você use um `<deny>` como o último elemento na configuração de autorização de URL. **Se as regras de autorização de URL não incluir um**`<deny>`**elemento, todos os usuários terão acesso.** Para obter uma discussão mais completa sobre como as regras de autorização de URL são analisadas, consulte novamente o "uma olhada em como o `UrlAuthorizationModule` usa as regras de autorização para conceder ou negar acesso" seção o <a id="_msoanchor_8"> </a> [  *Autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Etapa 2: Limitar a funcionalidade baseada em funções do usuário conectado no momento

Torna a autorização URL fácil especificar autorização grosso regras estado quais identidades é permitida e quais têm permissão para exibir uma página específica (ou todas as páginas em uma pasta e suas subpastas). No entanto, em alguns casos, pode desejamos permitir que todos os usuários a visitar uma página, mas limitar a funcionalidade da página com base nas funções do usuário visita. Isso pode envolver mostrando ou ocultando dados com base na função do usuário ou oferecer funcionalidade adicional para usuários que pertencem a uma função específica.

Essas regras de autorização baseada em função refinadas podem ser implementadas declarativamente ou por meio de programação (ou por meio de uma combinação dos dois). Na próxima seção, veremos como implementar a autorização declarativa refinadas por meio do controle LoginView. Depois disso, iremos explorar as técnicas de programação. Antes que podemos ver a aplicação de regras de autorização refinadas, no entanto, primeiro precisamos criar uma página cuja funcionalidade depende da função do usuário visitando.

Vamos criar uma página que lista todas as contas de usuário em um controle GridView do sistema. GridView incluirá o nome de usuário, endereço de email, data do último logon e comentários sobre o usuário de cada usuário. Além de exibir informações de cada usuário, o GridView incluem editar e excluir recursos. Criaremos inicialmente nessa página com editar e excluir a funcionalidade disponível a todos os usuários. As seções "Usando o controle LoginView" e "Programaticamente limitar a funcionalidade" veremos como habilitar ou desabilitar esses recursos com base na função do usuário visita.

> [!NOTE]
> A página do ASP.NET que está prestes a criar usa um controle GridView para exibir as contas de usuário. Desde que este tutorial série se concentra em formulários de autenticação, autorização, contas de usuário e funções, não desejo gastar muito tempo discutindo o funcionamento interno do controle GridView. Enquanto este tutorial fornece instruções passo a passo específicas para a configuração nesta página, ele não entre em detalhes sobre por que certas escolhas feitas ou o que tem propriedades específicas do efeito na saída renderizada. Para obter um exame completo do controle GridView, confira meu *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais.


Comece abrindo o `RoleBasedAuthorization.aspx` página o `Roles` pasta. Arraste um controle GridView da página para o Designer e o conjunto de seus `ID` para `UserGrid`. Em alguns instantes, escreverá o código que chama o `Membership.GetAllUsers` método e associa resultante `MembershipUserCollection` objeto GridView. O `MembershipUserCollection` contém um `MembershipUser` objeto para cada conta de usuário no sistema. `MembershipUser` objetos têm propriedades como `UserName`, `Email`, `LastLoginDate`e assim por diante.

Antes que escrever o código que associa as contas de usuário para a grade, vamos primeiro definir campos do GridView. Marca inteligente do GridView, clique no link de "Editar colunas" para iniciar a caixa de diálogo campos caixa (veja a Figura 6). Aqui, desmarque a caixa de seleção "gerar automaticamente campos" no canto inferior esquerdo. Como queremos que este GridView incluem editar e excluir recursos, adicionar um CommandField e definir seu `ShowEditButton` e `ShowDeleteButton` propriedades como True. Em seguida, adicione os quatro campos para exibir o `UserName`, `Email`, `LastLoginDate`, e `Comment` propriedades. Usar um BoundField para as duas propriedades somente leitura (`UserName` e `LastLoginDate`) e TemplateFields para os dois campos editáveis (`Email` e `Comment`).

Tem a primeira exibição BoundField o `UserName` propriedade; defina seu `HeaderText` e `DataField` propriedades para "UserName". Este campo não será editável, portanto, definir seu `ReadOnly` propriedade como True. Configurar o `LastLoginDate` BoundField definindo seu `HeaderText` para "Último logon" e seu `DataField` para "LastLoginDate". Vamos formatar a saída desse BoundField para que apenas a data é exibida (em vez da data e hora). Para fazer isso, defina este BoundField `HtmlEncode` a propriedade como False e sua `DataFormatString` propriedade como "{0: d}". Defina também a `ReadOnly` propriedade como True.

Definir o `HeaderText` propriedades de as dois TemplateFields "Email" e "Comment".


[![Campos do GridView podem ser configurados por meio da caixa de diálogo de campos](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Figura 6**: campos pode ser configurado por meio da caixa o GridView de diálogo campos ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image18.png))


Agora, precisamos definir o `ItemTemplate` e `EditItemTemplate` para o "Email" e "Comment" TemplateFields. Adicionar um controle da Web de rótulo a cada o `ItemTemplate` s e associar seus `Text` propriedades para o `Email` e `Comment` propriedades, respectivamente.

Para TemplateField "Email", adicione uma caixa de texto denominada `Email` para seus `EditItemTemplate` e associar seu `Text` propriedade para o `Email` propriedade usando associação de dados bidirecional. Adicionar um RequiredFieldValidator e RegularExpressionValidator como o `EditItemTemplate` para garantir que um visitante de edição da propriedade Email inseriu um endereço de email válido. Para TemplateField "Comment", adicione uma caixa de texto de várias linha denominada `Comment` para seu `EditItemTemplate`. Defina a caixa de texto `Columns` e `Rows` propriedades 40 e 4, respectivamente e, em seguida, associar seu `Text` propriedade para o `Comment` propriedade usando associação de dados bidirecional.

Depois de configurar essas TemplateFields, sua marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Ao editar ou excluir uma conta de usuário que precisamos saber que o usuário `UserName` o valor da propriedade. Definir o GridView `DataKeyNames` propriedade como "UserName" para que essas informações estão disponíveis por meio do GridView `DataKeys` coleção.

Finalmente, adicione um controle ValidationSummary para a página e defina seu `ShowMessageBox` propriedade como True e sua `ShowSummary` a propriedade como False. Com essas configurações, o ValidationSummary exibirá um alerta do lado do cliente, se o usuário tentar editar uma conta de usuário com um endereço de email ausente ou inválido.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Agora podemos concluiu declarativo desta página. Nossa próxima tarefa é associar o conjunto de contas de usuário a GridView. Adicionar um método chamado `BindUserGrid` para o `RoleBasedAuthorization.aspx` classe code-behind da página que vincula a `MembershipUserCollection` retornado por `Membership.GetAllUsers` para o `UserGrid` GridView. Chamar esse método a partir de `Page_Load` manipulador de eventos na primeira visita de página.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Com esse código, visite a página por meio de um navegador. Como mostra a Figura 7, você deverá ver um GridView listando as informações sobre cada conta de usuário no sistema.


[![UserGrid GridView lista informações sobre cada usuário no sistema](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Figura 7**: O `UserGrid` GridView lista informações sobre cada usuário do sistema ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> O `UserGrid` GridView lista todos os usuários em uma interface não-paginável. Essa interface de grade simples não é adequado para cenários onde vários usuários dúzias ou mais. Uma opção é configurar o GridView para habilitar a paginação. O `Membership.GetAllUsers` método tem duas sobrecargas: um que não aceita entrados parâmetros e retorna todos os usuários e outro que usa valores inteiros para o índice de página e tamanho de página e retorna somente o subconjunto especificado dos usuários. A segunda sobrecarga pode ser usado para percorrer com mais eficiência os usuários já que ele retorna apenas o subconjunto preciso de contas de usuário em vez de *todos os* deles. Se você tem milhares de contas de usuário, você pode querer uma interface baseada em filtro, que mostra somente os usuários cujo nome começa com um caractere selecionado, por exemplo. O [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) é ideal para a criação de uma interface de usuário com base no filtro. Abordaremos a criação de tal interface em um tutorial futuras.


O controle GridView oferece edição e exclusão de suporte quando o controle está vinculado a um controle de fonte de dados configurada adequadamente, como o SqlDataSource ou ObjectDataSource internos. O `UserGrid` GridView, no entanto, tem seus dados por meio de programação associados; portanto, podemos deve escrever código para executar essas duas tarefas. Em particular, será necessário criar manipuladores de eventos do GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, e `RowDeleting` eventos que são disparados quando um visitante clica o GridView editar, cancelar, atualização, ou excluir botões.

Comece criando os manipuladores de eventos do GridView `RowEditing`, `RowCancelingEdit`, e `RowUpdating` eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

O `RowEditing` e `RowCancelingEdit` manipuladores de eventos simplesmente definem o GridView `EditIndex` propriedade e religue, em seguida, a lista de usuários de contas à grade. As coisas interessantes acontece no `RowUpdating` manipulador de eventos. Este manipulador de eventos é iniciado, garantindo que os dados são válidos e, em seguida, captura o `UserName` valor da conta do usuário editado do `DataKeys` coleção. O `Email` e `Comment` caixas de texto em 'TemplateFields dois `EditItemTemplate` s são referenciados por meio de programação. Seus `Text` propriedades contêm o endereço de email editado e o comentário.

Para atualizar uma conta de usuário por meio da API de associação, é necessário primeiro obter as informações do usuário, que é feito por meio de uma chamada para `Membership.GetUser(userName)`. Retornado `MembershipUser` do objeto `Email` e `Comment` propriedades, em seguida, são atualizadas com os valores inseridos nas duas caixas de texto de interface de edição. Por fim, essas modificações são salvos com uma chamada para [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). O `RowUpdating` manipulador de eventos é concluída, levando a GridView à sua interface previamente edição.

Em seguida, crie o `RowDeleting` manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

O manipulador de eventos acima começa captando a `UserName` valor a partir do GridView `DataKeys` coleção; isso `UserName` valor é então passado para a classe de associação [ `DeleteUser` método](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). O `DeleteUser` método exclui a conta de usuário do sistema, incluindo dados de associação relacionada (como as funções que este usuário pertence). Depois de excluir o usuário, a grade `EditIndex` é definido como -1 (no caso do usuário clicou em Excluir enquanto outra linha estava no modo de edição) e o `BindUserGrid` método é chamado.

> [!NOTE]
> O botão de exclusão não exige qualquer tipo de confirmação do usuário antes de excluir a conta de usuário. Recomendo que você adicione alguma forma de confirmação do usuário para reduzir a chance de uma conta que está sendo excluída acidentalmente. Uma das maneiras mais fáceis para confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [' adicionando confirmação do lado do cliente quando excluindo](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Verifique se essa página funciona conforme o esperado. Você deve ser capaz de editar o endereço de email de qualquer usuário e comentário, bem como excluir qualquer conta de usuário. Desde o `RoleBasedAuthorization.aspx` está acessível a todos os usuários, qualquer usuário – mesmo anônimos visitantes – visite essa página e editar e excluir contas de usuário! Vamos atualizar esta página para que somente os usuários em funções de supervisores e administradores podem editar o endereço de email do usuário e comentário, e somente os administradores podem excluir uma conta de usuário.

A seção "Usando o controle LoginView" examina usando o controle LoginView para mostrar as instruções específicas para a função do usuário. Se uma pessoa na função de administradores visita a esta página, mostraremos instruções sobre como editar e excluir usuários. Se um usuário na função de supervisores atingir essa página, mostraremos instruções sobre como editar os usuários. E se o visitante é anônimo ou não está na função de supervisores ou administradores, podemos exibirá uma mensagem explicando o que não é possível editar ou excluir informações de conta de usuário. Na seção "Programaticamente limitar a funcionalidade" nós irá escrever código que mostra ou oculta os botões Editar e excluir com base na função do usuário de forma programática.

### <a name="using-the-loginview-control"></a>Usando o controle LoginView

Como vimos nos tutoriais anteriores, o controle LoginView é útil para exibir as interfaces diferentes para usuários anônimos e autenticados, mas o controle LoginView também pode ser usado para exibir uma marcação diferente com base nas funções do usuário. Vamos usar um controle LoginView para exibir instruções diferentes com base na função do usuário visita.

Comece adicionando um LoginView acima de `UserGrid` GridView. Como já discutimos anteriormente, o controle LoginView tem dois modelos internos: `AnonymousTemplate` e `LoggedInTemplate`. Digite uma mensagem breve em ambos esses modelos que informa ao usuário que não é possível editar ou excluir informações de usuário.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Além de `AnonymousTemplate` e `LoggedInTemplate`, pode incluir o controle LoginView *RoleGroups*, que são modelos específicos de função. Cada RoleGroup contém uma única propriedade, `Roles`, que especifica as funções que o RoleGroup aplica-se a. O `Roles` propriedade pode ser definida para uma única função (como "administradores") ou uma lista delimitada por vírgulas de funções (como "administradores, supervisores").

Para gerenciar o RoleGroups, clique no link de "Editar RoleGroups" da marca inteligente do controle para exibir o Editor de coleção de RoleGroup. Adicione dois RoleGroups novo. Definir o RoleGroup primeiro `Roles` propriedade como "Administradores" e a segunda para "Supervisores".


[![Gerenciar modelos de função específica do LoginView por meio do Editor de coleção de RoleGroup](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Figura 8**: gerenciar específicas de função modelos por meio do RoleGroup Editor do LoginView de coleção ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image24.png))


Clique em Okey para fechar o Editor de coleção de RoleGroup; Isso atualiza a marcação declarativa do LoginView para incluir um `<RoleGroups>` seção com um `<asp:RoleGroup>` elemento filho para cada RoleGroup definido no Editor de coleção de RoleGroup. Além disso, na lista suspensa "Modos de exibição" lista na marca inteligente do LoginView - listados inicialmente apenas o `AnonymousTemplate` e `LoggedInTemplate` – agora inclui também o RoleGroups adicionado.

Edite o RoleGroups para que os usuários na função de supervisores exibidas instruções sobre como editar contas de usuário, enquanto os usuários da função de administradores são mostrados as instruções para edição e exclusão. Depois de fazer essas alterações, declarativo do LoginView deve ser semelhante ao seguinte.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Depois de fazer essas alterações, salve a página e, em seguida, visite-o por meio de um navegador. Primeiro, visite a página como um usuário anônimo. Você deve ser mostrada a mensagem, "você não está conectado ao sistema. Portanto, você não pode editar ou excluir informações de usuário." Em seguida, faça logon como um usuário autenticado, mas que não é nem na função supervisores nem os administradores. Neste momento, você verá a mensagem "você não é um membro das funções supervisores ou administradores. Portanto, você não pode editar ou excluir informações de usuário."

Em seguida, faça logon como um usuário que seja um membro da função de supervisores. Neste momento, você verá supervisores específicas de função da mensagem (consulte a Figura 9). E se você efetuar login como um usuário de administradores de função, você deve ver os administradores específicas de função da mensagem (veja a Figura 10).


[![Bruce é mostrado a mensagem de função específica de supervisores](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Figura 9**: Bruce é mostrado a mensagem de função específica de supervisores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image27.png))


[![Tito é mostrada a mensagem específicas de função de administradores](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Figura 10**: Tito é mostrada a mensagem específicas de função de administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image30.png))


Como as capturas de tela figuras 9 e 10 mostram, o LoginView processa apenas um modelo, mesmo se aplicam vários modelos. Bruce e Tito são registradas em usuários, mas o LoginView processa apenas a correspondência RoleGroup e não o `LoggedInTemplate`. Além disso, Tito pertence às funções de administradores e supervisores, ainda que o controle LoginView processa o modelo de função específica de administradores em vez de supervisores um.

A Figura 11 ilustra o fluxo de trabalho usado pelo controle LoginView para determinar qual modelo para renderizar. Observe que se houver mais de um RoleGroup especificado, o modelo LoginView renderiza o *primeiro* RoleGroup correspondente. Em outras palavras, se tivesse colocamos o RoleGroup supervisores como o primeiro RoleGroup e os administradores como o segundo, ao Tito visitou esta página ele seria verá a mensagem de supervisores.


[![Fluxo de trabalho do controle LoginView para determinar qual modelo de renderização](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Figura 11**: fluxo de trabalho do controle LoginView o para determinar o modelo de renderização ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funcionalidade de limitação programaticamente

Enquanto o controle LoginView exibe instruções diferentes com base na função do usuário visitar a página, os botões Editar e Cancelar permanecem visíveis para todos. É preciso ocultar os botões Editar e excluir programaticamente para visitantes anônimos e usuários que estão na função de supervisores nem os administradores. É preciso ocultar o botão Excluir para todas as pessoas que não seja um administrador. Para fazer isso podemos gravará um trecho de código que referencia programaticamente o CommandField editar e excluir botões de link e define suas `Visible` propriedades `false`, se necessário.

A maneira mais fácil de fazer referência a controles em um CommandField programaticamente é primeiro convertê-lo em um modelo. Para fazer isso, clique no link "Editar colunas" da marca inteligente do GridView, selecione o CommandField da lista de campos atuais e clique no link "Converter este campo em um TemplateField". Isso ativa a CommandField em um TemplateField com um `ItemTemplate` e `EditItemTemplate`. O `ItemTemplate` contém a editar e excluir botões de link enquanto o `EditItemTemplate` abriga a atualização e botões de link Cancelar.


[![Converter o CommandField em um TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Figura 12**: converter o CommandField em um TemplateField ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image36.png))


Atualizar o editar e excluir botões de link no `ItemTemplate`, configuração seus `ID` propriedades para valores de `EditButton` e `DeleteButton`, respectivamente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Sempre que os dados serão associados ao GridView, GridView enumera os registros no seu `DataSource` propriedade e gera um correspondente `GridViewRow` objeto. Como cada `GridViewRow` objeto é criado, o `RowCreated` evento é acionado. Para ocultar os botões Editar e excluir usuários não autorizados, precisamos criar um manipulador de eventos para esse evento e referenciar programaticamente a editar e excluir botões de link do, definindo suas `Visible` propriedades adequadamente.

Criar um manipulador de eventos de `RowCreated` evento e, em seguida, adicione o seguinte código:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Lembre-se de que o `RowCreated` evento é acionado para *todos os* das linhas GridView, incluindo o cabeçalho, rodapé, a interface de paginação e assim por diante. Queremos referenciar programaticamente a editar e excluir botões de link se estamos lidando com uma linha de dados não está no modo de edição (uma vez que a linha no modo de edição tem os botões de atualização e Cancelar em vez de editar e excluir). Essa verificação é tratada pelo `if` instrução.

Se estamos lidando com uma linha de dados que não está no modo de edição, o editar e excluir botões de link são referenciados e seus `Visible` propriedades são definidas com base nos valores boolianos retornados pelo `User` do objeto `IsInRole(roleName)` método. O objeto de usuário faz referência a entidade criada pelo `RoleManagerModule`; Consequentemente, o `IsInRole(roleName)` método usa a API de funções para determinar se o visitante atual pertence ao *roleName*.

> [!NOTE]
> Poderíamos ter usado a classe de funções diretamente, substituindo a chamada para `User.IsInRole(roleName)` com uma chamada para o [ `Roles.IsUserInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Decidi usar o objeto principal `IsInRole(roleName)` método neste exemplo porque é mais eficiente que usar a API de funções diretamente. Anteriormente neste tutorial, configuramos o Gerenciador de funções para armazenar em cache as funções do usuário em um cookie. Esse cookie dados armazenados em cache é utilizado apenas quando a entidade de segurança `IsInRole(roleName)` é chamado de método; chamadas diretas para a API de funções sempre envolvem uma viagem para o repositório da função. Mesmo que não estão em cache funções em um cookie, chamar o objeto principal `IsInRole(roleName)` método é geralmente mais eficiente, porque quando é chamado para a primeira vez durante uma solicitação ele armazena em cache os resultados. A API de funções, por outro lado, não execute nenhum cache. Porque o `RowCreated` evento é disparado uma vez para cada linha em GridView, usando `User.IsInRole(roleName)` envolve apenas uma viagem para o repositório de função enquanto `Roles.IsUserInRole(roleName)` requer *N* viagens onde *N* é o número de contas de usuário exibido na grade.


O botão de edição `Visible` está definida como `true` se o usuário visitar essa página está na função de administradores ou supervisores; caso contrário, ele é definido como `false`. O botão de exclusão `Visible` está definida como `true` somente se o usuário está na função de administradores.

Esta página por meio de um navegador de teste. Se você visitar a página como um visitante anônimo ou como um usuário que não é um Supervisor nem um administrador, o CommandField está vazio. ele ainda existe, mas como um Silver dinâmico sem de editar ou excluir botões.

> [!NOTE]
> É possível ocultar o CommandField completamente quando um Supervisor não e não-administrador está visitando a página. Posso deixar isso como um exercício para o leitor.


[![Os botões Editar e excluir estão ocultos não supervisores e não administradores](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Figura 13**: O editar e excluir botões estão ocultos não supervisores e não-administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image39.png))


Se um usuário que pertence à função de supervisores (mas não para a função de administradores) visita, ele vê o botão de edição.


[![Enquanto o botão Editar está disponível para os supervisores, no botão Excluir está oculto](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Figura 14**: enquanto o botão Editar está disponível para os supervisores, o botão Excluir é oculto ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image42.png))


E se a visita de um administrador, ela tem acesso para os botões Editar e excluir.


[![Os botões Editar e excluir estão disponíveis somente para administradores](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Figura 15**: O editar e excluir botões estão disponíveis somente para administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Etapa 3: Aplicando regras de autorização baseada em função para Classes e métodos

Na etapa 2 limitamos editar recursos para usuários em funções de supervisores e administradores e excluir recursos aos administradores. Isso foi feito, ocultando os elementos da interface de usuário associada para usuários não autorizados por meio de técnicas de programação. Essas medidas não garante que um usuário não autorizado poderão executar uma ação com privilégios. Pode haver elementos de interface do usuário que forem adicionados posteriormente ou que esquecemos ocultar para usuários não autorizados. Ou, um hacker pode detectar uma outra maneira de obter a página do ASP.NET para executar o método desejado.

É uma maneira fácil de garantir que uma determinada parte da funcionalidade não pode ser acessada por um usuário não autorizado decorar dessa classe ou método com o [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando o tempo de execução .NET usa uma classe ou executa um de seus métodos, ele verifica para garantir que o contexto de segurança atual tem permissão. O `PrincipalPermission` atributo fornece um mecanismo pelo qual podemos definir essas regras.

Analisamos usando o `PrincipalPermission` atributo novamente o <a id="_msoanchor_9"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial. Especificamente, vimos como decorar o GridView `SelectedIndexChanged` e `RowDeleting` manipulador de eventos para que eles somente podem ser executados por usuários autenticados e Tito, respectivamente. O `PrincipalPermission` atributo funciona tão bem com funções.

Vamos demonstrar o uso de `PrincipalPermission` atributo o GridView `RowUpdating` e `RowDeleting` manipuladores de eventos para impedir que usuários não autorizados de execução. Tudo que precisamos fazer é adicionar o atributo apropriado sobre cada definição de função:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

O atributo para o `RowUpdating` manipulador de eventos indica que somente os usuários em funções de administradores ou supervisores podem executar o manipulador de eventos, enquanto que o atributo o `RowDeleting` a execução para os usuários administradores limita o manipulador de eventos função.


> [!NOTE]
> O `PrincipalPermission` atributo é representado como uma classe no `System.Security.Permissions` namespace. Certifique-se de adicionar um `using System.Security.Permissions` instrução na parte superior do seu arquivo de classe code-behind para importar esse namespace.


Se, de alguma forma, não-administrador tenta executar o `RowDeleting` manipulador de eventos ou se um não Supervisor ou não-administrador tenta executar o `RowUpdating` manipulador de eventos, o tempo de execução .NET irá gerar um `SecurityException`.


[![Se o contexto de segurança não está autorizado a executar o método, um SecurityException é gerada](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Figura 16**: se o contexto de segurança não está autorizado a executar o método, uma `SecurityException` é gerada ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image48.png))


Além de páginas ASP.NET, muitos aplicativos também têm uma arquitetura que inclui várias camadas, como camadas de acesso de dados e lógica de negócios. Essas camadas são normalmente implementadas como bibliotecas de classes e oferecem classes e métodos para executar a funcionalidade de relacionadas a dados e lógica de negócios. O `PrincipalPermission` atributo é útil para aplicar regras de autorização a essas camadas também.

Para obter mais informações sobre como usar o `PrincipalPermission` atributo para definir regras de autorização em classes e métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog [adicionando regras de autorização para os negócios e dados camadas usando `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumo

Neste tutorial vimos como especificar grosso e regras de autorização granulares com base nas funções do usuário. AS PÁGINAS ASP. O recurso de autorização de URL do NET permite que um desenvolvedor de página especificar quais identidades têm permitidas ou negadas acesso a quais páginas. Como vimos no <a id="_msoanchor_10"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial, autorização de URL regras podem ser aplicadas em uma base por usuário. Eles também podem ser aplicados em uma base de função por função, conforme vimos na etapa 1 deste tutorial.

Regras de autorização granulares podem ser aplicadas declarativamente ou programaticamente. Na etapa 2, analisamos usando o recurso de RoleGroups do controle LoginView para renderizar a saída diferente com base nas funções do usuário visita. Também vimos maneiras de determinar programaticamente se um usuário pertencer a uma função específica e como ajustar adequadamente a funcionalidade da página.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Adicionando regras de autorização para os negócios e camadas de dados usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examinando o ASP.NET de 2.0 associação, funções e perfil: Trabalhando com funções](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista de pergunta de segurança para o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentação técnica para o `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial incluem Suchi Banerjee e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](assigning-roles-to-users-cs.md)
> [Próximo](creating-and-managing-roles-vb.md)
