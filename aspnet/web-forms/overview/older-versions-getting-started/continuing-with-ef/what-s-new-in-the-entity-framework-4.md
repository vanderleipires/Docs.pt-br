---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: O que há de novo no Entity Framework 4.0 | Microsoft Docs
author: tdykstra
description: Esta série de tutorial se baseia no aplicativo web Contoso Universidade que é criado pelo guia de Introdução com a série de tutoriais do Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889180"
---
<a name="whats-new-in-the-entity-framework-40"></a>O que há de novo no Entity Framework 4.0
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo da web Contoso Universidade que é criado pelo [guia de Introdução com o Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você pode ter sido criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série tutorial completo. Se você tiver dúvidas sobre os tutoriais, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


No tutorial anterior, você viu alguns métodos para maximizar o desempenho de um aplicativo web que usa o Entity Framework. Este tutorial examina alguns dos novos recursos mais importantes na versão 4 do Entity Framework e vincula a recursos que fornecem uma introdução mais completa a todos os novos recursos. Os recursos realçados neste tutorial incluem o seguinte:

- Associações de chave estrangeira.
- Execução de comandos SQL definida pelo usuário.
- Desenvolvimento do modelo primeiro.
- Suporte a POCO.

Além disso, o tutorial brevemente apresentará *primeiro código de desenvolvimento*, um recurso que está por vir na próxima versão do Entity Framework.

Para iniciar o tutorial, inicie o Visual Studio e abra o aplicativo web Contoso Universidade que você estava trabalhando no tutorial anterior.

## <a name="foreign-key-associations"></a>Associações de chave estrangeira

A versão 3.5 do Entity Framework inclui propriedades de navegação, mas ele não inclui propriedades de chave estrangeira no modelo de dados. Por exemplo, o `CourseID` e `StudentID` colunas do `StudentGrade` tabela deve ser omitida do `StudentGrade` entidade.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

A razão para essa abordagem era, estritamente falando, chaves estrangeiras são um detalhe de implementação física e não pertencem a um modelo de dados conceitual. No entanto, como na prática, geralmente é mais fácil trabalhar com entidades no código, quando você tem acesso direto para as chaves estrangeiras.

Para um exemplo de chaves estrangeiras como o modelo de dados pode simplificar seu código, considere como você teria de código a *DepartmentsAdd.aspx* página sem eles. No `Department` entidade, o `Administrator` propriedade é uma chave estrangeira que corresponde à `PersonID` no `Person` entidade. Para estabelecer a associação entre um departamento e administrador, tudo o que você tinha fazer foi definido o valor para o `Administrator` propriedade o `ItemInserting` manipulador de eventos do controle de associação de dados:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Sem chaves estrangeiras no modelo de dados, você usaria o `Inserting` eventos do controle de fonte de dados em vez do `ItemInserting` eventos do controle de associação de dados, para obter uma referência para a própria entidade antes da entidade é adicionada ao conjunto de entidades. Quando você tiver que fazem referência, você estabelece a associação usando código como nos exemplos a seguir:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Como você pode ver da equipe do Entity Framework [postagem de blog em associações de chave estrangeira](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), há outros casos em que a diferença em complexidade de código é muito maior. Para atender às necessidades das pessoas que preferem em tempo real com detalhes de implementação no modelo conceitual de dados para manter o código mais simples, o Entity Framework agora oferece a opção de incluir as chaves estrangeiras no modelo de dados.

Na terminologia do Entity Framework, se você incluir chaves estrangeiras no modelo de dados está usando *associações de chave estrangeiras*, e se você excluir chaves estrangeiras que você está usando *as associações independentes*.

## <a name="executing-user-defined-sql-commands"></a>Execução de comandos SQL definida pelo usuário

Em versões anteriores do Entity Framework, não havia uma maneira fácil para criar seus próprios comandos SQL em tempo real e executá-los. O Entity Framework gerado dinamicamente os comandos SQL para você ou era necessário criar um procedimento armazenado e importá-lo como uma função. Versão 4 adiciona `ExecuteStoreQuery` e `ExecuteStoreCommand` métodos de `ObjectContext` classe que facilitam para passar qualquer consulta diretamente para o banco de dados.

Suponha que os administradores de Contoso University desejam ser capaz de executar alterações em massa no banco de dados sem ter de passar pelo processo de criação de um procedimento armazenado e importá-los para o modelo de dados. Sua primeira solicitação é para uma página que permite alterar o número de créditos para todos os cursos no banco de dados. Na página da web, desejam inserir um número a ser usado para multiplicar o valor de cada `Course` da linha `Credits` coluna.

Criar uma nova página que usa o *Site.Master* página mestra e nomeie-o *UpdateCredits.aspx*. Em seguida, adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Essa marcação cria um `TextBox` controle no qual o usuário pode inserir o valor de multiplicador, um `Button` controle clique para executar o comando e um `Label` controle para indicar o número de linhas afetadas.

Abra *UpdateCredits.aspx.cs*e adicione o seguinte `using` instrução e um manipulador para o botão `Click` evento:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Esse código é executado o SQL `Update` usando o valor na caixa de texto de comando e usa o rótulo para exibir o número de linhas afetadas. Antes de executar a página, execute o *Courses.aspx* página para obter uma imagem "antes" de alguns dados.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Executar *UpdateCredits.aspx*, digite "10" como o multiplicador e, em seguida, clique em **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Execute o *Courses.aspx* novamente para ver os dados alterados.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Se você deseja definir o número de créditos de volta para seus valores originais, em *UpdateCredits.aspx.cs* alterar `Credits * {0}` para `Credits / {0}` e execute novamente a página, inserindo 10 como o divisor.)

Para obter mais informações sobre a execução de consultas que você definir no código, consulte [como: diretamente executar comandos em relação a fonte de dados](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Primeiro modelo de desenvolvimento

Essas instruções você primeiro criar o banco de dados e gerado, em seguida, o modelo de dados com base na estrutura de banco de dados. No Entity Framework 4, você pode iniciar com o modelo de dados e gerar o banco de dados com base na estrutura de modelo de dados. Se você estiver criando um aplicativo para o qual o banco de dados não existe, a abordagem de modelo permite criar entidades e relações que fazem sentido conceitualmente para o aplicativo, ao mesmo tempo em que não se preocupar com detalhes de implementação física . (Isso permanece válido apenas por meio de fases iniciais do desenvolvimento, no entanto. Eventualmente o banco de dados será criado e terá dados de produção no-lo e recriá-la do modelo não poderá mais ser prático; Neste ponto você estará volta para a abordagem de banco de dados.)

Esta seção do tutorial, você criará um modelo de dados simples e gerar o banco de dados a partir dele.

Em **Solution Explorer**, com o botão direito do *DAL* pasta e selecione **Adicionar Novo Item**. No **Adicionar Novo Item** caixa de diálogo **modelos instalados** selecione **dados** e, em seguida, selecione o **modelo de dados de entidade ADO.NET** modelo . Nomeie o novo arquivo *AlumniAssociationModel.edmx* e clique em **adicionar**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Isso inicia o Assistente de modelo de dados de entidade. No **escolher o modelo de conteúdo** etapa, selecione **modelo vazio** e, em seguida, clique em **concluir**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

O **Entity Data Model Designer** abre com uma superfície de design em branco. Arraste um **entidade** item do **caixa de ferramentas** na superfície de design.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Alterar o nome da entidade de `Entity1` para `Alumnus`, altere o `Id` nome da propriedade `AlumnusId`e adicione uma nova propriedade escalar chamada `Name`. Para adicionar novas propriedades, você pode pressionar Enter depois de alterar o nome do `Id` coluna, ou a entidade e selecione **adicionar propriedade de escalar**. É o tipo padrão para novas propriedades `String`, que é bom para esta demonstração simple, mas é claro, você pode alterar coisas como tipo de dados no **propriedades** janela.

Crie outra entidade da mesma forma e o nome `Donation`. Alterar o `Id` propriedade `DonationId` e adicionar uma propriedade escalar chamada `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Para adicionar uma associação entre essas duas entidades, clique com botão direito do `Alumnus` entidade, selecione **adicionar**e, em seguida, selecione **associação**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Os valores padrão no **Adicionar associação** caixa de diálogo são as que você deseja (um-para-muitos, incluir propriedades de navegação, chaves estrangeiras), então clique em **Okey**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

O designer adiciona uma linha de associação e uma propriedade de chave estrangeira.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Agora você está pronto para criar o banco de dados. Clique com botão direito a superfície de design e selecione **gerar banco de dados de modelo**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Isso inicia o Assistente para gerar banco de dados. (Se houver avisos que indicam que as entidades não são mapeadas, você pode ignorar os durante este período.)

No **escolha sua Conexão de dados** etapa, clique em **nova Conexão**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

No **propriedades de Conexão** caixa de diálogo caixa, selecione a instância local do SQL Server Express e o nome do banco de dados `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Clique em **Sim** quando você for questionado se deseja criar o banco de dados. Quando o **escolha sua Conexão de dados** etapa seja exibida novamente, clique em **próximo**.

No **resumo e configurações** etapa, clique em **concluir**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Um *. SQL* arquivo com os comandos DDL (linguagem) de definição de dados é criado, mas os comandos ainda não foi executados.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Usar uma ferramenta como **SQL Server Management Studio** para executar o script e criar as tabelas, como você faria quando você criou o `School` banco de dados para [o primeiro tutorial a série de tutoriais de Introdução ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A menos que você baixou o banco de dados).

Agora você pode usar o `AlumniAssociation` páginas de modelo de dados na web da mesma maneira que você usou o `School` modelo. Para testar isso, adicione alguns dados para as tabelas e criar uma página da web que exibe os dados.

Usando **Server Explorer**, adicione as seguintes linhas para o `Alumnus` e `Donation` tabelas.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Criar uma nova página da web denominado *Alumni.aspx* que usa o *Site.Master* página mestra. Adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Essa marcação cria aninhada `GridView` controla, o externa para exibir nomes de ex-alunos e o interno para exibir as datas de doação e os valores.

Abra *Alumni.aspx.cs*. Adicionar um `using` instrução para os dados de acessar a camada e um manipulador para o exterior `GridView` do controle `RowDataBound` evento:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Esse código databinds interna `GridView` controlar usando o `Donations` propriedade de navegação da linha atual `Alumnus` entidade.

Execute a página.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Observação: esta página está incluída no projeto que pode ser baixado, mas para que ele funcione, você deve criar o banco de dados na instância do SQL Server Express local; o banco de dados não está incluído como uma *. mdf* arquivo o *aplicativo\_ Dados* pasta.)

Para obter mais informações sobre como usar o recurso de primeiro do modelo do Entity Framework, consulte [Model-First no Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Suporte para POCO

Quando você usa a metodologia de design controlado por domínio, você pode criar classes de dados que representam dados e o comportamento que são relevante para o domínio da empresa. Essas classes devem ser independentes de qualquer tecnologia específica usada para armazenar (persistir) os dados. em outras palavras, eles deverão ser *com ignorância de persistência*. Ignorância de persistência pode também facilitar uma classe de teste de unidade porque o projeto de teste de unidade pode usar qualquer tecnologia de persistência é mais conveniente para teste. Versões anteriores do Entity Framework oferecido suporte limitado para ignorância de persistência porque classes de entidade herdam o `EntityObject` de classe e, portanto, incluída uma grande quantidade de recursos específicos do Entity Framework.

O Entity Framework 4 apresenta a capacidade de usar as classes de entidade que não herdam a `EntityObject` classe e, portanto, persistência com ignorância. No contexto do Entity Framework, classes como isso normalmente são chamados *objetos CLR simples antigo* (POCO ou POCOs). Você pode escrever classes POCO manualmente, ou você pode gerar automaticamente com base em um modelo de dados existente usando modelos de Text Template Transformation Toolkit (T4) fornecidos pelo Entity Framework.

Para obter mais informações sobre como usar POCOs no Entity Framework, consulte os seguintes recursos:

- [Trabalhando com entidades POCO](https://msdn.microsoft.com/library/dd456853.aspx). Este é um documento do MSDN que é uma visão geral de POCOs, com links para outros documentos que possuem informações mais detalhadas.
- [Passo a passo: POCO modelo para o Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) trata de uma postagem de blog da equipe de desenvolvimento do Entity Framework, com links para outras postagens de blog sobre POCOs.

## <a name="code-first-development"></a>Desenvolvimento do Code First

Suporte a POCO no Entity Framework 4 ainda exige que você cria um modelo de dados e vincular suas classes de entidade para o modelo de dados. A próxima versão do Entity Framework inclui um recurso chamado *primeiro código de desenvolvimento*. Esse recurso permite que você use o Entity Framework com suas próprias classes POCO sem a necessidade de usar o designer de modelo de dados ou um arquivo XML do modelo de dados. (Portanto, essa opção também foi chamada *somente código*; *primeiro código* e *somente código* se referem ao mesmo recurso do Entity Framework.)

Para obter mais informações sobre como usar a abordagem de código para desenvolvimento, consulte os seguintes recursos:

- [Desenvolvimento com o Entity Framework 4, primeiro código](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Isso é uma postagem de blog, Introdução ao desenvolvimento de código primeiro Scott Guthrie.
- [Postagens no Blog da equipe do Entity Framework desenvolvimento - CodeOnly marcado](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Postagens no Blog da equipe do Entity Framework desenvolvimento - marcado Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Tutorial de repositório de música MVC - parte 4: acesso a dados e modelos](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Guia de Introdução do MVC 3 - parte 4: desenvolvimento do Entity Framework Code First](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Além disso, um novo tutorial MVC primeiro código que cria um aplicativo semelhante ao aplicativo Contoso University é projetado para ser publicado no primeiro semestre de 2011 às [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Mais Informações

Isso conclui a visão geral para o que há de novo no Entity Framework e este continuar com a série de tutoriais do Entity Framework. Para obter mais informações sobre os novos recursos no Entity Framework 4 não são abordadas aqui, consulte os seguintes recursos:

- [O que há de novo no ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) tópico do MSDN sobre os novos recursos na versão 4 do Entity Framework.
- [Anunciando o lançamento do Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) postagem no blog da equipe de desenvolvimento o Entity Framework sobre novos recursos na versão 4.

> [!div class="step-by-step"]
> [Anterior](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
