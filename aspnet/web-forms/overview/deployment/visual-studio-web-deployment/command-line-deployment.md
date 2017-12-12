---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "Implantação de Web do ASP.NET usando o Visual Studio: implantação de linha de comando | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Implantação de Web do ASP.NET usando o Visual Studio: implantação de linha de comando
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão Geral

Este tutorial mostra como chamar a web do Visual Studio publicar pipeline da linha de comando. Isso é útil para situações em que você deseja [automatizar o processo de implantação](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) em vez de fazê-lo manualmente no Visual Studio, normalmente usando um [sistema de controle de versão do código de origem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Fazer uma alteração para implantar

Atualmente, a página sobre exibirá o código de modelo.

![Sobre a página de código de modelo](command-line-deployment/_static/image1.png)

Que será substituído com o código que exibe um resumo do registro do aluno.

Abra o *About* página, exclua todas a marcação dentro de `MainContent` `Content` elemento e inserir a seguinte marcação em seu lugar:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Execute o projeto e selecione o **sobre** página.

![Sobre a página](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Implantar para teste usando a linha de comando

Você não implantar outra alteração de banco de dados, portanto desabilitar dbDacFx banco de dados implantação para o banco de dados ContosoUniversity aspnet. Abra o **publicar na Web** assistente e, em cada um dos três perfis, desmarque de publicação a **Atualizar banco de dados** caixa de seleção a **configurações** guia.

Na página inicial do Windows 8, procure **Prompt de comando do desenvolvedor para VS2012**.

Clique no ícone de **Prompt de comando do desenvolvedor para VS2012** e clique em **executar como administrador**.

Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução com o caminho para o arquivo de solução:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild compila a solução e implanta para o ambiente de teste.

![Saída da linha de comando](command-line-deployment/_static/image3.png)

Abra um navegador e vá para `http://localhost/ContosoUniversity`, em seguida, clique no **sobre** página para verificar se a implantação foi bem-sucedida.

Se você não criou nenhum alunos no teste, você verá uma página vazia sob o **estatísticas de corpo de aluno** título. Vá para o **alunos** , clique em **adicionar aluno**, adicionar alguns alunos e, em seguida, retornar para o **sobre** página para ver as estatísticas de aluno.

![Sobre as páginas no ambiente de teste](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opções de linha de comando de chave

O comando que você inseriu passado duas propriedades e o caminho do arquivo de solução para o MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Implantar a solução em comparação com a implantação de projetos individuais

Especificar o arquivo de solução faz com que todos os projetos na solução a ser criado. Se você tiver vários projetos da web na solução, o comportamento de MSBuild a seguir se aplica:

- As propriedades que você especificar na linha de comando são passadas para cada projeto. Portanto, cada projeto da web deve ter um perfil de publicação com o nome que você especificar. Se você especificar `/p:PublishProfile=Test`, cada projeto da web deve ter um perfil de publicação nomeado *teste*.
- Você pode publicar com êxito um projeto quando outro ainda não compila. Para obter mais informações, consulte o thread de stackoverflow [MSBuild falhará com dois pacotes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Se você especificar um projeto individual em vez de uma solução, você precisa adicionar um parâmetro que especifica a versão do Visual Studio. Se você estiver usando o Visual Studio 2012, a linha de comando seria semelhante ao exemplo a seguir:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

O número de versão para o Visual Studio 2010 é 10.0. Para obter mais informações, consulte [compatibilidade de projeto do Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) no blog do Hashimi Sayed.

### <a name="specifying-the-publish-profile"></a>Especificar o perfil de publicação

Você pode especificar o perfil de publicação por nome ou o caminho completo para o *. pubxml* de arquivos, conforme mostrado no exemplo a seguir:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Métodos com suporte para publicação de linha de comando de publicação da Web

Três métodos de publicar têm suporte para publicação de linha de comando:

- `MSDeploy`-Publica usando a implantação da Web.
- `Package`-Publica, criando um pacote de implantação da Web. Você precisa instalar o pacote separadamente do comando MSBuild que o cria.
- `FileSystem`-Publica copiando arquivos para uma pasta especificada.

### <a name="specifying-the-build-configuration-and-platform"></a>Especifica a plataforma e a configuração de compilação

A plataforma e a configuração de compilação devem ser definidos no Visual Studio ou na linha de comando. Os perfis de publicação incluem propriedades que são nomeadas `LastUsedBuildConfiguration` e `LastUsedPlatform`, mas você não pode definir essas propriedades para determinar como o projeto é compilado. Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) no blog do Hashimi Sayed.

## <a name="deploy-to-staging"></a>Implantar o preparo

Para implantar no Azure, você deve adicionar a senha para a linha de comando. Se você salvou a senha no perfil de publicação no Visual Studio, ele foi armazenado em formato criptografado no seu *. pubxml.user* arquivo. Esse arquivo não é acessado pelo MSBuild quando você fizer uma implantação de linha de comando, você precisará passar a senha em um parâmetro de linha de comando.

1. Copie a senha que você precisa do *. publishsettings* arquivo que você baixou anteriormente para o site de preparo. A senha é o valor da `userPWD` atributo para a implantação da Web `publishProfile` elemento.

    ![Senha de implantação da Web](command-line-deployment/_static/image5.png)
2. Na página inicial do Windows 8, procure **Prompt de comando do desenvolvedor para VS2012**e clique no ícone para abrir o prompt de comando. (Você não precisa abri-lo como administrador neste momento porque você não implantar para o IIS no computador local.)
3. Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução com o caminho para o arquivo de solução e a senha com a sua senha:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Observe que essa linha de comando inclui um parâmetro extra: `/p:AllowUntrustedCertificate=true`. Como este tutorial está sendo gravado, o `AllowUntrustedCertificate` propriedade deve ser definida quando você publica no Azure da linha de comando. Quando a correção para esse bug for lançada, esse parâmetro não será necessário.
4. Abra um navegador e vá para a URL do seu site de preparo e, em seguida, clique no **sobre** página para verificar se a implantação foi bem-sucedida.

    Como você viu anteriormente para o ambiente de teste, talvez você precise criar alguns alunos para ver as estatísticas de **sobre** página.

## <a name="deploy-to-production"></a>Implantar na produção

O processo de implantação na produção é semelhante ao processo de preparo.

1. Copie a senha que você precisa do *. publishsettings* arquivo que você baixou anteriormente para o site de produção.
2. Abra **Prompt de comando do desenvolvedor para VS2012**.
3. Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução com o caminho para o arquivo de solução e a senha com a sua senha:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Para um site de produção real, se também houver uma alteração de banco de dados, normalmente copie o *aplicativo\_offline.htm* o arquivo para o site antes da implantação e excluí-lo após a implantação bem-sucedida.
4. Abra um navegador e vá para a URL do seu site de preparo e, em seguida, clique no **sobre** página para verificar se a implantação foi bem-sucedida.

## <a name="summary"></a>Resumo

Agora você implantou uma atualização do aplicativo por meio da linha de comando.

![Sobre as páginas no ambiente de teste](command-line-deployment/_static/image6.png)

O seguinte tutorial, você verá um exemplo de como estender a web pipeline de publicação. O exemplo mostram como implantar arquivos que não estão incluídos no projeto.

>[!div class="step-by-step"]
[Anterior](deploying-a-database-update.md)
[Próximo](deploying-extra-files.md)
