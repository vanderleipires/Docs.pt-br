---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Associação | Microsoft Docs
author: microsoft
description: Associação do ASP.NET compila o sucesso do modelo de autenticação de formulários do ASP.NET 1. x. Autenticação de formulários do ASP.NET fornece uma maneira conveniente de incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: d7fa3cb61608ea089141931cb9362359cdc92619
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830029"
---
<a name="membership"></a>Associação
====================
por [Microsoft](https://github.com/microsoft)

> Associação do ASP.NET compila o sucesso do modelo de autenticação de formulários do ASP.NET 1. x. Autenticação de formulários do ASP.NET fornece uma maneira conveniente de incorporar um formulário de logon em seu aplicativo ASP.NET e validar os usuários em relação a um banco de dados ou outro armazenamento de dados.


Associação do ASP.NET compila o sucesso do modelo de autenticação de formulários do ASP.NET 1. x. Autenticação de formulários do ASP.NET fornece uma maneira conveniente de incorporar um formulário de logon em seu aplicativo ASP.NET e validar os usuários em relação a um banco de dados ou outro armazenamento de dados. Os membros da classe FormsAuthentication tornam possível lidar com cookies de autenticação, verificar se há um logon válido, faça o logon de um usuário etc. No entanto, a implementação da autenticação de formulários em um aplicativo do ASP.NET 1. x pode exigir uma quantidade razoável de código.

A associação do ASP.NET 2.0 é um grande avanço em relação ao uso de autenticação de formulários sozinha. (A associação é mais robusta, quando combinado com a autenticação de formulários, mas não é um requisito usando a autenticação de formulários). Como você verá em breve, você pode usar os controles de logon e associação do ASP.NET no ASP.NET 2.0 para implementar um sistema de associação avançados sem escrever muito código em todos os.

## <a name="implementing-membership-in-aspnet-20"></a>Implementando a associação do ASP.NET 2.0

Associação é implementada por quatro etapas a seguir. Tenha em mente que há muitos subetapas que estão envolvidas, bem como a configuração opcional que pode ser implementada também. Essas etapas destinam-se a ilustrar o panorama geral de configuração de associação.

1. Criar seu banco de dados de associação (se o SQL Server é usado como o armazenamento de associação.)
2. Especifique as opções de associação nos arquivos de configuração de aplicativos. (A associação é habilitada por padrão.)
3. Determine o tipo de armazenamento de associação que você deseja usar. As opções são: 

    - Microsoft SQL Server (versão 7.0 ou posterior)
    - Store do Active Directory
    - Provedor de associação personalizado
4. Configure o aplicativo para autenticação de formulários do ASP.NET. Mais uma vez, associação é projetada para tirar proveito da autenticação de formulários, mas não é um requisito usando a autenticação de formulários.
5. Definir contas de usuário de associação e configurar funções, se desejado.

## <a name="creating-the-membership-database"></a>Criando o banco de dados de associação

Se você estiver usando o SQL Server 7.0 ou posterior como o armazenamento de associação, você pode usar o aspnet\_regsql utilitário (disponível com mais facilidade no Visual Studio .NET 2005 Prompt de comando) para configurar seu banco de dados. O aspnet\_regsql utilitário pode ser usado como uma ferramenta de prompt de comando ou por meio de um Assistente de GUI. O método de assistente é a maneira mais fácil de configurar seu banco de dados. Para acessar o assistente, basta execute o comando a seguir:

`aspnet_regsql W`

Depois de executar esse comando, você será apresentado com o Assistente de instalação de servidor do ASP.NET SQL conforme mostrado abaixo.


![](membership/_static/image1.jpg)

**Figura 1**


O Assistente de instalação do ASP.NET SQL Server cria o site da Web na instância que você especifica no assistente. No entanto, o ASP.NET usará a cadeia de caracteres de conexão no arquivo Machine. config para se conectar ao seu banco de dados. Por padrão, essa cadeia de caracteres de conexão aponta para uma instância do SQL Server 2005, portanto, se você estiver usando uma instância do SQL Server 2000 ou SQL Server 7.0, você precisará modificar a cadeia de caracteres de conexão no arquivo Machine. config. Essa cadeia de caracteres de conexão pode ser localizada aqui:

[!code-xml[Main](membership/samples/sample1.xml)]

Infelizmente, se você não modificar a cadeia de caracteres de conexão, ASP.NET não lhe dará um erro descritivo. Assim, ele continuará a reclamar dizendo que você não tiver criado o banco de dados. No caso acima, modifiquei a cadeia de caracteres de conexão para apontar para minha instância local do SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Especificando a configuração e adicionar usuários e funções

A próxima etapa na configuração de associação é adicionar as informações necessárias para o arquivo Web. config do aplicativo. No ASP.NET 1. x, modificando o arquivo Web. config, às vezes, era difícil devido ao uso de lowerCamelCase e da ausência do Intellisense. Visual Studio .NET 2005 torna a tarefa muito mais fácil com o Intellisense para arquivos de configuração, mas o ASP.NET 2.0 vai um passo além, fornecendo uma interface da Web para editar arquivos de configuração.

Você pode iniciar a interface da Web clicando no botão de configuração do ASP.NET na barra de ferramentas Solution Explorer conforme mostrado abaixo. Você também pode iniciar a interface da Web por meio de pop-ups que são exibidos quando os controles de logon são inseridos.


![](membership/_static/image2.jpg)

**Figura 2**


Isso inicia a ferramenta de administração de Site da Web do ASP.NET mostrado abaixo. A administração de Site da Web do ASP.NET é uma interface de guia de quatro que torna mais fácil de gerenciar configurações de aplicativo. As seguintes guias estão disponíveis:

- **Início**
- **Segurança** configurar usuários, funções e acesso.
- **Aplicativo** definir configurações do aplicativo.
- **Provedor** configurar e testar o seu provedor de associação de aplicativos.

A ferramenta de administração de Site da Web permite que você facilmente criar novos usuários, criar novas funções e para gerenciar usuários e funções. Essa capacidade não está disponível na interface do Windows. A interface do Windows permite que você facilmente definir as configurações de autorização e adicionar, excluir e gerenciar provedores de recursos que não estão na ferramenta de administração de Site da Web.

Para iniciar a interface do Windows, abra o snap-in Internet Information Services, clique com botão direito em seu aplicativo e escolha Propriedades. Clique na guia ASP.NET e, em seguida, clique no botão Editar configuração. (O aplicativo deve estar executando em ASP.NET 2.0 para o botão Editar configuração a ser habilitado. Você pode configurar a versão do ASP.NET no diálogo do ASP.NET.) A caixa de diálogo de configurações do ASP.NET é exibida conforme mostrado abaixo.


![](membership/_static/image3.jpg)

**Figura 3**


Na guia geral, cadeias de caracteres de conexão e as configurações do aplicativo são listadas. Todas as configurações em itálico são definidas em um arquivo de configuração pai (Machine. config ou um Web. config em um nível mais alto) e configurações não está em itálico são do arquivo de configuração de aplicativos. Se uma configuração for adicionada, removido ou editado no nível do aplicativo, ASP.NET será adicionar, remover ou modificar a configuração no Web. config a níveis de aplicativo em vez de remover a configuração do arquivo de configuração do qual ele é herdado.

A guia de autenticação é mostrada abaixo. Isso é onde você irá configurar as configurações de associação. Configurações de autenticação, provedores de associação de formulários e provedores de função podem ser configurados aqui.


![](membership/_static/image4.jpg)

**Figura 4**


## <a name="implementing-membership-in-your-application"></a>Implementando a associação em seu aplicativo

A maneira mais fácil de implementar a associação do ASP.NET 2.0 em seu aplicativo é usar os controles de Logon fornecidos. Esse método permite que você implemente os conceitos básicos da associação do ASP.NET 2.0 sem escrever nenhum código em todos os.

Os controles de Logon a seguir estão disponíveis no ASP.NET 2.0:

## <a name="login-control"></a>Controle de logon

O controle de logon fornece uma interface para que alguém faça logon no seu sistema de associação. Ele fornece um nome de usuário e senha textboxt e um botão de logon. Muitos outros recursos comuns, como um link para se registrar para as pessoas que ainda não tem feito isso, uma caixa de seleção que permite que o usuário automaticamente logon em visitas subsequentes, um link para um lembrete da senha, etc. Todos os recursos do controle Login são personalizáveis por meio de propriedades do controle.

No ASP.NET 1. x, os desenvolvedores tinham de gravar uma quantidade razoável de código para realizar uma pesquisa ao usar a autenticação de formulários. Com a associação do ASP.NET 2.0, você pode validar os usuários sem escrever nenhum código em todos os. ASP.NET fará automaticamente a pesquisa do usuário para você. (Se você estiver usando o controle de logon sem usar a associação do ASP.NET, você pode usar o **OnAuthenticate** método para validar o usuário.)

## <a name="loginview-control"></a>Controle LoginView

O controle LoginView é um controle personalizado que fornece dois modelos por padrão. o LoginView e o LoggedInTemplate. O modelo que é exibido é determinado pelo ou não o usuário está conectado ao seu sistema de associação. Normalmente, esse controle é usado para exibir quando um usuário ainda não fez um controle de logon e um controle LoginStatus e/ou outros controles de logon quando o usuário fez login. Se você estiver usando o gerenciamento de função em seu aplicativo ASP.NET, o controle LoginView pode exibir um modelo específico com base na função de usuários. (Mais sobre o ASP.NET gerenciamento de função será abordado posteriormente.)

## <a name="passwordrecovery-control"></a>Controle PasswordRecovery

O controle PasswordRecovery permite aos usuários receber um email com sua senha atual ou redefina sua senha. Texto não criptografado e senhas criptografadas podem ser recuperadas e enviados por email para os usuários. Se a senha será hashed, ele não pode ser recuperado. Em vez disso, o usuário será necessário para executar uma redefinição de senha.

## <a name="loginstatus-control"></a>Controle LoginStatus

O controle LoginStatus é usado para exibir um indicador de logon para usuários que não são registradas no e um indicador de logoff para usuários que estão conectados no momento. A propriedade Request.IsAuthenticated é usada para determinar qual indicador a ser exibido. O indicador exibido pelo controle LoginStatus pode ser um texto (implementado por meio do **LoginText** e **LogoutText** propriedades) ou imagens (implementado por meio do **LoginImageUrl**e **LogoutImageUrl** propriedades.)

Quando um usuário fizer logoff, por meio do controle LoginStatus, ele é redirecionado para a URL especificada pelo **LogoutPageUrl** propriedade. Se essa propriedade não for definida, a página atual for atualizada. Uma vez que o site provavelmente é protegido pela autenticação de formulários, a atualização da página atual redirecionará o usuário para a página de logon para o site.

## <a name="loginname-control"></a>Controle LoginName

O controle LoginName exibe o nome de usuário do usuário atualmente conectado ao site.

## <a name="createuserwizard-control"></a>Controle CreateUserWizard

O controle CreateUserWizard fornece aos usuários uma maneira conveniente de se registrar para o seu sistema de associação. Você pode adicionar etapas (implementadas como uma coleção de WizardSteps) por meio da interface mostrada abaixo.


![](membership/_static/image5.jpg)

**Figura 5**


O CreateUserWizard é um controle personalizado que deriva da classe de assistente e fornece os seguintes modelos:

- **HeaderTemplate** este modelo controla a aparência do cabeçalho do assistente.
- **SidebarTemplate** este modelo controla a aparência da barra lateral do assistente.
- **StartNavigationTemplate** essa a aparência do painel de navegação são do assistente na etapa inicial de controles de modelo.
- **StepNavigationTemplate** esse modelo controla a aparência da área de navegação quando não estiver na etapa início ou término.
- **FinishNavigationTemplate** esse modelo controla a aparência da área de navegação quando estiver na etapa concluir.

Além disso, para cada etapa que você adiciona ao assistente, o ASP.NET criará um modelo personalizado que contém um ContentTemplate e um CustomNavigationTemplate para essa etapa. Para obter detalhes completos sobre como personalizar o CreateUserWizard, consulte a documentação do VS.NET 2005:

## <a name="changepassword-control"></a>Controle ChangePassword

O controle ChangePassword permite que os usuários alterem sua senha. Se a propriedade DisplayUserName for verdadeira (é false por padrão), o usuário pode alterar sua senha quando eles não são registrados no. Se o usuário *é* já conectado e a propriedade DisplayUserName for true, o usuário será capaz de alterar a senha de outro usuário que não está conectado no fornecimento de souberem a identificação do usuário.

Tenha em mente que, se você quiser que os usuários possam alterar senhas sem necessidade de fazer logon, você precisará garantir que a página na qual o controle de alteração de senha é exibido permite o acesso anônimo. Obviamente, os usuários precisarão fornecer a senha antiga para alterar sua senha.

## <a name="role-management"></a>Gerenciamento de função

Gerenciamento de função permite que você atribuir usuários a uma função específica e, em seguida, restringir o acesso a determinados arquivos ou pastas com base na função. Gerenciamento de função também fornece uma API para que você possa programaticamente determinar função alguém ou determinar todos os usuários em uma função específica e responder de acordo.

Gerenciamento de função não é um requisito na associação do ASP.NET, nem é um requisito para usar o gerenciamento de função de associação. No entanto, os dois complementam entre si muito bem e é provável que os desenvolvedores usarão-los em conjunto com o outro.

Para habilitar o gerenciamento de função em seu aplicativo, faça a seguinte alteração em seu arquivo Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando o **cacheRolesInCookie** atributo é definido como true, o ASP.NET armazena uma associação de função de usuários em um cookie no cliente. Isso permite que as pesquisas de função para ocorrer sem chamadas em RoleProvider. Ao usar esse atributo, os desenvolvedores são incentivados a garantir que o **cookieProtection** atributo será definido como All. (Isso é a configuração padrão). Isso garante que os dados de cookie são criptografados e ajuda a garantir que o conteúdo de cookies não foram alterado. As funções podem ser adicionadas usando a ferramenta de administração de Site da Web. Ele permite que você facilmente definir funções, configurar o acesso a partes do site com base nessas funções e atribuir usuários às funções.


![](membership/_static/image6.jpg)

**Figura 6**


Como mostrado acima, novas funções podem ser adicionadas simplesmente inserir o nome da função e, em seguida, clicando em Adicionar função. As funções existentes podem ser gerenciadas ou excluídas, clicando no link apropriado na lista de funções existentes.

Quando você gerencia uma função, você pode adicionar ou remover usuários, conforme mostrado abaixo.


![](membership/_static/image7.jpg)

**Figura 7**


Ao marcar a caixa de seleção está na função de usuário, você pode facilmente adicionar um usuário a uma função específica. ASP.NET atualizará automaticamente o banco de dados de associação com as entradas apropriadas. Você também desejará configurar regras de acesso para seu aplicativo. Os desenvolvedores do ASP.NET 1. x estiver familiarizados com isso por meio de &lt;autorização&gt; elemento no arquivo Web. config e essa opção ainda está disponível no ASP.NET 2.0. No entanto, é mais fácil gerenciar o acesso regras usando a ferramenta Web Site Administration conforme mostrado abaixo.


![](membership/_static/image8.jpg)

**Figura 8**


Nesse caso, a pasta de administração é realçada (difícil ver porque a ferramenta destaca em cinza claro) e a função de administradores foi concedida acesso. Todos os outros usuários serão negados. Você pode clicar no ícone principal para selecionar uma regra e, em seguida, use os botões Mover para cima e mover para baixo para organizar as regras. Assim como acontece com o ASP.NET &lt;autorização&gt; elemento, as regras são processadas na ordem em que aparecem. Em outras palavras, se a ordem de regras na captura a acima foram revertida, ninguém teria acesso à pasta de administração porque a primeira regra que encontra ASP.NET seria a regra que nega todas as pessoas para a pasta.

O ASP.NET 2.0 adiciona um arquivo Web. config na pasta para o qual você está especificando uma regra de acesso. Regras de acesso podem ser editadas por meio do arquivo de configuração ou a ferramenta de administração de Site da Web. Em outras palavras, a ferramenta de administração de Site da Web é simplesmente uma interface por meio do qual o arquivo de configuração pode ser editado em um ambiente amigável.

## <a name="using-roles-in-code"></a>Usando as funções no código

A API de gerenciamento de função não foi alterado desde a versão 1.x. O **IsInRole** método é usado para determinar se um usuário está em uma função específica.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET também cria uma instância de RolePrincipal como um membro do contexto atual. O objeto RolePrincipal pode ser usado para obter todas as funções aos quais o usuário pertence da seguinte maneira:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Usando RoleGroups com o controle LoginView

Agora que você tem uma compreensão de gerenciamento de função e associação, permite que abordam brevemente como o controle LoginView tira proveito desse recurso no ASP.NET 2.0. Conforme discutido anteriormente, o controle LoginView é um controle modelo que contém dois modelos por padrão. o LoginView e o LoggedInTemplate. Dentro das tarefas LoginView caixa de diálogo é um link (mostrado abaixo) que permite que você edite RoleGroups.


![](membership/_static/image9.jpg)

**Figura 9**


Cada objeto RoleGroup contém uma matriz de cadeias de caracteres que define quais funções que RoleGroup aplica-se a. Para adicionar um novo RoleGroup para o controle LoginView, clique no link Editar RoleGroups. Na imagem acima, você pode ver que adicionei um novo RoleGroup para administradores. Ao selecionar esse RoleGroup (RoleGroup[0]) no menu suspenso de modos de exibição, posso configurar um modelo que será exibido apenas aos membros da função de administradores. Na imagem abaixo, adicionei um novo RoleGroup que se aplica a membros da função de vendas e a função de distribuição. Isso adiciona um segunda RoleGroup ao menu suspenso de modos de exibição na caixa de diálogo Tarefas LoginView e nada acrescentado a esse modelo será visível por qualquer usuário em Sales ou distribuição de função.


![](membership/_static/image10.jpg)

**Figura 10**


## <a name="overriding-the-existing-membership-provider"></a>Substituindo o provedor de associação existente

Há duas maneiras que você pode estender a funcionalidade de associação do ASP.NET. Em primeiro lugar, você pode alterar, obviamente, a funcionalidade existente da classe SqlMembershipProvider herdado dele e substituindo seus métodos. Por exemplo, se você quiser implementar sua própria funcionalidade quando os usuários são criados, você pode criar sua própria classe que herda de SqlMembershipProvider da seguinte maneira:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, por outro lado, você deseja criar seu próprio provedor (para armazenar as informações de associação em um banco de dados, por exemplo), você pode criar seu próprio provedor.

## <a name="creating-your-own-membership-provider"></a>Criar seu próprio provedor de associação

Para criar seu próprio provedor de associação, você precisa primeiro criar uma classe que herda da classe MembershipProvider. Se você estiver usando o VB.NET, o Visual Studio 2005 adicionará os stubs para todos os métodos que você precisa substituir. Se você estiver usando c#, depende de você adicionar os stubs.

Você precisará substituir o seguinte:

- Propriedade ApplicationName
- Função ChangePassword
- Função ChangePasswordQuestionAndAnswer
- Função CreateUser
- Função DeleteUser
- Propriedade EnablePasswordReset
- Propriedade EnablePasswordRetrieval
- Função FindUsersByEmail
- Função FindUsersByName
- Função GetAllUsers
- Função GetNumberOfUsersOnline
- Função GetPassword
- Função GetUser
- Função GetUserNameByEmail
- Propriedade MaxInvalidPasswordAttempts
- Propriedade MinRequiredNonAlphanumericCharacters
- Propriedade MinRequiredPasswordLength
- Propriedade PasswordAttemptWindow
- Propriedade PasswordFormat
- Propriedade PasswordStrengthRegularExpression
- Propriedade RequiresQuestionAndAnswer
- Propriedade RequiresUniqueEmail
- Função ResetPassword
- Desbloquear a função de usuário
- Função UpdateUser
- Função ValidateUser

Que uma grande lista implementar como desenvolvedor em c#. Você talvez ache mais fácil de criar a classe no VB.NET sem qualquer implementação e, em seguida, usar o .NET Reflector ou uma ferramenta semelhante para converter o código em c#.

A cadeia de caracteres de conexão e outras propriedades devem ser definidas como seus padrões no método Initialize. (O método Initialize é disparado quando o provedor é carregado no tempo de execução). O segundo parâmetro para o método Initialize é do tipo NameValueCollection e é uma referência para o &lt;adicionar&gt; elemento que está associado com o provedor personalizado no arquivo Web. config. Essa entrada é semelhante ao seguinte:

[!code-xml[Main](membership/samples/sample6.xml)]

Aqui está um exemplo do método Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Para validar o usuário ao enviar o formulário de logon, você precisará usar o método ValidateUser. Esse método é acionado quando o usuário clica no botão de logon no controle Login. Coloque o código que faz a pesquisa de usuário nesse método.

Como você pode ver, escrever seu próprio provedor de associação não é difícil e permite que você amplie essa poderosa funcionalidade do ASP.NET 2.0.
