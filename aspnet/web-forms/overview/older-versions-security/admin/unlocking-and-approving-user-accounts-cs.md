---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: "Desbloquear e aprovar as contas de usuário (c#) | Microsoft Docs"
author: rick-anderson
description: "Este tutorial mostra como criar uma página da web para os administradores gerenciarem bloqueada e status de aprovação dos usuários. Também veremos como aprovar o novo de usuários..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 65d32309cbd8bed6decbba4c5027d8e10a558ae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="unlocking-and-approving-user-accounts-c"></a>Contas de usuário de desbloqueio e aprovação (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Este tutorial mostra como criar uma página da web para os administradores gerenciarem bloqueada e status de aprovação dos usuários. Também veremos como aprovar novos usuários somente depois que eles verificar seu endereço de email.


## <a name="introduction"></a>Introdução

Junto com um nome de usuário, senha e email, cada conta de usuário tem dois campos de status que determinam se o usuário pode fazer logon no site: bloqueada e aprovada. Um usuário está bloqueado automaticamente se eles fornecem credenciais inválidas um número especificado de vezes dentro de um número especificado de minutos (as configurações padrão bloquearem um usuário após 5 tentativas de logon inválidas dentro de 10 minutos). O status aprovado é útil em cenários em que alguma ação deve ocorrer antes que um novo usuário é capaz de fazer logon site. Por exemplo, um usuário precisará primeiro verificar seu endereço de email ou ser aprovado por um administrador para poder logon.

Porque um bloqueado não aprovados ou fora o usuário não pode fazer logon, é natural apenas para saber como esses status podem ser redefinidas. ASP.NET não inclui nenhuma funcionalidade interna ou controles da Web para o gerenciamento de usuários bloqueados e aprovada status, em parte porque essas decisões que precisam ser manipulados em uma base de site a site. Alguns sites podem aprovar automaticamente todas as novas contas de usuário (o comportamento padrão). Outras pessoas tenham um administrador aprove novas contas ou não aprove usuários até que eles visitam um link enviado para o endereço de email fornecido quando eles se inscreveu. Da mesma forma, alguns sites podem bloquear usuários até que um administrador redefine o status, enquanto outros sites enviar um email para o usuário bloqueado com uma URL que eles podem visitar para desbloquear sua conta.

Este tutorial mostra como criar uma página da web para os administradores gerenciarem bloqueada e status de aprovação dos usuários. Também veremos como aprovar novos usuários somente depois que eles verificar seu endereço de email.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Etapa 1: Gerenciar usuários bloqueados e status de aprovação

No <a id="Tutorial12"> </a> [ *criação de uma Interface para selecionar uma conta de usuário de muitos* ](building-an-interface-to-select-one-user-account-from-many-cs.md) tutorial é construída uma página listada cada conta de usuário no paginado, filtrados GridView. A grade lista o nome de cada usuário e email, seus status aprovados e bloqueadas, se está online no momento e quaisquer comentários sobre o usuário. Para gerenciar usuários aprovados e bloqueada status, poderíamos criar esta grade editável. Para alterar o status aprovado do usuário, o administrador deve primeiro localize a conta de usuário e, em seguida, edite a linha correspondente do GridView, marcando ou desmarcando a caixa de seleção aprovada. Como alternativa, podemos pode gerenciar os status aprovados e bloqueados por meio de uma página separada do ASP.NET.

Para este tutorial vamos usar duas páginas do ASP.NET: `ManageUsers.aspx` e `UserInformation.aspx`. A ideia aqui é que `ManageUsers.aspx` lista as contas de usuário no sistema, enquanto `UserInformation.aspx` permite que o administrador gerencie os status aprovados e bloqueados para um usuário específico. Nossa empresa é aumentar o GridView no `ManageUsers.aspx` para incluir um HyperLinkField, que é renderizado como uma coluna de links. Queremos que cada link para apontar para `UserInformation.aspx?user=UserName`, onde *UserName* é o nome do usuário para editar.

> [!NOTE]
> Se você baixou o código para o <a id="Tutorial13"> </a> [ *Recovering e alterar senhas* ](recovering-and-changing-passwords-cs.md) tutorial, você deve ter notado que o `ManageUsers.aspx` página já contém um conjunto de " Gerenciar"links e `UserInformation.aspx` página fornece uma interface para alterar a senha do usuário selecionado. Decidi não replicar essa funcionalidade no código associado com este tutorial, porque ele trabalhou contornar a API de associação e operando diretamente com o banco de dados do SQL Server para alterar a senha do usuário. Este tutorial começa do zero com o `UserInformation.aspx` página.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Adicionando "gerenciar" Links para o`UserAccounts`GridView

Abra o `ManageUsers.aspx` página e adicione um HyperLinkField para o `UserAccounts` GridView. Definir o HyperLinkField `Text` propriedade como "Manage" e seu `DataNavigateUrlFields` e `DataNavigateUrlFormatString` propriedades `UserName` e "UserInformation.aspx?user={0}", respectivamente. Essas configurações definem o HyperLinkField, de modo que todos os hiperlinks exibem o texto "Manage", mas cada link passa no respectivo *UserName* valor na querystring.

Depois de adicionar o HyperLinkField a GridView, reserve um tempo para exibir o `ManageUsers.aspx` página através de um navegador. Como mostra a Figura 1, cada linha GridView agora inclui um link "Gerenciar". O link "Gerenciar" para Bruce aponta para `UserInformation.aspx?user=Bruce`, enquanto que o link "Gerenciar" para Dave aponta para `UserInformation.aspx?user=Dave`.


[![Adiciona o HyperLinkField um](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: O HyperLinkField adiciona um Link "Gerenciar" para cada conta de usuário ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Criaremos a interface do usuário e código para o `UserInformation.aspx` página em um momento, mas primeiro vamos falar sobre como alterar programaticamente um usuário bloqueado e status de aprovação. O [ `MembershipUser` classe](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx) tem [ `IsLockedOut` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.islockedout.aspx) e [ `IsApproved` propriedades](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.isapproved.aspx). A propriedade `IsLockedOut` é somente leitura. Não há nenhum mecanismo para bloquear um usuário; programaticamente para desbloquear um usuário, use o `MembershipUser` da classe [ `UnlockUser` método](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.unlockuser.aspx). O `IsApproved` propriedade está legível e gravável. Para salvar as alterações feitas nessa propriedade, precisamos chamar o `Membership` da classe [ `UpdateUser` método](https://msdn.microsoft.com/en-us/library/system.web.security.membership.updateuser.aspx), passando o `MembershipUser` objeto.

Porque o `IsApproved` é legível e gravável, um controle de caixa de seleção é provavelmente o elemento de interface do usuário recomendado para configurar essa propriedade. No entanto, uma caixa de seleção não funcionará para o `IsLockedOut` propriedade porque um administrador não pode bloquear um usuário, ela só pode desbloquear um usuário. Uma interface de usuário adequados para o `IsLockedOut` propriedade é um botão que, quando clicado, desbloqueie a conta de usuário. Esse botão deve ser habilitado apenas se o usuário está bloqueado.

### <a name="creating-theuserinformationaspxpage"></a>Criando o`UserInformation.aspx`página

Agora estamos prontos para implementar a interface do usuário em `UserInformation.aspx`. Abrir essa página e adicione os seguintes controles da Web:

- Um controle HyperLink que, quando clicado, retorna o administrador a `ManageUsers.aspx` página.
- Um controle de rótulo Web para exibir o nome do usuário selecionado. Definir este rótulo `ID` para `UserNameLabel` e limpar seu `Text` propriedade.
- Um controle de caixa de seleção denominado `IsApproved`. Definir seu `AutoPostBack` propriedade `true`.
- Um controle de rótulo para exibir o usuário do último bloqueado Data. Este rótulo de nome `LastLockedOutDateLabel` e limpar seu `Text` propriedade.
- Um botão para desbloquear o usuário. Nomeie esse botão `UnlockUserButton` e defina seu `Text` propriedade como "Desbloquear usuário".
- Um controle de rótulo para exibir mensagens de status, como "status aprovado do usuário foi atualizado." Nome deste controle `StatusMessage`, desmarque-out seu `Text` propriedade e defina seu `CssClass` propriedade `Important`. (O `Important` classe CSS é definida no `Styles.css` arquivo de folha de estilos; ele exibe o texto correspondente em uma fonte grande, vermelha.)

Depois de adicionar esses controles, o modo de exibição de Design no Visual Studio deve ser semelhante para a tela na Figura 2.


[![Criar Interface do usuário para UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: criar a Interface do usuário para `UserInformation.aspx` ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Com a interface de usuário completa, nossa próxima tarefa é definir o `IsApproved` caixa de seleção e outros controles com base nas informações do usuário selecionado. Criar um manipulador de eventos para a página `Load` evento e adicione o seguinte código:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Inicia o código acima, garantindo que se trata de primeira visita a página e não um postback subsequente. Ele lê o nome de usuário passado a `user` campo querystring e recupera informações sobre essa conta de usuário por meio de `Membership.GetUser(username)` método. Se nenhum nome de usuário foi fornecido por meio de querystring, ou se o usuário especificado não pôde ser encontrado, o administrador é enviado de volta ao `ManageUsers.aspx` página.

O `MembershipUser` do objeto `UserName` valor é então exibido no `UserNameLabel` e `IsApproved` caixa de seleção é marcada com base no `IsApproved` o valor da propriedade.

O `MembershipUser` do objeto [ `LastLockoutDate` propriedade](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.lastlockoutdate.aspx) retorna um `DateTime` valor que indica quando o usuário foi bloqueada. Se o usuário nunca foi bloqueado, o valor retornado depende do provedor de associação. Quando uma nova conta é criada, o `SqlMembershipProvider` define o `aspnet_Membership` da tabela `LastLockoutDate` campo `1754-01-01 12:00:00 AM`. O código acima exibe uma cadeia de caracteres vazia a `LastLockoutDateLabel` se o `LastLockoutDate` propriedade ocorre antes do ano 2000; caso contrário, a parte da data de `LastLockoutDate` propriedade é exibida no rótulo. O `UnlockUserButton'` s `Enabled` propriedade é definida para o usuário bloqueado status, que significa que esse botão só será habilitado se o usuário está bloqueado.

Reserve um tempo para testar o `UserInformation.aspx` página através de um navegador. Você, é claro, precisará iniciar em `ManageUsers.aspx` e selecione uma conta de usuário para gerenciar. Após que chegam ao `UserInformation.aspx`, observe que o `IsApproved` caixa de seleção é verificada apenas se o usuário for aprovado. Se o usuário já foi bloqueado, sua última data bloqueada é exibida. Botão Desbloquear usuário estará habilitado apenas se o usuário está bloqueado no momento. Marcar ou desmarcar o `IsApproved` caixa de seleção ou clicar no botão Desbloquear usuário causa um postback, mas sem modificações são feitas para a conta de usuário porque temos ainda para criar manipuladores de eventos para esses eventos.

Retorne ao Visual Studio e criar manipuladores de eventos para o `IsApproved` da caixa de seleção `CheckedChanged` evento e o `UnlockUser` do botão `Click` eventos. No `CheckedChanged` manipulador de eventos, defina o usuário `IsApproved` propriedade para o `Checked` propriedade da caixa de seleção e, em seguida, salve as alterações por meio de uma chamada para `Membership.UpdateUser`. No `Click` manipulador de eventos, simplesmente chamada de `MembershipUser` do objeto `UnlockUser` método. Em ambos os manipuladores de eventos, exibir uma mensagem apropriada no `StatusMessage` rótulo.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Teste o`UserInformation.aspx`página

Com esses manipuladores de eventos em vigor, revise a página aprovadas e um usuário. Como mostra a Figura 3, você deve ver um resumo da mensagem na página indicando que o usuário `IsApproved` propriedade foi modificada com êxito.


[![Carlos foi reprovado](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Chris foi reprovado ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Em seguida, faça logout e tente fazer logon como o usuário cuja conta foi apenas não aprovados. Porque o usuário não for aprovado, eles não podem fazer logon. Por padrão, o controle de logon exibe a mesma mensagem de erro se o usuário não pode fazer logon, independentemente do motivo. Mas no <a id="Tutorial6"> </a> [ *validar credenciais em relação a associação usuário repositório usuário* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) tutorial vimos aprimorando o controle de logon para exibir uma mensagem mais apropriada. Como mostra a Figura 4, Chris é mostrado uma mensagem explicando o que ele não pode fazer logon porque sua conta ainda não foi aprovada.


[![Carlos não é possível porque His conta de logon é reprovado](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris não é possível porque His conta de logon é reprovado ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Para testar a funcionalidade bloqueada, tente fazer logon como um usuário aprovado, mas usar uma senha incorreta. Repita esse processo, o número necessário de vezes até que a conta do usuário foi bloqueada. O controle de logon também foi atualizado para mostrar um personalizado de mensagem se a tentativa de logon em uma conta bloqueada. Você sabe que uma conta foi bloqueada quando você começar a ver a seguinte mensagem na página de logon: "sua conta foi bloqueada devido a muitas tentativas de logon inválidas. Entre em contato com o administrador para configurar o desbloqueio da conta."

Volte para o `ManageUsers.aspx` página e clique no link Gerenciar para o usuário bloqueado. Como mostra a Figura 5, você verá um valor de `LastLockedOutDateLabel` botão Desbloquear o usuário deve ser habilitado. Clique no botão de desbloqueio de usuário para desbloquear a conta de usuário. Depois que você destravar o usuário, eles poderão fazer logon novamente.


[![Dave foi bloqueado para o sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: Dave tem foi bloqueada fora do sistema ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Etapa 2: Especificar novos usuários aprovados Status

O status aprovado é útil em cenários onde você deseja que alguma ação a ser executada antes de um novo usuário é capaz de fazer logon e acessar os recursos específicos do usuário do site. Por exemplo, você pode estar executando um site particular em que todas as páginas, exceto para as páginas de logon e a inscrição, são acessíveis somente para usuários autenticados. No entanto, o que acontece se um estranho atingir seu site, localiza a página de inscrição e cria uma conta? Para evitar que isso aconteça, você poderá mover a página de inscrição para um `Administration` pasta e exigem que um administrador crie manualmente cada conta. Como alternativa, você pode permitir que qualquer pessoa para a inscrição, mas proibir o acesso do site até que um administrador aprove a conta de usuário.

Por padrão, o controle CreateUserWizard aprova novas contas. Você pode configurar esse comportamento usando o controle [ `DisableCreatedUser` propriedade](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Defina essa propriedade como `true` não aprovar novas contas de usuário.

> [!NOTE]
> Por padrão o controle CreateUserWizard logon automaticamente a nova conta de usuário. Esse comportamento é determinado pelo controle de [ `LoginCreatedUser` propriedade](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Porque os usuários não aprovados não podem fazer logon no site, quando `DisableCreatedUser` é `true` a nova conta de usuário não está conectada ao site, independentemente do valor de `LoginCreatedUser` propriedade.


Se você estiver criando programaticamente novas contas de usuário por meio de `Membership.CreateUser` método para criar uma conta de usuário não aprovados usar uma das sobrecargas que aceitam o novo usuário `IsApproved` valor da propriedade como um parâmetro de entrada.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Etapa 3: Aprovar usuários verificando seu endereço de Email

Muitos sites que dão suporte a contas de usuário não aprovem novos usuários até que eles Verifique se eles fornecidos ao registrar o endereço de email. Esse processo de verificação normalmente é usado para impedir bots, spam e outros vigaristas conforme ele requer um endereço de email exclusivo, verificado e adiciona uma etapa extra no processo de inscrição. Com esse modelo, quando um novo usuário se inscreve, elas são enviadas uma mensagem de email que inclui um link para uma página de verificação. Visitando o link o usuário tiver receberam o email e, portanto, se o endereço de email fornecido é válido. A página de verificação é responsável pela aprovação do usuário. Isso pode ocorrer automaticamente, aprovando, portanto, qualquer usuário que atinge essa página, ou somente depois que o usuário fornece algumas informações adicionais, como um [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Para acomodar esse fluxo de trabalho, é preciso primeiro atualize a página de criação de conta para que os novos usuários não aprovados. Abra o `EnhancedCreateUserWizard.aspx` página o `Membership` pasta e definir o controle CreateUserWizard `DisableCreatedUser` propriedade `true`.

Em seguida, é preciso configurar o controle CreateUserWizard para enviar um email para o novo usuário com instruções sobre como verificar sua conta. Em particular, incluem um link no email para o `Verification.aspx` página (que ainda temos para criar), passando o novo usuário `UserId` por meio de querystring. O `Verification.aspx` página irá pesquisar o usuário especificado e marcá-los aprovadas.

### <a name="sending-a-verification-email-to-new-users"></a>Enviar um Email de verificação para novos usuários

Para enviar um email do controle CreateUserWizard, configurar seu `MailDefinition` propriedade adequadamente. Como discutido o <a id="Tutorial13"> </a> [tutorial anterior](recovering-and-changing-passwords-cs.md), os controles ChangePassword e PasswordRecovery incluem um [ `MailDefinition` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) que funciona da mesma maneira como do controle CreateUserWizard.

> [!NOTE]
> Para usar o `MailDefinition` opções de propriedade que você precisa especificar a entrega de email no `Web.config`. Para obter mais informações, consulte [enviar o Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Comece criando um novo modelo de email chamado `CreateUserWizard.txt` no `EmailTemplates` pasta. Use o seguinte texto para o modelo:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Definir o `MailDefinition'` s `BodyFileName` propriedade como "~ / EmailTemplates/CreateUserWizard.txt" e seu `Subject` propriedade como "Bem-vindo ao meu site! Ative sua conta".

Observe que o `CreateUserWizard.txt` modelo de email inclui um `<%VerificationUrl%>` espaço reservado. Isso é onde a URL para o `Verification.aspx` página será colocada. CreateUserWizard substitui automaticamente o `<%UserName%>` e `<%Password%>` espaços reservados com a nova conta nome de usuário e senha, mas não há nenhum interno `<%VerificationUrl%>` espaço reservado. Precisamos manualmente substituí-lo com a URL de verificação apropriadas.

Para fazer isso, crie um manipulador de eventos para o CreateUserWizard [ `SendingMail` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) e adicione o seguinte código:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

O `SendingMail` evento é acionado depois que o `CreatedUser` evento, o que significa que quando o manipulador de eventos acima executa o novo usuário conta já foi criada. Podemos acessar o novo usuário `UserId` valor chamando o `Membership.GetUser` método, passando o `UserName` inseridos no controle CreateUserWizard. Em seguida, a URL de verificação é formada. A instrução `Request.Url.GetLeftPart(UriPartial.Authority)` retorna o `http://yourserver.com` parte da URL; `Request.ApplicationPath` retorna caminho onde o aplicativo raiz. A verificação de URL é definida como `Verification.aspx?ID=userId`. Essas duas cadeias de caracteres, em seguida, são concatenadas para formar a URL completa. Por fim, o corpo da mensagem de email (`e.Message.Body`) tem todas as ocorrências de `<%VerificationUrl%>` substituído com a URL completa.

O efeito líquido é que novos usuários são não aprovados, o que significa que eles não podem fazer logon no site. Além disso, eles são enviados automaticamente um email com um link para a URL de verificação (consulte a Figura 6).


[![O novo usuário recebe um Email com um Link para a URL de verificação](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: O novo usuário recebe um Email com um Link para a URL de verificação ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Etapa de CreateUserWizard padrão do controle CreateUserWizard exibe uma mensagem informando ao usuário sua conta foi criada e exibe um botão de continuação. Se você clicar nesta leva o usuário para a URL especificada pelo controle de `ContinueDestinationPageUrl` propriedade. CreateUserWizard em `EnhancedCreateUserWizard.aspx` está configurado para enviar os novos usuários a `~/Membership/AdditionalUserInfo.aspx`, que solicita ao usuário sua cidade natal, a URL da home page e a assinatura. Como essas informações só podem ser adicionadas por usuários conectados, faz sentido para atualizar esta propriedade para enviar aos usuários para a home page do site (`~/Default.aspx`). Além disso, o `EnhancedCreateUserWizard.aspx` página ou a etapa CreateUserWizard deve ser aumentada para informar ao usuário que eles receberam um email de verificação e sua conta não será ativada até que eles sigam as instruções neste email. Eu deixe essas modificações como um exercício para o leitor.


### <a name="creating-the-verification-page"></a>Criando a página de verificação

Nossa tarefa final é criar o `Verification.aspx` página. Adicione esta página para a pasta raiz, associá-lo com o `Site.master` página mestra. Como fizemos com a maioria das páginas de conteúdo anteriores adicionadas ao site, remova o controle de conteúdo que faz referência a `LoginContent` ContentPlaceHolder para que a página de conteúdo usa a página mestra conteúdo padrão.

Adicionar um controle de Web de rótulo para o `Verification.aspx` , defina seu `ID` para `StatusMessage` e limpar sua propriedade text. Em seguida, crie o `Page_Load` manipulador de eventos e adicione o seguinte código:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

A maior parte do código acima verifica se o `UserId` fornecido por meio de querystring existe, que é uma opção válida `Guid` valor e que ela faz referência a uma conta de usuário. Se todas essas verificações passarem, a conta de usuário for aprovada; Caso contrário, será exibida uma mensagem de status adequado.

A Figura 7 mostra o `Verification.aspx` página quando visitado por meio de um navegador.


[![A nova conta de usuário é agora aprovadas](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: conta de usuário nova a é agora aprovada ([clique para exibir a imagem em tamanho normal](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Resumo

Todas as contas de usuário de associação tem dois status que determinam se o usuário pode fazer logon no site: `IsLockedOut` e `IsApproved`. Essas propriedades devem ser `true` para o usuário faça logon.

O usuário do bloqueado status é usado como uma medida de segurança para reduzir a probabilidade de um hacker invadir a um site por meio de métodos de força bruta. Especificamente, o usuário é bloqueado se houver um determinado número de tentativas de logon inválidas dentro de um determinado período de tempo. Esses limites são configuráveis por meio das configurações de provedor de associação em `Web.config`.

O status aprovado normalmente é usado como um meio para impedir que novos usuários de log em até que alguma ação ocorridos. Talvez o site requer que novas contas primeiro ser aprovadas pelo administrador ou, conforme vimos na etapa 3, verificando seu endereço de email.

Boa programação!

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](recovering-and-changing-passwords-cs.md)
[Próximo](building-an-interface-to-select-one-user-account-from-many-vb.md)
