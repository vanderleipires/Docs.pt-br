---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Criar o capítulo Downloads para o EF 5 MVC 4 tutoriais | Microsoft Docs
author: Rick-Anderson
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6f1a28a2703fa543430d0210cc7792cb19439136
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379914"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Criar o capítulo Downloads para o EF 5 MVC 4 tutoriais
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Criando os Downloads do capítulo

1. Baixe e descompacte o arquivo zip de exemplo de projeto. O pacote de download descompactado, você encontrará os arquivos zip adicionais, um para a conclusão de cada capítulo.
2. Clique com botão direito no arquivo zip desejado, clique em **propriedades**e clique no **Unblock** botão.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Descompacte o arquivo.
4. Clique duas vezes o *CUx.sln* arquivo para iniciar o Visual Studio.
5. Do **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca**, em seguida, **Package Manager Console**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. No pacote Manager Console (PMC), clique em **restaurar**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Saia do Visual Studio.
8. Reinicie o Visual Studio, abrir o arquivo de solução que você fechou na etapa anterior.
9. No pacote Manager Console (PMC), insira o `Update-Database` comando:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Se você receber o erro a seguir:  
    >   
    >  *O termo 'Update-Database' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome, ou se um caminho foi incluído, verifique se o caminho está correto e tente novamente.*  
    > Saia e reinicie o Visual Studio.

    Cada migração será executado e, em seguida, o método de propagação será executado. Agora você pode executar o aplicativo.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Anterior](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
