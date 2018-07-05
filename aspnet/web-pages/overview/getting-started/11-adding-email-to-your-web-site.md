---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Enviar Email de uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este capítulo explica como enviar uma mensagem de email automatizadas de um site.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 8a65a152dc72725b1ff65eefd47890b42cb7ea90
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394042"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Enviar Email de um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como enviar uma mensagem de email de um site quando você usa o ASP.NET Web Pages (Razor).
> 
> O que você aprenderá:
> 
> - Como enviar uma mensagem de email do seu site.
> - Como anexar um arquivo a uma mensagem de email.
> 
> Esse é o recurso ASP.NET introduzido no artigo:
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
> Este tutorial também funciona com ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Enviar mensagens de Email do seu site

Há inúmeras razões por que talvez você precise enviar um email de seu site. Você pode enviar mensagens de confirmação para os usuários, ou você pode enviar notificações para si mesmo (por exemplo, que um novo usuário registrado.) O `WebMail` auxiliar torna mais fácil de enviar email.

Para usar o `WebMail` auxiliar, você precisa ter acesso a um servidor SMTP. (SMTP significa *Simple Mail Transfer Protocol*.) Um servidor SMTP é um servidor de email que apenas encaminha mensagens para o servidor do destinatário &#8212; é o lado de saída de email. Se você usar um provedor de hospedagem para seu site, eles provavelmente configuraram você com o email e elas podem dizer o que é o nome do servidor SMTP. Se você estiver trabalhando em uma rede corporativa, um administrador ou o departamento de TI poderá geralmente oferecem as informações sobre um servidor SMTP que você pode usar. Se você estiver trabalhando em casa, você poderá até mesmo testar usando o provedor de email comum, que pode informar o nome do seu servidor SMTP. Normalmente, você precisa:

- O nome do servidor SMTP.
- O número da porta. Isso quase sempre é 25. No entanto, seu ISP pode exigir que você use a porta 587. Se você estiver usando o protocolo SSL (SSL) para email, talvez seja necessário uma porta diferente. Verifique com seu provedor de email.
- Credenciais (nome de usuário, senha).

Neste procedimento, você criará duas páginas. A primeira página tem um formulário que permite aos usuários inserir uma descrição, como se eles foram preenchendo um formulário de suporte técnico. A primeira página envia suas informações em uma segunda página. Na segunda página, o código extrai as informações do usuário e envia uma mensagem de email. Ele também exibe uma mensagem confirmando que o relatório de problema foi recebido.

![[imagem]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Para simplificar este exemplo, o código inicializa o `WebMail` direita de auxiliar na página em que você usá-lo. No entanto, para sites real, é uma ideia melhor colocar o código de inicialização como este em um arquivo global, para que você inicializa o `WebMail` auxiliar para todos os arquivos em seu site. Para obter mais informações, consulte [Personalizando o comportamento de todo o Site do ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Crie um novo site.
2. Adicionar uma nova página chamada *EmailRequest.cshtml* e adicione a seguinte marcação: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Observe que o `action` atributo do elemento de formulário tiver sido definido *ProcessRequest.cshtml*. Isso significa que o formulário será enviado para a página em vez de volta para a página atual.
3. Adicionar uma nova página chamada *ProcessRequest.cshtml* para o site e adicione o código e a marcação a seguir:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    No código, você obterá os valores dos campos de formulário que foram enviados para a página. Depois, chame o `WebMail` do auxiliar `Send` método para criar e enviar a mensagem de email. Nesse caso, os valores a serem usados são compostos de texto que você concatenar com os valores que foram enviados do formulário.

    O código para essa página está dentro de um `try/catch` bloco. Se por qualquer motivo, a tentativa de enviar um email não funcionar (por exemplo, as configurações não são à direita), o código a `catch` bloco é executado e define o `errorMessage` variável para o erro que ocorreu. (Para obter mais informações sobre `try/catch` blocos ou o `<text>` marca, consulte [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    No corpo da página, se o `errorMessage` variável está vazia (o padrão), o usuário vê uma mensagem de que a mensagem de email foi enviada. Se o `errorMessage` variável é definida como true, o usuário vê uma mensagem que houve um problema ao enviar a mensagem.

    Observe que, na parte da página que exibe uma mensagem de erro, há um teste adicional: `if(debuggingFlag)`. Essa é uma variável que você pode definir como true se você estiver tendo problemas para enviar email. Quando `debuggingFlag` for true, e se houver um problema ao enviar email, uma mensagem de erro adicional é exibida que mostra o que for ASP.NET tiver relatado ao tentar enviar a mensagem de email. Aviso razoável, embora: as mensagens de erro ASP.NET relata quando ele não é possível enviar uma mensagem de email podem ser genéricas. Por exemplo, se ASP.NET não é possível contatar o servidor SMTP (por exemplo, porque você fez um erro no nome do servidor), o erro é `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** quando você obtém uma mensagem de erro de um objeto de exceção (`ex` no código), fazer *não* rotineiramente passar essa mensagem por meio para os usuários. Objetos de exceção geralmente incluem informações que os usuários não podem ver e até mesmo, que pode ser uma vulnerabilidade de segurança. É por isso que esse código inclui a variável `debuggingFlag` que é usado como uma opção para exibir a mensagem de erro, e por que a variável por padrão é definida como false. Você deve definir essa variável como true (e, portanto, exibir a mensagem de erro) *apenas* se você estiver tendo problemas com o envio de email e você precisa depurar. Depois de corrigir quaisquer problemas, defina `debuggingFlag` novamente como falsa.

    Modificar configurações relacionadas no código de email a seguir:

   - Definir `your-SMTP-host` como o nome do servidor SMTP que você tem acesso.
   - Definir `your-user-name-here` ao nome de usuário para sua conta do servidor SMTP.
   - Definir `your-account-password` para a senha de sua conta do servidor SMTP.
   - Definir `your-email-address-here` para seu próprio endereço de email. Esse é o endereço de email que a mensagem é enviada. (Alguns provedores de email não permitem que você especifique uma opção diferente `From` tratar e usará seu nome de usuário como o `From` endereço.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Definir configurações de Email
     > 
     > Ele pode ser um desafio, às vezes, para garantir que você tem as configurações corretas para o servidor SMTP, número da porta e assim por diante. Veja a seguir algumas dicas:
     > 
     > - O nome do servidor SMTP costuma ser algo como `smtp.provider.com` ou `smtp.provider.net`. No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse ponto pode ser `localhost`. Isso ocorre porque depois que você publicou e seu site está em execução no servidor do provedor, o servidor de email pode ser local da perspectiva do seu aplicativo. Essa alteração de nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte do processo de publicação.
     > - Geralmente, o número da porta é 25. No entanto, alguns provedores exigem que você usar a porta 587 ou alguma outra porta.
     > - Certifique-se de que você use as credenciais corretas. Se você publicar seu site em um provedor de hospedagem, use as credenciais que o provedor indicou especificamente são para email. Eles podem ser diferentes das credenciais que usar para publicar.
     > - Às vezes, você não precisa credenciais em todos os. Se você estiver enviando um email usando o seu ISP pessoal, seu provedor de email talvez já saiba suas credenciais. Depois de publicar, você talvez precise usar credenciais diferentes quando você testa em seu computador local.
     > - Se seu provedor de email usa a criptografia, você deve definir `WebMail.EnableSsl` para `true`.
4. Execute o *EmailRequest.cshtml* página em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.)
5. Insira seu nome e uma descrição do problema e, em seguida, clique no **enviar** botão. Você será redirecionado para o *ProcessRequest.cshtml* página, que confirma que sua mensagem e que envia uma mensagem de email. 

    ![[imagem]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Envia um arquivo usando o Email

Você também pode enviar os arquivos que estão anexados a mensagens de email. Neste procedimento, você cria um arquivo de texto e duas páginas HTML. Você usará o arquivo de texto como um anexo de email.

1. No site, adicione um novo arquivo de texto e nomeie- *myfile*.
2. Copie o seguinte texto e cole-o no arquivo: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Crie uma página chamada *SendFile.cshtml* e adicione a seguinte marcação: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Crie uma página chamada *ProcessFile.cshtml* e adicione a seguinte marcação: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modificar configurações relacionadas no código do exemplo de email a seguir:

    - Definir `your-SMTP-host` ao nome de um servidor SMTP que você tem acesso.
    - Definir `your-user-name-here` ao nome de usuário para sua conta do servidor SMTP.
    - Definir `your-email-address-here` para seu próprio endereço de email. Esse é o endereço de email que a mensagem é enviada.
    - Definir `your-account-password` para a senha de sua conta do servidor SMTP.
    - Definir `target-email-address-here` para seu próprio endereço de email. (Como antes, você normalmente poderia enviar um email para outra pessoa, mas para teste, é possível enviar a mesmo.)
6. Execute o *SendFile.cshtml* página em um navegador.
7. Insira seu nome, uma linha de assunto e o nome do arquivo de texto para anexar (*myfile*).
8. Clique no botão `Submit`. Como antes, você será redirecionado para o *ProcessFile.cshtml* página, que confirma que sua mensagem e que envia uma mensagem de email com o arquivo anexado.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protocolo SMTP](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizando o comportamento de todo o Site para páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
