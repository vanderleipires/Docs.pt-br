---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "Proteger aplicativos usando a autenticação e autorização | Microsoft Docs"
author: microsoft
description: "Etapa 9 mostra como adicionar autenticação e autorização para proteger o nosso aplicativo NerdDinner, para que os usuários precisam registrar e faça logon no site para criar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>Proteger aplicativos usando a autenticação e autorização
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 9 da livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 9 mostra como adicionar autenticação e autorização para proteger o nosso aplicativo NerdDinner, para que os usuários precisam registrar e faça logon no site para criar novo jantares e somente o usuário que está hospedando uma refeição pode editá-lo mais tarde.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner etapa 9: Autenticação e autorização

Agora o nosso NerdDinner aplicativo concede a qualquer pessoa visitando o site a capacidade de criar e editar os detalhes de qualquer refeição. Vamos alterar isso para que os usuários precisam registrar e faça logon no site para criar novo jantares e adicionar uma restrição de forma que apenas o usuário que está hospedando uma refeição pode editá-lo mais tarde.

Para habilitar isso, que vamos usar autenticação e autorização para proteger o nosso aplicativo.

### <a name="understanding-authentication-and-authorization"></a>Noções básicas sobre autenticação e autorização

*Autenticação* é o processo de identificar e validar a identidade de um cliente de acesso a um aplicativo. Simplesmente é sobre como identificar "quem é o usuário final é que visitam um site". ASP.NET oferece várias maneiras para autenticar os usuários do navegador. Para aplicativos da web da Internet, a abordagem mais comum de autenticação usada é chamada "Autenticação de formulários". Autenticação de formulários permite que um desenvolvedor criar um formulário de logon HTML em seus aplicativos e, em seguida, validar a nome de usuário/senha que envia um usuário final em um banco de dados ou outro repositório de credenciais de senha. Se a combinação de nome de usuário e senha estiver correta, o desenvolvedor pode, em seguida, peça ao ASP.NET para emitir um cookie HTTP criptografado para identificar o usuário em solicitações futuras. Vamos enviar uma mensagem usando a autenticação de formulários com nosso aplicativo NerdDinner.

*Autorização* é o processo de determinar se um usuário autenticado tem permissão para acessar um recurso/URL específico ou executar alguma ação. Por exemplo, em nosso aplicativo NerdDinner queremos autorizar que apenas os usuários que estão conectados podem acessar o *jantares/criar* URL e criar novos jantares. Podemos também vai querer adicionar lógica de autorização para que somente o usuário que está hospedando uma refeição possa editá-lo – e negar o acesso de edição para todos os outros usuários.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticação de formulários e a AccountController

O modelo de projeto do Visual Studio padrão do ASP.NET MVC automaticamente habilita a autenticação de formulários quando são criados novos aplicativos ASP.NET MVC. Ele também adicionará automaticamente uma implementação de página de logon de conta pré-criada para o projeto – o que realmente facilita a integração de segurança dentro de um site.

Página mestra padrão Site.master exibe um link de "Logon" na parte superior direita do site quando o usuário acessando não for autenticado:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Clique no link "Logon" leva a um usuário para o *conta/LogOn* URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Os visitantes que ainda não registrou podem fazer isso clicando no link "Register" – que o levará para a *conta/registro* URL e permitir que eles insira os detalhes da conta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Clicar no botão "Registrar" criar um novo usuário no sistema de associação do ASP.NET e autenticar o usuário para o site usando a autenticação de formulários.

Quando um usuário estiver conectado, o Site.master altera o canto superior direito da página para gerar um "Bem-vindo [username]!" mensagem e processa um "fazer logoff" link em vez de um "logon" um. Clique no link "Fazer logoff" faz logoff do usuário:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

A funcionalidade de logon, logout e registro acima é implementada na classe AccountController que foi adicionada ao nosso projeto pelo Visual Studio quando ele criou o projeto. A interface do usuário para o AccountController é implementado usando modelos de exibição dentro do diretório \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

A classe AccountController usa o sistema de autenticação de formulários do ASP.NET para emitir cookies de autenticação criptografada e a API de associação do ASP.NET para armazenar e validar nomes de usuário/senha. A API de associação do ASP.NET é extensível e permite que qualquer armazenamento de credenciais de senha a ser usada. ASP.NET é fornecido com implementações do provedor de associação interna que armazena o nome de usuário/senha em um banco de dados SQL ou no Active Directory.

É possível configurar o provedor de associação deve usar nosso aplicativo NerdDinner abrindo o arquivo Web. config"na raiz do projeto e procurando o &lt;associação&gt; seção dentro dele. O Web. config padrão adicionada quando o projeto foi criado registra o provedor de associação do SQL e o configura para usar uma cadeia de conexão denominada "ApplicationServices" para especificar o local do banco de dados.

A cadeia de conexão padrão "ApplicationServices" (que é especificado dentro de &lt;connectionStrings&gt; seção do arquivo Web. config) está configurado para usar o SQL Express. Aponta para um banco de dados do SQL Express denominado "ASPNETDB. MDF"sob o aplicativo" aplicativo\_dados "diretório. Se esse banco de dados não existe na primeira vez que a API de associação é usada dentro do aplicativo, o ASP.NET automaticamente cria o banco de dados e provisionar o esquema de banco de dados de associação apropriado dentro dele:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se, em vez de usar o SQL Express queremos usar uma instância completa do SQL Server (ou se conectar a um banco de dados remoto), tudo o que seria necessário tarefas é atualizar a cadeia de conexão "ApplicationServices" dentro do arquivo Web. config e certifique-se de que o esquema de associação apropriado foi adicionado ao banco de dados que ele aponta. Você pode executar o "aspnet\_regsql.exe" utilitário dentro do diretório \Windows\Microsoft.NET\Framework\v2.0.50727\ para adicionar o esquema apropriado para a associação e os outros serviços do aplicativo ASP.NET a um banco de dados.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorização de URL jantares/criar usando o filtro [autorizar]

Não temos gravar qualquer código para habilitar uma autenticação segura e a implementação de gerenciamento de conta para o aplicativo NerdDinner. Os usuários podem registrar novas contas com nosso aplicativo e o logon/logoff do site.

Agora podemos adicionar lógica de autorização para o aplicativo e usar o status de autenticação e o nome de usuário de visitantes para controlar o que podem ou não é possível fazer dentro do site. Vamos começar com a adição de lógica de autorização para os métodos de ação de nossa classe DinnersController "Criar". Especificamente, podemos exigirá que os usuários que acessam o *jantares/criar* URL deve estar conectada. Se não estiver conectados, será redirecioná-los para a página de logon para que eles podem entrar.

Implementar essa lógica é bem fácil. Tudo o que precisamos fazer é adicionar um atributo de filtro [autorizar] ao nosso crie métodos de ação da seguinte forma:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC suporta a capacidade de criar "filtros de ação" que podem ser usados para implementar a lógica reutilizável que pode ser aplicada de forma declarativa para métodos de ação. O filtro [autorizar] é um dos filtros de ação interna fornecidos pelo ASP.NET MVC, e ele permite que um desenvolvedor declarativamente aplicar regras de autorização para os métodos de ação e classes do controlador.

Quando aplicado sem parâmetros (como acima) o filtro [autorizar] impõe que o usuário que fez a solicitação do método de ação deve ser conectado – e ela será automaticamente redirecionada do navegador para a URL de logon se eles não são. Ao realizar esse redirecionamento a URL solicitada originalmente é passada como um argumento de querystring (por exemplo: / conta/LogOn? ReturnUrl = % 2fDinners % 2fCreate). O AccountController redirecionará o usuário, em seguida, volta para a URL solicitada originalmente depois que eles logon.

O filtro [autorizar] ofereça suporte opcionalmente a capacidade de especificar uma propriedade "Usuários" ou "Funções" que pode ser usada para exigir que o usuário é efetuado em e em uma lista de usuários permitidos ou um membro de uma função de segurança permitidos. Por exemplo, o código a seguir somente permite que dois usuários específicos, "scottgu" e "billg" acessar a URL de jantares/criar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Inserir nomes de usuário específico no código tende a ser muito não sustentável embora. Uma abordagem melhor é definir o nível mais alto "funções" que verifica o código e, em seguida, mapear os usuários para a função usando um banco de dados ou o sistema do active directory (permitindo que a lista de mapeamento de usuários reais a ser armazenada externamente do código). O ASP.NET inclui uma API de gerenciamento de função interna, bem como um conjunto interno de provedores de função (incluindo aqueles para SQL e Active Directory) que podem ajudar a realizar o mapeamento de usuário/função. Podemos pode, em seguida, atualize o código para permitir que somente usuários dentro de uma função específica "admin" acessar a URL de jantares/criar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Usando a propriedade User.Identity.Name ao criar jantares

Podemos recuperar o nome de usuário do usuário conectado no momento uma solicitação usando a propriedade User.Identity.Name exposta na classe base do controlador.

Anteriores quando implementamos a versão HTTP POST nosso Create () do método de ação que tivemos codificado a propriedade "HostedBy" de refeição para uma cadeia de caracteres estática. Podemos pode agora atualizar esse código em vez de usar a propriedade User.Identity.Name, bem como adicionar RSVP automaticamente para o host de criação de uma refeição:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Porque, adicionamos um atributo [autorizar] para o método Create (), o ASP.NET MVC garante que o método de ação só será executada se o usuário visita a URL de jantares/criar será registrado no site. Como tal, o valor da propriedade User.Identity.Name sempre conterá um nome de usuário válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Usando a propriedade User.Identity.Name ao editar jantares

Agora vamos adicionar alguma lógica de autorização que restringe os usuários para que eles só podem editar as propriedades de jantares que eles próprios hospedam.

Para ajudar com isso, vamos primeiro adicionar um método auxiliar de "IsHostedBy(username)" ao nosso objeto refeição (dentro da classe parcial do Dinner.cs que criamos anteriormente). Esse método auxiliar retorna true ou false dependendo se um nome de usuário fornecido corresponde a propriedade de uma refeição HostedBy e encapsula a lógica necessária para executar uma comparação de cadeia de caracteres de maiusculas e minúsculas deles:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Em seguida, adicionaremos um atributo [autorizar] para os métodos de ação Edit() em nossa classe DinnersController. Isso irá garantir que os usuários devem entrar a solicitação de um */Dinners/Editar / [id]* URL.

Em seguida, podemos adicionar código para nossos métodos de edição que usa o método auxiliar Dinner.IsHostedBy(username) para verificar se o usuário conectado corresponde o host refeição. Se o usuário não é o host, vamos mostrar uma exibição de "InvalidOwner" e encerrar a solicitação. O código para fazer isso é semelhante a abaixo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Podemos, em seguida, clique no diretório \Views\Dinners e escolha Adicionar -&gt;exibir comando de menu para criar uma nova exibição de "InvalidOwner". Podemos será preenchê-la com o abaixo de mensagem de erro:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

E agora quando um usuário tenta editar uma refeição não possuem, ele receberá uma mensagem de erro:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Podemos pode repetir as mesmas etapas para os métodos de ação Delete () em nosso controlador bloquear permissão para excluir jantares bem e certifique-se de que apenas o host de uma refeição poderá excluí-la.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrar/ocultar editar e excluir Links

Estamos está vinculando o método de ação de edição e exclusão de nossa classe DinnersController da nossa URL detalhes:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Atualmente estamos mostrando os links de ação de editar e excluir independentemente se o visitante para a URL de detalhes é o host de uma refeição o. Vamos alterar isso para que os links são exibidos somente se o usuário visitando for o proprietário de uma refeição.

O método de ação Details() em nosso DinnersController recupera um objeto de uma refeição e passa como o objeto de modelo para nosso modelo de exibição:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos atualizar nosso modelo de exibição para condicionalmente Mostrar/ocultar os links de edição e exclusão usando o Dinner.IsHostedBy() método auxiliar como abaixo:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Próximas etapas

Agora vamos examinar como é possível habilitar os usuários autenticados para RSVP para jantares usando AJAX.

>[!div class="step-by-step"]
[Anterior](implement-efficient-data-paging.md)
[Próximo](use-ajax-to-deliver-dynamic-updates.md)
