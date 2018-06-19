---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperando e exibindo dados com o modelo de associação e web forms | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889089"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperando e exibindo dados com o modelo de associação e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.
> 
> O padrão de associação de modelo funciona com qualquer tecnologia de acesso a dados. Neste tutorial, você usará o Entity Framework, mas você pode usar a tecnologia de acesso a dados que é mais familiar para você. De um controle de servidor associado a dados, como um controle ListView, GridView, DetailsView ou FormView, você deve especificar os nomes dos métodos a ser usado para selecionar, atualizar, excluir e criação de dados. Neste tutorial, você especificará um valor para o SelectMethod.
> 
> Dentro desse método, você pode fornecer a lógica para recuperar os dados. O seguinte tutorial, você definirá valores para UpdateMethod e DeleteMethod de InsertMethod.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.
> 
> No tutorial você executar o aplicativo no Visual Studio. Você também pode tornar o aplicativo disponível pela Internet por implantá-lo em um provedor de hospedagem. A Microsoft oferece gratuito da web de hospedagem para até 10 sites da web em um  
>  [conta de avaliação do Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obter informações sobre como implantar um projeto da web do Visual Studio para aplicativos de Web do serviço de aplicativo do Azure, consulte o [implantação de Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) série. Esse tutorial também mostra como usar o Entity Framework Code First Migrations para implantar seu banco de dados do SQL Server para o banco de dados do SQL Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Microsoft Visual Studio 2013 ou o Microsoft Visual Studio Express 2013 para Web
>   
> 
> Este tutorial também funciona com o Visual Studio 2012, mas haverá algumas diferenças no modelo de projeto e de interface de usuário.


## <a name="what-youll-build"></a>Será possível compilar

Neste tutorial, você vai:

1. Criar objetos de dados que refletem uma universidade com alunos inscritos em cursos
2. Criar tabelas de banco de dados de objetos
3. Preencher o banco de dados com dados de teste
4. Exibir dados em um formulário da web

## <a name="set-up-project"></a>Configurar o projeto

No Visual Studio 2013, crie um novo **aplicativo Web ASP.NET** chamado **ContosoUniversityModelBinding**.

![Criar projeto](retrieving-data/_static/image2.png)

Selecione o modelo de Web Forms e deixar as outras opções padrão. Clique em Okey para o projeto de instalação.

![Selecionar os formulários da web](retrieving-data/_static/image3.png)

Primeiro, você criará algumas pequenas alterações para personalizar a aparência do site. Abra o **Site.Master** de arquivo e altere o título para incluir Contoso University em vez de meu aplicativo ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Em seguida, altere o texto de cabeçalho de **nome do aplicativo** para **University Contoso**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Também Site.Master, altere os links de navegação que aparecem no cabeçalho para refletir as páginas que são relevantes para esse site. Você não precisará de um a **sobre** página ou o **contato** para esses links podem ser removidos da página. Em vez disso, você precisará de um link para uma página chamada **alunos**. Esta página ainda não foi criada.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Salve e feche Site.Master.

Agora, você criará o formulário da web para exibir dados de student. Clique em seu projeto, e **adicionar** um **Novo Item**. Selecione o **Web Form com página mestra** modelo e nomeie-o **Students.aspx**.

![Criar página](retrieving-data/_static/image5.png)

Selecione **Site.Master** como a página mestra para o novo formulário da web.

## <a name="create-the-data-models-and-database"></a>Criar o banco de dados e modelos de dados

Você usará migrações do Code First para criar objetos e as tabelas de banco de dados correspondente. Essas tabelas armazenará as informações sobre seus cursos e alunos.

Na pasta de modelos, adicione uma nova classe chamada **UniversityModels.cs**.

![criar a classe de modelo](retrieving-data/_static/image7.png)

Nesse arquivo, defina as classes SchoolContext, aluno, registro e curso da seguinte maneira:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

A classe SchoolContext deriva de DbContext, que gerencia a conexão de banco de dados e as alterações nos dados.

Na classe aluno, observe os atributos que foram aplicados a **FirstName**, **LastName**, e **ano** propriedades. Esses atributos serão usados para validação de dados neste tutorial. Para simplificar o código para este projeto de demonstração, apenas essas propriedades foram marcadas com atributos de validação de dados. Em um projeto real, seria aplicada atributos de validação para todas as propriedades que precisam de dados validados, como propriedades nas classes de registro e o curso.

Save UniversityModels.cs.

Você usará as ferramentas para migrações do Code First para definir um banco de dados com base nessas classes.

Em **Package Manager Console**, execute o comando:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Se o comando for concluído com êxito, você receberá uma mensagem informando que as migrações foram habilitadas,

![permitir as migrações](retrieving-data/_static/image8.png)

Observe que um novo arquivo chamado **Configuration.cs** foi criado. No Visual Studio, esse arquivo é aberto automaticamente depois que ele é criado. A classe de configuração contém uma **semente** método que permite que você preencher previamente as tabelas de banco de dados com dados de teste.

Adicione o seguinte código ao método de propagação. Você precisará adicionar um **usando** instrução para o **ContosoUniversityModelBinding.Models** namespace.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Salve Configuration.cs.

No Console do Gerenciador de pacotes, execute o comando `add-migration initial`.

Em seguida, execute o comando `update-database`.

Se você receber uma exceção ao executar esse comando, é possível que os valores de StudentID e CourseID têm variados dos valores no método de propagação. Abra as tabelas no banco de dados e localizar os valores existentes para StudentID e CourseID. Adicione esses valores no código para a tabela de registros a propagação.

O arquivo de banco de dados foi adicionado, mas ele está oculto no momento no projeto. Clique em **Mostrar todos os arquivos** para exibir o arquivo.

![Mostrar todos os arquivos](retrieving-data/_static/image10.png)

Observe que o arquivo. mdf agora aparece no aplicativo\_pasta de dados.

![arquivo de banco de dados](retrieving-data/_static/image12.png)

Clique duas vezes o arquivo. mdf e abra o Gerenciador de servidores. As tabelas agora existem e são preenchidas com dados.

![tabelas de banco de dados](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Exibir dados de tabelas relacionadas e alunos

Com os dados no banco de dados, agora você está pronto para recuperar dados e exibi-lo em uma página da web. Você usará um **GridView** controle para exibir os dados em colunas e linhas.

Abra Students.aspx e localize o **MainContent** espaço reservado. Dentro do espaço reservado, adicione um **GridView** controle que inclui o código a seguir.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Há alguns conceitos importantes nesse código de marcação para que você perceba. Primeiro, observe que um valor está definido para o **SelectMethod** propriedade no elemento GridView. Esse valor Especifica o nome do método que é usado para recuperar dados do GridView. Você criará esse método na próxima etapa. Segundo, observe que o **ItemType** está definida como a classe de estudante que você criou anteriormente. Ao definir esse valor, você pode fazer referência às propriedades dessa classe no código de marcação. Por exemplo, a classe do aluno contém uma coleção de registros. Você pode usar **Item.Enrollments** para recuperar a coleção e, em seguida, use a sintaxe LINQ para recuperar a soma dos créditos registrados para cada aluno.

O arquivo code-behind, você precisa adicionar o método que é especificado para o **SelectMethod** valor. Abra **Students.aspx.cs**e adicione **usando** instruções para o **ContosoUniversityModelBinding.Models** e **System.Data.Entity**namespaces.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Em seguida, adicione o seguinte método. Observe que o nome desse método corresponde ao nome fornecido para SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

O **incluir** cláusula melhora o desempenho dessa consulta, mas não é essencial para a consulta trabalhar. Sem a cláusula Include, os dados seriam ser recuperados usando o carregamento lento, que envolve o envio de que uma consulta separada no banco de dados cada vez relacionadas a dados são recuperados. Fornecendo a cláusula Include, os dados são recuperados usando o carregamento rápido, o que significa que todos os dados relacionados são recuperados por meio de uma única consulta do banco de dados. Em casos em que a maioria dos dados relacionados não é seja usado, eager carregamento pode ser menos eficiente, pois mais dados são recuperados. No entanto, nesse caso, carregamento rápido fornece o melhor desempenho porque os dados relacionados são exibidos para cada registro.

Para obter mais informações sobre considerações sobre o desempenho durante o carregamento de dados relacionados, consulte a seção intitulada **Lazy, Eager e explícitas carregamento de dados relacionados** no [leitura de dados relacionados com o Entity Framework em uma ASP Aplicativo do .NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tópico.

Por padrão, eles são classificados pelos valores da propriedade marcada como a chave. Você pode adicionar uma cláusula OrderBy para especificar um valor diferente para classificação. Neste exemplo, a propriedade de StudentID padrão é usada para classificação. No [classificação, paginação e filtragem de dados](sorting-paging-and-filtering-data.md) tópico, você permitirá que o usuário selecione uma coluna para classificação.

Executar seu aplicativo da web e navegue até a página de alunos. A página de alunos exibe as seguintes informações do aluno.

![Mostrar dados](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Geração automática de métodos do modelo de associação

Ao trabalhar com esta série de tutoriais, você pode simplesmente copiar o código do tutorial ao seu projeto. No entanto, uma desvantagem dessa abordagem é que você pode não estar ciente de recurso fornecido pelo Visual Studio para gerar automaticamente o código para os métodos do modelo de associação. Ao trabalhar em seus próprios projetos, geração de código automática pode eliminar tempo e ajuda a obter uma ideia de como implementar uma operação. Esta seção descreve o recurso de geração de código automática. Esta seção é apenas informativa e não contém qualquer código que você precisa implementar em seu projeto.

Ao definir um valor para o **SelectMethod**, **UpdateMethod**, **InsertMethod**, ou **DeleteMethod** propriedades no código de marcação, Você pode selecionar o **criar novo método** opção.

![Criar novo método](retrieving-data/_static/image18.png)

O Visual Studio não só cria um método no code-behind com a assinatura correta, mas também gera o código de implementação para ajudar a executar a operação. Se você definir primeiro o **ItemType** propriedade antes de usar o recurso de geração de código automática, o código gerado usará esse tipo para as operações. Por exemplo, ao definir a propriedade UpdateMethod, o código a seguir é gerado automaticamente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Novamente, o código acima não precisa ser adicionado ao seu projeto. No próximo tutorial, você irá implementar métodos para atualizar, excluir e adicionar novos dados.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou as classes de modelo de dados e gerado um banco de dados a partir dessas classes. As tabelas de banco de dados tenha sido preenchido com dados de teste. Usado associação de modelo para recuperar dados do banco de dados e depois exibidos os dados em um controle GridView.

Na próxima [tutorial](updating-deleting-and-creating-data.md) nesta série, você habilitará atualizar, excluir e criar dados.

> [!div class="step-by-step"]
> [Avançar](updating-deleting-and-creating-data.md)
