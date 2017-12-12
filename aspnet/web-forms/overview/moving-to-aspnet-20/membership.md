---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: "Associação | Microsoft Docs"
author: microsoft
description: "Associação ASP.NET amplia o sucesso do modelo de autenticação de formulários do ASP.NET 1. x. Autenticação de formulários do ASP.NET fornece uma maneira conveniente de incorp..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 7e6bff33e9ec1de03c591de6eee2e632c7b7509d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="membership"></a>Associação
====================
por [Microsoft](https://github.com/microsoft)

> Associação ASP.NET amplia o sucesso do modelo de autenticação de formulários do ASP.NET 1. x. Autenticação de formulários do ASP.NET fornece uma maneira conveniente para incorporar um formulário de login em seu aplicativo ASP.NET e validar os usuários em um banco de dados ou outro repositório de dados.


Associação ASP.NET amplia o sucesso do modelo de autenticação de formulários do ASP.NET 1. x. Autenticação de formulários do ASP.NET fornece uma maneira conveniente para incorporar um formulário de login em seu aplicativo ASP.NET e validar os usuários em um banco de dados ou outro repositório de dados. Os membros da classe FormsAuthentication tornam possível lidar com cookies de autenticação, verificar um logon válido, logon de um usuário out etc. No entanto, a implementação da autenticação de formulários em um aplicativo do ASP.NET 1. x pode exigir uma quantidade razoável de código.

A associação no ASP.NET 2.0 é um grande avanço com o uso de autenticação de formulários sozinha. (A associação é mais eficiente quando combinado com autenticação de formulários, mas não é um requisito usando a autenticação de formulários.) Como você verá em breve, você pode usar os controles de logon e a associação do ASP.NET no ASP.NET 2.0 para implementar um sistema de associação avançada sem escrever tanto código em todos os.

## <a name="implementing-membership-in-aspnet-20"></a>Implementando a associação em ASP.NET 2.0

Associação é implementada por quatro etapas a seguir. Tenha em mente que há muitos subetapas que estão envolvidas, bem como configuração opcional que pode ser implementada também. Estas etapas devem para ilustrar a visão geral de configuração de associação.

1. Criar o banco de dados de associação (se o SQL Server é usado como o repositório de associação.)
2. Especifique as opções de participação nos arquivos de configuração de aplicativos. (Associação é habilitada por padrão).
3. Determine o tipo de armazenamento de associação que você deseja usar. As opções são: 

    - Microsoft SQL Server (versão 7.0 ou posterior)
    - Armazenamento do Active Directory
    - Provedor de associação personalizado
4. Configure o aplicativo para autenticação de formulários do ASP.NET. Novamente, associação foi projetada para tirar proveito da autenticação de formulários, mas não é um requisito usando a autenticação de formulários.
5. Definir contas de usuário de associação e configurar funções se desejado.

## <a name="creating-the-membership-database"></a>Criando o banco de dados de associação

Se você estiver usando o SQL Server 7.0 ou posterior como o armazenamento de associação, você pode usar o aspnet\_regsql utilitário (disponível mais facilmente no Visual Studio .NET 2005 Prompt de comando) para configurar seu banco de dados. O aspnet\_regsql utilitário pode ser usado como uma ferramenta de prompt de comando ou por meio de um Assistente de interface gráfica do usuário. O método de assistente é a maneira mais fácil de configurar seu banco de dados. Para acessar o assistente, simplesmente execute o seguinte comando:

`aspnet_regsql W`

Depois de executar esse comando, você verá o Assistente de instalação do ASP.NET SQL Server conforme mostrado abaixo.


![](membership/_static/image1.jpg)

**Figura 1**


O Assistente de instalação do ASP.NET SQL Server cria o site da Web na instância que você especificar no assistente. No entanto, o ASP.NET usará a cadeia de caracteres de conexão no arquivo Machine. config para se conectar ao banco de dados. Por padrão, essa cadeia de caracteres de conexão aponta para uma instância do SQL Server 2005, portanto, se você estiver usando uma instância do SQL Server 2000 ou SQL Server 7.0, será necessário modificar a cadeia de caracteres de conexão no arquivo Machine. config. Essa cadeia de caracteres de conexão pode ser localizada aqui:

[!code-xml[Main](membership/samples/sample1.xml)]

Infelizmente, se você não modifique a cadeia de caracteres de conexão, ASP.NET não lhe dará um erro descritiva. Assim, ele continuará reclamar dizendo que você não criou o banco de dados. Nesse caso, eu modificou a cadeia de caracteres de conexão para apontar para minha instância local do SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Especificando configuração adicionando usuários e funções

A próxima etapa na configuração de associação é adicionar as informações necessárias para o arquivo Web. config do aplicativo. 1. x, modificando o arquivo Web. config no ASP.NET, às vezes, era difícil devido ao uso de lowerCamelCase e da ausência do Intellisense. Visual Studio .NET 2005 torna a tarefa muito mais fácil com o Intellisense para arquivos de configuração, mas o ASP.NET 2.0 é uma etapa posterior, fornecendo uma interface da Web para edição de arquivos de configuração.

Você pode iniciar a interface da Web, clique no botão de configuração do ASP.NET na barra de ferramentas Gerenciador de soluções conforme mostrado abaixo. Você também pode iniciar a interface da Web via pop-ups que são exibidas quando forem inseridos controles de logon.


![](membership/_static/image2.jpg)

**Figura 2**


Isso inicia a ferramenta de administração de Site da Web do ASP.NET mostrado abaixo. A administração de Site da Web do ASP.NET é uma interface de quatro guias que torna mais fácil gerenciar as configurações do aplicativo. As seguintes guias estão disponíveis:

- **Casa**
- **Segurança** configurar o acesso de usuários e funções.
- **Aplicativo** definir configurações do aplicativo.
- **Provedor** configurar e testar seu provedor de associação de aplicativos.

A ferramenta de administração de Site permite que você criar novos usuários, criar novas funções e para gerenciar usuários e funções. Esse recurso não está disponível na interface do Windows. A interface do Windows permite que você defina as configurações de autorização com facilidade e adicionar, excluir e gerenciar provedores de recursos que não estão na ferramenta de administração de Site da Web.

Para iniciar a interface do Windows, abra o snap-in Serviços de informações da Internet, clique em seu aplicativo e escolha Propriedades. Clique na guia ASP.NET e, em seguida, clique no botão Editar configuração. (O aplicativo deve estar executando em ASP.NET 2.0 para o botão Editar configuração seja habilitada. Você pode configurar a versão do ASP.NET no diálogo ASP.NET.) A caixa de diálogo de configurações do ASP.NET é exibida conforme mostrado abaixo.


![](membership/_static/image3.jpg)

**Figura 3**


Na guia geral, cadeias de caracteres de conexão e as configurações do aplicativo são listadas. As configurações em itálico são definidas em um arquivo de configuração pai (Machine. config ou um Web. config em um nível superior) e configurações não em itálico estão no arquivo de configuração de aplicativos. Se uma configuração for adicionada, removido ou editado no nível do aplicativo, ASP.NET será adicionar, remover ou modificar a configuração no Web. config do aplicativo níveis em vez de remover a configuração do arquivo de configuração do qual ele é herdado.

Na guia de autenticação é mostrada abaixo. Isso é onde você irá configurar as configurações de associação. Provedores de associação, configurações de autenticação de formulários e provedores de função podem ser configuradas aqui.


![](membership/_static/image4.jpg)

**Figura 4**


## <a name="implementing-membership-in-your-application"></a>Implementando a associação em seu aplicativo

A maneira mais fácil de implementar a associação do ASP.NET 2.0 no seu aplicativo é usar os controles de Logon fornecidos. Esse método permite que você implemente os fundamentos da associação ASP.NET 2.0 sem gravar qualquer código em todos os.

Os seguintes controles de Logon estão disponíveis no ASP.NET 2.0:

## <a name="login-control"></a>Controle de logon

O controle de logon fornece uma interface para alguém entrar em seu sistema de associação. Ele fornece um nome de usuário e senha textboxt e um botão de logon. Muitos outros recursos comuns, como um link para inscrever-se as pessoas que ainda não tem feito isso, uma caixa de seleção que permite que o usuário automaticamente o logon em visitas subsequentes, um link para um lembrete da senha, etc. Todos os recursos do controle de logon são personalizáveis por meio de propriedades do controle.

No ASP.NET 1. x, os desenvolvedores precisavam que gravar uma quantidade razoável de código para fazer uma pesquisa ao usar a autenticação de formulários. Com a associação do ASP.NET 2.0, você pode validar os usuários sem gravar qualquer código em todos os. ASP.NET fará automaticamente a pesquisa do usuário para você. (Se você estiver usando o controle de logon sem usar a associação do ASP.NET, você pode usar o **OnAuthenticate** método para validar o usuário.)

## <a name="loginview-control"></a>Controle LoginView

O controle LoginView é um controle de modelo que fornece dois modelos padrão. o LoginView e o LoggedInTemplate. O modelo que é exibido é determinado pelo ou não o usuário está conectado em seu sistema de associação. Esse controle é normalmente usado para exibir quando um usuário não registrou ainda em um controle de logon e um controle de status de logon e/ou outros controles de logon quando o usuário efetuou logon no. Se você estiver usando o gerenciamento de função em seu aplicativo ASP.NET, o controle LoginView pode exibir um modelo específico com base na função de usuários. (Mais sobre o ASP.NET gerenciamento de função será abordado posteriormente.)

## <a name="passwordrecovery-control"></a>Controle PasswordRecovery

O controle PasswordRecovery permite aos usuários receber um email com sua senha atual ou redefinir sua senha. Texto não criptografado e senhas criptografadas podem ser recuperadas e enviados por email para os usuários. Se a senha terá hash, ele não pode ser recuperado. Em vez disso, o usuário será necessário para executar uma redefinição de senha.

## <a name="loginstatus-control"></a>Controle de status de logon

O controle de status de logon é usado para exibir um indicador de logon para os usuários que não são registradas no e um indicador de logoff para os usuários que estão conectados no momento. A propriedade Request.IsAuthenticated é usada para determinar qual indicador a ser exibido. O indicador exibido pelo controle de status de logon pode ser um texto (implementadas por meio de **LoginText** e **LogoutText** propriedades) ou imagens (implementadas por meio de **LoginImageUrl**e **LogoutImageUrl** propriedades.)

Quando um usuário faz logoff por meio do controle de status de logon, ele é redirecionado para a URL especificada pelo **LogoutPageUrl** propriedade. Se essa propriedade não for definida, a página atual for atualizada. Desde que o site provavelmente está protegido pela autenticação de formulários, a atualização da página atual redirecionará o usuário para a página de logon para o site.

## <a name="loginname-control"></a>Controle LoginName

O controle LoginName exibe o nome do usuário atualmente conectado ao site.

## <a name="createuserwizard-control"></a>Controle CreateUserWizard

O controle CreateUserWizard fornece aos usuários uma maneira conveniente de se registrar para o seu sistema de associação. Você pode adicionar etapas (implementadas como uma coleção de WizardSteps) por meio da interface mostrada abaixo.


![](membership/_static/image5.jpg)

**Figura 5**


CreateUserWizard é um controle de modelo que deriva da classe do assistente e fornece os seguintes modelos:

- **HeaderTemplate** este modelo controla a aparência do cabeçalho do assistente.
- **SidebarTemplate** este modelo controla a aparência da barra lateral do assistente.
- **StartNavigationTemplate** esse controles de modelo são a aparência do painel de navegação do assistente na etapa inicial.
- **StepNavigationTemplate** esse modelo controla a aparência da área de navegação quando não está na etapa início ou término.
- **FinishNavigationTemplate** esse modelo controla a aparência da área de navegação quando na etapa concluir.

Além disso, para cada etapa que você adiciona ao assistente, o ASP.NET criará um modelo personalizado que contém um ContentTemplate e um CustomNavigationTemplate para essa etapa. Para obter detalhes completos sobre como personalizar o CreateUserWizard, consulte a documentação do VS.NET 2005:

## <a name="changepassword-control"></a>Controle de alteração de senha

O controle de alteração de senha permite aos usuários alterar sua senha. Se a propriedade DisplayUserName for true (é false por padrão), o usuário pode alterar sua senha quando eles não estão conectados. Se o usuário *é* já conectado e a propriedade DisplayUserName for true, o usuário será capaz de alterar a senha de outro usuário não estiver logado fornecendo a eles saibam que a ID de usuário do usuário.

Tenha em mente que, se desejar que os usuários poderão alterar as senhas sem precisar fazer logon, você precisará garantir que a página em que o controle de alteração de senha é exibido permite acesso anônimo. Obviamente, os usuários precisam fornecer a senha antiga para alterar sua senha.

## <a name="role-management"></a>Gerenciamento de função

Gerenciamento de função permite atribuir usuários a uma função específica e restringir o acesso a determinados arquivos ou pastas com base em função. Gerenciamento de função também fornece uma API para que você possa programaticamente determinar função someones ou determinar todos os usuários em uma função específica e responder adequadamente.

Gerenciamento de função não é um requisito na associação do ASP.NET, nem é um requisito para usar o gerenciamento de função de associação. No entanto, os dois complementam uns aos outros bem e é provável que os desenvolvedores usará-los em conjunto com o outro.

Para habilitar o gerenciamento de função em seu aplicativo, faça a seguinte alteração no arquivo Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando o **cacheRolesInCookie** atributo é definido como true, o ASP.NET armazena uma associação de função de usuários em um cookie no cliente. Isso permite que as pesquisas de função sem chamadas para a propriedade RoleProvider. Ao usar esse atributo, os desenvolvedores são encorajados para garantir que o **cookieProtection** atributo é definido como todos. (Essa é a configuração padrão). Isso garante que os dados do cookie são criptografados e ajudam a garantir que o conteúdo de cookies não foram alterado. Funções podem ser adicionadas usando a ferramenta de administração de Site. Ele permite que você facilmente definir funções, configure o acesso a partes do site com base nessas funções e atribuir usuários a funções.


![](membership/_static/image6.jpg)

**Figura 6**


Como mostrado acima, é possível adicionar novas funções, basta digitar o nome da função e clique em Adicionar função. Funções existentes podem ser gerenciadas ou excluídas, clicando no link apropriado na lista de funções existentes.

Quando você gerencia uma função, você pode adicionar ou remover usuários, conforme mostrado abaixo.


![](membership/_static/image7.jpg)

**Figura 7**


Ao marcar a caixa de seleção está na função de usuário, você pode adicionar facilmente um usuário a uma função específica. ASP.NET atualizará automaticamente o banco de dados de associação com as entradas apropriadas. Você também deve configurar regras de acesso para seu aplicativo. Os desenvolvedores do ASP.NET 1. x estiver familiarizados com a fazer isso por meio de &lt;autorização&gt; elemento no arquivo Web. config e essa opção ainda está disponível no ASP.NET 2.0. No entanto, o mais fácil de gerenciar o acesso regras usando o Site da ferramenta de administração conforme mostrado abaixo.


![](membership/_static/image8.jpg)

**Figura 8**


Nesse caso, a pasta de administração é realçada (difícil ver porque a ferramenta destaca em cinza claro) e a função de administradores têm acesso. Todos os outros usuários são negados. Você pode clicar no ícone do cabeçalho para selecionar uma regra e, em seguida, use os botões Mover para cima e mover para baixo para organizar as regras. Assim como acontece com o ASP.NET &lt;autorização&gt; elemento, as regras são processadas na ordem em que aparecem. Em outras palavras, se a ordem das regras na captura a acima foram revertida, não teria acesso à pasta administração porque a primeira regra que encontra ASP.NET seria a regra que nega todos para a pasta.

O ASP.NET 2.0 adiciona um arquivo Web. config para a pasta para a qual você está especificando uma regra de acesso. Regras de acesso podem ser editadas por meio do arquivo de configuração ou a ferramenta de administração de Site. Em outras palavras, a ferramenta de administração de Site é simplesmente uma interface por meio do qual o arquivo de configuração pode ser editado em um ambiente amigável.

## <a name="using-roles-in-code"></a>Usando funções em código

A API de gerenciamento de função não foi alterado desde a versão 1. x. O **IsInRole** método é usado para determinar se um usuário está em uma função específica.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET também cria uma instância de RolePrincipal como um membro do contexto atual. O objeto RolePrincipal pode ser usado para obter todas as funções às quais o usuário pertence da seguinte maneira:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Usando RoleGroups com o controle LoginView

Agora que você tem uma compreensão do gerenciamento de função e associação, permite que abordam brevemente como o controle LoginView tira proveito desse recurso no ASP.NET 2.0. Como discutido anteriormente, o controle LoginView é um controle de modelo que contém dois modelos por padrão. o LoginView e o LoggedInTemplate. Dentro das tarefas LoginView caixa de diálogo é um link (mostrado abaixo) que permite que você edite RoleGroups.


![](membership/_static/image9.jpg)

**Figura 9**


Cada objeto de RoleGroup contém uma matriz de cadeias de caracteres que define as funções de RoleGroup aplica-se a. Para adicionar um RoleGroup novo para o controle LoginView, clique no link Editar RoleGroups. Na imagem acima, você pode ver que adicionei um RoleGroup nova para administradores. Selecionando esse RoleGroup (RoleGroup[0]) no menu suspenso de modos de exibição, posso configurar um modelo que será exibido apenas aos membros da função de administradores. Na imagem abaixo, adicionei um RoleGroup novo que se aplica a membros da função de vendas e a função de distribuição. Isso adiciona um RoleGroup segundo menu suspenso de modos de exibição na caixa de diálogo Tarefas LoginView e nada é adicionado a esse modelo ficarão visível por qualquer usuário no Sales ou distribuição função.


![](membership/_static/image10.jpg)

**Figura 10**


## <a name="overriding-the-existing-membership-provider"></a>Substituindo o provedor de associação existente

Há duas maneiras que você pode estender a funcionalidade de associação do ASP.NET. Em primeiro lugar, você pode alterar, obviamente, a funcionalidade existente da classe SqlMembershipProvider herdando a ele e substituindo seus métodos. Por exemplo, se você quiser implementar sua própria funcionalidade quando os usuários são criados, você pode criar sua própria classe que herda de SqlMembershipProvider da seguinte maneira:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, por outro lado, você deseja criar seu próprio provedor (para armazenar as informações de associação em um banco de dados, por exemplo), você pode criar seu próprio provedor.

## <a name="creating-your-own-membership-provider"></a>Criar seu próprio provedor de associação

Para criar seu próprio provedor de associação, primeiro você precisará criar uma classe que herda da classe MembershipProvider. Se você estiver usando o VB.NET, o Visual Studio 2005 adicionará os stubs para todos os métodos que você precisa substituir. Se você estiver usando c#, seu até você adicionar os stubs.

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

Que uma lista para implementar como um desenvolvedor de c#. Você pode achar mais fácil de criar a classe no VB.NET sem qualquer implementação e, em seguida, usar .NET Reflector ou uma ferramenta semelhante para converter o código em c#.

A cadeia de caracteres de conexão e outras propriedades devem ser definidas para seus padrões no método Initialize. (O método Initialize é acionado quando o provedor é carregado no tempo de execução). O segundo parâmetro para o método Initialize é do tipo System.Collections.Specialized.NameValueCollection e é uma referência para o &lt;adicionar&gt; elemento que está associado com o provedor personalizado no arquivo Web. config. Essa entrada é semelhante ao seguinte:

[!code-xml[Main](membership/samples/sample6.xml)]

Aqui está um exemplo do método de inicialização.

[!code-csharp[Main](membership/samples/sample7.cs)]

Para validar o usuário ao enviar o formulário de logon, você precisará usar o método ValidateUser. Esse método é acionado quando o usuário clica no botão de logon no controle de logon. Coloque o código que faz a pesquisa de usuário nesse método.

Como você pode ver, escrever seu próprio provedor de associação não é difícil e permite estender essa funcionalidade avançada do ASP.NET 2.0.
