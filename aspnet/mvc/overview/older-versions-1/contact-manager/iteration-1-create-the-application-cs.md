---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Iteração #1 – criar o aplicativo (c#) | Microsoft Docs'
author: microsoft
description: 'A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, ler, atualizar e D...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f626511164363fea2195a05e73aeee5764933b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="iteration-1--create-the-application-c"></a>Iteração #1 – criar o aplicativo (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, leitura, atualização e exclusão (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de entre em contato com toda a partir do início ao fim. O aplicativo Gerenciador de entrar em contato com permite armazenar informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Vamos criar o aplicativo em várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. O objetivo dessa abordagem iteração vários é para que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, leitura, atualização e exclusão (CRUD).

- Iteração #2 - Verifique o aplicativo parecer adequado. Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos.

- Iteração #3 - adicionar validação do formulário. Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar endereços de email e números de telefone.

- Iteração #4 - Verifique o aplicativo acoplados de forma flexível. Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 - Use desenvolvimento controlado por testes. Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração, adicionamos grupos de contatos.

- Iteração #7 - adicionar funcionalidade Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

Esta primeira iteração, criamos o aplicativo básico. O objetivo é criar o gerente do contato da maneira mais simples e mais rápido possível. Em iterações mais recente, podemos melhorar o design do aplicativo.

O aplicativo Gerenciador de contato é um aplicativo básico para banco de dados. Você pode usar o aplicativo para criar novos contatos, editar contatos existentes e excluir contatos.

Essa iteração, execute as seguintes etapas:

1. Aplicativo ASP.NET MVC
2. Criar um banco de dados para armazenar os contatos
3. Gerar uma classe de modelo para nosso banco de dados com o Microsoft Entity Framework
4. Criar uma ação do controlador e o modo de exibição que permite que lista todos os contatos no banco de dados
5. Criar ações do controlador e uma exibição que permite criar um novo contato no banco de dados
6. Criar ações do controlador e uma exibição que nos permite editar um contato existente no banco de dados
7. Criar ações do controlador e uma exibição que nos permite excluir um contato existente no banco de dados

## <a name="software-prerequisites"></a>Pré-requisitos de software

Em aplicativos ASP.NET MVC, você deve ter o Visual Studio 2008 ou o Visual Web Developer 2008 instalado em seu computador (Visual Web Developer é uma versão gratuita do Visual Studio que não inclui todos os recursos avançados do Visual Studio). Você pode baixar a versão de avaliação do Visual Studio 2008 ou o Visual Web Developer do seguinte endereço:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Para aplicativos ASP.NET MVC com o Visual Web Developer, você deve ter o Visual Web Developer Service Pack 1 instalado. Sem Service Pack 1, você não pode criar projetos de aplicativo Web.


Estrutura do ASP.NET MVC. Você pode baixar a estrutura ASP.NET MVC do seguinte endereço:

[https://www.asp.net/mvc](../../../index.md)

Neste tutorial, usamos o Entity Framework da Microsoft para acessar um banco de dados. O Entity Framework está incluído no .NET Framework 3.5 Service Pack 1. Você pode baixar esse service pack do seguinte local:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como uma alternativa para executar cada uma downloads, você pode aproveitar o Web Platform Installer (Web PI). Você pode baixar o Web PI do seguinte endereço:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC Project

Projeto de aplicativo Web ASP.NET MVC. Inicie o Visual Studio e selecione a opção de menu **arquivo, novo projeto**. O **novo projeto** caixa de diálogo é exibida (consulte a Figura 1). Selecione o **Web** tipo de projeto e o **aplicativo Web ASP.NET MVC** modelo. Nomeie seu novo projeto *ContactManager* e clique no botão Okey.


Certifique-se de que você tenha selecionado na lista suspensa na parte superior do .NET Framework 3.5 à direita do **novo projeto** caixa de diálogo. Caso contrário, o modelo de aplicativo Web ASP.NET MVC ganha t aparecer.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figura 01**: caixa de diálogo Novo projeto a ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image2.png))


Aplicativo ASP.NET MVC, a **criar o projeto de teste de unidade** caixa de diálogo é exibida. Você pode usar essa caixa de diálogo para indicar que você deseja criar e adicionar um projeto de teste de unidade para sua solução quando você criar seu aplicativo ASP.NET MVC. Embora nós ganha t ser criar testes de unidade nessa iteração, você deve selecionar a opção **Sim, crie um projeto de teste de unidade** porque estamos planejando adicionar testes de unidade em uma iteração mais recente. Adicionar um projeto de teste quando você cria um novo projeto ASP.NET MVC é muito mais fácil do que adicionar um projeto de teste depois que o projeto ASP.NET MVC foi criado.

> [!NOTE] 
> 
> Como o Visual Web Developer não oferece suporte a projetos de teste, você não obtém a caixa de diálogo Criar projeto de teste de unidade ao usar o Visual Web Developer.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figura 02**: caixa de diálogo a criar projeto de teste de unidade ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image4.png))


Aplicativo ASP.NET MVC é exibida na janela do Gerenciador de soluções do Visual Studio (consulte a Figura 3). Se você don t consulte a janela Gerenciador de soluções, em seguida, você pode abrir essa janela, selecionando a opção de menu **exibir, Gerenciador de soluções**. Observe que a solução contém dois projetos: o projeto ASP.NET MVC e o projeto de teste. O projeto ASP.NET MVC é denominado ContactManager, e o projeto de teste ContactManager.Tests.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figura 03**: janela do Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Excluindo os arquivos de exemplo de projeto

O modelo de projeto ASP.NET MVC inclui os arquivos de exemplo para controladores e exibições. Antes de criar um novo aplicativo ASP.NET MVC, você deve excluir esses arquivos. Você pode excluir arquivos e pastas na janela do Gerenciador de soluções clicando duas vezes um arquivo ou pasta e selecionando a opção de menu **excluir**.

Você precisa excluir os seguintes arquivos do projeto ASP.NET MVC:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

E, então, você precisa excluir o seguinte arquivo de projeto de teste:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Criando o banco de dados

O aplicativo Gerenciador de contato é um aplicativo web para banco de dados. Usamos um banco de dados para armazenar as informações de contato.

A estrutura ASP.NET MVC com qualquer banco de dados moderna, incluindo bancos de dados do Microsoft SQL Server, Oracle, MySQL e IBM DB2. Neste tutorial, usamos um banco de dados do Microsoft SQL Server. Quando você instala o Visual Studio, são fornecidos com a opção de instalação do Microsoft SQL Server Express que é uma versão gratuita do banco de dados do Microsoft SQL Server.

Criar um novo banco de dados clicando com o aplicativo\_pasta de dados na janela do Gerenciador de soluções e selecionando a opção de menu **adicionar, o novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione o **dados** categoria e o **o banco de dados do SQL Server** modelo (consulte a Figura 4). Nome do novo banco de dados ContactManagerDB.mdf e clique no botão Okey.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figura 04**: Criando um novo banco de dados do Microsoft SQL Server Express ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image8.png))


Depois de criar o novo banco de dados, o banco de dados é exibido no aplicativo\_pasta de dados na janela do Gerenciador de soluções. Clique duas vezes no arquivo de ContactManager.mdf para abrir a janela Gerenciador de servidores e conecte-se ao banco de dados.

> [!NOTE] 
> 
> A janela Gerenciador de servidores é chamada de janela Pesquisador de objetos de banco de dados no caso do Microsoft Visual Web Developer.


Você pode usar a janela Gerenciador de servidores para criar novos objetos de banco de dados, como procedimentos armazenados, exibições, gatilhos e tabelas de banco de dados. Clique na pasta de tabelas e selecione a opção de menu **adicionar nova tabela**. O Designer de tabela do banco de dados é exibida (consulte a Figura 5).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figura 05**: O Designer de tabela de banco de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image10.png))


É preciso criar uma tabela que contém as seguintes colunas:

<a id="0.1_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Telefone | nvarchar(50) | false |
| Email | nvarchar(255) | false |


A primeira coluna, a coluna de Id é especial. Você precisa marcar a coluna Id como uma coluna de identidade e uma coluna de chave primária. Você indicar que uma coluna é uma coluna de identidade, expandindo propriedades de coluna (procure na parte inferior da Figura 6) e rolar para baixo para a propriedade de especificação de identidade. Definir o **(é identidade)** propriedade para o valor **Sim**.

Você pode marcar uma coluna como uma coluna de chave primária, selecionando a coluna e clicar no botão com o ícone de uma chave. Depois de uma coluna está marcada como uma coluna de chave primária, um ícone de uma chave aparecerá ao lado da coluna (consulte a Figura 6).

Depois de terminar de criar a tabela, clique no botão de salvar (o botão com um ícone de um disquete) para salvar a nova tabela. Forneça o nome de sua nova tabela *contatos*.

Depois de concluir a criação da tabela de banco de dados de contatos, você deve adicionar alguns registros à tabela. A tabela contatos na janela do Gerenciador de servidores e selecione a opção de menu **Mostrar dados da tabela**. Insira um ou mais contatos na grade que aparece.

## <a name="creating-the-data-model"></a>Criando o modelo de dados

O aplicativo ASP.NET MVC consiste em modelos, exibições e controladores. Vamos começar criando uma classe de modelo que representa a tabela contatos que criamos na seção anterior.

Neste tutorial, usamos o Microsoft Entity Framework para gerar uma classe de modelo do banco de dados automaticamente.

> [!NOTE] 
> 
> A estrutura MVC do ASP.NET não está ligada à Microsoft Entity Framework de qualquer forma. Você pode usar o ASP.NET MVC com tecnologias de acesso de banco de dados alternativos incluindo NHibernate, LINQ to SQL ou ADO.NET.


Siga estas etapas para criar as classes de modelo de dados:

1. Clique na pasta de modelos na janela do Gerenciador de soluções e selecione **adicionar, o novo Item**. O **Adicionar Novo Item** caixa de diálogo é exibida (consulte a Figura 6).
2. Selecione o **dados** categoria e o **modelo de dados de entidade ADO.NET** modelo. Nome do seu modelo de dados *ContactManagerModel.edmx* e clique no **adicionar** botão. O Assistente de modelo de dados de entidade é exibido (consulte a Figura 7).
3. No **escolher o modelo de conteúdo** etapa, selecione **gerar do banco de dados** (consulte a Figura 7).
4. No **escolha sua Conexão de dados** etapa, selecione o banco de dados ContactManagerDB.mdf e digite o nome *ContactManagerDBEntities* para as configurações de Conexão de entidade (consulte a Figura 8).
5. No **escolher seus objetos de banco de dados** etapa, selecione a caixa de seleção rotulada tabelas (consulte a Figura 9). O modelo de dados incluirá todas as tabelas contidas no banco de dados (há apenas um, a tabela Contatos). Insira o namespace *modelos*. Clique no botão Concluir para concluir o assistente.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figura 06**: O diálogo Adicionar Novo Item ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image12.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figura 07**: Escolher conteúdo de modelo ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image14.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figura 08**: escolha sua Conexão de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image16.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figura 09**: Escolha seus objetos de banco de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image18.png))


Depois de concluir o Assistente de modelo de dados de entidade, o Designer de modelo de dados de entidade é exibida. O designer exibe uma classe que corresponde a cada tabela que está sendo modelada. Você deve ver uma classe denominada contatos.

O Assistente de modelo de dados de entidade gera nomes de classes com base em nomes de tabela de banco de dados. Quase sempre, você precisa alterar o nome da classe gerada pelo assistente. A classe de contatos no designer e selecione a opção de menu **Renomear**. Altere o nome da classe de contatos (plurais) para o contato (singular). Depois de alterar o nome da classe, a classe deve aparecer como a Figura 10.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figura 10**: classe o contato ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image20.png))


Neste ponto, criamos nosso modelo de banco de dados. Podemos usar a classe de contato para representar um determinado registro de contato em nosso banco de dados.

## <a name="creating-the-home-controller"></a>Criando o controlador principal

A próxima etapa é criar o nosso controlador Home. O controlador inicial é o controlador de padrão chamado em um aplicativo ASP.NET MVC.

Criar a classe do controlador Home clicando duas vezes a pasta controladores na janela do Gerenciador de soluções e selecionando a opção de menu **adicionar, controlador** (consulte a Figura 11). Observe a caixa de seleção **adicionar métodos de ação para criar, atualizar e detalhes cenários**. Certifique-se de que essa caixa de seleção é marcada antes de clicar no **adicionar** botão.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figura 11**: Adicionar controlador Home ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image22.png))


Quando você cria o controlador Home, você obtém a classe na listagem 1.

**Listando 1 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Lista de contatos

Para exibir os registros na tabela de banco de dados de contatos, precisamos criar uma ação Index () e um modo de exibição do índice.

O controlador Home já contém uma ação Index (). É preciso modificar esse método para que ele se parece com a listagem 2.

**A listagem 2 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Observe que a classe do controlador Home na listagem 2 contém um campo particular chamado \_entidades. O \_campo entidades representa as entidades do modelo de dados. Usamos o \_entidades campo para se comunicar com o banco de dados.

O método Index () retorna uma exibição que representa todos os contatos da tabela de banco de dados de contatos. A expressão \_entidades. ContactSet.ToList() retorna a lista de contatos como uma lista genérica.

Agora que nós criou o controlador de índice, em seguida, precisamos criar o modo de exibição do índice. Antes de criar o modo de exibição do índice, compilar o aplicativo, selecionando a opção de menu **criar, compilar solução**. Você sempre deve compilar o projeto antes de adicionar um modo de exibição na ordem da lista de classes de modelo a ser exibida no **adicionar exibição** caixa de diálogo.

Criar a exibição índice clicando duas vezes o método Index () e selecionando a opção de menu **adicionar exibição** (veja a Figura 12). Selecionar essa opção de menu abre o **adicionar exibição** caixa de diálogo (consulte a Figura 13).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figura 12**: adicionando a exibição do índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image24.png))


No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada**. Selecione a classe de dados de exibição ContactManager.Models.Contact e a lista de conteúdo de exibição. Seleção dessas opções gera uma exibição que exibe uma lista de registros de contato.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figura 13**: caixa de diálogo Adicionar exibir ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image26.png))


Quando você clica o **adicionar** botão, a exibição do índice na listagem 3 é gerado. Observe o &lt;% @ Page %&gt; diretiva que aparece na parte superior do arquivo. A exibição índice herda ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; classe. Em outras palavras, a classe de modelo no modo de exibição representa uma lista de entidades de contato.

O corpo da exibição de índice contém um loop foreach que itera por meio de cada um dos contatos representados pela classe de modelo. O valor de cada propriedade da classe de contato é exibido dentro de uma tabela HTML.

**A listagem 3 - Views\Home\Index.aspx (não modificado)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

É necessário fazer uma modificação para a exibição do índice. Como não, estamos criando uma exibição de detalhes, podemos remover o link de detalhes. Localize e remova o seguinte código da exibição do índice:

{id = item. % De ID})&gt;

Depois de modificar a exibição do índice, você pode executar o aplicativo Gerenciador de contato. Selecione a opção de menu Depurar, iniciar depuração ou pressione F5. Na primeira vez que você executar o aplicativo, você obtém a caixa de diálogo na Figura 14. Selecione a opção **modificar o arquivo Web. config para habilitar a depuração** e clique no botão Okey.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figura 14**: Habilitando a depuração ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image28.png))


Por padrão, o modo de exibição do índice é retornado. Este modo de exibição lista todos os dados da tabela de banco de dados de contatos (consulte a Figura 15).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figura 15**: exibir o índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image30.png))


Observe que a exibição índice inclui um link rotulado criar novo na parte inferior do modo de exibição. A próxima seção, você aprenderá como criar novos contatos.

## <a name="creating-new-contacts"></a>Criando novos contatos

Para permitir que os usuários criem novos contatos, precisamos adicionar duas ações Create () para o controlador Home. É preciso criar uma ação de Create () que retorna um formulário HTML para criar um novo contato. É preciso criar uma segunda ação Create () que executa a inserção de banco de dados real do novo contato.

Os novos métodos Create () que precisamos adicionar ao controlador Home estão contidos na listagem 4.

**A listagem 4 - Controllers\HomeController.cs (com métodos de criação)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

O primeiro método Create () pode ser chamado com um HTTP GET, enquanto o segundo método Create () pode ser chamado apenas por um HTTP POST. Em outras palavras, o segundo método Create () pode ser chamado somente durante o lançamento de um formulário HTML. O primeiro método Create () simplesmente retorna uma exibição que contém o formulário HTML para criar um novo contato. O segundo método Create () é muito mais interessante: adiciona o novo contato para o banco de dados.

Observe que o segundo método Create () foi modificado para aceitar uma instância da classe Contact. Os valores de formulário postados do formulário HTML estão associados a essa classe de contato pela estrutura do ASP.NET MVC automaticamente. Cada campo de formulário do formulário HTML criar é atribuído a uma propriedade do parâmetro de contato.

Observe que o parâmetro de contato é decorado com um atributo [ligação]. O atributo [Bind] é usado para excluir a propriedade de Id de contato da associação. Como a propriedade Id representa uma propriedade de identidade, podemos don t deseja definir a propriedade Id.

No corpo do método Create (), o Entity Framework é usado para inserir o novo contato no banco de dados. O novo contato é adicionado ao conjunto existente de contatos e o método SaveChanges () é chamado para enviar por push essas alterações de volta para o banco de dados subjacente.

Você pode gerar um formulário HTML para a criação de novos contatos clicando em um dos dois métodos Create () e selecionando a opção de menu **adicionar exibição** (consulte a Figura 16).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figura 16**: adicionar o modo de criar ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image32.png))


No **adicionar exibição** caixa de diálogo, selecione o **ContactManager.Models.Contact** classe e o **criar** opção para exibir o conteúdo (consulte a Figura 17). Quando você clica o **adicionar** botão, uma criar modo de exibição é gerado automaticamente.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figura 17**: vendo uma página Detalhar ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image34.png))


Create view contém campos de formulário para cada uma das propriedades da classe Contact. O código para o modo de exibição de criar está incluído na listagem 5.

**Listagem 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Depois de modificar os métodos Create () e adicionar o modo de exibição de criar, você pode executar o aplicativo Gerenciador de contato e criar novos contatos. Clique o **criar novo** link que aparece na exibição de índice para navegar até o modo de exibição de criar. Você deve ver a exibição na Figura 18.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figura 18**: O Create View ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Edição de contatos

Adicionando a funcionalidade para editar um registro de contato é muito semelhante à adição de funcionalidade para a criação de novos registros de contato. Primeiro, precisamos adicionar dois novos métodos de edição para a classe do controlador Home. Esses novos métodos Edit() estão contidos na listagem 6.

**Listando 6 - Controllers\HomeController.cs (com métodos de edição)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

O primeiro método Edit() é invocado por uma operação HTTP GET. Um parâmetro de Id é passado para este método que representa a Id do registro de contato que está sendo editado. O Entity Framework é usado para recuperar um contato que coincide com a ID. Uma exibição que contém um formulário HTML para a edição de um registro é retornada.

O segundo método Edit() executa a atualização real no banco de dados. Esse método aceita uma instância da classe contato como um parâmetro. A estrutura ASP.NET MVC associa os campos do formulário de edição para esta classe automaticamente. Observe que você não t incluem o atributo [Bind] ao editar um contato (é necessário o valor da propriedade Id).

O Entity Framework ser usado para salvar o contato modificado no banco de dados. O contato original deve ser recuperado do banco de dados pela primeira vez. Em seguida, o método de Entity Framework ApplyPropertyChanges() é chamado para registrar as alterações para o contato. Finalmente, o método SaveChanges () do Entity Framework é chamado para manter as alterações no banco de dados subjacente.

Você pode gerar o modo de exibição que contém o formulário de edição clicando duas vezes o método Edit() e selecionando a opção de menu Adicionar modo de exibição. Na caixa de diálogo Adicionar modo de exibição, selecione o **ContactManager.Models.Contact** classe e o **editar** exibir conteúdo (consulte a Figura 19).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figura 19**: adicionando uma exibição Editar ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image38.png))


Quando você clica no botão Adicionar, um novo modo de exibição de edição é gerado automaticamente. O formulário HTML gerado contém campos que correspondem a cada uma das propriedades da classe contato (consulte a listagem 7).

**Listando 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Excluindo contatos

Se você quiser excluir contatos, em seguida, você precisa adicionar duas ações Delete () para a classe do controlador Home. A primeira ação Delete () exibe um formulário de confirmação de exclusão. A segunda ação Delete () executa a exclusão real.

> [!NOTE] 
> 
> Mais tarde, na iteração #7, podemos modificar o gerente do contato para que ele oferece suporte a uma etapa de um Ajax excluir.


Dois novos métodos de Delete () estão contidos na listagem 8.

**Listando 8 - Controllers\HomeController.cs (métodos de exclusão)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

O primeiro método Delete () retorna um formulário de confirmação para excluir um registro de contato do banco de dados (consulte Figure20). O segundo método Delete () executa a operação de exclusão real no banco de dados. Depois que o contato original é recuperado do banco de dados, os métodos Entity Framework DeleteObject() e SaveChanges () são chamados para executar a exclusão do banco de dados.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figura 20**: O modo de exibição de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image40.png))


É preciso modificar a exibição do índice para que ele contém um link para a exclusão de registros de contato (consulte a Figura 21). Você precisa adicionar o código a seguir a mesma célula da tabela que contém o link de edição:

Html.ActionLink( { id=item.Id }) %&gt;


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figura 21**: modo de exibição com um link de edição de índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image42.png))


Em seguida, é preciso criar o modo de confirmação de exclusão. O método Delete () na classe de controlador Home e selecione a opção de menu Adicionar modo de exibição. A caixa de diálogo Adicionar modo de exibição é exibida (consulte a Figura 22).

Ao contrário no caso de modos de exibição de lista, criar e editar, a caixa de diálogo Adicionar modo de exibição não tem uma opção para criar uma exibição de exclusão. Em vez disso, selecione o **ContactManager.Models.Contact** classe de dados e o **vazio** exibir o conteúdo. Selecionar a exibição vazia conteúda opção exigirá a criar o modo de nós.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**A Figura 22**: adicionar o modo de exibição de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image44.png))


O conteúdo da exibição Delete está contido na listagem 9. Essa exibição contém um formulário que confirma ou não um contato específico deve ser excluído (consulte a Figura 21).

**Listagem 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Alterar o nome do controlador padrão

Ele pode incomodá-lo que o nome da nossa classe de controlador para trabalhar com contatos é chamado de classe HomeController. T deveria o controlador ser chamado ContactController?

Esse problema é muito fácil de corrigir. Primeiro, precisamos refatorar o nome do controlador Home. Abra a classe HomeController no Editor de código do Visual Studio, o nome da classe clique com botão direito e selecione a opção de menu **refatorar Renomear**. Selecionar essa opção de menu abre a caixa de diálogo Renomear.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figura 23**: um nome de controlador de refatoração ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image46.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figura 24**: usando a caixa de diálogo Renomear ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image48.png))


Se você renomear a classe do controlador, o Visual Studio atualizará o nome da pasta na pasta modos de exibição. O Visual Studio irá renomear a pasta de \Views\Home para a pasta \Views\Contact.

Depois de fazer essa alteração, seu aplicativo não terá mais um controlador de Home. Quando você executar o aplicativo, você obterá a página de erro na Figura 25.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figura 25**: não há nenhum controlador padrão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-cs/_static/image50.png))


É necessário atualizar a rota padrão no arquivo global. asax para usar o controlador de contato em vez de controlador Home. Abra o arquivo global. asax e modificar o controlador de padrão usado pela rota padrão (consulte a listagem 10).

**Listando 10 - asax**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Depois de fazer essas alterações, o gerente do contato será executado corretamente. Agora, ele usará a classe do controlador contato como o controlador padrão.

## <a name="summary"></a>Resumo

Nessa primeira iteração, criamos um aplicativo básico do Gerenciador de contato da maneira mais rápido possível. Aproveitamos do Visual Studio para gerar o código inicial para nossos controladores e exibições automaticamente. Também aproveitamos do Entity Framework para gerar automaticamente o nosso classes de modelo de banco de dados.

No momento, estamos pode listar, criar, editar e excluir registros de contato com o aplicativo entre em contato com o Gerenciador. Em outras palavras, podemos executar todas as operações de banco de dados básicos necessárias para um aplicativo web para banco de dados.

Infelizmente, nosso aplicativo tem alguns problemas. Primeiro e hesitam admitir que isso, o aplicativo Gerenciador de contato não é o aplicativo mais atraente. Ele precisa de um trabalho de design. A próxima iteração, analisaremos como podemos alterar a página mestra do modo de exibição padrão e a folha de estilos em cascata para melhorar a aparência do aplicativo.

Em segundo lugar, não implementamos qualquer validação do formulário. Por exemplo, não há nada para impedir o envio do formulário de contato de criar sem inserir valores para os campos do formulário. Além disso, você pode inserir endereços de email e números de telefone inválido. Começar a resolver o problema de validação do formulário na iteração #3.

Por fim e mais importante, a iteração atual do aplicativo Gerenciador de contato não pode ser facilmente modificada ou mantida. Por exemplo, a lógica de acesso de banco de dados é implantada direita nas ações do controlador. Isso significa que podemos não é possível modificar o nosso código de acesso de dados sem modificar os controladores. Últimas iterações, exploraremos padrões de design de software que podemos implementar para tornar o gerente do contato mais resiliente a alterar.

> [!div class="step-by-step"]
> [Avançar](iteration-2-make-the-application-look-nice-cs.md)
