---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Banco de dados EF primeiro com o ASP.NET MVC: Criando o aplicativo Web e modelos de dados | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>Banco de dados EF primeiro com o ASP.NET MVC: Criando o aplicativo Web e modelos de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra em criar o aplicativo web e gerar os modelos de dados com base em suas tabelas de banco de dados.


## <a name="create-a-new-aspnet-web-application"></a>Criar um novo aplicativo Web ASP.NET

Em uma nova solução ou a mesma solução que o projeto de banco de dados, crie um novo projeto no Visual Studio e selecione o **aplicativo Web ASP.NET** modelo. Nomeie o projeto **ContosoSite**.

![Criar projeto](creating-the-web-application/_static/image1.png)

Clique em **OK**.

Na janela do novo projeto ASP.NET, selecione o **MVC** modelo. Você pode limpar o **Host na nuvem** opção agora porque você implantará o aplicativo para a nuvem. Clique em **Okey** para criar o aplicativo.

![Selecione o modelo mvc](creating-the-web-application/_static/image2.png)

O projeto é criado com o padrão de arquivos e pastas.

Neste tutorial, você usará o Entity Framework 6. Você pode verificar a versão do Entity Framework em seu projeto usando a janela Gerenciar pacotes NuGet. Se necessário, atualize sua versão do Entity Framework.

![Mostrar versão](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Gerar os modelos

Agora você criará modelos Entity Framework das tabelas do banco de dados. Esses modelos são classes que você usará para trabalhar com os dados. Cada modelo reflete uma tabela no banco de dados e contém propriedades que correspondem às colunas na tabela.

Clique com botão direito do **modelos** pasta e selecione **adicionar** e **Novo Item**.

![Adicionar novo item](creating-the-web-application/_static/image4.png)

Na janela Adicionar Novo Item, selecione **dados** no painel esquerdo e **modelo de dados de entidade ADO.NET** nas opções do painel central. Nomeie o novo arquivo de modelo **ContosoModel**.

![Criar modelo](creating-the-web-application/_static/image5.png)

Clique em **Adicionar**.

No Assistente de modelo de dados de entidade, selecione **EF Designer do banco de dados**.

![Gerar do banco de dados](creating-the-web-application/_static/image6.png)

Clique em **Avançar**.

Se você tiver conexões de banco de dados definidas dentro de seu ambiente de desenvolvimento, você poderá ver uma dessas conexões previamente selecionadas. No entanto, você deseja criar uma nova conexão para o banco de dados que você criou na primeira parte deste tutorial. Clique o **nova Conexão** botão.

![Conecte-se ao banco de dados](creating-the-web-application/_static/image7.png)

Na janela Propriedades de Conexão, forneça o nome do servidor local onde seu banco de dados foi criado (neste caso **\ProjectsV12 (localdb)**). Depois de fornecer o nome do servidor, selecione o ContosoUniversityData de bancos de dados disponíveis.

![definir propriedades de conexão](creating-the-web-application/_static/image8.png)

Clique em **OK**.

As propriedades de conexão corretas são exibidas agora. Você pode usar o nome padrão para a conexão no arquivo Web. config

![configurações de conexão](creating-the-web-application/_static/image9.png)

Clique em **Avançar**.

Selecione **tabelas** para gerar modelos para todas as três tabelas.

![Selecionar tabelas](creating-the-web-application/_static/image10.png)

Clique em **Finalizar**.

Se você receber um aviso de segurança, selecione **Okey** para continuar a execução do modelo.

Os modelos são gerados a partir de tabelas de banco de dados e um exibição do diagrama que mostra as propriedades e as relações entre as tabelas.

![diagrama do modelo](creating-the-web-application/_static/image11.png)

A pasta de modelos agora inclui muitos novos arquivos relacionados a modelos que foram gerados do banco de dados.

![Mostrar os novos arquivos de modelo](creating-the-web-application/_static/image12.png)

O **ContosoModel.Context.cs** arquivo contém uma classe que deriva de **DbContext** classe e fornece uma propriedade para cada classe de modelo que corresponde a uma tabela de banco de dados. O **Course.cs**, **Enrollment.cs**, e **Student.cs** arquivos contêm as classes de modelo que representam as tabelas de bancos de dados. Você usará a classe de contexto e as classes de modelo ao trabalhar com scaffolding.

Antes de continuar com este tutorial, crie o projeto. Na próxima seção, você irá gerar o código com base nos modelos de dados, mas essa seção não funcionará se o projeto não foi criado.

> [!div class="step-by-step"]
> [Anterior](setting-up-database.md)
> [Próximo](generating-views.md)
