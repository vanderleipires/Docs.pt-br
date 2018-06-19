---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizar o comportamento de todo o Site para o ASP.NET Web Pages Sites (Razor) | Microsoft Docs
author: tfitzmac
description: Este capítulo explica como fazer configurações para seu site inteiro ou uma pasta inteira, em vez de apenas uma página.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899195"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizar o comportamento de todo o Site para Sites de páginas (Razor) da Web ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como fazer configurações de site para as páginas em um site de páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como executar um código que lhe permite conjunto de valores (valores globais ou as configurações de auxiliar) para todas as páginas em um site.
> - Como executar o código que permite que você defina valores para todas as páginas em uma pasta.
> - Como executar código antes e depois de uma página é carregado.
> - Como enviar erros para uma página de erro central.
> - Como adicionar autenticação a todas as páginas em uma pasta.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (pacote do NuGet)
>   
> 
> Este tutorial também funciona com o ASP.NET Web Pages 3 e o Visual Studio 2013 (ou o Visual Studio Express 2013 para Web), exceto que você não pode usar o ASP.NET Web Helpers Library.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Adicionando o código de inicialização do site para páginas da Web ASP.NET

Para a maior parte do código que você escrever em páginas da Web do ASP.NET, uma página individual pode conter todo o código necessário para a página. Por exemplo, se uma página envia uma mensagem de email, é possível colocar todo o código para essa operação em uma única página. Isso pode incluir o código para inicializar as configurações para enviar email (ou seja, para o servidor SMTP) e para enviar a mensagem de email.

No entanto, em algumas situações, você talvez queira executar algum código antes de executa qualquer página no site. Isso é útil para definir valores que podem ser usados em qualquer lugar no site (conhecido como *valores globais*.) Por exemplo, alguns auxiliares exigem que você forneça valores como configurações de email ou chaves de conta. Ele pode ser útil para manter essas configurações em valores globais.

Você pode fazer isso criando uma página chamada  *\_AppStart.cshtml* na raiz do site. Se não houver essa página, ele é executado na primeira vez que qualquer página no site é solicitada. Portanto, é um bom lugar para executar o código para definir os valores globais. (Como  *\_AppStart.cshtml* tem um prefixo de sublinhado, ASP.NET não enviará a página em um navegador, mesmo se os usuários solicitarem diretamente.)

O diagrama a seguir mostra como o  *\_AppStart.cshtml* página funciona. Quando chega uma solicitação para uma página, e se esta for a primeira solicitação para qualquer página no site, o ASP.NET primeiro verifica se um  *\_AppStart.cshtml* página existe. Nesse caso, qualquer código de  *\_AppStart.cshtml* página é executado, e, em seguida, a página solicitada é executado.

![[imagem]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Definindo valores globais para o seu site

1. Na pasta raiz de um site do WebMatrix, crie um arquivo chamado  *\_AppStart.cshtml*. O arquivo deve estar na raiz do site.
2. Substitua o conteúdo existente com o seguinte: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Esse código armazena um valor de `AppState` dicionário, que está disponível automaticamente para todas as páginas no site. Observe que o  *\_AppStart.cshtml* arquivo não tem qualquer marcação nele. A página executa o código e, em seguida, Redirecione para a página originalmente solicitada.

    > [!NOTE]
    > Tenha cuidado ao colocar o código no  *\_AppStart.cshtml* arquivo. Se ocorrerem erros no código do  *\_AppStart.cshtml* arquivo, o site não será iniciado.
3. Na pasta raiz, crie uma nova página chamada *AppName.cshtml*.
4. Substitua a marcação padrão e o código com o seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Esse código extrai o valor do `AppState` que podem ser definidas no objeto de  *\_AppStart.cshtml* página.
5. Execute o *AppName.cshtml* página em um navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.) A página exibe o valor global. 

    ![[imagem]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Valores de configuração para os auxiliares

Um bom uso para o  *\_AppStart.cshtml* arquivo é definir valores para os auxiliares que podem ser usadas em seu site e que precisam ser inicializados. Exemplos comuns são as configurações de email para o `WebMail` auxiliar e as chaves públicas e privadas para o `ReCaptcha` auxiliar. Em casos assim, você pode definir os valores de uma vez no  *\_AppStart.cshtml* e, em seguida, eles são já definidos para todas as páginas em seu site.

Este procedimento mostra como definir `WebMail` configurações globalmente. (Para obter mais informações sobre como usar o `WebMail` auxiliar, consulte [Email adicionando a um Site de páginas da Web ASP.NET](../getting-started/11-adding-email-to-your-web-site.md).)

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.
2. Se você ainda não tiver um  *\_AppStart.cshtml* de arquivo, na pasta raiz de um site, crie um arquivo chamado  *\_AppStart.cshtml*.
3. Adicione o seguinte `WebMail` configurações para o  *\_AppStart.cshtml* arquivo: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modificar configurações relacionadas no código de email a seguir:

   - Defina `your-SMTP-host` para o nome do servidor SMTP que você tem acesso ao.
   - Defina `your-user-name-here` para o nome de usuário para sua conta do servidor SMTP.
   - Definir `your-account-password` para a senha de sua conta do servidor SMTP.
   - Definir `your-email-address-here` para seu próprio endereço de email. Este é o endereço de email que a mensagem é enviada. (Alguns provedores de email não permitem que você especifique outro `From` endereço e usará o nome de usuário como o `From` endereço.)

     Para obter mais informações sobre as configurações de SMTP, consulte [definindo as configurações de Email](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) no artigo [Enviar Email de um Site de páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) e [problemas com o envio de Email](https://go.microsoft.com/fwlink/?LinkId=253001#email)no [(Razor) guia de solução de problemas de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Salve o  *\_AppStart.cshtml* de arquivo e fechá-la.
5. Na pasta raiz de um site, crie a nova página chamada *TestEmail.cshtml*.
6. Substitua o conteúdo existente com o seguinte: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Execute o *TestEmail.cshtml* página em um navegador.
8. Preencha os campos para enviar uma mensagem de email e, em seguida, clique em **enviar**.
9. Verifique seu email para verificar se que você tiver chegado a mensagem.

A parte importante deste exemplo é que as configurações que você normalmente não são alterados — como o nome do servidor SMTP e suas credenciais de email — são definidas no  *\_AppStart.cshtml* arquivo. Dessa forma, que não é necessário defini-las novamente em cada página para onde enviar o email. (Embora se por algum motivo, você precisa alterar essas configurações, você pode defini-las individualmente em uma página.) Na página, você apenas definir os valores que normalmente mudar cada vez, como o destinatário e o corpo da mensagem de email.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Executar código antes e depois de arquivos em uma pasta

Assim como você pode usar  *\_AppStart.cshtml* para escrever código antes de executam páginas no site, você pode escrever código que é executado antes (e depois) qualquer página em uma pasta específica de execução. Isso é útil para coisas como configurar a mesma página de layout para todas as páginas em uma pasta, ou para a verificação de que um usuário está conectado antes de executar uma página na pasta.

Para as páginas em particular pastas, você pode criar código em um arquivo chamado  *\_Pagestart*. O diagrama a seguir mostra como o  *\_Pagestart* página funciona. Quando uma solicitação chega para uma página, o ASP.NET verifica primeiro para um  *\_AppStart.cshtml* executa que e página. Em seguida, o ASP.NET verifica se há um  *\_Pagestart* página e, nesse caso, executa que. Ele é executado, em seguida, a página solicitada.

Dentro de  *\_Pagestart* página, você pode especificar onde durante processamento que você deseja que a página solicitada a execução, incluindo um `RunPage` método. Isso permite executar código antes de executa a página solicitada e, em seguida, novamente depois. Se você não incluir `RunPage`, todo o código em  *\_Pagestart* é executado e, em seguida, a página solicitada é executado automaticamente.

![[imagem]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET permite que você crie uma hierarquia de  *\_Pagestart* arquivos. Você pode colocar um  *\_Pagestart* arquivo na raiz do site e em qualquer subpasta. Quando uma página é solicitada, o  *\_Pagestart* arquivo em que a execução de nível superior (mais próximos a raiz do site), seguido de  *\_Pagestart* arquivo na próxima subpasta, e assim por diante na estrutura de subpasta até que a solicitação atinja a pasta que contém a página solicitada. Após todos os aplicável  *\_Pagestart* arquivos tem executado, a página solicitada é executada.

Por exemplo, você pode ter a seguinte combinação de  *\_Pagestart* arquivos e *cshtml* arquivo:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Quando você executa */myfolder/default.cshtml*, você verá o seguinte:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Executando o código de inicialização para todas as páginas em uma pasta

Um bom uso  *\_Pagestart* de arquivos é a mesma página de layout para todos os arquivos em uma única pasta de inicialização.

1. Na pasta raiz, crie uma nova pasta chamada *InitPages*.
2. No *InitPages* pasta do seu site, crie um arquivo chamado  *\_Pagestart* e substitua a marcação padrão e o código a seguir: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Na raiz do site, crie uma pasta chamada *compartilhado*.
4. No *compartilhado* pasta, crie um arquivo chamado  *\_Layout1.cshtml* e substitua a marcação padrão e o código a seguir: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. No *InitPages* pasta, crie um arquivo chamado *Content1.cshtml* e substitua o conteúdo existente pelo seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. No *InitPages* pasta, criar outro arquivo chamado *Content2.cshtml* e substitua a marcação padrão pelo seguinte: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Executar *Content1.cshtml* em um navegador. 

    ![[imagem]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Quando o *Content1.cshtml* página é executado, o  *\_Pagestart* conjuntos de arquivos `Layout` e também define `PageData["MyBackground"]` como uma cor. Em *Content1.cshtml*, o layout e cores são aplicadas.
8. Exibição *Content2.cshtml* em um navegador. 

    O layout é o mesmo, porque as duas páginas usam a mesma página de layout e cores como inicializado em  *\_Pagestart*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Usando \_Pagestart para lidar com erros

Use outro bom para o  *\_Pagestart* arquivo é criar uma maneira de lidar com erros de programação (exceções) que podem ocorrer em qualquer *. cshtml* página em uma pasta. Este exemplo mostra uma maneira de fazer isso.

1. Na pasta raiz, crie uma pasta chamada *InitCatch*.
2. No *InitCatch* pasta do seu site, crie um arquivo chamado  *\_Pagestart* e substitua a marcação existente e o código a seguir: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    Nesse código, tente executar a página solicitada explicitamente chamando o `RunPage` método dentro de um `try` bloco. Se ocorrer algum erro de programação em que a solicitação de página, o código dentro do `catch` bloco é executado. Nesse caso, o código redireciona para uma página (*Error.cshtml*) e passa o nome do arquivo que ocorreu o erro como parte da URL. (Você criará a página em breve.)
3. No *InitCatch* pasta do seu site, crie um arquivo chamado *Exception.cshtml* e substitua a marcação existente e o código a seguir: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Para fins deste exemplo, o que você está fazendo nesta página é deliberadamente criando um erro ao tentar abrir um arquivo de banco de dados que não existe.
4. Na pasta raiz, crie um arquivo chamado *Error.cshtml* e substitua a marcação existente e o código a seguir: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Nesta página, a expressão `@Request["source"]` obtém o valor da URL e o exibe.
5. Na barra de ferramentas, clique em **salvar**.
6. Executar *Exception.cshtml* em um navegador. 

    ![[imagem]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Porque ocorre um erro em *Exception.cshtml*, o  *\_Pagestart* redirecionamentos de página para o *Error.cshtml* arquivo, que exibe a mensagem.

    Para obter mais informações sobre exceções, consulte [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Usando \_Pagestart para restringir o acesso à pasta

Você também pode usar o  *\_Pagestart* arquivo para restringir o acesso a todos os arquivos em uma pasta.

1. No WebMatrix, crie um novo site usando o **Site a partir do modelo** opção.
2. Os modelos disponíveis, selecione **Site inicial**.
3. Na pasta raiz, crie uma pasta chamada *AuthenticatedContent*.
4. No *AuthenticatedContent* pasta, crie um arquivo chamado  *\_Pagestart* e substitua a marcação existente e o código a seguir: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    O código começa, impedindo que todos os arquivos na pasta que está sendo armazenado em cache. (Isso é necessário para cenários como computadores públicos, onde você não deseja páginas armazenadas em cache de um usuário esteja disponível para o próximo usuário.) Em seguida, o código determina se o usuário tem conectado ao site antes que possam exibir qualquer uma das páginas na pasta. Se o usuário não está conectado, o código redireciona para a página de logon. A página de logon pode retornar o usuário para a página originalmente solicitada se você incluir um valor de cadeia de caracteres de consulta chamado `ReturnUrl`.
5. Criar uma nova página no *AuthenticatedContent* pasta denominada *Page.cshtml*.
6. Substitua a marcação padrão pelo seguinte:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Executar *Page.cshtml* em um navegador. O código redireciona para uma página de logon. Você deve registrar antes de efetuar login. Depois de registrado e conectado, você pode navegar até a página e exibir seu conteúdo.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Introdução a páginas da Web ASP.NET usando a sintaxe do Razor de programação](https://go.microsoft.com/fwlink/?LinkID=251587)
