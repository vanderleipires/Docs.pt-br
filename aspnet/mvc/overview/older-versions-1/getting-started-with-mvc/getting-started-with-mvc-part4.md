---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Criando um banco de dados | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868068"
---
<a name="creating-a-database"></a>Criando um banco de dados
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos criar um novo SQL Express banco de dados que usaremos para armazenar e recuperar os dados do filme. Modo de exibição no IDE do Visual Web Developer selecione | Gerenciador de servidores. Clique com o botão direito em conexões de dados e clique em Adicionar Conexão...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Na caixa de diálogo Escolher fonte de dados, selecione Microsoft SQL Server e selecione continuar.

![](getting-started-with-mvc-part4/_static/image2.png)

Na caixa de diálogo Adicionar Conexão, digite ". \SQLEXPRESS" para o nome do servidor e digite "Filmes" como o nome do novo banco de dados.

[![Adicionar caixa de diálogo de Conexão](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Clique em Okey e será perguntado se você deseja criar esse banco de dados. Selecione Sim.

[![Criar filmes?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Agora você tem um banco de dados vazio no Gerenciador de servidores.

![Adicionar nova tabela](getting-started-with-mvc-part4/_static/image7.png)

Clique com o botão direito em tabelas e clique em Adicionar tabela. O Designer de tabela será exibida. Adicione colunas de Id, título, ReleaseDate, gênero e preço. Clique com o botão direito na coluna de ID e clique em define chave primária. É aqui que meu áreas de design parece.

[![Editor de tabela de banco de dados](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Além disso, selecione a coluna de Id e em Propriedades da coluna abaixo, altere "Especificação de identidade" para "Sim".

[![IsIdentity - propriedades da coluna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Quando você tem feito, clique no ícone Salvar na barra de ferramentas ou selecione arquivo | Salvar no menu e nomeie a tabela "**filme**" (singular). Temos um banco de dados e uma tabela!

[![Escolher nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Vá para Gerenciador de servidores, a tabela de filme clique com botão direito e selecione "Mostrar dados da tabela". Insira algumas filmes para que nosso banco de dados tenha alguns dados.

[![Edição de tabelas do banco de dados](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Criar um Modelo

Agora, alterne para o Gerenciador de soluções, no lado direito do IDE e com o botão direito na pasta modelos e selecione Adicionar | Novo Item.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vamos criar um modelo de entidade do nosso novo banco de dados. Isso adicionará um conjunto de classes ao nosso projeto que torna mais fácil para nós consultar e manipular os dados em nosso banco de dados. Selecione o nó de dados no lado esquerdo da caixa de diálogo e, em seguida, selecione o modelo de item de modelo de dados de entidade ADO.NET. O nome Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Clique no botão "Adicionar". Isso, em seguida, iniciará o "Assistente dados de entidade modelo".

Na caixa de diálogo Novo pop-up, selecione Gerar do banco de dados. Desde que acabou de criar um banco de dados, apenas precisamos saber o Entity Framework sobre nosso novo banco de dados e sua tabela. Clique em próximo a salvar nosso conexão de banco de dados de configuração do nosso aplicativo web. Agora, verifique as tabelas e filme caixa de seleção e clique em Concluir.

[![Assistente de modelo de dados de entidade](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Agora podemos ver nossa nova tabela de filme no Entity Framework Designer e acessá-lo a partir do código.

[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na superfície de design, você pode ver uma classe "Filme". Essa classe é mapeado para a tabela "Filme" em nosso banco de dados, e cada propriedade dentro dele é mapeado para uma coluna com a tabela. Cada instância de uma classe "Filme" corresponderá a uma linha na tabela "Filme".

Se você não gosta de nomenclatura padrão e convenções usadas pelo Entity Framework de mapeamento, você pode usar o designer do Entity Framework para alterar ou personalizá-los. Para este aplicativo vamos usar os padrões e basta salvar o arquivo como-é.

Agora, vamos trabalhar com alguns dados reais!

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part3.md)
> [Próximo](getting-started-with-mvc-part5.md)
