---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Implantando arquivos extras | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 41bea625a53afbf7b39c03a2e8a92a03ce144289
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371814"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando arquivos extras
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como estender o Visual Studio web publicar pipeline para fazer uma tarefa adicional durante a implantação. A tarefa é copiar os arquivos extras que não estão na pasta do projeto para o site de destino.

Para este tutorial, você copiará um arquivo extra: *robots*. Você deseja implantar esse arquivo para o preparo, mas não para produção. Na [a implantação para produção](deploying-to-production.md) tutorial, você adicionou esse arquivo ao projeto e configurou a produção publicar perfil para excluí-la. Neste tutorial, você verá um método alternativo para lidar com essa situação, o que será útil para todos os arquivos que você deseja implantar, mas não deseja incluir no projeto.

## <a name="move-the-robotstxt-file"></a>Mover o arquivo robots.

Para se preparar para um método diferente de manipulação *robots*nesta seção do tutorial, você mover o arquivo para uma pasta que não está incluída no projeto e você excluir *robots* do preparo ambiente. É necessário excluir o arquivo de preparo para que você possa verificar se o novo método de implantação do arquivo para o ambiente está funcionando corretamente.

1. No **Gerenciador de soluções**, clique com botão direito do *robots* de arquivo e clique em **excluir do projeto**.
2. Usando o Explorador de arquivos do Windows, crie uma nova pasta na pasta da solução e nomeie- *ExtraFiles*.
3. Mover o *robots* arquivo o *ContosoUniversity* pasta do projeto para o *ExtraFiles* pasta.

    ![Pasta ExtraFiles](deploying-extra-files/_static/image1.png)
4. Usando a ferramenta FTP, exclua o *robots* arquivo a partir do site de preparo.

    Como alternativa, você pode selecionar **remover arquivos adicionais no destino** sob **opções de publicação do arquivo** sobre o **configurações** guia do perfil de publicação de preparo, e publicar novamente para o preparo.

## <a name="update-the-publish-profile-file"></a>Atualizar o arquivo de perfil de publicação

Você só precisa *robots* no preparo, portanto, o perfil de publicação única, você precisará atualizar para implantá-lo é de preparo.

1. No Visual Studio, abra *Staging.pubxml*.
2. No final do arquivo, antes do fechamento `</Project>` marca, adicione a seguinte marcação:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Esse código cria uma nova *destino* que irá coletar arquivos adicionais a serem implantados. Um destino é composto de um ou mais tarefas MSBuild serão executadas com base nas condições especificadas.

    O `Include` atributo especifica que a pasta na qual localizar os arquivos *ExtraFiles*, localizado no mesmo nível que a pasta do projeto. MSBuild coletará todos os arquivos dessa pasta e recursivamente de todas as subpastas (o asterisco duplo Especifica subpastas recursivas). Com esse código, você pode colocar vários arquivos e arquivos em subpastas dentro de *ExtraFiles* pasta e todos serão implantados.

    O `DestinationRelativePath` elemento Especifica que os arquivos e pastas devem ser copiados para a pasta raiz do site da web de destino, na mesma estrutura de arquivo e pasta conforme forem encontradas em de *ExtraFiles* pasta. Se você quiser copiar o *ExtraFiles* pasta propriamente dita, o `DestinationRelativePath` valor seria *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. No final do arquivo, antes do fechamento `</Project>` marca, adicione a seguinte marcação que especifica quando executar o novo destino.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Esse código faz com que o novo `CustomCollectFiles` destino seja executado sempre que o destino que copia arquivos para a pasta de destino é executado. Há um destino separado para publicar em comparação com a criação do pacote de implantação e o novo destino é injetado em ambos os destinos, caso você pretenda implantar por meio de um pacote de implantação, em vez de publicação.

    O *. pubxml* arquivo agora se parece com o exemplo a seguir:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Salve e feche o *Staging.pubxml* arquivo.

## <a name="publish-to-staging"></a>Publicar para o preparo

Usando um clique a publicação ou a linha de comando, publicar o aplicativo usando o perfil de preparo.

Se você usar um clique em Publicar, você pode verificar a **versão prévia** janela que *robots* serão copiados. Caso contrário, use sua ferramenta FTP para verificar se o *robots* arquivo está na pasta raiz do site da web após a implantação.

## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como implantar um aplicativo da web ASP.NET em um provedor de hospedagem de terceiros. Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Mais informações

Se você souber como trabalhar com arquivos do MSBuild, você pode automatizar muitas outras tarefas de implantação, escrevendo código em *. pubxml* arquivos (para tarefas específicas de perfil) ou o projeto *. wpp.targets* arquivo (para as tarefas se aplicam a todos os perfis). Para obter mais informações sobre *. pubxml* e *. wpp.targets* arquivos, consulte [como: Editar configurações de implantação nos arquivos de perfil de publicação (. pubxml) e o. wpp.targets arquivo na Web do Visual Studio Projetos](https://msdn.microsoft.com/library/ff398069). Para obter uma introdução básica para o código do MSBuild, consulte **a anatomia de um arquivo de projeto** na [série sobre implantação corporativa: Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Para saber como trabalhar com arquivos do MSBuild para executar tarefas para seus próprios cenários, consulte este livro: [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi e William Bartholomew.

## <a name="acknowledgements"></a>Confirmações

Eu gostaria de agradecer a pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, dos Estados Unidos
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papa, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itália](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sérvia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](command-line-deployment.md)
> [Próximo](troubleshooting.md)
