---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Validando credenciais de usuário contra o usuário de associação de Store (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como validar as credenciais do usuário no repositório de usuário associado usando o meio programático e o controle de logon...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: f5f1121bacdf287e346419d70ac155f47bc826ac
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830019"
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Validando credenciais de usuário contra o usuário de associação de Store (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> Neste tutorial, examinaremos como validar as credenciais do usuário no repositório de usuário associado usando o meio programático e o controle de logon. Podemos também examinar como personalizar a aparência e o comportamento do controle de logon.


## <a name="introduction"></a>Introdução

No <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-vb.md) vimos como criar uma nova conta de usuário na estrutura de associação. Nós examinamos primeiro criando programaticamente as contas de usuário por meio de `Membership` da classe `CreateUser` método e, em seguida, examinado usando o controle CreateUserWizard Web. No entanto, a página de logon no momento, valida as credenciais fornecidas em relação a uma lista codificada de pares de nome de usuário e senha. É necessário atualizar a lógica da página de logon para que ele valida as credenciais no repositório de usuário da estrutura de associação.

Muito, como com a criação de contas de usuário, as credenciais podem ser validadas forma declarativa ou programaticamente. A API Membership inclui um método para validar programaticamente credenciais do usuário no repositório de usuário. E o ASP.NET é fornecido com o controle de Web de logon, que renderiza uma interface do usuário com caixas de texto para o nome de usuário e senha e um botão para fazer logon.

Neste tutorial, examinaremos como validar as credenciais do usuário no repositório de usuário associado usando o meio programático e o controle de logon. Podemos também examinar como personalizar a aparência e o comportamento do controle de logon. Vamos começar!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Etapa 1: Validar as credenciais contra o Store de usuário de associação

Para sites que usam a autenticação de formulários, um usuário faz logon no site visitar uma página de logon e inserindo suas credenciais. Essas credenciais, em seguida, são comparadas com o repositório do usuário. Se eles forem válidos, o usuário recebe um tíquete de autenticação de formulários, que é um token de segurança que indica a identidade e a autenticidade do visitante.

Para validar um usuário em relação a estrutura de associação, use o `Membership` da classe [ `ValidateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). O `ValidateUser` método utiliza dois parâmetros de entrada - `username` e `password` - e retorna um valor booliano que indica se as credenciais eram válidas. Como com o `CreateUser` método examinamos no tutorial anterior, o `ValidateUser` método delega a validação real para o provedor de associação configurado.

O `SqlMembershipProvider` valida as credenciais fornecidas por meio da obtenção de senha do usuário especificado por meio de `aspnet_Membership_GetPasswordWithFormat` procedimento armazenado. Lembre-se de que o `SqlMembershipProvider` armazena as senhas dos usuários usando um dos três formatos: clear, criptografia ou hash. O `aspnet_Membership_GetPasswordWithFormat` procedimento armazenado retorna a senha em seu formato bruto. Para senhas criptografadas ou hash, o `SqlMembershipProvider` transforma os `password` valor passado para o `ValidateUser` método em seu equivalente criptografadas ou hash do estado e, em seguida, compara-o com o que foi retornado do banco de dados. Se a senha armazenada no banco de dados corresponde à senha formatada inserida pelo usuário, as credenciais são válidas.

Vamos atualizar nossa página de logon (~ /`Login.aspx`), de modo que ele valida as credenciais fornecidas no repositório de usuário associado do framework. Criamos essa página de logon novamente o <a id="Tutorial02"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, criando uma interface com duas caixas de texto para o nome de usuário e senha, um Lembrar-Me checkbox e um botão de Login (veja a Figura 1). O código valida as credenciais inseridas em uma lista de embutido em código de pares de nome de usuário e senha (Scott/Jisun/senha, senha e Sam /). No <a id="Tutorial03"> </a> [ *configuração de autenticação de formulários e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) tutorial atualizamos o código da página de logon para armazenar informações adicionais em formulários tíquete de autenticação `UserData` propriedade.


[![Interface a página de logon inclui duas caixas de texto, um CheckBoxList e um botão](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Figura 1**: Interface inclui duas caixas de texto da página de logon a, um CheckBoxList e um botão ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Interface do usuário da página de logon pode permanecer inalterado, mas precisamos substituir o botão de logon `Click` manipulador de eventos com o código que valida o usuário no repositório de usuário associado do framework. Atualize o manipulador de eventos para que seu código aparece da seguinte maneira:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Esse código é surpreendentemente simple. Começamos chamando o `Membership.ValidateUser` método, passando o nome de usuário fornecido e a senha. Se esse método retorna True, o usuário está conectado ao site por meio de `FormsAuthentication` RedirectFromLoginPage de método classe do. (Como discutimos na <a id="Tutorial02"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, o `FormsAuthentication.RedirectFromLoginPage` cria os formulários de tíquete de autenticação e, em seguida, redireciona o usuário para a página apropriada.) Se as credenciais são inválidas, no entanto, o `InvalidCredentialsMessage` rótulo for exibido, informando ao usuário seu nome de usuário ou senha estava incorreta.

Isso é tudo para ele!

Para testar se a página de login funciona conforme o esperado, tente fazer logon com uma das contas de usuário que você criou no tutorial anterior. Ou, se você ainda não tiver criado uma conta, vá em frente e crie um no `~/Membership/CreatingUserAccounts.aspx` página.

> [!NOTE]
> Quando o usuário insere suas credenciais e envia o formulário da página de logon, as credenciais, incluindo sua senha, são transmitidas pela Internet para o servidor web no *texto sem formatação*. Isso significa que qualquer hacker detecção o tráfego de rede pode ver o nome de usuário e senha. Para evitar isso, é essencial para criptografar o tráfego de rede usando [camadas de soquete seguro (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Isso garantirá que as credenciais (bem como uma marcação HTML da página inteira) é criptografada desde o momento em que eles deixam o navegador até que elas são recebidas pelo servidor web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Como a estrutura de associação lida com tentativas inválidas de logon

Quando um visitante atinge a página de logon e envia suas credenciais, o navegador faz uma solicitação HTTP para a página de logon. Se as credenciais forem válidas, a resposta HTTP inclui o tíquete de autenticação em um cookie. Portanto, um hacker tentar invadir o seu site pode criar um programa que exaustivamente envia solicitações HTTP para a página de logon com um nome de usuário válido e uma estimativa a senha. Se o Palpite de senha estiver correto, a página de logon retornará o cookie do tíquete de autenticação, no ponto em que o programa sabe que ele tem deparei com um par de nome de usuário e senha válidos. Por força bruta, um programa desse tipo pode ser capaz de se deparar com uma senha de usuário, especialmente se a senha é fraca.

Para impedir esses ataques de força bruta, a estrutura de associação bloqueia um usuário se não houver um determinado número de tentativas de logon malsucedidas dentro de um determinado período de tempo. Os parâmetros exatos são configuráveis por meio de duas definições de configuração de provedor de associação:

- `maxInvalidPasswordAttempts` -Especifica quantas senhas inválidas tentativas são permitidas para o usuário dentro do período de tempo antes que a conta está bloqueada. O valor padrão é 5.
- `passwordAttemptWindow` -indica o período de tempo em minutos durante o qual o número especificado de tentativas inválidas de logon fará com que a conta seja bloqueada. O valor padrão é 10.

Se um usuário foi bloqueado, ela não é possível fazer logon até que um administrador a desbloqueie sua conta. Quando um usuário é bloqueado, o `ValidateUser` método serão *sempre* retornar `False`, mesmo se as credenciais válidas sejam fornecidas. Embora esse comportamento reduz a probabilidade de que um hacker invadir o seu site por meio de métodos de força bruta, ele pode acabar bloquear um usuário válido que simplesmente esqueceu sua senha ou acidentalmente tem a tecla Caps Lock na ou está tendo um dia ruim de digitação.

Infelizmente, não há nenhuma ferramenta interna para desbloquear uma conta de usuário. Para desbloquear uma conta, você pode modificar o banco de dados diretamente - alterar o `IsLockedOut` campo o `aspnet_Membership` de tabela para a conta de usuário apropriado – ou crie uma interface baseada na web que lista as contas com opções para desbloqueá-los bloqueada. Vamos examinar Criando interfaces administrativo para a realização de tarefas comuns relacionadas à conta e à função de usuário em um tutorial futuro.

> [!NOTE]
> A desvantagem de `ValidateUser` método é que quando as credenciais fornecidas são inválidas, ele não fornece nenhuma explicação sobre o motivo. As credenciais podem estar inválidas porque não há nenhum par de nome de usuário/senha correspondente no repositório do usuário, ou porque o usuário ainda não foram aprovado ou porque o usuário foi bloqueado. Na etapa 4, veremos como mostrar uma mensagem mais detalhada para o usuário quando sua tentativa de logon falha.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Etapa 2: Coletar credenciais por meio do controle de Web de logon

O [controle de Web de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) renderiza uma interface de usuário padrão muito semelhante ao que criamos na <a id="Tutorial02"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial. Usar o controle de logon nos poupa o trabalho de ter de criar a interface para coletar credenciais do visitante. Além disso, o controle de logon entra automaticamente no usuário (supondo que as credenciais enviadas forem válidas), assim, salvando da necessidade de escrever nenhum código.

Vamos atualizar `Login.aspx`, substituindo a interface criada manualmente e de código com um controle de logon. Inicie removendo a marcação existente e o código em `Login.aspx`. Você pode excluí-lo imediatamente, ou simplesmente comentá-lo. Para comentar marcação declarativa, coloque-o com o `<%--` e `--%>` delimitadores. Você pode inserir esses delimitadores manualmente ou, como mostra a Figura 2, você pode selecionar o texto para comentar e, em seguida, clique no comente o ícone de linhas selecionadas na barra de ferramentas. Da mesma forma, você pode usar o comente o ícone de linhas selecionadas para comentar o código selecionado na classe code-behind.


[![Comente o código-fonte no login. aspx e marcação declarativa existente](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Figura 2**: comentário horizontalmente o existente marcação declarativa e código-fonte no login. aspx ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> O comentário no ícone de linhas selecionadas não está disponível ao exibir a marcação declarativa no Visual Studio 2005. Se você não estiver usando o Visual Studio 2008, você precisará adicionar manualmente os `<%--` e `--%>` delimitadores.


Em seguida, arraste um controle de logon da caixa de ferramentas para a página e defina suas `ID` propriedade para `myLogin`. Neste ponto, sua tela deve ser semelhante à Figura 3. Observe que a interface de padrão do controle Login inclui controles de caixa de texto para o nome de usuário e senha, um lembrar-me próxima vez que caixa de seleção e um botão Log. Também há `RequiredFieldValidator` controles para as duas caixas de texto.


[![Adicionar um controle de logon para a página](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Figura 3**: adicionar um controle de logon para a página ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


E pronto! Quando Log no botão do controle de logon é clicado, ocorrerá um postback e o controle de logon chamará o `Membership.ValidateUser` método, passando o nome de usuário inserido e a senha. Se as credenciais forem inválidas, o controle de logon exibe uma mensagem informando tal. Se, no entanto, as credenciais forem válidas, o controle de logon cria os formulários de tíquete de autenticação e redireciona o usuário para a página apropriada.

O controle de logon usa quatro fatores para determinar a página apropriada para redirecionar o usuário após um logon bem-sucedido:

- Se o controle de logon é na página de logon conforme definido pelo `loginUrl` na configuração de autenticação de formulários; o valor dessa configuração padrão é `Login.aspx`
- A presença de um `ReturnUrl` parâmetro querystring
- O valor do controle de logon [ `DestinationUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- O `defaultUrl` valor especificado nos formulários de definições de configuração de autenticação; o valor dessa configuração padrão é default. aspx

A Figura 4 ilustra como o controle de logon usa essas quatro parâmetros para chegar a sua decisão de página apropriado.


[![Adicionar um controle de logon para a página](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Figura 4**: adicionar um controle de logon para a página ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


Reserve um tempo para testar o controle de logon visitando o site por meio de um navegador e fazer logon como um usuário existente no framework de associação.

Interface de renderizado do controle de logon é altamente configurável. Há uma série de propriedades que influenciam sua aparência; Além disso, o controle de logon pode ser convertido em um modelo para um controle preciso sobre o layout de elementos de interface do usuário. O restante desta etapa examina como personalizar a aparência e layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizando a aparência do controle de logon

Configurações de propriedade do controle de logon padrão renderizam uma interface do usuário com um título (Log), controles TextBox e Label para as entradas de nome de usuário e senha, um lembrar-me na próxima vez CheckBox e um botão de Log. As aparências desses elementos são configuráveis por meio das propriedades de inúmeros do controle de logon. Além disso, os elementos de interface do usuário adicionais - como um link para uma página para criar uma nova conta de usuário - podem ser adicionados, definindo uma propriedade ou dois.

Vamos gastar algum tempo para enfeitar a aparência do nosso controle de logon. Uma vez que o `Login.aspx` página já tem o texto na parte superior da página que diz que o logon, o título do controle de logon é supérfluo. Portanto, limpar o [ `TitleText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) valor para remover o título do controle de logon.

: O nome de usuário e a senha: rótulos para a esquerda dos dois controles de caixa de texto podem ser personalizados usando o [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) e [ `PasswordLabelText` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivamente. Vamos alterar o nome de usuário: rótulo para ler Username:. Os estilos de rótulo e a caixa de texto são configuráveis por meio de [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) e [ `TextBoxStyle` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivamente.

A lembrar-me propriedade de texto próxima hora da caixa de seleção pode ser definida por meio do controle de logon [ `RememberMeText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), e seu padrão verificado o estado é configurável por meio de [ `RememberMeSet` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx)(o padrão é False). Vá em frente e defina o `RememberMeSet` propriedade como True para que lembrar-me na próxima vez caixa de seleção será verificada por padrão.

O controle de logon oferece duas propriedades para ajustar o layout de seus controles de interface do usuário. O [ `TextLayout` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica se o nome de usuário: e a senha: rótulos aparecem à esquerda das suas caixas de texto correspondentes (o padrão) ou acima deles. O [ `Orientation` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica se as entradas de nome de usuário e senha estão situadas verticalmente (um acima do outro) ou horizontalmente. Vou deixar essas duas propriedades definidas como seus padrões, mas eu recomendo que você tente definir essas duas propriedades para seus valores não padrão para ver o efeito resultante.

> [!NOTE]
> Na próxima seção, configurando o Layout do controle de logon, vamos examinar usando modelos para definir o layout preciso dos elementos de interface do usuário do controle de Layout.


Encapsular as configurações de propriedade do controle de logon, definindo o [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) e [ `CreateUserUrl` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) para Not registrado ainda? Crie uma conta! e `~/Membership/CreatingUserAccounts.aspx`, respectivamente. Isso adiciona um hiperlink a interface do controle de logon que aponta para a página criada na <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-vb.md). O controle de logon [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) e [ `HelpPageUrl` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) e [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) e [ `PasswordRecoveryUrl` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funcionam da mesma maneira, renderização de links para uma página de Ajuda e uma página de recuperação de senha.

Depois de fazer essas alterações de propriedade, marcação declarativa e a aparência do seu controle de logon devem ser semelhantes ao mostrado na Figura 5.


[![Os valores de propriedades do controle Login determinam sua aparência](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Figura 5**: valores determinam sua aparência propriedades do controle de logon a ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configurando o Layout do controle de logon

Interface do usuário do controle da Web de logon padrão apresenta a interface em um HTML `<table>`. Mas e se é necessário ter maior controle sobre a saída renderizada? Talvez desejemos substituir a `<table>` com uma série de `<div>` marcas. Ou, e se o nosso aplicativo exige credenciais adicionais para autenticação? Muitos sites financeiros, por exemplo, exigem que os usuários fornecer não somente um nome de usuário e senha, mas também um número de identificação pessoal (PIN) ou outras informações de identificação. Tudo o que os motivos podem ser, é possível converter o controle de logon em um modelo, na qual podemos pode explicitamente definir marcação declarativa da interface.

Precisamos fazer duas coisas para atualizar o controle de logon para coletar credenciais adicionais:

1. Atualize a interface do controle de logon para incluir controles de Web para coletar as credenciais adicionais.
2. Substitua a lógica de autenticação interna do controle de logon para que um usuário seja autenticado apenas se o nome de usuário e a senha for válido e suas credenciais adicionais são válidos, também.

Para executar a tarefa de primeiro, precisamos converter o controle de logon em um modelo e adicione os controles de Web necessários. Para a segunda tarefa, lógica de autenticação do controle de logon pode ser substituída pela criação de um manipulador de eventos para o controle [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Vamos atualizar o controle de logon para que ele solicita aos usuários por nome de usuário, senha e endereço de email e autentica o usuário apenas se o endereço de email fornecido corresponde ao seu endereço de email no arquivo. Primeiro, precisamos converter a interface do controle de logon em um modelo. Na Smart Tag do controle de logon, escolha a converter para a opção de modelo.


[![Converter o controle de logon em um modelo](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Figura 6**: converter o controle de logon em um modelo ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Para reverter o controle de logon para sua versão de pré-template, clique no link de redefinição de Smart Tag do controle.


Convertendo o controle de logon em um modelo adiciona um `LayoutTemplate` para marcação declarativa do controle com elementos HTML e controles da Web definindo a interface do usuário. Como mostra a Figura 7, convertendo o controle em um modelo remove um número de propriedades na janela Propriedades, tais como `TitleText`, `CreateUserUrl`e assim por diante, pois esses valores de propriedade são ignorados ao usar um modelo.


[![Menos propriedades estão que disponíveis quando o controle de logon é convertido em um modelo](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Figura 7**: menos propriedades estão disponíveis quando o controle de logon é convertido em um modelo ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


A marcação HTML no `LayoutTemplate` podem ser modificados conforme necessário. Da mesma forma, fique à vontade adicionar todos os novos controles da Web para o modelo. No entanto, é importante controles de Web core desse controle Login permanecem no modelo e manter atribuído `ID` valores. Em particular, não remova ou renomeie o `UserName` ou `Password` caixas de texto, o `RememberMe` caixa de seleção, o `LoginButton` botão, o `FailureText` rótulo, ou o `RequiredFieldValidator` controles.

Para coletar o endereço de email do visitante, precisamos adicionar uma caixa de texto para o modelo. Adicione a seguinte marcação declarativa entre a linha da tabela (`<tr>`) que contém o `Password` caixa de seleção de tempo de caixa de texto e a linha da tabela que contém a lembrar-me em seguida:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Depois de adicionar o `Email` caixa de texto, visite a página por meio de um navegador. Como mostra a Figura 8, a interface do usuário do controle de logon agora inclui uma terceira caixa de texto.


[![O controle de logon agora inclui uma caixa de texto para o endereço de Email do usuário](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Figura 8**: O controle de logon agora inclui uma caixa de texto para o endereço de Email do usuário ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


Neste ponto, o controle de logon ainda está usando o `Membership.ValidateUser` método para validar as credenciais fornecidas. Do mesmo modo, o valor digitado para o `Email` caixa de texto não tem nenhuma relevância em se o usuário pode fazer logon. Na etapa 3 vamos examinar como substituir a lógica de autenticação do controle de logon para que as credenciais só são consideradas válidas se o nome de usuário e senha são válidos e o endereço de email fornecido correspondências com o endereço de email no arquivo.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Etapa 3: Modificando a lógica de autenticação de logon do controle

Quando um visitante fornece dela credenciais e cliques que no botão logon, um postback massacre e o logon de controle avança por meio de seu fluxo de trabalho de autenticação. O fluxo de trabalho é iniciado, gerando o [ `LoggingIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Os manipuladores de evento associados com esse evento podem cancelar o log de operação definindo as `e.Cancel` propriedade para `True`.

Se o log de operação não for cancelado, o fluxo de trabalho progride, gerando o [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Se não houver um manipulador de eventos para o `Authenticate` evento, em seguida, ele é responsável por determinar se as credenciais fornecidas são válidas ou não. Se nenhum manipulador de eventos for especificado, o controle de logon usa o `Membership.ValidateUser` método para determinar a validade das credenciais.

Se as credenciais fornecidas são válidas e, em seguida, o tíquete de autenticação de formulários é criado, o [ `LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) é gerado, e o usuário é redirecionado à página apropriada. Se, no entanto, as credenciais são consideradas inválidas, o [ `LoginError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) é gerado e será exibida uma mensagem informando ao usuário que suas credenciais eram inválidas. Por padrão, em caso de falha de logon controle simplesmente define seu `FailureText` rótulo de propriedade de texto do controle a uma mensagem de falha (a tentativa de logon não foi bem-sucedida. Tente novamente). No entanto, se o controle de logon [ `FailureAction` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) é definido como `RedirectToLoginPage`, em seguida, os problemas de controle de logon um `Response.Redirect` para a página de logon, acrescentando o parâmetro querystring `loginfailure=1` (que faz com que o logon controle para exibir a mensagem de falha).

Figura 9 oferece um fluxograma de fluxo de trabalho de autenticação.


[![Fluxo de trabalho de autenticação de logon do controle](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Figura 9**: fluxo de trabalho do controle de logon a autenticação ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Se você estiver se perguntando quando você usaria o `FailureAction`do `RedirectToLogin` página de opção, considere o cenário a seguir. Agora nosso `Site.master` página mestra atualmente tem o texto Hello, stranger exibido na coluna à esquerda quando visitado por um usuário anônimo, mas imagine que queremos substituir esse texto com um controle de logon. Isso permitiria que um usuário anônimo fazer logon em qualquer página no site, em vez de exigir que eles para visitar a página de logon diretamente. No entanto, se um usuário não pôde fazer logon por meio do controle Login renderizado pela página mestre, ele pode fazer sentido redirecioná-las para a página de logon (`Login.aspx`) porque essa página provavelmente inclui instruções adicionais, links e outras ajuda – como links para criar um nova conta ou recuperar uma senha perdida - que não foram adicionados à página mestra.


### <a name="creating-theauthenticateevent-handler"></a>Criando o`Authenticate`manipulador de eventos

Para conectar nossa lógica de autenticação personalizada, precisamos criar um manipulador de eventos para o controle de Login `Authenticate` eventos. Criando um manipulador de eventos para o `Authenticate` evento gerará a seguinte definição de manipulador de eventos:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Como você pode ver, o `Authenticate` manipulador de eventos recebe um objeto do tipo [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) como seu segundo parâmetro de entrada. O `AuthenticateEventArgs` classe contém uma propriedade booleana chamada `Authenticated` que é usado para especificar se as credenciais fornecidas são válidas. Nossa tarefa, em seguida, é escrever o código que determina se as credenciais fornecidas são válidas ou não e para definir o `e.Authenticate` propriedade adequadamente.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinando e validar as credenciais fornecidas

Use o controle de logon [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) e [ `Password` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) para determinar as credenciais de nome de usuário e senha inseridas pelo usuário. Para determinar os valores inseridos em todos os controles da Web adicionais (como o `Email` caixa de texto que adicionamos na etapa anterior), use `LoginControlID.FindControl`("*`controlID`*") para obter uma referência de programação na Web controle no modelo cuja `ID` propriedade é igual a *`controlID`*. Por exemplo, para obter uma referência para o `Email` caixa de texto, use o seguinte código:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Para validar as credenciais do usuário, precisamos fazer duas coisas:

1. Certifique-se de que o nome de usuário fornecido e a senha são válidos
2. Certifique-se de que o endereço de email inserido corresponde ao endereço de email no arquivo para o usuário tentar fazer logon no

Para realizar a primeira verificação, podemos simplesmente usar o `Membership.ValidateUser` método, como vimos na etapa 1. A segunda verificação, precisamos determinar o endereço de email do usuário para que podemos pode compará-lo para o endereço de email que eles inseriram no controle TextBox. Para obter informações sobre um usuário específico, use o `Membership` da classe [ `GetUser` método](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

O `GetUser` método tem várias sobrecargas. Se usado sem passar todos os parâmetros, ele retorna informações sobre o usuário conectado no momento. Para obter informações sobre um determinado usuário, chame `GetUser` passando o nome de usuário. De qualquer forma `GetUser` retorna um [ `MembershipUser` objeto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que tem propriedades como `UserName`, `Email`, `IsApproved`, `IsOnline`e assim por diante.

O código a seguir implementa essas duas verificações. Se ambos, em seguida, passarem `e.Authenticate` é definido como `True`, caso contrário, ele é atribuído `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Com esse código funcionando, tente fazer logon como um usuário válido, inserindo o nome de usuário correto, senha e endereço de email. Tente novamente, mas desta vez propositadamente usar um endereço de email incorreto (consulte a Figura 10). Por fim, tente uma terceira vez usando um nome de usuário inexistente. No primeiro caso você deve estar com êxito conectado ao site, mas nos dois últimos casos, você deve ver a mensagem de credenciais inválidas do controle de logon.


[![Tito não consegue fazer logon ao fornecer um endereço de Email incorreto](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Figura 10**: Tito não é possível Log em quando fornecendo um endereço de Email incorreto ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> Conforme discutido na seção como a associação do Framework manipula inválido tentativas de logon na etapa 1, quando o `Membership.ValidateUser` método é chamado e passado credenciais inválidas, ele controla a tentativa de logon inválido e impeça o usuário se eles excederem um determinado limite de tentativas inválidas de dentro de uma janela de tempo especificado. Desde nossas chamadas de lógica de autenticação personalizada a `ValidateUser` método, uma senha incorreta para um nome de usuário válido incrementará o contador de tentativas de logon inválidas, mas esse contador não é incrementado no caso em que o nome de usuário e senha são válidos, mas o endereço de email está incorreto. Provavelmente, esse comportamento é adequado, pois é improvável que um hacker souber o nome de usuário e senha mas precisa usar técnicas de força bruta para determinar o endereço de email do usuário.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Etapa 4: Melhorando a mensagem de credenciais inválidas de logon do controle

Quando um usuário tenta fazer logon com credenciais inválidas, o controle de logon exibe uma mensagem explicando que a tentativa de logon foi malsucedida. Em particular, o controle exibe a mensagem especificada pelo seu [ `FailureText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), que tem um valor padrão de sua tentativa de logon não foi bem-sucedida. Tente novamente.

Lembre-se de que há muitas razões por que as credenciais do usuário podem ser inválidas:

- O nome de usuário pode não existir
- O nome de usuário existe, mas a senha é inválida
- O nome de usuário e senha são válidos, mas o usuário ainda não aprovado
- O nome de usuário e senha são válidos, mas o usuário está bloqueado out (provavelmente porque eles excederam o número de tentativas de logon inválido no período de tempo especificado)

E pode haver outros motivos ao usar a lógica de autenticação personalizada. Por exemplo, com o código que escrevemos na etapa 3, o nome de usuário e senha pode ser válida, mas o endereço de email pode estar incorreto.

Por que as credenciais são inválidas, independentemente do controle de logon exibe a mesma mensagem de erro. Essa falta de comentários pode ser confusa para um usuário cuja conta ainda não foram aprovada ou que foi bloqueada. Com um pouco de trabalho, no entanto, podemos ter o controle Login exibirá uma mensagem mais apropriada.

Sempre que um usuário tenta fazer logon com credenciais inválidas, o controle de logon gera seu `LoginError` eventos. Vá em frente e crie um manipulador de eventos para esse evento e adicione o seguinte código:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

O código acima inicia, definindo o controle de logon `FailureText` propriedade para o valor padrão (a tentativa de logon não foi bem-sucedida. Tente novamente). Em seguida, ele verifica para ver se o nome de usuário fornecido é mapeado para uma conta de usuário. Se assim, ele consulta resultante `MembershipUser` do objeto `IsLockedOut` e `IsApproved` propriedades para determinar se a conta foi bloqueada ou se ainda não foram aprovada. Em ambos os casos, o `FailureText` propriedade é atualizada em um valor correspondente.

Para testar esse código, propositadamente tente fazer logon como um usuário existente, mas usar uma senha incorreta. Faça isso cinco vezes em uma linha em um período de 10 minutos e a conta será bloqueada. Conforme mostrado na Figura 11, logon subsequentes tentativas sempre irá falhar (até mesmo com a senha correta), mas agora exibe o mais descritivo sua conta foi bloqueada devido a muitas tentativas de logon inválido. Entre em contato com o administrador para configurar sua mensagem desbloqueada da conta.


[![Tito executada muitas tentativas de logon inválidas e foi bloqueada](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Figura 11**: Tito executadas muito muitas tentativas inválidas de logon e tem sido bloqueados ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Resumo

Anteriores neste tutorial, nossa página de logon validadas as credenciais fornecidas em relação a uma lista embutido em código de pares de nome de usuário e senha. Neste tutorial, atualizamos a página para validar as credenciais em relação a estrutura de associação. Na etapa 1 analisamos usando o `Membership.ValidateUser` método programaticamente. Na etapa 2 substituímos nossa interface do usuário criado manualmente e o código com o controle de logon.

O controle de logon apresenta uma interface do usuário de logon padrão e valida automaticamente as credenciais do usuário em relação a estrutura de associação. Além disso, no caso de credenciais válidas, o controle de logon se conecta o usuário por meio da autenticação de formulários. Em resumo, uma experiência de usuário de logon totalmente funcional está disponível, simplesmente arrastando o controle de logon para uma página, nenhuma marcação declarativa extra ou o código necessário. O que é mais, o controle de logon é altamente personalizável, permitindo um alto grau de controle sobre ambos os lógica de autenticação e a interface do usuário renderizado.

Neste momento os visitantes do nosso site podem criar uma nova conta de usuário e o log para o site, mas ainda precisamos examinar restringindo o acesso a páginas com base no usuário autenticado. Atualmente, qualquer usuário autenticado ou anônimo, pode exibir qualquer página em nosso site. Além de controlar o acesso a páginas do nosso site em uma base por usuário, podemos ter determinadas páginas cuja funcionalidade depende do usuário. O próximo tutorial aborda como limitar o acesso e funcionalidade na página com base no usuário conectado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Exibir mensagens personalizadas para bloqueada e os usuários não-aprovadas](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examinando o ASP.NET da 2.0 associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como: Criar uma página de logon do ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentação técnica do controle de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Usando os controles de logon](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy e Michael Olivero. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-user-accounts-vb.md)
> [Próximo](user-based-authorization-vb.md)
