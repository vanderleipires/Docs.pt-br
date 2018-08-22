---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Criando um banco de dados | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 596a491b4152da341a7779236dab17967a6de670
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824568"
---
<a name="creating-a-database"></a>Criando um banco de dados
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos criar um novo SQL Express banco de dados que usaremos para armazenar e recuperar os dados de filme. No IDE do Visual Web Developer, selecione Exibir | Gerenciador de servidores. Clique com o botão direito em conexões de dados e clique em Adicionar Conexão...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Na caixa de diálogo Escolher fonte de dados, selecione Microsoft SQL Server e selecione continuar.

![](getting-started-with-mvc-part4/_static/image2.png)

Na caixa de diálogo Adicionar Conexão, insira ". \SQLEXPRESS" para o nome do servidor e digite "Filmes" como o nome do novo banco de dados.

[![Adicionar caixa de diálogo de Conexão](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Clique em Okey e você será solicitado se você deseja criar esse banco de dados. Selecione Sim.

[![Criar filmes?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Agora você tem um banco de dados vazio no Gerenciador de servidores.

![Adicionar nova tabela](getting-started-with-mvc-part4/_static/image7.png)

Clique com botão direito em tabelas e clique em Adicionar tabela. O Designer de tabela será exibida. Adicione colunas para Id, título, ReleaseDate, gênero e preço. Clique com o botão direito na coluna de ID e clique em define chave primária. É aqui que minhas áreas de design é semelhante.

[![Editor de tabela de banco de dados](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Além disso, selecione a coluna de Id e em Propriedades de coluna abaixo, altere "Especificação de identidade" para "Sim".

[![IsIdentity - propriedades da coluna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Quando você tem feito, clique no ícone Salvar na barra de ferramentas ou selecione arquivo | Salvar no menu e nomear sua tabela "**filme**" (singular). Temos um banco de dados e uma tabela!

[![Escolher nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Volte para o Gerenciador de servidores, clique com botão direito tabela Movie e selecione "Mostrar dados da tabela". Insira alguns filmes para que nosso banco de dados tenha alguns dados.

[![Edição de tabela do banco de dados](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Criar um Modelo

Agora, volte para o Gerenciador de soluções no lado direito do IDE e com o botão direito na pasta modelos e selecione Adicionar | Novo Item.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vamos criar um modelo de entidade de nosso novo banco de dados. Isso adicionará um conjunto de classes ao nosso projeto que torna mais fácil para nós consultar e manipular os dados dentro de nosso banco de dados. Selecione o nó de dados no lado esquerdo da caixa de diálogo e, em seguida, selecione o modelo de item de modelo de dados de entidade ADO.NET. O nome Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Clique no botão "Adicionar". Isso iniciará, em seguida, "Entidade de dados modelo de assistente".

Na nova caixa de diálogo pop-up, selecione Gerar do banco de dados. Desde que acabamos de criar um banco de dados, só precisaremos dizer o Entity Framework sobre nosso novo banco de dados e sua tabela. Clique em próximo a salvar nossa conexão de banco de dados na configuração do nosso aplicativo web. Agora, marque as tabelas e o filme caixa de seleção e clique em Finish.

[![Assistente de modelo de dados de entidade](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Agora podemos ver nossa nova tabela de filme no Entity Framework Designer e acessá-lo a partir do código.

[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na superfície de design, você pode ver uma classe "Filme". Essa classe é mapeada para a tabela "Filme" em nosso banco de dados, e cada propriedade dentro dele é mapeado para uma coluna com a tabela. Cada instância de uma classe "Filme" corresponderá a uma linha dentro da tabela "Filme".

Se você não gostar de padrão de nomenclatura e convenções usadas pelo Entity Framework de mapeamento, você pode usar o designer do Entity Framework para alterar ou personalizá-los. Para este aplicativo usaremos os padrões e salvar o arquivo como-está.

Agora, vamos trabalhar com alguns dados reais!

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part3.md)
> [Próximo](getting-started-with-mvc-part5.md)
