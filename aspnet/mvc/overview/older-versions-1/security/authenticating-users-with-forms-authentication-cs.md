---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: "Autenticar usuários com a forma de autenticação (c#) | Microsoft Docs"
author: microsoft
description: "Saiba como usar o atributo [autorizar] senha proteger páginas específicas em seu aplicativo MVC. Você aprenderá a usar a administração de Site da Web também..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 17bcf02e1351587d64b72ee2b40393e0f748f23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-forms-authentication-c"></a>Autenticar usuários com a autenticação de formulários (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como usar o atributo [autorizar] senha proteger páginas específicas em seu aplicativo MVC. Você aprenderá a usar a ferramenta de administração de Site para criar e gerenciar usuários e funções. Você também aprenderá como configurar o armazenamento de informações de conta e a função de usuário.


O objetivo deste tutorial é explicar como você pode usar formulários de autenticação de senha proteger os modos de exibição em seus aplicativos ASP.NET MVC. Você aprenderá a usar a ferramenta de administração de Site para criar usuários e funções. Você também aprenderá a impedir que usuários não autorizados invocar ações do controlador. Por fim, você aprenderá a configurar onde os nomes de usuário e senhas são armazenados.

#### <a name="using-the-web-site-administration-tool"></a>Usando a ferramenta de administração de Site

Antes de fazermos qualquer outra coisa, deve começar criando alguns usuários e funções. É a maneira mais fácil de criar novos usuários e funções aproveitar a ferramenta de administração de Site da Web do Visual Studio 2008. Você pode iniciar essa ferramenta ao selecionar a opção de menu **projeto, configuração de ASP.NET**. Como alternativa, você pode iniciar a ferramenta de administração de Site, clicando no ícone (um pouco assustador) do martelo atingir o mundo que aparece na parte superior da janela do Gerenciador de soluções (consulte a Figura 1).

**Figura 1 – iniciar a ferramenta de administração de Site**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

Em que a ferramenta de administração de Site, criar novos usuários e funções, selecionando a guia Segurança. Clique o **criar usuário** link para criar um novo usuário chamado Stephen (consulte a Figura 2). Fornecer ao usuário Stephen qualquer senha que você deseja (por exemplo, *segredo*).

**Figura 2 – criar um novo usuário**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Você pode criar novas funções primeiro habilitar funções e definir uma ou mais funções. Habilitar funções clicando o **Habilitar funções** link. Em seguida, crie uma função chamada *administradores* clicando o **criar ou gerenciar funções** link (consulte a Figura 3).

**Figura 3 – Criando uma nova função**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Finalmente, crie um novo usuário chamado Sally e associar Sally com a função de administradores clicando no link de Create User e selecionando os administradores ao criar Sally (consulte a Figura 4).

**Figura 4 – adicionar um usuário a uma função**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Quando todos os resolvido e pronto, você deve ter dois novos usuários nomeados Stephen e Sally. Você também deve ter uma nova função chamada administradores. Sally é um membro da função de administradores e Stephen não é.

#### <a name="requiring-authorization"></a>Exigir autorização

Você pode exigir que um usuário seja autenticado antes do usuário invoca uma ação do controlador, adicionando o atributo [autorizar] para a ação. Você pode aplicar o atributo [autorizar] para uma ação de controlador individuais ou você pode aplicar esse atributo a uma classe inteira de controlador.

Por exemplo, o controlador na listagem 1 expõe uma ação chamada CompanySecrets(). Porque essa ação está decorada com o atributo [autorizar], essa ação não pode ser invocada, a menos que um usuário é autenticado.

**Listando 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Se você chama a ação de CompanySecrets() digitando a URL /Home/CompanySecrets na barra de endereços do navegador e você não for um usuário autenticado, você será redirecionado para o modo de logon automaticamente (consulte a Figura 5).

**Figura 5 – o modo de exibição de logon**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Você pode usar o modo de exibição de logon para inserir seu nome de usuário e senha. Se você não for um usuário registrado, você pode clicar no **registrar** link para navegar até o registro (consulte a Figura 6). Você pode usar o modo de exibição de registro para criar uma nova conta de usuário.

**Figura 6 – o modo de exibição de registro**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Depois que você fizer logon, você pode ver o CompanySecrets (consulte a Figura 7). Por padrão, você continuará a ser registrada até que você feche a janela do navegador.

**Figura 7 – o modo de exibição CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizando por nome de usuário ou função de usuário

Você pode usar o atributo [autorizar] para restringir o acesso a uma ação do controlador para um conjunto específico de usuários ou de um determinado conjunto de funções de usuário. Por exemplo, o controlador Home modificado na listagem 2 contém duas novas ações denominadas StephenSecrets() e AdministratorSecrets().

**A listagem 2 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Somente um usuário com o nome de usuário Stephen pode chamar a ação de StephenSecrets(). Todos os outros usuários ser direcionados para o modo de exibição de logon. A propriedade de usuários aceita uma lista separada por vírgulas de nomes de conta de usuário.

Somente os usuários na função de administradores podem chamar a ação de AdministratorSecrets(). Por exemplo, como Sally é um membro do grupo Administradores, ela pode chamar a ação de AdministratorSecrets(). Todos os outros usuários ser direcionados para o modo de exibição de logon. A propriedade funções aceita uma lista separada por vírgulas de nomes de função.

#### <a name="configuring-authentication"></a>Configuração de autenticação

Neste ponto, você deve estar se perguntando onde as informações de conta e a função de usuário estão sendo armazenadas. Por padrão, as informações são armazenadas no banco de dados (RANU) SQL Express Aspnetdb nomeada localizado no aplicativo do seu aplicativo MVC\_pasta de dados. Este banco de dados é gerado automaticamente pela estrutura do ASP.NET quando você começar a usar a associação.

Para ver o banco de dados Aspnetdb na janela do Gerenciador de soluções, você primeiro precisa selecionar a opção de menu projeto, mostrar todos os arquivos.

Usar o banco de dados SQL Express padrão é bom ao desenvolver um aplicativo. Muito provavelmente, no entanto, você não deseja usar o banco de dados padrão Aspnetdb para um aplicativo de produção. Nesse caso, você pode alterar onde as informações de conta de usuário são armazenadas completando as etapas a seguir:

1. Adicionar os objetos de banco de dados de serviços de aplicativo para o banco de dados de produção - altere sua cadeia de caracteres de conexão do aplicativo para apontar para seu banco de dados de produção

A primeira etapa é adicionar todos os objetos de banco de dados necessários (tabelas e procedimentos armazenados) para seu banco de dados de produção. É a maneira mais fácil para adicionar esses objetos para um novo banco de dados aproveitar o Assistente de instalação do ASP.NET SQL Server (consulte a Figura 8). Você pode iniciar essa ferramenta, abra o Prompt de comando do Visual Studio 2008 no grupo de programas do Microsoft Visual Studio 2008 e executando o seguinte comando no prompt de comando:

ASPNET\_regsql

**Figura 8 – o Assistente de instalação do servidor SQL do ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

O Assistente de instalação do ASP.NET SQL Server permite que você selecione um banco de dados do SQL Server em sua rede e instalar todos os objetos de banco de dados necessários para os serviços do aplicativo ASP.NET. O servidor de banco de dados não é necessário a ser localizado no computador local.

> [!NOTE] 
> 
> Se você não quiser usar o Assistente de instalação do ASP.NET SQL Server, você pode encontrar scripts SQL para adicionar os objetos de banco de dados de serviços de aplicativo na seguinte pasta:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


Depois de criar os objetos de banco de dados necessários, você precisa modificar a conexão de banco de dados usado pelo seu aplicativo MVC. Modifique a cadeia de conexão do ApplicationServices no arquivo de configuração (Web. config) da web para que ele aponte para o banco de dados de produção. Por exemplo, a conexão modificado na listagem 3 aponta para um banco de dados denominado MyProductionDB (a cadeia de conexão do ApplicationServices original foi comentada).

**A listagem 3 – Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurar permissões de banco de dados

Se você usar a segurança integrada para se conectar ao banco de dados, em seguida, você precisará adicionar a conta de usuário correta do Windows como um logon para o banco de dados. A conta correta depende se você estiver usando o ASP.NET Development Server ou serviços de informações da Internet como seu servidor web. A conta de usuário correto também depende de seu sistema operacional.

Se você estiver usando o ASP.NET Development Server (o servidor de web padrão usado pelo Visual Studio), em seguida, seu aplicativo executa no contexto da conta de usuário do Windows. Nesse caso, você precisará adicionar sua conta de usuário do Windows como um logon de servidor de banco de dados.

Como alternativa, se você estiver usando serviços de informações da Internet, em seguida, você precisa adicionar a conta ASPNET ou a conta de serviço de rede/Autoridade NT como um logon de servidor de banco de dados. Se você estiver usando o Windows XP, adicione a conta ASPNET como um logon para o banco de dados. Se você estiver usando um sistema operacional mais recente, como o Windows Vista ou Windows Server 2008, em seguida, adicione a conta de serviço de rede/Autoridade NT como o logon de banco de dados.

Você pode adicionar uma nova conta de usuário para seu banco de dados usando o Microsoft SQL Server Management Studio (consulte a Figura 9).

**Figura 9 – Criando um novo logon do Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Depois de criar o logon necessário, é necessário mapear o logon para um usuário de banco de dados com as funções de banco de dados correto. Clique duas vezes o logon e selecione a guia mapeamento de usuário. Selecione uma ou mais funções de banco de dados de serviços de aplicativo. Por exemplo, para autenticar usuários, você precisa habilitar o aspnet\_associação\_BasicAccess função de banco de dados. Para criar novos usuários, você precisa habilitar o aspnet\_associação\_FullAccess função de banco de dados (consulte a Figura 10).

**Figura 10 – Adicionando funções de banco de dados de serviços de aplicativo**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar a autenticação de formulários ao compilar um aplicativo ASP.NET MVC. Primeiro, você aprendeu a criar novos usuários e funções, tirando proveito da ferramenta de administração de Site da Web. Em seguida, você aprendeu a usar o atributo [autorizar] para impedir que usuários não autorizados invocar ações do controlador. Por fim, você aprendeu como configurar seu aplicativo MVC para armazenar informações de usuário e funções em um banco de dados de produção.

>[!div class="step-by-step"]
[Avançar](authenticating-users-with-windows-authentication-cs.md)
