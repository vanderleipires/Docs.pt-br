---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: "Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 formulários da Web | Microsoft Docs"
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: 5a852ec2301e63bde9a5ce99db80224dad7fb258
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 formulários da Web
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. O aplicativo de exemplo é um site de uma universidade Contoso fictícia. Ele inclui a funcionalidade como admissão do aluno, criação de curso e atribuições do instrutor.
> 
> O tutorial mostra exemplos em c#. O [exemplo disponível para download](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contém código em c# e Visual Basic.
> 
> ## <a name="database-first"></a>Primeiro banco de dados
> 
> Há três maneiras que você pode trabalhar com dados no Entity Framework: *Database First*, *Model First*, e *Code First*. Este tutorial destina-se primeiro banco de dados. Para obter informações sobre as diferenças entre esses fluxos de trabalho e orientação sobre como escolher a melhor para seu cenário, consulte [fluxos de trabalho de desenvolvimento do Entity Framework](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Esta série de tutoriais usa o modelo de Web Forms do ASP.NET e considera que você sabe como trabalhar com o Web Forms do ASP.NET no Visual Studio. Se você não fizer isso, consulte [guia de Introdução ao Web Forms do ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se você preferir trabalhar com a estrutura ASP.NET MVC, consulte [guia de Introdução com o Entity Framework usando o ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versões de software
> 
> | **Mostrado no tutorial** | **Também funciona com** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | O Visual Studio 2010 Express para Web. O tutorial não foi testado com versões posteriores do Visual Studio. Há muitas diferenças nas opções de menu, caixas de diálogo e modelos. |
> | .NET 4 | .NET 4.5 é compatível com o .NET 4, mas o tutorial não foi testado com o .NET 4.5. |
> | Entity Framework 4 | O tutorial não foi testado com versões posteriores do Entity Framework. A partir do Entity Framework 5, EF usa por padrão o `DbContext API` que foi introduzida com o EF 4.1. O controle de EntityDataSource foi projetado para usar o `ObjectContext` API. Para obter informações sobre como usar o EntityDataSource controlar com o `DbContext` API, consulte [esta postagem de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Perguntas
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), o [do Entity Framework e LINQ to Fórum de entidades](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Visão Geral

O aplicativo que você criará nos tutoriais é um site de university simples.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Os usuários podem exibir e atualizar aluno, curso e informações do instrutor. Algumas das telas, você criará são mostradas abaixo.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Criando o aplicativo Web

Para iniciar o tutorial, abra o Visual Studio e, em seguida, crie um novo projeto de aplicativo Web ASP.NET usando o **aplicativo Web ASP.NET** modelo:

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Este modelo cria um projeto de aplicativo web que já inclui uma folha de estilos e páginas mestras:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Abra o *Site.Master* de arquivo e altere "My ASP.NET Application" para "Contoso Universidade".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Localizar o *Menu* controle chamado `NavigationMenu` e substituí-lo com a seguinte marcação, que adiciona itens de menu para as páginas que você criará.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Abra o *Default.aspx* página e alterar o `Content` controle chamado `BodyContent` a este:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Agora você tem uma home page simples com links para as várias páginas, você estará criando:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Criando o banco de dados

Para esses tutoriais, você usará o designer de modelo de dados do Entity Framework para criar automaticamente o modelo de dados com base em um banco de dados (geralmente chamado de *banco de dados, primeiro* abordagem). Uma alternativa que não é abordada neste tutorial série é criar o modelo de dados manualmente e, então, os designer de gerar scripts que cria o banco de dados (o *primeiro modelo* abordagem).

Para o método primeiro banco de dados usado neste tutorial, a próxima etapa é adicionar um banco de dados para o site. A maneira mais fácil é fazer o download do projeto que acompanha este tutorial. Em seguida, clique o *aplicativo\_dados* pasta, selecione **Add Existing Item**e selecione o *School.mdf* arquivo de banco de dados do projeto baixado.

Uma alternativa é siga as instruções em [criando o banco de dados de exemplo School](https://msdn.microsoft.com/en-us/library/bb399731.aspx). Se você baixar o banco de dados ou criá-la, copie o *School.mdf* arquivo da pasta a seguir para seu aplicativo *aplicativo\_dados* pasta:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(A localização do *. mdf* arquivo presume que você está usando o SQL Server 2008 Express.)

Se você criar o banco de dados de um script, execute as seguintes etapas para criar um diagrama de banco de dados:

1. Em **Server Explorer**, expanda **conexões de dados**, expanda *School.mdf*, clique com botão direito **diagramas de banco de dados**e selecione **Adicionar novo diagrama**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Selecione todas as tabelas e, em seguida, clique em **adicionar**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server cria um diagrama de banco de dados que mostra tabelas, colunas em tabelas e relações entre as tabelas. Você pode mover as tabelas para organizá-los de forma que desejar.
3. Salve o diagrama como "SchoolDiagram" e fechá-lo.

Se você baixar o *School.mdf* arquivo que acompanha este tutorial, você pode exibir o diagrama de banco de dados clicando duas vezes em **SchoolDiagram** em **diagramas de banco de dados** em **Gerenciador de servidores**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

O diagrama tem a seguinte aparência (as tabelas podem ser em locais diferentes do que é mostrado aqui):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Criando o modelo de dados do Entity Framework

Agora você pode criar um modelo de dados do Entity Framework deste banco de dados. Você pode criar o modelo de dados na pasta raiz do aplicativo, mas para este tutorial, você irá colocá-lo em uma pasta chamada *DAL* (para a camada de acesso a dados).

Em **Solution Explorer**, adicionar uma pasta de projeto chamada *DAL* (certifique-se de estar sob o projeto, não sendo a solução).

Clique com botão direito do *DAL* pasta e, em seguida, selecione **adicionar** e **Novo Item**. Em **modelos instalados**, selecione **dados**, selecione o **modelo de dados de entidade ADO.NET** modelo, nomeie-o *SchoolModel.edmx*, e em seguida, clique em **adicionar**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Isso inicia o Assistente de modelo de dados de entidade. Na primeira etapa do assistente, o **gerar do banco de dados** opção é selecionada por padrão. Clique em **Avançar**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

No **escolha sua Conexão de dados** etapa, deixe os valores padrão e clique em **próximo**. O banco de dados School é selecionado por padrão e a configuração de conexão é salva no *Web. config* de arquivos como **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

No **escolher seus objetos de banco de dados** etapa do assistente, selecione todas as tabelas, exceto `sysdiagrams` (que foi criado para o diagrama que você gerou anteriormente) e, em seguida, clique em **concluir**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Após concluir a criação do modelo, o Visual Studio mostra uma representação gráfica dos objetos do Entity Framework (entidades) que correspondem às suas tabelas de banco de dados. (Assim como acontece com o diagrama de banco de dados, o local dos elementos individuais pode ser diferente do que você vê nesta ilustração. Você pode arrastar os elementos para corresponder a ilustração, se desejar.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Explorando o modelo de dados do Entity Framework

Você pode ver que o diagrama de entidade parece muito semelhante ao diagrama de banco de dados, com algumas diferenças. Uma diferença é a adição de símbolos no final de cada associação que indicam o tipo de associação (relações de tabela são chamadas de associações de entidade no modelo de dados):

- Uma associação de um-para-zero-ou-um é representada por "1" e "entre 0 e 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    Nesse caso, um `Person` entidade pode ou não pode ser associada com um `OfficeAssignment` entidade. Um `OfficeAssignment` entidade deve ser associada com um `Person` entidade. Em outras palavras, instrutor pode ou não pode ser atribuído a um escritório, e qualquer office pode ser atribuído a apenas um instrutor.
- Uma associação de um-para-muitos é representada por "1" e "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    Nesse caso, um `Person` entidade pode ou não podem estar associados `StudentGrade` entidades. Um `StudentGrade` entidade deve ser associada a um `Person` entidade. `StudentGrade`entidades realmente representam cursos registrados neste banco de dados; Se um aluno está registrado em um curso e ainda não houver nenhuma classificação, o `Grade` propriedade é nula. Em outras palavras, um aluno não pode ser registrado em qualquer cursos ainda, pode ser registrado em um curso ou pode ser registrado em vários cursos. Cada nível em um curso registrado aplica-se apenas um aluno.
- Uma associação de muitos-para-muitos é representada por "\*"e"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    Nesse caso, um `Person` entidade pode ou não podem estar associados `Course` entidades e o inverso também é verdadeiro: um `Course` entidade pode ou não podem estar associados `Person` entidades. Em outras palavras, instrutor pode ensinar em vários cursos e um curso pode ser ensinado por vários instrutores. (Neste banco de dados, essa relação se aplica somente aos instrutores; ele não link alunos cursos. Os alunos estão vinculados aos cursos pela tabela StudentGrades.)

Outra diferença entre o diagrama de banco de dados e o modelo de dados é adicional **propriedades de navegação** seção para cada entidade. Uma propriedade de navegação de uma entidade faz referência a entidades relacionadas. Por exemplo, o `Courses` propriedade em uma `Person` entidade contém uma coleção de todos os a `Course` entidades relacionadas ao `Person` entidade.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Outra diferença entre o modelo e banco de dados é a ausência do `CourseInstructor` tabela de associação que é usada no banco de dados para vincular o `Person` e `Course` tabelas em uma relação muitos-para-muitos. As propriedades de navegação permitem que você obtenha relacionadas `Course` entidades a `Person` entidade e relacionados `Person` entidades a `Course` entidade, portanto não é necessário para representar a tabela de associação no modelo de dados.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Para fins deste tutorial, vamos supor que o `FirstName` coluna o `Person` tabela realmente contém tanto o nome e o segundo nome de uma pessoa. Para alterar o nome do campo para refletir isso, mas o administrador de banco de dados (DBA) não pode ser necessário alterar o banco de dados. Você pode alterar o nome do `FirstName` inalterado de propriedade no modelo de dados, enquanto deixa seu banco de dados equivalentes.

No designer, clique com botão direito **FirstName** no `Person` entidade e selecione **Renomear**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Digite o novo nome "FirstMidName". Isso altera a maneira como você se referir à coluna no código sem alterar o banco de dados.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

O navegador de modelo oferece uma maneira de exibir a estrutura de banco de dados, a estrutura do modelo de dados e o mapeamento entre eles. Para vê-lo, clique em uma área em branco no entity designer e, em seguida, clique em **modelo navegador**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

O **modelo navegador** painel exibe uma exibição de árvore. (O **modelo navegador** painel pode ser encaixado com o **Solution Explorer** painel.) O **SchoolModel** nó representa a estrutura do modelo de dados e o **SchoolModel.Store** nó representa a estrutura de banco de dados.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Expanda **SchoolModel.Store** para ver as tabelas, expanda **tabelas / modos de exibição** para ver as tabelas e, em seguida, expanda **curso** para ver as colunas dentro de uma tabela.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Expanda **SchoolModel**, expanda **tipos de entidade**e, em seguida, expanda o **curso** nó para ver as entidades e as propriedades dentro de entidades.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Em um designer ou o **modelo navegador** painel, você pode ver como o Entity Framework se relaciona os objetos dos dois modelos. Clique com botão direito do `Person` entidade e selecione **mapeamento de tabela**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Isso abre o **detalhes de mapeamento** janela. Observe que essa janela permite ver que a coluna de banco de dados `FirstName` é mapeado para `FirstMidName`, que é o que você renomeou no modelo de dados.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

O Entity Framework usa XML para armazenar informações sobre o banco de dados, o modelo de dados e os mapeamentos entre eles. O *SchoolModel.edmx* arquivo é realmente um arquivo XML que contém essas informações. O designer processa as informações em um formato gráfico, mas você também pode exibir o arquivo como XML clicando com o *. edmx* do arquivo em **Gerenciador de soluções**, clicando em **abrir com**e selecionando **Editor XML (texto)**. (O designer de modelo de dados e um editor de XML são apenas duas diferentes maneiras de abrir e trabalhar com o mesmo arquivo, portanto, você não pode ter o designer de abrir e abra o arquivo em um editor de XML ao mesmo tempo).

Agora você criou um site, um banco de dados e um modelo de dados. A próximo passo a passo, você começará trabalhar com dados usando o modelo de dados e o ASP.NET `EntityDataSource` controle.

>[!div class="step-by-step"]
[Avançar](the-entity-framework-and-aspnet-getting-started-part-2.md)
