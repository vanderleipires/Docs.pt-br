---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Criar um banco de dados | Microsoft Docs
author: microsoft
description: "Etapa 2 mostra as etapas para criar o banco de dados contendo todos a refeição e RSVP dados de nosso aplicativo NerdDinner."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a>Criar um banco de dados
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 2 da livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 2 mostra as etapas para criar o banco de dados contendo todos a refeição e RSVP dados de nosso aplicativo NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner etapa 2: Criar o banco de dados

Usaremos um banco de dados para armazenar todos os dados de uma refeição e RSVP para nosso aplicativo NerdDinner.

As etapas a seguir mostram a criação de banco de dados usando a edição gratuita do SQL Server Express (que você pode instalar facilmente usando V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Todo o código que vamos gravar funciona com o SQL Server Express e o SQL Server completo.

### <a name="creating-a-new-sql-server-express-database"></a>Criando um novo banco de dados do SQL Server Express

Vamos começar clicando com o projeto da web e, em seguida, selecione o **Add -&gt;Novo Item** comando de menu:

![](create-a-database/_static/image1.png)

Isso fará a caixa de diálogo de "Adicionar Novo Item" do Visual Studio. Vamos filtrar por categoria de "Dados" e selecione o modelo de item "Banco de dados do SQL Server":

![](create-a-database/_static/image2.png)

Chamaremos o banco de dados do SQL Server Express, desejamos criar "NerdDinner.mdf" e pressione okey. O Visual Studio perguntará conosco se queremos adicionar este arquivo ao nosso \App\_diretório de dados (que é um diretório já instalação com leitura e gravação ACLs de segurança):

![](create-a-database/_static/image3.png)

Podemos clique "Sim" e nosso novo banco de dados será criado e adicionado ao nosso Solution Explorer:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Criando tabelas em nosso banco de dados

Agora temos um novo banco de dados vazio. Vamos adicionar algumas tabelas.

Para fazer isso, podemos navegará para a janela de guia "Gerenciador de servidores" dentro do Visual Studio, que permite gerenciar servidores e bancos de dados. Bancos de dados SQL Server Express armazenados no \App\_a pasta dados de nosso aplicativo será automaticamente exibida no Gerenciador de servidores. Opcionalmente, pode usar o ícone de "Conectar-se ao banco de dados" na parte superior da janela "Server Explorer" para adicionar bancos de dados do SQL Server adicionais (locais e remotos) para a lista também:

![](create-a-database/_static/image5.png)

Vamos adicionar duas tabelas para nosso banco de dados NerdDinner – um para armazenar nosso jantares e outro para rastrear RSVP aceitações-los. Podemos criar novas tabelas clicando duas vezes na pasta "Tabelas" em nosso banco de dados e escolha o comando de menu "Adicionar nova tabela":

![](create-a-database/_static/image6.png)

Isso abrirá um designer de tabela que permite configurar o esquema da tabela. Para nossa tabela "Jantares" adicionaremos 10 colunas de dados:

![](create-a-database/_static/image7.png)

Queremos a coluna "DinnerID" para ser uma chave primária exclusiva para a tabela. Podemos pode configurá-la clicando duas vezes na coluna "DinnerID" e escolhendo o item de menu "Definir chave primária":

![](create-a-database/_static/image8.png)

Além de fazer DinnerID uma chave primária, também gostaríamos de configurá-lo como uma coluna de "identidade de" cujo valor é incrementado automaticamente à medida que novas linhas de dados são adicionadas à tabela (ou seja, a primeira linha de uma refeição inserida terão um DinnerID de 1, o segundo inseridos linha será necessário um DinnerID de 2, etc).

Podemos fazer isso selecionando a coluna "DinnerID" e, em seguida, use o editor de "Propriedades da coluna" para definir a propriedade "(é identidade)" na coluna para "Sim". Vamos usar os padrões de identidade padrão (começam em 1 e incrementado 1 em cada nova linha de uma refeição):

![](create-a-database/_static/image9.png)

Será, em seguida, salvar nossa tabela digitando Ctrl-S ou usando o **arquivo -&gt;salvar** comando de menu. Essa ação solicitará a nome de tabela. Chamaremos ele "Jantares":

![](create-a-database/_static/image10.png)

Nossa nova tabela jantares aparecerá em nosso banco de dados no Gerenciador de servidores.

Vamos, em seguida, repita as etapas acima e criar uma tabela de "RSVP". Essa tabela com tem 3 colunas. Podemos configurar a coluna RsvpID como a chave primária e também o tornam uma coluna de identidade:

![](create-a-database/_static/image11.png)

Vamos salvá-lo e dê a ele o nome "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configurar uma relação de chave estrangeira entre tabelas

Agora temos duas tabelas em nosso banco de dados. Nossa última etapa de design de esquema será configurar uma relação "um-para-muitos" entre essas duas tabelas – para que é possível associar cada linha de uma refeição com zero ou mais linhas RSVP que se aplicam a ele. Faremos isso ao configurar a RSVP coluna da tabela "DinnerID" para ter uma relação de chave estrangeira para a coluna "DinnerID" na tabela "Jantares".

Para fazer isso, abrirá a tabela RSVP dentro do designer de tabela, clique duas vezes no Gerenciador de servidores. Será, em seguida, selecionamos a coluna "DinnerID" dentro dele, clique com botão direito e escolha "Relationshps …" comando de menu de contexto:

![](create-a-database/_static/image12.png)

Isso abrirá uma caixa de diálogo que podemos usar relações de configuração entre as tabelas:

![](create-a-database/_static/image13.png)

Podemos será clique no botão "Adicionar" para adicionar uma nova relação na caixa de diálogo. Após a adição de uma relação, vamos expanda o nó de exibição de árvore de "Tabelas e especificação de coluna" na grade de propriedades à direita da caixa de diálogo e, em seguida, clique no botão "..." para a direita:

![](create-a-database/_static/image14.png)

Clique no botão "…" abrirá outra caixa de diálogo que permite especificar quais tabelas e colunas estejam na relação, bem como nos permitem nomear a relação.

Iremos alterar a tabela de chaves primárias para ser "Jantares" e selecionar a coluna "DinnerID" dentro da tabela jantares como a chave primária. Nossa tabela RSVP será o RSVP e a tabela de chave estrangeira. DinnerID coluna será associada a chave estrangeira:

![](create-a-database/_static/image15.png)

Agora cada linha na tabela RSVP será associada uma linha na tabela de uma refeição. SQL Server será manter a integridade referencial para que possamos – e nos impedem de adicionar uma nova linha RSVP se ele não apontar para uma linha de uma refeição válida. Ele também impedirá nos de exclusão de uma linha de uma refeição se houver ainda RSVP linhas se referindo a ele.

### <a name="adding-data-to-our-tables"></a>Adicionando dados a nossas tabelas

Vamos concluir a adição de alguns dados de exemplo à nossa tabela jantares. Podemos adicionar dados a uma tabela clicando duas vezes no Gerenciador de servidores e escolhendo o comando "Mostrar dados da tabela":

![](create-a-database/_static/image16.png)

Vamos adicionar algumas linhas de dados de uma refeição que podemos usar posteriormente começarmos a implementar o aplicativo:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Próxima etapa

Podemos acabou de criar nosso banco de dados. Agora vamos criar classes de modelo que podemos usar para consultar e atualizá-lo.

>[!div class="step-by-step"]
[Anterior](create-a-new-aspnet-mvc-project.md)
[Próximo](build-a-model-with-business-rule-validations.md)
