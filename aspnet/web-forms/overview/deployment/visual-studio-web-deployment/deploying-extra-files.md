---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "Implantação de Web do ASP.NET usando o Visual Studio: Implantando arquivos extras | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: a34607b25f6cf502f5fbf2fe51bf1937f470159e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando arquivos extras
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão Geral

Este tutorial mostra como estender o Visual Studio web publicar pipeline para fazer uma tarefa adicional durante a implantação. A tarefa é copiar os arquivos adicionais que não estão na pasta do projeto para o site de destino.

Para este tutorial, você copiará um arquivo extra: *robots*. Você deseja implantar esse arquivo para preparo mas não para produção. Em [a implantação na produção](deploying-to-production.md) tutorial adicionado esse arquivo ao projeto e configurado a produção publicar perfil para excluí-la. Neste tutorial, você verá um método alternativo para lidar com essa situação, o que será útil para todos os arquivos que você deseja implantar, mas não deseja incluir no projeto.

## <a name="move-the-robotstxt-file"></a>Mover o arquivo robots.

Para se preparar para um método diferente de manipulação de *robots*, nesta seção do tutorial você mover o arquivo para uma pasta que não está incluída no projeto e excluir *robots* de preparo ambiente. É necessário excluir o arquivo de preparo para que você possa verificar se o novo método de implantação do arquivo para o ambiente está funcionando corretamente.

1. Em **Solution Explorer**, com o botão direito do *robots* de arquivo e clique em **excluir do projeto**.
2. Usando o Explorador de arquivos do Windows, crie uma nova pasta na pasta da solução e nomeie- *ExtraFiles*.
3. Mover o *robots* arquivo o *ContosoUniversity* pasta do projeto para o *ExtraFiles* pasta.

    ![Pasta ExtraFiles](deploying-extra-files/_static/image1.png)
4. Usando a ferramenta FTP, exclua o *robots* arquivo a partir do site de preparo.

    Como alternativa, você pode selecionar **remover arquivos adicionais no destino** em **opções de publicação do arquivo** no **configurações** guia do perfil de publicação de preparação, e republicar preparo.

## <a name="update-the-publish-profile-file"></a>Atualizar o arquivo de perfil de publicação

Você só precisa *robots* em preparo, para o perfil de publicação só é necessário atualizar para implantá-lo é de preparo.

1. No Visual Studio, abra *Staging.pubxml*.
2. No final do arquivo, antes do fechamento `</Project>` marca, adicione a seguinte marcação:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Esse código cria um novo *destino* que irá coletar arquivos adicionais a serem implantados. Um destino é composto de um ou mais tarefas MSBuild será executado com base nas condições especificadas.

    O `Include` atributo especifica que a pasta na qual a localização dos arquivos *ExtraFiles*, localizado no mesmo nível como a pasta do projeto. MSBuild coletará todos os arquivos dessa pasta e recursivamente de quaisquer subpastas (o asterisco duplo Especifica recursiva subpastas). Com este código poderia colocar vários arquivos e arquivos em subpastas dentro de *ExtraFiles* pasta e todo o será implantado.

    O `DestinationRelativePath` elemento Especifica que os arquivos e pastas devem ser copiados para a pasta raiz do site da web de destino, na mesma estrutura de arquivo e pasta como eles são encontrados no *ExtraFiles* pasta. Se você quiser copiar o *ExtraFiles* pasta propriamente dita, o `DestinationRelativePath` valor seria *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. No final do arquivo, antes do fechamento `</Project>` marca, adicione a seguinte marcação que especifica quando executar o novo destino.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Este código faz com que o novo `CustomCollectFiles` destino a ser executado sempre que o destino que copia arquivos para a pasta de destino é executado. Há um destino separado para publicar em comparação com a criação de pacote de implantação e o novo destino é injetado em ambos os destinos no caso de você optar por implantar usando um pacote de implantação, em vez de publicação.

    O *. pubxml* arquivo agora é semelhante ao exemplo a seguir:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Salve e feche o *Staging.pubxml* arquivo.

## <a name="publish-to-staging"></a>Publicar preparo

Publicar usando um clique ou a linha de comando, publicar o aplicativo usando o perfil de preparo.

Se você usar um clique em Publicar, você pode verificar no **visualização** janela que *robots* serão copiados. Caso contrário, use a ferramenta de FTP para verificar se o *robots* arquivo está na pasta raiz do site após a implantação.

## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como implantar um aplicativo da web ASP.NET em um provedor de hospedagem de terceiros. Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Mais informações

Se você souber como trabalhar com arquivos do MSBuild, você pode automatizar muitas outras tarefas de implantação, escrevendo código no *. pubxml* arquivos (para tarefas específicas do perfil) ou o projeto *. wpp.targets* arquivo (para as tarefas aplica a todos os perfis). Para obter mais informações sobre *. pubxml* e *. wpp.targets* arquivos, consulte [como: Editar configurações de implantação nos arquivos de perfil de publicação (. pubxml) e o. wpp.targets arquivo da Web do Visual Studio Projetos](https://msdn.microsoft.com/en-us/library/ff398069). Para obter uma introdução básica para o código do MSBuild, consulte **a anatomia de um arquivo de projeto** na [série de implantação corporativa: Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Para saber como trabalhar com arquivos de MSBuild para executar tarefas para seus próprios cenários, consulte este livro: [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://msbuildbook.com) Sayed Hashimi de Ibraham e William Bartholomew.

## <a name="acknowledgements"></a>Confirmações

Gostaria de agradecer seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série tutorial:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, EUA
- Adversos Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itália](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Hashimi sayed, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sérvia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Anterior](command-line-deployment.md)
[Próximo](troubleshooting.md)
