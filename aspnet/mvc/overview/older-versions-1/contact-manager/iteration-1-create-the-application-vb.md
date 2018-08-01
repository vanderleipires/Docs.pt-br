---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iteração #1 – criar o aplicativo (VB) | Microsoft Docs'
author: microsoft
description: 'A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e D....'
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 1a61699ef3ac2936df85ea38cd472ad3f107c070
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396121"
---
<a name="iteration-1--create-the-application-vb"></a>Iteração #1 – criar o aplicativo (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e excluir (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Gerenciador de contatos permite que você armazene informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Criamos o aplicativo ao longo de várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. A meta dessa abordagem de iteração vários é que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e excluir (CRUD).

- Iteração #2 - tornar o aplicativo interessante. Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.

- Iteração #3 - adicionar validação de formulário. Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Podemos também validar endereços de email e números de telefone.

- Iteração #4 – tornar o aplicativo fracamente acoplado. Nesta quarta iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 – usar desenvolvimento controlado por teste. Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Essa iteração, adicionamos os grupos de contatos.

- Iteração #7 - adicionar a funcionalidade do Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

Esta primeira iteração, vamos criar o aplicativo básico. O objetivo é criar o Gerenciador de contatos da maneira mais rápida e simples possível. Em iterações posteriores, podemos melhorar o design do aplicativo.

O aplicativo do Gerenciador de contatos é um aplicativo básico para banco de dados. Você pode usar o aplicativo para criar novos contatos, editar contatos existentes e excluir contatos.

Nesta iteração, podemos concluir as etapas a seguir:

1. Aplicativo ASP.NET MVC
2. Criar um banco de dados para armazenar nosso contatos
3. Gerar uma classe de modelo para nosso banco de dados com o Entity Framework da Microsoft
4. Criar uma ação do controlador e o modo de exibição que nos permite listar todos os contatos no banco de dados
5. Criar ações do controlador e uma exibição que nos permite criar um novo contato no banco de dados
6. Criar ações do controlador e uma exibição que nos permite editar um contato existente no banco de dados
7. Criar ações do controlador e uma exibição que nos permite excluir um contato existente no banco de dados

## <a name="software-prerequisites"></a>Pré-requisitos de software

Em aplicativos ASP.NET MVC, você deve ter o Visual Studio 2008 ou no Visual Web Developer 2008 instalado em seu computador (Visual Web Developer é uma versão gratuita do Visual Studio que não inclui todos os recursos avançados do Visual Studio). Você pode baixar a versão de avaliação do Visual Studio 2008 ou o Visual Web Developer o seguinte endereço:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Para aplicativos ASP.NET MVC com o Visual Web Developer, você deve ter o Visual Web Developer Service Pack 1 instalado. Sem Service Pack 1, você não pode criar projetos de aplicativos Web.


Estrutura do ASP.NET MVC. Você pode baixar o ASP.NET MVC framework do seguinte endereço:

[https://www.asp.net/mvc](../../../index.md)

Neste tutorial, usamos o Microsoft Entity Framework para acessar um banco de dados. O Entity Framework está incluído com o .NET Framework 3.5 Service Pack 1. Você pode baixar esse service pack do seguinte local:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como alternativa à execução de cada um desses downloads individualmente, você pode aproveitar o Web Platform Installer (Web PI). Você pode baixar o Web PI o seguinte endereço:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projeto ASP.NET MVC

Projeto de aplicativo Web ASP.NET MVC. Inicie o Visual Studio e selecione a opção de menu **arquivo, novo projeto**. O **novo projeto** caixa de diálogo aparece (veja a Figura 1). Selecione o **Web** tipo de projeto e o **aplicativo Web ASP.NET MVC** modelo. Nomeie seu novo projeto *ContactManager* e clique no botão Okey.


Certifique-se de que você tenha o .NET Framework 3.5 está selecionada na lista suspensa na parte superior direita do **novo projeto** caixa de diálogo. Caso contrário, o modelo de aplicativo Web ASP.NET MVC que ganhou um t aparecer.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Figura 01**: caixa de diálogo New Project ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image2.png))


Aplicativo ASP.NET MVC, o **criar o projeto de teste de unidade** caixa de diálogo é exibida. Você pode usar essa caixa de diálogo para indicar que você deseja criar e adicionar um projeto de teste de unidade à sua solução quando você cria seu aplicativo ASP.NET MVC. Embora ganhamos t ser criando testes de unidade nesta iteração, você deve selecionar a opção **Sim, crie um projeto de teste de unidade** porque estamos planejando adicionar testes de unidade em uma iteração posterior. Adicionar um projeto de teste quando você cria um novo projeto ASP.NET MVC é muito mais fácil do que adicionar um projeto de teste depois que o projeto ASP.NET MVC foi criado.

> [!NOTE] 
> 
> Como o Visual Web Developer não oferece suporte a projetos de teste, você obtém a caixa de diálogo Criar projeto de teste de unidade ao usar o Visual Web Developer.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Figura 02**: caixa de diálogo do projeto de teste de unidade de criar a ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image4.png))


Aplicativo ASP.NET MVC aparece na janela do Gerenciador de soluções do Visual Studio (veja a Figura 3). Se don t vir a janela do Gerenciador de soluções, em seguida, você pode abrir essa janela, selecionando a opção de menu **exibir, Gerenciador de soluções**. Observe que a solução contém dois projetos: o projeto ASP.NET MVC e o projeto de teste. O projeto ASP.NET MVC é denominado ContactManager, e o projeto de teste ContactManager.Tests.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Figura 03**: janela o Gerenciador de soluções ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Excluindo os arquivos de projeto de exemplo

O modelo de projeto do ASP.NET MVC inclui arquivos de exemplo para controladores e exibições. Antes de criar um novo aplicativo ASP.NET MVC, você deve excluir esses arquivos. Você pode excluir arquivos e pastas na janela do Gerenciador de soluções clicando duas vezes um arquivo ou pasta e selecionando a opção de menu **excluir**.

Você precisará excluir os seguintes arquivos do projeto ASP.NET MVC:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

E, você precisa excluir o seguinte arquivo de projeto de teste:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Criando o banco de dados

O aplicativo Contact Manager é um aplicativo web controlado por banco de dados. Usamos um banco de dados para armazenar as informações de contato.

A estrutura ASP.NET MVC com qualquer banco de dados moderna, incluindo bancos de dados do Microsoft SQL Server, Oracle, MySQL e IBM DB2. Neste tutorial, usamos um banco de dados do Microsoft SQL Server. Quando você instala o Visual Studio, são fornecidos com a opção de instalação do Microsoft SQL Server Express que é uma versão gratuita do banco de dados Microsoft SQL Server.

Crie um novo banco de dados clicando com o aplicativo\_pasta de dados na janela Gerenciador de soluções e selecionando a opção de menu **adicionar, Item novo**. No **Adicionar Novo Item** caixa de diálogo, selecione o **dados** categoria e o **banco de dados do SQL Server** modelo (consulte a Figura 4). Nomeie o novo banco de dados ContactManagerDB.mdf e clique no botão Okey.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Figura 04**: criar um novo banco de dados do Microsoft SQL Server Express ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image8.png))


Depois de criar o novo banco de dados, o banco de dados é exibida no aplicativo\_pasta de dados na janela do Gerenciador de soluções. Clique duas vezes no arquivo de ContactManager.mdf para abrir a janela do Gerenciador de servidores e conecte-se ao banco de dados.

> [!NOTE] 
> 
> A janela do Gerenciador de servidores é chamada da janela do Gerenciador de banco de dados no caso do Microsoft Visual Web Developer.


Você pode usar a janela do Gerenciador de servidores para criar novos objetos de banco de dados como tabelas de banco de dados, exibições, gatilhos e procedimentos armazenados. Clique com botão direito na pasta tabelas e selecione a opção de menu **adicionar nova tabela**. O Designer de tabela do banco de dados é exibida (consulte a Figura 5).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Figura 05**: O Designer de tabela do banco de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image10.png))


É preciso criar uma tabela que contém as seguintes colunas:

<a id="0.2_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Telefone | nvarchar(50) | false |
| Email | nvarchar (255) | false |


A primeira coluna, a coluna de Id é especial. Você precisa marcar a coluna de Id como uma coluna de identidade e uma coluna de chave primária. Você indica que uma coluna é uma coluna de identidade, expandindo a propriedades de coluna (procure na parte inferior da Figura 6) e rolando para baixo até a propriedade de especificação de identidade. Defina as **(é identidade)** propriedade para o valor **Sim**.

Você pode marcar uma coluna como uma coluna de chave primária, selecionando a coluna e clicando no botão com o ícone de uma chave. Depois que uma coluna está marcada como uma coluna de chave primária, um ícone de uma chave é exibido ao lado da coluna (veja a Figura 6).

Depois de concluir a criação da tabela, clique no botão de salvar (o botão com um ícone de um disquete) para salvar a nova tabela. Dê o nome de sua nova tabela *contatos*.

Depois de concluir a criação da tabela de banco de dados de contatos, você deve adicionar alguns registros à tabela. A tabela de contatos na janela do Gerenciador de servidores com o botão direito e selecione a opção de menu **Mostrar dados da tabela**. Insira um ou mais contatos na grade que aparece.

## <a name="creating-the-data-model"></a>Criando o modelo de dados

O aplicativo ASP.NET MVC consiste em modelos, exibições e controladores. Vamos começar criando uma classe de modelo que representa a tabela de contatos que criamos na seção anterior.

Neste tutorial, usamos o Microsoft Entity Framework para gerar uma classe de modelo do banco de dados automaticamente.

> [!NOTE] 
> 
> O ASP.NET MVC framework não está ligado à Microsoft Entity Framework de forma alguma. Você pode usar o ASP.NET MVC com tecnologias de acesso de banco de dados alternativos incluindo NHibernate, LINQ to SQL ou ADO.NET.


Siga estas etapas para criar as classes de modelo de dados:

1. Clique com botão direito na pasta de modelos na janela do Gerenciador de soluções e selecione **adicionar, Item novo**. O **Adicionar Novo Item** caixa de diálogo aparece (veja a Figura 6).
2. Selecione o **dados** categoria e o **modelo de dados de entidade ADO.NET** modelo. Nomeie seu modelo de dados *ContactManagerModel.edmx* e clique no **Add** botão. O Assistente de modelo de dados de entidade é exibida (veja a Figura 7).
3. No **escolher conteúdo do modelo** etapa, selecione **gerar a partir do banco de dados** (veja a Figura 7).
4. No **escolha sua Conexão de dados** etapa, selecione o banco de dados ContactManagerDB.mdf e insira o nome *ContactManagerDBEntities* para as configurações de Conexão de entidade (consulte a Figura 8).
5. No **Choose Your Database Objects** etapa, marque a caixa de seleção rotulada como tabelas (consulte a Figura 9). O modelo de dados incluirá todas as tabelas contidas no banco de dados (há apenas um, a tabela Contatos). Insira o namespace *modelos*. Clique no botão Concluir para concluir o assistente.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Figura 06**: O diálogo Add New Item ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image12.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Figura 07**: Escolher conteúdo do modelo ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image14.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Figura 08**: escolha sua Conexão de dados ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image16.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Figura 09**: Choose Your Database Objects ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image18.png))


Depois de concluir o Assistente de modelo de dados de entidade, o Designer de modelo de dados de entidade é exibida. O designer exibe uma classe que corresponde a cada tabela que está sendo modelada. Você deve ver uma classe chamada contatos.

O Assistente de modelo de dados de entidade gera nomes de classes com base em nomes de tabela do banco de dados. Quase sempre você precisará alterar o nome da classe gerada pelo assistente. A classe de contatos no designer com o botão direito e selecione a opção de menu **Renomear**. Altere o nome da classe de contatos (plurais) para o contato (singular). Depois de alterar o nome de classe, a classe deve aparecer semelhante à Figura 10.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Figura 10**: classe entre em contato com o ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image20.png))


Neste ponto, criamos nosso modelo de banco de dados. Podemos usar a classe de contato para representar um registro de contato específico em nosso banco de dados.

## <a name="creating-the-home-controller"></a>Criando o controlador Home

A próxima etapa é criar nosso controlador Home. O controlador Home é o controlador padrão invocado em um aplicativo ASP.NET MVC.

Criar a classe de controlador Home clicando duas vezes na pasta controladores na janela do Gerenciador de soluções e selecionando a opção de menu **Add, controlador** (veja a Figura 11). Observe a caixa de seleção rotulada **adicionar métodos de ação para criar, atualizar e detalhes cenários**. Verifique se essa caixa de seleção está marcada antes de clicar na **adicionar** botão.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Figura 11**: adicionar o controlador Home ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image22.png))


Quando você cria o controlador Home, você obtém a classe na listagem 1.

**Listagem 1 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Listando os contatos

Para exibir os registros na tabela de banco de dados de contatos, precisamos criar uma ação Index () e uma exibição índice.

O controlador Home já contém uma ação Index (). Precisamos modificar esse método para que ele se parece com a listagem 2.

**Listagem 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Observe que a classe de controlador Home na listagem 2 contém um campo privado chamado \_entidades. O \_campo entidades representa as entidades do modelo de dados. Usamos o \_entidades de campo para se comunicar com o banco de dados.

O método Index () retorna uma exibição que representa todos os contatos da tabela de banco de dados de contatos. A expressão \_entidades. ContactSet.ToList() retorna a lista de contatos como uma lista genérica.

Agora que estamos ve criou o controlador de índice, em seguida, precisamos criar a exibição de índice. Antes de criar o modo de exibição de índice, compilar o aplicativo, selecionando a opção de menu **Build Build Solution**. Você sempre deve compilar o projeto antes de adicionar um modo de exibição em ordem para a lista de classes de modelo a ser exibido na **adicionar exibição** caixa de diálogo.

Criar o modo de exibição do índice clicando duas vezes o método Index () e selecionando a opção de menu **adicionar exibição** (veja a Figura 12). Selecionar essa opção de menu abre a **adicionar exibição** caixa de diálogo (consulte a Figura 13).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Figura 12**: adicionar o modo de exibição de índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image24.png))


No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada**. Selecione a classe de dados de exibição ContactManager.Contact e a lista de conteúdo do modo de exibição. Selecione essas opções gera uma exibição que exibe uma lista de registros de contato.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Figura 13**: caixa de diálogo Adicionar exibição do ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image26.png))


Quando você clica o **adicionar** botão, o modo de exibição de índice na listagem 3 é gerado. Observe que o &lt;% @ Page %&gt; diretiva aparece na parte superior do arquivo. O modo de exibição de índice herda ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; classe. Em outras palavras, a classe de modelo no modo de exibição representa uma lista de entidades de contato.

O corpo da exibição índice contém um loop foreach que itera por meio de cada um dos contatos representados pela classe de modelo. O valor de cada propriedade da classe contato é exibido em uma tabela HTML.

**Listagem 3 - Views\Home\Index.aspx (não modificado)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Precisamos fazer uma modificação para a exibição de índice. Porque não estamos criando uma exibição de detalhes, podemos remover o link de detalhes. Localizar e remover o código a seguir da exibição índice:

{ID = item. % De ID})&gt;

Depois de modificar o modo de exibição de índice, você pode executar o aplicativo Gerenciador de contatos. Selecione a opção de menu Depurar, iniciar depuração ou simplesmente pressione F5. A primeira vez que você executar o aplicativo, você obtém a caixa de diálogo na Figura 14. Selecione a opção **modificar o arquivo Web. config para habilitar a depuração** e clique no botão Okey.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Figura 14**: Habilitando a depuração ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image28.png))


O modo de exibição de índice é retornado por padrão. Este modo de exibição lista todos os dados da tabela de banco de dados de contatos (consulte a Figura 15).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Figura 15**: exibição o índice ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image30.png))


Observe que o modo de exibição de índice inclui um link rotulado criar novo na parte inferior do modo de exibição. A próxima seção, você aprenderá como criar novos contatos.

## <a name="creating-new-contacts"></a>Criando novos contatos

Para habilitar os usuários criem novos contatos, precisamos adicionar duas ações Create () para o controlador residencial. É necessário criar uma ação de Create () que retorna um formulário HTML para criar um novo contato. É necessário criar uma segunda ação Create () que executa a inserção de banco de dados real do novo contato.

Os novos métodos de Create (), precisamos adicionar ao controlador Home estão contidos na listagem 4.

**Listagem 4 - Controllers\HomeController.vb (com métodos de criação)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

O primeiro método Create () pode ser chamado com um HTTP GET, enquanto o segundo método Create () pode ser invocado apenas por um HTTP POST. Em outras palavras, o segundo método Create () pode ser invocado somente durante o lançamento de um formulário HTML. O primeiro método Create () simplesmente retorna uma exibição que contém o formulário HTML para criar um novo contato. O segundo método Create () é muito mais interessante: ele adiciona o novo contato no banco de dados.

Observe que o segundo método Create () foi modificado para aceitar uma instância da classe Contact. Os valores de formulário postados do formulário HTML são associados a essa classe de contato pela estrutura do ASP.NET MVC automaticamente. Cada campo de formulário do formulário HTML criar é atribuído a uma propriedade do parâmetro de contato.

Observe que o parâmetro de contato é decorado com um atributo [Bind]. O atributo [Bind] é usado para excluir a propriedade de Id de contato da associação. Como a propriedade Id representa uma propriedade de identidade, podemos don t deseja definir a propriedade Id.

No corpo do método Create (), o Entity Framework é usado para inserir o novo contato no banco de dados. O novo contato é adicionado ao conjunto existente de contatos e o método SaveChanges () é chamado para enviar por push essas alterações de volta para o banco de dados subjacente.

Você pode gerar um formulário HTML para a criação de novos contatos clicando duas vezes qualquer um dos dois métodos Create () e selecionando a opção de menu **adicionar exibição** (consulte a Figura 16).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Figura 16**: adição da exibição Create ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image32.png))


No **adicionar exibição** caixa de diálogo, selecione o **ContactManager.Contact** classe e o **criar** opção para exibir o conteúdo (consulte a Figura 17). Quando você clica o **adicionar** botão Criar modo de exibição é gerado automaticamente.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Figura 17**: vendo uma página explodir ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image34.png))


Criar exibição contém campos de formulário para cada uma das propriedades da classe Contact. O código da exibição Create está incluído na listagem 5.

**Listagem 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Depois de modificar os métodos Create () e adicionar o Create view, você pode executar o aplicativo Gerenciador de contato e criar novos contatos. Clique o **criar novo** link que aparece na exibição de índice para navegar até o modo de exibição de criar. Você deverá ver a exibição na Figura 18.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Figura 18**: O Create View ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Edição de contatos

Adicionar a funcionalidade para editar um registro de contato é muito semelhante a adicionar a funcionalidade para a criação de novos registros de contato. Primeiro, precisamos adicionar dois novos métodos de edição para a classe de controlador Home. Esses novos métodos Edit () estão contidos na listagem 6.

**Listagem 6 - Controllers\HomeController.vb (com métodos de edição)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

O primeiro método Edit () é invocado por uma operação HTTP GET. Um parâmetro de Id é passado para esse método que representa a Id de registro de contato que está sendo editado. O Entity Framework é usado para recuperar um contato que corresponda a ID. Uma exibição que contém um formulário HTML para edição de um registro é retornada.

O segundo método Edit () executa a atualização real no banco de dados. Esse método aceita uma instância da classe contato como um parâmetro. A estrutura ASP.NET MVC associa os campos de formulário do formulário de edição para esta classe automaticamente. Observe que você não t incluem o atributo [Bind] ao editar um contato (é necessário o valor da propriedade Id).

O Entity Framework é usado para salvar o contato modificado no banco de dados. O contato original deve ser recuperado do banco de dados pela primeira vez. Em seguida, o método de Entity Framework ApplyPropertyChanges() é chamado para registrar as alterações para o contato. Por fim, o método SaveChanges () do Entity Framework é chamado para manter as alterações no banco de dados subjacente.

Você pode gerar a exibição que contém o formulário de edição clicando duas vezes o método Edit () e selecionando a opção de menu Adicionar modo de exibição. Na caixa de diálogo Adicionar modo de exibição, selecione a **ContactManager.Models.Contact** classe e o **editar** exibir o conteúdo (consulte a Figura 19).


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Figura 19**: adicionar um modo de exibição Editar ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image38.png))


Quando você clica no botão Adicionar, um novo modo de exibição de edição é gerado automaticamente. O formulário HTML que é gerado contém campos que correspondem a cada uma das propriedades da classe contato (veja a listagem 7).

**Listagem 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Excluindo contatos

Se você quiser excluir contatos, em seguida, você precisará adicionar duas ações Delete () para a classe de controlador Home. A primeira ação Delete () exibe um formulário de confirmação de exclusão. A segunda ação Delete () executa a exclusão real.

> [!NOTE] 
> 
> Posteriormente, iteração n º 7, modificamos o gerente do contato para que ele dá suporte a uma etapa de um excluir Ajax.


Os dois novos métodos Delete () estão contidos na listagem 8.

**Listagem 8 - Controllers\HomeController.vb (métodos de exclusão)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

O primeiro método Delete () retorna um formulário de confirmação para excluir um registro de contato do banco de dados (consulte Figure20). O segundo método Delete () executa a operação de exclusão real no banco de dados. Depois que o contato original tiver sido recuperado do banco de dados, os métodos do Entity Framework DeleteObject() e SaveChanges () são chamados para executar a exclusão do banco de dados.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Figura 20**: O modo de exibição de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image40.png))


Precisamos modificar a exibição de índice para que ele contém um link para a exclusão de registros de contato (consulte a Figura 21). Você precisa adicionar o código a seguir a mesma célula da tabela que contém o link de edição:

{ID = item. % De ID})&gt;


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Figura 21**: indexa a exibição com o link de edição ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image42.png))


Em seguida, precisamos criar o modo de confirmação de exclusão. O método Delete () na classe do controlador inicial com o botão direito e selecione a opção de menu Adicionar modo de exibição. A caixa de diálogo Adicionar modo de exibição aparece (veja a Figura 22).

Ao contrário no caso das exibições de lista, criar e editar, a caixa de diálogo Adicionar modo de exibição não contém uma opção para criar um modo de exibição de exclusão. Em vez disso, selecione o **ContactManager.Models.Contact** classe de dados e o **vazia** exibir o conteúdo. Selecionando o modo de exibição vazio conteúda opção exigirá a criar a exibição de nós mesmos.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Figura 22**: adicionar o modo de exibição de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image44.png))


O conteúdo da exibição de exclusão está contido na listagem 9. Essa exibição contém um formulário que confirme se deseja ou não um contato específico deve ser excluído (veja a Figura 21).

**Listagem 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Alterando o nome do controlador padrão

Ele pode se preocupar com você que o nome da nossa classe de controlador para trabalhar com contatos é chamado da classe HomeController. Não deve t o controlador ser denominado ContactController?

Esse problema é fácil de corrigir. Primeiro, é necessário refatorar o nome do controlador Home. Abra a classe HomeController no Editor de código do Visual Studio, clique com botão direito o nome da classe e selecione a opção de menu **Renomear**. Selecionar essa opção de menu abre a caixa de diálogo de renomeação.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Figura 23**: um nome de controlador de refatoração ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image46.png))


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Figura 24**: na caixa de diálogo Rename ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image48.png))


Se você renomear sua classe de controlador, o Visual Studio atualizará o nome da pasta na pasta modos de exibição. Visual Studio irá renomear a pasta de \Views\Home para a pasta \Views\Contact.

Depois de fazer essa alteração, seu aplicativo não terá um controlador Home. Quando você executa seu aplicativo, você obterá a página de erro na Figura 25.


[![A caixa de diálogo Novo projeto](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Figura 25**: nenhum controlador padrão ([clique para exibir a imagem em tamanho normal](iteration-1-create-the-application-vb/_static/image50.png))


É necessário atualizar a rota padrão no arquivo global. asax para usar o controlador de contato em vez do controlador Home. Abra o arquivo global. asax e modificar o controlador padrão usado pela rota padrão (consulte a listagem 10).

**Listagem 10 - Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Depois de fazer essas alterações, o Gerenciador de contatos será executado corretamente. Agora, ele usará a classe de controlador de contato como o controlador padrão.

## <a name="summary"></a>Resumo

Esta primeira iteração, criamos um aplicativo básico do Gerenciador de contatos da maneira mais rápida possível. Aproveitamos do Visual Studio para gerar o código inicial para nossos controladores e modos de exibição automaticamente. Também aproveitamos o Entity Framework para gerar a nossas classes de modelo de banco de dados automaticamente.

No momento, podemos pode listar, criar, editar e excluir registros de contato com o aplicativo Gerenciador de contatos. Em outras palavras, podemos executar todas as operações de banco de dados básicos necessárias para um aplicativo web controlado por banco de dados.

Infelizmente, nosso aplicativo tem alguns problemas. Primeiro e eu hesito em admitir que isso, o aplicativo Gerenciador de contatos não é o aplicativo mais atraente. Ele precisa de um trabalho de design. Na próxima iteração, vamos examinar como podemos alterar a página mestra do modo de exibição padrão e a folha de estilos em cascata para melhorar a aparência do aplicativo.

Em segundo lugar, não implementamos nenhuma validação de formulário. Por exemplo, não há nada para impedir o envio do formulário de contato de criar sem inserir valores para qualquer um dos campos de formulário. Além disso, você pode inserir endereços de email e números de telefone inválido. Começamos a resolver o problema de validação de formulário na iteração #3.

Por fim e o mais importante, a iteração atual do aplicativo Contact Manager pode ser facilmente modificada ou mantida. Por exemplo, a lógica de acesso do banco de dados está incorporada à direita para as ações do controlador. Isso significa que não podemos modificar nosso código de acesso a dados sem modificar os controladores. Iterações posteriores, exploramos os padrões de design de software que podemos implementar para tornar o Gerenciador de contatos mais resiliente a alterar.

> [!div class="step-by-step"]
> [Anterior](iteration-7-add-ajax-functionality-cs.md)
> [Próximo](iteration-2-make-the-application-look-nice-vb.md)
