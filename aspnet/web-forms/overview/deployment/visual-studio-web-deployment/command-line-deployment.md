---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Implantação de Web do ASP.NET usando o Visual Studio: implantação de linha de comando | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5673a4733257fae88fb66a3da43dfceb98c3b37a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390868"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Implantação de Web do ASP.NET usando o Visual Studio: implantação de linha de comando
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como invocar a Visual Studio web publicar pipeline da linha de comando. Isso é útil para cenários em que você deseja [automatizar o processo de implantação](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) em vez de fazê-lo manualmente no Visual Studio, geralmente, usando uma [sistema de controle de versão do código de origem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Faça uma alteração para implantar

Atualmente, a página sobre exibirá o código de modelo.

![Página sobre com código de modelo](command-line-deployment/_static/image1.png)

Você irá substituí-lo com o código que exibe um resumo de registro de aluno.

Abra o *About* página, exclua todos a marcação dentro a `MainContent` `Content` elemento e inserir a marcação a seguir em seu lugar:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Execute o projeto e selecione o **sobre** página.

![Página Sobre](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Implantar em teste usando a linha de comando

Você não implantar outra alteração de banco de dados, portanto, implantação de banco de dados dbDacFx desabilitar para o banco de dados do aspnet ContosoUniversity. Abra o **publicar na Web** do assistente e em cada um dos três perfis, claros de publicação a **Atualizar banco de dados** caixa de seleção na **configurações** guia.

Na página Iniciar do Windows 8, pesquise **Prompt de comando do desenvolvedor para VS2012**.

Clique com botão direito no ícone **Prompt de comando do desenvolvedor para VS2012** e clique em **executar como administrador**.

Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução com o caminho para o arquivo da solução:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

O MSBuild compila a solução e implanta para o ambiente de teste.

![Saída da linha de comando](command-line-deployment/_static/image3.png)

Abra um navegador e vá para `http://localhost/ContosoUniversity`, em seguida, clique em de **sobre** página para verificar se a implantação foi bem-sucedida.

Se você não tiver criado todos os alunos no teste, você verá uma página vazia sob o **estatísticas do corpo de aluno** título. Vá para o **alunos** , clique em **adicionar aluno**e adicionar alguns alunos e, em seguida, retornar para o **sobre** página para ver as estatísticas de alunos.

![Sobre a página no ambiente de teste](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opções de chave de linha de comando

O comando que você inseriu passado duas propriedades e o caminho do arquivo de solução para o MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Implantar a solução em vez de implantar projetos individuais

Especificar o arquivo de solução faz com que todos os projetos na solução a ser criado. Se você tiver vários projetos da web na solução, o comportamento de MSBuild a seguir se aplica:

- As propriedades que você especifica na linha de comando são passadas para cada projeto. Portanto, cada projeto da web deve ter um perfil de publicação com o nome que você especificar. Se você especificar `/p:PublishProfile=Test`, cada projeto da web deve ter um perfil de publicação denominado *teste*.
- Com êxito, você pode publicar um projeto quando outro ainda não compila. Para obter mais informações, consulte o thread de stackoverflow [MSBuild falha com dois pacotes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Se você especificar um projeto individual, em vez de uma solução, você precisa adicionar um parâmetro que especifica a versão do Visual Studio. Se você estiver usando o Visual Studio 2012 da linha de comando seria semelhante ao exemplo a seguir:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

O número de versão para Visual Studio 2010 é 10.0. Para obter mais informações, consulte [compatibilidade de projeto do Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) no blog do Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Especificar o perfil de publicação

Você pode especificar o perfil de publicação por nome ou o caminho completo para o *. pubxml* de arquivos, conforme mostrado no exemplo a seguir:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Métodos com suporte para publicação de linha de comando de publicar Web

Três métodos de publicação têm suporte para publicação de linha de comando:

- `MSDeploy` -Publica usando a implantação da Web.
- `Package` -Publica, criando um pacote de implantação da Web. Você precisa instalar o pacote separadamente do comando MSBuild que o cria.
- `FileSystem` -Publica copiando arquivos para uma pasta especificada.

### <a name="specifying-the-build-configuration-and-platform"></a>Especificando a configuração de compilação e plataforma

A configuração de compilação e a plataforma devem ser definidos no Visual Studio ou na linha de comando. Os perfis de publicação incluem propriedades que são nomeadas `LastUsedBuildConfiguration` e `LastUsedPlatform`, mas você não pode definir essas propriedades para determinar como o projeto é compilado. Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) no blog do Sayed Hashimi.

## <a name="deploy-to-staging"></a>Implantar no preparo

Para implantar no Azure, você deve adicionar a senha para a linha de comando. Se você salvou a senha no perfil de publicação no Visual Studio, ele foi armazenado em formato criptografado no seu *. pubxml* arquivo. Esse arquivo não é acessado pelo MSBuild, quando você fizer uma implantação de linha de comando, portanto, você precisa passar a senha em um parâmetro de linha de comando.

1. Copie a senha que você precisa do *. publishsettings* arquivo que você baixou anteriormente para o site de preparo. A senha é o valor de `userPWD` atributo para a implantação da Web `publishProfile` elemento.

    ![Senha de implantação da Web](command-line-deployment/_static/image5.png)
2. Na página Iniciar do Windows 8, pesquise **Prompt de comando do desenvolvedor para VS2012**e clique no ícone para abrir o prompt de comando. (Você não precisa para abri-lo como administrador neste momento porque você não estiver implantando no IIS no computador local).
3. Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução com o caminho para o arquivo da solução e a senha com a sua senha:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Observe que essa linha de comando inclui um parâmetro extra: `/p:AllowUntrustedCertificate=true`. Neste tutorial está sendo escrito, o `AllowUntrustedCertificate` propriedade deve ser definida quando você publica no Azure da linha de comando. Quando a correção para esse bug for lançada, você não precisa desse parâmetro.
4. Abra um navegador e vá para a URL do seu site de preparo e, em seguida, clique no **sobre** página para verificar se a implantação foi bem-sucedida.

    Como você viu anteriormente para o ambiente de teste, talvez você precise criar alguns alunos para ver as estatísticas sobre o **sobre** página.

## <a name="deploy-to-production"></a>Implantar na produção

O processo de implantação para produção é semelhante ao processo de preparo.

1. Copie a senha que você precisa do *. publishsettings* arquivo que você baixou anteriormente para o site da web de produção.
2. Abra **Prompt de comando do desenvolvedor para VS2012**.
3. Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução com o caminho para o arquivo da solução e a senha com a sua senha:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Para um site de produção real, se também houve uma alteração de banco de dados, você normalmente copiaria os *app\_offline.htm* de arquivo para o site antes da implantação e excluí-lo após a implantação bem-sucedida.
4. Abra um navegador e vá para a URL do seu site de preparo e, em seguida, clique no **sobre** página para verificar se a implantação foi bem-sucedida.

## <a name="summary"></a>Resumo

Agora você já implantou uma atualização de aplicativo usando a linha de comando.

![Sobre a página no ambiente de teste](command-line-deployment/_static/image6.png)

No próximo tutorial, você verá um exemplo de como estender a web publicar pipeline. O exemplo mostra como implantar arquivos que não estão incluídos no projeto.

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-update.md)
> [Próximo](deploying-extra-files.md)
