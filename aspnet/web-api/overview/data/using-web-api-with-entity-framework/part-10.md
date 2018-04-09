---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar o aplicativo do Azure do serviço de aplicativo do Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publicar o aplicativo do Azure do serviço de aplicativo do Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Como a última etapa, você publicará o aplicativo no Azure. No Gerenciador de soluções, clique com o botão direito e selecione **publicar**.

![](part-10/_static/image1.png)

Clicando em **publicar** invoca o **Publicar Web** caixa de diálogo. Se você tiver marcado **Host na nuvem** quando criou o projeto e, em seguida, a conexão e já estão configuradas. Nesse caso, basta clicar o **configurações** guia e verificar &quot;executar migrações do Code First&quot;. (Se você não marcar **Host na nuvem** no início, em seguida, siga as etapas a [próxima seção](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Para implantar o aplicativo, clique em **publicar**. Você pode exibir o andamento da publicação na **atividade de publicar Web** janela. (Da **exibição** menu, selecione **outras janelas**, em seguida, selecione **atividade de publicar Web**.)

![](part-10/_static/image4.png)

Quando o Visual Studio terminar a implantação do aplicativo, o navegador padrão é aberto automaticamente para a URL do site da Web implantado e o aplicativo que você criou agora está em execução na nuvem. A URL na barra de endereços do navegador mostra que o site está sendo carregado pela Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Implantando um novo site

Se você não marcar **Host na nuvem** quando o projeto foi criado, você pode configurar um novo aplicativo web agora. No Gerenciador de soluções, clique com o botão direito e selecione **publicar**. Selecione o **perfil** guia e clique em **sites do Microsoft Azure**. Se você não estiver conectado atualmente no Azure, você será solicitado a entrar.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

No **de sites da Web** caixa de diálogo, clique em **novo**.

![](part-10/_static/image9.png)

Insira um nome de site. Selecione sua assinatura do Azure e a região. Em **o servidor de banco de dados**, selecione **criar novo servidor**, ou selecione um servidor existente. Clique em **Criar**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Clique o **configurações** guia e verificar &quot;executar migrações do Code First&quot;. Em seguida, clique em **publicar**.

> [!div class="step-by-step"]
> [Anterior](part-9.md)
