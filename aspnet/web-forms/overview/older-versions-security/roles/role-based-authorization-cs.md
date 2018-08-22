---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autorização baseada em função (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial começa com uma olhada em como o framework de funções associa uma função do usuário com seu contexto de segurança. Ele, em seguida, examina como aplicar a URL baseada em função...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f947e35164724b99507858a19bdd9cd1154768a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825699"
---
<a name="role-based-authorization-c"></a>Autorização baseada em função (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Este tutorial começa com uma olhada em como o framework de funções associa uma função do usuário com seu contexto de segurança. Ele, em seguida, examina como aplicar regras de autorização de URL baseada em função. A seguir que, vamos examinar usando meios programáticos e declarativos para alterar os dados exibidos e a funcionalidade oferecida por uma página ASP.NET.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial vimos como usar a autorização de URL para especificar que os usuários podem visitar um determinado conjunto de páginas. Com apenas um pouco de marcação em `Web.config`, podemos pode instruir o ASP.NET para permitir que somente usuários autenticados visitar uma página. Ou podemos poderia ditam que somente usuários Tito e Bob eram permitidos ou indicar que todos os usuários autenticados, exceto para Sam eram permitidos.

Além de autorização de URL, também analisamos técnicas programáticas e declarativas para controlar os dados exibidos e a funcionalidade oferecida por uma página com base no usuário visitar. Em particular, nós criamos uma página que lista o conteúdo do diretório atual. Qualquer pessoa pode visitar essa página, mas somente usuários autenticados podem exibir os conteúdos dos arquivos e Tito só pode excluir os arquivos.

Aplicar regras de autorização em uma base por usuário pode se tornar um pesadelo de contabilidade. Uma abordagem de manutenção mais fácil é usar a autorização baseada em função. A boa notícia é que as ferramentas à nossa disposição para a aplicação de regras de autorização funcionam igualmente bem com as funções que funcionam para contas de usuário. As regras de autorização de URL podem especificar funções em vez de usuários. O controle LoginView, que renderiza o saída diferente para usuários autenticados e anônimos, pode ser configurado para exibir conteúdo diferente com base nas funções do usuário conectado. E a API de funções inclui métodos para determinar as funções do usuário conectado.

Este tutorial começa com uma olhada em como o framework de funções associa uma função do usuário com seu contexto de segurança. Ele, em seguida, examina como aplicar regras de autorização de URL baseada em função. A seguir que, vamos examinar usando meios programáticos e declarativos para alterar os dados exibidos e a funcionalidade oferecida por uma página ASP.NET. Vamos começar!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Compreendendo como funções são associadas com o contexto de segurança do usuário

Sempre que uma solicitação entra o pipeline do ASP.NET é associado um contexto de segurança, que inclui informações que identificam o solicitante. Ao usar a autenticação de formulários, um tíquete de autenticação é usado como um token de identidade. Como discutimos na <a id="_msoanchor_2"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) e <a id="_msoanchor_3"> </a> [ *formulários Configuração de autenticação e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutoriais, o `FormsAuthenticationModule` é responsável por determinar a identidade do solicitante, que é feito durante o [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Se um tíquete de autenticação válido não expirado é encontrado, o `FormsAuthenticationModule` decodifica-a para verificar a identidade do solicitante. Ele cria um novo `GenericPrincipal` do objeto e o atribui como a `HttpContext.User` objeto. A finalidade de uma entidade, como `GenericPrincipal`, é identificar o nome do usuário autenticado e quais funções que ela pertencem a. Essa finalidade fica evidente pelo fato de que todos os objetos de entidade de segurança têm um `Identity` propriedade e um `IsInRole(roleName)` método. O `FormsAuthenticationModule`, no entanto, não está interessado no registro de informações de função e o `GenericPrincipal` objeto, ele cria não especifica todas as funções.

Se a estrutura de funções estiver habilitada, o [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) módulo HTTP etapas após a `FormsAuthenticationModule` e identifica as funções de usuário autenticado durante o [ `PostAuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que é acionado depois que o `AuthenticateRequest` eventos. Se a solicitação for de um usuário autenticado, o `RoleManagerModule` substitui o `GenericPrincipal` objeto criado pelo `FormsAuthenticationModule` e substitui-lo com um [ `RolePrincipal` objeto](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). O `RolePrincipal` classe usa a API de funções para determinar as funções que o usuário pertence.

Figura 1 ilustra o fluxo de trabalho de pipeline do ASP.NET ao usar a autenticação de formulários e a estrutura de funções. O `FormsAuthenticationModule` é executado pela primeira vez, identifica o usuário por meio de seu tíquete de autenticação e cria um novo `GenericPrincipal` objeto. Em seguida, o `RoleManagerModule` as etapas em e substitui o `GenericPrincipal` do objeto com um `RolePrincipal` objeto.

Se um usuário anônimo visita o site, nem o `FormsAuthenticationModule` nem o `RoleManagerModule` cria um objeto de entidade.


[![Os eventos de Pipeline do ASP.NET para um usuário autenticado ao usar a autenticação de formulários e a estrutura de funções](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Figura 1**: A eventos de Pipeline do ASP.NET para um autenticado quando usando formulários de autenticação de usuário e a estrutura de funções ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Armazenar em cache informações de função em um Cookie

O `RolePrincipal` do objeto `IsInRole(roleName)` chamadas de método `Roles.GetRolesForUser` para obter as funções para o usuário para determinar se o usuário é um membro da *roleName*. Ao usar o `SqlRoleProvider`, isso resulta em uma consulta no banco de dados do repositório de função. Ao usar regras de autorização de URL baseada em função do `RolePrincipal`do `IsInRole` método será chamado em cada solicitação para uma página que é protegida pelas regras de autorização de URL baseado em função. Em vez de ter que pesquisar as informações de função no banco de dados em cada solicitação, a estrutura de funções inclui uma opção para armazenar em cache as funções do usuário em um cookie.

Se a estrutura de funções é configurada para armazenar em cache as funções do usuário em um cookie, o `RoleManagerModule` cria o cookie durante o pipeline do ASP.NET [ `EndRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Esse cookie é usado em solicitações subsequentes na `PostAuthenticateRequest`, que é quando o `RolePrincipal` objeto é criado. Se o cookie for válido e não expirou, os dados no cookie são analisados e usados para preencher as funções do usuário, economizando a `RolePrincipal` precise fazer uma chamada para o `Roles` classe para determinar as funções do usuário. Figura 2 ilustra esse fluxo de trabalho.


[![Informações de função do usuário podem ser armazenadas em um Cookie para melhorar o desempenho](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Figura 2**: função de informações podem ser armazenadas do usuário para a em um Cookie para melhorar o desempenho ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image6.png))


Por padrão, o mecanismo de cookie do cache de função está desabilitado. Ele pode ser habilitado por meio de `<roleManager>` marcação de configuração em `Web.config`. Discutimos o uso de [ `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx) para especificar provedores de função no <a id="_msoanchor_4"> </a> [ *criando e gerenciando funções* ](creating-and-managing-roles-cs.md) tutorial, Portanto, você já deve ter esse elemento em seu aplicativo `Web.config` arquivo. As configurações de cookie do cache de função são especificadas como atributos do `<roleManager>` elemento e são resumidos na tabela 1.

> [!NOTE]
> As definições de configuração listadas na tabela 1 especificam as propriedades do cookie de cache de função resultante. Para obter mais informações sobre cookies, como elas funcionam e suas várias propriedades, leia [este tutorial de Cookies](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descrição</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Um valor booliano que indica se o cache de cookie é usado. Assume o padrão de `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     O nome do cookie de cache de função. O valor padrão é ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                O caminho para o cookie de nome de funções. O atributo de caminho permite que um desenvolvedor limitar o escopo de um cookie para uma hierarquia de diretório específico. O valor padrão é a barra "/", que informa ao navegador para enviar o cookie do tíquete de autenticação para qualquer solicitação feita ao domínio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica quais técnicas são usadas para proteger o cookie de cache de função. Os valores permitidos são: `All` (padrão); `Encryption`; `None`; e `Validation`. Voltar para a etapa 3 das <a id="_anchor_5"> </a> [ *configuração de autenticação de formulários e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) para obter mais informações sobre esses níveis de proteção.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Um valor booliano que indica se uma conexão SSL é necessária para transmitir o cookie de autenticação. O valor padrão é `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Um valor booliano que indica que se o tempo de limite do cookie é redefinido cada vez que o usuário visita o site durante uma única sessão. O valor padrão é `false`. Esse valor só é pertinente quando `createPersistentCookie` é definido como `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Especifica o tempo, em minutos, após o qual o cookie do tíquete de autenticação expira. O valor padrão é `30`. Esse valor só é pertinente quando `createPersistentCookie` é definido como `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Um valor booliano que especifica se o cookie do cache de função é um cookie de sessão ou um cookie persistente. Se `false` (padrão), um cookie de sessão for usado, qual é excluído quando o navegador é fechado. Se `true`, um cookie persistente é usado; ele expira `cookieTimeout` número de minutos após ele ter sido criado ou após a visita anterior, dependendo do valor de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Especifica o valor de domínio do cookie. O valor padrão é uma cadeia de caracteres vazia, o que faz com que o navegador para usar o domínio do qual ele foi emitido (como www.yourdomain.com). Nesse caso, o cookie será <strong>não</strong> ser enviada quando a tomada de solicitações para subdomínios, como admin.yourdomain.com. Se você quiser que o cookie a ser passado para todos os subdomínios, você precisará personalizar o `domain` atributo, configurando-a como "seudomínio.com".                                                                                                                                                 |
|    `maxCachedResults`     | Especifica o número máximo de nomes de função são armazenados em cache no cookie. O padrão é 25. O `RoleManagerModule` não cria um cookie para que os usuários que pertencem a mais de `maxCachedResults` funções. Consequentemente, o `RolePrincipal` do objeto `IsInRole` método usará o `Roles` classe para determinar as funções do usuário. O motivo pelo qual `maxCachedResults` existe porque muitos agentes de usuário não permitem que os cookies maiores que 4.096 bytes. Portanto, esse limite destina-se para reduzir a probabilidade de exceder esse limite de tamanho. Se você tiver nomes de função muito longas, você talvez queira considerar especificando uma menor `maxCachedResults` valor; contrariwise, se você tiver nomes de função extremamente curto, você provavelmente pode aumentar esse valor. |

**Tabela 1:** as opções de configuração de Cookie de Cache de função

Vamos configurar nosso aplicativo para usar cookies de cache de função não persistente. Para fazer isso, atualize o `<roleManager>` elemento no `Web.config` para incluir os seguintes atributos de cookie:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

Atualizei o `<roleManager>` elemento adicionando três atributos: `cacheRolesInCookie`, `createPersistentCookie`, e `cookieProtection`. Definindo `cacheRolesInCookie` à `true`, o `RoleManagerModule` agora será cache automaticamente as funções do usuário em um cookie em vez de precisar pesquisar informações de função do usuário em cada solicitação. Posso definir explicitamente o `createPersistentCookie` e `cookieProtection` atributos `false` e `All`, respectivamente. Tecnicamente, não preciso especificar valores para esses atributos, pois apenas atribuí-los para seus valores padrão, mas vou colocá-los aqui para torná-lo explicitamente limpar que eu não estou usando cookies persistentes e que o cookie é criptografado e validado.

Isso é tudo para ele! Daqui em diante, o framework de funções armazenará em cache as funções de usuários em cookies. Se o navegador do usuário não dá suporte a cookies, ou se os cookies são excluídos ou perdidos, de alguma forma, ele não é grande coisa – o `RolePrincipal` objeto simplesmente usará o `Roles` classe no caso de nenhum cookie (ou um inválido ou expirado) está disponível.

> [!NOTE]
> Padrões da Microsoft &amp; Practices group desencoraja o uso de cookies de cache de função persistente. Uma vez que a posse do cookie de cache de função é suficiente para comprovar a associação de função, se um hacker pode ter alguma forma o acesso ao cookie de um usuário válido ele pode representar o usuário. A probabilidade disso acontecer aumenta se o cookie é persistente no navegador do usuário. Para obter mais informações sobre essa recomendação de segurança, bem como outras preocupações de segurança, consulte o [lista de perguntas de segurança para o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Etapa 1: Definindo regras de autorização de URL baseada em função

Conforme discutido na <a id="_msoanchor_6"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial, a autorização de URL oferece um meio para restringir o acesso a um conjunto de páginas em um usuário por usuário ou função por função base. As regras de autorização de URL são indicadas claramente no `Web.config` usando o [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) com `<allow>` e `<deny>` elementos filho. Além das regras relacionadas ao usuário autorização discutidas nos tutoriais anteriores, cada `<allow>` e `<deny>` elemento filho também pode incluir:

- Uma função específica
- Uma lista delimitada por vírgulas de funções

Por exemplo, as regras de autorização de URL concedem acesso aos usuários nas funções de administradores e supervisores, mas negam acesso a todos os outros:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

O `<allow>` elemento na marcação acima indica que as funções de administradores e supervisores são permitidas; o `<deny>` instrui o elemento que *todos os* usuários serão negados.

Vamos configurar nosso aplicativo para que o `ManageRoles.aspx`, `UsersAndRoles.aspx`, e `CreateUserWizardWithRoles.aspx` páginas só são acessíveis aos usuários na função de administradores, enquanto o `RoleBasedAuthorization.aspx` página permanece acessível a todos os visitantes.

Para fazer isso, comece adicionando um `Web.config` o arquivo para o `Roles` pasta.


[![Adicionar um arquivo Web. config para o diretório de funções](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Figura 3**: adicionar um `Web.config` do arquivo para o `Roles` diretório ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image9.png))


Em seguida, adicione a seguinte marcação de configuração ao `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

O `<authorization>` elemento na `<system.web>` seção indica que somente os usuários na função de administradores podem acessar recursos do ASP.NET no `Roles` directory. O `<location>` elemento define um conjunto alternativo de regras de autorização de URL para o `RoleBasedAuthorization.aspx` página, permitindo que todos os usuários a visitar a página.

Depois de salvar suas alterações para `Web.config`, faça logon como um usuário que não está na função de administradores e, em seguida, tentar visitar uma das páginas protegidas. O `UrlAuthorizationModule` detectará que você não tem permissão para visitar o recurso solicitado; Consequentemente, o `FormsAuthenticationModule` redirecionará você para a página de logon. A página de logon, em seguida, você será encaminhado para o `UnauthorizedAccess.aspx` página (consulte a Figura 4). Esse redirecionamento final da página de logon para `UnauthorizedAccess.aspx` ocorre devido ao código que adicionamos à página de logon na etapa 2 do <a id="_msoanchor_7"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial. Em particular, a página de logon redireciona automaticamente qualquer usuário autenticado `UnauthorizedAccess.aspx` se a cadeia de consulta contém um `ReturnUrl` parâmetro, como esse parâmetro indica que o usuário acessou a página de logon depois de tentar exibir uma página em que ele não era autorizado a exibir.


[![Somente os usuários na função de administradores podem exibir as páginas protegidas](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Figura 4**: apenas os usuários à função administradores podem exibir as páginas protegidas ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image12.png))


Faça logoff e, em seguida, faça logon como um usuário que está na função de administradores. Agora você deve ser capaz de exibir três páginas protegidas.


[![Tito pode visitar que o UsersAndRoles.aspx página porque ele está na função de administradores](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Figura 5**: Tito pode visitar o `UsersAndRoles.aspx` página porque ele está na função de administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> Ao especificar regras de autorização de URL – para funções ou usuários – é importante ter em mente que as regras são analisados um por vez, de cima para baixo. Assim que uma correspondência for encontrada, o usuário é concedido ou negado acesso, dependendo se a correspondência foi encontrada em um `<allow>` ou `<deny>` elemento. **Se nenhuma correspondência for encontrada, o usuário recebe acesso.** Consequentemente, se você quiser restringir o acesso a uma ou mais contas de usuário, é imperativo que você use um `<deny>` como o último elemento na configuração de autorização de URL. **Se as regras de autorização de URL não incluir um**`<deny>`**elemento, todos os usuários terão acesso.** Para obter uma discussão mais completa sobre como as regras de autorização de URL são analisadas, consulte o "uma olhada em como o `UrlAuthorizationModule` usa as regras de autorização para conceder ou negar acesso" seção do <a id="_msoanchor_8"> </a> [  *Autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Etapa 2: Limitando a funcionalidade com base nas funções do usuário conectado no momento

Faz de autorização de URL fácil especificar o grosso autorização regras que o estado quais identidades têm permissão e quais delas são negadas de visualizar uma página específica (ou todas as páginas em uma pasta e suas subpastas). No entanto, em certos casos podemos querer permitir que todos os usuários visitam uma página, mas limitar a funcionalidade da página com base nas funções do usuário visitante. Isso pode envolver mostrando ou ocultando a dados com base na função do usuário ou oferecer funcionalidade adicional para os usuários que pertencem a uma função específica.

Essas regras de autorização baseada em função de alta granularidade podem ser implementadas de forma declarativa ou programaticamente (ou através de uma combinação dos dois). Na próxima seção, veremos como implementar a autorização declarativa refinado por meio do controle LoginView. Depois disso, iremos explorar técnicas de programação. Antes que possamos observar a aplicação de regras de autorização de alta granularidade, no entanto, primeiro precisamos criar uma página cuja funcionalidade depende da função do usuário visitá-lo.

Vamos criar uma página que lista todas as contas de usuário no sistema em um GridView. O GridView incluirá o nome de usuário, endereço de email, data do último logon e comentários sobre o usuário de cada usuário. Além de exibir informações de cada usuário, o GridView será incluem editar e excluir recursos. Criaremos inicialmente nessa página com o editar e excluir a funcionalidade disponível para todos os usuários. As seções "Usando o controle LoginView" e "Por meio de programação limitando a funcionalidade", veremos como habilitar ou desabilitar esses recursos com base na função do usuário visitante.

> [!NOTE]
> Estamos prestes a criar a página do ASP.NET usa um controle GridView para exibir as contas de usuário. Desde neste tutorial na série se concentra em formulários de autenticação, autorização, contas de usuário e funções, não quero dedicar muito tempo discutindo o funcionamento interno do controle GridView. Enquanto este tutorial fornece instruções passo a passo específicas para a configuração nesta página, ele não me aprofundar nos detalhes de por que determinadas escolhas feitas ou o que tem propriedades específicas do efeito na saída renderizada. Para obter uma análise minuciosa do controle GridView, confira minha *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais.


Comece abrindo o `RoleBasedAuthorization.aspx` página o `Roles` pasta. Arraste um GridView da página para o Designer e defina suas `ID` para `UserGrid`. Daqui a pouco vamos escrever código que chama o `Membership.GetAllUsers` método e associa resultante `MembershipUserCollection` objeto para o GridView. O `MembershipUserCollection` contém um `MembershipUser` objeto para cada conta de usuário no sistema; `MembershipUser` objetos têm propriedades como `UserName`, `Email`, `LastLoginDate`e assim por diante.

Antes que escrevemos o código que associa as contas de usuário para a grade, vamos primeiro definir os campos do GridView. Na marca inteligente do GridView, clique no link "Edit Columns" para iniciar a caixa de diálogo campos caixa (veja a Figura 6). A partir daqui, desmarque a caixa de seleção "gerar automaticamente campos" no canto inferior esquerdo. Como queremos que essa GridView para incluir a edição e exclusão de recursos, adicione um CommandField e defina suas `ShowEditButton` e `ShowDeleteButton` propriedades como True. Em seguida, adicione quatro campos para exibir o `UserName`, `Email`, `LastLoginDate`, e `Comment` propriedades. Usar um BoundField para as duas propriedades somente leitura (`UserName` e `LastLoginDate`) e TemplateFields nos dois campos editáveis (`Email` e `Comment`).

Ter a primeira exibição BoundField a `UserName` propriedade; defina seus `HeaderText` e `DataField` propriedades para "UserName". Este campo não podem ser editado, portanto, defina seu `ReadOnly` propriedade como True. Configurar o `LastLoginDate` BoundField definindo seu `HeaderText` para "Último logon" e seu `DataField` para "Data do último logon". Vamos formatar a saída desse BoundField para que apenas a data é exibida (em vez da data e hora). Para fazer isso, defina este BoundField `HtmlEncode` a propriedade como False e suas `DataFormatString` propriedade como "{0:d}". Defina também a `ReadOnly` propriedade como True.

Defina o `HeaderText` propriedades de as dois TemplateFields "Email" e "Comment".


[![Campos do GridView podem ser configurados por meio da caixa de diálogo de campos](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Figura 6**: campos pode ser configurado por meio da caixa do. O controle GridView de diálogo campos ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image18.png))


Agora, precisamos definir a `ItemTemplate` e `EditItemTemplate` para o "Email" e "Comentário" TemplateFields. Adicionar um controle de Web de rótulo para cada um dos `ItemTemplate` s e associar seus `Text` propriedades para o `Email` e `Comment` propriedades, respectivamente.

Para o TemplateField "Email", adicione uma caixa de texto denominada `Email` para seu `EditItemTemplate` e associar seu `Text` propriedade para o `Email` propriedade usando vinculação de dados bidirecional. Adicionar um RequiredFieldValidator e RegularExpressionValidator como o `EditItemTemplate` para garantir que um visitante editando a propriedade Email inseriu um endereço de email válido. Para o TemplateField "Comentário", adicione uma caixa de texto multilinha denominada `Comment` para seu `EditItemTemplate`. Defina a caixa de texto `Columns` e `Rows` as propriedades para 40 e 4, respectivamente e, em seguida, associar seu `Text` propriedade para o `Comment` propriedade usando vinculação de dados bidirecional.

Depois de configurar essas TemplateFields, sua marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Ao editar ou excluir uma conta de usuário, precisamos saber que o usuário `UserName` valor da propriedade. Defina o GridView `DataKeyNames` propriedade para "UserName" para que essas informações estão disponíveis por meio do GridView `DataKeys` coleção.

Por fim, adicione um controle ValidationSummary para a página e defina suas `ShowMessageBox` propriedade como True e sua `ShowSummary` propriedade como False. Com essas configurações, o ValidationSummary exibirá um alerta do lado do cliente, se o usuário tentar editar uma conta de usuário com um endereço de email inválido ou ausente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Agora concluímos marcação declarativa dessa página. Nossa próxima tarefa é associar o conjunto de contas de usuário para o GridView. Adicione um método chamado `BindUserGrid` para o `RoleBasedAuthorization.aspx` classe code-behind da página que vincula a `MembershipUserCollection` retornado por `Membership.GetAllUsers` para o `UserGrid` GridView. Chamar esse método a partir de `Page_Load` manipulador de eventos na primeira visita de página.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Com esse código, visite a página por meio de um navegador. Como mostra a Figura 7, você deverá ver um GridView listando as informações sobre cada conta de usuário no sistema.


[![O UserGrid GridView lista informações sobre cada usuário no sistema](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Figura 7**: O `UserGrid` GridView lista informações sobre cada usuário no sistema ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> O `UserGrid` GridView lista todos os usuários em uma interface não-paginável. Essa interface de grade simples não é adequado para cenários onde vários usuários dúzia ou mais. Uma opção é configurar o GridView para habilitar a paginação. O `Membership.GetAllUsers` método tem duas sobrecargas: uma que não aceita a nenhum parâmetro de entrada e retorna todos os usuários e outro que usa valores de inteiro para o índice da página e o tamanho de página e retorna somente o subconjunto especificado dos usuários. A segunda sobrecarga pode ser usado com mais eficiência uma página por meio dos usuários, pois ele retorna apenas o subconjunto exato das contas de usuário em vez de *todos os* deles. Se você tiver milhares de contas de usuário, você talvez queira considerar uma interface baseada em filtro, que mostra apenas os usuários cujo nome de usuário começa com um caractere selecionado, por exemplo. O [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) é ideal para a criação de uma interface do usuário baseada em filtros. Vamos examinar a criação de uma interface desse tipo em um tutorial futuro.


O controle GridView oferece edição e exclusão de suporte quando o controle é associado a um controle de fonte de dados configurados corretamente, como o SqlDataSource ou ObjectDataSource internos. O `UserGrid` GridView, no entanto, tem seus dados por meio de programação associados; portanto, precisamos escrever código para executar essas duas tarefas. Em particular, precisamos criar manipuladores de eventos para o GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, e `RowDeleting` eventos, que são acionados quando um visitante clica o GridView editar, cancelar, atualização, ou botões de exclusão.

Comece criando os manipuladores de eventos para o GridView `RowEditing`, `RowCancelingEdit`, e `RowUpdating` eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

O `RowEditing` e `RowCancelingEdit` manipuladores de eventos simplesmente definem o GridView `EditIndex` propriedade e, em seguida, reassociar a lista do usuário de contas à grade. O fato interessante acontece no `RowUpdating` manipulador de eventos. Esse manipulador de eventos é iniciado, garantindo que os dados é válidos e, em seguida, captura a `UserName` o valor da conta de usuário editada do `DataKeys` coleção. O `Email` e `Comment` caixas de texto em 'TemplateFields dois `EditItemTemplate` s, em seguida, são referenciados por meio de programação. Seus `Text` propriedades contêm o endereço de email editado e o comentário.

Para atualizar uma conta de usuário por meio da API de associação, precisamos primeiro obter as informações do usuário, que podemos fazer por meio de uma chamada para `Membership.GetUser(userName)`. Retornado `MembershipUser` do objeto `Email` e `Comment` propriedades, em seguida, são atualizadas com os valores inseridos nas duas caixas de texto da interface de edição. Por fim, essas modificações são salvos com uma chamada para [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). O `RowUpdating` manipulador de eventos é concluída por meio da reversão GridView à sua interface de edição previamente.

Em seguida, crie o `RowDeleting` manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

O manipulador de eventos acima começa captando a `UserName` valor a partir do GridView `DataKeys` coleção; isso `UserName` valor é então passado para a classe de associação [ `DeleteUser` método](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). O `DeleteUser` método exclui a conta de usuário do sistema, incluindo dados de associação relacionados (como as funções que este usuário pertence). Depois de excluir o usuário, a grade `EditIndex` é definido como -1 (no caso do usuário clicou em Excluir enquanto outra linha estava no modo de edição) e o `BindUserGrid` método é chamado.

> [!NOTE]
> O botão de exclusão não exige qualquer tipo de confirmação do usuário antes de excluir a conta de usuário. Eu recomendo que você adicionar alguma forma de confirmação do usuário para reduzir a chance de uma conta que está sendo excluída acidentalmente. Uma das maneiras mais fáceis para confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [' adicionando confirmação do lado do cliente quando excluindo](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Verifique se que essa página funciona conforme o esperado. Você deve ser capaz de editar o endereço de email de qualquer usuário e comentário, bem como excluir qualquer conta de usuário. Uma vez que o `RoleBasedAuthorization.aspx` está acessível a todos os usuários, qualquer usuário – visitantes anônimos mesmo – visite essa página e editar e excluir contas de usuário! Vamos atualizar esta página para que somente os usuários nas funções de supervisores e os administradores podem editar endereço de email do usuário e o comentário, e somente os administradores podem excluir uma conta de usuário.

A seção "Usando o controle LoginView" aborda usando o controle LoginView para mostrar as instruções específicas para a função do usuário. Se uma pessoa na função de administradores visitar essa página, mostraremos as instruções sobre como editar e excluir usuários. Se um usuário na função de supervisores atingir essa página, mostraremos as instruções sobre como editar os usuários. E se o visitante é anônimo ou não está na função de supervisores ou administradores, podemos exibirá uma mensagem explicando o que eles não é possível editar ou excluir informações de conta de usuário. Na seção "Programaticamente limitando a funcionalidade" Vamos escrever código que mostra ou oculta os botões Editar e excluir com base na função do usuário programaticamente.

### <a name="using-the-loginview-control"></a>Usar o controle LoginView

Como vimos nos tutoriais anteriores, o controle LoginView é útil para exibir as interfaces diferentes para usuários autenticados e anônimos, mas o controle LoginView também pode ser usado para exibir uma marcação diferente com base nas funções do usuário. Vamos usar um controle LoginView para exibir instruções diferentes com base na função do usuário visitante.

Comece adicionando um LoginView acima a `UserGrid` GridView. Como já discutimos anteriormente, o controle LoginView tem dois modelos internos: `AnonymousTemplate` e `LoggedInTemplate`. Insira uma breve mensagem em ambos esses modelos que informa ao usuário que não é possível editar ou excluir qualquer informação de usuário.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Além de `AnonymousTemplate` e `LoggedInTemplate`, o controle LoginView pode incluir *RoleGroups*, que são modelos específicos de função. Cada RoleGroup contém uma única propriedade, `Roles`, que especifica as funções que o RoleGroup aplica-se a. O `Roles` propriedade pode ser definida para uma única função (como "administradores") ou uma lista delimitada por vírgulas de funções (como "administradores, os supervisores").

Para gerenciar o RoleGroups, clique no link "Editar RoleGroups" da marca inteligente do controle para transferir anterior o Editor de coleção de RoleGroup. Adicione dois RoleGroups novo. Defina o RoleGroup de primeira `Roles` propriedade como "Administradores" e a segunda para "Supervisores".


[![Gerenciar modelos de específicas de função do LoginView através do Editor de coleção de RoleGroup.](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Figura 8**: gerenciar específicas de função modelos por meio do Editor do LoginView Kolekce RoleGroup. ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image24.png))


Clique em Okey para fechar o Editor de coleção de RoleGroup; Isso atualiza a marcação declarativa do LoginView para incluir um `<RoleGroups>` seção com um `<asp:RoleGroup>` elemento filho para cada RoleGroup definido no Editor de coleção de RoleGroup. Além disso, na lista suspensa "Exibições" Listar na marca inteligente do LoginView - que inicialmente listadas apenas o `AnonymousTemplate` e `LoggedInTemplate` – agora inclui também o RoleGroups adicionado.

Edite o RoleGroups para que os usuários na função de supervisores exibidas instruções sobre como editar contas de usuário, enquanto os usuários na função de administradores são mostrados as instruções para edição e exclusão. Depois de fazer essas alterações, marcação declarativa do seu LoginView deve ser semelhante ao seguinte.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Depois de fazer essas alterações, salve a página e, em seguida, visite-o por meio de um navegador. Primeiro, visite a página como um usuário anônimo. Você deve ser exibida a mensagem, "você não está conectado ao sistema. Portanto, é possível editar ou excluir qualquer informação do usuário." Em seguida, faça logon como um usuário autenticado, mas que não é nem a função supervisores nem os administradores. Neste momento, você deverá ver a mensagem "você não é um membro das funções de supervisores ou administradores. Portanto, é possível editar ou excluir qualquer informação do usuário."

Em seguida, faça logon como um usuário que seja um membro da função de supervisores. Neste momento, você deve ver os supervisores específicas de função da mensagem (veja a Figura 9). E se você fazer logon como um usuário em administradores de função, você deve ver os administradores de função específicos de mensagem (consulte a Figura 10).


[![Bruce é mostrado a mensagem de função específica de supervisores](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Figura 9**: Bruce é mostrado a mensagem de função específica de supervisores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image27.png))


[![Tito é mostrada a mensagem específicas de função de administradores](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Figura 10**: Tito é mostrada a mensagem específicas de função de administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image30.png))


Como capturas de tela figuras 9 e 10 mostram, o LoginView processa apenas um modelo, mesmo se aplicam vários modelos. Bruce e Tito são registradas em usuários, ainda que o LoginView renderiza apenas o RoleGroup correspondente e não o `LoggedInTemplate`. Além disso, Tito pertence às funções de administradores e supervisores, ainda que o controle LoginView renderiza o modelo de função específica de administradores em vez de supervisores um.

Figura 11 ilustra o fluxo de trabalho usado pelo controle LoginView para determinar qual modelo para renderizar. Observe que se houver mais de um RoleGroup especificado, o modelo de LoginView renderiza os *primeiro* RoleGroup corresponde. Em outras palavras, se tinha colocamos o RoleGroup supervisores, como o primeiro RoleGroup e os administradores como o segundo, em seguida, quando Tito visita essa página ele veria a mensagem de supervisores.


[![Fluxo de trabalho do controle LoginView para determinar qual modelo de renderização](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Figura 11**: fluxo de trabalho do controle LoginView o para determinar qual modelo de renderização ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitando a funcionalidade de forma programática

Enquanto o controle LoginView exibe instruções diferentes com base na função do usuário visitar a página, os botões Editar e Cancelar permanecem visíveis para todos. É necessário ocultar os botões Editar e excluir para visitantes anônimos e os usuários que estão na função de supervisores nem os administradores de forma programática. É necessário ocultar o botão Excluir para todas as pessoas que não seja um administrador. Para conseguir isso vamos escrever um pouco de código que referencia programaticamente o CommandField editar e excluir LinkButtons e conjuntos de suas `Visible` propriedades a serem `false`, se necessário.

A maneira mais fácil para fazer referência a controles em um CommandField programaticamente é primeiro convertê-lo em um modelo. Para fazer isso, clique no link "Edit Columns" na marca inteligente do GridView, selecione o CommandField na lista de campos atuais e clique no link "Converter este campo em um TemplateField". Isso transforma o CommandField em um TemplateField com um `ItemTemplate` e `EditItemTemplate`. O `ItemTemplate` contém o editar e excluir LinkButtons enquanto o `EditItemTemplate` abriga a atualização e botões de link Cancelar.


[![Converter o CommandField em um TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Figura 12**: converter o CommandField em um TemplateField ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image36.png))


Atualizar o editar e excluir LinkButtons na `ItemTemplate`, definindo suas `ID` propriedades para valores de `EditButton` e `DeleteButton`, respectivamente.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Sempre que dados forem vinculados à GridView, GridView enumera os registros no seu `DataSource` propriedade e gera um correspondente `GridViewRow` objeto. Como cada `GridViewRow` objeto é criado, o `RowCreated` evento é disparado. Para ocultar os botões Editar e excluir usuários não-autorizados, precisamos criar um manipulador de eventos para esse evento e referenciar programaticamente o editar e excluir LinkButtons, definindo suas `Visible` propriedades adequadamente.

Criar um manipulador de eventos a `RowCreated` eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Tenha em mente que o `RowCreated` evento é acionado para *todos os* das linhas GridView, incluindo o cabeçalho, rodapé, a interface de paginação e assim por diante. Queremos apenas referenciar programaticamente o editar e excluir LinkButtons se estivermos lidando com uma linha de dados não está no modo de edição (uma vez que a linha no modo de edição tem os botões de atualização e Cancelar em vez de editar e excluir). Essa verificação é tratada pelo `if` instrução.

Se estivermos lidando com uma linha de dados que não está no modo de edição, o editar e excluir LinkButtons são referenciados e seus `Visible` propriedades são definidas com base nos valores boolianos retornados pelo `User` do objeto `IsInRole(roleName)` método. O objeto de usuário faz referência a entidade criada pelo `RoleManagerModule`; Consequentemente, o `IsInRole(roleName)` método usa a API de funções para determinar se o visitante atual pertence à *roleName*.

> [!NOTE]
> Poderíamos ter usado a classe de funções diretamente, substituindo a chamada para `User.IsInRole(roleName)` com uma chamada para o [ `Roles.IsUserInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Decidi usar o objeto de entidade `IsInRole(roleName)` método neste exemplo porque é mais eficiente do que usar a API de funções diretamente. Neste tutorial, configuramos o Gerenciador de funções para armazenar em cache as funções do usuário em um cookie. Esse cookie dados armazenados em cache é utilizado apenas quando a entidade de segurança `IsInRole(roleName)` é chamado de método; chamadas diretas para a API de funções sempre envolvem uma viagem para o repositório da função. Mesmo se as funções não são armazenados em cache em um cookie, chamar o objeto de entidade `IsInRole(roleName)` método é geralmente mais eficiente, porque quando ele é chamado na primeira vez durante uma solicitação armazena em cache os resultados. A API de funções, por outro lado, não execute nenhum cache. Porque o `RowCreated` evento é acionado uma vez para cada linha de GridView, usando `User.IsInRole(roleName)` envolve apenas uma viagem para o repositório da função, enquanto `Roles.IsUserInRole(roleName)` requer *N* viagens, onde *N* é o número de contas de usuário exibidas na grade.


O botão de edição `Visible` estiver definida como `true` se o usuário ao visitar essa página está na função de administradores ou os supervisores; caso contrário, ele será definido como `false`. O botão de exclusão `Visible` estiver definida como `true` somente se o usuário está na função de administradores.

Teste esta página por meio de um navegador. Se você visitar a página como um visitante anônimo ou como um usuário que não é um Supervisor nem um administrador, o CommandField está vazio. ele ainda existe, mas como uma thin prata sem de editar ou excluir botões.

> [!NOTE]
> É possível ocultar o CommandField completamente quando um não-Supervisor e não-administrador está visitando a página. Posso deixar isso como um exercício para o leitor.


[![Os botões Editar e excluir são ocultados para não supervisores e não-administradores](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Figura 13**: O editar e excluir botões estão ocultos para não supervisores e não-administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image39.png))


Se um usuário que pertence à função de supervisores (mas não à função de administradores) visita, ele vê apenas o botão Editar.


[![Enquanto o botão Editar está disponível para os supervisores, o botão Excluir está oculto](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Figura 14**: enquanto o botão Editar está disponível para os supervisores, o botão Excluir é ocultado ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image42.png))


E se a visita de um administrador, ela tem acesso a ambos os botões Editar e excluir.


[![Os botões Editar e excluir estão disponíveis somente para administradores](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Figura 15**: O editar e excluir botões estão disponíveis somente para administradores ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Etapa 3: Aplicando regras de autorização baseada em função para Classes e métodos

Na etapa 2, limitamos editar recursos para os usuários nas funções de supervisores e administradores e excluir recursos para administradores. Isso foi feito, ocultando os elementos da interface de usuário associada para usuários não autorizados por meio de técnicas de programação. Essas medidas não garantem que um usuário não autorizado não poderá executar uma ação com privilégios. Pode haver elementos de interface do usuário que forem adicionados posteriormente ou que esquecemos de ocultar para usuários não autorizados. Ou, um hacker pode descobrir uma outra maneira de obter a página ASP.NET para executar o método desejado.

Uma maneira fácil de garantir que uma parte específica da funcionalidade não pode ser acessada por um usuário não autorizado é decorar a classe ou método com o [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando o tempo de execução do .NET usa uma classe ou executa um de seus métodos, ele verifica para garantir que o contexto de segurança atual tem permissão. O `PrincipalPermission` atributo fornece um mecanismo por meio do qual podemos definir essas regras.

Analisamos usando o `PrincipalPermission` atributo volta a <a id="_msoanchor_9"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial. Especificamente, vimos como decorar o GridView `SelectedIndexChanged` e `RowDeleting` manipulador de eventos para que eles só podem ser executados por usuários autenticados e Tito, respectivamente. O `PrincipalPermission` atributo funciona tão bem com as funções.

Vamos demonstrar o uso de `PrincipalPermission` atributo do GridView `RowUpdating` e `RowDeleting` manipuladores de eventos para proibir a execução para que os usuários não autorizados. Tudo o que precisamos fazer é adicionar o atributo apropriado por cima de cada definição de função:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

O atributo para o `RowUpdating` manipulador de eventos que dita que somente os usuários nas funções de administradores ou os supervisores podem executar o manipulador de eventos, enquanto que o atributo no `RowDeleting` manipulador de eventos limita a execução para usuários em que os administradores função.


> [!NOTE]
> O `PrincipalPermission` atributo é representado como uma classe no `System.Security.Permissions` namespace. Certifique-se de adicionar um `using System.Security.Permissions` instrução na parte superior do seu arquivo de classe code-behind para importar esse namespace.


Se, de alguma forma, um não administrador tenta executar o `RowDeleting` manipulador de eventos ou se um não-Supervisor ou não-administrador tenta executar o `RowUpdating` manipulador de eventos, o tempo de execução do .NET irá gerar um `SecurityException`.


[![Se o contexto de segurança não está autorizado a executar o método, um SecurityException é gerado](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Figura 16**: se o contexto de segurança não está autorizado a executar o método, uma `SecurityException` é gerada ([clique para exibir a imagem em tamanho normal](role-based-authorization-cs/_static/image48.png))


Além das páginas ASP.NET, muitos aplicativos também têm uma arquitetura que inclui várias camadas, como lógica de negócios e as camadas de acesso de dados. Essas camadas normalmente são implementadas como bibliotecas de classes e classes e métodos para executar a funcionalidade de relacionadas a dados e lógica de negócios da oferta. O `PrincipalPermission` atributo é útil para aplicar as regras de autorização para essas camadas também.

Para obter mais informações sobre como usar o `PrincipalPermission` atributo para definir regras de autorização em classes e métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog [adicionando regras de autorização para os negócios e camadas de dados usando `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumo

Neste tutorial vimos como especificar grosso e regras de autorização de alta granularidade com base nas funções do usuário. ASP. Recurso de autorização de URL da rede permite que um desenvolvedor de página especificar quais identidades têm permitidas ou negadas acesso a quais páginas. Como vimos na <a id="_msoanchor_10"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial, autorização de URL regras podem ser aplicadas em uma base por usuário. Elas também podem ser aplicadas em uma base de função por função, como vimos na etapa 1 deste tutorial.

As regras de autorização de alta granularidade podem ser aplicadas declarativamente ou programaticamente. Na etapa 2, analisamos usando o recurso de RoleGroups do controle LoginView para renderizar a saída diferente com base nas funções do usuário visitante. Também vimos maneiras de determinar programaticamente se um usuário pertence a uma função específica e como ajustar a funcionalidade da página adequadamente.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Adicionando regras de autorização para os negócios e camadas de dados usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examinando o ASP.NET da 2.0 associação, funções e perfil: Trabalhando com funções](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista de perguntas de segurança para o ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentação técnica para o `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial incluem Suchi Banerjee e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](assigning-roles-to-users-cs.md)
> [Próximo](creating-and-managing-roles-vb.md)
