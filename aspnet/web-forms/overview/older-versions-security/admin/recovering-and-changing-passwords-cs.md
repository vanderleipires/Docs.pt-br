---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Recuperando e alterar senhas (c#) | Microsoft Docs
author: rick-anderson
description: "O ASP.NET inclui dois controles da Web para ajudar com a recuperação e a alteração de senhas. O controle PasswordRecovery habilita um visitante recuperar seu pa perdido..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 76c02a3da7dffad25a7bee03efff6b693f261d85
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="recovering-and-changing-passwords-c"></a>Recuperando e alterar senhas (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> O ASP.NET inclui dois controles da Web para ajudar com a recuperação e a alteração de senhas. O controle PasswordRecovery permite que um visitante recuperar sua senha perdida. O controle ChangePassword permite ao usuário atualizar sua senha. Com os outros controles da Web relacionadas a logon já vimos em toda esta série de tutoriais, o PasswordRecovery e ChangePassword controles funcionam com a estrutura de associação em segundo plano para redefinir ou modificar senhas de usuários.


## <a name="introduction"></a>Introdução

Entre os sites para meu banco da empresa do utilitário, empresa de telefone, contas de email e portais da web personalizado, como a maioria das pessoas, tenho dezenas de senhas diferentes para lembrar. Com tantas credenciais para memorizar hoje em dia, não é incomum para as pessoas esquecer sua senha. Para levar isso em conta, sites que oferecem as contas de usuário precisam incluir uma forma de um usuário para recuperar sua senha. Esse processo normalmente envolve a geração de uma nova senha aleatória e enviar um email para o endereço de email do usuário no arquivo. Após receber sua nova senha a maioria dos usuários retorne ao site e alterar sua senha do gerada aleatoriamente para um mais fácil de lembrar.

O ASP.NET inclui dois controles da Web para ajudar com a recuperação e a alteração de senhas. O controle PasswordRecovery permite que um visitante recuperar sua senha perdida. O controle ChangePassword permite ao usuário atualizar sua senha. Com os outros controles da Web relacionadas a logon já vimos em toda esta série de tutoriais, o PasswordRecovery e ChangePassword controles funcionam com a estrutura de associação em segundo plano para redefinir ou modificar senhas de usuários.

Neste tutorial, examinaremos usando esses dois controles. Também veremos como alterar programaticamente e redefinir a senha do usuário por meio de `MembershipUser` da classe `ChangePassword` e `ResetPassword` métodos.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Etapa 1: Ajudando os usuários recuperar perdido senhas

Todos os sites que dão suporte a contas de usuário necessário fornecer aos usuários um mecanismo para recuperar suas senhas esquecidas. A boa notícia é que implementar essa funcionalidade no ASP.NET é muito fácil graças ao controle PasswordRecovery Web. O controle PasswordRecovery renderiza uma interface que solicita ao usuário seu nome de usuário e, se necessário, a resposta à sua pergunta de segurança. Ele envia o usuário a senha.

> [!NOTE]
> Como as mensagens de email são transmitidas eletronicamente em texto sem formatação riscos de segurança são envolvidos com o envio de uma senha de usuário por email.


O controle PasswordRecovery consiste em três modos de exibição:

- **Nome de usuário** -solicita que o visitante do seu nome de usuário. Esta é a exibição inicial.
- **Pergunta**-exibe a pergunta de segurança e o nome de usuário do usuário como texto, juntamente com uma caixa de texto para o usuário insira a resposta à sua pergunta de segurança.
- **Sucesso**-exibe uma mensagem informando ao usuário que sua senha foi enviado por email.

Os modos de exibição exibido e ações realizadas pelo controle PasswordRecovery dependem dos seguintes parâmetros de configuração de associação:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

A estrutura de associação `RequiresQuestionAndAnswer` configuração indica se os usuários devem especificar uma pergunta de segurança e uma resposta ao se inscrever para uma conta. Conforme abordado no <a id="_msoanchor_1"> </a> [ *criando contas de usuário* ](../membership/creating-user-accounts-cs.md) tutorial, se `RequiresQuestionAndAnswer` for True (o padrão) e a interface do CreateUserWizard inclui a caixa de texto controles para o novo usuário pergunta de segurança e resposta; Se `RequiresQuestionAndAnswer` for False, sem essas informações são coletadas. Da mesma forma, se `RequiresQuestionAndAnswer` for verdadeiro, as exibições de controle PasswordRecovery a pergunta exibir depois que o usuário insere seu nome de usuário e a senha é recuperada somente se o usuário insere a resposta de segurança correto. Se `RequiresQuestionAndAnswer` for False, no entanto, o controle PasswordRecovery move diretamente da exibição de nome de usuário para o modo de exibição de êxito.

Depois que o usuário forneceu seu nome de usuário - ou sua resposta de nome de usuário e segurança, se `RequiresQuestionAndAnswer` for True - o PasswordRecovery emails sua senha de usuário. Se o `EnablePasswordRetrieval` opção é definida como True, então o usuário é enviado por email a senha atual. Se ele for definido como False e `EnablePasswordReset` é definida como True, o controle PasswordRecovery gera uma nova senha aleatória para o usuário e emails essa nova senha para eles. Se ambos os `EnablePasswordRetrieval` e `EnablePasswordReset` são False, o controle PasswordRecovery lança uma exceção.

> [!NOTE]
> Lembre-se de que o `SqlMembershipProvider` armazena as senhas dos usuários em um dos três formatos: Clear, Hashed (o padrão) ou criptografado. O mecanismo de armazenamento usado depende das configurações de configuração de associação; o aplicativo de demonstração usa o formato da senha Hashed. Ao usar o formato da senha Hashed o `EnablePasswordRetrieval` opção deve ser definida como False, porque o sistema não pode determinar a senha do usuário real da versão de hash armazenada no banco de dados.


A Figura 1 ilustra como a interface e o comportamento do PasswordRecovery é influenciado pela configuração de associação.


[![O RequiresQuestionAndAnswer, EnablePasswordRetrieval e EnablePasswordReset influenciar a aparência e o comportamento do controle PasswordRecovery](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Figura 1**: O `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, e `EnablePasswordReset` influenciar a aparência e o comportamento do controle PasswordRecovery ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> No <a id="_msoanchor_2"> </a> [ *criar o esquema de associação no SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial configuramos o provedor de associação, definindo `RequiresQuestionAndAnswer` como True, `EnablePasswordRetrieval` para False, e `EnablePasswordReset` como True.


### <a name="using-the-passwordrecovery-control"></a>Usando o controle PasswordRecovery

Vamos examinar usando o controle PasswordRecovery em uma página ASP.NET. Abra `RecoverPassword.aspx` e arraste e solte um controle PasswordRecovery da caixa de ferramentas para o Designer; definir seu `ID` para `RecoverPwd`. Como os controles de logon e CreateUserWizard Web, modos de exibição do controle PasswordRecovery renderizam uma interface avançada composta que inclui os rótulos, caixas de texto, botões e controles de validação. Você pode personalizar a aparência das exibições por meio das propriedades de estilo do controle ou convertendo os modos de exibição para um modelo. Posso deixar isso como um exercício para o leitor de interesse.

Quando um usuário visita esta página, ela será insira seu nome de usuário e clique no botão Enviar. Como definimos o `RequiresQuestionAndAnswer` propriedade como True no nosso definições de configuração de associação, o PasswordRecovery controlar será, em seguida, exibe o modo de exibição de pergunta. Depois que o usuário insere sua resposta de segurança corretas e clicar em enviar, o controle PasswordRecovery atualizar a senha do usuário para um gerada aleatoriamente e essa senha para o endereço de email no arquivo de email. Tudo isso era possível sem nós precisar escrever uma única linha de código!

Antes de você testa esta página, há uma parte final da configuração para tendem a: é necessário especificar as configurações de entrega de email em `Web.config`. O controle PasswordRecovery depende dessas configurações para enviar o email.

A configuração de entrega de email for especificada por meio de [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx)do [ `<mailSettings>` elemento](https://msdn.microsoft.com/library/w355a94k.aspx). Use o [ `<smtp>` elemento](https://msdn.microsoft.com/library/ms164240.aspx) para indicar o método de entrega e o padrão de endereço. A seguinte marcação define as configurações de email para usar um servidor de rede SMTP chamado `smtp.example.com` na porta 25 e com as credenciais de nome de usuário e senha de usuário e senha.

> [!NOTE]
> `<system.net>`é um elemento filho da raiz `<configuration>` elemento e um irmão de `<system.web>`. Portanto, não coloque o `<system.net>` elemento dentro do `<system.web>` elemento; em vez disso, coloque-o no mesmo nível.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Além de usar um servidor SMTP na rede, você poderá especificar um diretório de retirada de onde as mensagens de email a ser enviada devem depositadas.

Depois de configurar as configurações de SMTP, visite o `RecoverPassword.aspx` página através de um navegador. Primeiro, tente inserir um nome de usuário que não existe no repositório do usuário. Como mostra a Figura 2, o controle PasswordRecovery exibe uma mensagem indicando que não foi possível acessar as informações do usuário. O texto da mensagem pode ser personalizado por meio do controle [ `UserNameFailureText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Uma mensagem de erro é exibida se for digitado um nome de usuário inválido](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Figura 2**: uma mensagem de erro é exibida se for digitado um nome de usuário inválido ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image6.png))


Agora, insira um nome de usuário. Use o nome de usuário de uma conta do sistema com um endereço de email que você pode acessar e cuja segurança responder você sabe. Depois de inserir o nome de usuário e clicar em enviar, o controle PasswordRecovery exibe sua pergunta. Como com o modo de exibição de nome de usuário, se você inserir um incorreto responder as exibições de controle PasswordRecovery uma mensagem de erro (consulte a Figura 3). Use o [ `QuestionFailureText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) personalizar essa mensagem de erro.


[![Uma mensagem de erro será exibida se o usuário insere uma resposta de segurança inválido](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Figura 3**: uma mensagem de erro será exibida se o usuário insere uma resposta de segurança inválido ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image9.png))


Finalmente, digite a resposta de segurança correto e clique em enviar. Nos bastidores, o controle PasswordRecovery gera uma senha aleatória, atribui à conta de usuário, envia um email informando ao usuário de sua nova senha (consulte a Figura 4) e, em seguida, exibe o modo de exibição de êxito.


[![O usuário é enviado um Email com a nova senha His](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Figura 4**: O usuário é enviado um Email com a nova senha His ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>Personalizando o Email

O email padrão enviado pelo controle PasswordRecovery é bastante opaco (veja a Figura 4). A mensagem é enviada da conta especificada no `<smtp>` do elemento `from` atributo com a senha de assunto e o corpo de texto sem formatação:

Volte para o site e faça logon usando as informações a seguir.

Nome de usuário: *nome de usuário*

senha: *senha*

Essa mensagem pode ser personalizada programaticamente por meio de um manipulador de eventos do controle PasswordRecovery [ `SendingMail` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), ou declarativamente por meio de [ `MailDefinition` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Vamos explorar essas duas opções.

O `SendingMail` evento é acionado logo antes da mensagem de email é enviada e é nossa última oportunidade de ajustar programaticamente a mensagem de email. Quando esse evento é gerado, o manipulador de eventos é passado um objeto do tipo [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), cujo `Message` propriedade contém uma referência para o email prestes a ser enviado.

Criar um manipulador de eventos para o `SendingMail` evento e adicione o código a seguir, que adiciona programaticamente `webmaster@example.com` à lista CC.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

A mensagem de email também pode ser configurada por meio de declarativo. O PasswordRecovery `MailDefinition` propriedade é um objeto do tipo [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). O `MailDefinition` classe oferece um conjunto de propriedades relacionadas a email, incluindo `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`e outros. Para começar, defina o [ `Subject` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) para algo mais descritivo do que aquele usado por padrão (senha), como sua senha foi redefinida...

Para personalizar o corpo da mensagem de email, precisamos criar um arquivo de modelo de email separado que contém o conteúdo do corpo. Comece criando uma nova pasta no site de chamada `EmailTemplates`. Em seguida, adicione um novo arquivo de texto para essa pasta chamada `PasswordRecovery.txt` e adicione o seguinte conteúdo:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Observe o uso dos espaços reservados `<%UserName%>` e `<%Password%>`. O controle PasswordRecovery substitui automaticamente esses espaços duas reservados com o nome de usuário e a senha recuperada antes de enviar o email do usuário.

Por fim, aponte o `MailDefinition`do [ `BodyFileName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) para o modelo de email que acabamos de criar (`~/EmailTemplates/PasswordRecovery.txt`).

Depois de fazer essas alterações revisitar o `RecoverPassword.aspx` página e digite sua resposta de nome de usuário e segurança. Você receberá deve um email que será semelhante à mostrada na Figura 5. Observe que `webmaster@example.com` foi seria CC e o assunto e corpo foi atualizados.


[![Lista CC, assunto e corpo foram atualizadas](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Figura 5**: O assunto, corpo e CC lista foram atualizadas ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image15.png))


Para enviar um email formatado em HTML definido [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) como True (o padrão é falso) e atualizar o modelo de email para incluir HTML.

O `MailDefinition` propriedade não é exclusiva para a classe PasswordRecovery. Como veremos na etapa 2, o controle ChangePassword também oferece um `MailDefinition` propriedade. Além disso, o controle CreateUserWizard inclui uma propriedade que você pode configurar para enviar uma mensagem de email de boas-vindas automaticamente para novos usuários.

> [!NOTE]
> Atualmente não existem links no painel de navegação esquerdo para alcançar o `RecoverPassword.aspx` página. Um usuário somente gostariam de visitar essa página se ela não foi possível para fazer logon com êxito o site. Portanto, atualizar o `Login.aspx` página para incluir um link para o `RecoverPassword.aspx` página.


### <a name="programmatically-resetting-a-users-password"></a>Programaticamente, redefinindo a senha do usuário

Quando redefinir uma senha de usuário a PasswordRecovery controlar chamadas de `MembershipUser` do objeto [ `ResetPassword` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Esse método tem duas sobrecargas:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)**-Redefine a senha do usuário. Use essa sobrecarga se `RequiresQuestionAndAnswer` é False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)**-Redefine se somente de senha do usuário fornecido *securityAnswer* está correto. Use essa sobrecarga se `RequiresQuestionAndAnswer` for True.

As duas sobrecargas retornam a nova senha gerada aleatoriamente.

Como com outros métodos na estrutura de associação, o `ResetPassword` delegados de método para o provedor configurado. O `SqlMembershipProvider` invoca o `aspnet_Membership_ResetPassword` procedimento armazenado, passando o nome do usuário, a nova senha e a resposta de senha fornecida, entre outros campos. O procedimento armazenado garante que a resposta de senha corresponde e, em seguida, atualiza a senha do usuário.

Algumas observações de nível baixo de implementação:

- Um usuário bloqueado não pode redefinir sua senha. No entanto, talvez um usuário não aprovado. Podemos discutir o bloqueado e aprovada estados mais detalhadamente o <a id="_msoanchor_3"> </a> [ *Unlocking e o usuário aprovar* ](unlocking-and-approving-user-accounts-cs.md) tutorial de contas.
- Se a resposta da senha está incorreta, a contagem de tentativas de resposta de senha do usuário é incrementada. Se um número de tentativas de resposta de segurança inválido especificado ocorrer dentro de uma janela de tempo especificado, o usuário está bloqueado.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Uma palavra em como as senhas aleatórias são gerados

As senhas gerado aleatoriamente que mostra as mensagens de email em figuras 4 e 5 são criadas, a classe de associação [ `GeneratePassword` método](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Esse método aceita dois parâmetros de entrada de inteiro - *comprimento* e *numberOfNonAlphanumericCharacters* - e retorna uma cadeia de caracteres de pelo menos *comprimento* caracteres no menos *numberOfNonAlphanumericCharacters* número de caracteres não alfanuméricos. Quando este método é chamado de dentro de classes de associação ou controles da Web relacionadas a logon, os valores para esses dois parâmetros são determinados pela configuração de associação `MinRequiredPasswordLength` e `MinRequiredNonalphanumericCharacters` propriedades, que são definidos como 7 e 1, respectivamente.

O `GeneratePassword` método usa um gerador de números aleatórios criptograficamente forte para garantir que não há nenhuma diferença na qual caracteres aleatórios estão selecionados. Além disso, `GeneratePassword` é `public`, que significa que você pode usá-lo diretamente do seu aplicativo ASP.NET se você precisar gerar cadeias de caracteres aleatórias ou senhas.

> [!NOTE]
> O `SqlMembershipProvider` classe sempre gera uma senha aleatória de no mínimo 14 caracteres, portanto, se `MinRequiredPasswordLength` for inferior a 14, seu valor é ignorado.


## <a name="step-2-changing-passwords"></a>Etapa 2: Alteração de senhas

As senhas gerado aleatoriamente são difíceis de lembrar. Considere a senha mostrada na Figura 4: `WWGUZv(f2yM:Bd`. Tente confirmando que a memória! Obviamente, depois que um usuário é enviado uma senha gerada aleatoriamente desse tipo, ela vai querer alterar a senha para algo mais fácil de lembrar.

Use o controle de alteração de senha para criar uma interface para um usuário altere sua senha. Bem como o controle PasswordRecovery, o controle ChangePassword consiste em dois modos de exibição: alterar a senha e o sucesso. O modo de exibição de alterar a senha solicita ao usuário para suas senhas antigas e novas. Após fornecer a senha antiga correta e uma nova senha que atenda os requisitos de caractere não alfanumérico e um comprimento mínimo, o controle ChangePassword atualiza a senha do usuário e exibe o modo de exibição de êxito.

> [!NOTE]
> O controle ChangePassword modifica a senha do usuário chamando o `MembershipUser` do objeto [ `ChangePassword` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). O método ChangePassword aceita dois `string` entrada parâmetros - *Senha_atual* e *newPassword*- e atualiza a conta do usuário com o *newPassword*, Supondo que a fornecida *Senha_atual* está correto.


Abra o `ChangePassword.aspx` página e adicionar um controle de alteração de senha para a página, nomeando- `ChangePwd`. Neste ponto, a exibição de Design deve mostrar a alterar a senha (consulte a Figura 6). Como com o controle PasswordRecovery, você pode alternar entre os modos de exibição por meio de marcas inteligentes do controle. Além disso, aparências essas exibições são personalizáveis através das propriedades de estilo variados ou convertê-las em um modelo.


[![Adicionar um controle de alteração de senha para a página](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Figura 6**: adicionar um controle de alteração de senha para a página ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image18.png))


O controle de alteração de senha pode atualizar a senha do usuário conectado no momento *ou* a senha de outro usuário especificado. Como mostra a Figura 6, a exibição de alterar a senha padrão processa apenas três controles de caixa de texto: uma para a senha antiga e dois para a nova senha. Essa interface padrão é usado para atualizar a senha do usuário conectado no momento.

Para usar o controle ChangePassword para atualizar a senha de outro usuário, defina o controle [ `DisplayUserName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) como True. Isso adiciona uma quarta caixa de texto para a página, solicitar o nome do usuário cuja senha para alterar.

Definindo `DisplayUserName` para True será útil se você deseja permitir que um usuário conectado out alterar sua senha sem precisar fazer logon. Pessoalmente, acho que não há nada errado com a necessidade de um usuário para logon antes de permitir que o a alterar sua senha. Portanto, deixe `DisplayUserName` definido como False (padrão). Tomar essa decisão, no entanto, é essencialmente são bloqueio usuários anônimos acessem esta página. Atualizar regras de autorização de URL do site para negar a usuários anônimos visitação `ChangePassword.aspx`. Se você precisar atualizar a memória de sintaxe de regra de autorização da URL, consulte novamente o <a id="_msoanchor_4"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial.

> [!NOTE]
> Pode parecer que o `DisplayUserName` propriedade é útil para os administradores podem alterar as senhas de outros usuários. No entanto, mesmo quando `DisplayUserName` está definida como True, a senha antiga correta deve ser conhecida e inserida. Falaremos sobre as técnicas para permitir que os administradores alterar as senhas dos usuários na etapa 3.


Visite o `ChangePassword.aspx` página através de um navegador e altere sua senha. Observe que uma mensagem de erro é exibida se você inserir uma nova senha que não atender os requisitos de caractere não alfanumérico especificados na configuração de associação e o comprimento da senha (consulte a Figura 7).


[![Adicionar um controle de alteração de senha para a página](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Figura 7**: adicionar um controle de alteração de senha para a página ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image21.png))


Após inserir a senha antiga correta e uma senha nova válida, o log do usuário a senha for alterada e a exibição de êxito.

### <a name="sending-a-confirmation-email"></a>Enviar um Email de confirmação

Por padrão, o controle ChangePassword não envia uma mensagem de email para o usuário cuja senha acabou de ser atualizada. Se você deseja enviar um email, basta configurar o controle `MailDefinition` propriedade. Vamos configurar o controle de alteração de senha para que o usuário é enviado um email formatado em HTML que contém a nova senha.

Comece criando um novo arquivo no `EmailTemplates` pasta denominada `ChangePassword.htm`. Adicione a seguinte marcação:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Em seguida, defina o controle de alteração de senha `MailDefinition` da propriedade `BodyFileName`, `IsBodyHtml`, e `Subject` propriedades ~ / EmailTemplates/ChangePassword.htm, True e sua senha foi alterada!, respectivamente.

Depois de fazer essas alterações, revise a página e alterar sua senha novamente. Neste momento, o controle ChangePassword envia um email de personalizados, formatado em HTML para o endereço de email do usuário no arquivo (consulte a Figura 8).


[![Uma mensagem de Email que informa a senha do usuário que seus foi alterado](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Figura 8**: uma mensagem de Email informa a senha do usuário que seus foi alterada ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Etapa 3: Os administradores podem alterar as senhas dos usuários

Um recurso comum em aplicativos que dão suporte a contas de usuário é a capacidade de um usuário administrativo alterar senhas de outros usuários. Às vezes, essa funcionalidade é necessária porque o sistema não tem a capacidade dos usuários podem alterar suas próprias senhas. Nesse caso, a única maneira de um usuário para recuperar uma senha esquecida seria o administrador atribuir uma nova senha. Com os controles PasswordRecovery e ChangePassword, no entanto, os usuários administrativos necessitam não ocupado se alterar senhas de usuários, como os usuários são capazes de fazer isso.

Mas e se o cliente insiste para que os usuários administrativos devem ser capazes de alterar as senhas de outros usuários? Infelizmente, adicionar essa funcionalidade pode ser um pouco de trabalho. Para alterar a senha do usuário, a senha antiga e nova deve ser fornecida para o `MembershipUser` do objeto `ChangePassword` método, mas um administrador não deve ter que saber a senha do usuário para modificá-lo.

Uma solução alternativa é primeiro redefinir a senha do usuário e, em seguida, altere-o para a nova senha usando código semelhante ao seguinte:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Esse código inicia Recuperando informações sobre *nome de usuário*, que é o usuário cuja senha que o administrador deseja alterar. Em seguida, o `ResetPassword` método é chamado, que atribui e o usuário uma nova senha aleatória. Essa senha gerada aleatoriamente que é retornada pelo método e armazenada na variável `resetPwd`. Agora que sabemos que a senha do usuário, pode alterá-la por meio de uma chamada para `ChangePassword`.

O problema é que este código só funcionará se a configuração do sistema de associação é definida, de modo que `RequiresQuestionAndAnswer` é False. Se `RequiresQuestionAndAnswer` for True, como em nosso aplicativo, em seguida, o `ResetPassword` método deve ser passado a resposta de segurança, caso contrário, ela irá gerar uma exceção.

Se a estrutura de associação está configurada para exigir uma pergunta de segurança e uma resposta, e ainda seu cliente insiste para que os administradores poderão alterar as senhas dos usuários, você tem três opções:

- Gerar suas mãos no ar e informe seu cliente que se trata de apenas uma coisa que não pode ser feita.
- Definir `RequiresQuestionAndAnswer` como False. Isso resulta em um aplicativo menos seguro. Imagine que um usuário mal-intencionado obteve acesso à caixa de entrada de email do outro usuário. Talvez o usuário comprometido sai para almoçar suas mesas e não bloquear sua estação de trabalho, ou talvez elas acessar seus emails de um terminal público e não sair. Em ambos os casos, o usuário mal-intencionado pode visitar o `RecoverPassword.aspx` página e digite o nome do usuário. O sistema, em seguida, enviar email a senha recuperada sem solicitar a resposta de segurança.
- Ignore a camada de abstração criada pela estrutura de associação e trabalhar diretamente com o banco de dados do SQL Server. O esquema de associação inclui um procedimento armazenado chamado `aspnet_Membership_SetPassword` que define a senha do usuário e não requer a resposta de segurança ou a senha antiga para realizar sua tarefa.

Nenhuma dessas opções é especialmente atraente, mas isso é como a vida útil de um desenvolvedor vai às vezes.

Eu fui em frente e implementado a terceira abordagem, escrevendo código que ignora o `Membership` e `MembershipUser` classes e opera diretamente o `SecurityTutorials` banco de dados.

> [!NOTE]
> Trabalhando diretamente com o banco de dados, o encapsulamento fornecido pela estrutura de associação é quebrado. Essa decisão vincula ao `SqlMembershipProvider`, tornando o nosso código menos portátil. Além disso, esse código pode não funcionar conforme o esperado em futuras versões do ASP.NET se altera o esquema de associação. Essa abordagem é uma solução alternativa e, como a maioria das soluções alternativas, não é um exemplo de práticas recomendadas.


O código tem alguns bits atraente e é muito longo. Portanto, não desejo sobrecarregar neste tutorial com uma análise detalhada dele. Se você estiver interessado em saber mais, baixe o código para este tutorial e visite o `~/Administration/ManageUsers.aspx` página. Nesta página, que criamos no <a id="_msoanchor_5"> </a> [tutorial anterior](building-an-interface-to-select-one-user-account-from-many-cs.md), lista cada usuário. Atualizei o GridView para incluir um link para o `UserInformation.aspx` página, passando o nome de usuário do usuário selecionado por meio de querystring. O `UserInformation.aspx` página exibe informações sobre o usuário selecionado e caixas de texto para alterar sua senha (consulte a Figura 9).

Depois de inserir a nova senha, confirmá-la na segunda caixa de texto e clique no botão de usuário de atualização, um postback tem lugar e `aspnet_Membership_SetPassword` procedimento armazenado é chamado, atualizando a senha do usuário. Recomendo que os leitores interessados nessa funcionalidade para se familiarizar com o código e estender a funcionalidade para incluir na enviando um email para o usuário cuja senha foi alterada.


[![Um administrador pode alterar a senha do usuário](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Figura 9**: um administrador pode alterar a senha do usuário ([clique para exibir a imagem em tamanho normal](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> O `UserInformation.aspx` página atualmente só funciona se a estrutura de associação é configurada para armazenar senhas em formato criptografado ou Hashed. Ele não tem o código para criptografar a senha nova, embora você está convidado a adicionar essa funcionalidade. O modo é recomendável adicionar o código necessário é usar um descompilador como [refletor](http://www.aisto.com/roeder/dotnet/) para examinar o código-fonte para os métodos do .NET Framework; iniciar examinando o `SqlMembershipProvider` da classe `ChangePassword` método. Essa é a técnica, usado para gravar o código para criar um hash da senha.


## <a name="summary"></a>Resumo

O ASP.NET oferece dois controles para ajudar os usuários a gerenciar sua senha. O controle PasswordRecovery é útil para aqueles que esqueceram suas senhas. Dependendo da configuração da estrutura de associação, ou é um email ao usuário sua senha atual ou uma nova senha gerada aleatoriamente. O controle de alteração de senha permite que um usuário atualizar sua senha.

Como os controles de logon e CreateUserWizard, os controles PasswordRecovery e ChangePassword renderizam uma interface de usuário sem precisar escrever uma linha de marcação declarativa ou a linha de código. Se a interface de usuário padrão não atender às suas necessidades, você pode personalizá-lo por meio de uma variedade de propriedades de estilo. Como alternativa, interfaces os controles podem ser convertidos em modelos para um grau de controle ainda mais detalhado. Nos bastidores esses controles usam a API de associação, invocando o `MembershipUser` do objeto `ResetPassword` e `ChangePassword` métodos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Início rápido do controle de alteração de senha](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Início rápido do controle PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Enviar o Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`Perguntas frequentes](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial incluem Michael Emmings e Suchi Banerjee. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](building-an-interface-to-select-one-user-account-from-many-cs.md)
[Próximo](unlocking-and-approving-user-accounts-cs.md)
