---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperando e exibindo dados com formulários da web e de associação de modelo | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 8153642762cf7032f335d21c8c67eac9b52ed2f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823917"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperando e exibindo dados com a associação de modelos e formulários da web
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.
> 
> O padrão de associação de modelo funciona com qualquer tecnologia de acesso a dados. Neste tutorial, você usará o Entity Framework, mas você pode usar a tecnologia de acesso de dados que é mais familiar para você. De um controle de servidor associado a dados, como um controle ListView, GridView, DetailsView ou FormView, você deve especificar os nomes dos métodos a ser usado para selecionar, atualizando, excluindo e criação de dados. Neste tutorial, você especificará um valor para o SelectMethod.
> 
> Dentro desse método, você pode fornecer a lógica para recuperar os dados. O próximo tutorial, você definirá valores para InsertMethod e DeleteMethod, o UpdateMethod.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.
> 
> No tutorial, você executar o aplicativo no Visual Studio. Você também pode fazer o aplicativo disponível na Internet, implantá-lo em um provedor de hospedagem. A Microsoft oferece a hospedagem de web gratuita para até 10 sites em um  
>  [conta gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obter informações sobre como implantar um projeto de web do Visual Studio para aplicativos de Web do serviço de aplicativo do Azure, consulte a [implantação de Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) série. Esse tutorial também mostra como usar o Entity Framework Code First Migrations para implantar seu banco de dados do SQL Server no banco de dados SQL.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Microsoft Visual Studio 2013 ou o Microsoft Visual Studio Express 2013 para Web
>   
> 
> Este tutorial também funciona com o Visual Studio 2012, mas haverá algumas diferenças no modelo de projeto e de interface do usuário.


## <a name="what-youll-build"></a>O que você vai criar

Neste tutorial, você vai:

1. Criar objetos de dados que refletem uma universidade com os alunos matriculados em cursos
2. Criar tabelas de banco de dados dos objetos
3. Preencher o banco de dados com dados de teste
4. Exibir dados em um formulário da web

## <a name="set-up-project"></a>Configurar o projeto

No Visual Studio 2013, crie um novo **aplicativo Web ASP.NET** chamado **ContosoUniversityModelBinding**.

![Criar projeto](retrieving-data/_static/image2.png)

Selecione o modelo de Web Forms e deixe as outras opções padrão. Clique em Okey para configurar o projeto.

![Selecione os formulários da web](retrieving-data/_static/image3.png)

Primeiro, você fará algumas pequenas alterações para personalizar a aparência do site. Abra o **Master** de arquivo e altere o título para incluir Contoso University, em vez de meu aplicativo ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Em seguida, altere o texto do cabeçalho da **nome do aplicativo** à **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Também no site. master, altere os links de navegação que aparecem no cabeçalho para refletir as páginas que são relevantes para este site. Você não precisarão a **sobre** página ou o **entre em contato com** página para que esses links podem ser removidos. Em vez disso, você precisará de um link para uma página chamada **alunos**. Esta página ainda não foi criada.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Salve e feche o Master.

Agora, você criará o formulário da web para exibir dados de aluno. Clique em seu projeto, e **Add** um **Novo Item**. Selecione o **Web Form com página mestra** modelo e nomeie-o **Students.aspx**.

![Criar página](retrieving-data/_static/image5.png)

Selecione **Master** como a página mestra para o novo formulário da web.

## <a name="create-the-data-models-and-database"></a>Criar o banco de dados e modelos de dados

Você usará migrações do Code First para criar objetos e as tabelas de banco de dados correspondente. Essas tabelas armazena informações sobre os alunos e seus cursos.

Na pasta modelos, adicione uma nova classe chamada **UniversityModels.cs**.

![Criar classe de modelo](retrieving-data/_static/image7.png)

Nesse arquivo, defina as classes SchoolContext, o aluno, o registro e o curso da seguinte maneira:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

A classe SchoolContext deriva de DbContext, que gerencia a conexão de banco de dados e as alterações nos dados.

A classe de aluno, observe os atributos que foram aplicados ao **FirstName**, **Sobrenome**, e **ano** propriedades. Esses atributos serão usados para validação de dados neste tutorial. Para simplificar o código para este projeto de demonstração, apenas essas propriedades foram marcadas com atributos de validação de dados. Em um projeto real, você aplicaria atributos de validação para todas as propriedades que precisam de dados validados, como as propriedades nas classes de registro e curso.

Salve UniversityModels.cs.

Você usará as ferramentas para migrações do Code First para definir um banco de dados com base nessas classes.

Na **Package Manager Console**, execute o comando:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Se o comando for concluído com êxito, você receberá uma mensagem informando que as migrações foram habilitadas,

![Habilitar migrações](retrieving-data/_static/image8.png)

Observe que um novo arquivo chamado **Configuration.cs** foi criado. No Visual Studio, esse arquivo é aberto automaticamente depois que ele é criado. A classe de configuração contém um **semente** método que permite a você preencher previamente as tabelas de banco de dados com dados de teste.

Adicione o seguinte código ao método de semente. Você precisará adicionar um **usando** instrução para o **ContosoUniversityModelBinding.Models** namespace.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Salve Configuration.cs.

No Console do Gerenciador de pacotes, execute o comando `add-migration initial`.

Em seguida, execute o comando `update-database`.

Se você receber uma exceção ao executar esse comando, é possível que os valores de StudentID e CourseID variaram dos valores no método de semente. Abra as tabelas no banco de dados e localizar os valores existentes para StudentID e CourseID. Adicione esses valores no código para a tabela de registros de propagação.

O arquivo de banco de dados foi adicionado, mas ele está oculto no momento no projeto. Clique em **Show All Files** para exibir o arquivo.

![Mostrar todos os arquivos](retrieving-data/_static/image10.png)

Observe que o arquivo. mdf agora aparece no aplicativo\_pasta de dados.

![arquivo de banco de dados](retrieving-data/_static/image12.png)

Clique duas vezes no arquivo. mdf e abra o Gerenciador de servidores. As tabelas agora existem e são preenchidas com dados.

![tabelas de banco de dados](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Exibir dados de alunos e tabelas relacionadas

Com os dados no banco de dados, agora você está pronto para recuperar dados e exibi-los em uma página da web. Você usará um **GridView** controle para exibir os dados em colunas e linhas.

Abra Students.aspx e localize o **MainContent** espaço reservado. Dentro do espaço reservado, adicione uma **GridView** controle que inclui o código a seguir.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Há alguns conceitos importantes nesse código de marcação para que você observe. Primeiro, observe que um valor está definido para o **SelectMethod** propriedade no elemento GridView. Esse valor Especifica o nome do método que é usado para recuperar dados para o GridView. Você criará esse método na próxima etapa. Em segundo lugar, observe que o **ItemType** estiver definida como a classe de aluno que você criou anteriormente. Ao definir esse valor, você pode consultar as propriedades dessa classe no código de marcação. Por exemplo, a classe de aluno contém uma coleção de registros de chamada. Você pode usar **Item.Enrollments** para recuperar essa coleção e, em seguida, use a sintaxe LINQ para recuperar a soma dos créditos registrados para cada aluno.

No arquivo code-behind, você precisará adicionar o método que é especificado para o **SelectMethod** valor. Abra **Students.aspx.cs**e adicione **usando** instruções para o **ContosoUniversityModelBinding.Models** e **Entity**namespaces.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Em seguida, adicione o seguinte método. Observe que o nome desse método corresponde ao nome que você forneceu para SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

O **Include** cláusula melhora o desempenho dessa consulta, mas não é essencial para a consulta trabalhar. Sem a cláusula Include, os dados deve ser recuperados usando o carregamento lento, o que envolve o envio de que uma consulta separada no banco de dados sempre relacionadas de dados são recuperados. Fornecendo a cláusula Include, os dados são recuperados usando o carregamento adiantado, que significa que todos os dados relacionados são recuperados por meio de uma única consulta do banco de dados. Em casos em que a maioria dos dados relacionados não ser o carregamento adiantado, usado pode ser menos eficiente, pois mais dados são recuperados. No entanto, nesse caso, o carregamento adiantado fornece o melhor desempenho porque os dados relacionados são exibidos para cada registro.

Para obter mais informações sobre considerações sobre o desempenho ao carregar dados relacionados, consulte a seção intitulada **Lazy, adiantado e explícito carregamento de dados relacionados** no [leitura de dados relacionados com o Entity Framework em um ASP Aplicativo do .NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tópico.

Por padrão, eles são classificados pelos valores da propriedade marcada como a chave. Você pode adicionar uma cláusula OrderBy para especificar um valor diferente para classificação. Neste exemplo, a propriedade de StudentID padrão é usada para classificação. No [classificação, paginação e filtragem de dados](sorting-paging-and-filtering-data.md) tópico, você permitirá que o usuário selecione uma coluna para classificação.

Executar o aplicativo web e navegue até a página de alunos. A página de alunos exibe as seguintes informações de aluno.

![Mostrar dados](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Geração automática de métodos de associação de modelo

Ao trabalhar com esta série de tutoriais, você pode simplesmente copiar o código do tutorial ao seu projeto. No entanto, uma desvantagem dessa abordagem é que você não ficar ciente do recurso fornecido pelo Visual Studio para gerar automaticamente o código para métodos de associação de modelo. Ao trabalhar em seus próprios projetos, geração automática de código pode economizar tempo e ajudar a você obter uma ideia de como implementar uma operação. Esta seção descreve o recurso de geração de código automática. Esta seção é apenas informativa e não contém qualquer código que você precisa implementar em seu projeto.

Ao definir um valor para o **SelectMethod**, **UpdateMethod**, **InsertMethod**, ou **DeleteMethod** propriedades no código de marcação, Você pode selecionar o **criar novo método** opção.

![Criar novo método](retrieving-data/_static/image18.png)

Visual Studio não só cria um método no code-behind, com a assinatura correta, mas também gera um código de implementação para auxiliá-lo executando a operação. Se você definir primeiro os **ItemType** propriedade antes de usar o recurso de geração automática de código, o código gerado usará esse tipo para as operações. Por exemplo, ao definir a propriedade UpdateMethod, o código a seguir é gerado automaticamente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Novamente, o código acima não precisa ser adicionado ao seu projeto. No próximo tutorial, você irá implementar os métodos para atualizar, excluir e adicionar novos dados.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou classes de modelo de dados e gerado um banco de dados a partir dessas classes. As tabelas de banco de dados é preenchido com dados de teste. Você usado a associação de modelo para recuperar dados do banco de dados e exibidos, em seguida, os dados em um GridView.

Nos próximos [tutorial](updating-deleting-and-creating-data.md) nesta série, você habilitará o atualização, exclusão e criação de dados.

> [!div class="step-by-step"]
> [Avançar](updating-deleting-and-creating-data.md)
