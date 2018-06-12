---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Validando credenciais de usuário no repositório do usuário de associação (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como validar as credenciais do usuário em relação ao armazenamento de usuário de associação usando meios programáticos e o controle de logon...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: 484a0f16265ee2d887ee08f6ae7ada47047f1f04
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30891800"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Validando credenciais de usuário no repositório do usuário de associação (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> Neste tutorial, examinaremos como validar as credenciais do usuário em relação ao armazenamento de usuário de associação usando meios programáticos e o controle de logon. Também examinaremos como personalizar a aparência e o comportamento do controle de logon.


## <a name="introduction"></a>Introdução

No <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-cs.md) vimos como criar uma nova conta de usuário na estrutura de associação. Primeiro, examinamos programaticamente criar contas de usuário por meio de `Membership` da classe `CreateUser` método e, em seguida, examinado usando o controle CreateUserWizard Web. No entanto, a página de logon atualmente valida as credenciais fornecidas com uma lista codificada de pares de nome de usuário e senha. É necessário atualizar a lógica da página de logon para que ele valida as credenciais no repositório do usuário da estrutura de associação.

Muito semelhante com a criação de contas de usuário, as credenciais podem ser validadas programaticamente ou declarativamente. A API de associação inclui um método para validar programaticamente credenciais do usuário em relação ao armazenamento do usuário. E o ASP.NET é fornecido com o controle da Web de logon, que renderiza uma interface do usuário com caixas de texto para o nome de usuário e senha e um botão para fazer logon.

Neste tutorial, examinaremos como validar as credenciais do usuário em relação ao armazenamento de usuário de associação usando meios programáticos e o controle de logon. Também examinaremos como personalizar a aparência e o comportamento do controle de logon. Vamos começar!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Etapa 1: Validar as credenciais no repositório do usuário de associação

Para sites que usam a autenticação de formulários, um usuário faz logon para o site visitando uma página de logon e inserir suas credenciais. Essas credenciais são comparadas em relação ao armazenamento do usuário. Se eles forem válidos, o usuário recebe um tíquete de autenticação de formulários, que é um token de segurança que indica a identidade e a autenticidade do visitante.

Para validar um usuário em relação a estrutura de associação, use o `Membership` da classe [ `ValidateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). O `ValidateUser` método usa dois parâmetros de entrada - *`username`* e *`password`* - e retorna um valor booliano que indica se as credenciais foram válidas. Como com o `CreateUser` método examinamos no tutorial anterior, o `ValidateUser` método delega a validação real para o provedor de associação configurado.

O `SqlMembershipProvider` valida as credenciais fornecidas ao obter a senha do usuário especificado por meio de `aspnet_Membership_GetPasswordWithFormat` procedimento armazenado. Lembre-se de que o `SqlMembershipProvider` armazena as senhas dos usuários usando um dos três formatos: clear, criptografia ou hash. O `aspnet_Membership_GetPasswordWithFormat` procedimento armazenado retorna a senha em seu formato bruto. Para senhas criptografadas ou hash, o `SqlMembershipProvider` transforma o *`password`* valor passado para o `ValidateUser` método para seu equivalente criptografados ou com hash estado e, em seguida, compara com o que foi retornado da banco de dados. Se a senha armazenada no banco de dados corresponde a formatado senha inserida pelo usuário, as credenciais são válidas.

Vamos atualizar nossa página de logon (~ /`Login.aspx`) para que ele valida as credenciais fornecidas no repositório de usuário de associação do framework. Criamos essa página de logon na <a id="Tutorial02"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, criando uma interface com duas caixas de texto para o nome de usuário e senha, um Lembrar na caixa de seleção e um botão de Login (consulte a Figura 1). O código valida as credenciais inseridas em uma lista codificada de pares de nome de usuário e senha (Scott/Jisun/senha, senha e Sam /). No <a id="Tutorial03"> </a> [ *configuração de autenticação de formulários e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutorial atualizamos o código da página de logon para armazenar informações adicionais nos formulários tíquete de autenticação `UserData` propriedade.


[![Interface a página de logon inclui duas caixas de texto, um CheckBoxList e um botão](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figura 1**: Interface inclui duas caixas de texto da página de logon a, um CheckBoxList e um botão ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Interface do usuário da página de logon pode permanecer inalterada, mas é preciso substituir o botão de logon `Click` manipulador de eventos com o código que valida o usuário no repositório de usuário de associação do framework. Atualize o manipulador de eventos, de modo que seu código aparece da seguinte maneira:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Esse código é extremamente simple. Vamos começar chamando o `Membership.ValidateUser` método, passando o nome de usuário fornecido e a senha. Se esse método retornar true, o usuário está conectado ao site por meio de `FormsAuthentication` RedirectFromLoginPage de método classe do. (Conforme abordado no <a id="Tutorial2"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, o `FormsAuthentication.RedirectFromLoginPage` cria formulários de tíquete de autenticação e, em seguida, redireciona o usuário para a página apropriada.) Se as credenciais são inválidas, no entanto, o `InvalidCredentialsMessage` rótulo é exibido, informando ao usuário seu nome de usuário ou senha estava incorreta.

Isso é tudo que é necessário para que ele!

Para testar se a página de logon funciona conforme esperado, tente fazer logon com uma das contas de usuário que você criou no tutorial anterior. Ou, se você ainda não tiver criado uma conta, vá em frente e criar um a partir de `~/Membership/CreatingUserAccounts.aspx` página.

> [!NOTE]
> Quando o usuário insere suas credenciais e envia o formulário da página de logon, as credenciais, incluindo sua senha são transmitidas pela Internet para o servidor web no *texto sem formatação*. Isso significa que qualquer hacker detecção o tráfego de rede pode ver o nome de usuário e senha. Para evitar isso, é essencial para criptografar o tráfego de rede usando [camadas de soquete seguro (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Isso garantirá que as credenciais (bem como uma marcação HTML da página inteira) é criptografada desde o momento em que ele deixar o navegador até que eles sejam recebidos pelo servidor web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Como a estrutura de associação trata a tentativas inválidas de logon

Quando um visitante chega à página de logon e envia suas credenciais, o navegador faz uma solicitação HTTP para a página de logon. Se as credenciais forem válidas, a resposta HTTP inclui o tíquete de autenticação em um cookie. Portanto, um hacker tentar invadir o seu site pode criar um programa que exaustivamente envia solicitações HTTP para a página de logon com um nome de usuário válido e uma estimativa de senha. Se a estimativa de senha está correta, a página de logon retornará o cookie de tíquete de autenticação, no ponto em que o programa sabe falhou após um par de nome de usuário/senha válido. Por força bruta, esse programa pode ser capaz de se deparar com uma senha de usuário, especialmente se a senha é fraca.

Para evitar tais ataques de força bruta, a estrutura de associação bloqueia um usuário se houver um determinado número de tentativas de logon sem êxito dentro de um determinado período de tempo. Os parâmetros exatos são configuráveis por meio de dois parâmetros de configuração de provedor de associação:

- `maxInvalidPasswordAttempts` -Especifica a senha inválida quantas tentativas são permitidas para o usuário dentro do período de tempo antes que a conta está bloqueada. O valor padrão é 5.
- `passwordAttemptWindow` -indica o período de tempo em minutos durante o qual o número especificado de tentativas de logon inválidas fará com que a conta seja bloqueada. O valor padrão é 10.

Se um usuário estiver bloqueado, ela não é possível fazer logon até que um administrador a desbloqueie sua conta. Quando um usuário está bloqueado, o `ValidateUser` método será *sempre* retornar `false`, mesmo se as credenciais válidas são fornecidas. Embora esse comportamento reduz a probabilidade de que um hacker invadir o seu site por meio de métodos de força bruta, ele pode acabar bloquear um usuário válido que simplesmente esqueceu sua senha ou acidentalmente tem a tecla Caps Lock no ou está tendo um dia de digitação incorreto.

Infelizmente, não há nenhuma ferramenta interna para desbloquear uma conta de usuário. Para desbloquear uma conta, você pode modificar o banco de dados diretamente - altere o `IsLockedOut` campo o `aspnet_Membership` de tabela para a conta de usuário apropriada - ou criar uma interface baseada na web que lista as contas com opções para desbloqueá-los bloqueada. Vamos examinar Criando interfaces administrativo para realizar tarefas comuns relacionadas a conta e função de usuário em um tutorial futuras.

> [!NOTE]
> Uma desvantagem do `ValidateUser` método é que quando as credenciais fornecidas são inválidas, ele não fornece nenhuma explicação por que. As credenciais podem ser inválidas porque não há nenhum par de nome de usuário/senha correspondente no repositório do usuário, ou porque o usuário ainda não foram aprovado, ou porque o usuário foi bloqueado. Na etapa 4, veremos como mostrar uma mensagem mais detalhada para o usuário quando sua tentativa de logon falhar.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Etapa 2: Coletar credenciais por meio do controle da Web de logon

O [controle da Web de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) renderiza uma interface de usuário padrão muito semelhante ao que criamos no <a id="SKM5"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial. Usando o controle de logon salva o trabalho de ter que criar a interface para coletar as credenciais de s visitante. Além disso, o controle de logon automaticamente entra no usuário (supondo que as credenciais enviadas são válidas), assim, salvando nos precisar gravar qualquer código.

Vamos atualizar `Login.aspx`, substituindo a interface criada manualmente e de código com um controle de logon. Iniciar, removendo a marcação existente e o código no `Login.aspx`. Você pode excluí-lo imediatamente, ou simplesmente comente-o. Para comentar declarativo, coloque-o com o `<%--` e `--%>` delimitadores. Você pode inserir esses delimitadores manualmente ou, como mostra a Figura 2, você pode selecionar o texto para comentar e, em seguida, clique em de comentários sobre o ícone de linhas selecionadas na barra de ferramentas. Da mesma forma, você pode usar os comentários sobre o ícone de linhas selecionadas para comentar o código selecionado na classe code-behind.


[![Comente a marcação declarativa existente e o código-fonte em aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figura 2**: comentário Out a existente declarativa marcação e código-fonte em `Login.aspx` ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Os comentários sobre o ícone de linhas selecionadas não está disponível ao exibir a marcação declarativa no Visual Studio 2005. Se você não estiver usando o Visual Studio 2008, será necessário adicionar manualmente o `<%--` e `--%>` delimitadores.


Em seguida, arraste um controle de logon da caixa de ferramentas para a página e defina seu `ID` propriedade `myLogin`. Neste ponto, sua tela deve ser semelhante à Figura 3. Observe que a interface de padrão de controle de logon inclui controles de caixa de texto para o nome de usuário e senha, um lembrar-me tempo próximo caixa de seleção e em um botão de Log. Também há `RequiredFieldValidator` controles para as duas caixas de texto.


[![Adicionar um controle de logon para a página](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figura 3**: adicionar um controle de logon para a página ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


E pronto! Quando Log no botão do controle de logon é clicado, ocorrerá um postback e o controle de logon chamará o `Membership.ValidateUser` método, passando o nome de usuário inserido e a senha. Se as credenciais são inválidas, o controle de logon exibe uma mensagem informando tal. Se, no entanto, as credenciais forem válidas, o controle de logon cria tíquete de autenticação de formulários e redireciona o usuário para a página apropriada.

O controle de logon usa quatro fatores para determinar a página apropriada para redirecionar o usuário após um logon bem-sucedido:

- Se o controle de logon está na página de logon conforme definido pelo `loginUrl` configuração na configuração de autenticação de formulários; o valor dessa configuração padrão é `Login.aspx`
- A presença de um `ReturnUrl` parâmetro querystring
- O valor do controle do logon [ `DestinationUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- O `defaultUrl` valor especificado em formulários de definições de configuração de autenticação; o valor padrão dessa configuração é `Default.aspx`

A Figura 4 ilustra como o controle de logon usa essas quatro parâmetros para chegar a sua decisão de página apropriada.


[![Adicionar um controle de logon para a página](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figura 4**: adicionar um controle de logon para a página ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Dedicar um tempo para testar o controle de logon visitando o site por meio de um navegador e fazer logon como um usuário existente na estrutura de associação.

Interface renderizado do controle de logon é altamente configurável. Há um número de propriedades que influenciam sua aparência; Além disso, o controle de logon pode ser convertido em um modelo para um controle preciso sobre o layout de elementos de interface do usuário. O restante desta etapa examina como personalizar a aparência e o layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizando a aparência do controle de logon

Configurações de propriedade do controle de logon padrão renderizam uma interface do usuário com um título (Log In), controles de caixa de texto e o rótulo para as entradas de nome de usuário e senha, um lembrar-me na próxima vez caixa de seleção e um botão de logon. A aparência desses elementos é configurável por meio de várias propriedades do controle de logon. Além disso, os elementos de interface de usuário adicionais - como um link para uma página para criar uma nova conta de usuário - podem ser adicionados ao definir uma propriedade ou dois.

Vamos alguns instantes para enfeitar a aparência de nosso controle de logon. Desde o `Login.aspx` página já tem o texto na parte superior da página que diz o logon, o título do controle de logon é supérfluo. Portanto, limpar o [ `TitleText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) valor para remover o título do controle de logon.

O nome de usuário: e a senha: rótulos à esquerda dos dois controles de caixa de texto podem ser personalizados por meio de [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) e [ `PasswordLabelText` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivamente. Vamos alterar o nome de usuário: rótulo ao ler o nome de usuário:. Os estilos de caixa de texto e o rótulo são configuráveis por meio de [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) e [ `TextBoxStyle` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivamente.

A lembrar-me a propriedade de texto próxima hora da caixa de verificação pode ser definida por meio do controle de logon [ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), e seu padrão verificado o estado é configurável por meio de [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (que padrão é falso). Vá em frente e defina o `RememberMeSet` propriedade como True para que lembrar-me na próxima vez caixa de seleção será verificada por padrão.

O controle de logon oferece duas propriedades para ajustar o layout de seus controles de interface de usuário. O [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica se o nome de usuário: e a senha: rótulos aparecem à esquerda de suas caixas de texto correspondentes (o padrão) ou acima deles. O [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica se as entradas de nome de usuário e senha estão situadas verticalmente (uns sobre os outros) ou horizontalmente. Vou deixar essas duas propriedades definidas como seus padrões, mas você deve tentar definir essas duas propriedades para seus valores não padrão para ver o efeito resultante.

> [!NOTE]
> Na próxima seção, configurando o Layout do controle de logon, examinaremos usando modelos para definir o layout preciso dos elementos de interface de usuário do controle de Layout.


Concluir as configurações de propriedade do controle de logon definindo o [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) e [ `CreateUserUrl` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) para não registrada ainda? Crie uma conta! e `~/Membership/CreatingUserAccounts.aspx`, respectivamente. Isso adiciona um hiperlink à interface do controle de logon apontando para a página criada no <a id="SKM6"> </a> [tutorial anterior](creating-user-accounts-cs.md). O controle de logon [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) e [ `HelpPageUrl` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) e [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) e [ `PasswordRecoveryUrl` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funcionam da mesma maneira, renderização de links para uma página de Ajuda e uma página de recuperação de senha.

Depois de fazer essas alterações de propriedade, declarativo e a aparência de seu controle de logon devem ser semelhantes ao mostrado na Figura 5.


[![Valores de propriedades do controle de logon, determinam a aparência](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figura 5**: valores determinam sua aparência propriedades do controle de logon a ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configurando o Layout do controle de logon

Interface de usuário do controle da Web de logon padrão apresenta a interface em um HTML `<table>`. Mas e se é preciso ter maior controle sobre a saída renderizada? Talvez desejemos substituir o `<table>` com uma série de `<div>` marcas. Ou, e se o nosso aplicativo exige credenciais adicionais para autenticação? Muitos sites financeiros, por exemplo, exigem que os usuários fornecer não apenas um nome de usuário e senha, mas também um número de identificação pessoal (PIN) ou outras informações de identificação. Tudo o que os motivos podem ser, é possível converter o controle de logon em um modelo, da qual é possível definir explicitamente declarativo da interface.

Precisamos fazer duas coisas para atualizar o controle de logon para coletar credenciais adicionais:

1. Atualize a interface do controle de logon para incluir o controle da Web para coletar as credenciais adicionais.
2. Substitua a lógica de autenticação interna do controle de logon para que um usuário é autenticado somente se o nome de usuário e a senha é válida e suas credenciais adicionais são válidos, muito.

Para executar a tarefa primeiro, precisamos converter o controle de logon em um modelo e adicione os controles da Web necessários. Para a segunda tarefa, lógica de autenticação do controle de logon pode ser substituída por criar um manipulador de eventos para o controle [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Vamos atualizar o controle de logon para que ele solicita que os usuários para seu nome de usuário, senha e endereço de email e autentica o usuário apenas se o endereço de email fornecido corresponde a seu endereço de email no arquivo. Primeiro, precisamos converter a interface do controle de logon em um modelo. Marca inteligente do controle de logon, escolha a converter para a opção de modelo.


[![Converter o controle de logon em um modelo](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figura 6**: converter o controle de logon em um modelo ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Para reverter o controle de logon com sua versão de pré-template, clique no link de redefinição de marca inteligente do controle.


Convertendo o controle de logon em um modelo adiciona um `LayoutTemplate` para a marcação do controle declarativa com elementos HTML e controles da Web definindo a interface do usuário. Como mostra a Figura 7, convertendo o controle em um modelo remove um número de propriedades na janela Propriedades, como `TitleText`, `CreateUserUrl`e assim por diante, desde que esses valores de propriedade são ignorados ao usar um modelo.


[![Menos propriedades estão que disponíveis quando o controle de logon é convertido em um modelo](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figura 7**: menos propriedades estão disponíveis quando o controle de logon é convertido em um modelo ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


A marcação HTML no `LayoutTemplate` podem ser modificados conforme necessário. Da mesma forma, fique à vontade para adicionar novos controles da Web para o modelo. No entanto, é importante controles de Web core desse controle logon permanecem no modelo e manter atribuído `ID` valores. Em particular, não remova ou renomeie o `UserName` ou `Password` caixas de texto, o `RememberMe` caixa de seleção, o `LoginButton` botão, o `FailureText` rótulo, ou o `RequiredFieldValidator` controles.

Para coletar o endereço de email do visitante, é preciso adicionar uma caixa de texto para o modelo. Adicione a seguinte marcação declarativa entre a linha da tabela (`<tr>`) que contém o `Password` caixa de texto e a linha da tabela que contém lembrar-me próximo tempo de caixa de seleção:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Depois de adicionar o `Email` caixa de texto, visite a página por meio de um navegador. Como mostra a Figura 8, a interface do usuário do controle de logon agora inclui uma terceira caixa de texto.


[![O controle de logon agora inclui uma caixa de texto para o endereço de Email do usuário](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figura 8**: O controle de logon agora inclui uma caixa de texto para o endereço de Email do usuário ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


Neste ponto, o controle de logon ainda está usando o `Membership.ValidateUser` método para validar as credenciais fornecidas. De forma correspondente, o valor digitado para o `Email` caixa de texto não tem efeito sobre se o usuário pode fazer logon. Na etapa 3, examinaremos como substituir uma lógica de autenticação do controle de logon para que as credenciais só são consideradas válidas se o nome de usuário e senha são válidos e o endereço de email fornecido coincide com o endereço de e-mail no arquivo.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Etapa 3: Modificando a lógica de autenticação de logon do controle

Quando um visitante fornece sua credenciais e clica que no botão logon, um postback tem lugar e o logon do controlam é feita por meio de seu fluxo de trabalho de autenticação. O fluxo de trabalho é iniciado, gerando o [ `LoggingIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Os manipuladores de evento associados ao evento podem cancelar o log de operação definindo o `e.Cancel` propriedade `true`.

Se o log de operação não for cancelado, o fluxo de trabalho progride, gerando o [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Se não houver um manipulador de eventos para o `Authenticate` evento, ele é responsável por determinar se as credenciais fornecidas são válidas ou não. Se nenhum manipulador de eventos for especificado, o controle de logon usa o `Membership.ValidateUser` método para determinar a validade das credenciais.

Se as credenciais fornecidas são válidas e, em seguida, o tíquete de autenticação de formulários é criado, o [ `LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) é gerado, e o usuário é redirecionado à página apropriada. Se, no entanto, as credenciais são consideradas inválidas, o [ `LoginError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) é gerado e será exibida uma mensagem informando ao usuário que suas credenciais eram inválidas. Por padrão, em caso de falha de logon controle simplesmente define seu `FailureText` rótulo de propriedade de texto do controle a uma mensagem de falha (tentativa de logon não foi bem-sucedida. Tente novamente). No entanto, se o controle de logon [ `FailureAction` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) é definido como `RedirectToLoginPage`, em seguida, os problemas de controle de logon um `Response.Redirect` a página de logon, acrescentando o parâmetro querystring `loginfailure=1` (que faz com que o logon controle para exibir a mensagem de falha).

Figura 9 oferece um gráfico de fluxo do fluxo de trabalho de autenticação.


[![Fluxo de trabalho de autenticação de logon do controle](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figura 9**: fluxo de trabalho do controle de logon a autenticação ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Se você estiver se perguntando quando você usaria o `FailureAction`do `RedirectToLogin` página opção, considere o cenário a seguir. Agora nosso `Site.master` página mestra atualmente tem o texto, Olá, stranger exibido na coluna esquerda quando visitado por um usuário anônimo, mas imagine que queremos substituir texto com um controle de logon. Isso permitiria que um usuário anônimo fazer logon em qualquer página no site, em vez de exigir que eles para visitar a página de logon diretamente. No entanto, se um usuário não pôde fazer logon por meio de controle de logon renderizado pela página mestre, ele pode fazer sentido redirecioná-los para a página de logon (`Login.aspx`) porque a página provavelmente inclui instruções adicionais, links e outros ajuda - como links para criar um nova conta ou recuperar uma senha perdida - que não foram adicionados para a página mestra.


### <a name="creating-theauthenticateevent-handler"></a>Criando o`Authenticate`manipulador de eventos

Para conectar-se em nossa lógica de autenticação personalizada, precisamos criar um manipulador de eventos para o controle de logon `Authenticate` eventos. Criar um manipulador de eventos para o `Authenticate` evento gerará a seguinte definição de manipulador de eventos:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Como você pode ver, o `Authenticate` manipulador de eventos é passado um objeto do tipo [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) como seu segundo parâmetro de entrada. O `AuthenticateEventArgs` classe contém uma propriedade booleana chamada `Authenticated` que é usado para especificar se as credenciais fornecidas são válidas. Nossa tarefa, em seguida, é escrever código aqui que determina se as credenciais fornecidas são válidas ou não e para definir o `e.Authenticate` propriedade adequadamente.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinando e validar as credenciais fornecidas

Use o controle de logon [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) e [ `Password` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) para determinar as credenciais de usuário e senha inseridas pelo usuário. Para determinar os valores inseridos em todos os controles da Web adicionais (como o `Email` caixa de texto são adicionados na etapa anterior), use *`LoginControlID`* `.FindControl`("*`controlID`*") para obter uma programação de referência para o controle da Web no modelo cujo `ID` propriedade é igual a *`controlID`*. Por exemplo, para obter uma referência para o `Email` caixa de texto, use o seguinte código:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Para validar as credenciais do usuário que precisamos fazer duas coisas:

1. Certifique-se de que o nome de usuário fornecido e a senha são válidos
2. Certifique-se de que o endereço de email inserido corresponde ao endereço de email no arquivo para o usuário tentar fazer logon no

Para realizar a primeira verificação podemos simplesmente usar a `Membership.ValidateUser` método que vimos na etapa 1. Para a verificação de segundo, é preciso determinar o endereço de email do usuário para que podemos pode compará-lo com o endereço de email que inseriram no controle de caixa de texto. Para obter informações sobre um usuário específico, use o `Membership` da classe [ `GetUser` método](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

O `GetUser` método tem várias sobrecargas. Se usado sem passar os parâmetros, ele retornará informações sobre o usuário conectado no momento. Para obter informações sobre um usuário específico, chame `GetUser` transmitindo seu nome de usuário. De qualquer forma, `GetUser` retorna um [ `MembershipUser` objeto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que tem propriedades como `UserName`, `Email`, `IsApproved`, `IsOnline`e assim por diante.

O código a seguir implementa essas duas verificações. Se ambos, em seguida, passarem `e.Authenticate` é definido como `true`, caso contrário, ele é atribuído `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Com esse código, tente fazer logon como um usuário válido, digitando o nome de usuário correto, a senha e o endereço de email. Tente novamente, mas desta vez intencionalmente use um endereço de email incorreto (consulte a Figura 10). Por fim, tente uma terceira vez usando um nome de usuário inexistente. No primeiro caso você deve estar com êxito conectado ao site, mas nos últimos dois casos, você deve ver a mensagem de credenciais inválidas do controle de logon.


[![Tito não pode efetuar login ao fornecer um endereço de Email incorreto](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figura 10**: Tito não é possível Log em quando fornecendo um endereço de Email incorreto ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Como discutido na seção na etapa 1, como a associação do Framework manipula inválido tentativas de logon quando o `Membership.ValidateUser` método é chamado e passado credenciais inválidas, ele controla a tentativa de logon inválido e impeça o usuário se ele excederem um determinado limite de tentativas inválidas dentro de uma janela de tempo especificado. Desde o nosso chamadas de lógica de autenticação personalizada a `ValidateUser` método, uma senha incorreta para um nome de usuário válido incrementar o contador de tentativas de logon inválidas, mas esse contador não é incrementado no caso em que o nome de usuário e senha são válidos, mas o endereço de email está incorreto. A probabilidade é, esse comportamento é adequado, pois é improvável que um hacker será saber o nome de usuário e senha, mas precisa usar técnicas de força bruta para determinar o endereço de email do usuário.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Etapa 4: Melhorando a mensagem de credenciais inválidas do controle de logon

Quando um usuário tenta fazer logon com credenciais inválidas, o controle de logon exibe uma mensagem explicando que a tentativa de logon foi bem sucedida. Em particular, o controle exibe a mensagem especificada pelo seu [ `FailureText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), que tem um valor padrão de tentativa de logon não foi bem-sucedida. Tente novamente.

Lembre-se de que há muitas razões por que as credenciais do usuário podem ser inválidas:

- O nome de usuário pode não existir
- O nome de usuário existe, mas a senha é inválida
- O nome de usuário e senha são válidos, mas o usuário ainda não aprovado
- O nome de usuário e senha são válidos, mas o usuário é bloqueado out (provavelmente porque eles excederam o número de tentativas de logon inválido no período de tempo especificado)

E pode haver outros motivos ao usar a lógica de autenticação personalizada. Por exemplo, com o código escrevemos na etapa 3, o nome de usuário e senha pode ser válida, mas o endereço de email pode estar incorreto.

Por que as credenciais são inválidas, independentemente do controle de logon exibe a mesma mensagem de erro. Esta falta de comentários pode ser confusa para um usuário cuja conta ainda não foi aprovada ou que foi bloqueada. Com um pouco de trabalho, no entanto, nós pode ter o controle de logon exiba uma mensagem mais apropriada.

Sempre que um usuário tenta fazer logon com credenciais inválidas, o controle de logon gera seu `LoginError` eventos. Vá em frente e crie um manipulador de eventos para esse evento e adicione o seguinte código:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Inicia o código acima, definindo o controle de logon `FailureText` propriedade para o valor padrão (tentativa de logon não foi bem-sucedida. Tente novamente). Em seguida, ele verifica para ver se o nome de usuário fornecido é mapeado para uma conta de usuário. Se assim, ela consulta resultante `MembershipUser` do objeto `IsLockedOut` e `IsApproved` propriedades para determinar se a conta foi bloqueada ou ainda não foram aprovada. Em ambos os casos, o `FailureText` propriedade é atualizada para um valor correspondente.

Para testar esse código, intencionalmente uma tentativa de logon como um usuário existente, mas usar uma senha incorreta. Faça isso cinco vezes em uma linha em um período de 10 minutos e a conta será bloqueada. Conforme mostrado na Figura 11, logon subsequentes tentativas sempre irá falhar (até mesmo com a senha correta), mas agora exibe mais descritivo sua conta foi bloqueada devido a muitas tentativas de logon inválidas. Entre em contato com o administrador para que a mensagem desbloqueada da conta.


[![Tito executada muitas tentativas de logon inválido e foi bloqueada](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figura 11**: Tito executadas muito muitos inválido tentativas de logon e tem sido bloqueados ([clique para exibir a imagem em tamanho normal](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Resumo

Anteriores neste tutorial, nossa página de logon validar as credenciais fornecidas com uma lista codificada de pares de nome de usuário e senha. Neste tutorial, atualizamos a página para validar as credenciais em relação a estrutura de associação. Na etapa 1 analisamos usando o `Membership.ValidateUser` método programaticamente. Na etapa 2 substituímos nossa interface do usuário criado manualmente e o código com o controle de logon.

O controle de logon apresenta uma interface de usuário de logon padrão e valida automaticamente as credenciais do usuário em relação a estrutura de associação. Além disso, no caso de credenciais válidas, o controle de logon entra o usuário por meio de autenticação de formulários. Em resumo, uma experiência de usuário de logon totalmente funcional está disponível, simplesmente arrastando o controle de logon para uma página, sem marcação extra declarativa ou código necessário. O que é mais, o controle de logon é altamente personalizável, permitindo um bom nível de controle em ambos os lógica de autenticação e a interface de usuário renderizado.

Neste ponto visitantes nosso site podem criar uma nova conta de usuário e de log para o site, mas ainda precisamos examinar restringir o acesso a páginas com base no usuário autenticado. Atualmente, qualquer usuário autenticado ou anônimo, pode exibir qualquer página no site. Juntamente com controle de acesso a páginas do nosso site em uma base por usuário, talvez temos certas páginas cuja funcionalidade depende do usuário. O seguinte tutorial examina como limitar o acesso e a funcionalidade da página com base no usuário conectado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Exibir mensagens personalizadas para bloqueada e os usuários não aprovado](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examinando o ASP.NET de 2.0 associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como: Criar uma página de logon do ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentação técnica do controle de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Usando os controles de logon](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Teresa Murphy e Michael Olivero. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-user-accounts-cs.md)
> [Próximo](user-based-authorization-cs.md)
