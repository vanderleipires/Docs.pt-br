---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteger aplicativos usando a autenticação e autorização | Microsoft Docs
author: microsoft
description: Etapa 9 mostra como adicionar autenticação e autorização para proteger o nosso aplicativo NerdDinner, para que os usuários precisam registrar e faça logon no site para criar...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 0005b99dbf7d59e96313f025880c46cdec4838b6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801386"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Proteger aplicativos usando a autenticação e autorização
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 9 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 9 mostra como adicionar autenticação e autorização para proteger o nosso aplicativo NerdDinner, para que os usuários precisam registrar e faça logon no site para criar novo jantares e somente o usuário que está hospedando um jantar pode editá-lo mais tarde.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner etapa 9: Autenticação e autorização

No momento a nosso NerdDinner aplicativo concede a qualquer pessoa que visitar o site a capacidade de criar e editar os detalhes de qualquer jantar. Vamos alterar isso para que os usuários precisam registrar e faça logon no site para criar novo jantares e adicionar uma restrição de modo que somente o usuário que está hospedando um jantar pode editá-lo mais tarde.

Para habilitar isso, usaremos a autenticação e autorização para proteger o nosso aplicativo.

### <a name="understanding-authentication-and-authorization"></a>Noções básicas sobre autenticação e autorização

*Autenticação* é o processo de identificar e validar a identidade de um cliente acessando um aplicativo. Em termos mais simples, é sobre como identificar "quem é o usuário final é quando eles visitam um site da Web". ASP.NET oferece suporte a várias maneiras de autenticar os usuários do navegador. Para aplicativos da web da Internet, a abordagem mais comum de autenticação usada é chamada de "Autenticação de formulários". Autenticação de formulários permite que um desenvolvedor criar um formulário de logon HTML dentro de seu aplicativo e, em seguida, validar o nome de usuário/senha de que um usuário final envia em relação a um banco de dados ou outro armazenamento de credenciais de senha. Se a combinação de nome de usuário/senha estiver correta, o desenvolvedor pode, em seguida, peça ao ASP.NET para emitir um cookie HTTP criptografado para identificar o usuário entre as solicitações futuras. Vamos usar a autenticação de formulários com nosso aplicativo NerdDinner.

*Autorização* é o processo de determinar se um usuário autenticado tem permissão para acessar um recurso/URL específico ou para realizar alguma ação. Por exemplo, dentro do nosso aplicativo NerdDinner, desejaremos autorizar que somente os usuários que estão conectados podem acessar o *jantares/criar* URL e criar novos jantares. Podemos também desejará adicionar lógica de autorização para que somente o usuário que está hospedando um jantar possa editá-lo – e negar acesso de edição para todos os outros usuários.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticação de formulários e a AccountController

O modelo de projeto do Visual Studio padrão para o ASP.NET MVC automaticamente habilita a autenticação de formulários quando novos aplicativos ASP.NET MVC são criados. Ele também adicionará automaticamente uma implementação de página de logon de conta criado previamente para o projeto – o que torna muito fácil integrar a segurança dentro de um site.

Página mestra padrão Master exibe um link "Fazer logon" na parte superior direita do site quando o usuário acessá-los não for autenticado:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Ao clicar no link "Fazer logon" leva a um usuário para o */conta/LogOn* URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Os visitantes que ainda não registrou podem fazer isso clicando no link "Registrar" – que o levará para o */conta/registro* URL e permitir que eles Insira detalhes da conta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Clicar no botão "Registrar" criar um novo usuário no sistema de associação do ASP.NET e autenticar o usuário no site usando a autenticação de formulários.

Quando um usuário estiver conectado, o site altera o canto superior direito da página para gerar um "Bem-vindo [username]!" mensagem e processa um "fazer logoff" link em vez de um "fazer logon" um. Ao clicar no link "Fazer logoff" faz logoff do usuário:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

A funcionalidade de login, logout e registro acima é implementada na classe AccountController que foi adicionada ao nosso projeto pelo Visual Studio quando ele criou o projeto. A interface do usuário para o AccountController é implementado usando modelos de exibição dentro do diretório \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

A classe AccountController usa o sistema de autenticação de formulários ASP.NET para emitir cookies de autenticação criptografado e a API Membership do ASP.NET para armazenar e validar os nomes de usuário/senhas. A API Membership do ASP.NET é extensível e permite que qualquer armazenamento de credenciais de senha a ser usado. ASP.NET é fornecido com implementações do provedor de associação interna que armazena o nome de usuário/senha dentro de um banco de dados SQL ou dentro do Active Directory.

Podemos configurar qual provedor de associação que nosso aplicativo NerdDinner deve usar abrindo o arquivo Web. config"na raiz do projeto e procurando o &lt;associação&gt; seção dentro dele. Registra o provedor de associação SQL Web. config padrão adicionada quando o projeto foi criado e configura-o para usar uma cadeia de conexão nomeada "ApplicationServices" para especificar o local do banco de dados.

A cadeia de conexão padrão "ApplicationServices" (que é especificado dentro de &lt;connectionStrings&gt; seção do arquivo Web. config) está configurado para usar o SQL Express. Ele aponta para um banco de dados do SQL Express chamado "ASPNETDB. Arquivos MDF"sob o aplicativo" aplicativo\_dados "directory. Se esse banco de dados não existir na primeira vez em que a API de associação é usada dentro do aplicativo, o ASP.NET será criar o banco de dados automaticamente e provisione o esquema de banco de dados de associação apropriado dentro dele:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se, em vez de usar o SQL Express queríamos usar uma instância completa do SQL Server (ou se conectar a um banco de dados remoto), tudo o que seria necessário tarefas pendentes é atualizar a cadeia de conexão "ApplicationServices" dentro do arquivo Web. config e certifique-se de que o esquema de associação apropriado foi adicionado ao banco de dados que aponta para ele. Você pode executar o "aspnet\_regsql.exe" utilitário dentro do diretório \Windows\Microsoft.NET\Framework\v2.0.50727\ para adicionar o esquema apropriado para a associação e outros serviços do aplicativo ASP.NET a um banco de dados.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorização de URL jantares/criar usando o filtro [Authorize]

Não temos de escrever nenhum código para habilitar uma autenticação segura e a implementação de gerenciamento de conta para o aplicativo NerdDinner. Os usuários podem registrar novas contas com nosso aplicativo e logon/logoff do site.

Agora podemos adicionar lógica de autorização para o aplicativo e usar o status de autenticação e o nome de usuário de visitantes para controlar o que eles podem e não podem fazer dentro do site. Vamos começar pela adição de lógica de autorização para os métodos de ação de "Criar" da nossa classe DinnersController. Especificamente, será exigido que os usuários que acessam o *jantares/criar* URL deve estar conectado. Se eles não são registrados nós será redirecioná-las para a página de logon para que eles possam entrar no.

Implementar essa lógica é muito fácil. Tudo que precisamos de tarefas pendentes é adicionar um atributo de filtro [Authorize] para nossos métodos de ação de criar da seguinte forma:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC suporta a capacidade de criar "filtros de ação" que podem ser usados para implementar a lógica reutilizável que pode ser aplicada declarativamente aos métodos de ação. O filtro [Authorize] é um dos filtros de ação integrada fornecidos pelo ASP.NET MVC, e ele permite que um desenvolvedor declarativamente aplicar regras de autorização para métodos de ação e as classes do controlador.

Quando aplicado sem parâmetros (como acima) o filtro [Authorize] impõe que o usuário que fez a solicitação do método de ação deve estar conectado – e ele será redirecionado automaticamente o navegador para a URL de logon se eles não forem. Ao realizar esse redirecionamento, a URL solicitada originalmente é passada como um argumento de cadeia de consulta (por exemplo: / conta/LogOn? ReturnUrl = % 2fDinners % 2fCreate). O AccountController, em seguida, redirecione o usuário para a URL solicitada originalmente depois que ele faça logon.

O filtro [Authorize] ofereça suporte opcionalmente a capacidade de especificar uma propriedade "Usuários" ou "Funções" que pode ser usada para exigir que o usuário ambos registrado no e dentro de uma lista de usuários permitidos ou um membro de uma função de segurança permitidos. Por exemplo, o código a seguir somente permite que dois usuários específicos, "scottgu" e "billg" acessar a URL de jantares/criar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Inserir nomes de usuário específicos dentro do código tende a ser muito não passível de manutenção no entanto. Uma abordagem melhor é definir "funções" de nível mais altos que o código verifica contra e, então, mapear os usuários na função usando um banco de dados ou o sistema do Active Directory (habilitando a lista de mapeamento de usuário real a ser armazenada externamente do código). O ASP.NET inclui uma API de gerenciamento de função interna, bem como um conjunto interno de provedores de função (incluindo aqueles para SQL e Active Directory) que podem ajudar a realizar esse mapeamento de função/usuário. Em seguida, podemos foi possível atualizar o código para permitir que somente usuários dentro de uma função específica "admin" acessar a URL de jantares/criar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Usando a propriedade User.Identity.Name durante a criação de jantares

Podemos recuperar o nome de usuário do usuário conectado no momento de uma solicitação usando a propriedade User.Identity.Name exposta na classe base do controlador.

Anteriores quando implementamos a versão do HTTP POST do nosso método de ação Create () tínhamos codificado a propriedade "HostedBy" de jantar para uma cadeia de caracteres estática. Podemos pode agora atualizar esse código em vez de usar a propriedade User.Identity.Name, bem como adicionar automaticamente uma confirmação de presença para o host criando o jantar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Como adicionamos um atributo [autorizar] ao método Create (), ASP.NET MVC garante que o método de ação é executada apenas se o usuário visitar a URL de jantares/criar estiver conectado no site. Como tal, o valor da propriedade User.Identity.Name sempre conterá um nome de usuário válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Usando a propriedade User.Identity.Name ao editar jantares

Agora, vamos adicionar alguma lógica de autorização que restringe os usuários para que eles só podem editar as propriedades de jantares que eles mesmos estão hospedando.

Para ajudar com isso, vamos primeiro adicionar um método auxiliar de "IsHostedBy(username)" ao nosso objeto de jantar (dentro da classe parcial do Dinner.cs que criamos anteriormente). Esse método auxiliar retorna true ou false dependendo se um nome de usuário fornecido corresponde à propriedade HostedBy jantar e encapsula a lógica necessária executar uma comparação de cadeia de caracteres de maiusculas e minúsculas delas:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Em seguida, adicionaremos um atributo [autorizar] para os métodos de ação Edit () dentro de nossa classe DinnersController. Isso garantirá que os usuários devem estar conectados a solicitação de um */Dinners/Editar / [id]* URL.

Em seguida, podemos adicionar código para nossos métodos de edição que usa o método auxiliar Dinner.IsHostedBy(username) para verificar se o usuário conectado corresponde ao host do jantar. Se o usuário não é o host, vamos mostrar uma exibição de "InvalidOwner" e encerrar a solicitação. O código para fazer isso se parece com abaixo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Podemos, em seguida, clique com botão direito no diretório \Views\Dinners e escolha Add -&gt;exibir comando de menu para criar uma nova exibição de "InvalidOwner". Podemos irá preenchê-lo com a mensagem de erro abaixo:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

E agora quando um usuário de tenta editar um jantar não possuem, ele obterá uma mensagem de erro:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Podemos pode repetir as mesmas etapas para os métodos de ação Delete () dentro do nosso controlador para bloquear a permissão para excluir os jantares também e, em seguida, certifique-se de que somente o host de um jantar poderá excluí-lo.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrar/ocultar editar e excluir Links

Podemos está vinculando o método de ação de edição e exclusão de nossa classe DinnersController da nossa URL de detalhes:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

No momento, estamos mostrando os links de ação de editar e excluir, independentemente se o visitante para a URL de detalhes é o host do jantar. Vamos alterar isso para que os links serão exibidos somente se o usuário visitante é o proprietário do jantar.

O método de ação Details() dentro do nosso DinnersController recupera um objeto de jantar e, em seguida, passa-lo como o objeto de modelo para nosso modelo de exibição:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos atualizar nosso modelo de exibição para condicionalmente Mostrar/ocultar os links Edit e Delete usando o Dinner.IsHostedBy() método auxiliar como abaixo:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Próximas etapas

Agora vejamos como podemos pode permitir que usuários autenticados RSVP para jantares usando AJAX.

> [!div class="step-by-step"]
> [Anterior](implement-efficient-data-paging.md)
> [Próximo](use-ajax-to-deliver-dynamic-updates.md)
