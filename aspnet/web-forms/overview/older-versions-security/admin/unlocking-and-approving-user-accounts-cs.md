---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Desbloqueio e aprovação de contas de usuário (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como criar uma página da web para os administradores gerenciarem bloqueada e aprovado o status dos usuários. Também veremos como aprovar o novo de usuários...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a8373f62833c3a76d2e7f96193e5ecbe2d9c593
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833687"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Contas de usuário de desbloqueio e aprovação (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Este tutorial mostra como criar uma página da web para os administradores gerenciarem bloqueada e aprovado o status dos usuários. Também veremos como aprovar novos usuários somente depois que eles confirmaram seu endereço de email.


## <a name="introduction"></a>Introdução

Junto com um nome de usuário, senha e email, cada conta de usuário tem dois campos de status que determinam se o usuário pode fazer logon no site: bloqueada e aprovada. Um usuário está bloqueado automaticamente se eles fornecem credenciais inválidas, um número especificado de vezes dentro de um número especificado de minutos (as configurações padrão bloquearem um usuário após 5 tentativas de logon inválidas dentro de 10 minutos). O status aprovado é útil em cenários em que alguma ação deve ocorrer antes que um novo usuário é capaz de fazer logon site. Por exemplo, um usuário talvez precise primeiro verificar seu endereço de email ou ser aprovada por um administrador antes de poder fazer logon.

Porque um bloqueados fora ou não aprovado o usuário não pode fazer logon, é natural apenas para estar se perguntando como esses status podem ser redefinidas. ASP.NET não inclui nenhuma funcionalidade interna ou controles da Web para o gerenciamento de usuários bloqueados e aprovada status, em parte porque essas decisões que precisam ser manipulados em uma base de site a site. Alguns sites podem aprovar automaticamente todas as novas contas de usuário (o comportamento padrão). Outros têm um administrador aprove novas contas ou não aprovar os usuários até que eles visitam um link enviado para o endereço de email fornecido ao se inscrever. Da mesma forma, alguns sites podem bloquear os usuários até que um administrador redefine seu status, enquanto outros sites envie um email para o usuário bloqueado com uma URL que eles podem visitar para desbloquear sua conta.

Este tutorial mostra como criar uma página da web para os administradores gerenciarem bloqueada e aprovado o status dos usuários. Também veremos como aprovar novos usuários somente depois que eles confirmaram seu endereço de email.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Etapa 1: Gerenciar usuários bloqueada e status de aprovação

No <a id="Tutorial12"> </a> [ *criação de uma Interface para selecionar uma conta de usuário de muitos* ](building-an-interface-to-select-one-user-account-from-many-cs.md) tutorial, nós criamos uma página que listada cada conta de usuário no paginada, filtrados GridView. A grade lista o nome de cada usuário e email, seus status aprovados e bloqueadas, se eles estão online no momento e quaisquer comentários sobre o usuário. Para gerenciar usuários aprovados e bloqueada status, poderíamos tornar essa grade editável. Para alterar o status de um usuário aprovado, o administrador teria primeiro localize a conta de usuário e, em seguida, edite a linha correspondente do GridView, marcando ou desmarcando a caixa de seleção aprovada. Como alternativa, podemos pode gerenciar os status aprovados e bloqueados por meio de uma página separada do ASP.NET.

Para este tutorial vamos usar duas páginas do ASP.NET: `ManageUsers.aspx` e `UserInformation.aspx`. A ideia aqui é que `ManageUsers.aspx` lista as contas de usuário no sistema, enquanto `UserInformation.aspx` permite que o administrador gerencie os status aprovados e bloqueados para um usuário específico. Nossa tarefa prioritária é aumentar o GridView no `ManageUsers.aspx` para incluir um HyperLinkField, que é renderizado como uma coluna de links. Queremos que cada link para apontar para `UserInformation.aspx?user=UserName`, onde *nome de usuário* é o nome do usuário para editar.

> [!NOTE]
> Se você baixou o código para o <a id="Tutorial13"> </a> [ *Recovering e alterar as senhas* ](recovering-and-changing-passwords-cs.md) tutorial, você deve ter observado que o `ManageUsers.aspx` página já contém um conjunto de " Gerenciar"links e o `UserInformation.aspx` página fornece uma interface para alterar a senha do usuário selecionado. Decidi não replicar essa funcionalidade no código associado com este tutorial porque funcionou, evitando a API Membership e operar diretamente com o banco de dados do SQL Server para alterar a senha do usuário. Este tutorial começa do zero com o `UserInformation.aspx` página.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Adicionando "gerenciar" Links para o`UserAccounts`GridView

Abra o `ManageUsers.aspx` da página e adicionar um HyperLinkField para o `UserAccounts` GridView. Defina o HyperLinkField `Text` propriedade para "Gerenciar" e seu `DataNavigateUrlFields` e `DataNavigateUrlFormatString` propriedades a serem `UserName` e "UserInformation.aspx?user={0}", respectivamente. Essas configurações definem o HyperLinkField, de modo que todos os hiperlinks exibem o texto "Gerenciar", mas cada link passa no respectivo *nome de usuário* valor na cadeia de consulta.

Depois de adicionar o HyperLinkField a GridView, reserve um tempo para exibir o `ManageUsers.aspx` página por meio de um navegador. Como mostra a Figura 1, cada linha de GridView agora inclui um link "Gerenciar". O link "Gerenciar" para Bruce aponta para `UserInformation.aspx?user=Bruce`, enquanto que o link "Gerenciar" de Dave aponta para `UserInformation.aspx?user=Dave`.


[![Adiciona o HyperLinkField um](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: O HyperLinkField adiciona um Link "Gerenciar" para cada conta de usuário ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Criaremos a interface do usuário e de código para o `UserInformation.aspx` página em um momento, mas primeiro vamos falar sobre como alterar programaticamente um usuário do bloqueada e status de aprovação. O [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) tem [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) e [ `IsApproved` propriedades](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). A propriedade `IsLockedOut` é somente leitura. Não há nenhum mecanismo para bloquear um usuário; por meio de programação para desbloquear um usuário, use o `MembershipUser` da classe [ `UnlockUser` método](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). O `IsApproved` propriedade é legível e gravável. Para salvar as alterações a essa propriedade, precisamos chamar o `Membership` da classe [ `UpdateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), passando o modificado `MembershipUser` objeto.

Porque o `IsApproved` é legível e gravável, um controle de caixa de seleção é provavelmente o melhor elemento de interface do usuário para configurar essa propriedade. No entanto, uma caixa de seleção não funcionará para o `IsLockedOut` propriedade porque um administrador não é possível bloquear um usuário, ela só poderá desbloquear um usuário. Uma interface de usuário adequados para o `IsLockedOut` propriedade é um botão que, quando clicado, desbloqueia a conta de usuário. Esse botão só deve ser habilitado se o usuário está bloqueado.

### <a name="creating-theuserinformationaspxpage"></a>Criando o`UserInformation.aspx`página

Agora estamos prontos para implementar a interface do usuário em `UserInformation.aspx`. Abrir essa página e adicione os seguintes controles da Web:

- Controlar um hiperlink que, quando clicado, retorna ao administrador o `ManageUsers.aspx` página.
- Um controle de Web de rótulo para exibir o nome do usuário selecionado. Definir este rótulo `ID` à `UserNameLabel` e limpe seu `Text` propriedade.
- Um controle de caixa de seleção denominado `IsApproved`. Defina suas `AutoPostBack` propriedade para `true`.
- Um controle de rótulo para exibir o usuário do bloqueado pela última data. Nomeie este rótulo `LastLockedOutDateLabel` e limpe seu `Text` propriedade.
- Um botão para desbloquear o usuário. Nomeie esse botão `UnlockUserButton` e defina seu `Text` propriedade para "Desbloquear usuário".
- Um controle de rótulo para exibir mensagens de status, como, "o status do usuário aprovado foi atualizado." Nomeie esse controle `StatusMessage`, desmarque out seus `Text` propriedade e defina seu `CssClass` propriedade para `Important`. (O `Important` classe CSS é definida no `Styles.css` arquivo de folha de estilos; ele exibe o texto correspondente em uma fonte grande, vermelha.)

Depois de adicionar esses controles, o modo de exibição de Design no Visual Studio deve ser semelhante para a tela na Figura 2.


[![Criar a Interface do usuário para UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: criar a Interface do usuário para `UserInformation.aspx` ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Com a interface de usuário completa, nossa próxima tarefa é definir o `IsApproved` caixa de seleção e outros controles com base nas informações do usuário selecionado. Criar um manipulador de eventos para a página `Load` eventos e adicione o seguinte código:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

O código acima inicia, garantindo que se trata de primeira visita a página e não um postback subsequente. Em seguida, lê o nome de usuário passado a `user` campo querystring e recupera informações sobre essa conta de usuário por meio de `Membership.GetUser(username)` método. Se nenhum nome de usuário foi fornecido por meio da cadeia de consulta, ou se o usuário especificado não pôde ser encontrado, o administrador é enviado de volta ao `ManageUsers.aspx` página.

O `MembershipUser` do objeto `UserName` valor, em seguida, é exibido na `UserNameLabel` e o `IsApproved` caixa de seleção é marcada com base no `IsApproved` valor da propriedade.

O `MembershipUser` do objeto [ `LastLockoutDate` propriedade](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) retorna um `DateTime` valor que indica quando o usuário foi bloqueada. Se o usuário nunca foi bloqueado, o valor retornado depende do provedor de associação. Quando uma nova conta é criada, o `SqlMembershipProvider` define o `aspnet_Membership` da tabela `LastLockoutDate` campo `1754-01-01 12:00:00 AM`. O código acima exibe uma cadeia de caracteres vazia do `LastLockoutDateLabel` se o `LastLockoutDate` propriedade ocorre antes do ano 2000; caso contrário, a parte de data a `LastLockoutDate` propriedade é exibida no rótulo. O `UnlockUserButton'` s `Enabled` propriedade é definida para o usuário bloqueado status, que significa que esse botão só será habilitado se o usuário está bloqueado.

Reserve um tempo para testar o `UserInformation.aspx` página por meio de um navegador. Você, certamente, precisará iniciar em `ManageUsers.aspx` e selecione uma conta de usuário para gerenciar. Ao chegar `UserInformation.aspx`, observe que o `IsApproved` caixa de seleção é verificada apenas se o usuário for aprovado. Se o usuário já foi bloqueado, seu último bloqueado data é exibido. O botão Desbloquear usuário é habilitado apenas se o usuário está bloqueado no momento. Marcando ou desmarcando a `IsApproved` caixa de seleção ou clicando no botão Desbloquear usuário causa um postback, mas sem modificações são feitas para a conta de usuário porque ainda não encontramos criar manipuladores de eventos para esses eventos.

Retorne ao Visual Studio e criar manipuladores de eventos para o `IsApproved` da caixa de seleção `CheckedChanged` evento e o `UnlockUser` do botão `Click` eventos. No `CheckedChanged` manipulador de eventos, defina o usuário `IsApproved` propriedade para o `Checked` propriedade da caixa de seleção e, em seguida, salve as alterações por meio de uma chamada para `Membership.UpdateUser`. No `Click` manipulador de eventos, simplesmente a chamada a `MembershipUser` do objeto `UnlockUser` método. Em ambos os manipuladores de eventos, exiba uma mensagem adequada no `StatusMessage` rótulo.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Testando o`UserInformation.aspx`página

Com esses manipuladores de eventos em vigor, revisita a página aprovadas e um usuário. Como mostra a Figura 3, você deverá ver um resumo da mensagem na página indicando que o usuário `IsApproved` propriedade foi modificada com êxito.


[![Chris foi reprovado](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Chris foi reprovado ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Em seguida, logout e tente fazer logon como o usuário cuja conta foi apenas não aprovados. Porque o usuário não for aprovado, eles não podem fazer logon. Por padrão, o controle de logon exibe a mesma mensagem se o usuário não pode fazer logon, independentemente do motivo. Mas no <a id="Tutorial6"> </a> [ *Validar usuário as credenciais contra a associação de usuário Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) tutorial vimos aprimorando o controle de logon para exibir uma mensagem mais apropriada. Como mostra a Figura 4, Chris é mostrado uma mensagem explicando o que ele não pode fazer logon porque sua conta ainda não foi aprovada.


[![Chris não é possível porque o His conta de logon é reprovado](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris não é possível porque o His conta de logon é reprovado ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Para testar a funcionalidade bloqueada, tente fazer logon como um usuário aprovado, mas usar uma senha incorreta. Repita esse processo, o número necessário de vezes até que a conta do usuário foi bloqueada. O controle de logon também foi atualizado para mostrar um personalizado de mensagem se a tentativa de logon em uma conta bloqueada. Você sabe que uma conta foi bloqueada depois que você começar a ver a seguinte mensagem na página de logon: "sua conta foi bloqueada devido a muitas tentativas de logon inválido. Entre em contato com o administrador para configurar o desbloqueio da conta."

Volte para o `ManageUsers.aspx` da página e clique no link Gerenciar para o usuário bloqueado. Como mostra a Figura 5, você deverá ver um valor no `LastLockedOutDateLabel` botão Desbloquear o usuário deve ser habilitado. Clique no botão Desbloquear usuário para desbloquear a conta de usuário. Depois que você conquistou o usuário, eles poderão fazer logon novamente.


[![Dave foi bloqueado para o sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: Dave tem foram bloqueados fora do sistema ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Etapa 2: Especificar novos usuários aprovados Status

O status aprovado é útil em cenários em que você deseja que alguma ação a ser executada antes de um novo usuário é capaz de fazer logon e acessar os recursos específicos do usuário do site. Por exemplo, você pode estar executando um site privado onde todas as páginas, exceto para as páginas de logon e inscrição, são acessíveis somente para usuários autenticados. Mas o que acontece se um estranho atingir seu site, localiza a página de inscrição e cria uma conta? Para evitar que isso aconteça, você pode mover a página de inscrição para uma `Administration` pasta e exigem que um administrador crie manualmente cada conta. Como alternativa, você pode permitir que qualquer pessoa para a inscrição, mas proibir o acesso de site até que o administrador aprova a conta de usuário.

Por padrão, o controle CreateUserWizard aprova novas contas. Você pode configurar esse comportamento usando o controle [ `DisableCreatedUser` propriedade](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Defina essa propriedade como `true` por não aprovar as novas contas de usuário.

> [!NOTE]
> Por padrão o controle CreateUserWizard registra automaticamente na nova conta de usuário. Esse comportamento é determinado pelo controle [ `LoginCreatedUser` propriedade](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Porque não aprovados que os usuários não podem fazer logon no site, quando `DisableCreatedUser` é `true` nova conta de usuário não está conectada ao site, independentemente do valor de `LoginCreatedUser` propriedade.


Se você estiver criando programaticamente novas contas de usuário por meio de `Membership.CreateUser` método para criar uma conta de usuário não aprovados usam uma das sobrecargas que aceitam o novo usuário `IsApproved` valor da propriedade como um parâmetro de entrada.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Etapa 3: Aprovar os usuários, verificando seu endereço de Email

Muitos sites que dão suporte a contas de usuário não aprovem novos usuários até que eles Verifique se eles fornecidos ao registrar o endereço de email. Esse processo de verificação normalmente é usado para impedir que outros os vigaristas, os remetentes de spam e bots conforme ele requer um endereço de email exclusivo, verificado e adiciona uma etapa extra no processo de inscrição. Com esse modelo, quando um novo usuário se inscreve, elas são enviadas uma mensagem de email que inclui um link para uma página de verificação. Ao visitar o link o usuário tem comprovado que receberam o email e, portanto, o que o endereço de email fornecido é válido. A página de verificação é responsável pela aprovação do usuário. Isso pode ocorrer automaticamente, aprovação, portanto, qualquer usuário que atinge nessa página, ou somente depois que o usuário fornece informações adicionais, como uma [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Para acomodar esse fluxo de trabalho, é necessário primeiro atualizar a página de criação de conta para que os novos usuários não aprovados. Abra o `EnhancedCreateUserWizard.aspx` página o `Membership` pasta e defina o controle CreateUserWizard `DisableCreatedUser` propriedade para `true`.

Em seguida, precisamos configurar o controle CreateUserWizard para enviar um email para o novo usuário com instruções sobre como verificar sua conta. Em particular, estamos incluirá um link no email para o `Verification.aspx` página (que ainda não encontramos criar), passando o novo usuário `UserId` por meio da cadeia de consulta. O `Verification.aspx` página irá pesquisar o usuário especificado e marcá-los aprovada.

### <a name="sending-a-verification-email-to-new-users"></a>Enviar um Email de verificação para novos usuários

Para enviar um email do controle CreateUserWizard, configure seu `MailDefinition` propriedade adequadamente. Conforme discutido na <a id="Tutorial13"> </a> [tutorial anterior](recovering-and-changing-passwords-cs.md), os controles ChangePassword e PasswordRecovery incluem uma [ `MailDefinition` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) que funciona da mesma maneira como do controle CreateUserWizard.

> [!NOTE]
> Para usar o `MailDefinition` opções de propriedade que você precisa para especificar a entrega de email no `Web.config`. Para obter mais informações, consulte [envio de Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Comece criando um novo modelo de email chamado `CreateUserWizard.txt` no `EmailTemplates` pasta. Use o seguinte texto para o modelo:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Defina a `MailDefinition'` s `BodyFileName` propriedade como "~ / EmailTemplates/CreateUserWizard.txt" e seu `Subject` propriedade como "Bem-vindo ao meu site! Ative sua conta".

Observe que o `CreateUserWizard.txt` modelo de email inclui um `<%VerificationUrl%>` espaço reservado. É aí que a URL para o `Verification.aspx` página será colocada. O CreateUserWizard substitui automaticamente o `<%UserName%>` e `<%Password%>` espaços reservados pelo nome de usuário e a senha, a nova conta, mas não há nenhum interno `<%VerificationUrl%>` espaço reservado. Precisamos substitui-lo manualmente com a URL de verificação apropriadas.

Para fazer isso, crie um manipulador de eventos para o CreateUserWizard [ `SendingMail` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) e adicione o seguinte código:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

O `SendingMail` evento é acionado depois que o `CreatedUser` evento, o que significa que no momento em que o manipulador de eventos acima executa o novo usuário de conta já foi criada. Podemos acessar o novo usuário `UserId` valor chamando o `Membership.GetUser` método, passando o `UserName` inseridos no controle CreateUserWizard. Em seguida, a URL de verificação é formada. A instrução `Request.Url.GetLeftPart(UriPartial.Authority)` retorna o `http://yourserver.com` parte da URL; `Request.ApplicationPath` caminho retorna em que o aplicativo está enraizado. A verificação de URL, em seguida, é definido como `Verification.aspx?ID=userId`. Essas duas cadeias de caracteres, em seguida, são concatenadas para formar a URL completa. Por fim, o corpo da mensagem de email (`e.Message.Body`) tem todas as ocorrências de `<%VerificationUrl%>` substituído com a URL completa.

O efeito líquido é que novos usuários são não aprovados, o que significa que eles não podem fazer logon no site. Além disso, elas são enviadas automaticamente um email com um link para a URL de verificação (veja a Figura 6).


[![O novo usuário recebe um Email com um Link para a URL de verificação](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: O novo usuário recebe um Email com um Link para a URL de verificação ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Etapa de CreateUserWizard padrão do controle CreateUserWizard exibe uma mensagem informando ao usuário sua conta foi criada e exibe um botão continuar. Se você clicar nesta leva o usuário para a URL especificada pelo controle de `ContinueDestinationPageUrl` propriedade. O CreateUserWizard na `EnhancedCreateUserWizard.aspx` está configurado para enviar os novos usuários a `~/Membership/AdditionalUserInfo.aspx`, que solicita ao usuário sua cidade natal, a URL da home page e a assinatura. Como essas informações só podem ser adicionadas por usuários conectados, faz sentido para atualizar essa propriedade para enviar aos usuários para a home page do site (`~/Default.aspx`). Além disso, o `EnhancedCreateUserWizard.aspx` página ou a etapa CreateUserWizard deve ser aumentada para informar ao usuário que eles receberam um email de verificação e sua conta não será ativada até que eles sigam as instruções neste email. Vou deixar essas modificações como um exercício para o leitor.


### <a name="creating-the-verification-page"></a>Criando a página de verificação

Nossa tarefa final é criar o `Verification.aspx` página. Adicione esta página para a pasta raiz, associando-o `Site.master` página mestra. Assim como fizemos com a maioria das páginas de conteúdo anteriores adicionadas ao site, remova o controle de conteúdo que faz referência a `LoginContent` ContentPlaceHolder para que a página de conteúdo usa a página mestra padrão conteúdo.

Adicionar um controle de Web de rótulo para o `Verification.aspx` , defina sua `ID` para `StatusMessage` e limpar sua propriedade text. Em seguida, crie o `Page_Load` manipulador de eventos e adicione o seguinte código:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

A maior parte do código acima verifica se o `UserId` fornecido por meio de querystring existe, que é válido `Guid` valor e que ela faz referência a uma conta de usuário. Se todas essas verificações passarem, a conta de usuário for aprovada; Caso contrário, será exibida uma mensagem de status adequado.

A Figura 7 mostra o `Verification.aspx` página quando acessadas por meio de um navegador.


[![A nova conta de usuário é agora aprovadas](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: conta de usuário nova a é agora aprovadas ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Resumo

Todas as contas de usuário de associação tem dois status que determinam se o usuário pode fazer logon no site: `IsLockedOut` e `IsApproved`. Ambas as propriedades devem ser `true` para o usuário faça logon.

O usuário do bloqueado status é usado como uma medida de segurança para reduzir a probabilidade de um hacker invadir um site por meio de métodos de força bruta. Especificamente, um usuário é bloqueado se houver um determinado número de tentativas inválidas de logon dentro de um determinado intervalo de tempo. Esses limites são configuráveis por meio das configurações de provedor de associação no `Web.config`.

O status aprovado normalmente é usado como um meio para impedir que novos usuários de fazer logon até que alguma ação ocorreu. Talvez o site requer que novas contas primeiro sejam aprovadas pelo administrador ou, como vimos na etapa 3, verificando seu endereço de email.

Boa programação!

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](recovering-and-changing-passwords-cs.md)
> [Próximo](building-an-interface-to-select-one-user-account-from-many-vb.md)
