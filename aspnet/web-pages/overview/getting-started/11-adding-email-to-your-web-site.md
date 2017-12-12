---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "Enviar Email de um Web ASP.NET páginas Site (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este capítulo explica como enviar uma mensagem de email automatizada de um site."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2db94088a04e87c6ab89df26a7a7b0e2a2653a1a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Enviar Email de um Site ASP.NET da páginas (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como enviar uma mensagem de email de um site quando você usa páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como enviar uma mensagem de email de seu site.
> - Como anexar um arquivo a uma mensagem de email.
> 
> Este é o recurso ASP.NET introduzido no artigo:
> 
> - O `WebMail` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Enviar mensagens de Email do seu site

Há inúmeras razões por que você talvez precise enviar email de seu site. Você pode enviar mensagens de confirmação para os usuários, ou você pode enviar notificações a você mesmo (por exemplo, um novo usuário registrado.) O `WebMail` auxiliar torna mais fácil de enviar email.

Para usar o `WebMail` auxiliar, que você precisa ter acesso a um servidor SMTP. (SMTP significa *Simple Mail Transfer Protocol*.) Um servidor SMTP é um servidor de email que só encaminha mensagens para o destinatário server &#8212; é o lado de saída de emails. Se você usar um provedor de hospedagem para o seu site, eles provavelmente configurá-las com email e elas podem dizer o que é o nome do servidor SMTP. Se você estiver trabalhando em uma rede corporativa, um administrador ou o departamento de TI pode geralmente fornecer as informações sobre um servidor SMTP que você pode usar. Se você estiver trabalhando em casa, você ainda poderá testar usando seu provedor de email comum, que pode informar o nome de seu servidor SMTP. Normalmente são necessários:

- O nome do servidor SMTP.
- O número da porta. Isso é quase sempre 25. No entanto, seu provedor pode exigir que você usar a porta 587. Se você estiver usando o protocolo SSL (SSL) para email, talvez seja necessário uma porta diferente. Verifique com seu provedor de email.
- Credenciais (nome de usuário, senha).

Neste procedimento, você criará duas páginas. A primeira página tem um formulário que permite aos usuários inserir uma descrição, como se eles foram preenchendo um formulário de suporte técnico. A primeira página envia suas informações para uma segunda página. Na segunda página, o código extrai as informações do usuário e envia uma mensagem de email. Ele também exibe uma mensagem confirmando que o relatório de problemas foi recebido.

![[imagem]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Para simplificar este exemplo, o código inicializa o `WebMail` direita de auxiliar na página onde você usá-lo. No entanto, para sites reais, é recomendável que você coloque o código de inicialização como isso em um arquivo global, para que você inicializar a `WebMail` auxiliar para todos os arquivos no seu site. Para obter mais informações, consulte [personalizar o comportamento de todo o Site para o ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Crie um novo site.
2. Adicionar uma nova página chamada *EmailRequest.cshtml* e adicione a seguinte marcação: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Observe que o `action` foi definido para o atributo do elemento form *ProcessRequest.cshtml*. Isso significa que o formulário será enviado para a página em vez de volta para a página atual.
3. Adicionar uma nova página chamada *ProcessRequest.cshtml* para o site e adicione o seguinte código e marcação:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    O código, você obtém os valores dos campos de formulário que foram enviados para a página. Em seguida, chamar o `WebMail` do auxiliar `Send` método para criar e enviar a mensagem de email. Nesse caso, os valores a serem usados são constituídos por texto concatenar com os valores que foram enviados do formulário.

    O código para essa página está dentro de um `try/catch` bloco. Se por qualquer motivo, a tentativa de enviar um email não funciona (por exemplo, as configurações não são à direita), o código de `catch` bloco é executado e define o `errorMessage` variável para o erro que ocorreu. (Para obter mais informações sobre `try/catch` blocos ou `<text>` marca, consulte [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    No corpo da página, se o `errorMessage` variável está vazia (o padrão), o usuário vê uma mensagem de que a mensagem de email foi enviada. Se o `errorMessage` variável é definida como true, o usuário vê uma mensagem que houve um problema ao enviar a mensagem.

    Observe que, na parte da página que exibe uma mensagem de erro, há um teste adicional: `if(debuggingFlag)`. Essa é uma variável que pode ser definido como verdadeiro se você estiver tendo problemas para enviar email. Quando `debuggingFlag` for true, e se houver um problema ao enviar o email, uma mensagem de erro adicional é exibida que mostra o ASP.NET relatou ao tentar enviar a mensagem de email. Aviso razoável, embora: as mensagens de erro ASP.NET relata quando ele não é possível enviar uma mensagem de email podem ser genéricas. Por exemplo, se ASP.NET não pode contatar o servidor SMTP (por exemplo, porque você fez um erro no nome do servidor), o erro é `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** quando você receber uma mensagem de erro de um objeto de exceção (`ex` no código), faça *não* rotineiramente passar por meio de mensagem aos usuários. Objetos de exceção geralmente incluem informações que os usuários não podem ver e até mesmo que pode ser uma vulnerabilidade de segurança. É por isso que esse código inclui a variável `debuggingFlag` que é usada como uma opção para exibir a mensagem de erro, e por que a variável por padrão é definida como false. Você deve definir essa variável como true (e, portanto, exibir a mensagem de erro) *somente* se você tiver um problema com o envio de email e você precisa depurar. Depois de corrigir os problemas, defina `debuggingFlag` para false.

    Modificar configurações relacionadas no código de email a seguir:

    - Defina `your-SMTP-host` para o nome do servidor SMTP que você tem acesso ao.
    - Defina `your-user-name-here` para o nome de usuário para sua conta do servidor SMTP.
    - Definir `your-account-password` para a senha de sua conta do servidor SMTP.
    - Definir `your-email-address-here` para seu próprio endereço de email. Este é o endereço de email que a mensagem é enviada. (Alguns provedores de email não permitem que você especifique outro `From` endereço e usará o nome de usuário como o `From` endereço.)

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a>Configurar as configurações de Email
    > 
    > Ele pode ser um desafio, às vezes, verifique se que você tem as configurações corretas para o servidor SMTP, número da porta e assim por diante. Veja a seguir algumas dicas:
    > 
    > - O nome do servidor SMTP costuma ser algo como `smtp.provider.com` ou `smtp.provider.net`. No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse momento pode ser `localhost`. Isso ocorre porque, após ter publicado seu site estiver sendo executado no servidor do provedor, o servidor de email pode ser local da perspectiva do seu aplicativo. Essa alteração nos nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte do processo de publicação.
    > - O número da porta normalmente é 25. No entanto, alguns provedores exigem que você usar a porta 587 ou alguma outra porta.
    > - Certifique-se de que você use as credenciais corretas. Se você tiver publicado seu site para um provedor de hospedagem, use as credenciais que o provedor indicou especificamente são para email. Eles podem ser diferentes das credenciais que você usa para publicar.
    > - Às vezes, você não precisará de credenciais em todos os. Se você estiver enviando um email usando o provedor pessoal, seu provedor de email já saberá suas credenciais. Depois de publicar, você precisará usar credenciais diferentes quando você testar no computador local.
    > - Se seu provedor de email usa criptografia, você deve definir `WebMail.EnableSsl` para `true`.
4. Execute o *EmailRequest.cshtml* página em um navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.)
5. Insira seu nome e uma descrição do problema e, em seguida, clique no **enviar** botão. Você será redirecionado para a *ProcessRequest.cshtml* página, que confirma a mensagem e que envia uma mensagem de email. 

    ![[imagem]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Envia um arquivo usando o Email

Você também pode enviar arquivos anexados a mensagens de email. Neste procedimento, você criará um arquivo de texto e duas páginas HTML. Você usará o arquivo de texto como um anexo de email.

1. No site, adicione um novo arquivo de texto e nomeie- *MyFile.txt*.
2. Copie o seguinte texto e cole-o no arquivo: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Crie uma página chamada *SendFile.cshtml* e adicione a seguinte marcação: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Crie uma página chamada *ProcessFile.cshtml* e adicione a seguinte marcação: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modificar configurações relacionadas no código do exemplo de email a seguir:

    - Defina `your-SMTP-host` para o nome de um servidor SMTP que você tem acesso ao.
    - Defina `your-user-name-here` para o nome de usuário para sua conta do servidor SMTP.
    - Definir `your-email-address-here` para seu próprio endereço de email. Este é o endereço de email que a mensagem é enviada.
    - Definir `your-account-password` para a senha de sua conta do servidor SMTP.
    - Definir `target-email-address-here` para seu próprio endereço de email. (Como antes, normalmente você poderia enviar um email para outra pessoa, mas para teste, é possível enviar a mesmo.)
6. Execute o *SendFile.cshtml* página em um navegador.
7. Digite seu nome, uma linha de assunto e o nome do arquivo de texto para anexar (*MyFile.txt*).
8. Clique no botão `Submit`. Como antes, você será redirecionado para a *ProcessFile.cshtml* página, que confirma a mensagem e que envia uma mensagem de email com o arquivo anexado.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Guia de solução de problemas (Razor) de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protocolo SMTP](https://msdn.microsoft.com/en-us/library/aa480435.aspx)
- [Personalizar o comportamento de todo o Site para páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
