---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Banco de dados do EF primeiro com o ASP.NET MVC: Criando o aplicativo Web e os modelos de dados | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 343131d45ed0b2442f1b0b557c5b63f3877e5d0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830407"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>Banco de dados do EF primeiro com o ASP.NET MVC: Criando o aplicativo Web e os modelos de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados. O código gerado corresponde às colunas na tabela de banco de dados.
> 
> Esta parte da série se concentra em criar o aplicativo web e gerar os modelos de dados com base em suas tabelas de banco de dados.


## <a name="create-a-new-aspnet-web-application"></a>Criar um novo aplicativo Web ASP.NET

Em uma nova solução ou a mesma solução que o projeto de banco de dados, crie um novo projeto no Visual Studio e selecione o **aplicativo Web ASP.NET** modelo. Nomeie o projeto **ContosoSite**.

![Criar projeto](creating-the-web-application/_static/image1.png)

Clique em **OK**.

Na janela novo projeto ASP.NET, selecione o **MVC** modelo. Você pode limpar a **hospedar na nuvem** opção por enquanto porque você implantará o aplicativo para a nuvem. Clique em **Okey** para criar o aplicativo.

![Selecionar modelo do mvc](creating-the-web-application/_static/image2.png)

O projeto é criado com o padrão de arquivos e pastas.

Neste tutorial, você usará o Entity Framework 6. Você pode verificar a versão do Entity Framework em seu projeto por meio da janela Gerenciar pacotes NuGet. Se necessário, atualize sua versão do Entity Framework.

![Mostrar versão](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Gerar modelos

Agora você irá criar modelos do Entity Framework das tabelas do banco de dados. Esses modelos são classes que você usará para trabalhar com os dados. Cada modelo espelha a uma tabela no banco de dados e contém propriedades que correspondem às colunas na tabela.

Clique com botão direito do **modelos** pasta e selecione **Add** e **Novo Item**.

![Adicionar novo item](creating-the-web-application/_static/image4.png)

Na janela Adicionar Novo Item, selecione **dados** no painel esquerdo e **modelo de dados de entidade ADO.NET** nas opções do painel central. Nomeie o novo arquivo de modelo **ContosoModel**.

![Criar modelo](creating-the-web-application/_static/image5.png)

Clique em **Adicionar**.

No Assistente de modelo de dados de entidade, selecione **EF Designer do banco de dados**.

![Gerar do banco de dados](creating-the-web-application/_static/image6.png)

Clique em **Avançar**.

Se você tiver conexões de banco de dados definidas dentro do ambiente de desenvolvimento, você poderá ver uma dessas conexões pré-selecionada. No entanto, você deseja criar uma nova conexão ao banco de dados que você criou na primeira parte deste tutorial. Clique o **nova Conexão** botão.

![conectar-se ao banco de dados](creating-the-web-application/_static/image7.png)

Na janela Propriedades de Conexão, forneça o nome do servidor local em que seu banco de dados foi criado (neste caso **\ProjectsV12 (localdb)**). Depois de fornecer o nome do servidor, selecione o ContosoUniversityData de bancos de dados disponíveis.

![definir propriedades de conexão](creating-the-web-application/_static/image8.png)

Clique em **OK**.

As propriedades de conexão corretas são exibidas agora. Você pode usar o nome padrão para a conexão no arquivo Web. config

![configurações de conexão](creating-the-web-application/_static/image9.png)

Clique em **Avançar**.

Selecione **tabelas** para gerar modelos para todas as três tabelas.

![Selecionar tabelas](creating-the-web-application/_static/image10.png)

Clique em **Finalizar**.

Se você receber um aviso de segurança, selecione **Okey** para continuar a execução do modelo.

Os modelos são gerados a partir de tabelas de banco de dados e é exibido um diagrama que mostra as propriedades e relacionamentos entre as tabelas.

![diagrama de modelo](creating-the-web-application/_static/image11.png)

A pasta de modelos agora inclui muitos novos arquivos relacionados aos modelos que foram gerados por banco de dados.

![Mostrar os novos arquivos de modelo](creating-the-web-application/_static/image12.png)

O **ContosoModel.Context.cs** arquivo contém uma classe que deriva de **DbContext** de classe e fornece uma propriedade para cada classe de modelo que corresponde a uma tabela de banco de dados. O **Course.cs**, **Enrollment.cs**, e **Student.cs** arquivos contêm as classes de modelo que representam as tabelas de bancos de dados. Você usará a classe de contexto e as classes de modelo ao trabalhar com o scaffolding.

Antes de continuar com este tutorial, compile o projeto. Na próxima seção, você irá gerar código com base em modelos de dados, mas essa seção não funcionarão se o projeto não tiver sido compilado.

> [!div class="step-by-step"]
> [Anterior](setting-up-database.md)
> [Próximo](generating-views.md)
